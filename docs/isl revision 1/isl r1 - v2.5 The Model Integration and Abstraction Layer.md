# IMHOTEP Specification Language (ISL) v2.5

# The Model Integration and Abstraction Layer

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.4
**Supersedes:** ISL r0 v2.5 The Model Integration and Abstraction Layer
**Document Type:** Model Provider, Routing, and Interaction Abstraction Specification

---

## 1.0 Scope

This document defines the Model Integration and Abstraction Layer for the IMHOTEP platform. It establishes how reasoning agents obtain model capabilities through a standardized, provider-neutral interface.

This document applies to all model interactions used for specification interpretation, architecture planning, construction planning, implementation generation, repair analysis, test generation, review, security validation, deployment preparation, and other model-assisted reasoning activities.

This document defines:

* model provider abstraction
* local-first model strategy
* model provider registration
* model capability declaration
* model capability validation
* model selection and routing
* model invocation contracts
* context packaging and context control
* structured output requirements
* output normalization
* model failure handling
* fallback behavior
* governance and policy enforcement
* traceability and auditability
* telemetry and observability
* model lifecycle management
* conformance requirements

---

## 2.0 Purpose of the Model Integration and Abstraction Layer

The purpose of the Model Integration and Abstraction Layer is to isolate the IMHOTEP platform and its agents from dependency on any specific model provider, model runtime, model architecture, or model API. Agents require reasoning capability, but they MUST NOT depend directly on a particular provider interface.

This layer provides the controlled boundary through which agents request reasoning services. It allows the platform to use local models, enterprise-hosted models, cloud-hosted models, specialized coding models, review models, security models, or hybrid model configurations without changing the agent contracts or execution runtime.

The Model Integration and Abstraction Layer exists to ensure that:

* models are used through a standard platform contract
* provider-specific logic is isolated behind adapters
* model usage remains local-first where configured
* model capabilities are declared explicitly
* routing decisions are explainable and auditable
* context exposure is controlled by governance and sensitivity rules
* model outputs are normalized before downstream use
* model failures have defined handling paths
* model interactions are traceable to tasks, agents, artifacts, and decisions
* model providers can evolve without destabilizing platform architecture

---

## 3.0 Normative References

| Reference | Purpose                                                                            |
| --------- | ---------------------------------------------------------------------------------- |
| ISL v0.0  | Corpus principles, conformance language, and standards alignment                   |
| ISL v1.1  | Canonical semantic model and entity identifiers                                    |
| ISL v1.2  | Execution phases and runtime use of reasoning agents                               |
| ISL v1.4  | Traceability records, decision records, and model invocation traceability          |
| ISL v1.7  | Governance controls, model-use approvals, policy enforcement, waivers, and audit   |
| ISL v2.0  | Platform architecture and model abstraction boundary                               |
| ISL v2.1  | Agent role contracts, invocation contracts, context packages, and output contracts |
| ISL v2.2  | Model state, agent state, persistent memory, and recovery                          |
| ISL v2.4  | Runtime scheduling, worker execution, retry, timeout, and runtime error handling   |
| ISL v2.6  | Observability and telemetry model                                                  |
| RFC 2119  | Normative terminology                                                              |

---

## 4.0 Terms and Definitions

**Model**
An artificial intelligence model capable of producing reasoning, generation, classification, transformation, review, or structured output.

**Model Provider**
A local runtime, enterprise service, cloud API, embedded model host, or other source through which models are made available to the platform.

**Model Adapter**
A provider-specific implementation that translates platform model requests into provider-specific invocations and normalizes provider responses.

**Model Integration Layer**
The platform subsystem that registers model providers, validates model capabilities, routes requests, invokes models, normalizes outputs, and enforces model governance.

**Model Capability**
A declared ability of a model, such as code generation, structured output, long-context reasoning, repair analysis, security review, or test generation.

**Model Profile**
A structured record describing a model, its capabilities, constraints, cost, context limits, trust level, supported output formats, and provider relationship.

**Routing Policy**
A rule set used to select a model for an agent invocation.

**Model Invocation**
A structured request to a model through the abstraction layer.

**Model Invocation Result**
The structured response returned by the model integration layer after provider invocation, output normalization, validation, and error handling.

**Fallback Model**
An alternative model selected when the primary model is unavailable, fails, times out, violates policy, or fails output validation.

---

## 5.0 Design Principles

### 5.1 Provider Neutrality

Agents MUST interact with models only through the Model Integration and Abstraction Layer. Agents MUST NOT directly invoke local runtimes, remote APIs, provider SDKs, or provider-specific prompt interfaces.

### 5.2 Local-First Operation

The platform MUST be capable of operating with locally hosted or locally available models where the deployment profile requires local-first operation. External models MAY be used only when governance and configuration permit them.

### 5.3 Explicit Capability Declaration

Model capabilities MUST be declared explicitly. The platform MUST NOT infer capabilities from model names, provider branding, marketing descriptions, or informal labels.

### 5.4 Capability-Based Routing

The platform MUST select models based on declared capabilities, task requirements, agent role, context requirements, governance constraints, and runtime policy.

### 5.5 Context Minimization

The platform MUST provide models only the context required for the invocation. Context assembly MUST respect sensitivity, governance, risk tier, and provider restrictions.

### 5.6 Structured Output Preference

Model invocations SHOULD use structured output contracts when downstream platform components will interpret the result automatically.

### 5.7 Runtime-Governed Model Use

Restricted model use MUST be governed at runtime. A model invocation MUST NOT proceed when governance returns block, approval-required, waiver-required, override-required, or escalate.

### 5.8 Traceable Reasoning

Every model invocation that contributes to planning, generation, review, repair, security validation, deployment preparation, governance support, or artifact creation MUST be traceable.

---

## 6.0 Model Integration Architecture

The Model Integration and Abstraction Layer is the controlled interface between agents and model providers.

### 6.1 Logical Components

