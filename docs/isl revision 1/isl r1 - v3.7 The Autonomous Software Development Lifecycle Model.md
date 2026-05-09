# IMHOTEP Specification Language (ISL) v3.7

# The Autonomous Software Development Lifecycle Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1, ISL v3.2, ISL v3.3, ISL v3.4, ISL v3.5, ISL v3.6
**Supersedes:** ISL r0 v3.7 The Autonomous Software Development Lifecycle Model
**Document Type:** Autonomous SDLC Governance, Control, and Evidence Specification

---

## 1.0 Scope

This document defines the Autonomous Software Development Lifecycle Model for the IMHOTEP platform. It specifies how IMHOTEP organizes autonomous construction activity into a full enterprise SDLC, including lifecycle phases, phase gates, human roles, platform roles, controlled artifacts, evidence requirements, quality controls, governance controls, operational feedback, release readiness, change management, and lifecycle conformance.

This document does not redefine the Autonomous Development Loop. ISL v3.6 defines the loop-level control cycle for generation, validation, repair, convergence, packaging, deployment preparation, and re-entry. This document defines the broader lifecycle framework that governs when loops begin, what lifecycle phase they support, what evidence must be produced, who may approve transitions, how releases are controlled, how operations feed back into specifications, and how the platform’s autonomous activity maps to enterprise SDLC obligations.

This document defines:

* autonomous SDLC principles
* lifecycle phase model
* lifecycle phase gates
* lifecycle roles and responsibilities
* lifecycle evidence model
* mapping between traditional SDLC phases and IMHOTEP subsystems
* specification governance within the SDLC
* architecture and design governance
* implementation governance
* verification and validation governance
* security and compliance governance
* release and deployment readiness
* operations and monitoring feedback
* maintenance and continuous evolution
* lifecycle metrics
* lifecycle audit requirements
* lifecycle error classes
* conformance requirements

---

## 2.0 Purpose

The purpose of the Autonomous SDLC Model is to define how IMHOTEP fits into, replaces, augments, and governs the software development lifecycle at enterprise scale. Traditional SDLC processes rely on human teams to move requirements through design, implementation, testing, release, operations, and maintenance. IMHOTEP transforms many of those activities into autonomous platform functions performed by specifications, reasoning agents, deterministic tools, runtime orchestration, artifact repositories, traceability graphs, and governance controls.

The Autonomous SDLC Model exists to ensure that autonomous construction remains accountable as a software lifecycle, not merely successful as a code-generation workflow. A generated system is not enterprise-ready simply because it compiles or passes tests. It must have approved specifications, governed readiness, traceable architecture decisions, validated artifacts, security evidence, release evidence, operational readiness evidence, retention controls, and a feedback path for future evolution.

This model exists to ensure that:

* autonomous development is governed across the full lifecycle
* lifecycle phases have explicit entry and exit criteria
* phase transitions are evidence-based
* human oversight is assigned where required
* platform automation is bounded by governance and risk tier
* generated artifacts remain traceable to specification intent
* validation and security evidence are preserved
* release readiness is separated from deployment authorization
* operations and monitoring can trigger controlled re-entry
* lifecycle activity can be audited after completion

---

## 3.0 Relationship to the Autonomous Development Loop

This section distinguishes the Autonomous SDLC from the Autonomous Development Loop. This distinction is necessary because the loop defines the internal execution cycle, while the SDLC defines lifecycle governance, evidence, roles, and enterprise control.

The Autonomous Development Loop is the mechanism by which the platform performs construction. The Autonomous SDLC is the framework that determines when the loop may run, what lifecycle phase the loop supports, what evidence is required, what approvals are necessary, how outputs are accepted, and how operational feedback becomes future specification work.

### 3.1 Distinction Between v3.6 and v3.7

| Concern            | ISL v3.6 Autonomous Development Loop                                     | ISL v3.7 Autonomous SDLC                                                   |
| ------------------ | ------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| Primary question   | How does the platform iterate from specification to validated artifacts? | How does the enterprise govern autonomous software development end to end? |
| Unit of control    | Loop run                                                                 | Lifecycle phase and phase gate                                             |
| Main state object  | loop-run-id                                                              | lifecycle-run-id                                                           |
| Core activities    | interpret, plan, generate, validate, repair, converge                    | authorize, govern, evidence, review, release, operate, evolve              |
| Primary controls   | loop states, repair limits, convergence criteria                         | phase gates, role accountability, evidence packages, release criteria      |
| Completion concept | loop success or failure                                                  | lifecycle phase transition or lifecycle closure                            |
| Human involvement  | escalation and governed intervention                                     | phase ownership, approval, review, audit, release accountability           |

### 3.2 SDLC Rules

An Autonomous SDLC phase MAY contain one or more Autonomous Development Loop runs.

An Autonomous Development Loop run MUST be associated with an SDLC phase when executed inside a governed lifecycle.

A loop run that produces release-impacting artifacts MUST produce evidence usable by the SDLC phase gate.

A lifecycle phase MUST NOT transition solely because a loop run succeeded. The phase transition MUST also satisfy lifecycle evidence, governance, security, traceability, and release criteria.

---

## 4.0 Autonomous SDLC Design Principles

### 4.1 Lifecycle Governance Is Mandatory

Every enterprise use of autonomous construction MUST operate within a declared lifecycle model. The platform MUST know whether it is supporting discovery, specification, design, construction, verification, release preparation, deployment authorization, operations, maintenance, or retirement.

### 4.2 Specifications Are Lifecycle Assets

Specifications are not temporary prompts. Specifications MUST be treated as lifecycle assets with ownership, versioning, readiness state, governance history, traceability links, and change control.

### 4.3 Phase Gates Are Evidence-Based

A lifecycle phase MUST NOT close unless required evidence exists. Evidence MAY include specifications, canonical models, readiness reports, construction plans, execution graphs, artifact metadata, validation results, security results, governance decisions, traceability snapshots, release records, operational telemetry, and audit records.

