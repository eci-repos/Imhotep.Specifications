# IMHOTEP Specification Language (ISL) v3.4

# The Agent Implementation Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.4, ISL v2.5, ISL v2.6, ISL v3.0, ISL v3.1, ISL v3.2, ISL v3.3
**Supersedes:** ISL r0 v3.4 The Agent Implementation Model
**Document Type:** Agent Runtime, Packaging, and Implementation Specification

---

## 1.0 Scope

This document defines the implementation model for reasoning agents within the IMHOTEP platform. It specifies how agent roles are packaged, registered, invoked, configured, tested, validated, observed, governed, and deployed in the reference implementation.

This document applies to all agent implementations used by the IMHOTEP platform, including agents for specification interpretation, architecture planning, construction planning, implementation generation, repair analysis, test generation, review, security validation, and deployment preparation.

This document defines:

* agent implementation principles
* agent package structure
* agent role manifests
* agent runtime interface
* agent lifecycle implementation
* context package implementation
* prompt and instruction template implementation
* model binding
* output validation
* artifact-producing agent behavior
* repair agent behavior
* agent state and memory
* agent traceability
* agent observability
* agent testing
* agent deployment
* agent extension rules
* conformance requirements

---

## 2.0 Purpose of the Agent Implementation Model

The purpose of the Agent Implementation Model is to translate the normative agent role contracts defined in ISL v2.1 into implementable software components. ISL v2.1 defines what each agent role is allowed and required to do. This document defines how those roles are realized in the reference implementation.

Agents are specialized reasoning components. They perform interpretation, synthesis, review, generation, repair, testing, security analysis, and deployment preparation activities that cannot be reduced entirely to deterministic tools. However, agents are not free actors. They operate under runtime control, model governance, context limits, structured output contracts, deterministic validation, traceability, and observability.

This model exists to ensure that:

* each agent role is implemented as a bounded component
* every agent has a manifest describing its role, capabilities, inputs, outputs, and constraints
* agents are invoked only through the Agent Orchestrator
* agents receive controlled context packages
* agent requests are routed through the Model Integration Layer
* agent outputs are validated before use
* generated artifacts enter the Artifact Repository through admission controls
* repair behavior is bounded and traceable
* agent implementations can be tested independently
* agent implementations can evolve without breaking platform contracts

---

## 3.0 Agent Implementation Principles

### 3.1 Role Contract First

Every agent implementation MUST be based on a role contract defined in ISL v2.1 or an approved role extension.

An implementation MUST NOT create operational behavior for an agent role that exceeds the role’s permitted responsibilities.

### 3.2 Runtime-Mediated Invocation

Agents MUST be invoked by the Agent Orchestrator or authorized runtime component. Agents MUST NOT self-schedule, directly invoke other agents, directly mutate platform state, directly write stable artifacts, or directly approve governance gates.

### 3.3 Context Is an Input, Not Ambient Memory

Agents MUST receive context through explicit context packages. They MUST NOT rely on hidden ambient memory, global state, unrestricted repository access, or unrecorded prior conversation state.

### 3.4 Model Use Is Abstracted

Agents MUST request reasoning through the Model Integration Layer. Agent implementations MUST NOT call provider-specific model APIs directly.

### 3.5 Output Is a Contract

Agent outputs MUST conform to declared output contracts. Free-form text MAY be included as rationale, but machine-consumed outputs MUST be structured and validated.

### 3.6 Deterministic Follow-Up Is Required

Agent outputs that affect artifacts, validation, security, deployment, or governance MUST be followed by deterministic platform handling. Generated code MUST be compiled or tested where applicable. Proposed repair MUST be revalidated. Security findings MUST be routed through governance or deterministic checks where required.

### 3.7 Implementation Must Be Testable

Each agent implementation MUST be independently testable using fixture context packages, expected output contracts, invalid-output cases, and failure scenarios.

---

## 4.0 Standard Agent Implementations

The reference implementation SHOULD provide implementations for the standard agent roles defined in ISL v2.1.

| Agent Role                | Implementation Responsibility                                                                 |
| ------------------------- | --------------------------------------------------------------------------------------------- |
| Specification Interpreter | Converts canonical entities into execution-oriented interpretation records                    |
| Architecture Planner      | Produces architecture guidance, decomposition findings, and architecture decision suggestions |
| Construction Planner      | Supports task decomposition, dependency reasoning, and plan adaptation                        |
| Implementation Generator  | Produces candidate implementation artifacts                                                   |
| Repair Analyst            | Analyzes validation failures and proposes bounded repair actions                              |
| Test Generator            | Produces test plans and candidate test artifacts                                              |
| Review Agent              | Reviews outputs, artifacts, assumptions, and traceability completeness                        |
| Security Validator        | Reviews policies, security constraints, sensitive data handling, and security findings        |
| Deployment Preparer       | Produces deployment preparation artifacts and operational readiness findings                  |

### 4.1 Role Implementation Rule

