# IMHOTEP Specification Language (ISL) v1.2

# The Execution Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1
**Supersedes:** ISL r1 v1.2 ASC Execution Model
**Document Type:** Execution Model Specification

---

## 1.0 Scope

This document defines the execution model used by the IMHOTEP platform to transform an Autonomous-Ready ISL specification into validated construction outputs. It establishes the required execution phases, phase entry and exit criteria, execution records, artifact handling rules, validation behavior, repair cycle termination, governance interactions, and completion requirements.

The execution model applies after an authored ISL specification has been normalized into the canonical semantic model and has reached the readiness state required for autonomous construction. This document does not define the authored language syntax, canonical entity schemas, readiness criteria, construction planning model, tool integration interface, or governance role model. Those responsibilities are defined in other ISL documents. This document defines how execution proceeds once those preconditions are satisfied.

A conforming execution runtime MUST use this model to ensure that autonomous construction is:

* gated by readiness and governance
* driven by a validated canonical specification
* executed through ordered phases
* validated by deterministic tools
* traceable to specification entities
* bounded by repair termination rules
* observable through structured execution records
* halted or escalated when required conditions fail

---

## 2.0 Purpose of the Execution Model

The execution model defines how the IMHOTEP platform operationalizes a validated specification. It is the bridge between a machine-valid semantic blueprint and a constructed software system. Where ISL v1.1 defines what the system means, ISL v1.2 defines how that meaning enters a controlled execution lifecycle.

Autonomous construction cannot be treated as unconstrained generation. It must proceed through a repeatable sequence of interpretation, planning, generation, validation, repair, testing, policy review, consolidation, and deployment preparation. Each step must produce structured records so that later systems can inspect what happened, why it happened, and whether the output is trustworthy.

The execution model exists to ensure that:

* autonomous construction begins only from authorized specifications
* construction proceeds in a defined order
* generated artifacts are not trusted until validated
* failed artifacts enter bounded repair cycles
* testing and policy checks occur before consolidation
* deployment preparation does not occur prematurely
* all execution decisions remain traceable and auditable

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the execution model. Execution depends on prior language, semantic, readiness, traceability, planning, tool, and governance models. No conforming execution runtime may interpret this document independently of those dependencies.

The references in this section establish where specialized obligations are defined. For example, this document states that deterministic validation must occur, while ISL v1.6 defines the tool invocation contract used to perform that validation.

| Reference | Purpose                                                                       |
| --------- | ----------------------------------------------------------------------------- |
| ISL v0.0  | Corpus-level conformance, system principles, and governance of the ISL corpus |
| ISL v1.0  | Authored specification language and required specification structure          |
| ISL v1.1  | Canonical semantic model and normalization rules                              |
| ISL v1.3  | Specification readiness levels and authorization for execution                |
| ISL v1.4  | Traceability graph and artifact association rules                             |
| ISL v1.5  | Construction planning model and task graph definition                         |
| ISL v1.6  | Tool integration model and deterministic validation outcomes                  |
| ISL v1.7  | Governance roles, policies, approval gates, waivers, and audit events         |
| RFC 2119  | Normative requirement terminology                                             |

---

## 4.0 Terms and Definitions

This section defines execution-specific terms used throughout this document. These terms describe runtime behavior rather than authored language or canonical model structure.

**Execution**
The controlled process by which the platform transforms an Autonomous-Ready specification and construction plan into validated artifacts.

**Execution Runtime**
The platform component responsible for scheduling tasks, invoking agents and tools, recording outcomes, and enforcing execution state transitions.

**Execution Phase**
A defined stage of the construction lifecycle with specific entry criteria, activities, outputs, and exit criteria.

**Execution Graph**
The runtime representation of phases, construction tasks, dependencies, validation activities, repair tasks, and execution state.

**Artifact**
A tangible output produced during execution, including source code, configuration, schemas, contracts, tests, infrastructure definitions, documentation, or deployment materials.

**Validation Result**
A structured record of deterministic evaluation performed against an artifact or task.

**Repair Cycle**
A bounded execution loop initiated after validation failure to produce a corrected artifact.

**Escalation**
A controlled suspension or governance notification triggered when execution cannot proceed safely or automatically.

**Convergence**
The condition in which generated artifacts satisfy applicable validation, testing, policy, and completion criteria.

---

## 5.0 Execution Design Principles

This section defines the principles that govern all execution behavior. These principles ensure that autonomous construction remains controlled, repeatable, auditable, and safe.

An execution runtime that violates these principles MUST NOT claim conformance to ISL v1.2.

### 5.1 Readiness-Gated Execution

Execution MUST NOT begin until the specification has reached the readiness level required by ISL v1.3. The runtime MUST treat readiness as a hard gate, not as advisory metadata.

This protects the platform from constructing systems from incomplete, ambiguous, unapproved, or semantically invalid specifications.

### 5.2 Canonical Model Authority

Execution MUST operate from the canonical semantic model defined by ISL v1.1. The runtime MUST NOT use unnormalized authored prose as the authoritative execution source.

Authored documents may be retained for human review, but execution decisions MUST be derived from canonical entities, relationships, readiness state, and construction plan records.

### 5.3 Deterministic Validation Supremacy

Generated artifacts MUST NOT be accepted solely because they were produced by an agent, model, or generator. Artifacts MUST pass deterministic validation before they can be marked valid or used as stable construction outputs.

Reasoning agents may propose, generate, review, or repair artifacts. Deterministic validation determines whether artifacts satisfy objective execution criteria.

### 5.4 Traceable Execution

Every execution activity MUST be traceable to specification entities, construction tasks, artifacts, agents, tools, validation results, or governance events.

The runtime MUST preserve enough structured execution history to reconstruct how each artifact was produced, validated, repaired, accepted, or rejected.

### 5.5 Bounded Repair

Repair cycles MUST be bounded by explicit termination rules. The runtime MUST NOT allow unbounded repair loops.

Bounded repair protects reliability, cost, governance accountability, and operational predictability.

### 5.6 Governed Autonomy

Autonomous execution MUST operate within governance constraints. Governance policies, approval gates, waivers, risk tiers, and escalation rules MUST be enforced during execution.

The runtime MUST NOT treat autonomy as permission to bypass organizational control.

---

## 6.0 Preconditions for Execution

This section defines the conditions that MUST be satisfied before execution begins. These preconditions protect the construction process from invalid input, missing authorization, incomplete planning, and unresolved semantic ambiguity.

Execution is not permitted merely because a specification exists. The platform MUST verify that the specification, governance state, and construction plan are ready for controlled execution.

### 6.1 Required Preconditions

The platform MUST NOT begin execution unless all of the following preconditions are satisfied.

| Precondition                                               | Required Evidence            | Enforced By    |
| ---------------------------------------------------------- | ---------------------------- | -------------- |
| Specification has reached Autonomous-Ready readiness level | Readiness transition record  | ISL v1.3       |
| Canonical normalization completed without blocking errors  | Canonical validation report  | ISL v1.1       |
| All required governance approvals have been recorded       | Approval records             | ISL v1.7       |
| Construction plan has been generated and validated         | Construction Task Graph      | ISL v1.5       |
| No unresolved interpretation ambiguities remain            | Interpretation Record        | ISL v1.2 §10.0 |
| Required deterministic tools are registered                | Tool Records                 | ISL v1.6       |
| Traceability graph is initialized                          | Traceability state           | ISL v1.4       |
| Execution runtime configuration is valid                   | Runtime configuration record | ISL v1.2       |

