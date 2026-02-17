
# **Full-Stack Developer**

**Evaluation Criteria**

* Clarity + practicality of architecture
* Clean data model + API design
* Async job + storage thinking (uploads, outputs, retries)
* Security basics (auth, access control, safe downloads)
* Cost + scalability tradeoffs (MVP → v1)
* Ability to handle ambiguity + user review flows

---

## **Problem 1:** **Video-to-Notes Platform (Architecture Proposal)**

We have long videos (3–4 hours, 200MB+). We need an automated “summary package” per video: **Summary.md + highlights (timestamps) + screenshots/clips references**, organized per video. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Task (No code)**

Create a concise architecture proposal for an MVP.

**Your Solution must include**

* Minimal user flow (3–5 steps)
* High-level architecture diagram (UI, API, DB, worker/queue, storage, AI)
* Job lifecycle (queued → processing → success/failed) + progress reporting
* Data model (tables/entities only)
* API list (8–12 endpoints)

**Your Solution for problem 1:**

You need to put your solution here.

---

## **Problem 2:** **LinkedIn Automation Platform (Architecture + Prompt Spec)**

User connects LinkedIn, defines persona/tone, provides topics. System generates **3 post drafts**, user approves, schedules, and posts automatically. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Task (No code)**

Provide:

1. **Architecture proposal** for MVP (auth, scheduling, approvals, posting).
2. How to store prompts provide by GenAI team

**Your Solution must include**

* Minimal flow: connect → persona → generate → approve → schedule → post
* Architecture blocks + key integrations (LinkedIn, LLM, scheduler)
* Data model (User, Persona, Draft, Schedule, PostLog)
* API list (8–12 endpoints)

**Your Solution for problem 2:**

You need to put your solution here.

---

## **Problem 3:** **DOCX Template → Bulk DOCX/PDF Generator Architecture**

Users upload Word templates, system detects editable fields, supports **single fill** and **bulk generation via CSV/Sheet**, exports DOCX/PDF, provides ZIP + report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Task (No code)**

Provide MVP architecture + LLM prompt spec for:

* Template field detection
* Field schema generation (types, required, validations)

**Your Solution must include**

* Flow: upload template → field review → single generate → bulk generate
* Architecture blocks (template parser, worker, storage, export service)
* Data model (Template, TemplateField, BulkRun, RowResult, Artifact)
* Bulk report format (success/fail + reason)

**Your Solution for problem 3:**

You need to put your solution here.

---

## **Problem 4:** **Character-Based Video Series Generator (Architecture Proposal)**

User defines characters once (image + personality + relationships). For each episode, user provides a short story/situation. System outputs an “episode package” (script, scenes, assets list, voiceover plan) and optionally a final video.  [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Task (No code)**

Create a small architecture proposal for MVP.

**Your Solution must include**

* Data model for Character, Relationship, Episode, Scene, Asset
* Pipeline flow: story → scene breakdown → dialogues → asset plan → render plan
* Consistency strategy (character memory, style guide, asset reuse)
* MVP scope vs v1 scope (what you would ship first)

**Your Solution for problem 4:**

You need to put your solution here.
