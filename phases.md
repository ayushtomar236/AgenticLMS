# Agentic College LMS --- Features and Build Phases

**Purpose:** A practical implementation sequence for building the
platform from a secure academic foundation to an AI-powered college
learning and operations system.

**Planning assumption:** Small focused team, one pilot department, one
theory subject, and one programming subject to start. Time estimates are
indicative and should be adjusted after technical discovery.

------------------------------------------------------------------------

## Part 1 --- Complete feature inventory

This section lists the intended product scope before implementation
phases.

### A. Identity and academic administration

-   [ ] Login and account lifecycle
-   [ ] Role-based access for students, faculty, faculty mentors, department
    leave executives, HOD/coordinators, and administrators
-   [ ] Student-to-faculty mentor mapping
-   [ ] Department-specific HOD and Leave Executive role assignments
-   [ ] Department, program, semester, academic year, and course
    management
-   [ ] Course offerings and faculty allocation
-   [ ] Student enrollment
-   [ ] Subject and chapter structure
-   [ ] Learning outcomes and chapter prerequisites
-   [ ] Timetable and class-session management
-   [ ] Audit trail for important administrative actions

### B. Student learning workspace

-   [ ] Personalized student dashboard
-   [ ] Subject-wise chapter navigation
-   [ ] Notes, slides, links, and resource viewer
-   [ ] Upcoming classes and deadlines
-   [ ] Flashcard decks and review history
-   [ ] Assignment submission and status
-   [ ] Leave application submission with medical prescription upload
-   [ ] Multi-tier leave status tracking (Mentor -> HOD -> Approved/Rejected)
-   [ ] Chapter practice progress
-   [ ] Official chapter completion status
-   [ ] AI tutor chat
-   [ ] Revision recommendations and study plans
-   [ ] Accessibility and responsive mobile-friendly UI

### C. Faculty workspace

-   [ ] Faculty dashboard and class schedule
-   [ ] Mentor leave verification queue for assigned mentees
-   [ ] Prescription document inspector with attendance context
-   [ ] Mentor verification remarks and forward/reject actions
-   [ ] Upload, organize, version, and approve learning resources
-   [ ] Map resources to subjects, chapters, and learning outcomes
-   [ ] Create and manage assignments
-   [ ] Configure due dates, extensions, and late policies
-   [ ] Review AI-generated flashcards
-   [ ] Create and manage chapter labs
-   [ ] Create question banks, rubrics, and assessment sessions
-   [ ] Run classroom completion tests with access codes
-   [ ] View submissions, grades, and chapter progress
-   [ ] Review student-support flags and record interventions
-   [ ] Approve AI-generated question papers and reports

### D. Management and academic intelligence

-   [ ] Academic Command Center
-   [ ] Department-scoped HOD and Leave Executive decision console
-   [ ] Clinic blacklist management (flag/ban clinics issuing suspicious medicals)
-   [ ] Clinic certificate concentration monitoring and fake medical fraud alerts
-   [ ] Discretionary HOD approval override for blacklisted clinics with audit logging
-   [ ] Syllabus coverage versus planned schedule
-   [ ] Chapter completion and assessment overview
-   [ ] Assignment evaluation backlog
-   [ ] Department and course drill-down
-   [ ] Faculty workload and timetable conflict checks
-   [ ] Academic evidence repository
-   [ ] Learning-outcome mapping and report exports
-   [ ] Student early-warning support signals
-   [ ] Intervention tracking
-   [ ] AI usage, infrastructure health, and cost dashboard
-   [ ] Authorized report generation for academic reviews

### E. Flashcards and notification automation

-   [ ] Class-linked flashcard generation from approved material
-   [ ] Faculty review/approval workflow
-   [ ] Configurable pre-class delivery schedule
-   [ ] In-app notifications
-   [ ] Email reminders before assignment deadlines
-   [ ] Skip reminders when submitted or exempted
-   [ ] Extension and deadline-change handling
-   [ ] Retry, deduplication, and delivery logs
-   [ ] Notification preferences