| Component                 | Responsibility                                                             |
| ------------------------- | -------------------------------------------------------------------------- |
| Model Registry            | Stores provider records, model profiles, capabilities, and lifecycle state |
| Provider Adapter Registry | Stores provider adapter records and supported invocation methods           |
| Model Router              | Selects models for invocations                                             |
| Capability Evaluator      | Validates model capability compatibility with requested work               |
| Context Controller        | Enforces context selection, minimization, sensitivity, and redaction       |
| Invocation Manager        | Executes model invocations through provider adapters                       |
| Output Normalizer         | Converts provider responses into standard result structures                |
| Policy Enforcer           | Applies governance and model-use restrictions                              |
| Fallback Manager          | Selects alternate models when permitted                                    |
| Telemetry Emitter         | Emits model interaction telemetry                                          |
| Traceability Connector    | Records model invocation links to tasks, agents, outputs, and decisions    |

### 6.2 Architectural Rules

All model invocations MUST pass through the Model Integration Layer.

Provider-specific credentials, SDKs, endpoints, and runtime details MUST be isolated inside provider adapters or controlled configuration.

The Model Router MUST NOT select models that are not registered, not active, or not permitted by governance.

The Output Normalizer MUST validate model output before downstream platform use.

---

## 7.0 Model Provider Types

This section defines standard model provider types.

| Provider Type         | Description                                                                        |
| --------------------- | ---------------------------------------------------------------------------------- |
| local-runtime         | Model hosted locally on the same machine or local network                          |
| embedded-runtime      | Model embedded directly inside a platform process or component                     |
| enterprise-service    | Organization-managed model service                                                 |
| private-cloud-service | Cloud-hosted service under enterprise control                                      |
| public-cloud-api      | External public model provider API                                                 |
| specialized-service   | Domain-specific model service, such as code, security, test, or document reasoning |
| brokered-provider     | Provider accessed through an internal model gateway or broker                      |

### 7.1 Provider Type Rules

A provider MUST declare its provider type.

A public-cloud-api provider MUST be governed before use.

A local-runtime provider SHOULD be preferred when local-first mode is active and capability requirements are satisfied.

A specialized-service provider MAY be selected when it satisfies a task-specific capability better than a general-purpose model.

---

## 8.0 Model Provider Registration

A provider MUST be registered before any model associated with that provider may be invoked.

### 8.1 Provider Record Schema

| Field | Type | Required | Description |
|---|---|---|
| provider-id | string | REQUIRED | Unique provider identifier |
| provider-name | string | REQUIRED | Human-readable provider name |
| provider-type | enum | REQUIRED | Provider type from §7.0 |
| adapter-id | string | REQUIRED | Adapter used to communicate with provider |
| deployment-location | enum | REQUIRED | local, on-premises, private-cloud, public-cloud, hybrid |
| network-access-required | boolean | REQUIRED | Whether provider requires network access |
| data-residency-profile | string | CONDITIONAL | Data residency profile when applicable |
| supported-model-ids | array | REQUIRED | Models exposed by provider |
| authentication-profile-id | string | CONDITIONAL | Authentication profile reference |
| trust-profile-id | string | REQUIRED | Provider trust profile |
| status | enum | REQUIRED | proposed, active, restricted, disabled, deprecated, revoked |
| registered-at | ISO 8601 | REQUIRED | Registration time |
| registered-by | string | REQUIRED | Authority or subsystem registering provider |

### 8.2 Provider Registration Rules

A provider MUST NOT be used unless status is active or restricted with satisfied conditions.

A revoked provider MUST NOT be used.

Provider registration MUST be auditable.

Provider configuration changes affecting routing, trust, data exposure, authentication, or location MUST be recorded as governance-relevant configuration changes.

---

## 9.0 Provider Adapter Model

Provider adapters translate platform model invocation requests into provider-specific calls.

### 9.1 Provider Adapter Record Schema

| Field | Type | Required | Description |
|---|---|---|
| adapter-id | string | REQUIRED | Unique adapter identifier |
| provider-type-supported | enum | REQUIRED | Provider type supported |
| adapter-version | semver | REQUIRED | Adapter version |
| invocation-method | enum | REQUIRED | local-call, cli, http-api, grpc, sdk, message-bus |
| supported-request-version | semver | REQUIRED | Supported platform request schema version |
| supported-response-version | semver | REQUIRED | Supported platform response schema version |
| supports-streaming | boolean | REQUIRED | Whether streaming responses are supported |
| supports-structured-output | boolean | REQUIRED | Whether provider can enforce structured output |
| supports-tool-use | boolean | REQUIRED | Whether provider supports model-native tool calls |
| supports-context-cache | boolean | REQUIRED | Whether context caching is supported |
| status | enum | REQUIRED | active, deprecated, disabled, revoked |
| registered-at | ISO 8601 | REQUIRED | Registration time |

### 9.2 Adapter Rules

An adapter MUST normalize provider responses into the platform Model Invocation Result schema.

An adapter MUST NOT expose provider-specific response formats directly to agents.

An adapter MUST record provider error details in structured error records.

An adapter MUST enforce timeout and cancellation where supported by the provider.

---

## 10.0 Model Profile

A model MUST have a Model Profile before routing.

### 10.1 Model Profile Schema

