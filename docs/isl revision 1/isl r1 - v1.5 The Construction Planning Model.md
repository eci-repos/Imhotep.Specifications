# IMHOTEP Specification Language (ISL) v1.5

# The Construction Planning Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4
**Supersedes:** ISL r1 v1.5 Construction Planning Model
**Document Type:** Planning and Task Graph Specification

---

## 1.0 Scope

This document defines the Construction Planning Model used by the IMHOTEP platform to transform a validated canonical ISL specification into an executable construction task graph. The construction task graph identifies the work required to generate, validate, test, repair, consolidate, and prepare artifacts derived from the specification.

This document applies after a specification has been normalized into the canonical semantic model and has reached the readiness state required for planning. It defines how planning inputs are evaluated, how tasks are generated, how dependencies are resolved, how verification and repair activities are represented, how parallel execution opportunities are identified, and how the plan adapts when the specification changes.

This document defines:

* planning preconditions
* planning inputs and outputs
* construction task schema
* task classification
* task generation rules
* dependency resolution rules
* construction task graph structure
* verification task integration
* repair task insertion
* parallel planning
* critical path calculation
* plan validation
* plan adaptation
* monitoring requirements
* completion criteria
* conformance requirements

---

## 2.0 Purpose of the Construction Planning Model

The purpose of the Construction Planning Model is to convert the canonical specification graph into a structured plan that the execution runtime can schedule and execute. A valid specification describes what the system must be. A construction plan defines the work required to produce it.

Autonomous construction cannot proceed directly from semantic entities to artifact generation. The platform must first determine which tasks are necessary, which tasks depend on one another, which agent roles and tool capabilities are required, what artifacts are expected, and where verification must occur. This planning step transforms system intent into an executable workflow.

The Construction Planning Model exists to ensure that:

* every required construction activity traces to specification intent
* construction work is decomposed into explicit tasks
* task dependencies are resolved before execution
* verification is integrated into the plan rather than added afterward
* repair tasks are inserted under controlled rules
* parallel execution is identified safely
* affected work can be replanned when the specification changes
* the execution runtime receives a complete and valid task graph

---

## 3.0 Normative References

This section identifies the documents required to interpret and implement the Construction Planning Model. Planning depends on the canonical semantic model, readiness rules, traceability graph, execution phases, and governance constraints.

| Reference | Purpose                                                                     |
| --------- | --------------------------------------------------------------------------- |
| ISL v0.0  | Corpus-level principles, conformance, and system integrity rules            |
| ISL v1.1  | Canonical semantic model and entity relationships                           |
| ISL v1.2  | Execution phases, runtime behavior, artifact status, and repair termination |
| ISL v1.3  | Readiness levels and planning permissions                                   |
| ISL v1.4  | Traceability requirements and impact analysis                               |
| ISL v1.6  | Tool integration model and deterministic validation capabilities            |
| ISL v1.7  | Governance policies, approvals, waivers, and escalation controls            |
| RFC 2119  | Normative terminology                                                       |

---

## 4.0 Terms and Definitions

This section defines terms specific to construction planning. These terms are used by the Planning Engine, Execution Runtime, Traceability Engine, Governance Engine, and Tool Integration Layer.

**Construction Plan**
A structured plan describing the tasks required to construct a system from an ISL specification.

**Construction Task**
The atomic unit of planned work within a construction plan.

**Construction Task Graph**
A directed acyclic graph of construction tasks and dependencies.

**Planning Engine**
The platform component responsible for generating, validating, adapting, and maintaining the construction plan.

**Task Dependency**
A required ordering relationship between tasks.

**Verification Task**
A task that validates an artifact or task output using deterministic tools, review, or defined validation criteria.

**Repair Task**
A dynamically generated task created in response to failed validation or testing.

**Critical Path**
The longest dependency chain in the task graph, representing the minimum completion path assuming unlimited parallel capacity.

**Parallel Group**
A set of tasks that may execute concurrently because they have no unresolved dependencies between them.

**Plan Adaptation**
Modification of the construction plan in response to specification changes, impact analysis, failed execution, or governance action.

---

## 5.0 Planning Design Principles

This section defines the principles that govern construction planning. These principles ensure the plan is not an informal work breakdown but a controlled, traceable, executable graph.

### 5.1 Specification-Derived Planning

Every construction task MUST be derived from one or more canonical specification entities, platform-required lifecycle obligations, or governance-required controls.

The Planning Engine MUST NOT create arbitrary construction work without traceability to specification intent or a documented platform rule.

### 5.2 Explicit Task Decomposition

Construction work MUST be decomposed into explicit tasks. Each task MUST have a type, source entity, assigned role, dependencies, expected outputs, and status.

A plan that contains vague work items such as “build system,” “implement backend,” or “add security” MUST NOT be considered valid unless those items are decomposed into executable tasks.

### 5.3 Dependency Safety

The Planning Engine MUST resolve task dependencies before execution begins. A task MUST NOT be scheduled before the tasks it depends on have completed or been waived under governance rules.

Dependency safety protects build correctness, state consistency, traceability, and validation reliability.

### 5.4 Verification by Design

Verification MUST be planned as part of construction. It MUST NOT be treated as an optional post-processing step.

Every task that produces executable, structural, contractual, infrastructural, or policy-relevant artifacts MUST have associated verification tasks.

### 5.5 Controlled Adaptability

The construction plan MUST support adaptation when the specification changes or when execution introduces repair tasks. Adaptation MUST be controlled by traceability, impact analysis, and governance rules.

The platform MUST NOT restart the entire plan when only affected tasks require re-execution.

### 5.6 Execution Runtime Compatibility

The construction task graph MUST be executable by the runtime defined in ISL v1.2. Planning output MUST be structured, deterministic, and complete enough to support scheduling, execution, validation, repair, and monitoring.

---

## 6.0 Planning Preconditions

This section defines the conditions required before the Planning Engine may generate or update a construction plan. Planning is more permissive than execution, but formal executable planning still requires a sufficiently valid specification.

The platform may provide advisory planning for immature specifications, but executable construction planning requires stronger preconditions.

### 6.1 Formal Planning Preconditions

The Planning Engine MUST NOT generate an executable construction plan unless all of the following are true.

| Precondition                                                 | Required Evidence           | Enforced By             |
| ------------------------------------------------------------ | --------------------------- | ----------------------- |
| Specification has reached Machine-Valid or higher readiness  | Readiness record            | ISL v1.3                |
| Canonical normalization completed without blocking errors    | Canonical validation report | ISL v1.1                |
| Canonical graph contains required entities and relationships | Canonical model             | ISL v1.1                |
| Traceability graph is initialized                            | Traceability state          | ISL v1.4                |
| Planning governance constraints are available                | Governance state            | ISL v1.7                |
| Required construction profiles are known                     | Planning configuration      | Platform implementation |
| Tool capability needs can be identified                      | Tool capability model       | ISL v1.6                |

### 6.2 Advisory Planning