A platform MAY implement a subset of standard agents, but it MUST declare supported roles in the Agent Registry.

A platform claiming full autonomous construction capability MUST implement all standard agent roles.

---

## 5.0 Agent Package Structure

Agent implementations MUST be packaged in a predictable structure.

### 5.1 Reference Agent Package Layout

| Path                                         | Purpose                                         |
| -------------------------------------------- | ----------------------------------------------- |
| `/src/Imhotep.Agents/{role-name}`            | Agent role implementation                       |
| `/src/Imhotep.Agents/{role-name}/manifest`   | Agent role manifest                             |
| `/src/Imhotep.Agents/{role-name}/templates`  | Prompt and instruction templates                |
| `/src/Imhotep.Agents/{role-name}/validators` | Output and context validators                   |
| `/src/Imhotep.Agents/{role-name}/mappers`    | Mapping between model outputs and agent outputs |
| `/src/Imhotep.Agents/{role-name}/fixtures`   | Local fixture context packages for testing      |
| `/tests/agents/{role-name}`                  | Tests for the agent role                        |
| `/docs/agents/{role-name}`                   | Agent implementation documentation              |

### 5.2 Package Rules

An agent package MUST include a manifest.

An agent package MUST include at least one output validator.

An agent package MUST include tests or a test coverage plan.

An agent package MUST declare supported output contracts.

An agent package MUST NOT include provider-specific model credentials.

---

## 6.0 Agent Role Manifest

Each agent implementation MUST provide an Agent Role Manifest.

### 6.1 Agent Role Manifest Schema

| Field                             | Type     | Required    | Description                                             |
| --------------------------------- | -------- | ----------- | ------------------------------------------------------- |
| agent-implementation-id           | string   | REQUIRED    | Unique implementation identifier                        |
| agent-role                        | enum     | REQUIRED    | Standard or extension role                              |
| implementation-version            | semver   | REQUIRED    | Agent implementation version                            |
| supported-isl-versions            | array    | REQUIRED    | ISL versions supported                                  |
| role-contract-version             | semver   | REQUIRED    | Role contract version implemented                       |
| supported-task-types              | array    | REQUIRED    | Construction task types supported                       |
| required-capabilities             | array    | REQUIRED    | Required model capabilities                             |
| supported-output-contracts        | array    | REQUIRED    | Output contracts this agent can produce                 |
| required-context-categories       | array    | REQUIRED    | Context categories required                             |
| optional-context-categories       | array    | CONDITIONAL | Context categories optionally used                      |
| prohibited-context-categories     | array    | CONDITIONAL | Context categories the agent MUST NOT receive           |
| artifact-producing                | boolean  | REQUIRED    | Whether agent may produce artifact candidates           |
| deterministic-validation-required | boolean  | REQUIRED    | Whether outputs require deterministic validation        |
| risk-tier-limit                   | enum     | CONDITIONAL | Highest risk tier allowed without additional governance |
| human-review-required             | boolean  | REQUIRED    | Whether output requires human review                    |
| telemetry-profile                 | string   | CONDITIONAL | Agent telemetry profile                                 |
| status                            | enum     | REQUIRED    | active, experimental, deprecated, disabled, revoked     |
| registered-at                     | ISO 8601 | REQUIRED    | Registration timestamp                                  |
| registered-by                     | string   | REQUIRED    | Authority or subsystem registering implementation       |

### 6.2 Manifest Rules

An agent implementation MUST NOT be registered without a valid manifest.

An agent implementation with status revoked MUST NOT be invoked.

An experimental agent MUST NOT be used for High or Critical risk systems unless governance explicitly approves it.

The Agent Orchestrator MUST validate the manifest before invoking the agent.

---

## 7.0 Agent Registry

The Agent Registry stores available agent implementations and their manifests.

### 7.1 Agent Registry Record Schema

| Field                    | Type     | Required    | Description                                                     |
| ------------------------ | -------- | ----------- | --------------------------------------------------------------- |
| agent-registry-record-id | string   | REQUIRED    | Unique registry record                                          |
| agent-implementation-id  | string   | REQUIRED    | Registered implementation                                       |
| agent-role               | enum     | REQUIRED    | Role implemented                                                |
| implementation-version   | semver   | REQUIRED    | Version registered                                              |
| manifest-reference       | string   | REQUIRED    | Manifest location or record                                     |
| registration-status      | enum     | REQUIRED    | active, restricted, experimental, deprecated, disabled, revoked |
| governance-profile-id    | string   | CONDITIONAL | Governance profile controlling use                              |
| registered-at            | ISO 8601 | REQUIRED    | Registration time                                               |
| registered-by            | string   | REQUIRED    | Registering authority or subsystem                              |

### 7.2 Registry Rules

The Agent Orchestrator MUST select agent implementations from the Agent Registry.

A task requiring an unavailable agent role MUST fail, queue, or escalate according to runtime policy.

Agent registry changes MUST be auditable.

A registry update during active execution MUST trigger impact evaluation if it changes role behavior.

---