| Field | Type | Required | Description |
|---|---|---|
| model-profile-id | string | REQUIRED | Unique model profile identifier |
| model-id | string | REQUIRED | Provider-specific or platform-normalized model identifier |
| provider-id | string | REQUIRED | Provider exposing the model |
| model-family | string | CONDITIONAL | Model family or class |
| model-version | string | REQUIRED | Model version or release identifier |
| model-type | enum | REQUIRED | general, coding, reasoning, review, security, planning, embedding, classification, multimodal |
| supported-agent-roles | array | REQUIRED | Agent roles allowed to use model |
| declared-capabilities | array | REQUIRED | Capability declarations |
| context-window-tokens | integer | CONDITIONAL | Maximum context size when tokenized |
| max-output-tokens | integer | CONDITIONAL | Maximum output size |
| supports-json-output | boolean | REQUIRED | Whether JSON or schema output is supported |
| supports-markdown-output | boolean | REQUIRED | Whether Markdown output is supported |
| supports-function-calling | boolean | REQUIRED | Whether model-native structured calls are supported |
| supports-vision-input | boolean | REQUIRED | Whether image input is supported |
| supports-file-input | boolean | REQUIRED | Whether file input is supported |
| latency-profile | enum | REQUIRED | low, medium, high, variable, unknown |
| cost-profile | enum | REQUIRED | free, low, medium, high, variable, unknown |
| data-retention-profile | enum | REQUIRED | none, transient, provider-retained, unknown |
| trust-profile-id | string | REQUIRED | Model trust profile |
| status | enum | REQUIRED | active, restricted, experimental, deprecated, disabled, revoked |
| registered-at | ISO 8601 | REQUIRED | Registration time |
| registered-by | string | REQUIRED | Authority or subsystem registering model |

### 10.2 Model Profile Rules

A model MUST NOT be routed unless its profile status permits use.

A model with experimental status MAY be used only when governance permits experimental model use for the risk tier and environment.

A model with unknown data-retention-profile MUST NOT receive confidential or restricted context unless governance explicitly authorizes it.

---

## 11.0 Model Capability Declaration

This section defines how model capabilities are declared.

### 11.1 Standard Capability Categories

| Category          | Capabilities                                                                                        |
| ----------------- | --------------------------------------------------------------------------------------------------- |
| reasoning         | requirement-interpretation, architecture-reasoning, planning-reasoning, impact-analysis-reasoning   |
| generation        | code-generation, schema-generation, interface-generation, test-generation, documentation-generation |
| review            | artifact-review, plan-review, requirement-review, traceability-review                               |
| repair            | failure-analysis, repair-proposal, repair-risk-assessment                                           |
| security          | security-review, policy-interpretation, threat-analysis, data-protection-review                     |
| deployment        | deployment-preparation, infrastructure-reasoning, operational-readiness-review                      |
| structured-output | json-output, schema-constrained-output, table-output, decision-record-output                        |
| context           | long-context, multi-document-context, artifact-context, validation-context                          |
| multimodal        | image-understanding, diagram-understanding, document-vision                                         |
| classification    | routing-classification, risk-classification, sensitivity-classification                             |

### 11.2 Capability Declaration Schema

| Field | Type | Required | Description |
|---|---|---|
| capability-id | string | REQUIRED | Unique capability declaration identifier |
| capability-name | string | REQUIRED | Capability name |
| capability-category | enum | REQUIRED | Category from §11.1 |
| capability-level | enum | REQUIRED | unsupported, basic, standard, advanced, expert |
| confidence-basis | enum | REQUIRED | vendor-declared, benchmarked, validated, manually-approved, inferred |
| supported-agent-roles | array | REQUIRED | Roles permitted to use capability |
| supported-input-types | array | REQUIRED | text, markdown, json, yaml, code, artifact, image, file, table |
| supported-output-contracts | array | REQUIRED | Output contracts supported |
| context-requirements | array | CONDITIONAL | Context patterns required for effective use |
| limitations | array | CONDITIONAL | Known limitations |
| validation-record-id | string | CONDITIONAL | Capability validation record where available |
| last-validated-at | ISO 8601 | CONDITIONAL | Last capability validation time |

### 11.3 Capability Declaration Rules

A model MUST NOT claim a capability without a capability declaration.

A capability with confidence-basis inferred MUST NOT be used for High or Critical risk systems unless governance approves.

A capability required for artifact generation SHOULD be validated before use in Autonomous-Ready execution.

A capability required for security validation MUST be validated or manually approved before use in High or Critical risk systems.

---

## 12.0 Capability Validation

Capability validation determines whether a model’s declared capability may be trusted for routing.

### 12.1 Capability Validation Methods

| Method                 | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| provider-declaration   | Capability declared by provider                              |
| platform-benchmark     | Capability tested by platform benchmark                      |
| role-contract-test     | Model tested against agent role output contract              |
| historical-performance | Capability inferred from platform outcomes                   |
| governance-approval    | Capability approved by authorized role                       |
| manual-evaluation      | Human reviewer evaluated capability                          |
| external-certification | Capability supported by external certification or evaluation |

### 12.2 Capability Validation Record Schema

| Field | Type | Required | Description |
|---|---|---|
| validation-record-id | string | REQUIRED | Unique validation record |
| model-profile-id | string | REQUIRED | Model profile evaluated |
| capability-name | string | REQUIRED | Capability evaluated |
| validation-method | enum | REQUIRED | Method from §12.1 |
| validation-scope | string | REQUIRED | Scope of validation |
| validation-outcome | enum | REQUIRED | passed, failed, warning, expired, not-evaluated |
| evidence | array | CONDITIONAL | Evidence supporting outcome |
| evaluator | string | REQUIRED | Tool, role, benchmark, or subsystem evaluating |
| evaluated-at | ISO 8601 | REQUIRED | Evaluation time |
| expires-at | ISO 8601 | CONDITIONAL | Expiration when validation is time-bound |

### 12.3 Capability Validation Rules

A failed capability validation MUST prevent routing for that capability.

An expired capability validation MUST NOT satisfy a routing requirement unless governance permits temporary use.

Capability validation evidence SHOULD be retained for audit.

A model selected for a role MUST satisfy every required capability for that role invocation.

---

## 13.0 Agent Role Model Requirements

This section maps agent roles to required model capabilities.

### 13.1 Default Role Capability Requirements

| Agent Role                | Required Capabilities                                                          |
| ------------------------- | ------------------------------------------------------------------------------ |
| Specification Interpreter | requirement-interpretation, long-context, schema-constrained-output            |
| Architecture Planner      | architecture-reasoning, planning-reasoning, decision-record-output             |
| Construction Planner      | planning-reasoning, impact-analysis-reasoning, table-output                    |
| Implementation Generator  | code-generation, schema-constrained-output, artifact-context                   |
| Repair Analyst            | failure-analysis, repair-proposal, validation-context                          |
| Test Generator            | test-generation, requirement-interpretation, schema-constrained-output         |
| Review Agent              | artifact-review, traceability-review, decision-record-output                   |
| Security Validator        | security-review, policy-interpretation, data-protection-review                 |
| Deployment Preparer       | deployment-preparation, infrastructure-reasoning, operational-readiness-review |