For Draft or Reviewable specifications, the Planning Engine MAY produce advisory planning output. Advisory output MUST be clearly marked non-executable.

Advisory planning MAY identify:

* missing specification sections
* likely task categories
* missing validation coverage
* dependency risks
* tool capability needs
* governance issues
* readiness blockers

Advisory planning MUST NOT be used by the execution runtime.

### 6.3 Precondition Failure

If formal planning preconditions fail, the Planning Engine MUST produce a Planning Precondition Failure Record.

| Field                 | Type     | Required    | Description                 |
| --------------------- | -------- | ----------- | --------------------------- |
| failure-id            | string   | REQUIRED    | Unique failure identifier   |
| specification-id      | string   | REQUIRED    | System identifier           |
| specification-version | semver   | REQUIRED    | Specification version       |
| failed-precondition   | string   | REQUIRED    | Precondition that failed    |
| severity              | enum     | REQUIRED    | blocking, high, medium, low |
| evidence              | string   | CONDITIONAL | Missing or failed evidence  |
| required-action       | string   | REQUIRED    | Remediation required        |
| recorded-at           | ISO 8601 | REQUIRED    | Time failure was recorded   |

---

## 7.0 Planning Inputs

This section defines the structured inputs required by the Planning Engine. A valid construction plan cannot be generated from prose alone. It must be generated from canonical entities, relationships, readiness state, governance constraints, and tool capability information.

### 7.1 Required Inputs

| Input                   | Required    | Description                                                   |
| ----------------------- | ----------- | ------------------------------------------------------------- |
| Canonical Model         | YES         | ISL v1.1 canonical specification graph                        |
| Specification Version   | YES         | Version being planned                                         |
| Readiness Record        | YES         | Evidence that planning is permitted                           |
| Traceability State      | YES         | Existing traceability graph or initialized traceability state |
| Governance State        | YES         | Policies, risk tier, waivers, planning constraints            |
| Tool Capability Catalog | CONDITIONAL | Required for verification task planning                       |
| Planning Configuration  | YES         | Planning rules, task granularity, supported artifact types    |
| Prior Construction Plan | CONDITIONAL | Required for adaptation or replanning                         |
| Impact Analysis Result  | CONDITIONAL | Required when planning follows specification change           |

### 7.2 Input Consistency Rules

All planning inputs MUST reference the same specification identifier and specification version unless the Planning Engine is performing version comparison or impact-based adaptation.

If planning uses a prior construction plan, the Planning Engine MUST verify whether the prior plan was derived from the immediately preceding specification version or another explicitly declared baseline.

A planning input version mismatch MUST produce a blocking planning validation error.

---

## 8.0 Planning Outputs

This section defines the required outputs of construction planning. The principal output is the Construction Task Graph, but a complete planning operation also produces validation records, traceability updates, critical path information, and execution readiness evidence.

### 8.1 Required Outputs

| Output                      | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| Construction Task Graph     | Executable directed graph of construction tasks         |
| Planning Record             | Summary of planning operation                           |
| Task Definitions            | Structured records for every construction task          |
| Dependency Records          | Explicit dependency edges between tasks                 |
| Critical Path               | Longest dependency chain                                |
| Parallel Groups             | Sets of tasks eligible for concurrent execution         |
| Verification Plan           | Verification tasks and tool capability requirements     |
| Artifact Production Plan    | Expected artifact outputs and traceability associations |
| Planning Validation Report  | Result of validating the construction plan              |
| Traceability Updates        | Links from entities to tasks and planning decisions     |
| Execution Readiness Summary | Whether the plan is eligible for runtime execution      |

### 8.2 Planning Record Schema

| Field                   | Type     | Required    | Description                            |
| ----------------------- | -------- | ----------- | -------------------------------------- |
| planning-record-id      | string   | REQUIRED    | Unique planning operation identifier   |
| plan-id                 | string   | REQUIRED    | Construction plan identifier           |
| specification-id        | string   | REQUIRED    | System identifier                      |
| specification-version   | semver   | REQUIRED    | Specification version planned          |
| canonical-model-version | semver   | REQUIRED    | Canonical model version                |
| planning-mode           | enum     | REQUIRED    | advisory, formal, adaptation           |
| generated-at            | ISO 8601 | REQUIRED    | Time plan was generated                |
| generated-by            | string   | REQUIRED    | Planning Engine version or component   |
| outcome                 | enum     | REQUIRED    | generated, failed, partially-generated |
| validation-report-id    | string   | CONDITIONAL | Planning validation report reference   |

---

## 9.0 Construction Task Definition

This section defines the construction task as the atomic unit of planned work. Every executable construction activity must be represented as a task. Tasks provide the runtime with enough information to assign work, schedule dependencies, invoke agents or tools, verify outputs, and monitor progress.

### 9.1 Base Task Schema

Every construction task MUST conform to the following schema.

| Field                      | Type     | Required    | Description                                                 |
| -------------------------- | -------- | ----------- | ----------------------------------------------------------- |
| task-id                    | string   | REQUIRED    | Unique task identifier within construction plan             |
| task-type                  | enum     | REQUIRED    | Task type defined in §10.0                                  |
| source-entity-ids          | array    | REQUIRED    | Specification entities or platform rules originating task   |
| description                | string   | REQUIRED    | Human-readable description of work                          |
| assigned-agent-role        | string   | CONDITIONAL | Required when task requires reasoning or generation         |
| required-tool-capabilities | array    | CONDITIONAL | Tool capabilities required by task                          |
| dependencies               | array    | REQUIRED    | Task identifiers that MUST complete before task begins      |
| artifacts-produced         | array    | CONDITIONAL | Artifact types expected from task                           |
| artifacts-consumed         | array    | CONDITIONAL | Artifact identifiers or artifact types consumed             |
| verification-tasks         | array    | CONDITIONAL | Associated verification task identifiers                    |
| governance-constraints     | array    | CONDITIONAL | Policy or approval constraints applying to task             |
| priority                   | enum     | REQUIRED    | critical, high, medium, low                                 |
| status                     | enum     | REQUIRED    | pending, in-progress, completed, failed, escalated, skipped |
| created-at                 | ISO 8601 | REQUIRED    | Timestamp of task creation                                  |
| updated-at                 | ISO 8601 | CONDITIONAL | Timestamp of last task update                               |

### 9.2 Task Identifier Format

Task identifiers MUST use the traceability object identifier format defined in ISL v1.4.

Recommended format:

`TSK-{system-id}-{sequence}`

Example:

`TSK-acme-order-00001`

### 9.3 Task Description Rules

A task description MUST describe a single unit of work. It MUST be specific enough for an execution runtime, agent, tool, or human reviewer to understand expected activity.

Invalid task descriptions include:

* Build the app.
* Add backend.
* Do validation.
* Make it secure.

Valid task descriptions include:

* Generate the Order data model schema from DAT-acme-order-00001.
* Generate REST interface contract for INT-acme-order-00001.
* Validate generated Order API contract using OpenAPI validation.

### 9.4 Source Entity Rules