### 6.2 Precondition Failure

If any precondition fails, execution MUST NOT begin.

The runtime MUST produce a Precondition Failure Record containing:

| Field                 | Type     | Required    | Description                  |
| --------------------- | -------- | ----------- | ---------------------------- |
| failure-id            | string   | REQUIRED    | Unique failure identifier    |
| specification-id      | string   | REQUIRED    | Project or system identifier |
| specification-version | semver   | REQUIRED    | Version evaluated            |
| failed-precondition   | string   | REQUIRED    | Precondition that failed     |
| evidence              | string   | CONDITIONAL | Evidence or missing evidence |
| severity              | enum     | REQUIRED    | blocking, high, medium, low  |
| recorded-at           | ISO 8601 | REQUIRED    | Time failure was recorded    |
| required-action       | string   | REQUIRED    | Action required before retry |

Precondition failures MUST be recorded in governance state when they affect readiness, approval, policy, or authorization.

---

## 7.0 Execution Inputs

This section defines the required inputs to the execution runtime. The runtime must receive structured inputs, not informal instructions. Structured inputs make execution repeatable and auditable.

The runtime MUST reject execution requests that lack required inputs or contain inconsistent versions across inputs.

### 7.1 Required Input Set

| Input                         | Required | Description                                                        |
| ----------------------------- | -------- | ------------------------------------------------------------------ |
| Canonical Model               | YES      | Normalized ISL v1.1 specification graph                            |
| Readiness Record              | YES      | Evidence that specification is Autonomous-Ready                    |
| Construction Task Graph       | YES      | Plan generated under ISL v1.5                                      |
| Governance State              | YES      | Approvals, policies, waivers, risk tier                            |
| Tool Registry                 | YES      | Registered deterministic tools and capabilities                    |
| Traceability State            | YES      | Initial traceability graph and identifier registry                 |
| Runtime Configuration         | YES      | Execution limits, concurrency, timeout, repair limits              |
| Artifact Repository Reference | YES      | Target repository for generated artifacts                          |
| Execution Request             | YES      | Request to initiate execution for a specific specification version |

### 7.2 Input Consistency Rules

All execution inputs MUST reference the same specification identifier and specification version.

The Construction Task Graph MUST be derived from the same canonical model version submitted for execution.

The governance approvals MUST apply to the same specification version submitted for execution.

The runtime MUST reject execution if version inconsistency is detected.

---

## 8.0 Execution Outputs

This section defines the outputs that execution must produce. Execution is not complete merely because artifacts exist. It is complete only when the runtime has produced validated artifacts, records, telemetry, traceability links, and completion evidence.

Outputs MUST be structured so that governance, audit, debugging, and future reconstruction can inspect the execution lifecycle.

### 8.1 Required Outputs

| Output                         | Description                                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| Execution Record               | Summary of the full execution lifecycle                                                        |
| Execution Graph                | Runtime graph of phases, tasks, dependencies, and outcomes                                     |
| Generated Artifacts            | Source, config, schema, contract, test, infrastructure, documentation, or deployment artifacts |
| Artifact Metadata              | Artifact identifiers, types, statuses, sources, timestamps                                     |
| Validation Results             | Deterministic tool outcomes and findings                                                       |
| Repair Records                 | Records of repair cycles and outcomes                                                          |
| Test Results                   | Generated and executed test results                                                            |
| Policy Evaluation Records      | Security and governance validation outcomes                                                    |
| Traceability Updates           | Links between specification entities, tasks, artifacts, and validations                        |
| Deployment Preparation Records | Deployment artifacts and readiness evidence                                                    |
| Completion Report              | Final summary of success, failure, or escalation                                               |

### 8.2 Output Integrity Rules

Every generated artifact MUST be recorded in the Artifact Repository.

Every generated artifact MUST have traceability to at least one source entity or a justified inferred derivation.

Every validation result MUST reference the artifact, task, tool, and validation criteria evaluated.

Every execution failure MUST produce a structured error or escalation record.

---

## 9.0 Execution Phase Model

This section defines the ordered execution phases. The phase model ensures that construction proceeds through a controlled lifecycle rather than ad hoc generation.

Each phase has entry criteria, required activities, required outputs, and exit criteria. A phase MUST NOT be considered complete until its exit criteria are satisfied.

### 9.1 Phase Overview

Execution proceeds through the following ordered phases.

| Phase | Title                              | Depends On                      |
| ----- | ---------------------------------- | ------------------------------- |
| 1     | Specification Interpretation       | Preconditions satisfied         |
| 2     | Construction Planning Confirmation | Phase 1                         |
| 3     | Artifact Generation                | Phase 2                         |
| 4     | Deterministic Validation           | Phase 3                         |
| 5     | Repair Cycles                      | Phase 4, conditional on failure |
| 6     | Test Generation and Execution      | Phase 4 or Phase 5              |
| 7     | Security and Policy Validation     | Phase 6                         |
| 8     | Artifact Consolidation             | Phase 7                         |
| 9     | Deployment Preparation             | Phase 8                         |

### 9.2 Phase Ordering Rules

Each phase MUST complete successfully before the next phase begins unless this document explicitly permits conditional or parallel behavior.

Phase 5 occurs only when validation failure requires repair.

Execution of unaffected parallel tasks MAY continue while a specific task is in repair or escalation, provided dependency and governance rules permit continuation.

Phase 8 MUST NOT begin until all required validation, repair, test, and policy obligations have completed successfully or have been resolved through governance-approved waiver.

Phase 9 MUST NOT begin until artifacts are consolidated into a stable repository state.

---

## 10.0 Phase 1 — Specification Interpretation

Specification Interpretation converts the canonical model into an execution-oriented understanding of the system. Although the specification has already been normalized, execution still requires an explicit interpretation record that identifies the entities, assumptions, risks, and ambiguities relevant to construction.

This phase prevents the runtime from proceeding when semantic content is technically valid but operationally unclear. It is a final interpretation gate before construction planning is confirmed.

### 10.1 Entry Criteria

Phase 1 MUST NOT begin unless:

* all execution preconditions are satisfied
* canonical validation has passed
* governance approvals required for execution are present
* runtime configuration has been validated

### 10.2 Required Activities

The runtime or Specification Interpreter agent MUST:

* inspect the canonical model
* identify entities relevant to execution
* identify unresolved ambiguity
* identify assumptions affecting planning or generation
* identify policy constraints relevant to execution
* produce an Interpretation Record

### 10.3 Interpretation Record Schema