### 13.2 Role Capability Rules

An agent invocation MUST declare required model capabilities.

The Model Router MUST reject models that do not satisfy required capabilities.

A model MAY be selected for a role only if the role is listed in supported-agent-roles or governance authorizes exceptional use.

A role requiring structured output MUST select a model that supports the required output contract or select an adapter capable of reliable output normalization.

---

## 14.0 Model Selection and Routing

Model selection determines which model receives a given invocation.

### 14.1 Routing Inputs

The Model Router MUST consider:

* agent role
* task type
* required capabilities
* required output contract
* context size
* input type
* sensitivity classification
* risk tier
* provider trust profile
* model trust profile
* local-first policy
* latency constraints
* cost constraints
* availability
* fallback policy
* governance restrictions
* historical performance where available

### 14.2 Routing Decision Schema

| Field | Type | Required | Description |
|---|---|---|
| routing-decision-id | string | REQUIRED | Unique routing decision identifier |
| agent-invocation-id | string | REQUIRED | Agent invocation requiring model |
| task-id | string | REQUIRED | Related task |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| required-capabilities | array | REQUIRED | Capabilities required |
| candidate-models | array | REQUIRED | Models considered |
| rejected-models | array | CONDITIONAL | Rejected models and reasons |
| selected-model-profile-id | string | REQUIRED | Selected model |
| selected-provider-id | string | REQUIRED | Selected provider |
| routing-policy-id | string | REQUIRED | Routing policy used |
| selection-rationale | string | REQUIRED | Explanation for selected model |
| governance-check-id | string | CONDITIONAL | Governance check affecting routing |
| decided-at | ISO 8601 | REQUIRED | Decision time |
| decided-by | string | REQUIRED | Model Router component |

### 14.3 Routing Rules

The router MUST NOT select a model that lacks required capabilities.

The router MUST NOT select a model whose provider is revoked, disabled, or prohibited.

The router MUST NOT select a model whose trust profile forbids the context sensitivity or risk tier.

When local-first mode is active, the router MUST prefer local models that satisfy required capabilities, unless governance or routing policy permits remote selection.

When multiple models satisfy requirements, the router MUST apply deterministic tie-breaking rules.

A routing decision MUST be recorded for every model invocation.

---

## 15.0 Routing Policies

Routing policies define how the Model Router ranks and selects models.

### 15.1 Routing Policy Schema

| Field | Type | Required | Description |
|---|---|---|
| routing-policy-id | string | REQUIRED | Unique routing policy identifier |
| policy-name | string | REQUIRED | Human-readable name |
| scope | enum | REQUIRED | global, project, specification, agent-role, risk-tier, environment |
| priority-order | array | REQUIRED | Ordered routing criteria |
| local-first | boolean | REQUIRED | Whether local models are preferred |
| remote-allowed | boolean | REQUIRED | Whether remote providers may be used |
| restricted-context-allowed | boolean | REQUIRED | Whether restricted context may be sent |
| fallback-allowed | boolean | REQUIRED | Whether fallback routing is permitted |
| max-cost-profile | enum | CONDITIONAL | Maximum cost profile allowed |
| max-latency-profile | enum | CONDITIONAL | Maximum latency profile allowed |
| required-trust-level | string | CONDITIONAL | Required trust profile |
| active-from | ISO 8601 | REQUIRED | Effective time |
| status | enum | REQUIRED | draft, active, superseded, revoked |

### 15.2 Default Routing Priority

Unless overridden by governance, routing SHOULD prioritize:

1. governance permission
2. required capability satisfaction
3. context sensitivity compatibility
4. local-first preference
5. role suitability
6. output contract compatibility
7. latency profile
8. cost profile
9. historical success rate

### 15.3 Routing Policy Rules

Routing policies MUST be versioned.

Routing policies affecting High or Critical systems MUST be governed.

A revoked routing policy MUST NOT be used.

A routing policy change during active execution MUST trigger impact evaluation when it changes model selection behavior.

---

## 16.0 Model Invocation Contract

This section defines the standard model invocation request.

### 16.1 Model Invocation Request Schema

| Field | Type | Required | Description |
|---|---|---|
| model-invocation-id | string | REQUIRED | Unique invocation identifier |
| agent-invocation-id | string | REQUIRED | Agent invocation requesting model |
| agent-role | enum | REQUIRED | Agent role |
| task-id | string | REQUIRED | Related task |
| execution-graph-id | string | CONDITIONAL | Execution graph when active |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| selected-model-profile-id | string | REQUIRED | Model selected |
| selected-provider-id | string | REQUIRED | Provider selected |
| routing-decision-id | string | REQUIRED | Routing decision used |
| context-package-id | string | REQUIRED | Context package supplied |
| input-format | enum | REQUIRED | text, markdown, json, yaml, code, multimodal, mixed |
| output-contract-id | string | REQUIRED | Expected output contract |
| invocation-mode | enum | REQUIRED | synchronous, asynchronous, streaming, batch |
| parameters | object | CONDITIONAL | Temperature, limits, sampling, or provider-neutral settings |
| timeout-seconds | integer | REQUIRED | Invocation timeout |
| sensitivity-classification | enum | REQUIRED | public, internal, confidential, restricted |
| governance-check-id | string | CONDITIONAL | Governance authorization when required |
| requested-at | ISO 8601 | REQUIRED | Invocation request time |
| requested-by | string | REQUIRED | Model Integration Layer or Agent Orchestrator |

### 16.2 Invocation Rules

A model invocation MUST reference an agent invocation.

A model invocation MUST reference a routing decision.

A model invocation MUST reference a context package.