Every task MUST have at least one source-entity-id unless the task is a platform-required lifecycle task.

Platform-required lifecycle tasks MUST use a source reference to the rule or phase that caused the task to exist.

Examples of platform-required lifecycle tasks include:

* artifact consolidation
* deployment preparation
* deterministic validation
* traceability snapshot creation

---

## 10.0 Task Classification

This section defines the allowed task types. Task type determines agent role assignment, expected outputs, validation behavior, and runtime treatment.

A conforming Planning Engine MUST use the task types defined here unless an approved extension defines additional task types.

### 10.1 Task Types

| Task Type              | Description                                                         | Primary Agent Role        |
| ---------------------- | ------------------------------------------------------------------- | ------------------------- |
| interpretation         | Produces execution-oriented interpretation of specification content | Specification Interpreter |
| architecture           | Establishes structural elements and design decisions                | Architecture Planner      |
| planning               | Refines or adapts construction plan                                 | Construction Planner      |
| implementation         | Generates executable source code artifacts                          | Implementation Generator  |
| schema                 | Generates data model and schema definitions                         | Implementation Generator  |
| interface              | Generates interface contracts and API definitions                   | Implementation Generator  |
| infrastructure         | Generates deployment and environment definitions                    | Deployment Preparer       |
| test                   | Generates automated or semi-automated test artifacts                | Test Generator            |
| verification           | Validates an artifact using deterministic tools or review criteria  | Review Agent              |
| security               | Evaluates artifacts against security and policy constraints         | Security Validator        |
| repair                 | Corrects failed artifacts based on validation results               | Repair Analyst            |
| documentation          | Generates system, component, or operational documentation           | Implementation Generator  |
| consolidation          | Organizes validated artifacts into stable project structure         | Execution Runtime         |
| deployment-preparation | Produces deployment-ready outputs and readiness evidence            | Deployment Preparer       |
| traceability           | Creates snapshots or verifies traceability integrity                | Traceability Engine       |
| governance             | Performs governance checkpoint or approval-dependent action         | Governance Engine         |

### 10.2 Task Type Rules

A task MUST use exactly one primary task type.

A task MAY include secondary classifications in metadata, but runtime scheduling MUST use the primary task type.

A task type MUST be compatible with its assigned-agent-role and required-tool-capabilities.

A repair task MUST NOT be generated during initial planning unless it represents a planned repair review for a known existing artifact. Standard repair tasks are generated dynamically during execution.

---

## 11.0 Agent Role Mapping

This section defines the default mapping between task types and agent roles. Agent role mapping ensures that tasks are assigned to the correct reasoning function or runtime component.

Detailed agent contracts are defined later in the ISL corpus, but the Planning Engine must assign logical responsibility during task generation.

### 11.1 Default Mapping

| Task Type              | Default Responsible Role or Component |
| ---------------------- | ------------------------------------- |
| interpretation         | Specification Interpreter             |
| architecture           | Architecture Planner                  |
| planning               | Construction Planner                  |
| implementation         | Implementation Generator              |
| schema                 | Implementation Generator              |
| interface              | Implementation Generator              |
| infrastructure         | Deployment Preparer                   |
| test                   | Test Generator                        |
| verification           | Review Agent                          |
| security               | Security Validator                    |
| repair                 | Repair Analyst                        |
| documentation          | Implementation Generator              |
| consolidation          | Execution Runtime                     |
| deployment-preparation | Deployment Preparer                   |
| traceability           | Traceability Engine                   |
| governance             | Governance Engine                     |

### 11.2 Mapping Rules

A task requiring reasoning MUST include an assigned-agent-role.

A task performed entirely by deterministic tools MAY omit assigned-agent-role if required-tool-capabilities are declared.

A task performed by a platform subsystem MUST identify the responsible component.

A governance-constrained task MUST reference the applicable policy or governance checkpoint.

---

## 12.0 Task Generation Rules

This section defines how tasks are generated from canonical entities. Task generation must be deterministic enough that a conforming Planning Engine can produce repeatable plans for the same specification, configuration, and ISL version.

Task generation creates the first explicit connection between specification intent and construction work.

### 12.1 General Task Generation Rule

For each canonical entity that requires construction, validation, policy enforcement, documentation, or deployment preparation, the Planning Engine MUST generate one or more tasks.

The Planning Engine MUST NOT generate tasks for deprecated or superseded entities unless required for migration, cleanup, traceability, or governance review.

### 12.2 Entity-to-Task Mapping

| Canonical Entity | Required Task Categories                                                       |
| ---------------- | ------------------------------------------------------------------------------ |
| Project          | interpretation, planning, documentation, consolidation, deployment-preparation |
| Context          | architecture, verification, documentation                                      |
| Stakeholder      | governance, documentation                                                      |
| Actor            | interface, security, test                                                      |
| Requirement      | implementation, test, verification                                             |
| Capability       | architecture, implementation, test                                             |
| Service          | architecture, implementation, interface, verification, documentation           |
| DataEntity       | schema, implementation, verification, security                                 |
| Workflow         | implementation, test, verification, documentation                              |
| Interface        | interface, implementation, verification, security                              |
| Policy           | security, verification, governance                                             |
| Infrastructure   | infrastructure, verification, deployment-preparation                           |
| Validation       | test, verification                                                             |

### 12.3 Required Generation Rules

A must-have Requirement MUST produce or be linked to at least one implementation or verification path.

A DataEntity MUST produce schema-related tasks unless explicitly marked transient and non-structural.

A Service MUST produce implementation and verification tasks.

An Interface MUST produce contract generation or contract validation tasks.

A Policy MUST produce policy evaluation or security validation tasks.

An Infrastructure entity MUST produce infrastructure validation or deployment preparation tasks.

A Validation entity MUST produce verification or test tasks consistent with its validation type.

### 12.4 Task Granularity

The Planning Engine SHOULD generate tasks at a granularity that supports:

* independent validation
* targeted repair
* traceable artifact production
* parallel execution
* impact-based replanning

Tasks that are too broad reduce repair precision and traceability. Tasks that are too small may increase scheduling overhead. Planning configuration MAY control task granularity within the constraints of this model.

---

## 13.0 Dependency Resolution

This section defines how task dependencies are identified and enforced. Dependency resolution determines the order in which tasks may safely execute.

A valid construction plan MUST resolve all dependencies before execution begins.

### 13.1 Required Dependency Rules

The Planning Engine MUST enforce the following dependency rules.

| Rule                                        | Description                                                                                            |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Interpretation before planning              | Interpretation tasks MUST complete before plan confirmation                                            |
| Architecture before implementation          | Architecture tasks MUST complete before implementation tasks dependent on structural decisions         |
| Schema before implementation                | Schema tasks MUST complete before implementation tasks that consume schema artifacts                   |
| Interface before implementation             | Interface tasks MUST complete before implementations exposing those interfaces                         |
| Policy before security validation           | Policy interpretation tasks MUST complete before policy validation tasks                               |
| Implementation before verification          | Verification tasks MUST run after the task producing the artifact                                      |
| Implementation before test                  | Test execution MUST occur after relevant implementation artifacts exist                                |
| Verification before repair                  | Repair tasks MUST be triggered by failed verification or test outcomes                                 |
| Repair before revalidation                  | Repaired artifacts MUST be validated again before acceptance                                           |
| All valid artifacts before consolidation    | Consolidation MUST NOT begin until required artifacts are valid, waived, or escalated under governance |
| Consolidation before deployment preparation | Deployment preparation MUST depend on consolidated project state                                       |