### 4.4 Autonomy Is Risk-Tiered

The degree of permitted autonomy MUST depend on risk tier, governance profile, organizational policy, environment, and system criticality. Standard systems MAY allow more automated progression. High and Critical systems MUST require stronger evidence, review, approval, and retention controls.

### 4.5 Human Accountability Remains Explicit

Autonomous construction MAY automate development activity, but it MUST NOT erase accountability. Lifecycle owners, reviewers, authorizing officials, governance administrators, security reviewers, and operations owners MUST remain explicit where required by policy.

### 4.6 Operations Feed Back Into Specification

Operational telemetry, incidents, defects, security findings, performance findings, and governance findings MUST be capable of creating controlled change triggers that re-enter the lifecycle through specification or maintenance phases.

### 4.7 Release Readiness Is Not Deployment Authorization

A system may be release-ready without being authorized for deployment. Release readiness confirms that artifacts, evidence, packaging, and validation are complete. Deployment authorization is a governance decision allowing deployment to a target environment.

---

## 5.0 Autonomous SDLC Lifecycle Phases

This section defines the standard lifecycle phases. A conforming implementation MAY add phases, but it MUST preserve these standard phase meanings or define a documented equivalent.

| Phase | Name                        | Purpose                                                                  |
| ----: | --------------------------- | ------------------------------------------------------------------------ |
|     0 | Lifecycle Intake            | Register initiative, scope, ownership, risk tier, and lifecycle profile  |
|     1 | Specification Development   | Author and structure the ISL specification                               |
|     2 | Specification Readiness     | Validate specification and approve readiness progression                 |
|     3 | Architecture and Planning   | Normalize canonical model and generate construction plan                 |
|     4 | Autonomous Construction     | Generate, admit, validate, repair, and stabilize artifacts               |
|     5 | Verification and Validation | Confirm functional, structural, security, policy, and quality evidence   |
|     6 | Release Preparation         | Package stable artifacts and prepare release evidence                    |
|     7 | Deployment Authorization    | Approve or reject deployment to target environment                       |
|     8 | Operations and Monitoring   | Observe deployed system and collect operational feedback                 |
|     9 | Maintenance and Evolution   | Process changes, defects, enhancements, and re-entry                     |
|    10 | Retirement and Archival     | Retire systems, preserve records, archive artifacts, and close lifecycle |

### 5.1 Phase Rules

Each lifecycle phase MUST have an owner or responsible role.

Each lifecycle phase MUST have entry criteria and exit criteria.

Each lifecycle phase MUST produce or reference evidence.

A phase MAY be skipped only when the lifecycle profile explicitly permits skipping and records the rationale.

High and Critical risk systems MUST NOT skip Specification Readiness, Verification and Validation, Deployment Authorization, or Retirement and Archival.

---

## 6.0 Lifecycle Profile

A Lifecycle Profile defines which SDLC phases, gates, evidence, roles, and automation limits apply to a system.

### 6.1 Lifecycle Profile Schema

| Field                   | Type     | Required    | Description                                                        |
| ----------------------- | -------- | ----------- | ------------------------------------------------------------------ |
| lifecycle-profile-id    | string   | REQUIRED    | Unique lifecycle profile identifier                                |
| profile-name            | string   | REQUIRED    | Human-readable profile name                                        |
| profile-version         | semver   | REQUIRED    | Profile version                                                    |
| risk-tier               | enum     | REQUIRED    | Standard, Elevated, High, Critical                                 |
| applicable-environments | array    | REQUIRED    | local, dev, test, staging, prod, regulated-prod                    |
| required-phases         | array    | REQUIRED    | Lifecycle phases required                                          |
| optional-phases         | array    | CONDITIONAL | Lifecycle phases optional                                          |
| prohibited-skips        | array    | CONDITIONAL | Phases that may not be skipped                                     |
| required-gates          | array    | REQUIRED    | Phase gates required                                               |
| required-roles          | array    | REQUIRED    | Roles required by lifecycle                                        |
| evidence-requirements   | array    | REQUIRED    | Evidence requirements by phase                                     |
| automation-level        | enum     | REQUIRED    | assisted, supervised-autonomous, autonomous-with-gates, restricted |
| human-review-required   | boolean  | REQUIRED    | Whether human review is mandatory                                  |
| retention-profile-id    | string   | REQUIRED    | Retention profile                                                  |
| governance-profile-id   | string   | REQUIRED    | Governance profile                                                 |
| status                  | enum     | REQUIRED    | draft, active, superseded, revoked                                 |
| approved-at             | ISO 8601 | CONDITIONAL | Approval time                                                      |
| approved-by             | string   | CONDITIONAL | Approving authority                                                |

### 6.2 Lifecycle Profile Rules

Every autonomous SDLC run MUST reference a lifecycle-profile-id.

A revoked lifecycle profile MUST NOT be used for new lifecycle runs.

A lifecycle profile for High or Critical systems MUST require human review and deployment authorization.

Lifecycle profile changes affecting active systems MUST trigger impact analysis.

---

## 7.0 Autonomous SDLC Run Record

Each governed lifecycle execution MUST produce an Autonomous SDLC Run Record.