| Field                   | Type     | Required    | Description                                                 |
| ----------------------- | -------- | ----------- | ----------------------------------------------------------- |
| interpretation-id       | string   | REQUIRED    | Unique interpretation identifier                            |
| specification-id        | string   | REQUIRED    | System identifier                                           |
| specification-version   | semver   | REQUIRED    | Version interpreted                                         |
| canonical-model-version | semver   | REQUIRED    | Canonical model version                                     |
| interpreted-entities    | array    | REQUIRED    | Entity identifiers successfully interpreted                 |
| execution-assumptions   | array    | CONDITIONAL | Assumptions relevant to execution                           |
| ambiguities             | array    | CONDITIONAL | Entities requiring clarification                            |
| policy-constraints      | array    | CONDITIONAL | Policy identifiers affecting execution                      |
| generated-at            | ISO 8601 | REQUIRED    | Time interpretation completed                               |
| generated-by            | string   | REQUIRED    | Agent, runtime component, or processor producing the record |

### 10.4 Exit Criteria

Phase 1 completes successfully only when:

* an Interpretation Record has been produced
* all required canonical entities have been interpreted
* no unresolved blocking ambiguity remains
* interpretation output has been recorded in traceability state

Execution MUST NOT proceed to Phase 2 if unresolved blocking ambiguities remain.

---

## 11.0 Phase 2 — Construction Planning Confirmation

Construction Planning Confirmation verifies that the Construction Task Graph generated under ISL v1.5 is still valid for the interpreted specification. This phase does not replace the planning model. Instead, it confirms that the plan is executable in the current runtime context.

This phase is necessary because a plan may be structurally valid but not currently executable due to missing tools, governance restrictions, unavailable runtime capacity, or version mismatch.

### 11.1 Entry Criteria

Phase 2 MUST NOT begin unless Phase 1 completed successfully.

### 11.2 Required Activities

The runtime MUST verify:

* the Construction Task Graph references the current specification version
* all task dependencies are resolvable
* all required agent roles are available
* all required tool capabilities are registered
* all declared artifact types are supported
* all governance constraints affecting task execution are known
* parallel groups do not violate dependency or governance constraints

### 11.3 Planning Confirmation Record Schema

| Field                  | Type     | Required    | Description                     |
| ---------------------- | -------- | ----------- | ------------------------------- |
| confirmation-id        | string   | REQUIRED    | Unique confirmation identifier  |
| plan-id                | string   | REQUIRED    | Construction plan identifier    |
| specification-id       | string   | REQUIRED    | System identifier               |
| specification-version  | semver   | REQUIRED    | Specification version           |
| executable             | boolean  | REQUIRED    | Whether the plan is executable  |
| blocked-tasks          | array    | CONDITIONAL | Tasks that cannot execute       |
| required-tools         | array    | REQUIRED    | Tool capabilities required      |
| required-agent-roles   | array    | REQUIRED    | Agent roles required            |
| governance-constraints | array    | CONDITIONAL | Governance rules affecting plan |
| confirmed-at           | ISO 8601 | REQUIRED    | Time confirmation completed     |

### 11.4 Exit Criteria

Phase 2 completes successfully only when:

* the plan is confirmed executable
* no blocking task, tool, role, or governance dependency is unresolved
* the Planning Confirmation Record has been stored

If the plan is not executable, execution MUST halt and produce a Planning Execution Failure Record.

---

## 12.0 Phase 3 — Artifact Generation

Artifact Generation creates implementation artifacts from construction tasks. This phase is where reasoning agents, generators, templates, and other construction mechanisms produce tangible outputs.

Generated artifacts are not trusted at creation time. They enter the repository with a provisional status and must later pass deterministic validation before becoming stable outputs.

### 12.1 Entry Criteria

Phase 3 MUST NOT begin unless Phase 2 completed successfully.

A task MUST NOT enter artifact generation unless all of its dependencies are satisfied.

### 12.2 Artifact Types

The following artifact types are defined.

| Artifact Type  | Description                             |
| -------------- | --------------------------------------- |
| source         | Source code artifact                    |
| config         | Configuration artifact                  |
| schema         | Data or validation schema artifact      |
| infrastructure | Infrastructure or deployment definition |
| contract       | Interface or integration contract       |
| test           | Test artifact                           |
| documentation  | Generated documentation artifact        |

### 12.3 Required Activities

For each eligible construction task, the runtime MUST:

* assign the task to the appropriate agent, generator, or tool
* provide structured task context
* generate declared artifacts
* assign each artifact an artifact identifier
* associate each artifact with source specification entities
* record artifact metadata
* write artifacts to the Artifact Repository
* update task and traceability state

### 12.4 Artifact Metadata Schema

Each artifact MUST be recorded with the following metadata.

| Field               | Type     | Required    | Description                                                           |
| ------------------- | -------- | ----------- | --------------------------------------------------------------------- |
| artifact-id         | string   | REQUIRED    | Unique artifact identifier                                            |
| artifact-type       | enum     | REQUIRED    | source, config, schema, infrastructure, contract, test, documentation |
| source-entity-ids   | array    | REQUIRED    | Specification entities that originated the artifact                   |
| task-id             | string   | REQUIRED    | Construction task that produced the artifact                          |
| derivation-type     | enum     | REQUIRED    | direct, derived, inferred, repair                                     |
| generated-at        | ISO 8601 | REQUIRED    | Timestamp of creation                                                 |
| generated-by        | string   | REQUIRED    | Agent, generator, or tool that created artifact                       |
| status              | enum     | REQUIRED    | pending, valid, failed, repaired, escalated                           |
| repository-location | string   | REQUIRED    | Location in artifact repository                                       |
| content-hash        | string   | CONDITIONAL | Integrity hash when supported                                         |

### 12.5 Artifact Generation Rules

An artifact MUST NOT be created without a task-id.

An artifact MUST NOT be created without at least one source-entity-id unless it is marked `inferred` and includes a justification.

An artifact status MUST initially be `pending`.

An artifact MUST NOT be marked `valid` during generation.

If a task completes without producing its declared artifacts, the runtime MUST mark the task as failed.

### 12.6 Exit Criteria

Phase 3 completes successfully when all eligible artifact-generation tasks have either:

* produced their declared artifacts and moved to validation, or
* failed and produced structured failure records, or
* entered escalation according to governance rules

Execution MUST NOT proceed to final validation outcomes for an artifact that was not recorded in the Artifact Repository.

---

## 13.0 Phase 4 — Deterministic Validation

Deterministic Validation evaluates generated artifacts using registered deterministic tools. This phase is the foundation of execution reliability because it prevents reasoning outputs from being accepted without objective verification.

Validation MUST be performed through the Tool Integration Model defined in ISL v1.6. The execution runtime MUST treat tool results as authoritative for objective checks such as compilation, schema validation, static analysis, tests, and security scans where applicable.

### 13.1 Entry Criteria

Phase 4 begins for an artifact only after the artifact has been generated and recorded with status `pending` or `repaired`.

### 13.2 Required Activities

The runtime MUST:

* identify validation tasks associated with generated artifacts
* select registered tools with required capabilities
* invoke tools using the tool invocation contract
* record invocation results
* update artifact status based on validation outcomes
* initiate repair when required
* record validation results in traceability state

### 13.3 Validation Result Schema

