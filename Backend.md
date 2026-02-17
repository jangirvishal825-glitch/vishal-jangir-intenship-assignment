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

**Your Solution for problem 1:**

You need to put your solution here.

---

## **Problem 2: LinkedIn Automation Platform (Backend Architecture)**

**Goal:** Connect LinkedIn → store persona → generate drafts (handled by GenAI team) → approve → schedule → auto-post + audit logs. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Your solution must include**

* **System design:** OAuth flow, token storage/refresh, scheduler/worker design
* **Schema:** User, LinkedInAccount, Persona, Draft, Schedule, PostAttempt/PostLog
* **Security:** encrypt tokens at rest, least-privilege scopes, access control
* **Reliability:** retry posting, dedupe to prevent double-post, rate limiting
* **Prompt/config storage proposal:** how backend stores “prompt versions” or “config packs” provided by GenAI team (DB vs repo vs hybrid, versioning + rollback)

**Your Solution for problem 2:**

You need to put your solution here.

---

## **Problem 3: DOCX Template → Bulk DOCX/PDF Generator (Backend + Storage)**

**Goal:** Upload DOCX template → detect fields → single fill export → bulk fill via CSV/Sheet → ZIP + per-row report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Your solution must include**

* **System design:** template ingestion, field extraction service, bulk job worker, export service
* **Schema:** Template, TemplateVersion, TemplateField, BulkRun, BulkRow, Artifact, JobEvent
* **Bulk storage strategy:** where CSV input + generated docs + ZIP live, cleanup policy
* **Reliability:** partial success handling, per-row status, retries, resumable bulk run
* **Security:** template isolation per tenant/user, safe downloads, anti-path traversal

**Your Solution for problem 3:**

You need to put your solution here.

---

## **Problem 4: Character-Based Video Series Generator (Backend Architecture)**

**Goal:** Define characters once (image + traits + relationships). For each episode story → output episode package (script/scenes/assets plan/render plan), optionally render. [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Your solution must include**

* **System design:** episodic pipeline as jobs, asset management, consistency strategy storage
* **Schema:** Character, Relationship, Episode, Scene, Asset, RenderJob, Artifact
* **Consistency data:** what gets persisted to keep character continuity across episodes
* **Storage:** images/audio/video assets, versioning, dedupe strategy
* **Security + cost controls:** quotas, rate limits, large asset constraints

**Your Solution for problem 4:**

You need to put your solution here.

## Problem 5: Cross-Cutting

Answer briefly for the whole platform:

1. **Multi-tenancy:** user-level vs workspace-level, how you model it in schema
2. **AuthZ model:** RBAC or simple ownership rules, and where enforced (API + DB)
3. **Observability:** what you log for jobs + correlation ids + minimal metrics
4. **Data retention:** what to delete and when (inputs, artifacts, logs)
5. **Secrets & compliance:** token encryption, key management approach, PII handling

**Your Answer for problem 5:**

You need to put your solution here.