| Field                 | Type     | Required    | Description                                                         |
| --------------------- | -------- | ----------- | ------------------------------------------------------------------- |
| lifecycle-run-id      | string   | REQUIRED    | Unique lifecycle run identifier                                     |
| lifecycle-profile-id  | string   | REQUIRED    | Lifecycle profile used                                              |
| specification-id      | string   | REQUIRED    | Specification identifier                                            |
| specification-version | semver   | REQUIRED    | Specification version                                               |
| system-id             | string   | REQUIRED    | System under lifecycle control                                      |
| lifecycle-run-type    | enum     | REQUIRED    | initial, change, maintenance, release, deployment, retirement       |
| current-phase         | enum     | REQUIRED    | Current lifecycle phase                                             |
| current-phase-state   | enum     | REQUIRED    | not-started, active, blocked, completed, skipped, escalated, failed |
| risk-tier             | enum     | REQUIRED    | Risk tier                                                           |
| owner-role            | string   | REQUIRED    | Primary lifecycle owner role                                        |
| active-loop-run-ids   | array    | CONDITIONAL | Development loop runs associated                                    |
| active-gate-ids       | array    | CONDITIONAL | Open phase gates                                                    |
| evidence-package-id   | string   | CONDITIONAL | Current lifecycle evidence package                                  |
| started-at            | ISO 8601 | REQUIRED    | Lifecycle run start                                                 |
| completed-at          | ISO 8601 | CONDITIONAL | Lifecycle run completion                                            |
| final-outcome         | enum     | CONDITIONAL | succeeded, failed, cancelled, retired, superseded                   |

---

## 8.0 Phase Gate Model

Phase gates control movement between lifecycle phases.

### 8.1 Phase Gate Schema

| Field                  | Type     | Required    | Description                                                                                              |
| ---------------------- | -------- | ----------- | -------------------------------------------------------------------------------------------------------- |
| phase-gate-id          | string   | REQUIRED    | Unique phase gate identifier                                                                             |
| lifecycle-run-id       | string   | REQUIRED    | Lifecycle run                                                                                            |
| from-phase             | enum     | REQUIRED    | Phase being exited                                                                                       |
| to-phase               | enum     | REQUIRED    | Phase being entered                                                                                      |
| gate-type              | enum     | REQUIRED    | readiness, architecture, construction, validation, security, release, deployment, operations, retirement |
| required-evidence      | array    | REQUIRED    | Evidence required                                                                                        |
| required-approver-role | string   | CONDITIONAL | Approver role when approval required                                                                     |
| gate-decision          | enum     | REQUIRED    | pending, passed, failed, waived, escalated, blocked                                                      |
| decision-record-id     | string   | CONDITIONAL | Governance decision record                                                                               |
| findings               | array    | CONDITIONAL | Gate findings                                                                                            |
| opened-at              | ISO 8601 | REQUIRED    | Gate opened                                                                                              |
| closed-at              | ISO 8601 | CONDITIONAL | Gate closed                                                                                              |

### 8.2 Phase Gate Rules

A lifecycle phase MUST NOT close until its required gate passes, is waived, or is explicitly skipped according to lifecycle profile.

A failed phase gate MUST prevent transition to the next phase.

A waived phase gate MUST reference a valid governance waiver.

A deployment phase gate MUST require Authorizing Official approval for production and regulated production environments.

A phase gate decision MUST be auditable.

---

## 9.0 Lifecycle Roles and Responsibilities

The Autonomous SDLC distinguishes platform roles from human accountability roles.

### 9.1 Human Lifecycle Roles

| Role                     | Responsibility                                                   |
| ------------------------ | ---------------------------------------------------------------- |
| Lifecycle Owner          | Owns lifecycle execution and final business accountability       |
| Specification Owner      | Owns specification correctness and version control               |
| Domain Expert            | Validates domain meaning and business intent                     |
| Architecture Reviewer    | Reviews architecture and planning evidence                       |
| Security Reviewer        | Reviews security, privacy, and policy evidence                   |
| Quality Reviewer         | Reviews validation and quality evidence                          |
| Operations Owner         | Owns operational readiness and monitoring acceptance             |
| Authorizing Official     | Approves Autonomous-Ready readiness and deployment authorization |
| Governance Administrator | Manages lifecycle policies and governance configuration          |
| Auditor                  | Reviews lifecycle evidence and audit records                     |

### 9.2 Platform Lifecycle Roles

| Platform Role           | Responsibility                              |
| ----------------------- | ------------------------------------------- |
| Specification Engine    | Parses and validates authored specification |
| Semantic Model Engine   | Produces canonical model                    |
| Planning Engine         | Produces construction plan                  |
| Runtime Orchestrator    | Executes construction tasks                 |
| Agent Orchestrator      | Coordinates reasoning agents                |
| Model Integration Layer | Routes model invocations                    |
| Tool Integration Layer  | Performs deterministic validation           |
| Artifact Repository     | Stores and controls artifacts               |
| Traceability Engine     | Maintains lifecycle traceability            |
| Governance Engine       | Enforces policies and gates                 |
| Observability Layer     | Emits telemetry and operational signals     |
| State Manager           | Maintains lifecycle and execution state     |

### 9.3 Role Rules

A lifecycle run MUST identify a Lifecycle Owner.

A specification MUST identify a Specification Owner.

High and Critical risk systems MUST require Security Reviewer and Authorizing Official roles.

Separation-of-duties rules defined in governance policy MUST be enforced.

Platform automation MUST NOT impersonate a human approval role.

---

## 10.0 Phase 0 — Lifecycle Intake

Lifecycle Intake registers the initiative and establishes the control context for autonomous development.

This phase ensures that the system is not created as an uncontrolled generation exercise. It establishes ownership, risk tier, lifecycle profile, environment, governance profile, and initial scope.

### 10.1 Entry Criteria

Lifecycle Intake may begin when:

* a new system, change, maintenance request, release request, or retirement request exists
* a responsible owner is identified
* the system or initiative has a preliminary scope
* governance profile selection is possible

### 10.2 Required Activities

The platform or lifecycle owner MUST:

* create lifecycle-run-id
* identify system-id
* assign Lifecycle Owner
* assign Specification Owner
* select lifecycle profile
* assign initial risk tier
* identify target environments
* identify governance profile
* create initial lifecycle evidence package

### 10.3 Exit Criteria

Lifecycle Intake is complete only when:

