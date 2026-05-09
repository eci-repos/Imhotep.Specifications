# IMHOTEP Specification Language (ISL) v2.1

# The Agent Collaboration Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0
**Supersedes:** ISL r0 v2.1 The Agent Collaboration Model
**Document Type:** Agent Collaboration and Role Contract Specification

---

## 1.0 Scope

This document defines the Agent Collaboration Model for the IMHOTEP platform. It establishes the standard reasoning agent roles, agent lifecycle, agent input contracts, agent output contracts, collaboration patterns, handoff rules, coordination mechanisms, failure modes, reliability controls, governance constraints, traceability requirements, and conformance obligations required for agent-based autonomous software construction.

This document applies to reasoning agents operating under the IMHOTEP platform architecture defined in ISL v2.0. These agents participate in specification interpretation, architecture planning, construction planning, implementation generation, repair analysis, test generation, review, security validation, and deployment preparation. Agents do not operate independently of the platform. They are invoked by the runtime or orchestrator, receive structured context, produce structured outputs, and remain subject to deterministic validation, traceability, and governance controls.

This document defines:

* standard agent roles
* agent role contracts
* agent lifecycle states
* agent invocation inputs
* agent output contracts
* collaboration and handoff rules
* multi-agent review behavior
* conflict resolution
* agent failure handling
* context boundaries
* governance controls
* traceability requirements
* telemetry requirements
* conformance requirements

---

## 2.0 Purpose of the Agent Collaboration Model

The purpose of the Agent Collaboration Model is to define how specialized reasoning agents cooperate within the IMHOTEP platform to perform autonomous construction activities. The platform decomposes work into construction tasks, and many of those tasks require reasoning, interpretation, synthesis, review, or repair. These reasoning responsibilities are assigned to role-specific agents.

This document converts agent collaboration from a conceptual design into an implementable contract. Each agent role must have defined responsibilities, required inputs, expected outputs, failure behavior, and collaboration boundaries. Without these contracts, agent collaboration would depend on informal prompt design or ad hoc orchestration, which would not be sufficient for enterprise reliability.

The Agent Collaboration Model exists to ensure that:

* agents operate as bounded platform components
* each agent role has explicit responsibilities
* agent inputs are structured and traceable
* agent outputs are schema-validatable
* agent handoffs are recorded and controlled
* agent disagreements are resolved through defined procedures
* agent failures trigger retry, fallback, repair, escalation, or halt behavior
* deterministic tools remain authoritative for objective validation
* governance controls apply to model usage and agent decisions
* agent actions remain observable and auditable

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the Agent Collaboration Model. Agent collaboration depends on canonical specification entities, construction planning, execution runtime behavior, tool validation, traceability, governance, and platform architecture.

| Reference | Purpose                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------- |
| ISL v0.0  | Corpus-level principles, conformance language, and system integrity rules                      |
| ISL v1.1  | Canonical semantic model and specification entity graph                                        |
| ISL v1.2  | Execution phases, runtime behavior, validation, repair, and completion                         |
| ISL v1.4  | Traceability records, decision records, model invocation traceability                          |
| ISL v1.5  | Construction task graph, task types, role mapping, and planning handoff                        |
| ISL v1.6  | Tool integration, deterministic validation, findings, and repair feedback                      |
| ISL v1.7  | Governance roles, approval gates, waivers, overrides, and runtime governance checks            |
| ISL v2.0  | Platform architecture, Agent Orchestrator, Construction Engine, and model abstraction boundary |
| ISL v2.5  | Model integration and abstraction layer                                                        |
| ISL v2.6  | Observability and telemetry model                                                              |
| RFC 2119  | Normative terminology                                                                          |

---

## 4.0 Terms and Definitions

This section defines terminology specific to agent collaboration. These terms are used by the Agent Orchestrator, Construction Engine, Execution Runtime, Governance Engine, Model Integration Layer, Traceability Engine, and Observability components.

**Agent**
A role-bounded reasoning component invoked by the platform to perform interpretation, analysis, synthesis, generation, review, repair, validation support, or deployment preparation.

**Agent Role**
A defined responsibility category assigned to an agent, such as Specification Interpreter, Architecture Planner, Implementation Generator, or Repair Analyst.

**Agent Invocation**
A structured request from the platform to an agent asking it to perform a task.

**Agent Output**
A structured response produced by an agent and returned to the platform for validation, routing, artifact creation, repair, review, or governance action.

**Agent Handoff**
The controlled transfer of structured output from one agent role to another agent role, task, runtime phase, or subsystem.

**Agent Collaboration Session**
A coordinated sequence of agent invocations associated with a construction task, repair cycle, review activity, or execution phase.

**Agent Context Package**
The structured contextual information provided to an agent for a specific invocation.

**Agent Contract**
The role-specific definition of required inputs, permitted outputs, responsibilities, prohibited actions, and failure behavior.

**Agent Failure**
A condition in which an agent cannot produce a valid, usable, governed, or schema-compliant output.

**Cross-Agent Review**
A collaboration pattern in which one agent reviews, validates, critiques, or challenges the output of another agent before platform acceptance.

---

## 5.0 Agent Collaboration Design Principles

This section defines the principles that govern agent collaboration. These principles ensure that agents behave as controlled components within an enterprise platform rather than independent autonomous actors.

### 5.1 Role-Bounded Reasoning

Each agent MUST operate within the responsibilities of its assigned role. An agent MUST NOT assume authority outside its role contract unless explicitly authorized by the runtime and governance rules.

Role boundaries reduce ambiguity, prevent uncontrolled behavior, and make agent outputs easier to validate.

### 5.2 Structured Inputs and Outputs

Agents MUST receive structured inputs and MUST produce structured outputs. Free-form reasoning MAY be included as rationale, but operational outputs MUST conform to the applicable output schema.

Structured contracts are required so that downstream platform components can validate, route, trace, and act on agent outputs.

### 5.3 Runtime-Mediated Collaboration

Agents MUST collaborate through the Agent Orchestrator and Execution Runtime. Agents MUST NOT directly invoke other agents, mutate platform state, modify artifacts, bypass governance, or invoke deterministic tools unless mediated by authorized platform interfaces.

Runtime mediation ensures that collaboration remains traceable, governed, and aligned with the construction task graph.

### 5.4 Deterministic Validation Boundary

Agent outputs MUST NOT be treated as objective proof of correctness. Where objective validation is applicable, deterministic tools governed by ISL v1.6 MUST validate artifacts or claims before acceptance.

Agents may propose, generate, interpret, or repair. Deterministic tools validate.

### 5.5 Traceable Reasoning

Every agent invocation and significant agent output MUST be traceable to the task, specification entities, context package, model invocation, output record, and downstream decisions it affects.

Traceability ensures that platform behavior remains explainable and auditable.

### 5.6 Governed Model Usage

Agents obtain reasoning capability through model providers governed by the model abstraction boundary. Agent collaboration MUST obey model provider restrictions, context-sharing rules, risk-tier controls, and governance approvals.

An agent MUST NOT bypass model governance restrictions.

### 5.7 Failure Is Explicit

Agent failures MUST be detected, classified, recorded, and handled through defined retry, fallback, review, repair, escalation, or halt behavior.

The platform MUST NOT silently accept incomplete, malformed, contradictory, or unsupported agent outputs.

---

## 6.0 Standard Agent Roles

This section defines the standard agent roles used by IMHOTEP. These roles correspond to major reasoning responsibilities across the autonomous construction lifecycle. Additional roles MAY be introduced through approved extensions, but conforming implementations MUST support the standard roles required by their claimed capability level.

### 6.1 Standard Role Set

| Agent Role                | Primary Responsibility                                                           |
| ------------------------- | -------------------------------------------------------------------------------- |
| Specification Interpreter | Interprets canonical specification content for execution context                 |
| Architecture Planner      | Produces architectural interpretation and service/data/interface design guidance |
| Construction Planner      | Supports task graph generation, dependency reasoning, and plan adaptation        |
| Implementation Generator  | Generates implementation artifacts from task context                             |
| Repair Analyst            | Analyzes validation failures and proposes repair actions                         |
| Test Generator            | Generates test artifacts and validation scenarios                                |
| Review Agent              | Reviews agent outputs, artifacts, and reasoning consistency                      |
| Security Validator        | Reviews security, policy, access, data protection, and compliance implications   |
| Deployment Preparer       | Produces deployment preparation guidance and artifacts                           |