A model invocation MUST declare an output contract.

A model invocation MUST NOT proceed when governance blocks the selected provider, model, context, or task.

---

## 17.0 Model Invocation Result Contract

This section defines the standard model invocation result.

### 17.1 Model Invocation Result Schema

| Field | Type | Required | Description |
|---|---|---|
| model-invocation-result-id | string | REQUIRED | Unique result identifier |
| model-invocation-id | string | REQUIRED | Invocation request |
| agent-invocation-id | string | REQUIRED | Agent invocation |
| selected-model-profile-id | string | REQUIRED | Model used |
| selected-provider-id | string | REQUIRED | Provider used |
| outcome | enum | REQUIRED | completed, failed, timeout, cancelled, blocked, normalized, invalid-output |
| raw-output-reference | string | CONDITIONAL | Raw provider output reference where retained |
| normalized-output-reference | string | CONDITIONAL | Normalized output reference |
| output-validation-status | enum | REQUIRED | not-evaluated, passed, failed, warning |
| findings | array | CONDITIONAL | Findings from output validation or normalization |
| token-usage | object | CONDITIONAL | Token or size usage where available |
| duration-ms | integer | REQUIRED | Invocation duration |
| provider-status | string | CONDITIONAL | Provider-specific status normalized as metadata |
| completed-at | ISO 8601 | REQUIRED | Completion time |
| next-action | enum | REQUIRED | accept, normalize, retry, fallback, escalate, reject, halt |

### 17.2 Result Rules

A timeout MUST NOT be treated as completed.

An invalid-output outcome MUST NOT be accepted for downstream agent output use.

A result with output-validation-status failed MUST trigger retry, fallback, escalation, or rejection.

Raw output retention MUST follow governance and sensitivity rules.

---

## 18.0 Context Control

The Model Integration Layer MUST enforce context rules before invocation.

### 18.1 Context Control Checks

Before invocation, the Context Controller MUST verify:

* context-package-id exists
* context was assembled for the requesting agent role
* context sensitivity is compatible with selected provider and model
* context size fits selected model limits
* required entities and records are present
* excluded context is documented
* secrets are not present unless explicitly authorized
* untrusted content is marked or isolated
* governance constraints are included
* provider data retention profile is compatible with context sensitivity

### 18.2 Context Control Record Schema

| Field | Type | Required | Description |
|---|---|---|
| context-control-id | string | REQUIRED | Unique context control record |
| context-package-id | string | REQUIRED | Context package evaluated |
| model-invocation-id | string | REQUIRED | Invocation using context |
| selected-model-profile-id | string | REQUIRED | Model selected |
| sensitivity-classification | enum | REQUIRED | Context sensitivity |
| control-outcome | enum | REQUIRED | passed, failed, warning, redacted, escalated |
| redactions-applied | array | CONDITIONAL | Redactions applied |
| findings | array | CONDITIONAL | Findings from control checks |
| evaluated-at | ISO 8601 | REQUIRED | Evaluation time |
| evaluated-by | string | REQUIRED | Context Controller |

### 18.3 Context Control Rules

Context control MUST pass before model invocation.

A context package containing unauthorized secrets MUST block invocation.

A restricted context package MUST NOT be routed to a model or provider whose trust profile disallows restricted context.

If redaction changes required context, the invocation MUST be rejected or escalated unless the role contract accepts redacted context.

---

## 19.0 Structured Interaction and Output Contracts

Structured outputs allow platform components to consume model responses safely.

### 19.1 Output Contract Schema

| Field | Type | Required | Description |
|---|---|---|
| output-contract-id | string | REQUIRED | Unique output contract identifier |
| contract-name | string | REQUIRED | Human-readable contract name |
| contract-version | semver | REQUIRED | Contract version |
| applies-to-agent-roles | array | REQUIRED | Agent roles using contract |
| output-format | enum | REQUIRED | json, yaml, markdown, table, artifact-content, mixed |
| schema-reference | string | CONDITIONAL | Schema reference when structured |
| required-fields | array | REQUIRED | Fields required |
| allowed-output-types | array | REQUIRED | Output types permitted |
| validation-rules | array | REQUIRED | Rules used to validate output |
| fallback-contract-id | string | CONDITIONAL | Fallback contract when output fails |
| status | enum | REQUIRED | active, deprecated, disabled |

### 19.2 Structured Interaction Rules

An agent invocation requiring downstream automation MUST use an active output contract.

A model selected for structured output SHOULD support the required output format natively or through reliable adapter normalization.

A model response that does not satisfy the output contract MUST be rejected, retried, normalized if safe, or escalated.

Output contract versions MUST be recorded in model invocation records.

---

## 20.0 Output Normalization

Output normalization converts provider-specific responses into platform-compatible outputs.

### 20.1 Normalization Requirements

The Output Normalizer MUST:

* parse provider response
* extract the expected output payload
* remove provider-specific wrapper content
* validate against output contract
* identify missing required fields
* identify malformed content
* classify output confidence where applicable
* preserve raw output reference when required
* produce normalized output reference
* produce findings when normalization is incomplete or unsafe

### 20.2 Normalization Record Schema

| Field | Type | Required | Description |
|---|---|---|
| normalization-record-id | string | REQUIRED | Unique normalization identifier |
| model-invocation-id | string | REQUIRED | Invocation normalized |
| output-contract-id | string | REQUIRED | Contract applied |
| normalization-outcome | enum | REQUIRED | passed, failed, warning, partial |
| normalized-output-reference | string | CONDITIONAL | Normalized output location |
| findings | array | CONDITIONAL | Normalization findings |
| normalized-at | ISO 8601 | REQUIRED | Normalization time |
| normalized-by | string | REQUIRED | Normalizer component |

### 20.3 Normalization Rules

Normalization failure MUST prevent downstream use unless governance permits manual review.

Partial normalization MUST NOT satisfy a role contract unless the role contract accepts partial output.

A normalized output MUST remain linked to the original model invocation.

---

## 21.0 Model Failure and Error Handling

