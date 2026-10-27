# Agentic College LMS --- Architecture

**Status:** Initial technical architecture\
**Audience:** Product, engineering, college IT, and implementation
teams\
**Scope:** A modular college learning and academic-operations platform

## 1. Product goals

Build one platform for students, faculty, and college management that
combines academic administration with personalized learning and
controlled automation.

Primary outcomes: - Give students subject-wise resources, pre-class
flashcards, assignments, AI tutoring, programming labs, and chapter
completion tests. - Help faculty plan classes, manage learning content,
create assessments, monitor progress, and conduct classroom tests. -
Provide transparent, auditable student leave applications with prescription
verification, multi-tier approvals (mentor to HOD), clinic blacklisting, and
department-scoped governance. - Give authorized management staff visibility
into syllabus progress, assessment completion, operational issues, and
academic evidence. - Automate repetitive workflows with auditable,
permission-controlled services. - Keep official academic records
deterministic and explainable; AI recommendations must not silently alter
grades, attendance exemptions, or completion status.

## 2. Architecture principles

1.  **Modular monolith first:** one primary backend organized by domain
    modules. Split services only when scale, ownership, or security
    isolation justifies it.
2.  **PostgreSQL is the source of truth:** authoritative academic
    records, permissions, deadlines, attempts, and grades live in the
    relational database.
3.  **AI is an assistive layer:** LLMs generate drafts, retrieve course
    material, explain concepts, and recommend interventions.
    Deterministic services enforce policy and make official decisions.
4.  **Least privilege:** every API and agent tool checks identity, role,
    institutional scope, enrollment, and resource permissions.
5.  **Async work for slow tasks:** email, document ingestion, flashcard
    generation, and other retryable jobs run through background workers.
6.  **Secure code execution:** student code runs in a separate, hardened
    sandbox service, never inside the main API process.
7.  **Observable and auditable:** important actions, job outcomes,
    assessment changes, and administrative overrides are logged.
8.  **Provider-agnostic AI:** use a model adapter so the LLM provider
    can change without rewriting the product.

## 3. High-level system

``` mermaid
flowchart TD
    S[Student Web App] --> FE[React + TypeScript Frontend]
    F[Faculty Web App] --> FE
    M[Management Console] --> FE
    FE --> API[FastAPI Application / REST API]
    API --> AUTH[Identity and RBAC]
    API --> PG[(PostgreSQL)]
    API --> OBJ[Object Storage]
    API --> Q[Background Job Queue]
    Q --> W[Worker Processes]
    W --> EMAIL[Email Provider]
    W --> AI[AI Orchestration and RAG]
    AI --> LLM[LLM Provider Adapter]
    AI --> VDB[(pgvector / PostgreSQL)]
    API --> RUNNER[Code Execution Gateway]
    RUNNER --> SANDBOX[Isolated Code Runner]
    API --> OBS[Logs, Metrics, Traces, Audit Events]
    W --> OBS
    RUNNER --> OBS
```

### Main request flow

-   Browser calls the REST API over HTTPS.
-   API authenticates the user and authorizes each requested operation.
-   PostgreSQL stores canonical academic records.
-   Object storage holds uploaded documents and generated artifacts.
-   A queue processes non-interactive work with retries and idempotency.
-   AI orchestration retrieves only authorized material and calls an
    approved model.
-   The code execution gateway sends submissions to an isolated runner
    with strict resource limits.

