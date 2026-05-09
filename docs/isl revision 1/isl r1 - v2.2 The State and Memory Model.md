# IMHOTEP Specification Language (ISL) v2.2

# The State and Memory Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1
**Supersedes:** ISL r0 v2.2 The State and Memory Model
**Document Type:** Platform State, Memory, and Consistency Specification

---

## 1.0 Scope

This document defines the State and Memory Model for the IMHOTEP platform. It establishes how platform state is represented, persisted, versioned, updated, validated, recovered, queried, and exposed to authorized subsystems and agents during autonomous software construction.

This document applies to all state and memory maintained by the IMHOTEP platform across specification authoring, canonical normalization, readiness evaluation, construction planning, execution, agent collaboration, deterministic validation, repair cycles, artifact lifecycle management, governance enforcement, deployment preparation, observability, and audit.

This document defines:

* state and memory principles
* categories of platform state
* state record schemas
* state transition rules
* cross-state consistency requirements
* durable memory structures
* event log requirements
* snapshot requirements
* recovery and resumption behavior
* agent memory access rules
* state versioning and evolution
* retention and archival rules
* integrity checks
* state and memory error classes
* conformance requirements

---

## 2.0 Purpose of the State and Memory Model

The purpose of the State and Memory Model is to ensure that the IMHOTEP platform can maintain continuity across long-running autonomous construction activity. Autonomous construction may involve many specifications, task graphs, reasoning agents, deterministic tools, validation cycles, repair attempts, governance gates, artifact versions, and deployment preparation activities. Without a structured state and memory model, the platform would be unable to coordinate these activities safely.

State represents the current operational condition of the platform. Memory represents the durable historical record of how the platform reached that condition. State allows the platform to decide what should happen next. Memory allows the platform to explain what happened before, recover after interruption, support audit, and evolve systems over time.

The State and Memory Model exists to ensure that:

* execution can resume after interruption
* task status remains consistent with artifact and validation status
* specification versions are linked to derived plans and artifacts
* agent outputs are preserved and reusable under controlled rules
* governance records remain durable and auditable
* traceability relationships are preserved across lifecycle events
* impact analysis can compare current and historical states
* recovery procedures can detect and repair inconsistent state
* no subsystem relies on hidden, unstructured, or transient memory for critical decisions

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the State and Memory Model. State and memory interact with every major platform subsystem, so this document depends on the language, canonical, execution, readiness, traceability, planning, tool, governance, architecture, and agent models.

| Reference     | Purpose                                                                                      |
| ------------- | -------------------------------------------------------------------------------------------- |
| ISL v0.0      | Corpus-level principles, conformance, and system integrity rules                             |
| ISL v1.1      | Canonical entity schemas, identifiers, and canonical model versioning                        |
| ISL v1.2      | Execution graph, phase state, task state, artifact state, repair, recovery, and completion   |
| ISL v1.3      | Readiness state, transition records, regression, and evidence packages                       |
| ISL v1.4      | Traceability graph, snapshots, integrity checks, and audit queries                           |
| ISL v1.5      | Construction task graph, task status transitions, planning adaptation, and execution handoff |
| ISL v1.6      | Tool records, tool invocation results, findings, version drift, and tool telemetry           |
| ISL v1.7      | Governance state, approvals, waivers, overrides, escalations, and audit logs                 |
| ISL v2.0      | Platform architecture and subsystem boundaries                                               |
| ISL v2.1      | Agent invocation, context package, output, collaboration, and failure records                |
| OpenTelemetry | Observability alignment for state transition telemetry                                       |
| W3C PROV-DM   | Provenance alignment for historical memory and lifecycle derivation                          |

---

## 4.0 Terms and Definitions

This section defines terms specific to platform state and memory. These terms distinguish current operational state from durable historical memory and clarify how the platform records change over time.

**State**
The current operational condition of a platform object, subsystem, lifecycle phase, task, artifact, validation, governance gate, agent invocation, or execution graph.

**Memory**
Durable historical records preserving prior state, lifecycle events, decisions, artifacts, validations, governance actions, and traceability relationships.

**State Record**
A structured record representing the current state of a platform object.

**State Transition**
A controlled movement from one allowed state value to another.

**State Event**
An immutable record that a state transition or significant lifecycle event occurred.

**Memory Store**
A durable storage mechanism that preserves historical records, event logs, snapshots, documents, graph state, and evidence.

**Snapshot**
A point-in-time capture of one or more state categories or graph structures.

**Checkpoint**
A runtime marker indicating a safe recovery point.

**Recovery**
The process of restoring consistent platform state after interruption, crash, halt, or partial failure.

**State Consistency**
The condition in which related state categories agree according to defined invariants.

**State Drift**
A condition in which related state records become inconsistent, stale, orphaned, or contradictory.

**Working Memory**
Task-scoped or session-scoped context assembled for a limited operation.

**Persistent Memory**
Durable records retained beyond a single task, session, or runtime process.

---

## 5.0 State and Memory Design Principles

This section defines the principles governing state and memory. These principles ensure that the platform can coordinate long-running activity, recover safely, preserve audit evidence, and avoid hidden or inconsistent operational context.

### 5.1 State Is Explicit

All lifecycle-relevant state MUST be represented in structured platform records. The platform MUST NOT rely on hidden process memory, transient prompt context, implicit local variables, file names, or informal logs as the only source of truth for critical lifecycle decisions.

Explicit state is required for runtime scheduling, recovery, governance, traceability, and audit.

### 5.2 Memory Is Durable

Historical records that support traceability, audit, recovery, governance, validation, or artifact reconstruction MUST be persisted durably.

A platform MUST NOT lose governance approvals, traceability links, validation results, repair records, artifact associations, or readiness transitions when a process restarts.

### 5.3 State Transitions Are Controlled

State values MUST change only through defined transitions. Invalid transitions MUST be rejected or escalated.

Controlled transitions prevent artifacts, tasks, approvals, and execution phases from entering impossible or unsafe combinations.

### 5.4 State and Memory Are Versioned

Specifications, canonical models, construction plans, execution graphs, artifacts, governance records, agent outputs, and traceability snapshots MUST preserve version context.

The platform MUST be able to distinguish the current state of the latest version from the historical state of prior versions.

### 5.5 State Consistency Is Enforced

Related state categories MUST remain consistent. For example, a task MUST NOT be completed if its required artifact remains pending, and an artifact MUST NOT be stable if required validation failed.

The platform MUST perform consistency checks at defined checkpoints.

### 5.6 Memory Access Is Governed

Agents and subsystems MUST access memory through controlled interfaces. They MUST receive only the state and memory relevant to their task, role, risk tier, authorization, and context package.

Memory access MUST preserve security, privacy, governance, and traceability constraints.

### 5.7 Recovery Must Be Deterministic

After interruption, restart, crash, or controlled halt, the platform MUST reconstruct state deterministically from durable records, snapshots, and event logs.

A platform MUST NOT continue execution from uncertain or contradictory state without recovery validation.

---

## 6.0 State and Memory Architecture

This section defines the architectural organization of state and memory. The State and Memory Model does not require one specific database technology. It defines logical stores and responsibilities that a conforming implementation must provide.

State and memory MAY be implemented using relational databases, graph databases, document stores, object stores, event logs, filesystem-backed state, embedded databases, or hybrid storage mechanisms. The implementation is acceptable only if it preserves the semantics required by this document.

### 6.1 Logical Stores