| Field                  | Type     | Required    | Description                                     |
| ---------------------- | -------- | ----------- | ----------------------------------------------- |
| validation-result-id   | string   | REQUIRED    | Unique validation result identifier             |
| artifact-id            | string   | REQUIRED    | Artifact evaluated                              |
| task-id                | string   | REQUIRED    | Task associated with validation                 |
| tool-id                | string   | REQUIRED    | Tool used for validation                        |
| capability             | string   | REQUIRED    | Tool capability invoked                         |
| outcome                | enum     | REQUIRED    | passed, failed, warning, timeout, error         |
| findings               | array    | CONDITIONAL | Required for failed, warning, timeout, or error |
| evaluated-at           | ISO 8601 | REQUIRED    | Time validation completed                       |
| duration-ms            | integer  | REQUIRED    | Execution duration                              |
| tool-version-confirmed | string   | REQUIRED    | Tool version used                               |
| next-action            | enum     | REQUIRED    | proceed, repair, escalate, halt                 |

### 13.4 Validation Outcome Handling

| Outcome | Runtime Action                                                                     |
| ------- | ---------------------------------------------------------------------------------- |
| passed  | Mark artifact valid and proceed                                                    |
| failed  | Mark artifact failed and initiate repair cycle                                     |
| warning | Record finding and proceed unless governance requires escalation                   |
| timeout | Retry once; if timeout recurs, treat as failed                                     |
| error   | Record tool error; treat as failed unless governance permits retry or substitution |

### 13.5 Artifact Status Rules

An artifact MAY be marked `valid` only after required deterministic validation passes.

An artifact MUST be marked `failed` when validation produces a failed outcome.

An artifact MAY remain `pending` only while awaiting validation.

An artifact MUST be marked `escalated` when repair or validation cannot continue without governance or human intervention.

### 13.6 Exit Criteria

Phase 4 completes for an artifact when validation produces a terminal outcome of:

* passed
* warning accepted
* failed and transferred to repair
* escalated
* halted

Phase 4 completes for the construction plan only when all required validation activities have reached terminal outcomes.

---

## 14.0 Phase 5 — Repair Cycles

Repair Cycles are initiated when deterministic validation identifies failures in generated artifacts. This phase provides the controlled feedback loop required for autonomous construction to converge toward valid outputs.

Repair is not open-ended. It MUST operate under explicit iteration limits, escalation thresholds, and termination rules. Without such limits, autonomous repair becomes operationally unsafe and economically unpredictable.

### 14.1 Entry Criteria

Phase 5 begins when a validation result requires repair.

A repair cycle MUST NOT begin unless:

* a failed validation result exists
* the failed artifact is identified
* the failed task is identified
* repair limits have not been exceeded
* governance policy permits automated repair for the artifact or task type

### 14.2 Repair Termination Policy

The following default repair termination policy applies unless a stricter runtime configuration is approved by governance.

| Parameter                                             | Value                                       | Configurable |
| ----------------------------------------------------- | ------------------------------------------- | ------------ |
| Maximum repair iterations per artifact                | 5                                           | YES          |
| Maximum total repair iterations per construction plan | 50                                          | YES          |
| Escalation threshold                                  | 3 consecutive failures on the same artifact | NO           |
| Maximum repair duration per artifact                  | implementation-defined                      | YES          |
| Maximum repair duration per construction plan         | implementation-defined                      | YES          |

### 14.3 Repair Record Schema

The Repair Analyst agent or runtime MUST produce a Repair Record for each repair cycle.

| Field                       | Type     | Required    | Description                                |
| --------------------------- | -------- | ----------- | ------------------------------------------ |
| repair-id                   | string   | REQUIRED    | Unique repair cycle identifier             |
| artifact-id                 | string   | REQUIRED    | Artifact being repaired                    |
| task-id                     | string   | REQUIRED    | Task associated with repair                |
| failed-validation-result-id | string   | REQUIRED    | Validation result that triggered repair    |
| iteration                   | integer  | REQUIRED    | Repair attempt number for artifact         |
| failure-summary             | string   | REQUIRED    | Summary of validation failure              |
| root-cause-analysis         | string   | CONDITIONAL | Required when available                    |
| proposed-correction         | string   | REQUIRED    | Correction applied or proposed             |
| modified-artifacts          | array    | CONDITIONAL | Artifacts changed during repair            |
| outcome                     | enum     | REQUIRED    | resolved, unresolved, escalated, abandoned |
| recorded-at                 | ISO 8601 | REQUIRED    | Time repair record was created             |

### 14.4 Repair Execution Rules

The runtime MUST increment the repair iteration count for each repair attempt.

A repaired artifact MUST be returned to deterministic validation.

A repair cycle MUST NOT mark an artifact valid without validation.

A repair cycle MUST preserve prior artifact versions or content hashes where supported.

A repaired artifact MUST retain traceability to the original source entity and failed validation result.

### 14.5 Escalation Rules

The runtime MUST escalate when:

* three consecutive repair attempts fail on the same artifact
* the maximum repair iterations per artifact is reached
* the maximum total repair iterations per construction plan is reached
* a governance policy blocks automated repair
* the Repair Analyst cannot determine a proposed correction
* repair introduces a new blocking policy violation

When escalation occurs, the runtime MUST:

* suspend the affected task
* mark the affected artifact as escalated
* record an escalation event in governance state
* continue unaffected tasks only when dependency rules permit
* require governance or human resolution before retry

### 14.6 Convergence Failure

If repair limits are reached without successful validation, the construction plan MUST be marked failed unless governance explicitly authorizes continuation under a documented waiver.

A construction plan failure MUST produce a Completion Report with failure status.

### 14.7 Exit Criteria

Phase 5 exits for an artifact when:

* repair resolves the failure and validation passes
* repair reaches escalation threshold
* repair reaches iteration limits
* repair is blocked by governance
* construction plan is halted

---

## 15.0 Phase 6 — Test Generation and Execution

Test Generation and Execution verifies that generated artifacts satisfy functional requirements, workflows, policies, and acceptance criteria. This phase extends validation beyond artifact correctness into behavior verification.

Tests MUST be derived from Requirement and Validation entities. A conforming runtime MUST ensure that must-have requirements are covered by executable or reviewable validation before construction can proceed.

### 15.1 Entry Criteria

Phase 6 MUST NOT begin until generated artifacts required for test derivation have passed deterministic validation or have accepted warnings that do not block testing.

### 15.2 Required Activities

The runtime MUST:

* derive tests from Requirement and Validation entities
* generate test artifacts where required
* link each test to the entities it validates
* execute tests using registered testing tools where automation applies
* record individual test results
* initiate repair when test failures indicate artifact defects
* escalate when test failures require human decision or governance review

### 15.3 Test Types

| Test Type   | Description                                  |
| ----------- | -------------------------------------------- |
| unit        | Validates isolated implementation behavior   |
| integration | Validates interaction between components     |
| acceptance  | Validates requirement-level behavior         |
| contract    | Validates interface contract compliance      |
| security    | Validates security control behavior          |
| operational | Validates runtime or deployment expectations |

### 15.4 Test Result Schema