## 4. Recommended technology stack

  -----------------------------------------------------------------------
  Layer                   Recommended choice      Responsibility
  ----------------------- ----------------------- -----------------------
  Frontend                React, TypeScript, Vite Student, faculty, and
                                                  management web
                                                  experiences

  Styling                 Tailwind CSS and a      Accessible, responsive
                          consistent component    interface
                          library                 

  API                     Python + FastAPI +      Validation, REST
                          Pydantic                endpoints, application
                                                  services

  ORM/migrations          SQLAlchemy + Alembic    Database access and
                                                  schema evolution

  Database                PostgreSQL              Users, courses,
                                                  schedules, submissions,
                                                  assessments,
                                                  permissions, audit
                                                  metadata

  Vector search           pgvector in PostgreSQL  Embedding search for
                          initially               RAG

  Keyword search          PostgreSQL full-text    Exact terms,
                          search initially        identifiers, and hybrid
                                                  retrieval

  File storage            AWS S3 or compatible    PDFs, slides, images,
                          object storage          exports, lab assets

  Background work         Celery + Redis for      Flashcards, document
                          initial deployment      processing, scheduled
                                                  reminders, retries

  Durable queue option    Amazon SQS where        Managed queueing for
                          appropriate             AWS deployment

  Email                   Amazon SES or           Transactional academic
                          institution-approved    email
                          provider                

  Identity                College SSO if          Login, MFA where
                          available; otherwise a  appropriate, account
                          maintained identity     lifecycle
                          provider                

  AI                      Provider adapter +      RAG answers,
                          approved LLM and        flashcards, summaries,
                          embedding models        hints

  Code execution          Judge0-compatible       Compile/run code and
                          runner or equivalent    evaluate tests outside
                          hardened sandbox        the API

  Containers              Docker for application  Repeatable development
                          packaging               and deployment

  AWS deployment          ECS/Fargate, RDS        Managed production
                          PostgreSQL, S3,         infrastructure
                          SQS/Redis, SES,         
                          CloudWatch, Secrets     
                          Manager                 

  Testing                 Pytest, frontend unit   Regression and
                          tests, API integration  end-to-end validation
                          tests, Playwright       

  Observability           Structured logs,        Reliability and cost
                          metrics, traces, error  visibility
                          monitoring              

  CI/CD                   GitHub Actions or       Lint, test, build,
                          equivalent              scan, deploy
  -----------------------------------------------------------------------

**Keep the first version simple.** Do not introduce Kubernetes, multiple
vector databases, graph databases, or many autonomous agents before
there is a concrete requirement.

## 5. Application modules

### 5.1 Identity and access control

-   Users, roles, department memberships, course enrollment, account
    status.
-   Suggested roles: student, faculty, faculty mentor, department leave
    executive, HOD/academic coordinator, institution admin, support operator.
-   Student-to-mentor assignment mappings and department-level
    approval authority delegation.
-   Enforce permissions on the server for every resource and action.
-   Scope management access and leave approval queues by department and
    responsibility.
-   Prefer college SSO if the institution supports it.

### 5.2 Academic structure

-   Institution, department, program, semester, academic year, course
    offering.
-   Subject, chapter, learning outcome, prerequisite relationship.
-   Faculty allocation, enrollment, class session, timetable.
-   Keep course definitions separate from a particular semester's course
    offering.

### 5.3 Learning resources

-   Faculty uploads approved notes, slides, PDFs, and links.
-   Store file metadata, version, uploader, approval state, and
    course/chapter association.
-   Scan uploads, validate file types and sizes, and process documents
    asynchronously.
-   Preserve source references so RAG responses can cite the relevant
    file and page/chunk where possible.

### 5.4 Pre-class flashcards

-   Trigger generation when a class session is published or its linked
    content changes.
-   Generate a configurable number of cards from approved material.
-   Store source references and generation/model metadata.
-   Faculty reviews/edits/approves the deck before it is distributed,
    according to institutional policy.
-   Notify enrolled students at configurable times before class.
-   Record review activity separately from official grades.

### 5.5 Assignments and deadline reminders

-   Faculty creates an assignment with instructions, due date, late
    policy, and submission rules.
-   Scheduler creates reminder jobs based on configurable offsets.
-   Before sending, check enrollment, submission status, extensions,
    preferences, and assignment state.
-   Use idempotency keys to avoid duplicate reminders.
-   Record queued, sent, failed, retried, and skipped outcomes.
-   Keep deadline changes and extensions auditable.

### 5.6 RAG subject tutor

1.  Ingest approved documents.
2.  Extract text and metadata; preserve source and page information.
3.  Split into meaningful chunks.
4.  Generate embeddings and store them with course/chapter/permission
    metadata.
5.  Retrieve with a combination of semantic and keyword search.
6.  Filter results by user authorization before they reach the model.
7.  Generate an answer grounded in retrieved sources.
8.  Return citations and say when evidence is insufficient.
9.  Evaluate retrieval relevance, correctness, latency, and cost.