| Store                     | Responsibility                                                                                       |
| ------------------------- | ---------------------------------------------------------------------------------------------------- |
| Current State Store       | Maintains latest state records for platform objects                                                  |
| Event Log                 | Records immutable state transition and lifecycle events                                              |
| Graph Store               | Maintains canonical, traceability, and dependency relationships                                      |
| Artifact Metadata Store   | Maintains artifact metadata, status, versions, and repository links                                  |
| Governance Record Store   | Maintains approvals, waivers, overrides, policies, and audit records                                 |
| Agent Memory Store        | Maintains agent invocations, context packages, outputs, handoffs, failures, and conflicts            |
| Evidence Store            | Maintains validation reports, tool outputs, test reports, security findings, and deployment evidence |
| Snapshot Store            | Maintains point-in-time state, graph, and lifecycle snapshots                                        |
| Configuration State Store | Maintains versioned platform, runtime, tool, model, and governance configuration                     |

### 6.2 Store Rules

A conforming platform MAY combine logical stores into the same physical storage backend.

A conforming platform MUST preserve the logical responsibilities of each store.

State records that affect readiness, execution, artifact validity, governance, traceability, or deployment MUST be durable.

Event logs and governance audit records MUST be append-only or integrity-protected.

---

## 7.0 Platform State Categories

This section defines the required categories of platform state. These categories correspond to major lifecycle concerns and platform subsystems. The original v2.2 model identified specification, planning, execution, artifact, and governance state as core categories; this revision formalizes those categories and extends them to cover traceability, agent, tool, model, observability, and configuration state.

### 7.1 Required State Categories

| State Category        | Description                                                                            |
| --------------------- | -------------------------------------------------------------------------------------- |
| Specification State   | Current and historical authored specification and readiness metadata                   |
| Canonical Model State | Current and historical canonical semantic model state                                  |
| Readiness State       | Readiness level, transition records, evidence packages, and regression state           |
| Planning State        | Construction plans, task graphs, dependencies, critical path, and plan adaptation      |
| Execution State       | Execution graphs, phases, tasks, runtime activity, repair, and completion              |
| Artifact State        | Artifact metadata, versions, validation status, repository location, and lifecycle     |
| Traceability State    | Traceability graph, snapshots, integrity checks, and impact analysis                   |
| Governance State      | Policies, approvals, waivers, overrides, escalations, risk tiers, and audit logs       |
| Tool State            | Tool records, trust profiles, invocations, findings, and version drift                 |
| Agent State           | Agent invocations, context packages, outputs, handoffs, failures, and conflicts        |
| Model State           | Model provider records, model invocations, routing decisions, and output normalization |
| Observability State   | Telemetry events, alerts, metrics, logs, traces, and monitoring summaries              |
| Configuration State   | Runtime, platform, governance, tool, model, and deployment configuration               |

### 7.2 Category Rules

Each required state category MUST have a defined owner subsystem.

Each state category MUST define current state records and historical memory records where applicable.

State categories MUST be linked through identifiers rather than unstructured textual references.

State category updates MUST emit events when the update affects lifecycle behavior, governance, traceability, validation, or execution.

---

## 8.0 Common State Record Schema

This section defines the common fields that SHOULD appear in platform state records. Specific state categories extend this schema with category-specific fields.

The common schema ensures that state records can be correlated, audited, versioned, and queried consistently.

### 8.1 Base State Record Schema

| Field | Type | Required | Description |
|---|---|---|
| state-record-id | string | REQUIRED | Unique state record identifier |
| state-category | enum | REQUIRED | State category from §7.1 |
| object-id | string | REQUIRED | Identifier of object whose state is represented |
| object-type | string | REQUIRED | Type of object represented |
| specification-id | string | CONDITIONAL | Required when state is specification-scoped |
| specification-version | semver | CONDITIONAL | Required when state is specification-version scoped |
| current-state | string | REQUIRED | Current state value |
| prior-state | string | CONDITIONAL | Prior state value when transition occurred |
| state-version | semver | REQUIRED | Version of this state record |
| updated-at | ISO 8601 | REQUIRED | Time state was updated |
| updated-by | string | REQUIRED | Subsystem, role, agent, tool, or runtime updating state |
| correlation-id | string | CONDITIONAL | Related execution, task, event, or governance check |
| traceability-node-id | string | CONDITIONAL | Traceability node associated with object |
| integrity-hash | string | CONDITIONAL | Integrity hash when supported |

### 8.2 Base State Record Rules

A state record MUST identify the object whose state it represents.

A state record MUST identify the current-state value.

A state update affecting lifecycle behavior MUST preserve prior-state or emit a state event containing prior-state.

State records MUST NOT be silently overwritten when historical reconstruction is required.

State records associated with specification-scoped activity MUST include specification-id and specification-version.

---

## 9.0 State Event Model

This section defines the event model used to record state changes. State events are the durable memory of how state evolved over time. They provide the basis for recovery, audit, debugging, telemetry correlation, and historical reconstruction.

State records represent the current condition. State events represent the chronological history.

### 9.1 State Event Schema

| Field | Type | Required | Description |
|---|---|---|
| state-event-id | string | REQUIRED | Unique state event identifier |
| event-type | string | REQUIRED | Type of state event |
| state-category | enum | REQUIRED | State category affected |
| object-id | string | REQUIRED | Object affected |
| object-type | string | REQUIRED | Type of object affected |
| prior-state | string | CONDITIONAL | State before event |
| new-state | string | CONDITIONAL | State after event |
| specification-id | string | CONDITIONAL | Related specification identifier |
| specification-version | semver | CONDITIONAL | Related specification version |
| caused-by | string | REQUIRED | Task, invocation, governance action, subsystem, or user causing event |
| correlation-id | string | CONDITIONAL | Related execution graph, task, invocation, or transaction |
| event-time | ISO 8601 | REQUIRED | Time event occurred |
| event-sequence | integer | REQUIRED | Monotonic sequence within scope |
| evidence | array | CONDITIONAL | Evidence supporting event |
| metadata | object | OPTIONAL | Supplemental metadata |

### 9.2 State Event Rules

State events that affect readiness, governance, artifact validity, execution completion, validation, repair, or traceability MUST be durable.

State events MUST be append-only where used for audit or recovery.

State events MUST be ordered within their scope using event-sequence or equivalent ordering.

A state event MUST NOT be removed from historical memory unless retention policy permits archival and audit requirements are preserved.

### 9.3 Required State Event Types

The platform MUST emit state events for:

* specification-created
* specification-versioned
* canonical-model-generated
* readiness-transitioned
* readiness-regressed
* construction-plan-created
* construction-plan-adapted
* execution-started
* phase-state-changed
* task-state-changed
* artifact-created
* artifact-state-changed
* validation-recorded
* repair-started
* repair-completed
* traceability-link-created
* traceability-integrity-checked
* governance-event-recorded
* tool-invocation-recorded
* agent-invocation-recorded
* model-invocation-recorded
* deployment-preparation-recorded
* snapshot-created
* recovery-started
* recovery-completed

---

## 10.0 Specification State

This section defines Specification State. Specification State represents the current and historical condition of authored ISL specifications, including version, ownership, readiness pointer, and relationship to canonical model versions.

Specification State ensures that the platform always knows which blueprint it is acting upon.

### 10.1 Specification State Schema