### 6.2 Role Support Levels

A platform MAY support agent roles progressively based on implementation maturity, but a platform claiming full autonomous construction capability MUST support all standard roles.

| Capability Level | Required Roles                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------- |
| advisory         | Specification Interpreter, Review Agent                                                                       |
| planning         | Specification Interpreter, Architecture Planner, Construction Planner, Review Agent                           |
| generation       | Specification Interpreter, Architecture Planner, Construction Planner, Implementation Generator, Review Agent |
| repair-capable   | generation roles plus Repair Analyst and Test Generator                                                       |
| full-autonomous  | all standard roles                                                                                            |

### 6.3 Role Binding Rules

Each agent invocation MUST identify exactly one primary agent role.

An agent MAY support multiple roles only if the platform treats each role invocation as a separate role-bound execution.

A role-bound invocation MUST use the input and output contract for that role.

---

## 7.0 Agent Lifecycle

This section defines the lifecycle of an agent invocation. The lifecycle provides a consistent structure for assigning work, assembling context, invoking the model layer, validating output, recording traceability, and returning results to the runtime.

### 7.1 Lifecycle States

| State              | Meaning                                                                       |
| ------------------ | ----------------------------------------------------------------------------- |
| requested          | Runtime or orchestrator requested an agent invocation                         |
| context-assembled  | Context package has been assembled                                            |
| governance-checked | Governance checks required before invocation have passed or returned decision |
| model-invoked      | Model request has been submitted through model abstraction layer              |
| output-received    | Raw or structured output received from model/provider                         |
| output-normalized  | Output converted into expected agent output schema                            |
| output-validated   | Output passed schema and role-contract validation                             |
| completed          | Invocation completed successfully                                             |
| failed             | Invocation failed and cannot proceed automatically                            |
| escalated          | Invocation requires governance or human intervention                          |
| cancelled          | Invocation was cancelled before completion                                    |

### 7.2 Lifecycle Rules

An agent invocation MUST NOT proceed to model-invoked state until required context assembly and governance checks complete.

An agent invocation MUST NOT be marked completed until output validation succeeds.

An agent invocation with invalid output MUST enter failed or escalated state.

Lifecycle transitions MUST be recorded in telemetry.

Significant lifecycle transitions MUST be traceable to task and execution state.

---

## 8.0 Agent Invocation Contract

This section defines the common invocation contract used for all agent roles. Role-specific contracts extend this base contract.

The invocation contract ensures that every agent receives the task, specification, governance, traceability, and artifact context required to operate within platform boundaries.

### 8.1 Agent Invocation Request Schema

| Field                        | Type     | Required    | Description                                                                        |
| ---------------------------- | -------- | ----------- | ---------------------------------------------------------------------------------- |
| agent-invocation-id          | string   | REQUIRED    | Unique invocation identifier                                                       |
| agent-role                   | enum     | REQUIRED    | Agent role being invoked                                                           |
| task-id                      | string   | REQUIRED    | Construction task associated with invocation                                       |
| execution-graph-id           | string   | CONDITIONAL | Required during active execution                                                   |
| plan-id                      | string   | CONDITIONAL | Required when invocation relates to construction planning                          |
| specification-id             | string   | REQUIRED    | System identifier                                                                  |
| specification-version        | semver   | REQUIRED    | Specification version                                                              |
| canonical-model-version      | semver   | REQUIRED    | Canonical model version                                                            |
| source-entity-ids            | array    | REQUIRED    | Specification entities relevant to invocation                                      |
| invocation-purpose           | enum     | REQUIRED    | interpret, plan, generate, review, repair, test, security-validate, deploy-prepare |
| context-package-id           | string   | REQUIRED    | Context package provided to agent                                                  |
| expected-output-contract     | string   | REQUIRED    | Output contract identifier                                                         |
| governance-check-id          | string   | CONDITIONAL | Required when invocation is governance-controlled                                  |
| model-selection-requirements | object   | CONDITIONAL | Required model capabilities or restrictions                                        |
| timeout-seconds              | integer  | REQUIRED    | Maximum allowed invocation duration                                                |
| requested-at                 | ISO 8601 | REQUIRED    | Invocation request time                                                            |
| requested-by                 | string   | REQUIRED    | Runtime, orchestrator, or subsystem requesting invocation                          |

### 8.2 Invocation Rules

Every agent invocation MUST reference a task-id.

Every agent invocation MUST identify source specification entities.

Every agent invocation MUST use a declared expected-output-contract.

Every agent invocation MUST use the model abstraction boundary defined by ISL v2.0 and ISL v2.5.

An agent invocation MUST NOT include unrestricted platform state unless context rules explicitly allow it.

---

## 9.0 Agent Context Package

This section defines the context package provided to an agent. Context packages are critical because agent quality depends on relevant information, but excessive or uncontrolled context creates security, privacy, cost, and reliability risks.

The platform MUST assemble context according to task need, role contract, governance rules, and model provider constraints.

### 9.1 Context Package Schema

| Field                           | Type     | Required    | Description                                 |
| ------------------------------- | -------- | ----------- | ------------------------------------------- |
| context-package-id              | string   | REQUIRED    | Unique context package identifier           |
| agent-role                      | enum     | REQUIRED    | Agent role receiving context                |
| task-id                         | string   | REQUIRED    | Related task                                |
| specification-id                | string   | REQUIRED    | System identifier                           |
| specification-version           | semver   | REQUIRED    | Specification version                       |
| included-entities               | array    | REQUIRED    | Canonical entity identifiers included       |
| included-artifacts              | array    | CONDITIONAL | Artifact identifiers included or summarized |
| included-validation-results     | array    | CONDITIONAL | Validation results included                 |
| included-repair-records         | array    | CONDITIONAL | Repair records included                     |
| included-governance-constraints | array    | CONDITIONAL | Governance or policy constraints included   |
| included-prior-agent-outputs    | array    | CONDITIONAL | Prior agent outputs included                |
| exclusions                      | array    | CONDITIONAL | Relevant context intentionally excluded     |
| sensitivity-classification      | enum     | REQUIRED    | public, internal, confidential, restricted  |
| context-size-metrics            | object   | CONDITIONAL | Token, document, artifact, or byte counts   |
| assembled-at                    | ISO 8601 | REQUIRED    | Time assembled                              |
| assembled-by                    | string   | REQUIRED    | Component assembling context                |

### 9.2 Context Inclusion Rules

The context package MUST include the canonical entities required to satisfy the task.

The context package SHOULD include only the minimum relevant artifacts and records needed by the role.

The context package MUST include applicable governance constraints when they affect output.

The context package MUST include failed validation results when invoking the Repair Analyst.

The context package MUST include security policies when invoking the Security Validator.

The context package MUST NOT include secrets unless explicitly permitted by governance and task requirements.

### 9.3 Context Exclusion Rules

Context exclusions MUST be recorded when relevant information is withheld due to governance, sensitivity, provider restriction, or risk tier.

An agent output MUST NOT be accepted if the context package omitted required information for the role contract.

### 9.4 Context Integrity

A context package MUST be immutable after invocation begins.

Corrections require creation of a new context package and new invocation.

---

## 10.0 Agent Output Contract

This section defines the common output contract used by all agent roles. Role-specific outputs extend this base output schema.

The output contract ensures that agent responses can be normalized, validated, traced, reviewed, and routed by platform components.

### 10.1 Base Agent Output Schema

| Field                             | Type     | Required    | Description                                                                                                                           |
| --------------------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| agent-output-id                   | string   | REQUIRED    | Unique output identifier                                                                                                              |
| agent-invocation-id               | string   | REQUIRED    | Invocation that produced output                                                                                                       |
| agent-role                        | enum     | REQUIRED    | Role producing output                                                                                                                 |
| task-id                           | string   | REQUIRED    | Task associated with output                                                                                                           |
| output-type                       | enum     | REQUIRED    | interpretation, plan-fragment, artifact-content, repair-proposal, test-plan, review-finding, security-finding, deployment-preparation |
| summary                           | string   | REQUIRED    | Human-readable summary                                                                                                                |
| structured-output                 | object   | REQUIRED    | Role-specific structured payload                                                                                                      |
| produced-artifact-candidates      | array    | CONDITIONAL | Candidate artifacts proposed or generated                                                                                             |
| referenced-entities               | array    | REQUIRED    | Specification entities referenced                                                                                                     |
| referenced-artifacts              | array    | CONDITIONAL | Artifacts referenced                                                                                                                  |
| assumptions                       | array    | CONDITIONAL | Assumptions used by agent                                                                                                             |
| constraints-applied               | array    | CONDITIONAL | Governance, policy, or task constraints applied                                                                                       |
| confidence                        | enum     | REQUIRED    | high, medium, low, unknown                                                                                                            |
| requires-review                   | boolean  | REQUIRED    | Whether review is required before use                                                                                                 |
| requires-deterministic-validation | boolean  | REQUIRED    | Whether deterministic validation is required                                                                                          |
| produced-at                       | ISO 8601 | REQUIRED    | Output timestamp                                                                                                                      |
| output-status                     | enum     | REQUIRED    | valid, invalid, partial, failed, escalated                                                                                            |