### F. AI + RAG tutor

-   [ ] Document text extraction and chunking
-   [ ] Embedding generation and indexed storage
-   [ ] Semantic and keyword retrieval
-   [ ] Course- and role-scoped retrieval
-   [ ] Answers grounded in approved course material
-   [ ] Source citations where available
-   [ ] Teach, hint, practise, explain-code, and revision modes
-   [ ] Insufficient-evidence handling
-   [ ] Student feedback and quality evaluation
-   [ ] Assessment-aware restrictions
-   [ ] Usage, latency, and cost monitoring

### G. Programming labs and online execution

-   [ ] Chapter-wise coding exercises
-   [ ] Starter code and instructions
-   [ ] Code editor and language selection
-   [ ] Run and submit flows
-   [ ] Isolated execution service
-   [ ] Time, memory, process, output, and concurrency limits
-   [ ] Sample and hidden test cases
-   [ ] Compiler/runtime output and test results
-   [ ] AI debugging coach for permitted practice
-   [ ] Exercise grading and attempt history
-   [ ] Python first; add C++, Java, and JavaScript as validated
-   [ ] Abuse prevention and runner monitoring

### H. Classroom completion assessments

-   [ ] Faculty-created assessment sessions
-   [ ] Short-lived access codes
-   [ ] Student authentication and enrollment validation
-   [ ] Session start/end times and attempt limits
-   [ ] Theory and coding question support
-   [ ] Configurable AI-assistance policy
-   [ ] Server-side grading and pass threshold
-   [ ] Attempt history and authorized manual review
-   [ ] Official chapter completion record
-   [ ] Handling for network interruptions and approved retakes

### I. Student leave and medical verification system

-   [ ] Student leave application creation with leave type, date range, and reason
-   [ ] Mandatory medical prescription/certificate upload (PDF, JPEG, PNG)
-   [ ] Prescription metadata entry (clinic/hospital name, consulting doctor, registration number, issue date)
-   [ ] Secure file storage in object storage with virus/mime-type validation and presigned URLs
-   [ ] Automated routing to assigned faculty mentor (Tier 1 verification)
-   [ ] Mentor verification workspace with mentee attendance context and recommendation remarks
-   [ ] Routing to department HOD or designated Leave Executive (Tier 2 verification)
-   [ ] Department-level administrative boundary isolation for leave queues and approvals
-   [ ] Central management clinic blacklist registry (ban or flag fraudulent clinics)
-   [ ] Clinic submission frequency monitoring and fake certificate fraud alert triggers
-   [ ] Non-blocking warning banners on mentor and HOD review interfaces for blacklisted clinics
-   [ ] HOD discretionary final decision authority with mandatory justification note for overrides
-   [ ] Automated synchronization of approved leaves with official attendance registers
-   [ ] Immutable audit trail logging all submissions, reviews, clinic fraud flags, and approval overrides

### J. Premium and later-stage features

-   [ ] AI assessment/question-paper studio
-   [ ] Course outcome and program outcome mapping
-   [ ] Placement readiness tracks
-   [ ] Evidence-backed student skill passport
-   [ ] Parent/mentor portal subject to institutional policy
-   [ ] College operations assistant
-   [ ] Configurable academic workflows
-   [ ] Academic digital twin and scenario simulation
-   [ ] Existing LMS/ERP/SSO integration
-   [ ] Multi-college tenancy and institutional configuration

### K. Production readiness

-   [ ] Automated tests and CI/CD
-   [ ] Staging and production environments
-   [ ] Logging, metrics, traces, and alerts
-   [ ] Backup and restore drills
-   [ ] Security review and threat modelling
-   [ ] Privacy, retention, and access policies
-   [ ] Accessibility review
-   [ ] Load and sandbox security tests
-   [ ] Faculty onboarding and student support
-   [ ] Pilot evaluation and ROI report

------------------------------------------------------------------------

## Part 2 --- Phase-by-phase implementation

## Phase 0 --- Discovery and project setup

**Goal:** Confirm the pilot's requirements and create a reproducible
development foundation.