* lifecycle run record exists
* lifecycle profile is active
* owner roles are assigned
* risk tier is assigned
* governance profile is attached
* initial phase gate passes or is not required by profile

### 10.4 Intake Evidence

Required evidence includes:

* lifecycle run record
* lifecycle profile reference
* ownership record
* risk-tier assignment record
* governance profile reference

---

## 11.0 Phase 1 — Specification Development

Specification Development produces or updates the formal ISL specification.

This phase maps to the traditional requirements phase, but in IMHOTEP the output is not an informal requirements document. The output is a structured, versioned specification that can be parsed, normalized, validated, governed, and traced.

### 11.1 Entry Criteria

Specification Development may begin when:

* Lifecycle Intake is complete
* Specification Owner is assigned
* specification repository or source location is available
* required domain input is available or explicitly deferred

### 11.2 Required Activities

The responsible parties and platform MUST:

* author or update specification
* assign specification-id and specification-version
* define required canonical entities
* define requirements, capabilities, services, data, interfaces, policies, validations, and infrastructure where applicable
* classify non-functional requirements where applicable
* record assumptions and constraints
* run structural validation where supported
* preserve specification version history

### 11.3 Exit Criteria

Specification Development is complete only when:

* specification source exists
* required metadata exists
* required entity identifiers exist
* specification version is recorded
* known gaps are recorded
* Specification Owner submits specification for readiness evaluation

### 11.4 Specification Development Evidence

Required evidence includes:

* authored specification
* specification metadata record
* version record
* structural validation report when available
* open issue list or known limitation record

---

## 12.0 Phase 2 — Specification Readiness

Specification Readiness determines whether the specification may advance through readiness levels and eventually authorize autonomous construction.

This phase maps to requirements review, design readiness, and pre-construction approval in traditional processes. It uses the readiness levels defined in ISL v1.3 and governance gates defined in ISL v1.7.

### 12.1 Entry Criteria

Specification Readiness may begin when:

* Specification Development exit criteria are satisfied
* specification version is fixed for review
* canonical normalization can be attempted
* required reviewers are assigned according to risk tier

### 12.2 Required Activities

The platform MUST:

* parse specification
* normalize specification into canonical model
* validate canonical entities and relationships
* evaluate readiness exit criteria
* identify readiness findings
* open required readiness gates
* record approvals, rejections, waivers, or regressions

### 12.3 Exit Criteria

Specification Readiness is complete only when:

* readiness state is recorded
* canonical validation evidence exists
* readiness transition record exists
* required approval gates are closed
* Autonomous-Ready state exists if autonomous construction is requested

### 12.4 Readiness Evidence

Required evidence includes:

* parsed specification record
* canonical model record
* canonical validation report
* readiness evaluation report
* readiness transition record
* governance approval records
* waiver records where applicable

### 12.5 Readiness Rules

Autonomous construction MUST NOT begin unless the specification is Autonomous-Ready.

A specification change after Autonomous-Ready MUST trigger readiness regression evaluation.

A failed readiness gate MUST block construction admission.

---

## 13.0 Phase 3 — Architecture and Planning

Architecture and Planning transforms the canonical model into a governed construction plan.

This phase maps to system design and implementation planning in traditional SDLCs. In IMHOTEP, the Planning Engine, Architecture Planner, Construction Planner, and Traceability Engine produce a machine-executable construction task graph and planning evidence.

### 13.1 Entry Criteria

Architecture and Planning may begin when:

* specification is at Machine-Valid or higher for planning
* canonical model exists
* required planning agents or deterministic planners are available
* governance policy permits planning activity

### 13.2 Required Activities

The platform MUST:

* analyze canonical entities and relationships
* identify architectural components
* generate construction task graph
* identify task dependencies
* identify required agents and tools
* define expected artifacts
* define validation tasks
* define security and policy validation tasks where required
* seed traceability links
* validate construction plan

### 13.3 Exit Criteria

Architecture and Planning is complete only when:

* construction plan exists
* task graph is valid
* required dependencies are resolved
* required tool capabilities are available or exceptions are recorded
* required agent roles are available or exceptions are recorded
* planning validation passes
* planning gate passes where required

### 13.4 Planning Evidence

Required evidence includes:

* canonical model reference
* architecture decision records where applicable
* construction plan
* task graph
* dependency records
* expected artifact list
* validation plan
* planning validation report
* planning traceability records

---

## 14.0 Phase 4 — Autonomous Construction

Autonomous Construction executes the construction plan and produces controlled artifacts.

This phase maps to implementation in traditional SDLCs. In IMHOTEP, implementation is performed through runtime orchestration, reasoning agents, model integration, artifact repository admission, deterministic tool support, and bounded repair.

### 14.1 Entry Criteria

Autonomous Construction may begin only when:

* specification is Autonomous-Ready
* construction plan is executable
* execution admission passes
* runtime configuration is active
* required artifact repository is available
* required governance checks pass
* required agents, models, and tools are available

### 14.2 Required Activities

The platform MUST:

* initialize execution graph
* schedule and execute construction tasks
* invoke agents through Agent Orchestrator
* route model calls through Model Integration Layer
* generate artifact candidates
* admit artifacts into repository working state
* invoke deterministic tools
* record validation results
* perform bounded repair cycles
* update task, artifact, validation, repair, and execution state
* maintain traceability links
* emit telemetry

### 14.3 Exit Criteria

Autonomous Construction is complete only when:

* required construction tasks are completed, skipped, waived, or escalated according to policy
* required artifacts exist in repository state
* generated artifacts have metadata
* validation failures are repaired, waived, escalated, or terminally failed
* execution graph is complete for construction scope
* construction evidence exists

### 14.4 Construction Evidence

Required evidence includes:

* execution graph
* task execution records
* agent invocation records
* model invocation records where applicable
* artifact admission records
* artifact metadata records
* tool invocation records
* validation records
* repair records where applicable
* construction telemetry summary