### 10.2 Output Rules

An agent output MUST conform to the declared expected-output-contract.

An agent output MUST reference the invocation that produced it.

An agent output MUST reference the task it supports.

An agent output MUST identify whether deterministic validation is required.

An output containing artifact content MUST NOT be accepted as a stable artifact until it enters the Artifact Repository and passes required validation.

An output marked partial MUST NOT be consumed as completed work unless the downstream contract explicitly accepts partial output.

---

## 11.0 Agent Output Validation

This section defines how agent outputs are validated before downstream use. Agent output validation is distinct from deterministic artifact validation. It checks whether the output satisfies the expected structure, role contract, task relevance, and governance constraints.

### 11.1 Required Validation Checks

The Agent Orchestrator MUST validate:

* output schema compliance
* required fields
* role compatibility
* task compatibility
* referenced entity validity
* referenced artifact validity
* prohibited content or unsupported actions
* governance constraint compliance
* output completeness
* deterministic validation requirement flag
* structured-output payload validity

### 11.2 Output Validation Result Schema

| Field                      | Type     | Required    | Description                                                           |
| -------------------------- | -------- | ----------- | --------------------------------------------------------------------- |
| agent-output-validation-id | string   | REQUIRED    | Unique validation identifier                                          |
| agent-output-id            | string   | REQUIRED    | Output evaluated                                                      |
| agent-invocation-id        | string   | REQUIRED    | Invocation associated with output                                     |
| validation-outcome         | enum     | REQUIRED    | passed, failed, warning, escalated                                    |
| findings                   | array    | CONDITIONAL | Required for failed, warning, or escalated outcome                    |
| validated-at               | ISO 8601 | REQUIRED    | Time validation completed                                             |
| validated-by               | string   | REQUIRED    | Component performing validation                                       |
| next-action                | enum     | REQUIRED    | accept, retry, request-revision, cross-agent-review, escalate, reject |

### 11.3 Validation Rules

An output that fails schema validation MUST NOT be accepted.

An output that references undefined entities MUST NOT be accepted.

An output that proposes actions outside the role contract MUST be rejected or escalated.

An output that conflicts with governance constraints MUST be escalated or rejected according to governance response.

A valid agent output MAY still require deterministic validation before artifact acceptance.

---

## 12.0 Communication and Handoff Model

This section defines how agents communicate indirectly through the platform. Agents collaborate by producing structured outputs that are stored, validated, traced, and then supplied to other agents or subsystems as context.

Agents MUST NOT communicate through hidden channels, direct unmanaged calls, unrecorded messages, or implicit shared memory.

### 12.1 Handoff Record Schema

| Field              | Type     | Required    | Description                                                                          |
| ------------------ | -------- | ----------- | ------------------------------------------------------------------------------------ |
| handoff-id         | string   | REQUIRED    | Unique handoff identifier                                                            |
| from-agent-role    | enum     | REQUIRED    | Role producing output                                                                |
| to-agent-role      | enum     | CONDITIONAL | Role receiving output, when agent-to-agent                                           |
| from-task-id       | string   | REQUIRED    | Source task                                                                          |
| to-task-id         | string   | CONDITIONAL | Target task, when known                                                              |
| agent-output-id    | string   | REQUIRED    | Output being handed off                                                              |
| handoff-type       | enum     | REQUIRED    | sequential, review, repair, validation-support, escalation-support, planning-support |
| handoff-purpose    | string   | REQUIRED    | Why handoff is occurring                                                             |
| required           | boolean  | REQUIRED    | Whether handoff is required for downstream task                                      |
| accepted-by-target | boolean  | CONDITIONAL | Whether target accepted handoff                                                      |
| created-at         | ISO 8601 | REQUIRED    | Handoff creation time                                                                |
| created-by         | string   | REQUIRED    | Orchestrator or runtime component creating handoff                                   |

### 12.2 Handoff Rules

Every agent-to-agent handoff MUST be mediated by the Agent Orchestrator.

A handoff MUST reference a validated agent output.

A required handoff MUST be satisfied before the dependent task can proceed.

A handoff rejected by the target role MUST produce a collaboration finding or escalation.

Handoffs MUST be traceable.

---

## 13.0 Collaboration Patterns

This section defines standard collaboration patterns used by agents. Collaboration patterns describe how agent roles cooperate across construction tasks and execution phases.

A platform MAY implement additional patterns, but standard patterns MUST preserve the control, traceability, and validation rules defined here.

### 13.1 Sequential Handoff

Sequential handoff occurs when one agent’s output becomes the input to a later agent. For example, a Specification Interpreter output may feed an Architecture Planner, and an Architecture Planner output may feed an Implementation Generator.

Sequential handoff MUST preserve the upstream output, the downstream context package, and the handoff record.

### 13.2 Review and Challenge

Review and challenge occurs when one agent reviews another agent’s output and produces findings, objections, or validation support. For example, a Review Agent may evaluate an Implementation Generator output before artifact acceptance.

Review outputs MUST identify the reviewed output and any findings.

### 13.3 Repair Collaboration

Repair collaboration occurs when validation failure causes the Repair Analyst to analyze findings and provide repair guidance to the Implementation Generator.

Repair collaboration MUST include the failed validation result, failed artifact, prior repair history, and applicable repair limits.

### 13.4 Security Review Collaboration

Security review collaboration occurs when the Security Validator evaluates specification entities, planned tasks, generated artifacts, interface contracts, data handling, or deployment outputs.

Security review collaboration MUST include applicable Policy entities and governance constraints.

### 13.5 Planning Feedback Collaboration

Planning feedback collaboration occurs when execution, review, security validation, or repair feedback requires the Construction Planner to adapt the construction plan.

Planning feedback collaboration MUST preserve the feedback source and trace resulting plan changes.

### 13.6 Deployment Readiness Collaboration

Deployment readiness collaboration occurs when validated artifacts, infrastructure expectations, operational requirements, security findings, and governance constraints are supplied to the Deployment Preparer.

Deployment readiness collaboration MUST preserve deployment-related traceability to Infrastructure and Policy entities.

---

## 14.0 Agent Role Contracts

This section defines the common structure of an agent role contract. Specific role contracts are defined in later sections.

Each role contract defines responsibility, inputs, outputs, prohibited actions, validation requirements, failure behavior, and collaboration relationships.

### 14.1 Role Contract Schema

| Field                   | Type   | Required    | Description                            |
| ----------------------- | ------ | ----------- | -------------------------------------- |
| role-name               | enum   | REQUIRED    | Agent role name                        |
| role-purpose            | string | REQUIRED    | Purpose of the role                    |
| responsibilities        | array  | REQUIRED    | Actions the role may perform           |
| required-inputs         | array  | REQUIRED    | Required context and records           |
| permitted-outputs       | array  | REQUIRED    | Output types the role may produce      |
| prohibited-actions      | array  | REQUIRED    | Actions the role MUST NOT perform      |
| output-contracts        | array  | REQUIRED    | Applicable output contract identifiers |
| validation-requirements | array  | REQUIRED    | Required output validation checks      |
| downstream-consumers    | array  | CONDITIONAL | Roles or subsystems consuming outputs  |
| failure-modes           | array  | REQUIRED    | Role-specific failure conditions       |
| escalation-conditions   | array  | REQUIRED    | Conditions requiring escalation        |

### 14.2 Role Contract Rules

Every standard agent role MUST have a role contract.

An agent invocation MUST be validated against its role contract.

An output outside the role’s permitted outputs MUST be rejected or escalated.

A role contract MUST NOT grant authority to bypass runtime, tool, traceability, or governance controls.

---

## 15.0 Specification Interpreter Contract