## 8.0 Agent Runtime Interface

Agent implementations MUST expose a standard runtime interface.

### 8.1 Agent Runtime Request Schema

| Field                    | Type     | Required    | Description                                                                        |
| ------------------------ | -------- | ----------- | ---------------------------------------------------------------------------------- |
| agent-runtime-request-id | string   | REQUIRED    | Unique runtime request identifier                                                  |
| agent-invocation-id      | string   | REQUIRED    | Agent invocation identifier                                                        |
| agent-implementation-id  | string   | REQUIRED    | Agent implementation selected                                                      |
| agent-role               | enum     | REQUIRED    | Agent role                                                                         |
| task-id                  | string   | REQUIRED    | Construction task                                                                  |
| execution-graph-id       | string   | CONDITIONAL | Execution graph during runtime                                                     |
| context-package-id       | string   | REQUIRED    | Context package supplied                                                           |
| output-contract-id       | string   | REQUIRED    | Required output contract                                                           |
| invocation-mode          | enum     | REQUIRED    | interpret, plan, generate, review, repair, test, security-validate, deploy-prepare |
| model-routing-policy-id  | string   | CONDITIONAL | Routing policy to be used                                                          |
| governance-check-id      | string   | CONDITIONAL | Governance check authorizing invocation                                            |
| timeout-seconds          | integer  | REQUIRED    | Maximum execution time                                                             |
| correlation-id           | string   | REQUIRED    | Correlation identifier                                                             |
| requested-at             | ISO 8601 | REQUIRED    | Request timestamp                                                                  |

### 8.2 Agent Runtime Response Schema

| Field                        | Type     | Required    | Description                                                                 |
| ---------------------------- | -------- | ----------- | --------------------------------------------------------------------------- |
| agent-runtime-response-id    | string   | REQUIRED    | Unique response identifier                                                  |
| agent-runtime-request-id     | string   | REQUIRED    | Matching request                                                            |
| agent-invocation-id          | string   | REQUIRED    | Agent invocation identifier                                                 |
| outcome                      | enum     | REQUIRED    | completed, failed, timeout, cancelled, escalated                            |
| agent-output-id              | string   | CONDITIONAL | Output produced                                                             |
| validation-status            | enum     | REQUIRED    | not-evaluated, passed, failed, warning                                      |
| findings                     | array    | CONDITIONAL | Findings from execution or validation                                       |
| produced-artifact-candidates | array    | CONDITIONAL | Candidate artifacts produced                                                |
| model-invocation-ids         | array    | CONDITIONAL | Model invocations used                                                      |
| telemetry-event-ids          | array    | CONDITIONAL | Telemetry emitted                                                           |
| completed-at                 | ISO 8601 | REQUIRED    | Completion timestamp                                                        |
| next-action                  | enum     | REQUIRED    | accept, retry, revise, validate-artifacts, review, repair, escalate, reject |

### 8.3 Runtime Interface Rules

An agent runtime request MUST include task-id, agent-role, context-package-id, and output-contract-id.

An agent runtime response MUST identify outcome and next-action.

An agent runtime response MUST NOT claim completed when output validation fails unless the output is explicitly partial and accepted by the role contract.

---

## 9.0 Agent Lifecycle Implementation

Agent implementations MUST support lifecycle states consistent with ISL v2.1.

### 9.1 Implementation Lifecycle States

| State            | Meaning                                                              |
| ---------------- | -------------------------------------------------------------------- |
| registered       | Agent implementation is known to the platform                        |
| available        | Agent implementation can receive invocations                         |
| selected         | Agent implementation selected for a task                             |
| context-bound    | Context package bound to invocation                                  |
| model-bound      | Model routing decision selected or deterministic generation selected |
| invoked          | Agent execution started                                              |
| output-produced  | Agent produced output                                                |
| output-validated | Output validation completed                                          |
| completed        | Invocation completed successfully                                    |
| failed           | Invocation failed                                                    |
| escalated        | Invocation requires external resolution                              |
| disabled         | Agent implementation unavailable for new work                        |

### 9.2 Lifecycle Rules

An agent MUST NOT enter invoked state without context-bound state.

An agent requiring model reasoning MUST NOT enter invoked state without model-bound state.

An agent MUST NOT enter completed state unless output validation passes or the role contract permits partial output.

Agent lifecycle transitions MUST be recorded as state events.

---

## 10.0 Context Package Implementation

Agents receive context packages assembled by the platform.

### 10.1 Agent Context Categories

| Category             | Description                                                           |
| -------------------- | --------------------------------------------------------------------- |
| canonical-entities   | Canonical entities relevant to task                                   |
| task-definition      | Construction task details                                             |
| planning-context     | Plan, dependencies, critical path, or plan findings                   |
| artifact-context     | Prior or related artifact content and metadata                        |
| validation-context   | Validation results, tool findings, test failures                      |
| repair-context       | Prior repair records and repair limits                                |
| governance-context   | Policies, constraints, waivers, approvals, risk tier                  |
| traceability-context | Existing traceability links and identifiers                           |
| repository-context   | Branch, path, package, and artifact repository metadata               |
| security-context     | Security policies, sensitivity classification, findings               |
| deployment-context   | Deployment target, infrastructure, readiness, operational constraints |