Tutor modes can include teach, hint, practise, explain code, revision
plan, and test-my-understanding. The tutor should respect assessment
mode and must not disclose restricted questions or answers.

### 5.7 Programming labs and execution

-   Chapter-wise exercises, starter code, language, instructions, sample
    tests, hidden tests, hints, and grading rubric.
-   Editor supports run and submit workflows.
-   Practice runs may expose selected test results; graded runs must
    keep hidden tests and expected answers server-side.
-   API submits jobs to the execution gateway; gateway forwards them to
    a separate sandbox.
-   Enforce CPU/time, memory, process, output, disk, concurrency, and
    queue limits.
-   Disable outbound network access by default; run as non-root with
    restricted filesystem and short-lived workspaces.
-   Rate-limit submissions and isolate workloads.
-   Do not assume a basic container alone is a sufficient security
    boundary. Review the runner's isolation model and host hardening
    before production.

### 5.8 Classroom completion assessments

-   Faculty creates a session linked to a course offering and chapter.
-   Configure question set, duration, permitted languages, attempts,
    pass threshold, and AI assistance policy.
-   Generate a short-lived, high-entropy access code; store a hash
    rather than the raw code where practical.
-   Authenticate the student and validate enrollment, session state,
    expiry, and attempt limits.
-   Code grants entry only; it never directly grants completion.
-   Grade objective questions and coding tests on the server.
-   Store immutable attempt history and record authorized manual
    overrides with reasons.
-   Update official chapter completion only when the configured rule is
    satisfied.
-   Keep practice progress, estimated mastery, and official completion
    as separate concepts.

### 5.9 Analytics and early-warning support

-   Begin with transparent rules, such as repeated failed chapter tests
    or multiple overdue submissions.
-   Show the evidence and reason behind each flag.
-   Recommend human-led interventions, revision material, or mentoring.
-   Avoid opaque or high-stakes predictions; provide correction paths
    for inaccurate data.
-   Restrict identifiable student-level views to authorized staff.

### 5.10 Management and academic operations

-   Academic command center: syllabus progress, pending assessments,
    completion rates, and unresolved operational issues.
-   Course-outcome mapping and evidence repository for authorized
    academic reviews.
-   Faculty workload/timetable conflict checks.
-   AI question-paper drafting with faculty approval.
-   Clinic oversight and fake medical fraud mitigation: track clinic
    certificate volume, flag suspicious medical facilities, and maintain
    an institution-level clinic blacklist.
-   Placement skill evidence and student portfolio as later modules.
-   AI usage, delivery failures, code-runner health, and service cost
    dashboards.

### 5.11 Student leave and medical verification system

-   **Prescription and leave application intake:**
    -   Students apply for leave with start/end dates, session impact,
        reason, and mandatory medical prescription/certificate upload
        (PDF, JPEG, PNG).
    -   Capture structured prescription metadata: clinic/hospital name,
        consulting doctor name, medical registration/license number,
        and issue date.
    -   Prescriptions are stored in object storage with mime-type/virus
        validation and served via short-lived, pre-signed URLs.
-   **Multi-tier verification workflow:**
    -   *Tier 1 (Faculty Mentor Verification):* The leave request is
        automatically routed first to the student's designated faculty
        mentor. The mentor reviews the prescription, verifies attendance
        impact, adds verification remarks (recommend approval, recommend
        rejection, or request physical document verification), and
        forwards the application.
    -   *Tier 2 (Department HOD / Leave Executive Verification):*
        Following mentor verification, the request enters the
        department's executive review queue for final decision.
-   **Department-scoped authority and delegation:**
    -   Each academic department has its own Head of Department (HOD)
        and designated Leave Executives authorized to approve leaves.
    -   Approval queues and records are strictly segregated by
        department; personnel cannot review or decide leaves for
        students belonging to other departments.
-   **Clinic blacklisting and fraud mitigation:**
    -   Management and college administration can flag or ban (blacklist)
        specific clinics, hospitals, or practitioners when repeated
        suspicious medical certificates or fraudulent patterns are detected.
    -   The system tracks clinic submission frequencies across the
        student body, generating fraud alerts when an anomalous
        concentration of certificates originates from a single clinic.