| Field | Type | Required | Description |
|---|---|---|
| specification-state-id | string | REQUIRED | Unique specification state identifier |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Authored specification version |
| isl-version | string | REQUIRED | ISL version used |
| owner | string | REQUIRED | Owning organization, team, or role |
| authored-source-reference | string | REQUIRED | Location or reference to authored source |
| status | enum | REQUIRED | draft, reviewable, machine-valid, autonomous-ready, deprecated, superseded |
| current-canonical-model-version | semver | CONDITIONAL | Canonical model version derived from this specification |
| readiness-state-id | string | CONDITIONAL | Current readiness state record |
| created-at | ISO 8601 | REQUIRED | Creation time |
| updated-at | ISO 8601 | REQUIRED | Last update time |
| supersedes-version | semver | CONDITIONAL | Prior specification version replaced |
| superseded-by-version | semver | CONDITIONAL | Later specification version replacing this one |

### 10.2 Specification State Rules

Every authored specification version MUST have a Specification State record.

Specification State MUST preserve links to its canonical model versions.

A specification version that reached Machine-Valid or Autonomous-Ready MUST NOT be deleted from memory while its generated artifacts remain active.

A new specification version MUST NOT overwrite the prior version’s state.

---

## 11.0 Canonical Model State

This section defines Canonical Model State. Canonical Model State records the normalized semantic representation of a specification and its validation outcome.

Canonical Model State is required because planning, traceability, readiness, and execution depend on the canonical graph, not on authored prose alone.

### 11.1 Canonical Model State Schema

| Field | Type | Required | Description |
|---|---|---|
| canonical-state-id | string | REQUIRED | Unique canonical model state identifier |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Source specification version |
| canonical-model-version | semver | REQUIRED | Canonical model version |
| normalization-status | enum | REQUIRED | pending, completed, failed, superseded |
| validation-status | enum | REQUIRED | not-evaluated, passed, failed, warning |
| canonical-graph-reference | string | REQUIRED | Reference to canonical graph |
| validation-report-id | string | CONDITIONAL | Canonical validation report |
| generated-at | ISO 8601 | REQUIRED | Time generated |
| generated-by | string | REQUIRED | Semantic Model Engine or processor |
| superseded-by-model-version | semver | CONDITIONAL | Later canonical model version |

### 11.2 Canonical Model State Rules

A canonical model with validation-status failed MUST NOT support Machine-Valid readiness.

A canonical model version MUST reference exactly one specification version.

A specification version MAY have multiple canonical model attempts, but only one current valid canonical model MAY be active for planning at a time.

Canonical Model State MUST be preserved for historical versions used in planning or execution.

---

## 12.0 Readiness State

This section defines Readiness State. Readiness State records the maturity and authorization condition of a specification version according to ISL v1.3.

Readiness State is a hard control input for planning and execution. It must be durable, version-specific, and governance-aware.

### 12.1 Readiness State Schema

| Field | Type | Required | Description |
|---|---|---|
| readiness-state-id | string | REQUIRED | Unique readiness state identifier |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| readiness-level | enum | REQUIRED | draft, reviewable, machine-valid, autonomous-ready |
| transition-record-id | string | CONDITIONAL | Most recent transition record |
| readiness-report-id | string | CONDITIONAL | Most recent readiness report |
| evidence-package-id | string | CONDITIONAL | Evidence package supporting state |
| regression-status | enum | REQUIRED | none, regression-required, regressed, reauthorized |
| governance-authorization-id | string | CONDITIONAL | Required for Autonomous-Ready |
| updated-at | ISO 8601 | REQUIRED | Last readiness state update |

### 12.2 Readiness State Rules

A specification version MUST have exactly one current Readiness State.

Autonomous-Ready Readiness State MUST reference governance authorization.

Regression MUST update Readiness State and emit readiness-regressed event.

Planning and execution subsystems MUST consult Readiness State before formal planning or execution.

---

## 13.0 Planning State

This section defines Planning State. Planning State records the current and historical condition of construction plans, task graphs, dependencies, critical path, verification plans, and adaptation records.

Planning State enables the platform to resume planning, adapt plans after change, and hand off validated plans to execution.

### 13.1 Planning State Schema

| Field | Type | Required | Description |
|---|---|---|
| planning-state-id | string | REQUIRED | Unique planning state identifier |
| plan-id | string | REQUIRED | Construction plan identifier |
| plan-version | semver | REQUIRED | Plan version |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version planned |
| canonical-model-version | semver | REQUIRED | Canonical model version used |
| plan-status | enum | REQUIRED | draft, valid, executable, in-progress, completed, failed, superseded |
| task-graph-reference | string | REQUIRED | Reference to task graph |
| validation-report-id | string | CONDITIONAL | Planning validation report |
| critical-path-id | string | CONDITIONAL | Critical path record |
| adaptation-records | array | CONDITIONAL | Plan adaptation records |
| current-execution-graph-id | string | CONDITIONAL | Execution graph using this plan |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 13.2 Planning State Rules

A plan MUST NOT be marked executable unless planning validation passed.

A plan marked executable MUST reference Machine-Valid or Autonomous-Ready specification state.

A plan used by active execution MUST NOT be overwritten by adaptation. Adaptation MUST create a new plan version or explicitly versioned graph state.

A superseded plan MUST remain queryable if it was used for execution, governance review, or audit.

---

## 14.0 Execution State

This section defines Execution State. Execution State records the current condition of runtime execution, including execution graph status, phase status, task status, repair state, and completion state.

Execution State must be persistent because autonomous construction may run for long durations and must recover after interruption.

### 14.1 Execution State Schema

| Field | Type | Required | Description |
|---|---|---|
| execution-state-id | string | REQUIRED | Unique execution state identifier |
| execution-graph-id | string | REQUIRED | Execution graph identifier |
| plan-id | string | REQUIRED | Construction plan being executed |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| execution-status | enum | REQUIRED | pending, in-progress, completed, failed, escalated, halted, recovering |
| current-phase | string | CONDITIONAL | Current execution phase |
| phase-states | array | REQUIRED | Current phase state records |
| task-states | array | REQUIRED | Current task state records or references |
| active-agent-invocations | array | CONDITIONAL | Active agent invocation identifiers |
| active-tool-invocations | array | CONDITIONAL | Active tool invocation identifiers |
| active-repair-records | array | CONDITIONAL | Repair records currently in progress |
| last-checkpoint-id | string | CONDITIONAL | Most recent recovery checkpoint |
| completion-report-id | string | CONDITIONAL | Completion report when terminal |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 14.2 Execution State Rules

Execution State MUST be updated when phase, task, artifact, validation, repair, governance, tool, or agent state changes affect execution progress.

Execution State MUST NOT be marked completed unless ISL v1.2 completion criteria are satisfied.

Execution State MUST enter recovering during recovery validation after interruption.

Execution State MUST NOT resume from recovering until consistency checks pass.

### 14.3 Phase State Values

| State       | Meaning                                   |
| ----------- | ----------------------------------------- |
| not-started | Phase has not begun                       |
| in-progress | Phase is active                           |
| completed   | Phase completed successfully              |
| skipped     | Phase was not applicable                  |
| failed      | Phase failed                              |
| escalated   | Phase requires governance or human action |

### 14.4 Execution Status Values

| State       | Meaning                                                 |
| ----------- | ------------------------------------------------------- |
| pending     | Execution has not started                               |
| in-progress | Execution is active                                     |
| completed   | Execution completed successfully                        |
| failed      | Execution ended because required criteria failed        |
| escalated   | Execution requires governance or human action           |
| halted      | Execution was stopped by runtime, policy, or governance |
| recovering  | Execution is validating state for recovery              |

---

## 15.0 Task State

This section defines Task State. Task State records the current lifecycle status of construction tasks within a construction plan or execution graph.

Task State must remain consistent with artifact, validation, repair, and governance state.

### 15.1 Task State Schema