### Build

-   [ ] Identify pilot department, subjects, faculty, and student
    cohort.
-   [ ] Agree on roles, data access, chapter completion rules, and
    assessment policies.
-   [ ] Create a feature backlog and acceptance criteria.
-   [ ] Create repository and branching conventions.
-   [ ] Set up frontend and backend projects.
-   [ ] Configure environment variables and secret handling.
-   [ ] Set up Docker-based local development.
-   [ ] Set up linting, formatting, tests, and CI.
-   [ ] Create development, staging, and production configuration
    templates.
-   [ ] Seed synthetic academic data for local development.

### Deliverable

A runnable project skeleton with documented setup steps, CI checks, and
a prioritized MVP backlog.

### Exit criteria

A new developer can clone the repository, start the application, run
tests, and log in using a development-only account.

------------------------------------------------------------------------

## Phase 1 --- Identity, roles, and academic foundation

**Goal:** Establish the source of truth for users and academic
structure.

### Build

-   [ ] Login and logout.
-   [ ] Role and permission model.
-   [ ] Department, program, semester, and academic year records.
-   [ ] Course and course-offering records.
-   [ ] Faculty allocation and student enrollment.
-   [ ] Student-to-faculty mentor mappings and department leave executive
    role assignments.
-   [ ] Subjects, chapters, and chapter ordering.
-   [ ] Resource metadata and upload flow.
-   [ ] Student, faculty, and admin dashboard shells.
-   [ ] API validation and server-side authorization.
-   [ ] Database migrations and audit events for privileged actions.

### Deliverable

Users can sign in and see only the courses, subjects, and actions they
are authorized to access.

### Exit criteria

Permission tests prove that one student cannot read another student's
private records and that faculty cannot administer unrelated courses.

------------------------------------------------------------------------

## Phase 2 --- Course content and student learning workspace

**Goal:** Make the platform useful before introducing AI.

### Build

-   [ ] Subject-wise navigation.
-   [ ] Chapter pages and learning objectives.
-   [ ] Upload, version, approve, and view notes/slides.
-   [ ] Class sessions and upcoming-class view.
-   [ ] Basic practice progress tracking.
-   [ ] Faculty resource management.
-   [ ] Student dashboard with classes, subjects, and pending work.
-   [ ] Resource validation and secure file access.

### Deliverable

Faculty can organize a subject into chapters and students can access
approved learning material.

### Exit criteria

A pilot course can be configured end-to-end without editing the database
manually.

------------------------------------------------------------------------

## Phase 3 --- Assignments and dependable notifications

**Goal:** Automate deadlines with reliable, auditable workflows.

### Build

-   [ ] Assignment creation and editing.
-   [ ] Due dates, extensions, late policies, and submission status.
-   [ ] Student assignment submission.
-   [ ] Background worker and queue.
-   [ ] Configurable reminder offsets.
-   [ ] Email provider integration and in-app notifications.
-   [ ] Skip reminders for submitted/exempted work.
-   [ ] Retry and deduplication behavior.
-   [ ] Delivery history and failed-job view.
-   [ ] Time-zone-safe deadline handling.

### Deliverable

A faculty member can create an assignment and eligible students receive
reminders without duplicate or stale messages.

### Exit criteria

Tests cover submission before reminder, deadline extension, duplicate
job delivery, provider failure, and retry behavior.

------------------------------------------------------------------------

## Phase 4 --- Pre-class flashcards

**Goal:** Deliver approved, chapter-linked preparation material before
each class.

### Build

-   [ ] Link class sessions to chapters and approved resources.
-   [ ] Add LLM provider adapter.
-   [ ] Generate concise flashcards from authorized material.
-   [ ] Store source references and generation metadata.
-   [ ] Faculty review/edit/approve workflow.
-   [ ] Schedule deck publication and pre-class notifications.
-   [ ] Student flashcard review interface.
-   [ ] Track card review activity.
-   [ ] Handle changed content and failed generation jobs.

### Deliverable

A scheduled class can have a reviewed flashcard deck available to its
enrolled students before class.