| Field          | Type     | Required    | Description                                                    |
| -------------- | -------- | ----------- | -------------------------------------------------------------- |
| test-result-id | string   | REQUIRED    | Unique test result identifier                                  |
| test-id        | string   | REQUIRED    | Test artifact or case identifier                               |
| validation-id  | string   | REQUIRED    | Validation entity associated with test                         |
| requirement-id | string   | CONDITIONAL | Requirement validated by test                                  |
| artifact-ids   | array    | REQUIRED    | Artifacts under test                                           |
| test-type      | enum     | REQUIRED    | unit, integration, acceptance, contract, security, operational |
| outcome        | enum     | REQUIRED    | passed, failed, skipped, blocked                               |
| executed-at    | ISO 8601 | REQUIRED    | Time test executed                                             |
| failure-detail | string   | CONDITIONAL | Required when outcome is failed or blocked                     |
| evidence       | string   | CONDITIONAL | Test report or output artifact reference                       |

### 15.5 Test Coverage Rules

At least one test or validation method MUST exist for each must-have functional requirement.

A test associated with a must-have requirement MUST NOT be skipped without governance-approved justification.

A failed test linked to a must-have requirement MUST block progression to Phase 7 unless waived by governance.

Aggregate test results without individual case detail MUST NOT satisfy test execution requirements.

### 15.6 Exit Criteria

Phase 6 completes successfully when:

* all required tests have executed or have governance-approved justification
* all must-have requirement tests have passed or have approved waivers
* all test results have been recorded
* any failed tests have triggered repair, escalation, or halt

---

## 16.0 Phase 7 — Security and Policy Validation

Security and Policy Validation evaluates generated artifacts, execution outputs, and deployment preparation candidates against Policy entities and governance rules. This phase ensures that constructed systems do not merely function, but also comply with declared security, compliance, operational, technology, and risk constraints.

This phase integrates the canonical Policy entities, governance model, deterministic tools, and runtime enforcement behavior.

### 16.1 Entry Criteria

Phase 7 MUST NOT begin until Phase 6 has completed successfully or all blocking test failures have been resolved, repaired, or waived.

### 16.2 Required Activities

The Security Validator agent, deterministic tools, or governance engine MUST:

* identify all applicable Policy entities
* evaluate each policy against relevant artifacts and execution records
* perform required security scans or policy checks
* record policy evaluation outcomes
* initiate repair for correctable non-compliance
* escalate governance-controlled violations
* apply waiver rules where permitted

### 16.3 Minimum Security Validation Coverage

Security and policy validation MUST include, at minimum, evaluation of:

* authentication mechanisms
* authorization rules
* dependency vulnerability exposure
* secret detection when source or config artifacts exist
* access control constraints
* data protection policies
* interface exposure policies
* deployment policy constraints

### 16.4 Policy Evaluation Record Schema

| Field               | Type     | Required    | Description                                                         |
| ------------------- | -------- | ----------- | ------------------------------------------------------------------- |
| evaluation-id       | string   | REQUIRED    | Unique policy evaluation identifier                                 |
| policy-id           | string   | REQUIRED    | Policy evaluated                                                    |
| evaluated-target-id | string   | REQUIRED    | Artifact, service, interface, data entity, or infrastructure target |
| enforcement-point   | string   | REQUIRED    | Phase or point of enforcement                                       |
| outcome             | enum     | REQUIRED    | compliant, non-compliant, not-applicable, waived                    |
| evaluated-at        | ISO 8601 | REQUIRED    | Time evaluated                                                      |
| evaluated-by        | string   | REQUIRED    | Tool, agent, or governance authority                                |
| findings            | array    | CONDITIONAL | Required for non-compliant outcome                                  |
| waiver-id           | string   | CONDITIONAL | Required when outcome is waived                                     |
| next-action         | enum     | REQUIRED    | proceed, repair, escalate, block                                    |

### 16.5 Outcome Handling

| Outcome        | Runtime Action                                         |
| -------------- | ------------------------------------------------------ |
| compliant      | Proceed                                                |
| non-compliant  | Apply violation response defined by policy             |
| not-applicable | Record justification and proceed                       |
| waived         | Proceed only if waiver is valid under governance rules |

### 16.6 Blocking Rules

A non-compliant outcome for a policy with violation-response `block` MUST prevent progression until the violation is resolved or a valid waiver is granted.

Critical or high security findings MUST be treated as failed outcomes unless the governing policy explicitly defines a different response and governance approves it.

### 16.7 Exit Criteria

Phase 7 completes successfully only when:

* all applicable policies have been evaluated
* all blocking violations are resolved or waived
* all policy evaluation records are stored
* governance state has been updated

---

## 17.0 Phase 8 — Artifact Consolidation

Artifact Consolidation organizes validated artifacts into a stable project structure. This phase prepares the generated system for maintainability, packaging, review, and deployment preparation.

Consolidation is not simply copying files. It is the process of ensuring that validated artifacts form a coherent repository state with manifests, documentation, dependency declarations, traceability records, and build-ready organization.

### 17.1 Entry Criteria

Phase 8 MUST NOT begin until Phase 7 completes successfully.

### 17.2 Required Activities

The runtime MUST consolidate artifacts into an organized repository structure that includes:

* source artifacts
* configuration artifacts
* schema artifacts
* interface contract artifacts
* test artifacts
* infrastructure artifacts when applicable
* documentation artifacts
* dependency manifest
* build configuration
* traceability metadata
* generated execution summary

### 17.3 Consolidated Project Record Schema

| Field                  | Type     | Required    | Description                            |
| ---------------------- | -------- | ----------- | -------------------------------------- |
| consolidation-id       | string   | REQUIRED    | Unique consolidation identifier        |
| specification-id       | string   | REQUIRED    | System identifier                      |
| specification-version  | semver   | REQUIRED    | Specification version                  |
| repository-location    | string   | REQUIRED    | Consolidated project location          |
| artifact-ids           | array    | REQUIRED    | Artifacts included                     |
| build-manifest         | string   | REQUIRED    | Build or dependency manifest reference |
| documentation-location | string   | CONDITIONAL | Generated documentation reference      |
| traceability-snapshot  | string   | REQUIRED    | Traceability snapshot reference        |
| consolidated-at        | ISO 8601 | REQUIRED    | Time consolidation completed           |
| status                 | enum     | REQUIRED    | completed, failed, escalated           |

### 17.4 Consolidation Rules

Only valid, waived, or governance-approved artifacts MAY be included in the stable consolidated project.

Failed, pending, or escalated artifacts MUST NOT be included as stable outputs unless clearly isolated and marked non-deployable.

The consolidated project MUST be tagged with the specification version that produced it.

The consolidated project MUST preserve traceability metadata.

### 17.5 Exit Criteria

Phase 8 completes successfully when:

* consolidated project structure exists
* all included artifacts are eligible for consolidation
* build and dependency manifests exist
* traceability snapshot exists
* consolidation record is stored

---

## 18.0 Phase 9 — Deployment Preparation

Deployment Preparation produces or validates the artifacts needed to prepare the constructed system for controlled deployment. This phase does not necessarily deploy the system. It ensures the system is ready for deployment authorization under governance control.

Deployment preparation closes the execution lifecycle by translating consolidated implementation outputs into operationally usable packages, manifests, environment definitions, and readiness evidence.