### 13.2 Dependency Types

| Dependency Type | Meaning                                                                  |
| --------------- | ------------------------------------------------------------------------ |
| hard            | Target task MUST complete successfully before source task begins         |
| soft            | Target task SHOULD complete first but MAY be bypassed with justification |
| governance      | Target approval or policy decision MUST exist before source task begins  |
| artifact        | Source task requires artifact produced by target task                    |
| validation      | Source task requires validation outcome from target task                 |
| repair          | Source task is generated in response to failed target task               |
| traceability    | Source task requires traceability record or snapshot from target task    |

### 13.3 Dependency Record Schema

| Field           | Type     | Required | Description                                                        |
| --------------- | -------- | -------- | ------------------------------------------------------------------ |
| dependency-id   | string   | REQUIRED | Unique dependency identifier                                       |
| source-task-id  | string   | REQUIRED | Task that depends on another task                                  |
| target-task-id  | string   | REQUIRED | Task that must occur first                                         |
| dependency-type | enum     | REQUIRED | hard, soft, governance, artifact, validation, repair, traceability |
| rationale       | string   | REQUIRED | Reason dependency exists                                           |
| required        | boolean  | REQUIRED | Whether dependency blocks execution                                |
| created-at      | ISO 8601 | REQUIRED | Time dependency was created                                        |

### 13.4 Circular Dependency Detection

The Planning Engine MUST detect circular dependencies.

A circular dependency MUST cause planning validation failure unless the cycle is explicitly permitted by an approved extension and does not affect execution ordering.

Circular dependency errors MUST identify all tasks participating in the cycle.

### 13.5 Dependency Failure

If a required dependency cannot be resolved, the affected task MUST be marked blocked in the Planning Validation Report.

A construction plan with unresolved required dependencies MUST NOT be executable.

---

## 14.0 Construction Task Graph

This section defines the Construction Task Graph. The task graph is the authoritative planning artifact consumed by the execution runtime.

The Construction Task Graph must be complete enough to support scheduling, validation, monitoring, traceability, repair, and adaptation.

### 14.1 Graph Structure

The Construction Task Graph SHALL be a directed graph.

| Graph Element  | Description                                                |
| -------------- | ---------------------------------------------------------- |
| Node           | Construction task                                          |
| Edge           | Dependency relationship                                    |
| Root           | Planning or interpretation task initiating construction    |
| Terminal Nodes | Consolidation, deployment-preparation, or completion tasks |

### 14.2 Graph Schema

The Construction Task Graph MUST conform to the following structure.

| Field                    | Type     | Required    | Description                                                          |
| ------------------------ | -------- | ----------- | -------------------------------------------------------------------- |
| plan-id                  | string   | REQUIRED    | Unique construction plan identifier                                  |
| specification-id         | string   | REQUIRED    | System identifier                                                    |
| specification-version    | semver   | REQUIRED    | Specification version                                                |
| canonical-model-version  | semver   | REQUIRED    | Canonical model version                                              |
| readiness-level          | enum     | REQUIRED    | Readiness level at time of planning                                  |
| planning-mode            | enum     | REQUIRED    | advisory, formal, adaptation                                         |
| created-at               | ISO 8601 | REQUIRED    | Plan creation timestamp                                              |
| updated-at               | ISO 8601 | CONDITIONAL | Last update timestamp                                                |
| status                   | enum     | REQUIRED    | draft, valid, executable, in-progress, completed, failed, superseded |
| tasks                    | array    | REQUIRED    | Task definitions                                                     |
| dependencies             | array    | REQUIRED    | Dependency records                                                   |
| critical-path            | array    | REQUIRED    | Ordered task identifiers on critical path                            |
| parallel-groups          | array    | REQUIRED    | Groups of task identifiers eligible for parallel execution           |
| verification-plan        | object   | REQUIRED    | Verification task summary                                            |
| artifact-production-plan | object   | REQUIRED    | Expected artifact outputs                                            |
| governance-constraints   | array    | CONDITIONAL | Governance constraints affecting execution                           |
| traceability-snapshot-id | string   | CONDITIONAL | Traceability snapshot associated with plan                           |

### 14.3 Graph Validity Rules

A Construction Task Graph MUST contain at least one task.

Every task dependency MUST reference defined task identifiers.

Every non-root task MUST be reachable through dependency or sequencing relationships.

Every task producing artifacts MUST declare expected artifact types.

Every implementation, schema, interface, infrastructure, and security task MUST have associated verification coverage.

A formal Construction Task Graph MUST be acyclic for required execution dependencies.

### 14.4 Graph Status Rules

| Status      | Meaning                                                                |
| ----------- | ---------------------------------------------------------------------- |
| draft       | Plan is incomplete or advisory                                         |
| valid       | Plan satisfies planning validation but is not yet execution-authorized |
| executable  | Plan may be consumed by execution runtime                              |
| in-progress | Plan is actively executing                                             |
| completed   | Plan execution completed successfully                                  |
| failed      | Plan cannot complete                                                   |
| superseded  | Plan replaced by later plan version                                    |

A plan MUST NOT be marked executable unless all planning validation checks pass and readiness permits execution preparation.

---

## 15.0 Verification Task Integration

This section defines how verification is integrated into construction planning. Verification tasks validate artifacts and task outputs through deterministic tools, review criteria, security checks, or governance controls.

Verification must be planned before execution so that artifact acceptance criteria are known at generation time.

### 15.1 Required Verification Coverage

The Planning Engine MUST create or associate verification tasks for:

* implementation tasks
* schema tasks
* interface tasks
* infrastructure tasks
* security tasks
* test generation tasks
* deployment preparation tasks
* documentation tasks when documentation is required for governance or operation

### 15.2 Verification Task Schema Extensions

A verification task MUST include the base task schema and the following fields.

| Field                      | Type  | Required    | Description                                                           |
| -------------------------- | ----- | ----------- | --------------------------------------------------------------------- |
| verification-targets       | array | REQUIRED    | Artifact types, artifact identifiers, or task identifiers to validate |
| validation-criteria        | array | REQUIRED    | Conditions that define successful verification                        |
| required-tool-capabilities | array | CONDITIONAL | Tool capabilities required for deterministic verification             |
| expected-outcome           | enum  | REQUIRED    | passed, warning-acceptable                                            |
| failure-action             | enum  | REQUIRED    | repair, escalate, halt, warn                                          |
| validation-entity-ids      | array | CONDITIONAL | Validation entities linked to task                                    |

### 15.3 Verification Planning Rules