The Specification Interpreter converts canonical specification content into execution-oriented interpretation records. It clarifies how specification entities, relationships, assumptions, constraints, and ambiguity conditions should be understood for planning and execution.

The Specification Interpreter is typically invoked early in execution and may also be invoked during impact analysis or specification change review.

### 15.1 Responsibilities

The Specification Interpreter MAY:

* interpret canonical entities for execution context
* identify ambiguity in specification content
* identify execution-relevant assumptions
* identify policy constraints affecting interpretation
* produce interpretation records
* recommend clarification needs

### 15.2 Required Inputs

| Input                        | Required    |
| ---------------------------- | ----------- |
| Canonical model version      | YES         |
| Project entity               | YES         |
| Source entity identifiers    | YES         |
| Relevant Context entities    | YES         |
| Readiness record             | YES         |
| Governance constraints       | CONDITIONAL |
| Prior interpretation records | CONDITIONAL |

### 15.3 Permitted Outputs

| Output Type        | Description                              |
| ------------------ | ---------------------------------------- |
| interpretation     | Execution-oriented interpretation record |
| review-finding     | Ambiguity or clarification finding       |
| escalation-support | Support record for unresolved ambiguity  |

### 15.4 Structured Output Payload

| Field                   | Type    | Required    | Description                        |
| ----------------------- | ------- | ----------- | ---------------------------------- |
| interpreted-entities    | array   | REQUIRED    | Entities interpreted               |
| execution-assumptions   | array   | CONDITIONAL | Assumptions relevant to execution  |
| ambiguity-findings      | array   | CONDITIONAL | Ambiguities detected               |
| policy-constraints      | array   | CONDITIONAL | Policies affecting interpretation  |
| clarification-required  | boolean | REQUIRED    | Whether clarification is required  |
| recommended-next-action | enum    | REQUIRED    | proceed, review, clarify, escalate |

### 15.5 Prohibited Actions

The Specification Interpreter MUST NOT:

* generate implementation artifacts
* modify the canonical model
* approve readiness transitions
* waive ambiguity findings
* bypass governance approval

### 15.6 Failure Modes

| Failure Mode                                  | Handling                                      |
| --------------------------------------------- | --------------------------------------------- |
| required context missing                      | fail invocation and request corrected context |
| ambiguity cannot be resolved                  | escalate                                      |
| output schema invalid                         | retry or request revision                     |
| interpretation conflicts with canonical model | reject and escalate                           |

---

## 16.0 Architecture Planner Contract

The Architecture Planner interprets the canonical model into architectural structure. It reasons about service boundaries, data ownership, interface relationships, deployment implications, and architectural constraints.

The Architecture Planner informs the Planning Engine and may produce architecture decisions that guide task generation and artifact generation.

### 16.1 Responsibilities

The Architecture Planner MAY:

* propose service decomposition guidance
* identify data ownership boundaries
* identify interface responsibilities
* identify architectural dependencies
* identify architectural risks
* recommend artifact grouping or module structure
* produce architecture decision records

### 16.2 Required Inputs

| Input                                                          | Required    |
| -------------------------------------------------------------- | ----------- |
| Canonical model version                                        | YES         |
| Service, DataEntity, Interface, Workflow, Requirement entities | YES         |
| Context entities                                               | CONDITIONAL |
| Policy entities                                                | CONDITIONAL |
| Prior interpretation records                                   | YES         |
| Governance constraints                                         | CONDITIONAL |

### 16.3 Permitted Outputs

| Output Type        | Description                                |
| ------------------ | ------------------------------------------ |
| plan-fragment      | Architectural planning guidance            |
| review-finding     | Architectural issue or risk                |
| interpretation     | Architectural interpretation               |
| escalation-support | Support record for architecture escalation |

### 16.4 Structured Output Payload

| Field                     | Type  | Required    | Description                       |
| ------------------------- | ----- | ----------- | --------------------------------- |
| service-boundary-findings | array | CONDITIONAL | Findings about service scope      |
| data-ownership-findings   | array | CONDITIONAL | Findings about data ownership     |
| interface-findings        | array | CONDITIONAL | Findings about interfaces         |
| dependency-findings       | array | CONDITIONAL | Architectural dependencies        |
| architecture-decisions    | array | CONDITIONAL | Proposed or confirmed decisions   |
| risks                     | array | CONDITIONAL | Architectural risks               |
| recommended-plan-updates  | array | CONDITIONAL | Suggested planning actions        |
| recommended-next-action   | enum  | REQUIRED    | proceed, review, replan, escalate |

### 16.5 Prohibited Actions

The Architecture Planner MUST NOT:

* create executable tasks directly without Planning Engine mediation
* generate implementation artifacts
* override canonical requirements
* approve architecture review gates
* bypass governance constraints

### 16.6 Failure Modes

| Failure Mode                            | Handling                            |
| --------------------------------------- | ----------------------------------- |
| conflicting architecture interpretation | cross-agent review                  |
| missing service/data/interface context  | fail and request context correction |
| architecture violates policy            | escalate to Governance Engine       |
| output schema invalid                   | retry or request revision           |

---

## 17.0 Construction Planner Contract

The Construction Planner supports creation, validation, and adaptation of the Construction Task Graph. It reasons about task decomposition, dependency ordering, verification coverage, repair insertion, and impact-based replanning.

The Construction Planner assists the Planning Engine but does not replace Planning Engine validation.

### 17.1 Responsibilities

The Construction Planner MAY:

* recommend task decomposition
* recommend dependency relationships
* recommend verification task insertion
* identify planning gaps
* support critical path analysis
* support plan adaptation
* identify affected tasks after change impact analysis
* produce planning findings

### 17.2 Required Inputs

| Input                   | Required    |
| ----------------------- | ----------- |
| Canonical model version | YES         |
| Construction Task Graph | CONDITIONAL |
| Planning configuration  | YES         |
| Traceability state      | YES         |
| Impact analysis result  | CONDITIONAL |
| Governance constraints  | CONDITIONAL |
| Tool capability catalog | CONDITIONAL |

### 17.3 Permitted Outputs

| Output Type        | Description                           |
| ------------------ | ------------------------------------- |
| plan-fragment      | Task or dependency recommendation     |
| review-finding     | Planning gap or defect                |
| escalation-support | Support for unresolved planning issue |

### 17.4 Structured Output Payload

| Field                         | Type  | Required    | Description                                   |
| ----------------------------- | ----- | ----------- | --------------------------------------------- |
| proposed-tasks                | array | CONDITIONAL | Candidate tasks                               |
| proposed-dependencies         | array | CONDITIONAL | Candidate dependencies                        |
| verification-recommendations  | array | CONDITIONAL | Verification tasks or coverage                |
| planning-gaps                 | array | CONDITIONAL | Missing tasks or unresolved dependencies      |
| affected-task-recommendations | array | CONDITIONAL | Tasks affected by change                      |
| recommended-next-action       | enum  | REQUIRED    | proceed, revise-plan, validate-plan, escalate |

### 17.5 Prohibited Actions

The Construction Planner MUST NOT:

* mark a plan executable
* bypass Planning Engine validation
* bypass readiness restrictions
* directly execute tasks
* directly create artifacts

### 17.6 Failure Modes

| Failure Mode                       | Handling                |
| ---------------------------------- | ----------------------- |
| circular dependency proposed       | reject proposed output  |
| unresolved source entity reference | fail output validation  |
| missing verification coverage      | return planning finding |
| governance constraint conflict     | escalate                |

---

## 18.0 Implementation Generator Contract

The Implementation Generator produces candidate implementation artifacts based on construction tasks, canonical entities, architecture guidance, prior artifacts, and validation constraints.

The Implementation Generator is the primary artifact-producing agent, but its outputs remain candidates until repository admission, traceability association, and deterministic validation occur.

### 18.1 Responsibilities

The Implementation Generator MAY:

* generate source artifacts
* generate configuration artifacts
* generate schema artifacts
* generate interface implementation artifacts
* generate documentation artifacts
* modify artifacts during repair when instructed
* produce implementation rationale

### 18.2 Required Inputs

| Input                       | Required                                        |
| --------------------------- | ----------------------------------------------- |
| Task definition             | YES                                             |
| Source entity identifiers   | YES                                             |
| Relevant canonical entities | YES                                             |
| Architecture guidance       | CONDITIONAL                                     |
| Existing artifact context   | CONDITIONAL                                     |
| Validation criteria         | YES                                             |
| Governance constraints      | CONDITIONAL                                     |
| Repair proposal             | CONDITIONAL for repair-generated implementation |