### 10.2 Context Package Binding Rules

The Agent Orchestrator MUST assemble or request a context package before invocation.

The agent implementation MUST verify that required context categories are present.

The agent implementation MUST reject or escalate context packages containing prohibited categories.

Context package identifiers MUST be recorded in agent invocation records.

---

## 11.0 Prompt and Instruction Template Implementation

Agent implementations MAY use prompt or instruction templates to frame model interactions.

### 11.1 Template Record Schema

| Field                         | Type     | Required    | Description                                |
| ----------------------------- | -------- | ----------- | ------------------------------------------ |
| template-id                   | string   | REQUIRED    | Unique template identifier                 |
| agent-implementation-id       | string   | REQUIRED    | Agent implementation using template        |
| agent-role                    | enum     | REQUIRED    | Agent role                                 |
| template-version              | semver   | REQUIRED    | Template version                           |
| invocation-mode               | enum     | REQUIRED    | Invocation mode                            |
| required-context-categories   | array    | REQUIRED    | Context categories required                |
| output-contract-id            | string   | REQUIRED    | Output contract expected                   |
| safety-instructions-reference | string   | CONDITIONAL | Safety or policy instruction reference     |
| status                        | enum     | REQUIRED    | active, experimental, deprecated, disabled |
| created-at                    | ISO 8601 | REQUIRED    | Creation timestamp                         |

### 11.2 Template Rules

Templates MUST be versioned.

Templates MUST identify output contracts.

Templates MUST distinguish platform instructions from untrusted context.

Templates MUST NOT embed secrets.

A template change affecting output structure MUST trigger contract test review.

---

## 12.0 Model Binding

Agent implementations bind to model capabilities through the Model Integration Layer.

### 12.1 Model Binding Record Schema

| Field                     | Type     | Required | Description               |
| ------------------------- | -------- | -------- | ------------------------- |
| model-binding-id          | string   | REQUIRED | Unique binding identifier |
| agent-invocation-id       | string   | REQUIRED | Agent invocation          |
| agent-implementation-id   | string   | REQUIRED | Agent implementation      |
| required-capabilities     | array    | REQUIRED | Capabilities required     |
| routing-decision-id       | string   | REQUIRED | Model routing decision    |
| selected-model-profile-id | string   | REQUIRED | Selected model profile    |
| selected-provider-id      | string   | REQUIRED | Selected provider         |
| context-package-id        | string   | REQUIRED | Context supplied          |
| output-contract-id        | string   | REQUIRED | Expected output           |
| bound-at                  | ISO 8601 | REQUIRED | Binding time              |

### 12.2 Model Binding Rules

Agents MUST NOT bind directly to provider-specific model APIs.

The selected model MUST satisfy the required capabilities declared in the manifest.

Model binding MUST be recorded before model invocation.

Model fallback MUST produce a new or amended binding record.

---

## 13.0 Agent Output Implementation

Agent outputs must be structured and contract-validatable.

### 13.1 Agent Output Implementation Schema

| Field                             | Type     | Required    | Description                                                                                                                           |
| --------------------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| agent-output-id                   | string   | REQUIRED    | Unique output identifier                                                                                                              |
| agent-invocation-id               | string   | REQUIRED    | Invocation that produced output                                                                                                       |
| agent-implementation-id           | string   | REQUIRED    | Implementation that produced output                                                                                                   |
| agent-role                        | enum     | REQUIRED    | Agent role                                                                                                                            |
| output-contract-id                | string   | REQUIRED    | Output contract used                                                                                                                  |
| output-type                       | enum     | REQUIRED    | interpretation, plan-fragment, artifact-content, repair-proposal, test-plan, review-finding, security-finding, deployment-preparation |
| structured-output-reference       | string   | REQUIRED    | Structured output location or payload reference                                                                                       |
| rationale                         | string   | CONDITIONAL | Human-readable reasoning summary                                                                                                      |
| referenced-entities               | array    | REQUIRED    | Canonical entities referenced                                                                                                         |
| produced-artifact-candidates      | array    | CONDITIONAL | Candidate artifacts produced                                                                                                          |
| assumptions                       | array    | CONDITIONAL | Assumptions made                                                                                                                      |
| limitations                       | array    | CONDITIONAL | Known limitations                                                                                                                     |
| requires-deterministic-validation | boolean  | REQUIRED    | Whether deterministic validation is required                                                                                          |
| produced-at                       | ISO 8601 | REQUIRED    | Output timestamp                                                                                                                      |

### 13.2 Output Rules

An output MUST conform to the output-contract-id.

An output MUST reference canonical entities.

An output that produces artifacts MUST identify artifact candidates.

An output that cannot be validated MUST NOT be consumed downstream.

---

