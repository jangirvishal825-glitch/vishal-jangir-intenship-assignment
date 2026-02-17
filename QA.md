# **QA / Testing Engineer Assignment (No Code, Test Strategy Focus)**

## **Evaluation Criteria**

* Test plan quality (coverage, priorities, risk-based thinking)
* Edge cases and negative testing depth
* Test data strategy (PII-safe, reproducible datasets)
* Automation approach (what/where to automate, tooling choice)
* API + UI testing strategy (contracts, mocks, stubs)
* Performance + reliability testing for async jobs
* Security testing basics (auth, access control, safe downloads)
* Debuggability (logs, repro steps, bug reports quality)

---

## **Problem 1: Video-to-Notes Platform (QA Plan)**

**Goal:** Upload video → async job → outputs: transcript, **Summary.md**, highlights with timestamps, clip/screenshot references. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Your solution must include**

* **Test plan (high level):** smoke, functional, regression, exploratory
* **Critical test cases:** upload, job lifecycle, retry, partial outputs, download artifacts
* **Edge cases:** large file, unsupported format, network drop mid-upload, duplicate submit, stuck job, timeout from AI service
* **Validation:** highlights timestamps format, markdown rendering, ordering, missing sections
* **Automation scope:** what to automate (API job flows, UI status, downloads), what stays manual
* **Observability needs:** what logs/ids QA needs to debug failures

**Your Solution for problem 1:**

You need to put your solution here.

---

## **Problem 2: LinkedIn Automation Platform (QA Plan)**

**Goal:** Connect LinkedIn → persona → drafts → approve → schedule → auto post + audit logs. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Your solution must include**

* **OAuth tests:** connect, denied consent, expired token, refresh token failure, revoked access
* **Scheduling tests:** timezone correctness, daylight savings edge, duplicate schedules, missed runs, retry behavior
* **Posting tests:** rate limits, API errors, content validation, double post prevention
* **Auditability:** PostLog correctness (attempts, status, error messages)
* **Automation:** API-level scheduling tests + mocked LinkedIn API, UI tests for approval flow

**Your Solution for problem 2:**

You need to put your solution here.

---

## **Problem 3: DOCX Template → Bulk Generator (QA Plan)**

**Goal:** Upload template → detect fields → single fill → bulk fill via CSV → ZIP + per-row report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Your solution must include**

* **Template ingestion tests:** corrupted docx, password protected, huge templates, weird formatting
* **Field detection tests:** false positives/negatives, duplicate field names, invalid field types
* **Single generate tests:** required fields missing, type mismatch, output file integrity
* **Bulk tests:** CSV with missing columns, extra columns, wrong encodings, commas/quotes, 10k rows (design test), partial success
* **Report correctness:** row-level status + error reasons
* **Download tests:** ZIP integrity, file naming, safe access control

**Your Solution for problem 3:**

You need to put your solution here.

---

## **Problem 4: Character-Based Video Series Generator (QA Plan)**

**Goal:** Define characters once → generate episode package (script/scenes/assets plan/render plan) with character consistency. [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Your solution must include**

* **Consistency tests:** character traits stable across episodes, relationship references correct
* **Episode pipeline tests:** invalid story input, missing characters, conflicting instructions, partial outputs
* **Asset tests:** image upload validation, duplicates, versioning, missing asset references
* **Long-running jobs:** resume job, retry, cancel, progress reporting accuracy

**Your Solution for problem 4:**

You need to put your solution here.

---

## **Cross-Cutting (Mandatory, Max 1 page)**

Answer in bullets:

1. **Test types & priority**

* What is smoke vs regression vs exploratory here?
  `
  <EDIT YOUR ANSWER HERE>`

2. **Automation plan**

* What to automate first and why (API vs UI vs integration)
* Tooling preference (examples: Playwright/Cypress, Postman/Newman, REST-assured, k6/JMeter)
* Mocking strategy for AI/LinkedIn/external services

   `<EDIT YOUR ANSWER HERE>`

3. **Edge case checklist**

* Async jobs (retries, idempotency, race conditions)
* File handling (uploads/downloads, corruption, permissions)
* Multi-user access control (isolation testing)

  `<EDIT YOUR ANSWER HERE>`

4. **Performance & reliability**

* Upload performance
* Job queue throughput test approach
* Soak tests for scheduler/posting

  `<EDIT YOUR ANSWER HERE>`

5. **Bug reporting template**

* What must every bug include (steps, expected/actual, logs, correlation id, sample input)

  `<EDIT YOUR ANSWER HERE>`