### 18.3 Permitted Outputs

| Output Type      | Description                                            |
| ---------------- | ------------------------------------------------------ |
| artifact-content | Candidate artifact content                             |
| review-finding   | Implementation concern                                 |
| repair-proposal  | Minor corrective suggestion when repair context exists |

### 18.4 Structured Output Payload

| Field                             | Type    | Required    | Description                                                           |
| --------------------------------- | ------- | ----------- | --------------------------------------------------------------------- |
| artifact-candidates               | array   | REQUIRED    | Candidate artifacts produced                                          |
| artifact-type                     | enum    | REQUIRED    | source, config, schema, infrastructure, contract, test, documentation |
| source-entity-links               | array   | REQUIRED    | Specification entities implemented                                    |
| implementation-summary            | string  | REQUIRED    | Summary of generated implementation                                   |
| assumptions                       | array   | CONDITIONAL | Assumptions used                                                      |
| dependencies-introduced           | array   | CONDITIONAL | Dependencies introduced by artifact                                   |
| validation-expectations           | array   | REQUIRED    | Expected validations                                                  |
| requires-deterministic-validation | boolean | REQUIRED    | MUST be true for executable or structural artifacts                   |

### 18.5 Prohibited Actions

The Implementation Generator MUST NOT:

* mark artifacts valid
* write directly to stable artifact repository state without runtime mediation
* introduce behavior not traceable to specification entities or approved inference
* bypass security or policy constraints
* perform deployment authorization
* silently change requirements or policies

### 18.6 Failure Modes

| Failure Mode                             | Handling                                         |
| ---------------------------------------- | ------------------------------------------------ |
| output artifact lacks source links       | reject output                                    |
| artifact content missing or malformed    | request revision or retry                        |
| generated behavior exceeds specification | escalate or reject                               |
| generated artifact violates policy       | route to Security Validator or Governance Engine |
| repeated invalid outputs                 | escalate                                         |

---

## 19.0 Repair Analyst Contract

The Repair Analyst analyzes deterministic validation failures, test failures, policy findings, or runtime defects and proposes corrective action. It does not itself declare repaired artifacts valid.

Repair analysis is central to convergence, but it MUST remain bounded by repair termination rules defined in ISL v1.2 and v1.5.

### 19.1 Responsibilities

The Repair Analyst MAY:

* analyze validation findings
* identify likely root causes
* propose artifact corrections
* recommend repair scope
* recommend whether repair should proceed or escalate
* produce repair records
* identify non-converging repair patterns

### 19.2 Required Inputs

| Input                         | Required                |
| ----------------------------- | ----------------------- |
| Failed validation result      | YES                     |
| Failed artifact identifier    | YES                     |
| Failed task identifier        | YES                     |
| Tool findings                 | YES when tool-triggered |
| Test results                  | CONDITIONAL             |
| Prior repair records          | CONDITIONAL             |
| Source specification entities | YES                     |
| Repair limits                 | YES                     |
| Governance constraints        | CONDITIONAL             |

### 19.3 Permitted Outputs

| Output Type        | Description                       |
| ------------------ | --------------------------------- |
| repair-proposal    | Proposed correction and rationale |
| review-finding     | Repair risk or uncertainty        |
| escalation-support | Escalation record support         |

### 19.4 Structured Output Payload

| Field                   | Type    | Required    | Description                                              |
| ----------------------- | ------- | ----------- | -------------------------------------------------------- |
| failure-summary         | string  | REQUIRED    | Summary of failure                                       |
| likely-root-cause       | string  | CONDITIONAL | Root cause if determinable                               |
| repair-scope            | enum    | REQUIRED    | artifact, task, service, interface, schema, policy, plan |
| proposed-corrections    | array   | REQUIRED    | Proposed changes                                         |
| affected-artifacts      | array   | REQUIRED    | Artifacts expected to change                             |
| revalidation-required   | boolean | REQUIRED    | MUST be true when artifact changes                       |
| convergence-risk        | enum    | REQUIRED    | low, medium, high, unknown                               |
| recommended-next-action | enum    | REQUIRED    | repair, replan, escalate, halt                           |

### 19.5 Prohibited Actions

The Repair Analyst MUST NOT:

* mark repair successful without revalidation
* exceed repair iteration limits
* modify artifacts directly without runtime-mediated task assignment
* waive validation failures
* suppress tool findings
* bypass governance escalation

### 19.6 Failure Modes

| Failure Mode                    | Handling                   |
| ------------------------------- | -------------------------- |
| root cause cannot be determined | escalate or request review |
| repair would exceed limits      | escalate                   |
| repair would violate policy     | escalate                   |
| repeated same failure           | escalate after threshold   |
| repair proposal invalid schema  | retry or request revision  |

---

## 20.0 Test Generator Contract

The Test Generator produces test artifacts, test scenarios, validation support, and acceptance checks derived from Requirement, Workflow, Interface, Policy, and Validation entities.

The Test Generator supports validation coverage. It does not decide whether the system is accepted; test execution and policy evaluation determine outcomes.

### 20.1 Responsibilities

The Test Generator MAY:

* generate unit test candidates
* generate integration test candidates
* generate acceptance test candidates
* generate contract test candidates
* generate security test candidates when policy-driven
* map tests to Validation entities
* identify validation coverage gaps

### 20.2 Required Inputs

| Input                      | Required    |
| -------------------------- | ----------- |
| Requirement entities       | YES         |
| Validation entities        | YES         |
| Workflow entities          | CONDITIONAL |
| Interface entities         | CONDITIONAL |
| Policy entities            | CONDITIONAL |
| Generated artifacts        | CONDITIONAL |
| Test framework constraints | CONDITIONAL |
| Governance constraints     | CONDITIONAL |

### 20.3 Permitted Outputs

| Output Type      | Description                         |
| ---------------- | ----------------------------------- |
| test-plan        | Test plan or scenario set           |
| artifact-content | Candidate test artifacts            |
| review-finding   | Coverage gap or testability concern |

### 20.4 Structured Output Payload

| Field                   | Type  | Required    | Description                                                         |
| ----------------------- | ----- | ----------- | ------------------------------------------------------------------- |
| test-candidates         | array | REQUIRED    | Candidate tests or test artifacts                                   |
| validates               | array | REQUIRED    | Requirement, Policy, Workflow, Interface, or Validation IDs covered |
| test-type               | enum  | REQUIRED    | unit, integration, acceptance, contract, security, operational      |
| execution-requirements  | array | CONDITIONAL | Tool or environment requirements                                    |
| coverage-gaps           | array | CONDITIONAL | Missing validation coverage                                         |
| expected-evidence       | array | CONDITIONAL | Expected test result evidence                                       |
| recommended-next-action | enum  | REQUIRED    | proceed, add-tests, revise-requirement, escalate                    |

### 20.5 Prohibited Actions

The Test Generator MUST NOT:

* mark tests passed
* mark requirements satisfied
* waive missing validation coverage
* modify requirements to fit generated tests
* bypass deterministic test execution

### 20.6 Failure Modes

| Failure Mode                | Handling                             |
| --------------------------- | ------------------------------------ |
| requirement is not testable | return coverage finding              |
| missing validation entity   | return planning or readiness finding |
| unsupported test framework  | escalate or request tool capability  |
| generated test invalid      | retry or request revision            |

---

## 21.0 Review Agent Contract

The Review Agent evaluates agent outputs, candidate artifacts, plans, repair proposals, and reasoning consistency. The Review Agent provides structured findings but does not replace deterministic validation or governance approval.

Review is used to detect inconsistencies, missing traceability, role-boundary violations, low-quality outputs, or divergence from specification intent.

### 21.1 Responsibilities

The Review Agent MAY:

* review agent outputs for completeness and consistency
* review candidate artifacts for specification alignment
* review task outputs before downstream handoff
* detect missing traceability references
* detect unsupported assumptions
* detect role contract violations
* recommend cross-agent review or escalation

### 21.2 Required Inputs

| Input                         | Required    |
| ----------------------------- | ----------- |
| Agent output under review     | YES         |
| Task definition               | YES         |
| Source specification entities | YES         |
| Applicable output contract    | YES         |
| Validation criteria           | CONDITIONAL |
| Governance constraints        | CONDITIONAL |
| Related artifacts             | CONDITIONAL |

### 21.3 Permitted Outputs