## 14.0 Output Validation Implementation

Each agent package MUST include validators or reference shared validators.

### 14.1 Output Validation Checks

Agent output validation MUST check:

* schema conformance
* required field presence
* output type compatibility
* role compatibility
* task compatibility
* referenced entity validity
* artifact candidate completeness
* prohibited action detection
* governance constraint compliance
* deterministic validation requirement flag
* sensitivity and security constraints

### 14.2 Output Validation Result Schema

| Field                      | Type     | Required    | Description                                               |
| -------------------------- | -------- | ----------- | --------------------------------------------------------- |
| agent-output-validation-id | string   | REQUIRED    | Unique validation record                                  |
| agent-output-id            | string   | REQUIRED    | Output evaluated                                          |
| agent-implementation-id    | string   | REQUIRED    | Producing implementation                                  |
| output-contract-id         | string   | REQUIRED    | Contract evaluated                                        |
| validation-outcome         | enum     | REQUIRED    | passed, failed, warning, escalated                        |
| findings                   | array    | CONDITIONAL | Findings from validation                                  |
| validated-at               | ISO 8601 | REQUIRED    | Validation timestamp                                      |
| validated-by               | string   | REQUIRED    | Validator component                                       |
| next-action                | enum     | REQUIRED    | accept, retry, request-revision, review, escalate, reject |

### 14.3 Validation Rules

An output that fails validation MUST NOT be accepted.

An output that proposes prohibited actions MUST be rejected or escalated.

An output that omits required traceability identifiers MUST be rejected.

A warning MAY be accepted only if the role contract and governance policy permit it.

---

## 15.0 Artifact-Producing Agent Implementation

Artifact-producing agents include Implementation Generator, Test Generator, Deployment Preparer, and any approved extension role that produces artifacts.

### 15.1 Artifact Candidate Schema

| Field                   | Type     | Required | Description                                                                       |
| ----------------------- | -------- | -------- | --------------------------------------------------------------------------------- |
| artifact-candidate-id   | string   | REQUIRED | Candidate artifact identifier                                                     |
| proposed-artifact-name  | string   | REQUIRED | Proposed artifact name                                                            |
| artifact-type           | enum     | REQUIRED | source, config, schema, infrastructure, contract, test, documentation, deployment |
| source-entity-ids       | array    | REQUIRED | Canonical entities implemented or supported                                       |
| task-id                 | string   | REQUIRED | Producing task                                                                    |
| content-reference       | string   | REQUIRED | Artifact content reference                                                        |
| derivation-type         | enum     | REQUIRED | direct, derived, inferred, repair, template-assisted                              |
| validation-expectations | array    | REQUIRED | Expected deterministic validations                                                |
| created-at              | ISO 8601 | REQUIRED | Candidate creation timestamp                                                      |

### 15.2 Artifact-Producing Rules

Artifact-producing agents MUST produce artifact candidates, not stable artifacts.

Artifact candidates MUST enter the Artifact Repository through admission.

Artifact-producing agents MUST NOT directly write to stable repository state.

Artifact candidates MUST include source-entity-ids.

Executable or structural artifacts MUST require deterministic validation.

---

## 16.0 Repair Agent Implementation

Repair Analyst implementations analyze validation failures and propose repairs.

### 16.1 Repair Analysis Implementation Rules

Repair Analyst MUST receive failed validation results, failed artifact references, tool findings, source entity references, and repair limits.

Repair Analyst MUST produce a repair proposal.

Repair proposals MUST identify affected artifacts.

Repair proposals MUST state whether revalidation is required.

Repair proposals MUST identify convergence risk.

Repair Analyst MUST NOT mark artifacts repaired, valid, or stable.

### 16.2 Repair Proposal Implementation Schema

| Field                       | Type     | Required    | Description                                              |
| --------------------------- | -------- | ----------- | -------------------------------------------------------- |
| repair-proposal-id          | string   | REQUIRED    | Unique repair proposal                                   |
| failed-validation-result-id | string   | REQUIRED    | Triggering validation result                             |
| failed-artifact-ids         | array    | REQUIRED    | Failed artifacts                                         |
| likely-root-cause           | string   | CONDITIONAL | Root cause when determinable                             |
| repair-scope                | enum     | REQUIRED    | artifact, task, service, interface, schema, policy, plan |
| proposed-corrections        | array    | REQUIRED    | Proposed corrective actions                              |
| affected-artifacts          | array    | REQUIRED    | Artifacts expected to change                             |
| revalidation-required       | boolean  | REQUIRED    | MUST be true when artifacts change                       |
| convergence-risk            | enum     | REQUIRED    | low, medium, high, unknown                               |
| recommended-next-action     | enum     | REQUIRED    | repair, replan, escalate, halt                           |
| produced-at                 | ISO 8601 | REQUIRED    | Timestamp                                                |

---

## 17.0 Agent State and Memory

Agent state and memory MUST follow ISL v2.2.

### 17.1 Agent Implementation State Schema

