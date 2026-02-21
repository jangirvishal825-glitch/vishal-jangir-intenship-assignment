# Backend Developer

## **Evaluation Criteria**

* System design clarity (services, boundaries, async workflows)
* Schema design quality (entities, constraints, indexing, migrations order)
* Database + storage selection (why, tradeoffs, scaling path)
* Security (authN/authZ, secrets, data protection, safe downloads)
* Reliability (retries, idempotency, job state machine, observability)
* Cost + scalability thinking (MVP → v1)

---

## **Problem 1: Video-to-Notes Platform (Architecture + Schema)**

**Goal:** Upload video → async processing → outputs: transcript, **Summary.md**, highlights (timestamps), screenshot/clip references. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Your solution must include**

* **System design:** API + worker(s) + storage + external AI/transcription boundaries
* **DB choice:** Postgres vs other (short justification)
* **Schema:** tables only (User, VideoAsset, Job, JobEvent, Artifact, Highlight)
* **Key constraints/indexes:** (unique keys, foreign keys, indexes you’d add)
* **Job lifecycle:** queued → processing → success/failed (+ retry/idempotency rule)
* **Storage layout:** how files/artifacts are stored + safe download strategy

**my Solution for problem 1:**

System Design
API layer built using Django REST Framework handles video upload and job creation. Uploaded videos are stored in object storage. Celery workers consume jobs via Redis queue for async processing (transcription, summarization, highlight extraction). WebSocket (Django Channels) provides real-time job progress updates. External AI and transcription APIs are called from worker layer to maintain separation.

DB Choice
MySQL is chosen for strong relational integrity, indexing performance and cost-effective hosting. It fits MVP scale and supports read replica scaling later.

Schema (tables)
User(id, email, password, created_at)
VideoAsset(id, user_id, file_url, duration, status)
Job(id, video_id, type, status, retry_count, created_at)
JobEvent(id, job_id, event, created_at)
Artifact(id, job_id, type, file_url)
Highlight(id, artifact_id, start_ts, end_ts, text)

Key constraints/indexes
Unique index on User.email
FK constraints on all relations
Index on Job.status for worker polling
Index on VideoAsset.user_id
Composite index Highlight(artifact_id, start_ts)

Job lifecycle
queued → processing → success / failed
Celery retries failed jobs up to 3 times with exponential backoff. Idempotency ensured via unique job per video and job type.

Storage layout
videos/{userId}/{videoId}.mp4
artifacts/{jobId}/transcript.json
artifacts/{jobId}/summary.md
Safe downloads implemented using short-lived pre-signed URLs.

---

## **Problem 2: LinkedIn Automation Platform (Backend Architecture)**

**Goal:** Connect LinkedIn → store persona → generate drafts (handled by GenAI team) → approve → schedule → auto-post + audit logs. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Your solution must include**

* **System design:** OAuth flow, token storage/refresh, scheduler/worker design
* **Schema:** User, LinkedInAccount, Persona, Draft, Schedule, PostAttempt/PostLog
* **Security:** encrypt tokens at rest, least-privilege scopes, access control
* **Reliability:** retry posting, dedupe to prevent double-post, rate limiting
* **Prompt/config storage proposal:** how backend stores “prompt versions” or “config packs” provided by GenAI team (DB vs repo vs hybrid, versioning + rollback)

**my Solution for problem 2:**

System Design
OAuth handled via DRF endpoints. Access and refresh tokens stored securely. Drafts created and scheduled via API. Celery beat scheduler triggers posting workers. Audit logs captured for every attempt.

Schema
User
LinkedInAccount(user_id, access_token, refresh_token, expires_at)
Persona(user_id, tone, audience)
Draft(persona_id, content, status)
Schedule(draft_id, scheduled_at)
PostLog(schedule_id, status, linkedin_post_id, error)

Security
Tokens encrypted using Django encrypted fields. OAuth scopes restricted to posting permissions. DRF permission classes enforce ownership.