-   **HOD final decision authority and discretionary override:**
    -   When a student prescription originates from a banned or flagged
        clinic, the system attaches a prominent fraud warning badge with
        historical reasons on both mentor and HOD verification screens.
    -   The system does *not* auto-reject or hard-block the leave; the
        **final decision authority strictly lies in the hands of the
        department HOD (or authorized Leave Executive)**.
    -   The HOD retains full discretion to approve the leave (requiring a
        mandatory recorded audit reason/justification), reject it, or
        require a physical evaluation by the college medical officer.
-   **Attendance reconciliation and audit trail:**
    -   Approved medical leaves automatically adjust official attendance
        registers to reflect excused medical absences.
    -   An immutable audit log records every action: submission, mentor
        endorsements, clinic fraud warning triggers, and HOD decisions
        or discretionary overrides.

## 6. Initial data model

Core entities and relationships:

-   `users`
-   `roles`, `user_role_assignments`, `department_memberships`
-   `departments`, `programs`, `semesters`, `academic_years`
-   `courses`, `course_offerings`, `course_enrollments`
-   `subjects`, `chapters`, `learning_outcomes`, `chapter_prerequisites`
-   `learning_resources`, `resource_versions`, `document_chunks`
-   `class_sessions`, `flashcard_decks`, `flashcards`,
    `flashcard_reviews`
-   `assignments`, `assignment_submissions`, `submission_files`
-   `lab_exercises`, `lab_test_cases`, `code_runs`
-   `assessment_sessions`, `assessment_questions`,
    `assessment_attempts`, `assessment_responses`
-   `chapter_progress`, `chapter_completions`
-   `student_mentor_assignments`
-   `leave_applications`, `leave_attachments`
-   `medical_clinics`, `clinic_ban_records`
-   `department_leave_executives`, `leave_approval_actions`
-   `notifications`, `notification_jobs`, `notification_delivery_events`
-   `ai_conversations`, `ai_messages`, `ai_feedback`
-   `intervention_flags`, `intervention_actions`
-   `audit_events`

Use UUIDs or another consistent opaque identifier strategy. Add foreign
keys, indexes, unique constraints, and tenant/institution scoping as
needed. Keep official results and attempt history separate from mutable
UI state.

## 7. API organization

Use versioned REST endpoints, for example `/api/v1`.

Suggested domains: - `/auth`, `/users`, `/roles` - `/departments`,
`/programs`, `/semesters` - `/courses`, `/course-offerings`,
`/enrollments` - `/subjects`, `/chapters`, `/resources` - `/classes`,
`/flashcards` - `/assignments`, `/submissions` - `/leaves`, `/clinics`,
`/mentors`, `/department-leave-executives` - `/tutor/conversations`,
`/tutor/messages`, `/tutor/feedback` - `/labs`, `/code-runs` -
`/assessment-sessions`, `/assessment-attempts` - `/progress`,
`/analytics` - `/notifications`, `/admin`, `/audit`

Rules: - Validate input and output with Pydantic schemas. - Keep
authorization in backend services, not only frontend route guards. - Use
pagination and filters for lists. - Use idempotency keys for repeatable
creation/notification operations. - Use explicit state transitions for
assignments, sessions, attempts, completion records, and leave applications.
- Enforce multi-tier state transitions for leave requests (e.g. `draft` ->
`submitted` -> `mentor_verified` -> `approved` / `rejected` /
`clarification_requested`). - Restrict medical prescription document access
strictly to the student applicant, assigned mentor, and department HOD/leave
executives. - Never return hidden tests, answer keys, private course content,
or unauthorized student records.

## 8. Agent design and tool permissions