Verification tasks MUST be scheduled after the tasks that produce their verification targets.

Verification criteria MUST be traceable to Validation entities, policy rules, task outputs, or platform-defined quality rules.

Verification tasks MUST specify failure-action.

A verification task without validation criteria MUST fail planning validation.

---

## 16.0 Artifact Production Planning

This section defines how expected artifacts are planned before generation. Artifact production planning allows the runtime to detect missing outputs and allows traceability to be established at artifact creation time.

A task that produces artifacts must declare what it is expected to produce. This declaration becomes part of execution acceptance.

### 16.1 Artifact Production Declaration

Each task that produces artifacts MUST declare:

| Field                       | Type    | Required    | Description                                                           |
| --------------------------- | ------- | ----------- | --------------------------------------------------------------------- |
| expected-artifact-type      | enum    | REQUIRED    | source, config, schema, infrastructure, contract, test, documentation |
| expected-count              | integer | CONDITIONAL | Expected number of artifacts when known                               |
| source-entity-ids           | array   | REQUIRED    | Specification entities artifacts are expected to trace to             |
| repository-location-pattern | string  | CONDITIONAL | Expected repository location or path pattern                          |
| validation-required         | boolean | REQUIRED    | Whether deterministic validation is required                          |
| documentation-required      | boolean | CONDITIONAL | Whether generated documentation is required                           |
| traceability-required       | boolean | REQUIRED    | Whether traceability association must be recorded                     |

### 16.2 Artifact Production Rules

If a task completes without producing declared artifacts, the runtime MUST treat the task as failed.

If an artifact is produced that was not declared, the runtime MUST classify it as unexpected and require traceability justification before acceptance.

Expected artifacts MUST be mapped to source specification entities before execution.

A task that produces deployment artifacts MUST trace those artifacts to Infrastructure or operational Requirement entities.

---

## 17.0 Parallel Construction Planning

This section defines how the Planning Engine identifies tasks that may execute concurrently. Parallel execution improves performance but must not compromise dependency safety, artifact consistency, governance, or traceability.

Parallel groups are planning recommendations consumed by the execution runtime. The runtime may further restrict parallelism based on capacity, configuration, or governance constraints.

### 17.1 Parallel Eligibility Criteria

Tasks MAY be placed in the same parallel group when:

* no required dependency path exists between them
* they do not write to the same artifact target
* they do not require exclusive access to the same state resource
* they do not require sequential governance review
* they can produce traceability independently
* their required tools and agents can execute concurrently

### 17.2 Parallel Group Schema

| Field                  | Type   | Required    | Description                              |
| ---------------------- | ------ | ----------- | ---------------------------------------- |
| parallel-group-id      | string | REQUIRED    | Unique parallel group identifier         |
| task-ids               | array  | REQUIRED    | Tasks eligible for concurrent execution  |
| dependency-boundary    | string | REQUIRED    | Dependency condition that releases group |
| shared-resources       | array  | CONDITIONAL | Shared resources considered by planner   |
| governance-constraints | array  | CONDITIONAL | Constraints affecting concurrency        |
| rationale              | string | REQUIRED    | Why tasks are safe to run in parallel    |

### 17.3 Parallel Planning Rules

A task MUST NOT be placed in a parallel group with another task if either task depends on the other.

Tasks that modify the same artifact or repository location MUST NOT be parallel unless a coordination mechanism is declared.

Governance constraints MAY force sequential execution even where technical dependencies permit parallelism.

The runtime MAY reduce or serialize parallel groups but MUST NOT increase concurrency beyond dependency and governance limits without replanning or validation.

---

## 18.0 Critical Path Calculation

This section defines critical path calculation. The critical path identifies the longest dependency chain in the Construction Task Graph and provides a planning estimate of the minimum execution sequence assuming unlimited parallel capacity.

Critical path information supports scheduling, resource allocation, monitoring, and impact analysis.

### 18.1 Critical Path Requirements

The Planning Engine MUST compute and record the critical path for every formal Construction Task Graph.

The critical path MUST include ordered task identifiers.

The critical path MUST be recomputed when:

* tasks are added
* tasks are removed
* dependencies change
* repair tasks are inserted
* affected tasks are reset due to specification change
* governance constraints alter execution order

### 18.2 Critical Path Record Schema

| Field              | Type     | Required    | Description                            |
| ------------------ | -------- | ----------- | -------------------------------------- |
| critical-path-id   | string   | REQUIRED    | Unique critical path record identifier |
| plan-id            | string   | REQUIRED    | Construction plan identifier           |
| task-ids           | array    | REQUIRED    | Ordered tasks in critical path         |
| calculated-at      | ISO 8601 | REQUIRED    | Time calculated                        |
| calculation-method | string   | REQUIRED    | Algorithm or method used               |
| estimated-duration | string   | OPTIONAL    | Estimated duration when available      |
| assumptions        | array    | CONDITIONAL | Assumptions affecting path calculation |

### 18.3 Critical Path Rules

The critical path MUST contain only tasks present in the Construction Task Graph.

The critical path MUST respect dependency direction.

If task durations are unknown, the Planning Engine MAY calculate critical path by dependency length rather than time.

---

## 19.0 Repair Task Generation

This section defines how repair tasks are generated and inserted into the Construction Task Graph. Unlike initial construction tasks, repair tasks are typically generated dynamically during execution after validation or testing failure.

Repair planning must respect execution repair limits defined in ISL v1.2.

### 19.1 Repair Task Trigger

A repair task MAY be generated only when one of the following occurs:

* deterministic validation fails
* test execution fails
* policy validation identifies correctable non-compliance
* artifact production produces invalid structure
* review identifies a defect requiring correction

### 19.2 Repair Task Schema Extensions

A repair task MUST include the base task schema and the following fields.

| Field                       | Type    | Required    | Description                                        |
| --------------------------- | ------- | ----------- | -------------------------------------------------- |
| repair-task-id              | string  | REQUIRED    | Unique repair task identifier                      |
| failed-task-id              | string  | REQUIRED    | Task whose output failed                           |
| failed-validation-result-id | string  | REQUIRED    | Validation or test result triggering repair        |
| failed-artifact-id          | string  | REQUIRED    | Artifact being repaired                            |
| iteration                   | integer | REQUIRED    | Repair attempt number for artifact                 |
| termination-policy          | object  | REQUIRED    | Repair limits from ISL v1.2                        |
| repair-scope                | enum    | REQUIRED    | artifact, task, service, interface, schema, policy |
| expected-correction         | string  | CONDITIONAL | Correction expected or proposed                    |

### 19.3 Repair Task Insertion Rules

A repair task MUST be inserted after the failed verification or test task.

A repair task MUST inherit relevant dependencies from the failed task.

A repair task MUST be followed by revalidation.

A repair task MUST be traceable to the failed artifact, failed validation result, and source specification entities.

The Planning Engine MUST refuse to generate a repair task that would exceed repair limits defined in ISL v1.2.

### 19.4 Repair Dependency Rules

Repair task dependencies MUST ensure that:

* failed artifact state is available
* validation findings are available
* affected source entities remain valid
* governance permits repair
* revalidation follows repair

---

## 20.0 Governance Constraints in Planning

This section defines how governance affects construction planning. Governance constraints must be visible in the plan before execution, not discovered only after a task attempts to run.

Governance constraints may require approvals, block certain tools, restrict external model use, force human review, or impose additional validation.

### 20.1 Governance Constraint Types

| Constraint Type       | Description                                          |
| --------------------- | ---------------------------------------------------- |
| approval-required     | Task requires approval before execution              |
| policy-check-required | Task requires policy evaluation                      |
| tool-restricted       | Task may use only approved tools                     |
| model-restricted      | Task may use only approved model providers           |
| human-review-required | Task output requires human review                    |
| waiver-required       | Task cannot proceed unless waiver exists             |
| risk-tier-control     | Task behavior constrained by risk tier               |
| separation-of-duties  | Approval or review must be assigned to distinct role |

### 20.2 Governance Constraint Record Schema

| Field             | Type    | Required    | Description                                 |
| ----------------- | ------- | ----------- | ------------------------------------------- |
| constraint-id     | string  | REQUIRED    | Unique constraint identifier                |
| task-id           | string  | REQUIRED    | Task affected                               |
| policy-id         | string  | CONDITIONAL | Policy causing constraint                   |
| constraint-type   | enum    | REQUIRED    | Constraint type from §20.1                  |
| enforcement-point | enum    | REQUIRED    | planning, execution, validation, deployment |
| blocking          | boolean | REQUIRED    | Whether constraint blocks execution         |
| required-action   | string  | REQUIRED    | Action required to satisfy constraint       |

### 20.3 Governance Planning Rules

The Planning Engine MUST identify governance constraints applicable to tasks.

A task with unresolved blocking governance constraints MUST NOT be considered execution-ready.

Governance constraints MUST be included in the Construction Task Graph or associated planning records.

---

## 21.0 Planning Validation

This section defines validation rules for the Construction Task Graph. Planning validation determines whether the generated plan is complete, internally consistent, dependency-safe, traceable, and executable.

A plan that fails validation MUST NOT be consumed by the execution runtime as executable.

### 21.1 Required Validation Checks

The Planning Engine MUST validate:

* plan has required metadata
* plan references current specification version
* all tasks have required fields
* all task identifiers are unique
* all dependencies resolve
* no required dependency cycles exist
* all source entity references resolve
* all required task types are generated
* every artifact-producing task declares expected artifacts
* every artifact-producing task has verification coverage
* every must-have requirement has implementation and validation path
* every policy has planned evaluation or enforcement task
* every interface has contract generation or validation task
* every DataEntity has schema or validation task
* every Infrastructure entity has deployment or validation task
* governance constraints are represented
* traceability links from source entities to tasks exist or can be created
* critical path is calculated
* parallel groups are dependency-safe

### 21.2 Planning Validation Report Schema

| Field                 | Type     | Required    | Description                 |
| --------------------- | -------- | ----------- | --------------------------- |
| validation-report-id  | string   | REQUIRED    | Unique report identifier    |
| plan-id               | string   | REQUIRED    | Construction plan evaluated |
| specification-id      | string   | REQUIRED    | System identifier           |
| specification-version | semver   | REQUIRED    | Specification version       |
| outcome               | enum     | REQUIRED    | passed, failed, warning     |
| findings              | array    | CONDITIONAL | Validation findings         |
| validated-at          | ISO 8601 | REQUIRED    | Time validation completed   |
| validated-by          | string   | REQUIRED    | Planning Engine component   |
| executable            | boolean  | REQUIRED    | Whether plan is executable  |

### 21.3 Planning Validation Finding Schema

| Field           | Type   | Required    | Description                  |
| --------------- | ------ | ----------- | ---------------------------- |
| finding-id      | string | REQUIRED    | Unique finding identifier    |
| finding-class   | enum   | REQUIRED    | Finding class                |
| severity        | enum   | REQUIRED    | blocking, high, medium, low  |
| task-id         | string | CONDITIONAL | Related task                 |
| entity-id       | string | CONDITIONAL | Related specification entity |
| message         | string | REQUIRED    | Finding description          |
| required-action | string | REQUIRED    | Remediation required         |

### 21.4 Failed Validation

If planning validation fails with blocking findings, the plan status MUST be `draft` or `failed`.

The plan MUST NOT be marked executable.

---

## 22.0 Planning Error Classes

This section defines planning-specific error classes. These errors support consistent reporting across Planning Engines, authoring tools, governance systems, and execution runtimes.

### 22.1 Error Classes

| Error Class                            | Description                                                 | Default Severity |
| -------------------------------------- | ----------------------------------------------------------- | ---------------- |
| planning-precondition-failed           | Planning precondition was not satisfied                     | blocking         |
| planning-input-version-mismatch        | Planning inputs reference inconsistent versions             | blocking         |
| planning-missing-task                  | Required task was not generated                             | blocking         |
| planning-invalid-task-schema           | Task is missing required fields                             | blocking         |
| planning-duplicate-task-id             | Task identifier is duplicated                               | blocking         |
| planning-unresolved-dependency         | Dependency references missing task                          | blocking         |
| planning-circular-dependency           | Required dependency cycle exists                            | blocking         |
| planning-missing-source-trace          | Task lacks source entity or rule trace                      | blocking         |
| planning-missing-verification          | Artifact-producing task lacks verification coverage         | blocking         |
| planning-missing-artifact-declaration  | Artifact-producing task lacks expected artifact declaration | blocking         |
| planning-invalid-parallel-group        | Parallel group violates dependency or resource constraints  | high             |
| planning-missing-governance-constraint | Applicable governance constraint not represented            | high             |
| planning-repair-limit-exceeded         | Repair task would exceed termination policy                 | blocking         |
| planning-critical-path-unavailable     | Critical path could not be calculated                       | high             |
| planning-impact-analysis-required      | Change requires impact analysis before adaptation           | blocking         |

### 22.2 Error Record Schema

| Field           | Type     | Required    | Description                  |
| --------------- | -------- | ----------- | ---------------------------- |
| error-id        | string   | REQUIRED    | Unique error identifier      |
| error-class     | enum     | REQUIRED    | Error class                  |
| severity        | enum     | REQUIRED    | blocking, high, medium, low  |
| plan-id         | string   | CONDITIONAL | Related plan                 |
| task-id         | string   | CONDITIONAL | Related task                 |
| entity-id       | string   | CONDITIONAL | Related specification entity |
| message         | string   | REQUIRED    | Human-readable explanation   |
| required-action | string   | REQUIRED    | Remediation required         |
| recorded-at     | ISO 8601 | REQUIRED    | Time error was recorded      |

---

## 23.0 Construction Monitoring

This section defines planning obligations that support execution monitoring. Although execution monitoring is performed by the runtime, the construction plan must provide the status model and expected task transitions the runtime will use.