| Field                         | Type     | Required    | Description                                                                                                                                     |
| ----------------------------- | -------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| agent-implementation-state-id | string   | REQUIRED    | Unique state record                                                                                                                             |
| agent-implementation-id       | string   | REQUIRED    | Agent implementation                                                                                                                            |
| agent-role                    | enum     | REQUIRED    | Role implemented                                                                                                                                |
| implementation-version        | semver   | REQUIRED    | Implementation version                                                                                                                          |
| lifecycle-state               | enum     | REQUIRED    | registered, available, selected, context-bound, model-bound, invoked, output-produced, output-validated, completed, failed, escalated, disabled |
| active-invocation-id          | string   | CONDITIONAL | Active invocation if any                                                                                                                        |
| last-health-check-id          | string   | CONDITIONAL | Health check record                                                                                                                             |
| updated-at                    | ISO 8601 | REQUIRED    | Last update timestamp                                                                                                                           |

### 17.2 Memory Rules

Agent implementations MUST NOT maintain hidden persistent memory that affects output without being represented in platform state.

Reusable examples, templates, or learned heuristics MUST be versioned as configuration, templates, or model behavior.

Agent invocation memory MUST be associated with context packages, invocation records, and outputs.

---

## 18.0 Agent Traceability

Agent implementation activity MUST be traceable.

### 18.1 Required Traceability Links

The platform MUST record links between:

* construction task and agent invocation
* agent invocation and agent implementation
* agent invocation and context package
* agent invocation and model invocation
* agent implementation and output contract
* agent output and canonical entities
* artifact candidates and agent output
* repair proposal and failed validation result
* review finding and reviewed output
* agent output validation result and agent output

### 18.2 Traceability Rules

A task MUST NOT be marked completed if required agent traceability is missing.

An artifact candidate MUST NOT be admitted if its producing agent output is untraceable.

Agent traceability records MUST remain queryable after execution completion.

---

## 19.0 Agent Observability

Agent implementations MUST emit telemetry consistent with ISL v2.6.

### 19.1 Required Agent Telemetry Events

Agent implementations SHOULD emit:

* agent-implementation-selected
* agent-context-bound
* agent-model-bound
* agent-invocation-started
* agent-output-produced
* agent-output-validation-passed
* agent-output-validation-failed
* agent-artifact-candidate-produced
* agent-repair-proposal-produced
* agent-invocation-completed
* agent-invocation-failed
* agent-invocation-escalated

### 19.2 Agent Metrics

The platform SHOULD collect:

* invocation count by agent implementation
* output validation pass rate
* output validation failure rate
* average invocation duration
* retry rate
* escalation rate
* artifact candidate acceptance rate
* repair proposal success rate after revalidation

### 19.3 Observability Rules

Agent telemetry MUST include agent-implementation-id and agent-invocation-id when available.

Agent telemetry MUST include correlation-id.

Telemetry MUST NOT expose restricted context unless authorized.

---

## 20.0 Agent Testing Requirements

Agent implementations MUST be testable before use in autonomous execution.

### 20.1 Required Test Categories

| Test Category       | Purpose                                                      |
| ------------------- | ------------------------------------------------------------ |
| manifest-validation | Verify manifest completeness and correctness                 |
| context-validation  | Verify required and prohibited context behavior              |
| contract-validation | Verify output contract conformance                           |
| fixture-output      | Verify outputs from known context fixtures                   |
| invalid-output      | Verify malformed output rejection                            |
| role-boundary       | Verify prohibited actions are rejected                       |
| model-binding       | Verify model capability requirements and routing integration |
| artifact-candidate  | Verify artifact candidate structure                          |
| repair-scenario     | Verify repair proposals for validation failures              |
| telemetry           | Verify telemetry emission and correlation                    |
| traceability        | Verify traceability links are created                        |

### 20.2 Testing Rules

An agent implementation MUST NOT be marked active unless manifest-validation and contract-validation pass.

Artifact-producing agents MUST pass artifact-candidate tests.

Repair Analyst implementations MUST pass repair-scenario tests.

Security Validator implementations MUST pass policy and sensitive-context tests.

Experimental agents MAY have reduced test coverage only when governance permits experimental use.

---

## 21.0 Agent Health and Readiness

The platform MUST know whether an agent implementation can receive work.

### 21.1 Agent Health Check Schema

| Field                        | Type     | Required    | Description                              |
| ---------------------------- | -------- | ----------- | ---------------------------------------- |
| agent-health-check-id        | string   | REQUIRED    | Unique health check                      |
| agent-implementation-id      | string   | REQUIRED    | Agent checked                            |
| status                       | enum     | REQUIRED    | healthy, degraded, unavailable, disabled |
| manifest-valid               | boolean  | REQUIRED    | Whether manifest is valid                |
| required-templates-available | boolean  | REQUIRED    | Whether templates are available          |
| validators-available         | boolean  | REQUIRED    | Whether validators are available         |
| model-capabilities-available | boolean  | CONDITIONAL | Whether required models are routable     |
| last-error-id                | string   | CONDITIONAL | Last health error                        |
| checked-at                   | ISO 8601 | REQUIRED    | Check timestamp                          |