| Field | Type | Required | Description |
|---|---|---|
| task-state-id | string | REQUIRED | Unique task state record identifier |
| task-id | string | REQUIRED | Construction task identifier |
| plan-id | string | REQUIRED | Construction plan identifier |
| execution-graph-id | string | CONDITIONAL | Execution graph identifier when active |
| task-type | enum | REQUIRED | Task type from ISL v1.5 |
| current-state | enum | REQUIRED | pending, in-progress, completed, failed, escalated, skipped |
| assigned-agent-role | string | CONDITIONAL | Assigned agent role |
| assigned-tool-id | string | CONDITIONAL | Tool used for deterministic task |
| produced-artifact-ids | array | CONDITIONAL | Artifacts produced by task |
| validation-result-ids | array | CONDITIONAL | Validation results associated with task |
| repair-record-ids | array | CONDITIONAL | Repair records associated with task |
| governance-gate-ids | array | CONDITIONAL | Governance gates affecting task |
| started-at | ISO 8601 | CONDITIONAL | Time task started |
| completed-at | ISO 8601 | CONDITIONAL | Time task completed |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 15.2 Task State Transition Rules

Task State MUST follow the permitted task status transitions defined in ISL v1.5.

| From        | To          | Condition                                                          |
| ----------- | ----------- | ------------------------------------------------------------------ |
| pending     | in-progress | Task assigned to agent, tool, or platform component                |
| pending     | skipped     | Valid non-applicability or governance decision exists              |
| in-progress | completed   | Required outputs produced and required validation passed or waived |
| in-progress | failed      | Task failed or produced invalid outputs                            |
| in-progress | escalated   | Governance, repair, or runtime escalation required                 |
| failed      | in-progress | Repair or retry authorized                                         |
| failed      | escalated   | Repair not possible or threshold reached                           |
| escalated   | in-progress | Escalation resolved and retry authorized                           |
| escalated   | failed      | Governance closes task as failed                                   |
| completed   | pending     | Specification change or impact analysis invalidates task           |
| completed   | skipped     | Governance or impact analysis determines task no longer applicable |

No other Task State transitions are permitted.

### 15.3 Task State Consistency Rules

A task MUST NOT be marked completed if required artifacts are missing.

A task MUST NOT be marked completed if required validation failed and no valid waiver exists.

A failed task with generated artifacts MUST ensure those artifacts are marked failed, pending, or non-stable.

A task in escalated state MUST reference at least one escalation, approval gate, waiver requirement, or governance event.

---

## 16.0 Artifact State

This section defines Artifact State. Artifact State records the lifecycle condition of generated or managed artifacts, including status, version, repository location, validation state, traceability state, and supersession.

Artifact State is required for reconstruction, audit, repair, deployment preparation, and impact analysis.

### 16.1 Artifact State Schema

| Field | Type | Required | Description |
|---|---|---|
| artifact-state-id | string | REQUIRED | Unique artifact state identifier |
| artifact-id | string | REQUIRED | Artifact identifier |
| artifact-type | enum | REQUIRED | source, config, schema, infrastructure, contract, test, documentation, deployment |
| artifact-version | semver | REQUIRED | Artifact version |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| task-id | string | REQUIRED | Task that produced artifact |
| repository-location | string | REQUIRED | Artifact repository location |
| content-hash | string | CONDITIONAL | Artifact content hash when supported |
| current-state | enum | REQUIRED | pending, valid, failed, repaired, escalated, deprecated, superseded, stable |
| validation-status | enum | REQUIRED | not-evaluated, passed, failed, warning, waived |
| traceability-status | enum | REQUIRED | complete, incomplete, inconsistent |
| supersedes-artifact-id | string | CONDITIONAL | Prior artifact replaced |
| superseded-by-artifact-id | string | CONDITIONAL | Later artifact replacing this one |
| created-at | ISO 8601 | REQUIRED | Time artifact was created |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 16.2 Artifact State Rules

An artifact MUST NOT be marked valid unless required validation has passed or has valid waiver.

An artifact MUST NOT be marked stable unless traceability-status is complete.

An artifact with validation-status failed MUST NOT be marked stable.

A repaired artifact MUST reference the artifact it supersedes when a new version is produced.

A superseded artifact MUST remain available for audit or reconstruction according to retention rules.

### 16.3 Artifact State Values

| State      | Meaning                                                                 |
| ---------- | ----------------------------------------------------------------------- |
| pending    | Artifact created but not fully validated                                |
| valid      | Artifact passed required validation                                     |
| failed     | Artifact failed validation                                              |
| repaired   | Artifact was modified through repair and awaits or passed revalidation  |
| escalated  | Artifact requires governance or human intervention                      |
| deprecated | Artifact should not be used for new construction                        |
| superseded | Artifact has been replaced by a later version                           |
| stable     | Artifact is valid, traceable, and accepted into stable repository state |

---

## 17.0 Validation State

This section defines Validation State. Validation State records the condition and lifecycle of validation activities and outcomes.

Validation State connects tool results, test results, policy checks, artifact acceptance, repair triggers, and execution completion.

### 17.1 Validation State Schema

| Field | Type | Required | Description |
|---|---|---|
| validation-state-id | string | REQUIRED | Unique validation state identifier |
| validation-result-id | string | REQUIRED | Validation result identifier |
| validation-entity-id | string | CONDITIONAL | ISL Validation entity when applicable |
| task-id | string | REQUIRED | Task associated with validation |
| artifact-ids | array | CONDITIONAL | Artifacts evaluated |
| tool-invocation-id | string | CONDITIONAL | Tool invocation producing result |
| validation-type | enum | REQUIRED | test, inspection, static-analysis, security-scan, policy-check, operational-check, governance-review |
| current-state | enum | REQUIRED | pending, passed, failed, warning, waived, error, timeout |
| findings | array | CONDITIONAL | Findings associated with validation |
| waiver-id | string | CONDITIONAL | Waiver authorizing non-passed result |
| repair-record-id | string | CONDITIONAL | Repair triggered by failure |
| evaluated-at | ISO 8601 | CONDITIONAL | Time validation evaluated |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 17.2 Validation State Rules

A failed validation linked to a must-have requirement MUST block task completion unless a valid waiver exists.

A timeout MUST NOT be converted into passed state.

A waived validation MUST reference an active waiver record.

A failed validation that triggers repair MUST reference the associated Repair Record.

---

## 18.0 Repair State

This section defines Repair State. Repair State records the lifecycle condition of repair cycles initiated after validation, test, tool, policy, or execution failure.

Repair State is necessary to enforce termination limits, detect non-convergence, and preserve repair history.

### 18.1 Repair State Schema

| Field | Type | Required | Description |
|---|---|---|
| repair-state-id | string | REQUIRED | Unique repair state identifier |
| repair-record-id | string | REQUIRED | Repair record identifier |
| artifact-id | string | REQUIRED | Artifact being repaired |
| task-id | string | REQUIRED | Related task |
| failed-validation-result-id | string | REQUIRED | Validation result triggering repair |
| iteration | integer | REQUIRED | Repair attempt number |
| current-state | enum | REQUIRED | pending, in-progress, resolved, failed, escalated, abandoned |
| termination-policy | object | REQUIRED | Repair limits in effect |
| proposed-correction | string | CONDITIONAL | Correction proposed |
| modified-artifact-ids | array | CONDITIONAL | Artifacts modified |
| revalidation-result-id | string | CONDITIONAL | Validation result after repair |
| started-at | ISO 8601 | CONDITIONAL | Start time |
| completed-at | ISO 8601 | CONDITIONAL | Completion time |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 18.2 Repair State Rules