---

## 15.0 Phase 5 — Verification and Validation

Verification and Validation determines whether the constructed system satisfies specification, quality, security, policy, and repository integrity requirements.

This phase is distinct from validation tasks executed during construction. Construction validation may occur continuously; lifecycle Verification and Validation is the phase gate that determines whether the system may move toward release preparation.

### 15.1 Entry Criteria

Verification and Validation may begin when:

* construction scope is complete or sufficiently complete for verification
* required artifacts are available
* validation profile is active
* required deterministic tools are available
* required evidence from construction exists

### 15.2 Required Activities

The platform MUST:

* run required build, test, static analysis, security, policy, repository, and traceability checks according to validation profile
* normalize validation outcomes
* classify findings
* determine blocking and non-blocking findings
* route failed findings to repair, waiver, escalation, or failure
* produce verification summary
* produce validation evidence package

### 15.3 Exit Criteria

Verification and Validation is complete only when:

* required validation checks passed or have valid waivers
* no blocking validation finding remains unresolved
* traceability integrity passes
* repository integrity passes
* security findings are resolved, waived, or escalated according to governance
* verification gate passes

### 15.4 Verification Evidence

Required evidence includes:

* validation profile
* tool invocation results
* test results
* build results
* security validation results where applicable
* policy validation results where applicable
* repository integrity report
* traceability integrity report
* validation summary report
* waivers or escalations where applicable

### 15.5 Verification Rules

A system MUST NOT proceed to Release Preparation with unresolved blocking validation findings.

A failed security validation MUST block release preparation unless a valid governance waiver exists.

A missing traceability integrity report MUST block release preparation.

---

## 16.0 Phase 6 — Release Preparation

Release Preparation packages stable artifacts and creates release evidence.

This phase maps to release engineering in traditional SDLCs. It does not authorize deployment; it prepares release candidates that may later be submitted for deployment authorization.

### 16.1 Entry Criteria

Release Preparation may begin when:

* Verification and Validation exit criteria are satisfied
* required stable artifacts exist
* package profile exists
* release target is identified
* governance policy permits release preparation

### 16.2 Required Activities

The platform MUST:

* select stable artifacts for release candidate
* generate package manifest
* package artifacts
* record exact artifact versions
* run package validation
* create release evidence package
* identify waivers included in release
* create release readiness record

### 16.3 Exit Criteria

Release Preparation is complete only when:

* release candidate package exists
* package manifest references exact artifact versions
* package validation passes or valid waiver exists
* release evidence package exists
* release readiness gate passes

### 16.4 Release Evidence

Required evidence includes:

* stable artifact list
* artifact version list
* package manifest
* package validation results
* release notes or generated release summary where required
* traceability snapshot
* waiver list where applicable
* release readiness record

### 16.5 Release Rules

A release candidate MUST NOT include failed artifacts.

A release candidate MUST NOT include untraced artifacts.

A release candidate that includes waived findings MUST disclose those waivers in the release evidence package.

---

## 17.0 Phase 7 — Deployment Authorization

Deployment Authorization determines whether a release candidate may be deployed to a target environment.

This phase is a governance phase. It is intentionally separate from Release Preparation because a package may be technically ready but not authorized for a specific environment.

### 17.1 Entry Criteria

Deployment Authorization may begin when:

* release candidate exists
* release evidence package exists
* target environment is identified
* deployment preparation evidence exists where required
* required approvers are assigned

### 17.2 Required Activities

The platform and governance roles MUST:

* evaluate release evidence
* evaluate target environment profile
* evaluate risk tier
* evaluate security and policy findings
* evaluate operational readiness
* identify required approvals
* record deployment authorization decision

### 17.3 Exit Criteria

Deployment Authorization is complete only when:

* deployment authorization decision is recorded
* required approvals are recorded
* unresolved blocking findings are absent or waived
* authorization scope is defined
* target environment is defined
* authorization expiration is defined where policy requires it

### 17.4 Deployment Authorization Evidence

Required evidence includes:

* deployment authorization record
* release evidence package reference
* target environment profile
* operational readiness report
* security findings summary
* approval records
* waiver or override records where applicable

### 17.5 Deployment Authorization Rules

Production deployment MUST require Authorizing Official approval.

Regulated production deployment MUST require the lifecycle profile’s required security, quality, operations, and governance approvals.

Deployment authorization MUST be scoped to release candidate, environment, and time period.

Expired deployment authorization MUST NOT permit deployment.

---

## 18.0 Phase 8 — Operations and Monitoring

Operations and Monitoring observes deployed systems and collects feedback.

This phase maps to operations, monitoring, incident management, and production support in traditional SDLCs. IMHOTEP uses observability, governance, and traceability to connect operational behavior back to specification and lifecycle state.

### 18.1 Entry Criteria

Operations and Monitoring may begin when:

* deployment has occurred or operational monitoring is otherwise required
* operational telemetry is available
* operations owner is assigned
* monitoring profile exists

### 18.2 Required Activities

The platform SHOULD:

* collect operational telemetry
* correlate telemetry with release, package, artifact, and specification version
* monitor service health and quality indicators
* record incidents, defects, or operational findings
* detect policy or security events
* create change triggers when required
* preserve operational evidence

### 18.3 Exit Criteria

Operations and Monitoring does not necessarily close while the system remains active. It may transition to Maintenance and Evolution when operational feedback requires change.

A monitoring cycle may close when:

* monitoring period ends
* operational findings are reviewed
* required change triggers are created
* operational evidence is stored

### 18.4 Operations Evidence

Required evidence may include:

* operational telemetry summary
* incident records
* defect records
* security event records
* performance findings
* availability findings
* user feedback records
* change trigger records

### 18.5 Operations Rules

Operational findings that affect specification intent MUST create or link to a change trigger.

Security incidents MUST follow governance and security escalation rules.