### 21.2 Health Rules

An unavailable agent MUST NOT receive new work.

A degraded agent MAY receive work only when runtime policy permits it.

A failed health check for a required role MUST block execution admission or route to escalation.

---

## 22.0 Agent Deployment

Agent implementations may be deployed in-process, as worker modules, or as services.

### 22.1 Deployment Modes

| Mode               | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| in-process         | Agent implementation runs inside Execution Service or local host         |
| worker-hosted      | Agent implementation runs inside Agent Worker                            |
| service-hosted     | Agent implementation runs as independently deployable service            |
| sandboxed          | Agent implementation runs inside restricted execution environment        |
| external-extension | Agent implementation provided by an external plugin or extension service |

### 22.2 Deployment Rules

Local-first deployments MAY use in-process agents.

Distributed deployments SHOULD use worker-hosted or service-hosted agents.

Sandboxed deployment SHOULD be used for experimental or untrusted agent extensions.

External-extension agents MUST be governed and registered.

Deployment mode MUST NOT alter role contract obligations.

---

## 23.0 Agent Versioning and Compatibility

Agent implementations MUST be versioned.

### 23.1 Versioning Rules

Agent implementation versions MUST use semantic versioning.

A breaking change to output structure MUST increment the major version.

A change to prompt or instruction templates that affects output behavior SHOULD increment version.

A change to required context categories MUST be treated as compatibility-impacting.

Runtime records MUST capture the agent implementation version used for each invocation.

### 23.2 Compatibility Record Schema

| Field                      | Type     | Required    | Description                        |
| -------------------------- | -------- | ----------- | ---------------------------------- |
| compatibility-record-id    | string   | REQUIRED    | Unique compatibility record        |
| agent-implementation-id    | string   | REQUIRED    | Agent implementation               |
| implementation-version     | semver   | REQUIRED    | Version evaluated                  |
| supported-isl-versions     | array    | REQUIRED    | Supported ISL versions             |
| supported-output-contracts | array    | REQUIRED    | Supported output contract versions |
| compatibility-status       | enum     | REQUIRED    | compatible, warning, incompatible  |
| findings                   | array    | CONDITIONAL | Compatibility findings             |
| evaluated-at               | ISO 8601 | REQUIRED    | Evaluation timestamp               |

---

## 24.0 Agent Extension Implementation

New agent roles or implementations MAY be introduced through the extension model.

### 24.1 Extension Rules

An agent extension MUST provide a manifest.

An agent extension MUST define a role contract or reference an existing role contract.

An agent extension MUST declare required capabilities.

An agent extension MUST declare output contracts.

An agent extension MUST include tests.

An agent extension MUST NOT bypass model, governance, traceability, state, artifact, or validation boundaries.

High or Critical risk use of an extension agent MUST require governance approval.

---

## 25.0 Agent Security Requirements

Agent implementations must protect context, artifacts, and platform state.

### 25.1 Security Rules

Agent implementations MUST NOT receive secrets unless explicitly governed.

Agent implementations MUST treat specification content, artifact content, logs, and tool outputs as potentially untrusted.

Agent templates MUST distinguish platform instructions from untrusted context.

Agent outputs MUST be checked for prohibited actions, unsafe content, and policy violations.

Agent implementations MUST NOT bypass sandbox, repository, or model trust controls.

Restricted context MUST not be exposed in telemetry or logs.

### 25.2 Prompt and Context Injection Handling

If an agent detects instruction override attempts, secret exfiltration attempts, role changes, governance bypass requests, or output contract bypass attempts in context, the agent or context controller MUST record a security finding.

The invocation MUST be blocked, sanitized, or escalated according to governance policy.

---

## 26.0 Agent Error Classes

This section defines agent-implementation-specific errors.

| Error Class                      | Description                                                    | Default Handling             |
| -------------------------------- | -------------------------------------------------------------- | ---------------------------- |
| agent-manifest-missing           | Agent implementation has no manifest                           | disable                      |
| agent-manifest-invalid           | Manifest fails validation                                      | disable                      |
| agent-role-contract-mismatch     | Implementation does not match role contract                    | disable or escalate          |
| agent-required-context-missing   | Required context category missing                              | fail invocation              |
| agent-prohibited-context-present | Prohibited context included                                    | block and escalate           |
| agent-template-missing           | Required instruction template missing                          | fail invocation              |
| agent-model-binding-failed       | Required model capability cannot be bound                      | retry, fallback, or escalate |
| agent-output-contract-failed     | Output does not satisfy contract                               | retry or reject              |
| agent-output-role-violation      | Output exceeds role authority                                  | reject and escalate          |
| agent-artifact-candidate-invalid | Produced artifact candidate is incomplete or invalid           | reject                       |
| agent-repair-proposal-invalid    | Repair proposal is incomplete or unsafe                        | reject or escalate           |
| agent-traceability-missing       | Required traceability link missing                             | block downstream use         |
| agent-health-check-failed        | Agent health check failed                                      | mark degraded or unavailable |
| agent-version-incompatible       | Agent version incompatible with active ISL or contract version | disable or block             |
| agent-security-context-violation | Context or output violates security rule                       | block and escalate           |