Repair State MUST enforce repair iteration limits defined by ISL v1.2.

A repair MUST NOT be marked resolved without successful revalidation or valid waiver.

A repair reaching escalation threshold MUST enter escalated state.

Repair history MUST be preserved even when repair fails or is abandoned.

---

## 19.0 Governance State

This section defines Governance State. Governance State records policies, approvals, waivers, overrides, escalations, risk tiers, deployment authorizations, governance checks, and audit events.

Governance State is part of platform control. It must be durable, immutable where audit-relevant, and consulted by runtime subsystems.

### 19.1 Governance State Schema

| Field | Type | Required | Description |
|---|---|---|
| governance-state-id | string | REQUIRED | Unique governance state identifier |
| specification-id | string | CONDITIONAL | System identifier when scoped |
| specification-version | semver | CONDITIONAL | Specification version when scoped |
| active-risk-tier | enum | CONDITIONAL | standard, elevated, high, critical |
| active-approval-gates | array | CONDITIONAL | Pending or active approval gates |
| active-waivers | array | CONDITIONAL | Active waiver identifiers |
| active-overrides | array | CONDITIONAL | Active override identifiers |
| open-escalations | array | CONDITIONAL | Open escalation identifiers |
| policy-profile-id | string | CONDITIONAL | Active policy profile |
| audit-log-reference | string | REQUIRED | Governance audit log reference |
| current-state | enum | REQUIRED | active, blocked, escalated, suspended, closed |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 19.2 Governance State Rules

Governance State MUST be consulted before governed actions.

A blocked Governance State MUST prevent affected actions.

Expired waivers and overrides MUST NOT authorize progression.

Governance audit events MUST be immutable and queryable.

A governance audit write failure MUST block governance-controlled actions.

---

## 20.0 Traceability State

This section defines Traceability State. Traceability State records the lifecycle condition of the traceability graph, snapshots, impact analysis, and integrity checks.

Traceability State confirms whether the platform can explain relationships between specification intent and generated outputs.

### 20.1 Traceability State Schema

| Field | Type | Required | Description |
|---|---|---|
| traceability-state-id | string | REQUIRED | Unique traceability state identifier |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| traceability-graph-reference | string | REQUIRED | Traceability graph reference |
| current-snapshot-id | string | CONDITIONAL | Latest traceability snapshot |
| integrity-status | enum | REQUIRED | not-checked, passed, failed, warning |
| open-integrity-findings | array | CONDITIONAL | Active integrity findings |
| last-impact-analysis-id | string | CONDITIONAL | Most recent impact analysis |
| current-state | enum | REQUIRED | initialized, active, incomplete, inconsistent, archived |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 20.2 Traceability State Rules

Traceability State MUST be initialized before formal planning.

Traceability State MUST be active before execution begins.

A traceability state with integrity-status failed MUST block execution completion.

Artifacts MUST NOT reach stable state while traceability status is incomplete or inconsistent.

---

## 21.0 Agent State

This section defines Agent State. Agent State records agent invocations, context packages, outputs, handoffs, failures, conflicts, and collaboration sessions.

Agent State supports long-running collaboration, controlled memory access, auditability, and recovery after interruption.

### 21.1 Agent State Schema

| Field | Type | Required | Description |
|---|---|---|
| agent-state-id | string | REQUIRED | Unique agent state identifier |
| agent-invocation-id | string | REQUIRED | Agent invocation identifier |
| agent-role | enum | REQUIRED | Agent role |
| task-id | string | REQUIRED | Related task |
| context-package-id | string | REQUIRED | Context package used |
| model-invocation-id | string | CONDITIONAL | Model invocation supporting agent |
| agent-output-id | string | CONDITIONAL | Output produced |
| current-state | enum | REQUIRED | requested, context-assembled, governance-checked, model-invoked, output-received, output-normalized, output-validated, completed, failed, escalated, cancelled |
| failure-id | string | CONDITIONAL | Failure record when failed |
| collaboration-session-id | string | CONDITIONAL | Collaboration session when applicable |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 21.2 Agent State Rules

Agent State MUST follow lifecycle states defined in ISL v2.1.

Agent outputs MUST NOT be used downstream until output validation passes.

Agent State MUST preserve the context-package-id used for invocation.

Agent State containing restricted context MUST be governed by memory access controls.

---

## 22.0 Tool State

This section defines Tool State. Tool State records registered tools, trust profiles, invocations, results, findings, conflicts, version drift, and evidence.

Tool State supports deterministic validation, repair feedback, governance, reproducibility, and audit.

### 22.1 Tool State Schema

| Field | Type | Required | Description |
|---|---|---|
| tool-state-id | string | REQUIRED | Unique tool state identifier |
| tool-id | string | REQUIRED | Tool identifier |
| tool-record-version | string | REQUIRED | Version of Tool Record |
| trust-profile-id | string | REQUIRED | Tool trust profile |
| current-state | enum | REQUIRED | active, deprecated, disabled, experimental, revoked |
| active-invocations | array | CONDITIONAL | Active tool invocation identifiers |
| recent-version-drift-records | array | CONDITIONAL | Version drift records |
| recent-conflict-records | array | CONDITIONAL | Tool conflict records |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 22.2 Tool Invocation State Schema

| Field | Type | Required | Description |
|---|---|---|
| tool-invocation-state-id | string | REQUIRED | Unique invocation state identifier |
| invocation-id | string | REQUIRED | Tool invocation identifier |
| tool-id | string | REQUIRED | Tool invoked |
| task-id | string | REQUIRED | Related task |
| artifact-ids | array | CONDITIONAL | Artifacts evaluated or produced |
| current-state | enum | REQUIRED | requested, running, completed, failed, timeout, error, cancelled |
| invocation-result-id | string | CONDITIONAL | Result when completed |
| started-at | ISO 8601 | CONDITIONAL | Start time |
| completed-at | ISO 8601 | CONDITIONAL | Completion time |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 22.3 Tool State Rules

A revoked tool MUST NOT enter running state.

A tool invocation that times out MUST enter timeout state.

A completed tool invocation MUST reference an invocation result.

Tool version drift MUST update Tool State and emit an event.

---

## 23.0 Model State

This section defines Model State. Model State records model providers, model configurations, model invocations, routing decisions, context constraints, output normalization, and model-related failures.

Model State will be expanded by ISL v2.5, but this document defines the state and memory obligations required for continuity and audit.

### 23.1 Model Invocation State Schema

| Field | Type | Required | Description |
|---|---|---|
| model-invocation-state-id | string | REQUIRED | Unique model invocation state identifier |
| model-invocation-id | string | REQUIRED | Model invocation identifier |
| provider-id | string | REQUIRED | Model provider identifier |
| model-id | string | REQUIRED | Model identifier |
| agent-invocation-id | string | REQUIRED | Agent invocation using model |
| context-package-id | string | REQUIRED | Context used |
| current-state | enum | REQUIRED | requested, routed, running, completed, failed, timeout, cancelled |
| output-normalization-status | enum | REQUIRED | not-required, pending, passed, failed |
| failure-id | string | CONDITIONAL | Failure record if failed |
| started-at | ISO 8601 | CONDITIONAL | Start time |
| completed-at | ISO 8601 | CONDITIONAL | Completion time |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 23.2 Model State Rules

Model invocations MUST be linked to agent invocations.

Model State MUST preserve context-package-id without requiring raw context exposure to unauthorized roles.

A failed output normalization MUST prevent downstream use of the model output.