Reliability
Retry posting via Celery. Deduplication using unique constraint on draft_id + scheduled_at. Rate limiting via queue throttling.

Prompt/config storage
Hybrid approach: Prompt metadata stored in MySQL, prompt templates stored in Git repo. Version column enables rollback.

---

## **Problem 3: DOCX Template → Bulk DOCX/PDF Generator (Backend + Storage)**

**Goal:** Upload DOCX template → detect fields → single fill export → bulk fill via CSV/Sheet → ZIP + per-row report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Your solution must include**

* **System design:** template ingestion, field extraction service, bulk job worker, export service
* **Schema:** Template, TemplateVersion, TemplateField, BulkRun, BulkRow, Artifact, JobEvent
* **Bulk storage strategy:** where CSV input + generated docs + ZIP live, cleanup policy
* **Reliability:** partial success handling, per-row status, retries, resumable bulk run
* **Security:** template isolation per tenant/user, safe downloads, anti-path traversal

**my Solution for problem 3:**

System Design
DRF handles template upload. Celery worker extracts fields and stores metadata. Bulk worker processes CSV input and generates documents. Export service packages ZIP and stores artifact.

Schema
Template
TemplateVersion
TemplateField
BulkRun
BulkRow
Artifact
JobEvent

Bulk storage strategy
CSV inputs stored in object storage. Generated docs saved as artifacts. ZIP stored temporarily and auto-deleted after 7 days via scheduled cleanup.

Reliability
Each BulkRow maintains status (pending, success, failed). Failed rows retried independently. Bulk runs resumable using pending rows.

Security
Templates isolated per user via user_id. File downloads via pre-signed URLs. File path sanitization prevents traversal attacks.

---

## **Problem 4: Character-Based Video Series Generator (Backend Architecture)**

**Goal:** Define characters once (image + traits + relationships). For each episode story → output episode package (script/scenes/assets plan/render plan), optionally render. [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Your solution must include**

* **System design:** episodic pipeline as jobs, asset management, consistency strategy storage
* **Schema:** Character, Relationship, Episode, Scene, Asset, RenderJob, Artifact
* **Consistency data:** what gets persisted to keep character continuity across episodes
* **Storage:** images/audio/video assets, versioning, dedupe strategy
* **Security + cost controls:** quotas, rate limits, large asset constraints

**my Solution for problem 4:**

System Design
Character service manages character metadata. Episode generation implemented as Celery job pipeline. Asset service manages media storage. Render worker optionally generates final video output. WebSocket used for episode generation progress.

Schema
Character
Relationship
Episode
Scene
Asset
RenderJob
Artifact

Consistency data
Character traits, voice style, personality embeddings, relationship graph and episode memory summary persisted to maintain continuity.

Storage
Media assets stored in object storage with versioning enabled. Hash-based deduplication avoids duplicate uploads.

Security + cost controls
Storage quotas per user. Render job limits per workspace. Asset size validation enforced.
## Problem 5: Cross-Cutting

Answer briefly for the whole platform:

1. **Multi-tenancy:** user-level vs workspace-level, how you model it in schema
2. **AuthZ model:** RBAC or simple ownership rules, and where enforced (API + DB)
3. **Observability:** what you log for jobs + correlation ids + minimal metrics
4. **Data retention:** what to delete and when (inputs, artifacts, logs)
5. **Secrets & compliance:** token encryption, key management approach, PII handling

**my Answer for problem 5:**
Multi-tenancy
Workspace-level tenancy modeled using workspace_id across entities for better collaboration and scalability.

AuthZ model
RBAC with ownership rules enforced via DRF permissions and query filtering.

Observability
Celery job logs, correlation IDs, metrics like job latency, failure rate and queue size tracked.

Data retention
Raw inputs deleted after 30 days. Generated artifacts retained 90 days. Logs retained 15 days.

Secrets & compliance
Secrets stored in environment/secret manager. Tokens encrypted at rest. Key rotation supported. PII fields masked in logs.