### 18.1 Entry Criteria

Phase 9 MUST NOT begin until Phase 8 completes successfully.

### 18.2 Required Activities

The Deployment Preparer agent, runtime, or deterministic tools MUST produce or validate:

* deployment manifests
* infrastructure definitions
* runtime configuration
* environment variables or configuration templates
* health check definitions
* package or container build outputs when applicable
* operational documentation
* deployment validation results
* deployment authorization evidence

### 18.3 Deployment Preparation Record Schema

| Field                     | Type     | Required | Description                              |
| ------------------------- | -------- | -------- | ---------------------------------------- |
| deployment-preparation-id | string   | REQUIRED | Unique deployment preparation identifier |
| specification-id          | string   | REQUIRED | System identifier                        |
| specification-version     | semver   | REQUIRED | Specification version                    |
| consolidated-project-id   | string   | REQUIRED | Consolidated project reference           |
| deployment-artifacts      | array    | REQUIRED | Deployment artifacts produced            |
| target-environments       | array    | REQUIRED | Environments prepared for                |
| validation-results        | array    | REQUIRED | Deployment validation results            |
| health-checks             | array    | REQUIRED | Health check definitions                 |
| prepared-at               | ISO 8601 | REQUIRED | Time preparation completed               |
| status                    | enum     | REQUIRED | ready, failed, escalated                 |

### 18.4 Deployment Preparation Rules

The platform MUST NOT mark a construction plan complete until deployment preparation artifacts have been generated and validated.

Deployment artifacts MUST trace to Infrastructure entities or operational requirements.

Deployment preparation MUST respect governance rules for deployment authorization.

A status of `ready` does not imply that production deployment is approved. Deployment authorization is governed by ISL v1.7.

### 18.5 Exit Criteria

Phase 9 completes successfully when:

* deployment artifacts exist
* deployment artifacts pass required validation
* health checks are defined
* operational documentation exists
* deployment preparation record is stored
* governance is ready to evaluate deployment authorization

---

## 19.0 Execution Graph

The Execution Graph is the runtime structure used to coordinate phases, tasks, dependencies, artifacts, validations, repairs, and outcomes. It is derived from the Construction Task Graph but includes additional runtime state and execution records.

The Execution Graph allows the platform to monitor progress, recover from interruption, evaluate dependencies, and support audit reconstruction.

### 19.1 Execution Graph Schema

| Field                 | Type     | Required    | Description                                                |
| --------------------- | -------- | ----------- | ---------------------------------------------------------- |
| execution-graph-id    | string   | REQUIRED    | Unique execution graph identifier                          |
| plan-id               | string   | REQUIRED    | Construction plan identifier                               |
| specification-id      | string   | REQUIRED    | System identifier                                          |
| specification-version | semver   | REQUIRED    | Specification version                                      |
| phases                | array    | REQUIRED    | Execution phase nodes                                      |
| tasks                 | array    | REQUIRED    | Construction task nodes                                    |
| artifacts             | array    | CONDITIONAL | Artifact nodes                                             |
| validations           | array    | CONDITIONAL | Validation result nodes                                    |
| repairs               | array    | CONDITIONAL | Repair nodes                                               |
| governance-events     | array    | CONDITIONAL | Governance event nodes                                     |
| edges                 | array    | REQUIRED    | Dependency and derivation edges                            |
| status                | enum     | REQUIRED    | pending, in-progress, completed, failed, escalated, halted |
| created-at            | ISO 8601 | REQUIRED    | Creation timestamp                                         |
| updated-at            | ISO 8601 | REQUIRED    | Most recent update timestamp                               |

### 19.2 Execution Graph Rules

The Execution Graph MUST be stored as part of platform state.

The Execution Graph MUST be updated whenever task, artifact, validation, repair, or governance state changes.

The Execution Graph MUST preserve enough dependency information to resume execution after interruption.

The Execution Graph MUST support traversal from:

* phase to task
* task to artifact
* artifact to validation result
* validation failure to repair record
* artifact to source specification entity
* policy violation to governance event

---

## 20.0 Execution State Model

This section defines execution states used by the runtime. State consistency is required for safe scheduling, recovery, observability, and governance.

Execution state applies to phases, tasks, artifacts, validations, repairs, and the overall construction plan.

### 20.1 Construction Plan Execution States

| State       | Meaning                                                               |
| ----------- | --------------------------------------------------------------------- |
| pending     | Execution has not started                                             |
| in-progress | Execution is active                                                   |
| completed   | Execution completed successfully                                      |
| failed      | Execution ended because required criteria failed                      |
| escalated   | Execution requires governance or human action                         |
| halted      | Execution stopped by policy, runtime failure, or governance authority |

### 20.2 Phase States

| State       | Meaning                                   |
| ----------- | ----------------------------------------- |
| not-started | Phase has not begun                       |
| in-progress | Phase is active                           |
| completed   | Phase satisfied exit criteria             |
| skipped     | Phase was not applicable                  |
| failed      | Phase failed                              |
| escalated   | Phase requires governance or human action |

### 20.3 Artifact States

| State      | Meaning                                                              |
| ---------- | -------------------------------------------------------------------- |
| pending    | Artifact produced but not validated                                  |
| valid      | Artifact passed required validation                                  |
| failed     | Artifact failed validation                                           |
| repaired   | Artifact modified by repair cycle and awaiting or passing validation |
| escalated  | Artifact requires governance or human intervention                   |
| deprecated | Artifact has been superseded                                         |

### 20.4 State Transition Rules

The runtime MUST enforce valid state transitions.

Invalid state transitions MUST be recorded as execution integrity violations.

A failed artifact MAY transition to repaired only through a Repair Record.

A pending artifact MAY transition to valid only through a passed validation result.

An escalated task or artifact MUST NOT resume until the escalation has been resolved.

---

## 21.0 Agent Role Participation

This section defines how agent roles participate in execution. Detailed agent contracts are defined in later ISL documents, but the execution model must identify which roles are expected during each phase.

Agents operate under runtime control. They do not independently define execution flow.

### 21.1 Agent Role Mapping

| Role                      | Primary Execution Phase |
| ------------------------- | ----------------------- |
| Specification Interpreter | Phase 1                 |
| Architecture Planner      | Phase 2                 |
| Construction Planner      | Phase 2                 |
| Implementation Generator  | Phase 3                 |
| Review Agent              | Phase 4, Phase 6        |
| Repair Analyst            | Phase 5                 |
| Test Generator            | Phase 6                 |
| Security Validator        | Phase 7                 |
| Deployment Preparer       | Phase 9                 |

### 21.2 Agent Execution Rules

An agent MUST receive structured input from the runtime.

An agent output MUST be recorded before it influences execution state.

An agent output MUST NOT mark artifacts valid without deterministic validation.

An agent action MUST be associated with a task-id, phase, and traceability context.

If an agent fails to produce a valid output, the runtime MUST record an agent failure and determine whether to retry, route to another agent, repair, escalate, or halt.

---

## 22.0 Deterministic Tool Participation

This section defines how deterministic tools participate in execution. The detailed registration and invocation contract is defined by ISL v1.6, but this model establishes when and why tools are required during execution.