Model failures MUST be classified and handled predictably.

### 21.1 Model Error Classes

| Error Class                    | Description                                  | Default Handling                |
| ------------------------------ | -------------------------------------------- | ------------------------------- |
| model-provider-unavailable     | Provider unavailable                         | retry or fallback               |
| model-profile-not-found        | Selected model profile missing               | reject routing                  |
| model-capability-missing       | Model lacks required capability              | reject routing                  |
| model-governance-blocked       | Governance blocks model use                  | block or escalate               |
| model-context-too-large        | Context exceeds model limit                  | reduce context or fallback      |
| model-context-policy-violation | Context violates policy                      | block                           |
| model-timeout                  | Invocation exceeded timeout                  | retry then fallback or escalate |
| model-rate-limited             | Provider rate limit reached                  | retry with backoff or fallback  |
| model-authentication-failed    | Provider authentication failed               | halt or escalate                |
| model-output-empty             | No usable output returned                    | retry                           |
| model-output-invalid           | Output violates contract                     | retry, fallback, or escalate    |
| model-output-unsafe            | Output includes unsafe or prohibited content | block and escalate              |
| model-normalization-failed     | Output could not be normalized               | retry, fallback, or escalate    |
| model-routing-failed           | No eligible model found                      | escalate                        |
| model-fallback-exhausted       | Fallback options exhausted                   | escalate or fail task           |

### 21.2 Model Error Record Schema

| Field | Type | Required | Description |
|---|---|---|
| model-error-id | string | REQUIRED | Unique model error identifier |
| error-class | enum | REQUIRED | Error class from §21.1 |
| severity | enum | REQUIRED | blocking, high, medium, low |
| model-invocation-id | string | CONDITIONAL | Related invocation |
| routing-decision-id | string | CONDITIONAL | Related routing decision |
| model-profile-id | string | CONDITIONAL | Related model |
| provider-id | string | CONDITIONAL | Related provider |
| agent-invocation-id | string | CONDITIONAL | Related agent invocation |
| task-id | string | CONDITIONAL | Related task |
| message | string | REQUIRED | Human-readable explanation |
| default-handling | enum | REQUIRED | retry, fallback, block, escalate, reject, halt |
| handling-status | enum | REQUIRED | pending, applied, failed, escalated |
| detected-at | ISO 8601 | REQUIRED | Detection time |

### 21.3 Error Handling Rules

Model errors MUST be recorded.

Retryable model errors MUST respect runtime retry policy.

Governance-blocked model errors MUST NOT be retried with the same blocked model.

Output-invalid errors MAY be retried if retry policy permits.

Output-unsafe errors MUST block downstream use and escalate when policy requires.

---

## 22.0 Retry and Fallback

Fallback allows the platform to continue when a selected model fails, provided governance and routing policy permit fallback.

### 22.1 Fallback Preconditions

Fallback MAY occur only when:

* routing policy allows fallback
* an eligible fallback model exists
* fallback model satisfies required capabilities
* fallback model satisfies context sensitivity constraints
* governance permits fallback provider and model
* retry policy does not require escalation first

### 22.2 Fallback Record Schema

| Field | Type | Required | Description |
|---|---|---|
| fallback-record-id | string | REQUIRED | Unique fallback record |
| original-model-profile-id | string | REQUIRED | Initially selected model |
| fallback-model-profile-id | string | REQUIRED | Replacement model |
| provider-id | string | REQUIRED | Replacement provider |
| model-invocation-id | string | REQUIRED | Invocation being retried or replaced |
| fallback-reason | string | REQUIRED | Reason fallback occurred |
| capability-check-result | enum | REQUIRED | passed, failed |
| governance-check-id | string | CONDITIONAL | Governance check for fallback |
| selected-at | ISO 8601 | REQUIRED | Fallback decision time |

### 22.3 Fallback Rules

Fallback MUST produce a new routing decision or record an amendment to the original routing decision.

Fallback MUST NOT reduce required capability level unless governance approves degradation.

Fallback to a provider with weaker trust profile MUST require governance authorization when context is confidential or restricted.

Fallback exhaustion MUST escalate or fail the task.

---

## 23.0 Model Trust Profiles

Model trust profiles define where, how, and for what type of context a model may be used.

### 23.1 Model Trust Profile Schema

| Field | Type | Required | Description |
|---|---|---|
| trust-profile-id | string | REQUIRED | Unique trust profile identifier |
| trust-level | enum | REQUIRED | approved, restricted, experimental, prohibited |
| allowed-risk-tiers | array | REQUIRED | Risk tiers where model may be used |
| allowed-sensitivity-levels | array | REQUIRED | Context sensitivity levels allowed |
| allowed-agent-roles | array | REQUIRED | Agent roles allowed |
| allowed-environments | array | REQUIRED | Environments where model may run |
| external-transmission-allowed | boolean | REQUIRED | Whether context may leave local environment |
| provider-retention-allowed | boolean | REQUIRED | Whether provider may retain data |
| human-review-required | boolean | REQUIRED | Whether output requires human review |
| approval-required | boolean | REQUIRED | Whether invocation requires approval |
| policy-reference | string | CONDITIONAL | Governance policy controlling trust profile |
| status | enum | REQUIRED | active, superseded, revoked |

### 23.2 Trust Profile Rules

A prohibited model MUST NOT be used.

A restricted model MUST be used only within declared constraints.

A model requiring approval MUST NOT be invoked until approval is recorded.

A model trust profile change MUST be audited.

---

## 24.0 Governance and Policy Control

Model usage is governance-controlled.

### 24.1 Governed Model Actions

Governance MAY control:

* provider registration
* model registration
* external model use
* restricted context transmission
* use of experimental models
* model fallback
* model use by agent role
* model use by risk tier
* output use after failed normalization
* provider data retention
* model invocation for security-sensitive tasks

### 24.2 Governance Rules

The Model Integration Layer MUST request governance checks when model usage is governed.

A governance decision of block MUST prevent invocation.

A governance decision of approval-required MUST pause invocation until approval exists.