### Exit criteria

Generated content is traceable to approved sources, faculty can edit it,
and the system does not send unreviewed cards when approval is required.

------------------------------------------------------------------------

## Phase 5 --- Subject-wise RAG tutor

**Goal:** Give students a course-aware tutor grounded in approved
college material.

### Build

-   [ ] Document extraction pipeline.
-   [ ] Chunking and source metadata.
-   [ ] Embedding generation.
-   [ ] PostgreSQL/pgvector index and keyword retrieval.
-   [ ] Course/role filters enforced before model context is assembled.
-   [ ] Tutor chat API and conversation UI.
-   [ ] Source citations and insufficient-evidence response.
-   [ ] Teach, hint, practise, and revision modes.
-   [ ] Feedback collection.
-   [ ] AI quality, latency, and cost evaluation.
-   [ ] Assessment-mode restrictions.

### Deliverable

A student can ask subject-specific questions and receive grounded
explanations with source references.

### Exit criteria

Evaluation confirms that retrieval respects course permissions, answers
cite sources where available, and the tutor acknowledges when approved
materials do not support an answer.

------------------------------------------------------------------------

## Phase 6 --- Programming labs and code execution

**Goal:** Enable chapter-wise coding practice in the LMS.

### Build

-   [ ] Choose and configure a hardened execution engine.
-   [ ] Deploy runner separately from the main API.
-   [ ] Add code execution gateway and queue.
-   [ ] Define CPU, memory, time, output, process, and concurrency
    limits.
-   [ ] Disable network access by default and restrict filesystem
    access.
-   [ ] Create lab exercise and test-case models.
-   [ ] Build code editor and run/submit UI.
-   [ ] Display compiler/runtime errors and test results.
-   [ ] Start with one language, preferably Python, then add others.
-   [ ] Store code-run history and exercise progress.
-   [ ] Add rate limits, abuse detection, and runner monitoring.
-   [ ] Add AI debugging help for practice mode only.

### Deliverable

Students can solve chapter-linked coding exercises and receive reliable
test feedback.

### Exit criteria

Resource-limit tests, concurrent-run tests, authorization tests, and
sandbox security review pass. Hidden test cases and expected answers are
never returned to clients.

------------------------------------------------------------------------

## Phase 7 --- Teacher-controlled classroom completion tests

**Goal:** Make official chapter completion depend on a controlled
assessment rather than practice activity alone.

### Build

-   [ ] Faculty creates an assessment from approved questions/exercises.
-   [ ] Configure duration, allowed attempts, languages, pass threshold,
    and AI policy.
-   [ ] Generate short-lived session access code.
-   [ ] Store code securely and support revocation.
-   [ ] Validate student identity, enrollment, session status, and
    expiry.
-   [ ] Lock assessment content to the session's policy.
-   [ ] Grade objective and coding responses server-side.
-   [ ] Record immutable attempt history.
-   [ ] Add faculty review and reasoned manual overrides.
-   [ ] Update official chapter completion only after the configured
    rule passes.
-   [ ] Support approved retakes and documented interruption handling.

### Deliverable

A teacher can run a classroom completion test and see verified results
and chapter completion status.

### Exit criteria

The access code alone cannot grant completion, students cannot access
hidden answers, duplicate submissions are handled safely, and all
overrides are auditable.

------------------------------------------------------------------------

## Phase 8 --- Management command center and early-warning support

**Goal:** Convert academic activity into actionable, explainable
management insights.

### Build

-   [ ] Syllabus progress versus planned schedule.
-   [ ] Chapter completion and assessment summaries.
-   [ ] Assignment and evaluation backlog.
-   [ ] Department/course drill-down.
-   [ ] Faculty timetable conflict checks.
-   [ ] Rule-based early-warning signals.
-   [ ] Evidence and explanation for every flag.
-   [ ] Intervention tracking.
-   [ ] Exportable academic reports.
-   [ ] Role-scoped analytics.
-   [ ] AI usage, email delivery, and code-runner health views.

### Deliverable