Operational telemetry MUST NOT replace audit records.

---

## 19.0 Phase 9 — Maintenance and Evolution

Maintenance and Evolution processes change requests, defects, enhancements, reauthorization needs, and operational feedback.

This phase is the SDLC frame for the change-driven re-entry behavior defined in ISL v3.6. It ensures that change is controlled, assessed, governed, and traced.

### 19.1 Entry Criteria

Maintenance and Evolution may begin when:

* change trigger exists
* defect record exists
* enhancement request exists
* operational feedback requires action
* policy or environment change affects the system
* specification owner accepts change for analysis

### 19.2 Required Activities

The platform and responsible roles MUST:

* classify change type
* perform impact analysis
* identify affected specifications, canonical entities, tasks, artifacts, packages, and deployments
* update specification or create change proposal
* determine whether readiness regression is required
* determine whether reauthorization is required
* initiate change-driven loop run when required

### 19.3 Exit Criteria

Maintenance and Evolution is complete only when:

* change is rejected, deferred, or accepted
* accepted change has traceable lifecycle records
* affected artifacts are repaired, regenerated, revalidated, repackaged, or retired
* required reauthorization is completed
* lifecycle evidence is updated

### 19.4 Maintenance Evidence

Required evidence includes:

* change trigger record
* impact analysis report
* affected artifact list
* specification change record
* re-entry loop record where applicable
* revalidation evidence
* reauthorization record where applicable

---

## 20.0 Phase 10 — Retirement and Archival

Retirement and Archival closes the lifecycle of a system, release, package, or artifact set.

This phase ensures that obsolete systems are not simply abandoned. Retirement must preserve auditability, traceability, retention obligations, and operational closure.

### 20.1 Entry Criteria

Retirement and Archival may begin when:

* system, release, or artifact set is approved for retirement
* replacement or decommissioning plan exists where required
* retention obligations are known
* operations owner and governance roles are identified

### 20.2 Required Activities

The platform or responsible roles MUST:

* mark affected system, release, package, and artifacts as retired, deprecated, archived, or superseded
* preserve required artifact metadata
* preserve traceability snapshots
* preserve governance and audit records
* archive validation and release evidence
* close operational monitoring obligations where applicable
* record retirement decision

### 20.3 Exit Criteria

Retirement and Archival is complete only when:

* retirement decision is recorded
* artifact and package states are updated
* required evidence is archived
* retention policy is attached
* traceability remains queryable
* governance closure record exists

### 20.4 Retirement Evidence

Required evidence includes:

* retirement decision record
* retired artifact and package list
* archival records
* final traceability snapshot
* retention policy record
* governance closure record

---

## 21.0 Lifecycle Evidence Package

The lifecycle evidence package aggregates phase-level evidence.

### 21.1 Lifecycle Evidence Package Schema

| Field                         | Type     | Required    | Description                        |
| ----------------------------- | -------- | ----------- | ---------------------------------- |
| lifecycle-evidence-package-id | string   | REQUIRED    | Unique evidence package identifier |
| lifecycle-run-id              | string   | REQUIRED    | Lifecycle run                      |
| specification-id              | string   | REQUIRED    | Specification identifier           |
| specification-version         | semver   | REQUIRED    | Specification version              |
| lifecycle-profile-id          | string   | REQUIRED    | Lifecycle profile                  |
| included-phases               | array    | REQUIRED    | Phases represented                 |
| specification-evidence        | array    | CONDITIONAL | Specification evidence             |
| readiness-evidence            | array    | CONDITIONAL | Readiness evidence                 |
| planning-evidence             | array    | CONDITIONAL | Planning evidence                  |
| construction-evidence         | array    | CONDITIONAL | Construction evidence              |
| verification-evidence         | array    | CONDITIONAL | Verification evidence              |
| release-evidence              | array    | CONDITIONAL | Release evidence                   |
| deployment-evidence           | array    | CONDITIONAL | Deployment authorization evidence  |
| operations-evidence           | array    | CONDITIONAL | Operations evidence                |
| maintenance-evidence          | array    | CONDITIONAL | Maintenance evidence               |
| retirement-evidence           | array    | CONDITIONAL | Retirement evidence                |
| traceability-snapshot-ids     | array    | REQUIRED    | Traceability snapshots included    |
| governance-record-ids         | array    | CONDITIONAL | Governance records included        |
| created-at                    | ISO 8601 | REQUIRED    | Package creation time              |
| retention-class               | enum     | REQUIRED    | operational, audit, archival       |

### 21.2 Evidence Package Rules

A lifecycle phase gate MUST reference the evidence package or phase-specific evidence records.

A successful lifecycle run MUST have a lifecycle evidence package.

High and Critical risk lifecycle evidence packages MUST be retained as audit or archival records.

Evidence packages MUST remain queryable by specification-id, lifecycle-run-id, release candidate, and deployment authorization record.

---

## 22.0 SDLC Quality Model

The Autonomous SDLC MUST support quality evaluation across lifecycle phases.

### 22.1 Quality Categories

The platform SHOULD classify quality evidence using categories aligned with ISO/IEC 25010 concepts, including:

| Quality Category       | Example Evidence                                               |
| ---------------------- | -------------------------------------------------------------- |
| functional suitability | acceptance tests, behavior validation                          |
| performance efficiency | performance tests, resource telemetry                          |
| compatibility          | integration tests, environment compatibility reports           |
| usability              | review findings, documentation review                          |
| reliability            | resilience tests, recovery tests, failure analysis             |
| security               | vulnerability scans, policy checks, threat findings            |
| maintainability        | static analysis, modularity metrics, traceability completeness |
| portability            | package validation, deployment environment compatibility       |

### 22.2 Quality Evidence Rules

A lifecycle profile SHOULD define required quality categories.

High and Critical systems MUST define required security, reliability, and maintainability evidence.

Quality findings MUST be classified as blocking, high, medium, low, or informational.