Tools provide objective validation and transformation capabilities that reasoning agents cannot guarantee.

### 22.1 Required Tool Uses

Deterministic tools MUST be used for applicable checks including:

* compilation
* linting
* schema validation
* contract validation
* unit and integration test execution
* security scanning
* dependency auditing
* infrastructure validation
* package or container build validation

### 22.2 Tool Failure Handling

Tool failures MUST be handled according to ISL v1.6 outcome rules.

A tool timeout MUST NOT be treated as success.

A tool error MUST be recorded and evaluated for retry, substitution, escalation, or halt.

Tool version drift MUST be recorded when the confirmed tool version differs from the registered version.

---

## 23.0 Governance Integration

This section defines how execution integrates with governance. Governance is not an external review after execution. It is an active control layer that may authorize, restrict, block, waive, or halt execution activities.

Execution MUST operate under the governance model defined in ISL v1.7.

### 23.1 Governance Checkpoints

The runtime MUST consult governance before or during:

* execution initiation
* policy enforcement
* repair escalation
* waiver application
* manual override
* deployment preparation
* deployment authorization
* continuation after escalation

### 23.2 Governance Event Recording

The runtime MUST record governance-relevant execution events, including:

* execution-started
* execution-halted
* policy-evaluated
* violation-detected
* repair-escalated
* waiver-applied
* override-applied
* deployment-preparation-ready
* execution-completed
* execution-failed

### 23.3 Governance Blocking Rule

If governance blocks an execution action, the runtime MUST NOT proceed with that action until governance records a resolution, waiver, override, or authorization.

---

## 24.0 Traceability Requirements During Execution

This section defines execution-time traceability obligations. Traceability is not something reconstructed after construction. It must be recorded as execution occurs.

Execution traceability ensures that every generated artifact can be traced backward to specification intent and every specification entity can be traced forward to construction outputs.

### 24.1 Required Traceability Links

The runtime MUST record links between:

* specification entities and construction tasks
* construction tasks and generated artifacts
* artifacts and validation results
* validation failures and repair records
* repair records and modified artifacts
* policies and policy evaluation records
* deployment artifacts and infrastructure entities
* governance events and affected tasks or artifacts

### 24.2 Traceability Timing

Traceability links MUST be recorded at the time the related event occurs.

The runtime MUST NOT defer traceability reconstruction until after execution.

### 24.3 Traceability Failure

If required traceability cannot be recorded, the affected artifact or task MUST be marked failed or escalated.

A construction plan MUST NOT be marked completed while required traceability links are missing.

---

## 25.0 Execution Error Taxonomy

This section defines execution-level error classes. These classes provide consistent reporting for runtime failures and support governance review, repair analysis, monitoring, and audit.

Execution errors differ from authored-language errors and canonical semantic errors. They occur during runtime activity.

### 25.1 Execution Error Record Schema

| Field           | Type     | Required    | Description                 |
| --------------- | -------- | ----------- | --------------------------- |
| error-id        | string   | REQUIRED    | Unique error identifier     |
| error-class     | enum     | REQUIRED    | Error class                 |
| severity        | enum     | REQUIRED    | blocking, high, medium, low |
| phase           | string   | REQUIRED    | Phase where error occurred  |
| task-id         | string   | CONDITIONAL | Related task                |
| artifact-id     | string   | CONDITIONAL | Related artifact            |
| tool-id         | string   | CONDITIONAL | Related tool                |
| agent-role      | string   | CONDITIONAL | Related agent               |
| message         | string   | REQUIRED    | Human-readable explanation  |
| required-action | string   | REQUIRED    | Required remediation        |
| recorded-at     | ISO 8601 | REQUIRED    | Time error was recorded     |

### 25.2 Error Classes

| Error Class                             | Description                             | Default Handling               |
| --------------------------------------- | --------------------------------------- | ------------------------------ |
| execution-precondition-failed           | Required execution precondition missing | halt                           |
| execution-plan-not-executable           | Construction plan cannot execute        | halt                           |
| execution-task-failed                   | Task failed during execution            | repair or escalate             |
| execution-artifact-missing              | Declared artifact was not produced      | repair or fail                 |
| execution-validation-failed             | Deterministic validation failed         | repair                         |
| execution-tool-timeout                  | Tool invocation timed out               | retry then repair or escalate  |
| execution-tool-error                    | Tool returned error                     | retry, substitute, or escalate |
| execution-agent-failure                 | Agent failed to produce valid output    | retry, substitute, or escalate |
| execution-repair-limit-reached          | Repair termination limit reached        | escalate or fail               |
| execution-policy-violation              | Governance policy violation occurred    | apply policy response          |
| execution-traceability-failure          | Required traceability link missing      | escalate or fail               |
| execution-state-integrity-violation     | Invalid state transition attempted      | halt and escalate              |
| execution-repository-failure            | Artifact Repository operation failed    | retry or halt                  |
| execution-deployment-preparation-failed | Deployment preparation failed           | escalate or fail               |

### 25.3 Error Handling Rules

Blocking execution errors MUST prevent completion.

Errors affecting governance, policy, traceability, or authorization MUST be recorded in governance state.

Errors affecting artifacts MUST be linked to artifact metadata.

Errors affecting tasks MUST be reflected in the Execution Graph.

---

## 26.0 Parallel Execution Rules

This section defines how parallel execution may occur safely. Parallel execution improves performance, but it must not violate dependency, governance, artifact, or traceability rules.

Parallelism is permitted only where the Construction Task Graph and Execution Graph prove that tasks are independent or dependency-safe.

### 26.1 Permitted Parallelism

Tasks MAY execute in parallel when:

* no dependency relationship exists between them
* they do not write to the same artifact location
* they do not require exclusive access to the same state resource
* governance policies do not require sequential review
* required tools and agents are available
* traceability can be recorded independently

### 26.2 Prohibited Parallelism

Tasks MUST NOT execute in parallel when:

* one task depends on another
* both tasks modify the same artifact without coordination
* execution would violate a governance checkpoint
* shared state consistency cannot be guaranteed
* repair of one task may invalidate the other task’s outputs

### 26.3 Parallel Failure Handling

Failure of one parallel task MUST NOT automatically halt unrelated tasks unless dependency, governance, or runtime configuration requires halt.

If a failed task is on the critical path, the runtime MUST evaluate whether dependent tasks must pause.

---

## 27.0 Runtime Recovery and Resumption

This section defines recovery behavior after interruption, crash, halt, or controlled suspension. Long-running autonomous construction must be resumable without losing execution integrity.

Recovery depends on durable state, execution graph persistence, artifact repository consistency, and traceability records.

### 27.1 Recovery Preconditions

The runtime MAY resume execution only if:

* Execution Graph state is available
* Artifact Repository state is available
* traceability state is consistent
* governance state is available
* unresolved escalations are not bypassed
* runtime configuration remains compatible

### 27.2 Recovery Rules

On recovery, the runtime MUST:

* reload the Execution Graph
* verify artifact repository consistency
* verify task states
* verify validation and repair records
* identify incomplete tasks
* identify tasks safe to resume
* record a recovery event