A governance decision of escalate MUST open escalation and prevent invocation until resolved.

Expired approvals, waivers, or overrides MUST NOT authorize model use.

---

## 25.0 Model Invocation Traceability

Every meaningful model interaction MUST be traceable.

### 25.1 Required Traceability Links

The platform MUST record links between:

* task and agent invocation
* agent invocation and model invocation
* model invocation and routing decision
* model invocation and context package
* model invocation and model profile
* model invocation and provider
* model invocation and normalized output
* normalized output and agent output
* model output and artifact candidate where applicable
* model output and decision record where applicable
* model error and affected task
* fallback record and model invocation

### 25.2 Traceability Rules

A model invocation used for artifact generation MUST be traceable to the artifact candidate.

A model invocation used for repair MUST be traceable to the failed validation and repair record.

A model invocation used for governance support MUST be traceable to the governance event or decision.

A task MUST NOT be marked completed when required model traceability is missing.

---

## 26.0 Model Observability

Model observability provides operational visibility into model usage, quality, failure, latency, cost, routing, and output validation.

### 26.1 Required Telemetry Events

The platform SHOULD emit telemetry for:

* model routing requested
* routing decision made
* context control passed
* context control failed
* model invocation started
* model invocation completed
* model invocation failed
* model timeout
* model output normalization passed
* model output normalization failed
* fallback selected
* fallback exhausted
* model governance blocked
* model capability validation failed

### 26.2 Minimum Telemetry Fields

| Field                 | Description                       |
| --------------------- | --------------------------------- |
| event-id              | Unique telemetry event identifier |
| event-type            | Event type                        |
| timestamp             | Event time                        |
| provider-id           | Provider identifier               |
| model-profile-id      | Model profile identifier          |
| agent-role            | Agent role                        |
| task-id               | Task identifier                   |
| specification-id      | System identifier                 |
| specification-version | Specification version             |
| routing-decision-id   | Routing decision identifier       |
| model-invocation-id   | Invocation identifier             |
| outcome               | Event outcome                     |
| duration-ms           | Duration where applicable         |
| severity              | Severity where applicable         |

### 26.3 Observability Rules

Telemetry MUST NOT expose restricted context content to unauthorized observers.

Model observability MUST be correlated with agent, runtime, governance, and traceability records.

Model failure telemetry SHOULD support operational alerting.

---

## 27.0 Model Lifecycle Management

Models and providers evolve. The platform MUST manage lifecycle state explicitly.

### 27.1 Model Lifecycle States

| State        | Meaning                                          |
| ------------ | ------------------------------------------------ |
| proposed     | Model submitted for registration                 |
| active       | Model available for routing                      |
| restricted   | Model available only under constraints           |
| experimental | Model available only for governed evaluation     |
| deprecated   | Model should not be selected for new workflows   |
| disabled     | Model temporarily unavailable                    |
| revoked      | Model must not be used                           |
| retired      | Model no longer active but retained historically |

### 27.2 Lifecycle Rules

A revoked model MUST NOT be invoked.

A deprecated model SHOULD NOT be selected for new invocations.

A disabled model MUST NOT be selected until reactivated.

A model lifecycle change MUST be audited.

Lifecycle changes affecting active execution MUST trigger impact evaluation.

---

## 28.0 Model Configuration Management

Model configuration affects output behavior and MUST be controlled.

### 28.1 Model Configuration Schema

| Field | Type | Required | Description |
|---|---|---|
| model-configuration-id | string | REQUIRED | Unique configuration identifier |
| model-profile-id | string | REQUIRED | Model configured |
| configuration-version | semver | REQUIRED | Configuration version |
| parameter-profile | object | REQUIRED | Provider-neutral parameters |
| provider-parameter-map | object | CONDITIONAL | Provider-specific parameter mapping |
| output-contract-defaults | array | CONDITIONAL | Default output contracts |
| safety-profile-id | string | CONDITIONAL | Safety or moderation profile |
| context-policy-id | string | CONDITIONAL | Context policy |
| routing-policy-id | string | CONDITIONAL | Routing policy |
| status | enum | REQUIRED | draft, active, superseded, revoked |
| configured-at | ISO 8601 | REQUIRED | Configuration time |
| configured-by | string | REQUIRED | Authority or subsystem |

### 28.2 Configuration Rules

Model configuration MUST be versioned.

The model configuration used for an invocation MUST be recorded.

Configuration changes that materially affect output behavior SHOULD trigger revalidation of model capability declarations.

A revoked model configuration MUST NOT be used for new invocations.

---

## 29.0 Model Quality and Performance Evaluation

The platform SHOULD evaluate model quality over time.

### 29.1 Model Evaluation Metrics

The platform MAY track:

* output contract pass rate
* retry rate
* fallback rate
* timeout rate
* average latency
* cost per invocation
* agent role success rate
* repair convergence contribution
* human review rejection rate
* security finding rate
* hallucination or unsupported claim findings
* deterministic validation pass rate for generated artifacts

### 29.2 Model Performance Record Schema

| Field | Type | Required | Description |
|---|---|---|
| performance-record-id | string | REQUIRED | Unique performance record |
| model-profile-id | string | REQUIRED | Model evaluated |
| provider-id | string | REQUIRED | Provider |
| evaluation-window-start | ISO 8601 | REQUIRED | Window start |
| evaluation-window-end | ISO 8601 | REQUIRED | Window end |
| metrics | object | REQUIRED | Metrics collected |
| interpretation | string | CONDITIONAL | Interpretation of performance |
| recommended-action | enum | REQUIRED | continue, restrict, revalidate, deprecate, escalate |
| recorded-at | ISO 8601 | REQUIRED | Record time |

### 29.3 Evaluation Rules

Model performance records SHOULD inform routing policy updates.

A model with repeated output contract failures SHOULD be restricted, revalidated, or deprecated.

A model used for High or Critical systems SHOULD have periodic performance review.

---

## 30.0 Security and Context Protection

The Model Integration Layer MUST protect context and outputs from misuse.