Model provider restrictions MUST be reflected in Governance State or configuration state.

---

## 24.0 Configuration State

This section defines Configuration State. Configuration State records platform, runtime, tool, model, governance, repository, observability, and deployment configuration.

Configuration changes can materially affect platform behavior. They must be versioned and auditable.

### 24.1 Configuration State Schema

| Field | Type | Required | Description |
|---|---|---|
| configuration-state-id | string | REQUIRED | Unique configuration state identifier |
| configuration-type | enum | REQUIRED | platform, runtime, tool, model, governance, repository, observability, deployment |
| configuration-id | string | REQUIRED | Configuration identifier |
| configuration-version | semver | REQUIRED | Configuration version |
| status | enum | REQUIRED | draft, active, superseded, revoked |
| effective-from | ISO 8601 | REQUIRED | Effective start time |
| effective-until | ISO 8601 | CONDITIONAL | Expiration time |
| configuration-reference | string | REQUIRED | Location or record reference |
| configuration-hash | string | CONDITIONAL | Integrity hash when supported |
| changed-by | string | REQUIRED | Role, subsystem, or administrator changing config |
| changed-at | ISO 8601 | REQUIRED | Time changed |

### 24.2 Configuration State Rules

Configuration changes affecting readiness, execution, governance, tools, models, artifact repositories, or deployment MUST be versioned.

Configuration changes during active execution MUST trigger an impact evaluation.

A revoked configuration MUST NOT authorize new activity.

---

## 25.0 Working Memory and Persistent Memory

This section distinguishes working memory from persistent memory. The distinction is important because agents and runtime processes need temporary context, while the platform also needs durable historical records.

Working memory supports immediate task execution. Persistent memory supports continuity, audit, recovery, and evolution.

### 25.1 Working Memory

Working memory MAY include:

* task-scoped context packages
* active agent prompt context
* temporary validation working files
* runtime scheduling queues
* in-progress tool outputs
* transient repair analysis context

Working memory MUST NOT be the only storage location for records required for traceability, audit, governance, recovery, or artifact reconstruction.

### 25.2 Persistent Memory

Persistent memory MUST include:

* specification versions
* canonical model versions
* readiness records
* construction plans
* execution graphs
* task state history
* artifact metadata and versions
* validation results
* repair records
* governance records
* traceability graphs and snapshots
* agent invocation and output records
* tool invocation and evidence records
* model invocation metadata
* configuration history
* audit logs

### 25.3 Memory Promotion

When working memory produces lifecycle-relevant output, the platform MUST promote that output into persistent memory before it affects downstream state.

For example:

* an agent output used for implementation MUST be persisted
* a validation result used for artifact acceptance MUST be persisted
* a governance decision used for continuation MUST be persisted
* a repair proposal used to modify an artifact MUST be persisted

---

## 26.0 State Transition Transaction Model

This section defines how state transitions are committed. State transitions often affect multiple state categories. For example, a validation failure may update Validation State, Artifact State, Task State, Repair State, Execution State, Traceability State, and telemetry.

The platform must prevent partial state updates from creating inconsistent conditions.

### 26.1 Transaction Requirements

A state transition affecting multiple state categories MUST be applied atomically or through a compensating transaction pattern that preserves consistency.

At minimum, the platform MUST ensure that:

* either all required state updates complete successfully, or
* incomplete updates are detectable and recoverable, and
* recovery can determine the last consistent state

### 26.2 State Transition Transaction Schema

| Field | Type | Required | Description |
|---|---|---|
| transaction-id | string | REQUIRED | Unique transition transaction identifier |
| transaction-type | string | REQUIRED | Type of transition transaction |
| affected-state-records | array | REQUIRED | State records updated |
| affected-event-records | array | REQUIRED | Events emitted |
| initiated-by | string | REQUIRED | Subsystem, task, user, or governance action initiating transition |
| started-at | ISO 8601 | REQUIRED | Start time |
| completed-at | ISO 8601 | CONDITIONAL | Completion time |
| status | enum | REQUIRED | pending, committed, failed, compensating, compensated |
| recovery-action | string | CONDITIONAL | Required when transaction fails |

### 26.3 Transaction Rules

A committed transaction MUST have corresponding state events.

A failed transaction MUST enter failed or compensating state.

Compensating actions MUST be recorded as events.

A platform MUST NOT continue execution from a failed transaction unless recovery has validated consistency.

---

## 27.0 Cross-State Consistency Rules

This section defines consistency invariants across state categories. These rules are mandatory and MUST be checked at defined lifecycle checkpoints and recovery points.

Cross-state consistency is essential because different subsystems update different state categories. The platform must detect impossible combinations.

### 27.1 Required Consistency Invariants

| Invariant                  | Rule                                                                     |
| -------------------------- | ------------------------------------------------------------------------ |
| Specification-to-Canonical | Machine-Valid readiness requires a passed canonical model state          |
| Readiness-to-Planning      | Formal executable planning requires Machine-Valid or higher readiness    |
| Readiness-to-Execution     | Execution requires Autonomous-Ready readiness                            |
| Planning-to-Execution      | Execution State MUST reference a valid executable plan                   |
| Task-to-Artifact           | Completed artifact-producing tasks MUST reference produced artifacts     |
| Artifact-to-Validation     | Valid artifacts MUST reference passed or waived validation               |
| Artifact-to-Traceability   | Stable artifacts MUST have complete traceability                         |
| Validation-to-Repair       | Failed validation triggering repair MUST reference Repair State          |
| Repair-to-Artifact         | Repaired artifact state MUST reference repair record                     |
| Governance-to-Action       | Governed action MUST reference allow, approval, waiver, or override      |
| Tool-to-Validation         | Tool-produced validation state MUST reference tool invocation result     |
| Agent-to-Output            | Completed agent invocation MUST reference validated output               |
| Model-to-Agent             | Model invocation MUST reference agent invocation                         |
| Deployment-to-Artifact     | Deployment preparation state MUST reference stable or approved artifacts |
| Completion-to-Evidence     | Completed execution MUST reference completion report and evidence        |

### 27.2 Consistency Checkpoints

The platform MUST perform consistency checks at:

* canonical validation completion
* readiness transition
* construction plan validation
* execution start
* task completion
* artifact state transition to valid or stable
* validation failure
* repair completion
* governance approval or waiver application
* artifact consolidation
* deployment preparation
* execution completion
* recovery after interruption
* specification version change

### 27.3 Consistency Failure Behavior

A consistency failure MUST produce a State Consistency Error Record.

A blocking consistency failure MUST prevent the lifecycle action associated with the checkpoint.

Recovery MUST be initiated when a consistency failure is detected after interruption or crash.

---

## 28.0 Snapshot and Checkpoint Model

This section defines snapshots and checkpoints. Snapshots capture state at important lifecycle moments. Checkpoints identify safe recovery positions during execution.

Snapshots and checkpoints support recovery, audit, comparison, rollback analysis, and debugging.

### 28.1 Snapshot Types

| Snapshot Type          | Description                                               |
| ---------------------- | --------------------------------------------------------- |
| specification-snapshot | Captures authored specification state                     |
| canonical-snapshot     | Captures canonical model state                            |
| planning-snapshot      | Captures construction plan and task graph state           |
| execution-snapshot     | Captures execution graph and runtime state                |
| artifact-snapshot      | Captures artifact metadata and status                     |
| traceability-snapshot  | Captures traceability graph state                         |
| governance-snapshot    | Captures governance state                                 |
| full-platform-snapshot | Captures coordinated state across all required categories |