Treat agents as constrained capabilities, not unrestricted autonomous
programs.

  -----------------------------------------------------------------------
  Agent/capability        Allowed tools           Restricted actions
  ----------------------- ----------------------- -----------------------
  Preparation agent       Read approved chapter   Cannot publish without
                          material; draft         configured approval
                          flashcards              

  RAG tutor               Search authorized       Cannot access another
                          resources; answer;      course's private
                          create practice         content
                          questions               

  Coding coach            Read permitted          Cannot bypass
                          exercise/code output;   graded-assessment
                          explain errors          policy

  Learning insights       Read authorized         Cannot change grades or
                          progress aggregates;    make final academic
                          suggest interventions   decisions

  Operations assistant    Read approved           Must confirm before
                          policy/timetable data;  side effects
                          draft requests/tickets  

  Reminder workflow       Read assignment state;  Cannot invent deadlines
                          send eligible reminders or recipients

  Grading service         Evaluate configured     Must not delegate
                          rubric/tests            authoritative scoring
                                                  to free-form LLM
                                                  judgement alone

  Leave and clinic        Read leave metadata and Cannot approve or reject
  fraud monitor           clinic registry; flag   leaves; cannot ban
                          suspicious certificate  clinics without
                          frequency and           management confirmation;
                          blacklisted clinics     cannot disclose medical
                                                  data outside workflow
  -----------------------------------------------------------------------

Log tool calls and side effects. Use allowlisted tools, timeouts,
token/cost limits, and human confirmation for consequential actions.

## 9. Security, privacy, and compliance

-   HTTPS in transit and encryption at rest.
-   Secure session handling, password/identity-provider protections, and
    MFA for privileged accounts where feasible.
-   Role-based and resource-level authorization, with department/course
    scoping.
-   Secret storage through a managed secret service; never commit
    credentials.
-   Audit logs for grade changes, access-code creation/revocation, role
    changes, exports, and administrative overrides.
-   Backups and tested restore procedures.
-   Data retention and deletion rules for student records, chat history,
    generated artifacts, and logs.
-   Review model-provider data handling and retention terms before
    sending any student information.
-   Protect against prompt injection in uploaded documents; retrieved
    content is data, not instructions.
-   Restrict uploads and scan for malicious files.
-   Medical record privacy: uploaded prescriptions and medical diagnoses
    are classified as sensitive student records; access is strictly
    restricted to the applicant student, assigned mentor, and authorized
    department HOD/leave executive, with audit logs recorded on access.
-   Rate-limit AI and code execution; define usage quotas.
-   Review applicable Indian data-protection requirements, institutional
    IT policies, contracts, notices, and consent requirements with the
    college.
-   Pilot with synthetic data until deployment and data-handling
    approvals are in place.

## 10. Testing and reliability

Required test layers: - Unit tests for business rules and permission
checks. - Integration tests for database transactions, queue jobs, email
provider, and AI retrieval. - End-to-end tests for student/faculty/admin
workflows. - Authorization tests proving students cannot access another
student's submissions or restricted course material. - Assessment tests
for expiry, duplicate attempts, hidden tests, interruptions, and manual
overrides. - Sandbox security and resource-limit tests. - Load tests for
peak class starts and simultaneous code submissions. - AI evaluation set
for source relevance, correctness, refusal when unsupported, and
assessment-mode policy. - Backup restore and incident-response
exercises.

## 11. Deployment environments

Maintain separate development, staging, and production environments.

-   **Development:** local Docker Compose, seeded synthetic data, mocked
    email where practical.
-   **Staging:** production-like services and test identities; no real
    student data unless approved.
-   **Production:** managed database, private networking where possible,
    restricted service permissions, backups, monitoring, alerting, and
    documented release/rollback steps.

Deploy incrementally. Start as a modular monolith and separate the code
runner and background workers because they have distinct security and
scaling requirements.

## 12. Key architecture decisions

-   PostgreSQL is the canonical data store.
-   pgvector and PostgreSQL full-text search are the initial RAG
    retrieval path.
-   FastAPI is the primary backend.
-   React/TypeScript is the primary web frontend.
-   Background jobs handle reminders and generation tasks.
-   The code runner is a separately isolated service.
-   Official completion and grading use deterministic, server-side
    rules.
-   Management analytics are based on validated data with role-scoped
    access.
-   Multi-tier leave verification with HOD final authority: medical
    leaves follow a sequential mentor-to-HOD verification pipeline
    scoped strictly by academic department. Management clinic bans surface
    fraud warning alerts, but final approval discretion remains with the
    HOD with mandatory audit logging.
-   Integrations and multi-college tenancy should be designed
    deliberately before offering the product to multiple institutions.