### 30.1 Security Requirements

The platform MUST:

* prevent unauthorized context transmission
* classify context sensitivity before invocation
* prevent secret exposure
* isolate untrusted content in prompts or input packages
* prevent model outputs from bypassing validation
* enforce provider data retention restrictions
* record restricted model usage
* detect prompt or context injection attempts where possible
* block unsafe or policy-violating outputs when detected

### 30.2 Prompt and Context Injection Rules

Untrusted specification content, artifact content, logs, tool output, or external data MUST NOT be allowed to override platform instructions, agent role boundaries, output contracts, governance controls, or safety rules.

Detected injection attempts MUST produce a security finding.

A context injection finding MAY block invocation, trigger redaction, or escalate according to policy.

---

## 31.0 Model Interaction Evidence Retention

Model interaction evidence supports audit, debugging, quality evaluation, and traceability.

### 31.1 Evidence Types

Model evidence MAY include:

* routing decisions
* context control records
* context package references
* model invocation requests
* raw model outputs
* normalized outputs
* output validation records
* error records
* fallback records
* governance decisions
* telemetry summaries

### 31.2 Evidence Rules

Evidence MUST be retained when model output contributes to:

* artifact generation
* repair proposal
* security finding
* governance decision
* deployment preparation
* architecture decision
* traceability decision
* High or Critical risk execution

Raw model outputs MAY be redacted or withheld according to sensitivity and retention policy, but metadata sufficient for audit MUST be preserved.

---

## 32.0 Model Integration Error Classes

This section defines model-integration-specific errors.

### 32.1 Error Classes

| Error Class                       | Description                         | Default Handling              |
| --------------------------------- | ----------------------------------- | ----------------------------- |
| model-provider-not-registered     | Provider is unknown                 | reject                        |
| model-provider-disabled           | Provider disabled                   | reject or fallback            |
| model-provider-revoked            | Provider revoked                    | block                         |
| model-adapter-unavailable         | Required adapter unavailable        | retry or fallback             |
| model-profile-missing             | Model profile missing               | reject                        |
| model-capability-invalid          | Capability declaration invalid      | reject routing                |
| model-capability-not-validated    | Required validation missing         | governance check or reject    |
| model-routing-no-candidate        | No eligible model found             | escalate                      |
| model-routing-policy-missing      | Required routing policy unavailable | halt or escalate              |
| model-context-control-failed      | Context control failed              | block                         |
| model-context-too-large           | Context exceeds model limit         | reduce, fallback, or escalate |
| model-invocation-timeout          | Invocation timed out                | retry                         |
| model-output-contract-failed      | Output failed output contract       | retry, fallback, or escalate  |
| model-output-normalization-failed | Normalization failed                | retry, fallback, or escalate  |
| model-fallback-not-permitted      | Fallback blocked by policy          | escalate                      |
| model-trust-profile-violation     | Model use violates trust profile    | block                         |
| model-governance-check-failed     | Governance check failed             | halt governed action          |
| model-evidence-retention-failed   | Required evidence not retained      | escalate                      |
| model-injection-detected          | Prompt/context injection detected   | block or escalate             |

### 32.2 Error Record Requirements

Model integration errors MUST include:

* error-id
* error-class
* severity
* provider-id when applicable
* model-profile-id when applicable
* agent-invocation-id when applicable
* model-invocation-id when applicable
* task-id when applicable
* message
* required-action
* detected-at

---

## 33.0 Conformance Requirements

### 33.1 Model Provider Conformance

A model provider conforms to ISL v2.5 if it:

* has a valid Provider Record
* uses a registered provider adapter
* declares supported models
* declares provider type and deployment location
* declares trust profile
* supports required invocation and response semantics through its adapter
* is governed and auditable

### 33.2 Model Profile Conformance

A model profile conforms to ISL v2.5 if it:

* identifies model, provider, version, and type
* declares supported agent roles
* declares capabilities explicitly
* declares context and output limits where known
* declares supported output formats
* declares data retention and trust profile
* records lifecycle status
* supports capability validation records where required

### 33.3 Model Router Conformance

A Model Router conforms to ISL v2.5 if it can:

* evaluate required capabilities
* filter models by provider status
* filter models by trust profile
* filter models by context sensitivity
* enforce local-first policy
* enforce routing policies
* produce routing decision records
* support deterministic tie-breaking
* support governed fallback
* reject invocations when no eligible model exists

### 33.4 Model Integration Layer Conformance

A Model Integration Layer conforms to ISL v2.5 if it can:

* register model providers
* register model profiles
* register provider adapters
* manage model capabilities
* validate capabilities
* route invocations
* control context
* invoke models through provider adapters
* normalize outputs
* validate output contracts
* handle model errors
* apply retry and fallback rules
* enforce governance controls
* preserve model traceability
* emit model telemetry
* retain required evidence

### 33.5 Platform Conformance

A platform conforms to ISL v2.5 if it:

* prevents agents from directly invoking model providers
* routes all model use through the Model Integration Layer
* enforces model governance and trust profiles
* supports local-first model operation where configured
* records routing decisions and model invocations
* prevents invalid model outputs from downstream use
* links model interactions to agent, task, artifact, traceability, and governance records
* supports lifecycle management for providers, adapters, models, capabilities, and configurations

---

## 34.0 Summary

ISL v2.5 defines the Model Integration and Abstraction Layer for the IMHOTEP platform. It establishes how agents obtain reasoning capability through registered, governed, provider-neutral model interfaces.

This revision strengthens the original model integration concept by making provider registration, model profiles, capability declaration, capability validation, routing policies, model selection, context control, invocation contracts, output contracts, normalization, retry, fallback, governance, traceability, telemetry, lifecycle management, and error handling explicit.

A conforming IMHOTEP platform MUST treat models as governed reasoning resources, not direct dependencies. Models may interpret, generate, review, repair, and reason, but they MUST do so through controlled abstraction, declared capability, governed context, structured output, traceable invocation, and runtime validation.