### 26.1 Agent Error Record Schema

| Field                   | Type     | Required    | Description                                                        |
| ----------------------- | -------- | ----------- | ------------------------------------------------------------------ |
| agent-error-id          | string   | REQUIRED    | Unique error identifier                                            |
| error-class             | enum     | REQUIRED    | Error class                                                        |
| severity                | enum     | REQUIRED    | blocking, high, medium, low                                        |
| agent-implementation-id | string   | CONDITIONAL | Agent implementation affected                                      |
| agent-invocation-id     | string   | CONDITIONAL | Invocation affected                                                |
| task-id                 | string   | CONDITIONAL | Task affected                                                      |
| output-id               | string   | CONDITIONAL | Output affected                                                    |
| message                 | string   | REQUIRED    | Human-readable explanation                                         |
| default-handling        | enum     | REQUIRED    | disable, block, retry, fallback, reject, escalate, fail-invocation |
| required-action         | string   | REQUIRED    | Remediation required                                               |
| detected-at             | ISO 8601 | REQUIRED    | Detection timestamp                                                |

---

## 27.0 Agent Documentation Requirements

Each agent implementation SHOULD include documentation.

### 27.1 Required Documentation

| Document               | Purpose                                                     |
| ---------------------- | ----------------------------------------------------------- |
| Role Summary           | Describes role and implementation responsibility            |
| Manifest Reference     | Explains manifest fields                                    |
| Context Requirements   | Lists required, optional, and prohibited context categories |
| Output Contract Guide  | Describes supported outputs                                 |
| Template Guide         | Explains prompt/instruction templates                       |
| Testing Guide          | Explains fixture and contract tests                         |
| Failure Handling Guide | Explains common errors and recovery paths                   |
| Security Notes         | Explains sensitive context and injection risks              |

### 27.2 Documentation Rules

Documentation MUST identify supported ISL versions.

Documentation MUST identify output contract versions.

Documentation SHOULD include examples using non-sensitive fixtures.

---

## 28.0 Agent Implementation Conformance

### 28.1 Agent Package Conformance

An agent package conforms to ISL v3.4 if it:

* includes a valid manifest
* declares a role contract
* declares supported task types
* declares required model capabilities
* declares supported output contracts
* includes output validation
* includes tests or test coverage plan
* contains no provider credentials or secrets
* preserves required package structure or documented equivalent

### 28.2 Agent Runtime Conformance

An agent runtime conforms to ISL v3.4 if it:

* accepts standard runtime requests
* validates required context
* binds to models through the Model Integration Layer
* produces standard runtime responses
* validates outputs before downstream use
* records agent state
* records traceability links
* emits telemetry
* handles errors using defined error classes

### 28.3 Artifact-Producing Agent Conformance

An artifact-producing agent conforms to ISL v3.4 if it:

* produces artifact candidates rather than stable artifacts
* includes artifact type, content reference, source entity links, and validation expectations
* routes artifacts through repository admission
* requires deterministic validation where applicable
* preserves traceability to canonical entities

### 28.4 Repair Agent Conformance

A repair agent conforms to ISL v3.4 if it:

* consumes failed validation results and failed artifact references
* produces structured repair proposals
* declares affected artifacts
* declares revalidation requirements
* respects repair limits
* does not mark artifacts valid or stable

### 28.5 Platform Conformance

A platform conforms to ISL v3.4 if it:

* registers agent implementations through the Agent Registry
* invokes agents only through the Agent Orchestrator or authorized runtime path
* enforces role manifests and role contracts
* controls context packages
* routes model usage through the Model Integration Layer
* validates agent outputs
* prevents direct stable artifact mutation by agents
* records agent state, traceability, telemetry, and errors
* supports agent tests, health checks, versioning, and governance controls

---

## 29.0 Summary

ISL v3.4 defines the Agent Implementation Model for the IMHOTEP platform. It translates conceptual and normative agent roles into concrete implementation requirements for packages, manifests, registries, runtime interfaces, lifecycle states, context binding, prompt templates, model binding, output validation, artifact candidate creation, repair proposals, state, traceability, telemetry, testing, deployment, versioning, extensions, security, errors, and conformance.

This revision strengthens the original v3.4 by making agents implementable software components rather than conceptual participants. Agents remain the reasoning workforce of IMHOTEP, but they operate through controlled contracts, bounded context, model abstraction, validated outputs, deterministic follow-up, repository admission, governance, traceability, and observability.

A conforming IMHOTEP agent implementation MUST be role-bounded, context-controlled, model-abstracted, output-validated, traceable, observable, testable, governable, and unable to bypass deterministic platform controls.