Construction monitoring ensures that progress can be tracked and that invalid task state changes are detected.

### 23.1 Task Status Values

| Status      | Meaning                                                                         |
| ----------- | ------------------------------------------------------------------------------- |
| pending     | Task has not started                                                            |
| in-progress | Task has been assigned or is executing                                          |
| completed   | Task completed and required verification passed or was waived                   |
| failed      | Task failed due to invalid output, failed validation, or execution error        |
| escalated   | Task requires governance or human intervention                                  |
| skipped     | Task was not executed due to validated non-applicability or governance decision |

### 23.2 Permitted Status Transitions

| From        | To          | Condition                                                           |
| ----------- | ----------- | ------------------------------------------------------------------- |
| pending     | in-progress | Task assigned to agent, tool, or component                          |
| pending     | skipped     | Task determined not applicable by valid rule or governance decision |
| in-progress | completed   | Task outputs produced and required verification passed or waived    |
| in-progress | failed      | Task failed or produced invalid outputs                             |
| in-progress | escalated   | Escalation threshold or governance condition reached                |
| failed      | in-progress | Repair task generated and assigned                                  |
| failed      | escalated   | Repair not possible or repair threshold reached                     |
| escalated   | in-progress | Escalation resolved and retry authorized                            |
| escalated   | failed      | Governance closes task as failed                                    |
| completed   | pending     | Specification change invalidates completed task                     |
| completed   | skipped     | Governance or impact analysis determines task no longer applicable  |

No other transitions are permitted.

### 23.3 Monitoring Rules

The runtime MUST update task status as execution progresses.

Invalid status transitions MUST be recorded as execution integrity violations.

The Planning Engine MUST preserve status history when adapting an in-progress plan.

---

## 24.0 Plan Adaptation to Specification Changes

This section defines how the Planning Engine adapts a plan when the specification changes. Because ISL specifications are living artifacts, construction plans must evolve without requiring unnecessary full reconstruction.

Plan adaptation must be driven by traceability and impact analysis.

### 24.1 Adaptation Trigger

Plan adaptation MUST occur when:

* a specification change affects planned or completed tasks
* readiness regression affects execution eligibility
* impact analysis identifies affected artifacts or tasks
* governance requires replanning
* an interface, policy, data entity, infrastructure, or must-have requirement changes
* repair or execution feedback reveals a planning defect

### 24.2 Required Adaptation Inputs

Plan adaptation MUST use:

* prior construction plan
* previous specification version
* new specification version
* impact analysis result
* traceability graph
* governance constraints
* execution state when adaptation occurs during execution

### 24.3 Adaptation Process

When adapting a plan, the Planning Engine MUST:

1. Receive or generate an impact analysis result.
2. Identify affected specification entities.
3. Identify affected tasks.
4. Identify affected artifacts.
5. Reset affected incomplete or completed tasks when required.
6. Preserve unaffected completed tasks.
7. Generate tasks for newly added entities.
8. Remove or deprecate tasks for removed entities where safe.
9. Insert migration, cleanup, or supersession tasks when required.
10. Recompute dependencies.
11. Recompute critical path.
12. Recompute parallel groups.
13. Revalidate the adapted plan.
14. Record a Plan Adaptation Record.
15. Update traceability.

### 24.4 Plan Adaptation Record Schema

| Field                          | Type     | Required    | Description                                  |
| ------------------------------ | -------- | ----------- | -------------------------------------------- |
| adaptation-id                  | string   | REQUIRED    | Unique adaptation identifier                 |
| prior-plan-id                  | string   | REQUIRED    | Plan being adapted                           |
| new-plan-id                    | string   | REQUIRED    | Adapted plan identifier                      |
| previous-specification-version | semver   | REQUIRED    | Prior specification version                  |
| new-specification-version      | semver   | REQUIRED    | New specification version                    |
| impact-analysis-id             | string   | REQUIRED    | Impact analysis used                         |
| affected-task-ids              | array    | REQUIRED    | Tasks affected                               |
| preserved-task-ids             | array    | REQUIRED    | Tasks preserved                              |
| added-task-ids                 | array    | CONDITIONAL | New tasks added                              |
| removed-task-ids               | array    | CONDITIONAL | Tasks removed or deprecated                  |
| reset-task-ids                 | array    | CONDITIONAL | Tasks reset to pending                       |
| governance-events              | array    | CONDITIONAL | Governance events associated with adaptation |
| adapted-at                     | ISO 8601 | REQUIRED    | Time adaptation completed                    |
| outcome                        | enum     | REQUIRED    | adapted, failed, escalated                   |

### 24.5 Adaptation Rules

The platform MUST NOT restart the entire construction plan when impact analysis identifies a bounded affected scope.

Only affected tasks MUST be re-executed unless governance or traceability integrity requires broader reconstruction.

Unchanged completed tasks MAY be preserved if their artifacts remain valid and traceability confirms they are unaffected.

A task associated with a removed specification entity MUST be removed, deprecated, or converted into cleanup/migration work depending on artifact impact.

---

## 25.0 Plan Versioning and Supersession

This section defines how construction plans are versioned. Construction plans are derived artifacts and must be preserved across specification changes, replanning, repair insertion, and execution history.

Plan versioning supports audit, recovery, comparison, and traceability.

### 25.1 Plan Version Fields

Every construction plan MUST include:

| Field                 | Type     | Required    | Description                                                          |
| --------------------- | -------- | ----------- | -------------------------------------------------------------------- |
| plan-version          | semver   | REQUIRED    | Version of construction plan                                         |
| supersedes-plan-id    | string   | CONDITIONAL | Prior plan replaced by this plan                                     |
| superseded-by-plan-id | string   | CONDITIONAL | Later plan replacing this plan                                       |
| version-reason        | enum     | REQUIRED    | initial, adaptation, repair-insertion, governance-change, correction |
| created-at            | ISO 8601 | REQUIRED    | Version creation time                                                |

### 25.2 Versioning Rules

A new plan version MUST be created when:

* the specification version changes
* impact analysis modifies task scope
* task dependencies are materially changed
* repair tasks are inserted into the persistent graph
* governance constraints alter execution order
* critical path changes materially
* execution runtime requires replanning

Prior plan versions MUST remain queryable.

A superseded plan MUST NOT be deleted if it was used for execution or governance review.

---

## 26.0 Planning and Traceability Integration

This section defines how planning integrates with traceability. The construction plan is a traceability-critical artifact because it connects canonical entities to execution tasks.

### 26.1 Required Planning Traceability Links

The Planning Engine MUST create or prepare traceability links for:

* Project contains Construction Plan
* SpecificationEntity originates ConstructionTask
* ConstructionTask depends-on ConstructionTask
* ConstructionTask produces planned Artifact types
* ConstructionTask governed-by Policy where applicable
* VerificationTask validates planned Artifact or source entity
* Plan supersedes prior Plan when applicable

### 26.2 Traceability Timing

Traceability links from entities to tasks MUST be created during planning.

Traceability links from tasks to actual artifacts are created during execution.