| Output Type        | Description                          |
| ------------------ | ------------------------------------ |
| review-finding     | Review findings and recommendations  |
| escalation-support | Escalation support                   |
| interpretation     | Review interpretation where required |

### 21.4 Structured Output Payload

| Field                     | Type   | Required    | Description                                                |
| ------------------------- | ------ | ----------- | ---------------------------------------------------------- |
| reviewed-output-id        | string | REQUIRED    | Output reviewed                                            |
| review-outcome            | enum   | REQUIRED    | accepted, accepted-with-warning, rejected, escalate        |
| findings                  | array  | CONDITIONAL | Required for warning, rejected, or escalate                |
| specification-alignment   | enum   | REQUIRED    | aligned, partially-aligned, not-aligned, unknown           |
| traceability-completeness | enum   | REQUIRED    | complete, incomplete, not-applicable                       |
| contract-compliance       | enum   | REQUIRED    | compliant, non-compliant                                   |
| recommended-next-action   | enum   | REQUIRED    | accept, revise, revalidate, cross-review, escalate, reject |

### 21.5 Prohibited Actions

The Review Agent MUST NOT:

* approve governance gates
* mark artifacts valid without deterministic validation
* suppress policy findings
* modify outputs directly
* override role contract violations

### 21.6 Failure Modes

| Failure Mode                       | Handling                  |
| ---------------------------------- | ------------------------- |
| reviewed output missing            | fail invocation           |
| conflicting review results         | conflict resolution       |
| insufficient context for review    | request corrected context |
| review identifies governance issue | escalate                  |

---

## 22.0 Security Validator Contract

The Security Validator evaluates security, compliance, data protection, access control, dependency, interface exposure, tool/model usage, and policy implications of specifications, plans, artifacts, and deployment outputs.

The Security Validator supports governance and deterministic security tooling. It does not replace required security tools or authorized security review gates.

### 22.1 Responsibilities

The Security Validator MAY:

* review Policy entity coverage
* review authentication and authorization implications
* review sensitive data handling
* review interface exposure risks
* review dependency and supply-chain risks
* review generated artifacts for security concerns
* interpret security tool findings
* recommend escalation, waiver, repair, or additional validation

### 22.2 Required Inputs

| Input                                  | Required    |
| -------------------------------------- | ----------- |
| Policy entities                        | YES         |
| Security-related requirements          | YES         |
| DataEntity sensitivity classifications | CONDITIONAL |
| Interface definitions                  | CONDITIONAL |
| Generated artifacts                    | CONDITIONAL |
| Security tool findings                 | CONDITIONAL |
| Governance risk tier                   | YES         |
| Model/tool trust constraints           | CONDITIONAL |

### 22.3 Permitted Outputs

| Output Type        | Description                         |
| ------------------ | ----------------------------------- |
| security-finding   | Security finding or risk assessment |
| review-finding     | Policy or compliance review finding |
| repair-proposal    | Security repair recommendation      |
| escalation-support | Security escalation support         |

### 22.4 Structured Output Payload

| Field                | Type  | Required    | Description                                              |
| -------------------- | ----- | ----------- | -------------------------------------------------------- |
| security-findings    | array | REQUIRED    | Security or policy findings                              |
| affected-targets     | array | REQUIRED    | Entities, artifacts, interfaces, data, or tasks affected |
| severity             | enum  | REQUIRED    | critical, high, medium, low, informational               |
| policy-references    | array | CONDITIONAL | Policy entities or external controls                     |
| recommended-response | enum  | REQUIRED    | proceed, repair, escalate, block, waive-review           |
| evidence             | array | CONDITIONAL | Tool findings or review evidence                         |
| residual-risk        | enum  | CONDITIONAL | low, medium, high, critical, unknown                     |

### 22.5 Prohibited Actions

The Security Validator MUST NOT:

* waive security findings
* approve security review gates unless acting under a governance role outside agent context
* suppress tool findings
* mark security validation passed without evidence
* bypass governance risk-tier rules

### 22.6 Failure Modes

| Failure Mode                    | Handling                                 |
| ------------------------------- | ---------------------------------------- |
| policy context missing          | fail and request context correction      |
| critical finding detected       | escalate or block according to policy    |
| security tool findings conflict | conflict resolution or governance review |
| risk cannot be assessed         | escalate                                 |

---

## 23.0 Deployment Preparer Contract

The Deployment Preparer produces deployment preparation guidance and artifacts based on validated implementation outputs, Infrastructure entities, operational requirements, governance constraints, and deployment target profiles.

The Deployment Preparer prepares for deployment authorization but does not authorize deployment.

### 23.1 Responsibilities

The Deployment Preparer MAY:

* generate deployment artifact candidates
* generate deployment manifest candidates
* generate runtime configuration templates
* generate health check definitions
* identify operational readiness gaps
* identify deployment validation requirements
* produce deployment preparation records

### 23.2 Required Inputs

| Input                     | Required    |
| ------------------------- | ----------- |
| Consolidated artifact set | YES         |
| Infrastructure entities   | YES         |
| Operational requirements  | YES         |
| Deployment target profile | YES         |
| Validation results        | YES         |
| Policy evaluations        | CONDITIONAL |
| Governance constraints    | YES         |
| Traceability snapshot     | YES         |

### 23.3 Permitted Outputs

| Output Type            | Description                           |
| ---------------------- | ------------------------------------- |
| deployment-preparation | Deployment preparation plan or record |
| artifact-content       | Deployment artifact candidates        |
| review-finding         | Operational readiness finding         |
| escalation-support     | Deployment escalation support         |

### 23.4 Structured Output Payload

| Field                              | Type   | Required    | Description                                         |
| ---------------------------------- | ------ | ----------- | --------------------------------------------------- |
| deployment-artifact-candidates     | array  | CONDITIONAL | Candidate deployment artifacts                      |
| target-environments                | array  | REQUIRED    | Environments prepared for                           |
| runtime-configuration              | object | CONDITIONAL | Runtime configuration output                        |
| health-checks                      | array  | REQUIRED    | Health check definitions                            |
| operational-readiness-findings     | array  | CONDITIONAL | Gaps or risks                                       |
| deployment-validation-requirements | array  | REQUIRED    | Validation required before deployment authorization |
| governance-dependencies            | array  | REQUIRED    | Approval or policy dependencies                     |
| recommended-next-action            | enum   | REQUIRED    | proceed-to-validation, revise, escalate, block      |

### 23.5 Prohibited Actions

The Deployment Preparer MUST NOT:

* authorize production deployment
* bypass deployment authorization
* include invalid or untraced artifacts in stable deployment outputs
* ignore unresolved blocking policy violations
* change Infrastructure requirements without traceable reauthorization

### 23.6 Failure Modes

| Failure Mode                      | Handling                      |
| --------------------------------- | ----------------------------- |
| deployment target missing         | fail invocation               |
| consolidated artifacts incomplete | block deployment preparation  |
| unresolved policy violation       | escalate                      |
| deployment artifact invalid       | route to validation or repair |

---

## 24.0 Cross-Agent Review and Conflict Resolution

This section defines how the platform handles disagreement, contradiction, or inconsistency between agent outputs. Agents are probabilistic reasoning components; therefore, the platform must define how conflicting conclusions are detected and resolved.

### 24.1 Conflict Types

| Conflict Type         | Description                                                        |
| --------------------- | ------------------------------------------------------------------ |
| role-conflict         | Agent output exceeds or contradicts role authority                 |
| semantic-conflict     | Output contradicts canonical model or specification entity         |
| artifact-conflict     | Multiple outputs propose incompatible artifact changes             |
| architecture-conflict | Architecture recommendations conflict                              |
| repair-conflict       | Repair proposal conflicts with validation findings or prior repair |
| security-conflict     | Output conflicts with policy or security finding                   |
| plan-conflict         | Output conflicts with construction task graph                      |
| governance-conflict   | Output conflicts with governance decision                          |

### 24.2 Agent Conflict Record Schema