Management and authorized faculty can identify delays, understand the
evidence, and track follow-up actions.

### Exit criteria

Dashboard figures reconcile with source records, access is scoped, and
early-warning flags are explainable and correctable.

------------------------------------------------------------------------

## Phase 8b --- Student leave and medical verification workflow

**Goal:** Establish an auditable, multi-tier medical leave approval
process with prescription verification, clinic blacklisting, and
department-scoped executive governance.

### Build

-   [ ] Leave schema: `student_mentor_assignments`, `leave_applications`,
    `leave_attachments`, `medical_clinics`, `clinic_ban_records`,
    `department_leave_executives`, and `leave_approval_actions`.
-   [ ] Prescription document upload to object storage with virus scan,
    file validation (PDF/JPEG/PNG), and secure pre-signed URL generation.
-   [ ] Student leave request interface with start/end date range, leave
    type, justification, and doctor/clinic metadata fields.
-   [ ] Tier 1 mentor verification workspace: view mentee prescriptions,
    inspect attendance history and deficit, add recommendation remarks,
    and forward or reject.
-   [ ] Department-scoped routing: partition leave queues strictly by
    student department, routing only to that department's HOD or
    designated Leave Executives.
-   [ ] Central clinic blacklist management: administrative console for
    college management to search, flag, and ban clinics issuing
    suspicious medical certificates with documented ban reasons.
-   [ ] Anomaly detection & fake medical alerts: aggregate clinic
    submission volume and display non-blocking fraud warning banners on
    both mentor and HOD interfaces when a prescription originates from a
    blacklisted clinic.
-   [ ] Tier 2 HOD / Leave Executive decision console: review mentor
    recommendations and prescription details; exercise final
    discretionary approval or rejection.
-   [ ] Mandatory override audit logging: require the HOD to record a
    formal rationale when approving a leave associated with a blacklisted
    clinic.
-   [ ] Attendance reconciliation: automatically sync approved medical
    leaves with attendance records to mark officially excused absences.
-   [ ] Strict privacy controls and immutable audit log for all leave
    lifecycle actions.

### Deliverable

Students can apply for leave with medical prescriptions, mentors verify
and recommend, and department HODs retain final approval authority with
system-assisted warnings against fraudulent clinics.

### Exit criteria

End-to-end tests verify student submission, mentor endorsement, and HOD
decision. Security tests prove that HODs/executives from Department A
cannot access or approve leaves for Department B. Blacklist tests confirm
that submitting a prescription from a banned clinic surfaces clear fraud
warnings to both mentor and HOD, but preserves the HOD's final authority to
grant a reasoned discretionary approval. Prescription documents are
inaccessible to unauthorized users.

------------------------------------------------------------------------

## Phase 9 --- Pilot hardening and institutional rollout

**Goal:** Prove usability, security, reliability, and value before
expanding.

### Build

-   [ ] End-to-end student/faculty/admin test suite.
-   [ ] Security threat model and independent review where feasible.
-   [ ] Privacy and retention configuration.
-   [ ] Load tests for class-start and code-runner peaks.
-   [ ] AI evaluation and regression tests.
-   [ ] Backup restoration drill.
-   [ ] Alerts, dashboards, incident runbooks, and rollback procedure.
-   [ ] Faculty onboarding and support documentation.
-   [ ] Student feedback and accessibility review.
-   [ ] Baseline-versus-pilot measurement.
-   [ ] Pilot ROI and outcome report.

### Deliverable

A documented pilot with measured outcomes, known limitations, support
process, and a prioritized expansion plan.

### Exit criteria

The institution has approved data handling and deployment; key security,
reliability, and usability issues are resolved; and the pilot has
evidence of value.

------------------------------------------------------------------------

## Phase 10 --- Premium capabilities and integrations

Only start after the core platform is stable and pilot feedback supports
expansion.

### Candidate features

-   [ ] AI assessment/question-paper studio with faculty approval.
-   [ ] Course outcome/program outcome mapping and academic evidence
    packs.