A plan MUST NOT be marked executable if source entity to task traceability is missing.

---

## 27.0 Planning and Readiness Integration

This section defines how planning interacts with readiness levels. Readiness determines which planning actions are permitted.

### 27.1 Planning Permissions

| Readiness Level  | Permitted Planning Activity               |
| ---------------- | ----------------------------------------- |
| Draft            | Advisory planning only                    |
| Reviewable       | Advisory and preliminary planning         |
| Machine-Valid    | Formal construction planning              |
| Autonomous-Ready | Executable planning and execution handoff |

### 27.2 Readiness Rules

A formal construction plan MAY be generated at Machine-Valid readiness.

A construction plan MUST NOT be executed until Autonomous-Ready readiness and execution preconditions are satisfied.

If readiness regresses, the Planning Engine MUST determine whether the plan remains valid, must be adapted, or must be superseded.

---

## 28.0 Planning and Execution Handoff

This section defines the handoff from Planning Engine to Execution Runtime. The execution runtime depends on a validated, executable Construction Task Graph.

### 28.1 Handoff Preconditions

A plan MUST satisfy the following before handoff to execution:

* plan validation passed
* plan status is executable
* specification version is Autonomous-Ready
* governance constraints are represented
* required tool capabilities are known
* required agent roles are known
* artifact production plan exists
* verification plan exists
* traceability links from entities to tasks exist
* critical path is calculated
* parallel groups are calculated

### 28.2 Execution Handoff Record Schema

| Field                    | Type     | Required    | Description                                |
| ------------------------ | -------- | ----------- | ------------------------------------------ |
| handoff-id               | string   | REQUIRED    | Unique handoff identifier                  |
| plan-id                  | string   | REQUIRED    | Plan handed to execution                   |
| specification-id         | string   | REQUIRED    | System identifier                          |
| specification-version    | semver   | REQUIRED    | Specification version                      |
| readiness-level          | enum     | REQUIRED    | Readiness level at handoff                 |
| validation-report-id     | string   | REQUIRED    | Planning validation report                 |
| traceability-snapshot-id | string   | REQUIRED    | Traceability snapshot at handoff           |
| handed-off-at            | ISO 8601 | REQUIRED    | Time handoff occurred                      |
| accepted-by-runtime      | boolean  | REQUIRED    | Whether runtime accepted handoff           |
| rejection-reason         | string   | CONDITIONAL | Required when accepted-by-runtime is false |

### 28.3 Runtime Rejection

The Execution Runtime MUST reject a handoff when:

* plan status is not executable
* specification is not Autonomous-Ready
* required dependencies are unresolved
* required tools or agent roles are unavailable
* governance constraints are unresolved
* traceability snapshot is missing
* planning validation failed

---

## 29.0 Completion of the Construction Plan

This section defines when a construction plan may be considered complete. Planning completion is not the same as execution completion. A plan may be complete as a planning artifact before execution begins; it may also later be completed as an executed plan.

### 29.1 Planning Completion Criteria

A construction plan may be marked planning-complete when:

* all required tasks have been generated
* all required dependencies have been resolved
* all required verification tasks exist
* artifact production plan is complete
* governance constraints are represented
* critical path is calculated
* parallel groups are calculated
* planning validation passes
* traceability links from source entities to tasks exist

### 29.2 Execution Completion Criteria

A construction plan may be marked execution-completed only when the execution runtime reports completion under ISL v1.2.

Execution completion requires:

* all required tasks completed, skipped, or waived
* all required artifacts produced and validated or waived
* repair cycles resolved or escalated and closed
* tests completed or waived
* policy validation completed or waived
* artifacts consolidated
* deployment preparation completed
* traceability integrity checks passed
* Completion Report produced

### 29.3 Failed Plan Completion

A construction plan MUST be marked failed when:

* blocking planning validation errors remain unresolved
* required dependencies cannot be resolved
* required tasks cannot be generated
* repair limits prevent completion
* traceability integrity cannot be established
* governance blocks required planning or execution activity

---

## 30.0 Planning Records and Auditability

This section defines the records that must be preserved for planning auditability. Construction planning decisions affect architecture, execution, artifacts, and governance, so they must be auditable.

### 30.1 Required Planning Records

The platform MUST preserve:

* Planning Record
* Construction Task Graph
* Task Definitions
* Dependency Records
* Planning Validation Report
* Critical Path Record
* Parallel Group Records
* Artifact Production Plan
* Verification Plan
* Governance Constraint Records
* Plan Adaptation Records
* Execution Handoff Records
* Planning Error Records

### 30.2 Audit Query Requirements

The platform MUST support queries for:

* all tasks derived from a specification entity
* all tasks producing a given artifact type
* all dependencies for a task
* all verification tasks for an implementation task
* all tasks affected by a specification change
* all tasks blocked by governance constraints
* all repair tasks inserted during execution
* critical path for a plan
* parallel groups for a plan
* planning validation findings

### 30.3 Record Immutability

Planning records used for execution, governance review, or audit MUST NOT be overwritten.

Corrections MUST be recorded by creating a new plan version or corrective record.

---

## 31.0 Conformance Requirements

This section defines conformance for construction plans, Planning Engines, and platforms.

### 31.1 Construction Plan Conformance

A Construction Plan conforms to ISL v1.5 if it:

* includes required plan metadata
* contains valid task definitions
* contains valid dependency records
* resolves all required dependencies
* contains no invalid required dependency cycles
* includes verification coverage
* includes artifact production declarations
* includes governance constraints
* includes critical path
* includes parallel groups
* passes planning validation
* preserves traceability to source entities

### 31.2 Planning Engine Conformance

A Planning Engine conforms to ISL v1.5 if it can:

* validate planning preconditions
* consume canonical ISL models
* generate construction tasks from canonical entities
* assign task types and responsible roles
* resolve dependencies
* detect circular dependencies
* generate verification tasks
* generate artifact production plans
* identify parallel groups
* calculate critical path
* represent governance constraints
* validate construction plans
* adapt plans based on impact analysis
* produce required planning records
* support execution handoff

### 31.3 Platform Conformance

A platform conforms to ISL v1.5 if it:

* integrates planning with readiness state
* integrates planning with traceability
* integrates planning with governance
* integrates planning with tool capability discovery
* prevents execution of non-executable plans
* preserves planning records
* supports plan adaptation
* supports planning audit queries

---

## 32.0 Summary

ISL v1.5 defines the Construction Planning Model that transforms a canonical ISL specification into an executable construction task graph. It establishes how tasks are generated, classified, assigned, validated, ordered, verified, adapted, monitored, and handed off to execution.

This revision strengthens construction planning by making task schemas, dependency rules, verification integration, artifact production planning, repair insertion, governance constraints, parallel execution, critical path calculation, plan adaptation, traceability, and conformance requirements explicit.

A conforming IMHOTEP platform MUST treat the construction plan as the authoritative bridge between specification intent and autonomous execution. The platform MUST NOT execute work that is not represented in a valid, traceable, and executable Construction Task Graph.