| Field                | Type     | Required    | Description                                                                                 |
| -------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------- |
| conflict-id          | string   | REQUIRED    | Unique conflict identifier                                                                  |
| conflict-type        | enum     | REQUIRED    | Conflict type                                                                               |
| involved-output-ids  | array    | REQUIRED    | Agent outputs involved                                                                      |
| involved-agent-roles | array    | REQUIRED    | Agent roles involved                                                                        |
| task-id              | string   | REQUIRED    | Related task                                                                                |
| affected-entities    | array    | CONDITIONAL | Specification entities affected                                                             |
| affected-artifacts   | array    | CONDITIONAL | Artifacts affected                                                                          |
| severity             | enum     | REQUIRED    | blocking, high, medium, low                                                                 |
| resolution-method    | enum     | REQUIRED    | reviewer-decision, deterministic-validation, governance-review, re-invocation, replan, halt |
| resolution           | string   | CONDITIONAL | Resolution applied                                                                          |
| resolved-at          | ISO 8601 | CONDITIONAL | Resolution timestamp                                                                        |

### 24.3 Conflict Resolution Rules

Semantic conflicts with the canonical model MUST be resolved in favor of the canonical model unless the specification is formally changed.

Security conflicts MUST be routed to Security Validator and Governance Engine when policy-relevant.

Artifact conflicts MUST be resolved before artifact repository admission.

Plan conflicts MUST be routed to the Construction Planner or Planning Engine.

Governance conflicts MUST be resolved by the Governance Engine.

A blocking conflict MUST prevent downstream task completion until resolved.

---

## 25.0 Agent Failure Model

This section defines standard agent failure modes and handling behavior. Agent failures must be explicit so that the platform can recover, retry, escalate, or halt safely.

### 25.1 Standard Agent Failure Classes

| Failure Class                          | Description                                            | Default Handling                   |
| -------------------------------------- | ------------------------------------------------------ | ---------------------------------- |
| agent-context-missing                  | Required context was absent                            | fail and request corrected context |
| agent-context-invalid                  | Context package contained invalid references           | fail and request corrected context |
| agent-timeout                          | Agent invocation exceeded timeout                      | retry once or escalate             |
| agent-output-empty                     | Agent returned no usable output                        | retry or fail                      |
| agent-output-schema-invalid            | Output failed schema validation                        | retry or request revision          |
| agent-output-role-violation            | Output exceeded role authority                         | reject and escalate if material    |
| agent-output-untraceable               | Output lacks required traceability references          | reject                             |
| agent-output-contradicts-specification | Output contradicts canonical model                     | reject or escalate                 |
| agent-output-policy-violation          | Output violates governance or policy constraints       | escalate or block                  |
| agent-model-invocation-failed          | Underlying model invocation failed                     | retry, fallback, or escalate       |
| agent-nonconvergent                    | Repeated invocations fail to produce acceptable output | escalate                           |
| agent-conflict-unresolved              | Conflict between agent outputs remains unresolved      | escalate or halt                   |

### 25.2 Agent Failure Record Schema

| Field               | Type     | Required | Description                  |
| ------------------- | -------- | -------- | ---------------------------- |
| failure-id          | string   | REQUIRED | Unique failure identifier    |
| failure-class       | enum     | REQUIRED | Failure class                |
| agent-role          | enum     | REQUIRED | Role associated with failure |
| agent-invocation-id | string   | REQUIRED | Invocation that failed       |
| task-id             | string   | REQUIRED | Related task                 |
| severity            | enum     | REQUIRED | blocking, high, medium, low  |
| message             | string   | REQUIRED | Explanation of failure       |
| required-action     | string   | REQUIRED | Remediation required         |
| retry-count         | integer  | REQUIRED | Number of retries attempted  |
| recorded-at         | ISO 8601 | REQUIRED | Failure timestamp            |

### 25.3 Retry Rules

An agent invocation MAY be retried when failure is caused by timeout, transient model failure, empty output, or schema-invalid output.

An agent invocation MUST NOT be retried indefinitely.

Default retry limit SHOULD be two retries per invocation unless runtime configuration specifies a stricter limit.

A repeated failure after retry limit MUST escalate.

### 25.4 Escalation Rules

The platform MUST escalate when:

* output contradicts canonical specification and cannot be resolved
* output violates policy
* repeated retries fail
* required context cannot be assembled
* role conflict is material
* agent conflict remains unresolved
* governance requires human review

---

## 26.0 Agent Collaboration State

This section defines collaboration state tracked for agent workflows. Collaboration state allows the platform to understand which agents have been invoked, which outputs are pending review, which handoffs are complete, and which failures or conflicts remain unresolved.

### 26.1 Collaboration Session Schema

| Field                     | Type     | Required    | Description                                              |
| ------------------------- | -------- | ----------- | -------------------------------------------------------- |
| collaboration-session-id  | string   | REQUIRED    | Unique collaboration session identifier                  |
| task-id                   | string   | REQUIRED    | Task associated with session                             |
| execution-graph-id        | string   | CONDITIONAL | Execution graph when applicable                          |
| participating-agent-roles | array    | REQUIRED    | Roles involved                                           |
| invocation-ids            | array    | REQUIRED    | Agent invocations in session                             |
| output-ids                | array    | CONDITIONAL | Agent outputs produced                                   |
| handoff-ids               | array    | CONDITIONAL | Handoffs performed                                       |
| conflict-ids              | array    | CONDITIONAL | Conflicts detected                                       |
| failure-ids               | array    | CONDITIONAL | Failures detected                                        |
| status                    | enum     | REQUIRED    | pending, active, completed, failed, escalated, cancelled |
| created-at                | ISO 8601 | REQUIRED    | Creation timestamp                                       |
| updated-at                | ISO 8601 | CONDITIONAL | Last update timestamp                                    |

### 26.2 Collaboration State Rules

A collaboration session MUST be created when a task requires more than one agent invocation.

A collaboration session MUST record all invocations and handoffs.

A collaboration session MUST NOT be marked completed while blocking conflicts or failures remain unresolved.

Collaboration state MUST be persisted when required for execution recovery.

---

## 27.0 Governance of Agent Collaboration

This section defines governance controls over agent collaboration. Agent collaboration may involve model usage, sensitive context, security decisions, generated code, deployment artifacts, or policy-sensitive recommendations. Governance must control these activities where risk requires it.

### 27.1 Governed Agent Activities

Governance MAY control:

* model provider selection
* use of external model providers
* restricted context disclosure
* high-risk artifact generation
* security validation outputs
* repair of high-risk artifacts
* deployment preparation
* override of agent findings
* use of experimental agents
* cross-agent conflict resolution

### 27.2 Governance Rules

An agent invocation MUST NOT proceed when governance returns block.

An agent invocation requiring approval MUST pause until approval is granted.

Agent outputs that propose waivers, overrides, or policy exceptions MUST be routed to the Governance Engine.

Agents MUST NOT approve governance gates in their agent capacity.

Agent outputs affecting High or Critical risk systems MAY require review before use according to governance configuration.

---

## 28.0 Traceability Requirements for Agents

This section defines traceability requirements for agent activity. Agent actions are central to autonomous construction, so their inputs, outputs, decisions, and downstream effects must be explainable.

### 28.1 Required Traceability Links

The platform MUST record traceability links between:

* task and agent invocation
* agent invocation and context package
* agent invocation and model invocation
* agent invocation and agent output
* agent output and source specification entities
* agent output and produced artifact candidates
* agent output and downstream handoff
* agent output and review findings
* agent output and deterministic validation results where applicable
* agent failure and affected task
* agent conflict and involved outputs

### 28.2 Decision Records

A Decision Record MUST be created when an agent output materially affects:

* architecture
* implementation generation
* repair direction
* security response
* deployment preparation
* escalation
* governance recommendation

Decision Records MUST follow ISL v1.4.

### 28.3 Traceability Failure

An agent output that cannot be traced to source entities and task context MUST be rejected.

A task MUST NOT be marked completed if required agent traceability is missing.

---

## 29.0 Observability and Telemetry for Agents

This section defines minimum observability requirements for agent collaboration. Agent activity telemetry allows operators to understand agent performance, failure rates, latency, output quality, context usage, and collaboration patterns.

Telemetry supports improvement and monitoring, but it does not replace traceability or governance audit records.

### 29.1 Required Agent Telemetry Events

The platform SHOULD emit telemetry for:

* agent invocation requested
* context package assembled
* governance check completed
* model invocation started
* model invocation completed
* agent output received
* agent output normalized
* agent output validated
* handoff created
* review completed
* conflict detected
* failure detected
* retry attempted
* escalation opened
* invocation completed

### 29.2 Minimum Telemetry Fields

Each agent telemetry event SHOULD include:

| Field                 | Description                                |
| --------------------- | ------------------------------------------ |
| event-id              | Unique telemetry event identifier          |
| event-type            | Type of agent event                        |
| timestamp             | Event time                                 |
| agent-role            | Agent role                                 |
| agent-invocation-id   | Invocation identifier                      |
| task-id               | Related task                               |
| specification-id      | System identifier                          |
| specification-version | Specification version                      |
| context-package-id    | Context package identifier when applicable |
| output-id             | Agent output identifier when applicable    |
| outcome               | Event outcome                              |
| duration-ms           | Duration when applicable                   |
| severity              | Event severity when applicable             |

### 29.3 Alert Conditions

The platform SHOULD alert operators or governance roles when:

* repeated agent failures occur
* agent output schema failures exceed threshold
* context assembly failures recur
* security-sensitive agent output is escalated
* model provider errors affect multiple agents
* nonconvergent repair behavior is detected
* unauthorized context disclosure is attempted
* high-severity agent conflicts remain unresolved

---

## 30.0 Security and Context Protection

This section defines security requirements specific to agent collaboration. Agents may receive specification fragments, artifact content, validation results, policies, and operational context. Some of this information may be sensitive.

### 30.1 Minimum Security Requirements

The platform MUST:

* enforce context minimization
* classify context sensitivity
* restrict secret exposure
* enforce model provider restrictions
* prevent direct agent access to unauthorized repositories or state stores
* prevent agents from bypassing tool validation
* record context packages used for invocations
* apply governance policies before restricted model or context use
* sanitize or structure agent outputs before downstream processing

### 30.2 Prompt and Context Injection Protection

Agent context assembly MUST distinguish trusted platform instructions from untrusted specification content, artifact content, logs, tool output, or external input.

Untrusted content MUST NOT be allowed to override platform instructions, governance rules, output contracts, or role boundaries.

If a context package contains untrusted content that attempts to change agent role, bypass validation, reveal secrets, or override governance, the platform MUST record a security finding and MAY sanitize, exclude, or escalate the content.

### 30.3 Sensitive Context Rules

Restricted or confidential context MUST be provided only to model providers and agents authorized for that sensitivity level.

Agent outputs derived from restricted context MUST inherit appropriate sensitivity metadata unless downgraded by an approved governance process.

---

## 31.0 Agent Extension Model

This section defines how new agent roles may be introduced. Agent extensibility allows the platform to support domain-specific reasoning, specialized compliance review, performance analysis, migration planning, or other advanced functions.

Extensions must not weaken core platform controls.

### 31.1 Agent Role Extension Schema

| Field              | Type     | Required    | Description                             |
| ------------------ | -------- | ----------- | --------------------------------------- |
| extension-role-id  | string   | REQUIRED    | Unique role extension identifier        |
| role-name          | string   | REQUIRED    | Human-readable role name                |
| role-purpose       | string   | REQUIRED    | Purpose of role                         |
| responsibilities   | array    | REQUIRED    | Allowed responsibilities                |
| required-inputs    | array    | REQUIRED    | Required context inputs                 |
| permitted-outputs  | array    | REQUIRED    | Output types permitted                  |
| prohibited-actions | array    | REQUIRED    | Actions role may not perform            |
| output-contracts   | array    | REQUIRED    | Output contract identifiers             |
| governance-profile | string   | CONDITIONAL | Governance constraints applying to role |
| compatibility      | array    | REQUIRED    | Supported ISL versions                  |
| registered-at      | ISO 8601 | REQUIRED    | Registration timestamp                  |
| registered-by      | string   | REQUIRED    | Authority registering role              |

### 31.2 Extension Rules

An agent extension MUST declare a role contract.

An agent extension MUST NOT redefine standard roles.

An agent extension MUST NOT bypass model abstraction, traceability, deterministic validation, or governance controls.

An agent extension MUST be registered before use.

Governance MAY restrict extension roles by risk tier, environment, or project.

---

## 32.0 Agent Collaboration Error Classes

This section defines error classes specific to agent collaboration. These errors allow the runtime, orchestrator, governance system, and observability components to classify collaboration failures consistently.

### 32.1 Error Record Schema

| Field               | Type     | Required    | Description                 |
| ------------------- | -------- | ----------- | --------------------------- |
| error-id            | string   | REQUIRED    | Unique error identifier     |
| error-class         | enum     | REQUIRED    | Error class                 |
| severity            | enum     | REQUIRED    | blocking, high, medium, low |
| agent-role          | enum     | CONDITIONAL | Agent role involved         |
| agent-invocation-id | string   | CONDITIONAL | Invocation involved         |
| task-id             | string   | CONDITIONAL | Task involved               |
| output-id           | string   | CONDITIONAL | Output involved             |
| conflict-id         | string   | CONDITIONAL | Conflict involved           |
| message             | string   | REQUIRED    | Human-readable explanation  |
| required-action     | string   | REQUIRED    | Remediation action          |
| recorded-at         | ISO 8601 | REQUIRED    | Time error was recorded     |

### 32.2 Error Classes

| Error Class                      | Description                                             | Default Handling        |
| -------------------------------- | ------------------------------------------------------- | ----------------------- |
| agent-role-unsupported           | Requested role is not available                         | escalate or fail task   |
| agent-contract-missing           | Role contract is not defined                            | halt affected task      |
| agent-context-assembly-failed    | Required context could not be assembled                 | fail or escalate        |
| agent-context-policy-violation   | Context violates governance or sensitivity rules        | block or escalate       |
| agent-invocation-timeout         | Agent invocation timed out                              | retry then escalate     |
| agent-output-missing             | No output produced                                      | retry or fail           |
| agent-output-invalid-schema      | Output failed schema validation                         | retry or reject         |
| agent-output-role-violation      | Output exceeds role authority                           | reject or escalate      |
| agent-output-untraceable         | Output lacks required traceability references           | reject                  |
| agent-handoff-missing            | Required handoff did not occur                          | block downstream task   |
| agent-handoff-rejected           | Target role rejected handoff                            | review or escalate      |
| agent-conflict-detected          | Agent outputs conflict                                  | run conflict resolution |
| agent-conflict-unresolved        | Conflict remains unresolved                             | escalate or halt        |
| agent-governance-blocked         | Governance blocks invocation or output use              | block                   |
| agent-security-context-violation | Prompt/context injection or sensitive context violation | block and escalate      |

---

## 33.0 Conformance Requirements

This section defines conformance requirements for agent roles, agent orchestrators, and platforms.

### 33.1 Agent Role Conformance

An agent role conforms to ISL v2.1 if it:

* has a defined role contract
* accepts the required invocation schema
* consumes structured context packages
* produces outputs conforming to declared output contracts
* operates within prohibited-action boundaries
* supports required failure handling
* preserves traceability requirements
* supports governance controls

### 33.2 Agent Orchestrator Conformance

An Agent Orchestrator conforms to ISL v2.1 if it can:

* invoke standard agent roles
* validate role contracts
* assemble or request context packages
* route invocations through the model abstraction boundary
* validate agent outputs
* manage handoffs
* detect collaboration conflicts
* manage retries and escalation
* preserve collaboration state
* record traceability links
* emit agent telemetry
* enforce governance decisions

### 33.3 Platform Conformance

A platform conforms to ISL v2.1 if it:

* implements or supports required agent roles for its claimed capability level
* prevents agents from bypassing runtime, governance, traceability, model, and tool boundaries
* validates all agent outputs before downstream use
* requires deterministic validation for objective artifact acceptance
* records agent invocations, outputs, failures, handoffs, conflicts, and decisions
* supports agent audit queries through traceability and observability systems
* protects sensitive context
* supports governed model usage

---

## 34.0 Summary

ISL v2.1 defines the Agent Collaboration Model for the IMHOTEP platform. It establishes the standard agent roles, lifecycle, invocation contracts, context package schema, output contracts, handoff behavior, collaboration patterns, role-specific contracts, failure modes, conflict resolution, governance integration, traceability requirements, telemetry expectations, security controls, extension model, and conformance requirements.

This revision strengthens the original agent collaboration concept by making agent roles operationally bounded and machine-checkable. Agents are no longer described only as conceptual participants; they are governed platform components with defined inputs, outputs, responsibilities, prohibitions, failure paths, and collaboration rules.

A conforming IMHOTEP platform MUST treat agents as controlled reasoning components. Agents may interpret, plan, generate, review, repair, test, validate, and prepare deployment outputs, but they MUST remain mediated by the runtime, constrained by governance, grounded in canonical specifications, traced through the platform, and validated by deterministic tools where objective correctness is required.