-   [ ] Placement readiness paths and evidence-backed skill passport.
-   [ ] College operations assistant with approved tools.
-   [ ] Configurable workflows with human approval for consequential
    actions.
-   [ ] Parent/mentor portal where policy permits.
-   [ ] Academic scenario simulation/digital twin.
-   [ ] Existing LMS/ERP/SSO integration.
-   [ ] Multi-college tenancy and tenant-specific policies.
-   [ ] Advanced analytics and cost controls.

### Deliverable

A commercial-grade platform with a defined deployment model, support
model, institutional configuration, and pricing boundaries.

------------------------------------------------------------------------

## Part 3 --- Recommended first MVP scope

For the first pilot, do not build every feature in the inventory.

### Must-have

1.  Authentication and role-based access.
2.  Academic structure: course, subject, chapter, enrollment.
3.  Resource uploads and class sessions.
4.  Assignment deadlines and email reminders.
5.  Faculty-reviewed pre-class flashcards.
6.  RAG tutor for one or two subjects.
7.  One-language coding lab with a hardened runner.
8.  Teacher-controlled chapter completion assessment.
9.  Basic student/faculty progress views.
10. Minimum audit logging, monitoring, backups, and security tests.

### Defer until after pilot

-   Full timetable optimization.
-   Parent portal.
-   Digital twin and predictive analytics.
-   Multi-college tenancy.
-   Complex autonomous agents.
-   Large-scale ERP integrations.
-   Multiple code-runner languages if the first language is not yet
    secure and reliable.

## Part 4 --- Definition of done for every feature

A feature is not complete just because its UI works. Every feature
should include:

-   [ ] Database migration and data constraints.
-   [ ] API validation and permission checks.
-   [ ] Loading, empty, error, and success states in the UI.
-   [ ] Unit and integration tests.
-   [ ] Audit events for consequential actions.
-   [ ] Logging and operational visibility.
-   [ ] Accessibility and responsive behavior.
-   [ ] Documentation and acceptance criteria.
-   [ ] Security and privacy review appropriate to the feature.
-   [ ] Demonstration using synthetic data.

## Part 5 --- Suggested repository structure

``` text
agentic-lms/
├── apps/
│   └── web/                    # React + TypeScript
├── backend/
│   ├── app/
│   │   ├── api/                # Versioned route handlers
│   │   ├── core/               # Config, auth, permissions
│   │   ├── modules/
│   │   │   ├── users/
│   │   │   ├── academics/
│   │   │   ├── resources/
│   │   │   ├── classes/
│   │   │   ├── assignments/
│   │   │   ├── notifications/
│   │   │   ├── flashcards/
│   │   │   ├── tutor/
│   │   │   ├── labs/
│   │   │   ├── assessments/
│   │   │   ├── leaves/
│   │   │   └── analytics/
│   │   ├── workers/            # Background jobs
│   │   ├── integrations/       # Email, LLM, storage, runner
│   │   └── main.py
│   ├── migrations/
│   └── tests/
├── infrastructure/
│   ├── docker/
│   └── aws/
├── docs/
│   ├── architecture.md
│   └── phases.md
├── scripts/
├── .env.example
├── docker-compose.yml
└── README.md
```

This is a suggested structure, not a requirement to create every module
immediately. Add modules as their phase begins.

## Part 6 --- First implementation sprint

Start with these tasks in order:

1.  Create the repository and document local setup.
2.  Run React and FastAPI locally using Docker Compose.
3.  Connect PostgreSQL and create the first migration.
4.  Implement user, role, department, course, subject, and chapter
    models.
5.  Implement authentication and authorization tests.
6.  Create a minimal student dashboard and faculty course-management
    screen.
7.  Seed one sample programming subject with three chapters.
8.  Demonstrate that a faculty member can create a chapter and an
    enrolled student can view it.
9.  Review the implementation before moving to assignments and
    notifications.

**Guiding rule:** Build one complete, tested academic workflow at a
time. Keep the first release small enough to validate with real faculty
feedback, but establish permissions, auditability, and secure execution
boundaries from the beginning.