### 28.2 Required Snapshot Triggers

The platform MUST create snapshots at:

* Machine-Valid readiness
* Autonomous-Ready authorization
* execution start
* before artifact consolidation
* after artifact consolidation
* before deployment authorization
* execution completion
* before major plan adaptation
* after specification version change
* before destructive or superseding operations
* governance-required audit points

### 28.3 Snapshot Record Schema

| Field | Type | Required | Description |
|---|---|---|
| snapshot-id | string | REQUIRED | Unique snapshot identifier |
| snapshot-type | enum | REQUIRED | Snapshot type from §28.1 |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| included-state-categories | array | REQUIRED | State categories included |
| created-at | ISO 8601 | REQUIRED | Snapshot time |
| created-by | string | REQUIRED | Subsystem creating snapshot |
| storage-location | string | REQUIRED | Snapshot location |
| integrity-hash | string | CONDITIONAL | Integrity marker when supported |
| recovery-eligible | boolean | REQUIRED | Whether snapshot can be used for recovery |
| retention-class | enum | REQUIRED | transient, operational, audit, archival |

### 28.4 Checkpoint Rules

Execution checkpoints MUST be created before and after high-impact operations including:

* task graph adaptation
* artifact consolidation
* deployment preparation
* high-risk repair
* governance override application
* repository branch merge or promotion
* execution halt or suspension

A recovery-eligible checkpoint MUST include enough information to resume or safely roll back affected execution state.

---

## 29.0 Recovery and Resumption

This section defines recovery behavior after interruption, crash, process restart, infrastructure failure, governance halt, or partial transaction failure. Recovery is one of the primary reasons state and memory must be structured and durable.

Recovery must not guess. It must reconstruct and validate platform state before continuing.

### 29.1 Recovery Preconditions

The platform MAY attempt recovery only if:

* durable state records are accessible
* event logs are accessible
* latest relevant snapshots are accessible or reconstructible
* governance state is accessible
* traceability state is accessible
* artifact repository state is accessible
* configuration state is known
* recovery actor or subsystem has authority to perform recovery

### 29.2 Recovery Process

Recovery MUST proceed through the following steps:

1. Enter recovering state for affected execution or subsystem.
2. Load latest durable state records.
3. Load latest relevant snapshots.
4. Replay or inspect state events after the snapshot.
5. Validate state transition transactions.
6. Check cross-state consistency invariants.
7. Compare artifact repository state with artifact metadata.
8. Compare traceability graph with artifact and task state.
9. Compare governance state with pending or blocked actions.
10. Determine safe resume point, rollback action, or escalation.
11. Record Recovery Event.
12. Resume, halt, or escalate according to recovery outcome.

### 29.3 Recovery Outcome Values

| Outcome                | Meaning                                      |
| ---------------------- | -------------------------------------------- |
| resume-safe            | State is consistent and execution may resume |
| rollback-required      | State must return to prior checkpoint        |
| compensation-required  | Compensating state updates are required      |
| manual-review-required | Human or governance review required          |
| halt-required          | Execution cannot continue safely             |
| state-corrupt          | State is inconsistent or unrecoverable       |

### 29.4 Recovery Event Schema

| Field | Type | Required | Description |
|---|---|---|
| recovery-event-id | string | REQUIRED | Unique recovery event identifier |
| recovery-scope | enum | REQUIRED | task, execution, plan, artifact, governance, traceability, full-platform |
| affected-object-ids | array | REQUIRED | Objects affected by recovery |
| starting-snapshot-id | string | CONDITIONAL | Snapshot used for recovery |
| events-replayed | array | CONDITIONAL | Events replayed or inspected |
| consistency-status | enum | REQUIRED | consistent, inconsistent, partially-consistent, corrupt |
| recovery-outcome | enum | REQUIRED | Outcome from §29.3 |
| actions-taken | array | CONDITIONAL | Recovery or compensation actions |
| required-next-action | string | CONDITIONAL | Required follow-up |
| recovered-at | ISO 8601 | REQUIRED | Time recovery completed |
| recovered-by | string | REQUIRED | Component or authority performing recovery |

### 29.5 Recovery Rules

The platform MUST NOT resume execution unless recovery-outcome is resume-safe.

If recovery-outcome is rollback-required, rollback MUST use a recovery-eligible checkpoint.

If recovery-outcome is compensation-required, compensating actions MUST be recorded as state events.

If recovery-outcome is manual-review-required, the platform MUST open an escalation.

If state is corrupt, the platform MUST halt affected execution and notify governance or operations.

---

## 30.0 Memory Access for Agents and Subsystems

This section defines how agents and subsystems access platform memory. Memory access must be controlled, contextual, and traceable.

Agents need memory to reason effectively, but unrestricted memory access would create privacy, security, governance, and reliability risks.

### 30.1 Memory Access Request Schema

| Field | Type | Required | Description |
|---|---|---|
| memory-access-request-id | string | REQUIRED | Unique memory access request identifier |
| requester-id | string | REQUIRED | Agent, subsystem, user, or service requesting memory |
| requester-type | enum | REQUIRED | agent, subsystem, user, governance-role, tool |
| requester-role | string | CONDITIONAL | Agent role or governance role |
| task-id | string | CONDITIONAL | Related task |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| requested-state-categories | array | REQUIRED | Categories requested |
| requested-object-ids | array | CONDITIONAL | Specific objects requested |
| access-purpose | string | REQUIRED | Purpose for access |
| sensitivity-maximum | enum | REQUIRED | Maximum allowed sensitivity |
| requested-at | ISO 8601 | REQUIRED | Request time |

### 30.2 Memory Access Response Schema

| Field | Type | Required | Description |
|---|---|---|
| memory-access-response-id | string | REQUIRED | Unique response identifier |
| request-id | string | REQUIRED | Matching request |
| decision | enum | REQUIRED | allow, deny, partial, approval-required, redact |
| provided-records | array | CONDITIONAL | Records or references provided |
| redacted-records | array | CONDITIONAL | Records redacted |
| denied-records | array | CONDITIONAL | Records denied |
| governance-check-id | string | CONDITIONAL | Governance check if applicable |
| rationale | string | REQUIRED | Explanation of decision |
| decided-at | ISO 8601 | REQUIRED | Decision time |

### 30.3 Agent Memory Access Rules

Agents MUST access memory through context packages or approved memory access interfaces.

Agents MUST NOT query unrestricted platform memory directly.

Memory provided to agents MUST be scoped to task, role, governance constraints, and sensitivity classification.

Memory access MUST be recorded when it includes restricted, confidential, governance, security, or artifact content.

---

## 31.0 Memory Retention and Archival

This section defines retention and archival requirements. State and memory records may accumulate over time, but enterprise auditability requires that important records remain available according to retention policy.

Retention rules must balance storage cost, operational performance, auditability, privacy, and governance obligations.

### 31.1 Minimum Retention Requirements

The platform MUST retain the following for the life of any generated system unless governance defines longer retention:

* specification versions that reached Machine-Valid or Autonomous-Ready
* canonical model versions used for planning or execution
* readiness transition records
* construction plans used for execution
* execution graphs and completion reports
* artifact metadata and artifact version history
* validation results supporting artifact acceptance
* repair records
* traceability snapshots
* governance approvals, waivers, overrides, escalations, and audit logs
* deployment authorization records
* configuration versions active during execution

### 31.2 Archival Rules

Archived records MUST remain discoverable by identifier.

Archival MUST preserve traceability and audit relationships.

Archived records MUST NOT be required for active execution unless restored or rehydrated.