Blocking quality findings MUST prevent release readiness unless waived.

---

## 23.0 SDLC Metrics

Lifecycle metrics allow organizations to assess autonomous development performance and control effectiveness.

### 23.1 Recommended Metrics

The platform SHOULD collect:

* lifecycle phase duration
* phase gate pass/fail rate
* readiness regression count
* construction loop count per lifecycle run
* validation failure rate
* repair iteration count
* repair convergence rate
* waiver count by phase
* governance approval wait time
* release candidate rejection rate
* deployment authorization rejection rate
* operational defect rate by release
* change-driven re-entry frequency
* traceability completeness rate
* evidence package completeness rate

### 23.2 Metrics Rules

SDLC metrics MUST preserve correlation to lifecycle-run-id.

Metrics MUST NOT expose restricted artifacts, prompts, secrets, or personal data without authorization.

Metrics used for governance decisions MUST be traceable to source records.

---

## 24.0 SDLC Traceability Requirements

The Autonomous SDLC MUST maintain traceability across lifecycle phases.

### 24.1 Required Lifecycle Traceability Links

The platform MUST record links between:

* lifecycle run and specification version
* specification version and canonical model
* canonical model and construction plan
* construction plan and execution graph
* execution graph and generated artifacts
* artifacts and validation results
* validation failures and repair records
* stable artifacts and release candidate
* release candidate and deployment authorization
* deployed release and operational telemetry summary
* operational finding and change trigger
* change trigger and maintenance lifecycle run
* retired artifact and archival record

### 24.2 Traceability Rules

Lifecycle phase gates MUST NOT pass if required lifecycle traceability is missing.

Release preparation MUST include traceability from release candidate to specification version.

Maintenance and Evolution MUST use traceability for impact analysis.

Retirement MUST preserve traceability for audit and historical reconstruction.

---

## 25.0 SDLC Governance Requirements

Governance applies across the full lifecycle.

### 25.1 Required Governance Controls

The Autonomous SDLC MUST support:

* lifecycle profile approval
* risk-tier assignment
* readiness approval
* architecture or planning review where required
* security review where required
* waiver and override management
* release readiness review where required
* deployment authorization
* post-change reauthorization
* retirement approval where required

### 25.2 Governance Rules

Governance decisions MUST be recorded as audit records.

Governance audit records MUST be immutable or correction-only.

A blocked governance decision MUST prevent affected lifecycle progression.

A waiver MUST be scoped to phase, finding, artifact, release, or deployment as applicable.

A waiver MUST have expiration or review conditions when required by policy.

---

## 26.0 SDLC State Model

The SDLC state model tracks lifecycle phase progression.

### 26.1 Lifecycle Phase State Values

| State       | Meaning                                            |
| ----------- | -------------------------------------------------- |
| not-started | Phase has not begun                                |
| active      | Phase is in progress                               |
| blocked     | Phase cannot proceed due to unresolved condition   |
| completed   | Phase exit criteria satisfied                      |
| skipped     | Phase intentionally skipped under profile rules    |
| escalated   | Phase requires governance or human resolution      |
| failed      | Phase failed and cannot proceed without new action |

### 26.2 SDLC State Rules

Each phase MUST maintain phase state.

Phase state transitions MUST emit state events.

A lifecycle run MUST NOT enter a later phase until prior required phase is completed, skipped, or waived according to profile.

A failed phase MUST prevent lifecycle completion.

---

## 27.0 SDLC Error Classes

This section defines lifecycle-specific error classes.

| Error Class                           | Description                                       | Default Handling               |
| ------------------------------------- | ------------------------------------------------- | ------------------------------ |
| sdlc-lifecycle-profile-missing        | No lifecycle profile assigned                     | block intake                   |
| sdlc-lifecycle-profile-invalid        | Lifecycle profile invalid or revoked              | block lifecycle run            |
| sdlc-owner-missing                    | Required lifecycle owner missing                  | block phase                    |
| sdlc-risk-tier-missing                | Risk tier not assigned                            | block intake                   |
| sdlc-phase-entry-failed               | Phase entry criteria not satisfied                | block phase                    |
| sdlc-phase-exit-failed                | Phase exit criteria not satisfied                 | block transition               |
| sdlc-phase-gate-failed                | Phase gate failed                                 | block transition               |
| sdlc-evidence-missing                 | Required evidence missing                         | block gate                     |
| sdlc-readiness-not-authorized         | Required readiness state absent                   | block construction             |
| sdlc-planning-not-approved            | Planning gate failed or missing                   | block construction             |
| sdlc-validation-blocking-finding      | Blocking validation finding unresolved            | block release                  |
| sdlc-security-blocking-finding        | Blocking security finding unresolved              | block release or deployment    |
| sdlc-release-readiness-failed         | Release readiness criteria not satisfied          | block deployment authorization |
| sdlc-deployment-authorization-missing | Deployment authorization absent                   | block deployment               |
| sdlc-operational-feedback-untracked   | Operational feedback not linked to change trigger | warn or escalate               |
| sdlc-impact-analysis-missing          | Required impact analysis missing                  | block maintenance progression  |
| sdlc-retention-policy-missing         | Retention policy missing for retirement           | block retirement               |
| sdlc-traceability-incomplete          | Required lifecycle traceability missing           | block gate                     |
| sdlc-governance-audit-write-failed    | Governance audit record could not be written      | halt governed action           |

### 27.1 SDLC Error Record Schema