### 27.3 Recovery Event Schema

| Field              | Type     | Required    | Description                                    |
| ------------------ | -------- | ----------- | ---------------------------------------------- |
| recovery-id        | string   | REQUIRED    | Unique recovery identifier                     |
| execution-graph-id | string   | REQUIRED    | Execution graph recovered                      |
| recovered-at       | ISO 8601 | REQUIRED    | Time recovery occurred                         |
| recovery-reason    | string   | REQUIRED    | Reason recovery was required                   |
| resumed-tasks      | array    | CONDITIONAL | Tasks resumed                                  |
| blocked-tasks      | array    | CONDITIONAL | Tasks not safe to resume                       |
| consistency-status | enum     | REQUIRED    | consistent, inconsistent, partially-consistent |

If consistency-status is inconsistent, execution MUST NOT resume until the inconsistency is resolved.

---

## 28.0 Completion Criteria

This section defines when execution may be considered complete. Completion is not equivalent to artifact generation. Completion requires validation, testing, policy compliance, consolidation, deployment preparation, traceability, and governance readiness.

The runtime MUST evaluate completion criteria before marking the construction plan completed.

### 28.1 Required Completion Conditions

Execution MAY be marked completed only when all of the following are true:

* all required phases completed successfully
* all required tasks are completed or governance-waived
* all required artifacts are valid or governance-waived
* all must-have requirements have passed validation or approved waiver
* all blocking policy violations are resolved or waived
* all required tests passed or have approved waiver
* artifact consolidation completed successfully
* deployment preparation completed successfully
* traceability links are complete
* governance state contains required execution records
* Completion Report has been produced

### 28.2 Failed Completion

Execution MUST be marked failed when:

* a blocking precondition cannot be satisfied
* construction plan cannot execute
* repair limits are reached without convergence
* required validation cannot pass or be waived
* required policy violations cannot be resolved or waived
* traceability integrity cannot be established
* repository state cannot be made consistent

### 28.3 Escalated Completion State

Execution MUST be marked escalated when progress is suspended pending governance or human action.

An escalated execution MUST NOT be marked completed or failed until the escalation is resolved or governance formally closes the execution.

---

## 29.0 Completion Report

This section defines the required final execution report. The Completion Report provides the summary evidence needed for governance, audit, operations, and later maintenance.

The Completion Report MUST be generated for completed, failed, halted, or escalated executions.

### 29.1 Completion Report Schema

| Field                         | Type     | Required    | Description                          |
| ----------------------------- | -------- | ----------- | ------------------------------------ |
| completion-report-id          | string   | REQUIRED    | Unique report identifier             |
| execution-graph-id            | string   | REQUIRED    | Execution graph summarized           |
| specification-id              | string   | REQUIRED    | System identifier                    |
| specification-version         | semver   | REQUIRED    | Specification version                |
| final-status                  | enum     | REQUIRED    | completed, failed, halted, escalated |
| phases-completed              | array    | REQUIRED    | Completed phases                     |
| phases-failed                 | array    | CONDITIONAL | Failed phases                        |
| artifacts-produced            | array    | REQUIRED    | Artifact identifiers                 |
| artifacts-valid               | array    | CONDITIONAL | Valid artifact identifiers           |
| artifacts-failed              | array    | CONDITIONAL | Failed artifact identifiers          |
| validation-summary            | object   | REQUIRED    | Summary of validation outcomes       |
| repair-summary                | object   | REQUIRED    | Summary of repair attempts           |
| test-summary                  | object   | REQUIRED    | Summary of test outcomes             |
| policy-summary                | object   | REQUIRED    | Summary of policy outcomes           |
| traceability-status           | enum     | REQUIRED    | complete, incomplete, inconsistent   |
| deployment-preparation-status | enum     | REQUIRED    | ready, failed, not-applicable        |
| governance-events             | array    | CONDITIONAL | Governance event references          |
| generated-at                  | ISO 8601 | REQUIRED    | Report generation time               |

### 29.2 Report Integrity Rules

The Completion Report MUST be stored in the Artifact Repository or governance/audit record store.

The Completion Report MUST reference immutable execution records where supported.

The Completion Report MUST NOT claim completion if required validation, traceability, or governance conditions remain unresolved.

---

## 30.0 Observability Requirements

This section defines minimum observability requirements for execution. Detailed telemetry models are defined later in the ISL corpus, but execution must produce enough observable data to support monitoring, debugging, audit, and continuous improvement.

An execution runtime that cannot explain what it is doing cannot be trusted for autonomous construction.

### 30.1 Required Execution Telemetry

The runtime MUST record telemetry for:

* phase start and completion
* task start and completion
* artifact generation
* tool invocation
* validation result
* repair cycle
* test execution
* policy evaluation
* governance event
* escalation
* halt
* recovery
* completion

### 30.2 Minimum Telemetry Fields

Each telemetry event SHOULD include:

| Field              | Description                      |
| ------------------ | -------------------------------- |
| event-id           | Unique event identifier          |
| event-type         | Type of event                    |
| timestamp          | Event time                       |
| execution-graph-id | Execution graph reference        |
| phase              | Phase where event occurred       |
| task-id            | Related task when applicable     |
| artifact-id        | Related artifact when applicable |
| outcome            | Event outcome                    |
| severity           | Event severity when applicable   |

---

## 31.0 Conformance Requirements

This section defines what it means for execution runtimes and execution records to conform to ISL v1.2. Conformance is separated because a runtime, execution graph, and generated records have different responsibilities.

### 31.1 Runtime Conformance

An execution runtime conforms to ISL v1.2 if it can:

* enforce execution preconditions
* execute the required phase model
* maintain execution state
* invoke deterministic validation
* initiate bounded repair cycles
* enforce repair termination policy
* integrate governance checkpoints
* update traceability during execution
* produce required execution records
* enforce completion criteria
* produce Completion Reports

### 31.2 Execution Graph Conformance

An Execution Graph conforms to ISL v1.2 if it:

* references the correct specification and construction plan
* records phases, tasks, artifacts, validations, repairs, and governance events
* preserves dependency and derivation edges
* supports execution state recovery
* supports audit traversal
* reflects current runtime state accurately

### 31.3 Execution Record Conformance

Execution records conform to ISL v1.2 if they:

* use required schemas
* include required identifiers
* reference related tasks, artifacts, tools, agents, and policies
* use valid outcome values
* include timestamps
* preserve traceability context
* are stored in the required state, repository, telemetry, or governance location

---

## 32.0 Summary

ISL v1.2 defines the execution model that governs how an Autonomous-Ready specification is transformed into validated construction outputs. It establishes a controlled lifecycle of interpretation, planning confirmation, artifact generation, deterministic validation, bounded repair, testing, security and policy validation, consolidation, and deployment preparation.

This revision strengthens the execution model by making phase contracts, inputs, outputs, state transitions, repair termination, governance integration, traceability requirements, errors, recovery behavior, and completion criteria explicit.

A conforming IMHOTEP execution runtime MUST treat execution as a governed, traceable, deterministic-validation-driven process. It MUST NOT treat autonomous generation as sufficient evidence of correctness.