Archival MUST NOT break readiness, governance, traceability, or artifact audit queries for retained systems.

### 31.3 Deletion Rules

Deletion of state or memory records MUST be governed by retention policy.

Records required for audit, traceability, legal compliance, or active artifact reconstruction MUST NOT be deleted.

Deletion events MUST themselves be auditable when deletion is permitted.

---

## 32.0 State and Memory Integrity Checks

This section defines integrity checks required to verify that state and memory remain reliable. Integrity checks detect broken references, missing events, invalid transitions, inconsistent statuses, corrupted snapshots, and mismatched repository state.

### 32.1 Required Integrity Checks

A conforming platform MUST check:

* state record schema validity
* event sequence continuity
* allowed state transitions
* cross-state consistency invariants
* snapshot integrity
* traceability link presence
* governance audit record presence
* artifact metadata and repository consistency
* validation-to-artifact consistency
* repair-to-validation consistency
* configuration version consistency
* active execution recoverability

### 32.2 Integrity Check Record Schema

| Field | Type | Required | Description |
|---|---|---|
| state-integrity-check-id | string | REQUIRED | Unique integrity check identifier |
| check-scope | enum | REQUIRED | specification, planning, execution, artifact, governance, traceability, tool, agent, model, full-platform |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| outcome | enum | REQUIRED | passed, failed, warning |
| findings | array | CONDITIONAL | Integrity findings |
| checked-at | ISO 8601 | REQUIRED | Time checked |
| checked-by | string | REQUIRED | Component performing check |

### 32.3 Integrity Failure Rules

A failed integrity check affecting active execution MUST pause or halt affected execution until recovery or governance resolution.

A failed integrity check affecting governance audit records MUST block governance-controlled actions.

A failed integrity check affecting stable artifacts MUST prevent deployment authorization.

---

## 33.0 State and Memory Error Classes

This section defines error classes specific to state and memory. These errors allow the platform to report state and memory failures consistently.

### 33.1 Error Record Schema

| Field | Type | Required | Description |
|---|---|---|
| error-id | string | REQUIRED | Unique error identifier |
| error-class | enum | REQUIRED | Error class |
| severity | enum | REQUIRED | blocking, high, medium, low |
| state-category | enum | CONDITIONAL | State category affected |
| object-id | string | CONDITIONAL | Object affected |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| message | string | REQUIRED | Human-readable explanation |
| required-action | string | REQUIRED | Required remediation |
| detected-at | ISO 8601 | REQUIRED | Detection time |

### 33.2 Error Classes

| Error Class                     | Description                                           | Default Handling                    |
| ------------------------------- | ----------------------------------------------------- | ----------------------------------- |
| state-record-missing            | Required state record is missing                      | halt or recover                     |
| state-record-invalid            | State record fails schema validation                  | halt affected action                |
| state-transition-invalid        | Attempted transition is not permitted                 | reject transition                   |
| state-event-missing             | Required state event was not recorded                 | recover or escalate                 |
| state-event-sequence-gap        | Event sequence has gap or inconsistency               | recover or escalate                 |
| state-transaction-failed        | Multi-state transition transaction failed             | recover or compensate               |
| state-consistency-failed        | Cross-state invariant failed                          | pause or halt                       |
| state-snapshot-missing          | Required snapshot is missing                          | warning, escalate, or halt          |
| state-snapshot-corrupt          | Snapshot integrity check failed                       | recover from prior snapshot or halt |
| state-recovery-failed           | Recovery could not produce safe state                 | halt                                |
| memory-access-denied            | Memory access request denied                          | block request                       |
| memory-access-violation         | Unauthorized memory access attempted                  | block and escalate                  |
| memory-retention-violation      | Required memory record missing or deleted early       | escalate                            |
| artifact-state-mismatch         | Artifact metadata differs from repository state       | recover or halt                     |
| governance-state-unavailable    | Governance state unavailable for governed action      | halt governed action                |
| traceability-state-inconsistent | Traceability state conflicts with artifact/task state | pause or halt                       |

---

## 34.0 Observability of State and Memory

This section defines observability requirements for state and memory. Operators must be able to see platform state transitions, consistency failures, recovery attempts, memory access, and snapshot behavior.

Observability does not replace durable state or audit records. It provides operational visibility.

### 34.1 Required Telemetry Events

The platform SHOULD emit telemetry for:

* state record created
* state transition attempted
* state transition committed
* state transition rejected
* state transaction failed
* snapshot created
* checkpoint created
* recovery started
* recovery completed
* consistency check passed
* consistency check failed
* memory access requested
* memory access denied
* archival completed
* retention violation detected

### 34.2 Telemetry Rules

Telemetry events SHOULD include correlation identifiers linking them to state records, events, tasks, executions, artifacts, governance actions, or recovery actions.

Telemetry MUST NOT expose restricted memory content to unauthorized observers.

Telemetry SHOULD align with the observability conventions defined in ISL v2.6.

---

## 35.0 Conformance Requirements

This section defines conformance requirements for state records, memory stores, state managers, recovery mechanisms, and platforms.

### 35.1 State Record Conformance

A state record conforms to ISL v2.2 if it:

* includes required base fields
* uses a defined state category
* identifies the object represented
* preserves specification version context where applicable
* uses valid current-state values for its category
* preserves update timestamp and actor
* supports correlation with events or traceability where required

### 35.2 Memory Store Conformance

A memory store conforms to ISL v2.2 if it can:

* persist required state and memory records
* preserve historical records
* support versioned records
* support event log storage
* support snapshots or references to snapshots
* support identifier-based retrieval
* support lifecycle audit queries
* enforce access control
* preserve integrity for governance, traceability, and audit records

### 35.3 State Manager Conformance

A state manager conforms to ISL v2.2 if it can:

* apply allowed state transitions
* reject invalid state transitions
* emit state events
* manage state transition transactions
* enforce cross-state consistency invariants
* create checkpoints and snapshots
* perform integrity checks
* initiate recovery when required
* preserve state history
* expose state query interfaces

### 35.4 Recovery Mechanism Conformance

A recovery mechanism conforms to ISL v2.2 if it can:

* enter recovering state
* load durable records and snapshots
* replay or inspect state events
* validate transition transactions
* check cross-state consistency
* compare repository and metadata state
* determine safe resume, rollback, compensation, escalation, or halt outcomes
* record recovery events
* prevent unsafe resumption

### 35.5 Platform Conformance

A platform conforms to ISL v2.2 if it:

* maintains all required state categories
* distinguishes working memory from persistent memory
* persists lifecycle-critical memory
* enforces state transition rules
* enforces cross-state consistency
* supports recovery after interruption
* controls agent and subsystem memory access
* preserves governance, traceability, validation, and artifact memory
* supports snapshots and checkpoints
* supports retention and archival rules
* exposes required state and memory audit queries

---

## 36.0 Summary

ISL v2.2 defines the State and Memory Model for the IMHOTEP platform. It establishes how operational state and durable memory are represented, persisted, versioned, transitioned, validated, recovered, queried, retained, and governed.

This revision strengthens the original model by turning state and memory from broad conceptual categories into explicit platform contracts. It defines state categories, schemas, transition rules, transaction behavior, consistency invariants, snapshots, recovery procedures, memory access controls, retention obligations, integrity checks, error classes, and conformance requirements.

A conforming IMHOTEP platform MUST treat state and memory as foundational infrastructure. Autonomous construction cannot be reliable if the platform cannot remember what happened, know what is happening now, recover after interruption, and prove that its current state is consistent with its specification, artifacts, validations, governance, and traceability records.