| Field                       | Type     | Required    | Description                       |
| --------------------------- | -------- | ----------- | --------------------------------- |
| sdlc-error-id               | string   | REQUIRED    | Unique lifecycle error identifier |
| error-class                 | enum     | REQUIRED    | Error class                       |
| severity                    | enum     | REQUIRED    | blocking, high, medium, low       |
| lifecycle-run-id            | string   | REQUIRED    | Lifecycle run affected            |
| phase                       | enum     | CONDITIONAL | Phase affected                    |
| phase-gate-id               | string   | CONDITIONAL | Gate affected                     |
| specification-id            | string   | CONDITIONAL | Specification affected            |
| release-candidate-id        | string   | CONDITIONAL | Release affected                  |
| deployment-authorization-id | string   | CONDITIONAL | Deployment authorization affected |
| message                     | string   | REQUIRED    | Human-readable explanation        |
| required-action             | string   | REQUIRED    | Required remediation              |
| detected-at                 | ISO 8601 | REQUIRED    | Detection time                    |
| detected-by                 | string   | REQUIRED    | Detecting component or role       |

---

## 28.0 SDLC Audit Requirements

The Autonomous SDLC MUST preserve audit evidence for lifecycle decisions.

### 28.1 Required Audit Records

The platform MUST preserve audit records for:

* lifecycle profile approval
* lifecycle owner assignment
* risk-tier assignment
* readiness transition
* phase gate decisions
* approvals
* waivers
* overrides
* security review decisions
* release readiness decisions
* deployment authorization decisions
* post-change reauthorization
* retirement decisions

### 28.2 Audit Rules

Audit records MUST include actor, role, timestamp, decision, scope, and rationale.

Automated platform decisions MUST identify the platform component making the decision.

Human approvals MUST identify authenticated human or authorized role identity.

Audit records MUST be retained according to lifecycle profile and governance policy.

---

## 29.0 SDLC Testing Requirements

The lifecycle model itself MUST be testable.

### 29.1 Required Test Categories

| Test Category            | Purpose                                                    |
| ------------------------ | ---------------------------------------------------------- |
| lifecycle-profile        | Validate lifecycle profile schema and rules                |
| phase-entry              | Validate phase entry criteria                              |
| phase-exit               | Validate phase exit criteria                               |
| phase-gate               | Validate gate pass, fail, waiver, escalation behavior      |
| role-assignment          | Validate required role assignment                          |
| evidence-package         | Validate evidence completeness                             |
| readiness-control        | Validate construction cannot begin before Autonomous-Ready |
| release-control          | Validate release cannot proceed with failed evidence       |
| deployment-authorization | Validate deployment cannot proceed without authorization   |
| operations-feedback      | Validate telemetry and incidents create change triggers    |
| maintenance-reentry      | Validate change triggers start governed re-entry           |
| retirement               | Validate archival and retention requirements               |
| governance-audit         | Validate audit records are created and immutable           |

### 29.2 Testing Rules

Lifecycle tests MUST verify that missing evidence blocks phase gates.

Lifecycle tests MUST verify that governance blocks cannot be bypassed.

Lifecycle tests MUST verify that release readiness does not imply deployment authorization.

Lifecycle tests MUST verify that operational feedback can create maintenance re-entry.

---

## 30.0 SDLC Conformance Requirements

### 30.1 Lifecycle Profile Conformance

A lifecycle profile conforms to ISL v3.7 if it:

* declares required phases
* declares required gates
* declares required roles
* declares risk tier
* declares evidence requirements
* declares automation level
* declares retention profile
* declares governance profile
* is versioned and approved

### 30.2 Lifecycle Run Conformance

A lifecycle run conforms to ISL v3.7 if it:

* references a valid lifecycle profile
* identifies system, specification, version, owner, and risk tier
* tracks phase state
* enforces phase gates
* links loop runs to lifecycle phases
* records evidence packages
* preserves lifecycle traceability
* records governance decisions
* produces lifecycle completion or transition records

### 30.3 Phase Gate Conformance

A phase gate conforms to ISL v3.7 if it:

* identifies from-phase and to-phase
* declares required evidence
* declares required approver role where applicable
* records pass, fail, waiver, block, or escalation decision
* references governance records where applicable
* blocks transition when failed or pending

### 30.4 Release Readiness Conformance

Release readiness conforms to ISL v3.7 if it:

* references stable artifact versions
* references validation evidence
* references traceability snapshot
* references package manifest
* records unresolved findings and waivers
* confirms release candidate completeness
* does not imply deployment authorization

### 30.5 Deployment Authorization Conformance

Deployment authorization conforms to ISL v3.7 if it:

* references release candidate
* references target environment
* references operational readiness evidence
* records required approvals
* records validity scope and expiration where applicable
* is issued by required authority
* blocks deployment when absent, expired, or rejected

### 30.6 Platform Conformance

A platform conforms to ISL v3.7 if it:

* supports lifecycle profiles
* supports lifecycle run records
* supports lifecycle phases and gates
* associates autonomous development loop runs with lifecycle phases
* enforces readiness, validation, release, deployment, operations, maintenance, and retirement controls
* preserves lifecycle evidence packages
* preserves lifecycle traceability
* preserves lifecycle audit records
* supports lifecycle metrics
* prevents phase transitions without required evidence
* prevents deployment without deployment authorization

---

## 31.0 Summary

ISL v3.7 defines the Autonomous Software Development Lifecycle Model for the IMHOTEP platform. It places autonomous construction inside an enterprise SDLC framework with phases, gates, roles, evidence, governance, release readiness, deployment authorization, operations feedback, maintenance re-entry, retirement, metrics, audit, and conformance.

This revision makes v3.7 materially different from v3.6. ISL v3.6 defines the operational loop that constructs, validates, repairs, packages, and prepares artifacts. ISL v3.7 defines the lifecycle control system that governs when those loops occur, how their outputs are accepted, how human accountability is preserved, how release and deployment are controlled, how operations feeds future change, and how the full lifecycle remains auditable.

A conforming Autonomous SDLC MUST not treat autonomous construction as a shortcut around software engineering governance. It MUST make the lifecycle faster and more traceable while preserving the controls that make enterprise software safe, reviewable, releasable, operable, maintainable, and accountable.
