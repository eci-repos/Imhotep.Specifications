# IMHOTEP Specification Language (ISL) v3.8

# The Model Interaction Protocol

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.7, ISL v2.1, ISL v2.2, ISL v2.4, ISL v2.5, ISL v2.6, ISL v3.4, ISL v3.5, ISL v3.6
**Supersedes:** ISL r0 v3.8 The Model Interaction Protocol
**Document Type:** Model Request, Response, Output Contract, and Validation Protocol Specification

---

## 1.0 Scope

This document defines the Model Interaction Protocol for the IMHOTEP platform. It specifies the formal request, response, context, instruction, output, normalization, validation, error, retry, fallback, traceability, governance, and telemetry rules used when the platform interacts with reasoning models.

This document applies to all model interactions initiated by agents, runtime services, model gateways, repair flows, review flows, security analysis flows, planning flows, artifact generation flows, and deployment preparation flows.

This document defines:

* model interaction principles
* reasoning transaction lifecycle
* model request envelope
* model response envelope
* context package references
* instruction structure
* output contract schema
* standard output contract types
* response normalization
* response validation
* retry and correction behavior
* fallback behavior
* deterministic follow-up requirements
* policy and governance controls
* traceability requirements
* telemetry requirements
* protocol versioning
* conformance requirements

---

## 2.0 Purpose

The purpose of the Model Interaction Protocol is to make model use reliable enough for autonomous software construction. A reasoning model may generate plans, code, repair proposals, tests, reviews, security findings, and deployment recommendations. These outputs can affect downstream platform behavior, so model interaction MUST be structured, bounded, traceable, validated, and governed.

The protocol exists to ensure that:

* models are invoked through formal requests rather than informal prompts
* every model call is associated with a task, agent role, output contract, and context package
* requests are provider-neutral
* responses are normalized before platform use
* responses are validated before downstream consumption
* invalid responses trigger correction, retry, fallback, escalation, or rejection
* model outputs do not bypass deterministic validation
* sensitive context is controlled
* local and remote model providers use the same logical protocol
* model interaction evidence remains auditable

---

## 3.0 Protocol Design Principles

### 3.1 Structured Reasoning Transactions

Every model interaction MUST be treated as a structured reasoning transaction.

A transaction MUST have a request envelope, response envelope, output contract, context reference, selected model profile, validation status, and traceability links.

### 3.2 Model-Agnostic Protocol

The protocol MUST be independent of any specific model provider, runtime, API, SDK, message format, or deployment topology.

Provider adapters MAY translate the protocol into provider-specific calls, but the platform-facing request and response contracts MUST remain provider-neutral.

### 3.3 Agent-Bounded Invocation

A model interaction MUST be framed by an agent role or authorized platform reasoning role.

The agent role defines the purpose, permitted behavior, required context, and output contract. The model supplies reasoning capability but MUST NOT redefine the agent role.

### 3.4 Context Minimization

A model request MUST include only the context necessary for the task.

The platform MUST NOT include unrelated specification, artifact, governance, telemetry, secret, or user data in model context.

### 3.5 Output Contract Enforcement

Every model request MUST declare an output contract.

A model response MUST NOT be accepted unless it satisfies the declared output contract or enters an explicit correction, review, waiver, or escalation path.

### 3.6 Deterministic Follow-Up

A model response MUST NOT be treated as final merely because it was generated.

Model outputs that affect artifacts, validation, security, governance, deployment, or lifecycle state MUST be followed by deterministic validation, repository admission, governance checks, or human review as applicable.

### 3.7 Traceability and Auditability

Every model interaction that affects construction, review, validation, repair, governance, or deployment preparation MUST be traceable to the task, agent role, context package, model profile, output contract, response validation result, and downstream action.

---

## 4.0 Reasoning Transaction Lifecycle

A model interaction follows a defined lifecycle.

| Stage                 | Purpose                                              |
| --------------------- | ---------------------------------------------------- |
| requested             | Agent or runtime requests model reasoning            |
| context-bound         | Context package is selected and controlled           |
| contract-bound        | Output contract is selected                          |
| model-routed          | Model router selects model provider and profile      |
| governance-checked    | Governance controls are evaluated when required      |
| invocation-dispatched | Request is sent through provider adapter             |
| response-received     | Raw response is received                             |
| normalized            | Raw response is converted into platform output form  |
| validated             | Normalized output is checked against output contract |
| accepted              | Valid response is accepted for downstream handling   |
| corrected             | Response correction requested                        |
| retried               | Request retried under retry policy                   |
| fallback-routed       | Request routed to alternate model                    |
| escalated             | Human or governance intervention required            |
| rejected              | Response rejected and not used                       |
| completed             | Transaction closed                                   |

### 4.1 Lifecycle Rules

A transaction MUST NOT enter invocation-dispatched until context-bound, contract-bound, and model-routed stages are complete.

A governed transaction MUST NOT enter invocation-dispatched until governance permits the invocation.

A transaction MUST NOT enter accepted state unless response validation passes or a valid governance waiver permits manual acceptance.

A rejected response MUST NOT be consumed by downstream platform components.

---

## 5.0 Model Interaction Request Envelope

The request envelope is the normative structure for model invocation.

### 5.1 Request Envelope Schema

| Field                        | Type     | Required    | Description                                                  |
| ---------------------------- | -------- | ----------- | ------------------------------------------------------------ |
| model-interaction-request-id | string   | REQUIRED    | Unique request identifier                                    |
| protocol-version             | semver   | REQUIRED    | Model Interaction Protocol version                           |
| agent-invocation-id          | string   | REQUIRED    | Agent invocation causing request                             |
| agent-role                   | enum     | REQUIRED    | Agent role framing the request                               |
| agent-implementation-id      | string   | CONDITIONAL | Agent implementation selected                                |
| task-id                      | string   | REQUIRED    | Construction, review, repair, validation, or deployment task |
| execution-graph-id           | string   | CONDITIONAL | Runtime execution graph                                      |
| loop-run-id                  | string   | CONDITIONAL | Autonomous loop run                                          |
| lifecycle-run-id             | string   | CONDITIONAL | SDLC lifecycle run                                           |
| specification-id             | string   | REQUIRED    | Specification identifier                                     |
| specification-version        | semver   | REQUIRED    | Specification version                                        |
| context-package-id           | string   | REQUIRED    | Context package supplied                                     |
| output-contract-id           | string   | REQUIRED    | Expected output contract                                     |
| instruction-template-id      | string   | REQUIRED    | Instruction template used                                    |
| reasoning-objective          | string   | REQUIRED    | Bounded objective of request                                 |
| constraints                  | array    | REQUIRED    | Operational, governance, security, and role constraints      |
| selected-model-profile-id    | string   | REQUIRED    | Model profile selected by router                             |
| selected-provider-id         | string   | REQUIRED    | Provider selected                                            |
| routing-decision-id          | string   | REQUIRED    | Routing decision                                             |
| invocation-mode              | enum     | REQUIRED    | synchronous, asynchronous, streaming, batch                  |
| requested-output-format      | enum     | REQUIRED    | json, yaml, markdown, code, mixed, artifact-content          |
| timeout-seconds              | integer  | REQUIRED    | Invocation timeout                                           |
| retry-policy-id              | string   | CONDITIONAL | Retry policy                                                 |
| sensitivity-classification   | enum     | REQUIRED    | public, internal, confidential, restricted                   |
| governance-check-id          | string   | CONDITIONAL | Governance check authorizing request                         |
| correlation-id               | string   | REQUIRED    | Correlation identifier                                       |
| requested-at                 | ISO 8601 | REQUIRED    | Request timestamp                                            |
| requested-by                 | string   | REQUIRED    | Requesting subsystem                                         |

### 5.2 Request Envelope Rules

A request MUST include agent-role, task-id, context-package-id, output-contract-id, selected-model-profile-id, and correlation-id.

A request MUST identify specification-id and specification-version.

A request MUST declare timeout-seconds.

A request MUST declare sensitivity-classification.

A request MUST NOT include raw secrets.

A request MUST NOT include provider-specific fields except inside adapter-private metadata not visible to agent or runtime contracts.

---

## 6.0 Context Package Reference

The request envelope references a context package rather than embedding uncontrolled context.

### 6.1 Context Package Summary Schema

| Field                       | Type    | Required    | Description                                |
| --------------------------- | ------- | ----------- | ------------------------------------------ |
| context-package-id          | string  | REQUIRED    | Unique context package identifier          |
| context-package-version     | semver  | REQUIRED    | Context package version                    |
| assembled-for-agent-role    | enum    | REQUIRED    | Agent role                                 |
| assembled-for-task-id       | string  | REQUIRED    | Task                                       |
| included-context-categories | array   | REQUIRED    | Categories included                        |
| excluded-context-categories | array   | CONDITIONAL | Categories excluded                        |
| source-entity-ids           | array   | CONDITIONAL | Canonical entities included                |
| artifact-ids                | array   | CONDITIONAL | Artifacts included                         |
| validation-result-ids       | array   | CONDITIONAL | Validation results included                |
| governance-record-ids       | array   | CONDITIONAL | Governance records included                |
| traceability-record-ids     | array   | CONDITIONAL | Traceability records included              |
| sensitivity-classification  | enum    | REQUIRED    | public, internal, confidential, restricted |
| token-or-size-estimate      | integer | CONDITIONAL | Size estimate                              |
| redaction-status            | enum    | REQUIRED    | none, redacted, summarized, restricted     |
| context-control-id          | string  | REQUIRED    | Context control result                     |

### 6.2 Context Package Rules

The context package MUST be assembled before model invocation.

The context package MUST be controlled for sensitivity, redaction, relevance, and size.

The model request MUST reference context-package-id.

Restricted context MUST NOT be sent to a model whose trust profile disallows restricted context.

Context that contains secrets MUST block invocation unless explicitly permitted by governance.

---

## 7.0 Instruction Structure

The instruction structure defines how a request is framed for the model.

### 7.1 Required Instruction Sections

An instruction template MUST include:

| Section                   | Required | Purpose                                                            |
| ------------------------- | -------- | ------------------------------------------------------------------ |
| protocol-header           | YES      | Identifies protocol version and output format expectations         |
| agent-role                | YES      | Defines role boundary                                              |
| task-objective            | YES      | Defines the reasoning objective                                    |
| permitted-actions         | YES      | Defines what the model may do                                      |
| prohibited-actions        | YES      | Defines what the model must not do                                 |
| context-summary           | YES      | Summarizes context package contents                                |
| constraints               | YES      | Defines governance, security, artifact, and validation constraints |
| output-contract           | YES      | Defines required response shape                                    |
| traceability-requirements | YES      | Requires identifiers and references                                |
| deterministic-follow-up   | YES      | States required post-processing expectations                       |
| failure-behavior          | YES      | Defines what to return if unable to comply                         |

### 7.2 Instruction Rules

Instructions MUST distinguish platform instructions from untrusted context.

Instructions MUST identify the required output contract.

Instructions MUST direct the model to return only the requested output format when machine parsing is required.

Instructions MUST NOT invite the model to alter role, governance, validation, or traceability requirements.

If the model cannot satisfy the request, the instruction MUST require a structured failure response.

---

## 8.0 Output Contract Model

An output contract defines the response shape, validation rules, and downstream handling requirements.

### 8.1 Output Contract Schema

| Field                            | Type    | Required    | Description                                                                                                                                               |
| -------------------------------- | ------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| output-contract-id               | string  | REQUIRED    | Unique contract identifier                                                                                                                                |
| contract-name                    | string  | REQUIRED    | Human-readable name                                                                                                                                       |
| contract-version                 | semver  | REQUIRED    | Contract version                                                                                                                                          |
| applies-to-agent-roles           | array   | REQUIRED    | Agent roles allowed                                                                                                                                       |
| applies-to-task-types            | array   | REQUIRED    | Task types allowed                                                                                                                                        |
| output-type                      | enum    | REQUIRED    | interpretation, plan-fragment, artifact-candidate, repair-proposal, test-plan, review-finding, security-finding, deployment-preparation, failure-response |
| required-format                  | enum    | REQUIRED    | json, yaml, markdown-table, typed-object, artifact-content                                                                                                |
| schema-reference                 | string  | CONDITIONAL | Schema or type reference                                                                                                                                  |
| required-fields                  | array   | REQUIRED    | Required fields                                                                                                                                           |
| optional-fields                  | array   | CONDITIONAL | Optional fields                                                                                                                                           |
| prohibited-fields                | array   | CONDITIONAL | Fields not permitted                                                                                                                                      |
| required-traceability-fields     | array   | REQUIRED    | Required identifiers                                                                                                                                      |
| validation-rules                 | array   | REQUIRED    | Rules used to validate output                                                                                                                             |
| deterministic-follow-up-required | boolean | REQUIRED    | Whether deterministic follow-up is required                                                                                                               |
| downstream-consumers             | array   | REQUIRED    | Platform components expected to consume output                                                                                                            |
| correction-allowed               | boolean | REQUIRED    | Whether correction request may be issued                                                                                                                  |
| fallback-allowed                 | boolean | REQUIRED    | Whether fallback model may be used                                                                                                                        |
| human-review-required            | boolean | REQUIRED    | Whether human review is required before use                                                                                                               |
| status                           | enum    | REQUIRED    | active, deprecated, disabled, revoked                                                                                                                     |

### 8.2 Output Contract Rules

Every model request MUST reference an active output contract.

A revoked or disabled output contract MUST NOT be used.

A model response MUST be validated against the exact output contract version used in the request.

A breaking contract change MUST increment major version.

A response that omits required traceability fields MUST fail validation.

---

## 9.0 Standard Output Contract Types

The protocol defines standard output contract types.

### 9.1 Interpretation Output

Used by Specification Interpreter.

Required fields:

* interpretation-id
* referenced-entity-ids
* interpreted-scope
* assumptions
* ambiguities
* recommendations
* confidence
* requires-human-review

### 9.2 Plan Fragment Output

Used by Architecture Planner or Construction Planner.

Required fields:

* plan-fragment-id
* referenced-entity-ids
* proposed-task-ids
* dependencies
* expected-artifacts
* validation-requirements
* planning-rationale
* risks

### 9.3 Artifact Candidate Output

Used by Implementation Generator, Test Generator, or Deployment Preparer.

Required fields:

* artifact-candidate-id
* artifact-type
* proposed-artifact-name
* source-entity-ids
* task-id
* content-reference or content-payload
* derivation-type
* validation-expectations
* assumptions
* limitations

### 9.4 Repair Proposal Output

Used by Repair Analyst.

Required fields:

* repair-proposal-id
* failed-validation-result-id
* failed-artifact-ids
* likely-root-cause
* proposed-corrections
* affected-artifacts
* revalidation-required
* convergence-risk
* recommended-next-action

### 9.5 Test Plan Output

Used by Test Generator.

Required fields:

* test-plan-id
* referenced-requirement-ids
* referenced-interface-ids
* test-scenarios
* expected-results
* generated-test-artifact-candidates
* validation-tool-capabilities
* coverage-notes

### 9.6 Review Finding Output

Used by Review Agent.

Required fields:

* review-finding-id
* reviewed-object-id
* reviewed-object-type
* finding-category
* severity
* finding-description
* evidence-reference
* recommended-action
* blocks-progression

### 9.7 Security Finding Output

Used by Security Validator.

Required fields:

* security-finding-id
* affected-object-id
* affected-object-type
* security-category
* severity
* policy-reference
* finding-description
* recommended-action
* blocks-progression
* governance-required

### 9.8 Deployment Preparation Output

Used by Deployment Preparer.

Required fields:

* deployment-preparation-output-id
* package-id or artifact-ids
* target-environment
* deployment-artifact-candidates
* environment-assumptions
* operational-readiness-findings
* validation-requirements
* authorization-required

### 9.9 Failure Response Output

Used when the model cannot satisfy the request.

Required fields:

* failure-response-id
* failure-category
* failure-reason
* missing-context
* contract-field-unavailable
* recommended-next-action
* retry-recommended
* escalation-recommended

---

## 10.0 Model Response Envelope

The response envelope is the normative structure returned to the platform after provider invocation and normalization.

### 10.1 Response Envelope Schema

| Field                            | Type     | Required    | Description                                                                                |
| -------------------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------ |
| model-interaction-response-id    | string   | REQUIRED    | Unique response identifier                                                                 |
| model-interaction-request-id     | string   | REQUIRED    | Request being answered                                                                     |
| protocol-version                 | semver   | REQUIRED    | Protocol version used                                                                      |
| selected-model-profile-id        | string   | REQUIRED    | Model used                                                                                 |
| selected-provider-id             | string   | REQUIRED    | Provider used                                                                              |
| raw-provider-response-reference  | string   | CONDITIONAL | Raw response reference if retained                                                         |
| normalized-output-reference      | string   | CONDITIONAL | Normalized response reference                                                              |
| output-contract-id               | string   | REQUIRED    | Output contract used                                                                       |
| response-outcome                 | enum     | REQUIRED    | completed, failed, timeout, cancelled, blocked, invalid-output, normalized                 |
| output-validation-status         | enum     | REQUIRED    | not-evaluated, passed, failed, warning, waived                                             |
| findings                         | array    | CONDITIONAL | Validation, normalization, or policy findings                                              |
| token-or-size-usage              | object   | CONDITIONAL | Usage data where available                                                                 |
| duration-ms                      | integer  | REQUIRED    | Invocation duration                                                                        |
| retry-count                      | integer  | REQUIRED    | Retry count at response time                                                               |
| fallback-used                    | boolean  | REQUIRED    | Whether fallback model was used                                                            |
| deterministic-follow-up-required | boolean  | REQUIRED    | Whether follow-up is required                                                              |
| next-action                      | enum     | REQUIRED    | accept, correct, retry, fallback, validate-artifacts, human-review, escalate, reject, halt |
| correlation-id                   | string   | REQUIRED    | Correlation identifier                                                                     |
| received-at                      | ISO 8601 | REQUIRED    | Response receipt time                                                                      |

### 10.2 Response Envelope Rules

A response MUST reference the original request.

A response MUST identify the model and provider used.

A response MUST identify output-contract-id.

A timeout MUST NOT be marked completed.

A response with output-validation-status failed MUST NOT be accepted.

A response requiring deterministic follow-up MUST not be treated as final workflow success.

---

## 11.0 Output Normalization

Output normalization converts raw model output into the declared contract structure.

### 11.1 Normalization Steps

The platform MUST:

1. Retrieve raw provider response.
2. Identify expected output contract.
3. Extract structured payload.
4. Remove irrelevant provider wrapper text.
5. Parse required format.
6. Map provider-specific fields to platform fields.
7. Preserve raw response reference if retention policy permits.
8. Produce normalized output reference.
9. Record normalization findings.

### 11.2 Normalization Record Schema

| Field                           | Type     | Required    | Description                      |
| ------------------------------- | -------- | ----------- | -------------------------------- |
| normalization-record-id         | string   | REQUIRED    | Unique normalization record      |
| model-interaction-request-id    | string   | REQUIRED    | Request normalized               |
| model-interaction-response-id   | string   | REQUIRED    | Response normalized              |
| output-contract-id              | string   | REQUIRED    | Contract applied                 |
| raw-provider-response-reference | string   | CONDITIONAL | Raw response reference           |
| normalized-output-reference     | string   | CONDITIONAL | Normalized output reference      |
| normalization-outcome           | enum     | REQUIRED    | passed, failed, warning, partial |
| findings                        | array    | CONDITIONAL | Normalization findings           |
| normalized-at                   | ISO 8601 | REQUIRED    | Normalization time               |
| normalized-by                   | string   | REQUIRED    | Normalizer component             |

### 11.3 Normalization Rules

A response MUST be normalized before platform consumption.

Partial normalization MUST NOT be accepted unless the output contract permits partial output.

Normalization failure MUST trigger correction, retry, fallback, escalation, or rejection.

---

## 12.0 Response Validation

Response validation checks normalized output against the output contract.

### 12.1 Validation Checks

The platform MUST check:

* output contract version match
* required field presence
* prohibited field absence
* field type correctness
* enum value correctness
* required traceability identifiers
* referenced entity existence
* referenced artifact existence where applicable
* task compatibility
* agent role compatibility
* security and sensitivity constraints
* deterministic follow-up flag correctness
* downstream consumer compatibility

### 12.2 Response Validation Record Schema

| Field                         | Type     | Required    | Description                                                      |
| ----------------------------- | -------- | ----------- | ---------------------------------------------------------------- |
| response-validation-id        | string   | REQUIRED    | Unique validation record                                         |
| model-interaction-response-id | string   | REQUIRED    | Response validated                                               |
| output-contract-id            | string   | REQUIRED    | Contract used                                                    |
| validation-outcome            | enum     | REQUIRED    | passed, failed, warning, waived                                  |
| failed-rules                  | array    | CONDITIONAL | Failed validation rules                                          |
| findings                      | array    | CONDITIONAL | Validation findings                                              |
| validated-at                  | ISO 8601 | REQUIRED    | Validation time                                                  |
| validated-by                  | string   | REQUIRED    | Validator component                                              |
| next-action                   | enum     | REQUIRED    | accept, correct, retry, fallback, human-review, escalate, reject |

### 12.3 Validation Rules

A response MUST NOT be accepted unless validation passes, warning is accepted under policy, or waiver exists.

A response missing required traceability identifiers MUST fail validation.

A response that exceeds the agent role boundary MUST fail validation.

A response containing prohibited content MUST be rejected or escalated.

---

## 13.0 Correction Requests

A correction request asks the same model to repair its response format or missing fields without changing task scope.

### 13.1 Correction Request Preconditions

Correction MAY be attempted only when:

* output contract allows correction
* original request is valid
* failure is limited to correctable structure, missing field, formatting, or minor ambiguity
* retry policy permits correction
* governance does not require escalation

### 13.2 Correction Request Schema

| Field                                 | Type     | Required | Description                    |
| ------------------------------------- | -------- | -------- | ------------------------------ |
| correction-request-id                 | string   | REQUIRED | Unique correction request      |
| original-model-interaction-request-id | string   | REQUIRED | Original request               |
| failed-response-validation-id         | string   | REQUIRED | Failed validation              |
| correction-reason                     | string   | REQUIRED | Reason correction is requested |
| failed-rules                          | array    | REQUIRED | Rules to correct               |
| unchanged-context-package-id          | string   | REQUIRED | Context package reused         |
| unchanged-output-contract-id          | string   | REQUIRED | Output contract reused         |
| correction-attempt                    | integer  | REQUIRED | Correction attempt count       |
| requested-at                          | ISO 8601 | REQUIRED | Request time                   |

### 13.3 Correction Rules

Correction MUST NOT change the task objective.

Correction MUST NOT add new context except validation findings and correction instructions.

Correction attempts MUST be bounded.

Correction failure MUST trigger retry, fallback, escalation, or rejection.

---

## 14.0 Retry and Fallback Behavior

The protocol supports retry and fallback, but both MUST be controlled.

### 14.1 Retry Rules

Retry MAY occur when:

* provider timeout occurred
* provider unavailable
* response incomplete
* output malformed
* transient routing or adapter failure occurred

Retry MUST NOT occur when:

* governance blocks model use
* context violates policy
* output contains security violation requiring escalation
* requested model lacks required capability
* retry limit has been reached

### 14.2 Fallback Rules

Fallback MAY occur only when:

* routing policy permits fallback
* eligible fallback model exists
* fallback model satisfies required capabilities
* fallback model trust profile permits the context sensitivity
* governance permits fallback provider
* output contract remains unchanged or compatible

### 14.3 Retry and Fallback Limits

| Parameter                                         |                       Default |
| ------------------------------------------------- | ----------------------------: |
| Maximum correction attempts per response          |                             2 |
| Maximum retry attempts per request                |                             3 |
| Maximum fallback attempts per request             |                             2 |
| Escalation threshold for repeated invalid outputs | 3 consecutive invalid outputs |

### 14.4 Retry/Fallback Rules

Retry and fallback attempts MUST be recorded.

Retry and fallback MUST preserve correlation identifiers.

Fallback MUST produce or amend routing decision records.

Repeated invalid outputs MUST escalate.

---

## 15.0 Deterministic Follow-Up

Model outputs that affect platform state require deterministic follow-up.

### 15.1 Required Follow-Up by Output Type

| Output Type            | Required Follow-Up                                         |
| ---------------------- | ---------------------------------------------------------- |
| artifact-candidate     | Artifact Repository admission and deterministic validation |
| repair-proposal        | Repair task creation and revalidation after modification   |
| test-plan              | Test artifact generation and test execution                |
| review-finding         | Finding classification and governed handling               |
| security-finding       | Security policy evaluation and governance routing          |
| deployment-preparation | Manifest validation and operational readiness checks       |
| plan-fragment          | Planning validation and dependency validation              |
| interpretation         | Canonical model or task consistency checks                 |

### 15.2 Follow-Up Rules

A model output MUST NOT directly promote artifacts.

A model output MUST NOT directly mark validation passed.

A model output MUST NOT directly approve governance gates.

A model output MUST NOT directly authorize deployment.

Deterministic follow-up MUST be recorded as linked evidence.

---

## 16.0 Policy and Governance Control

Model interaction MUST obey governance policy.

### 16.1 Governed Model Interaction Actions

Governance MAY control:

* external model provider use
* restricted context transmission
* experimental model use
* fallback to weaker trust profile
* security task model use
* High or Critical risk model interaction
* raw response retention
* human review bypass
* response waiver
* model-generated artifact acceptance

### 16.2 Governance Rules

A governance block MUST prevent invocation.

An approval-required decision MUST pause invocation.

A waiver-required decision MUST prevent acceptance until waiver exists.

A model interaction involving restricted context MUST be governed when policy requires it.

Governance decisions MUST be recorded and linked to the model transaction.

---

## 17.0 Security and Prompt Injection Controls

The protocol MUST protect model interaction from malicious or untrusted context.

### 17.1 Required Controls

The platform MUST:

* classify context sensitivity
* separate platform instructions from untrusted context
* mark untrusted context explicitly
* prevent untrusted content from redefining role, output contract, governance, or validation rules
* redact secrets
* block unauthorized restricted context
* record prompt or context injection findings
* reject outputs that attempt governance, traceability, validation, or role bypass

### 17.2 Injection Finding Schema

| Field                        | Type     | Required | Description                                                                                                   |
| ---------------------------- | -------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| injection-finding-id         | string   | REQUIRED | Unique finding identifier                                                                                     |
| model-interaction-request-id | string   | REQUIRED | Request affected                                                                                              |
| context-package-id           | string   | REQUIRED | Context package affected                                                                                      |
| finding-type                 | enum     | REQUIRED | instruction-override, secret-exfiltration, role-change, contract-bypass, governance-bypass, validation-bypass |
| severity                     | enum     | REQUIRED | critical, high, medium, low                                                                                   |
| detected-at                  | ISO 8601 | REQUIRED | Detection time                                                                                                |
| detected-by                  | string   | REQUIRED | Detector component                                                                                            |
| handling                     | enum     | REQUIRED | block, redact, sanitize, escalate, warn                                                                       |

### 17.3 Security Rules

An instruction-override attempt MUST NOT alter platform instructions.

A governance-bypass attempt MUST be rejected or escalated.

A secret-exfiltration attempt MUST block invocation or output acceptance.

Security findings MUST be linked to governance where required.

---

## 18.0 Streaming Interaction

Some providers may support streaming responses.

### 18.1 Streaming Rules

Streaming MAY be used only when:

* output contract permits streaming
* partial output cannot be consumed as final output
* stream completion is detected
* final assembled response is normalized and validated

### 18.2 Streaming Record Schema

| Field                        | Type     | Required    | Description                                        |
| ---------------------------- | -------- | ----------- | -------------------------------------------------- |
| streaming-record-id          | string   | REQUIRED    | Unique streaming record                            |
| model-interaction-request-id | string   | REQUIRED    | Request streamed                                   |
| stream-started-at            | ISO 8601 | REQUIRED    | Stream start                                       |
| stream-ended-at              | ISO 8601 | CONDITIONAL | Stream end                                         |
| chunk-count                  | integer  | REQUIRED    | Number of chunks received                          |
| stream-outcome               | enum     | REQUIRED    | completed, interrupted, timeout, cancelled, failed |
| assembled-response-reference | string   | CONDITIONAL | Final assembled response                           |

### 18.3 Streaming Validation Rules

Individual chunks MUST NOT be treated as accepted model output unless the output contract explicitly supports chunk-level validation.

The assembled response MUST pass normal normalization and validation.

Interrupted streams MUST trigger retry, fallback, escalation, or rejection.

---

## 19.0 Batch Interaction

Batch interaction allows multiple model requests to be grouped.

### 19.1 Batch Request Rules

A batch request MAY contain multiple model interaction requests only when:

* each request has its own request identifier
* each request has its own output contract
* each request has its own context package or declared shared context
* failures can be isolated per request
* traceability can be recorded per request

### 19.2 Batch Interaction Record Schema

| Field                     | Type     | Required    | Description                           |
| ------------------------- | -------- | ----------- | ------------------------------------- |
| batch-interaction-id      | string   | REQUIRED    | Unique batch identifier               |
| protocol-version          | semver   | REQUIRED    | Protocol version                      |
| request-ids               | array    | REQUIRED    | Requests included                     |
| shared-context-package-id | string   | CONDITIONAL | Shared context if used                |
| batch-outcome             | enum     | REQUIRED    | completed, partial, failed, cancelled |
| completed-request-ids     | array    | CONDITIONAL | Completed requests                    |
| failed-request-ids        | array    | CONDITIONAL | Failed requests                       |
| created-at                | ISO 8601 | REQUIRED    | Batch creation time                   |
| completed-at              | ISO 8601 | CONDITIONAL | Batch completion time                 |

### 19.3 Batch Rules

A failed item in a batch MUST NOT invalidate successful items unless dependency or policy requires it.

Batch responses MUST be normalized and validated per request.

Batch interactions MUST preserve per-request traceability.

---

## 20.0 Model Interaction Traceability

Every model transaction MUST be traceable.

### 20.1 Required Traceability Links

The platform MUST record links between:

* model request and agent invocation
* model request and task
* model request and context package
* model request and output contract
* model request and routing decision
* model response and selected model profile
* model response and normalized output
* normalized output and response validation result
* response validation result and downstream action
* artifact candidate and model response where applicable
* repair proposal and failed validation where applicable
* fallback response and original request where applicable

### 20.2 Traceability Rules

A model output used for artifact generation MUST be traceable to the generated artifact candidate.

A model output used for repair MUST be traceable to the failed validation result.

A model output used for governance support MUST be traceable to the governance record.

A task MUST NOT complete when required model interaction traceability is missing.

---

## 21.0 Model Interaction Telemetry

The protocol MUST emit telemetry for operational visibility.

### 21.1 Required Telemetry Events

The platform SHOULD emit:

* model-interaction-request-created
* model-context-bound
* model-output-contract-bound
* model-routing-bound
* model-governance-check-completed
* model-invocation-dispatched
* model-response-received
* model-response-normalized
* model-response-validation-passed
* model-response-validation-failed
* model-correction-requested
* model-retry-requested
* model-fallback-selected
* model-interaction-escalated
* model-interaction-completed
* model-interaction-rejected

### 21.2 Telemetry Rules

Telemetry MUST include model-interaction-request-id.

Telemetry MUST include correlation-id.

Telemetry SHOULD include agent-role, task-id, selected-model-profile-id, and output-contract-id.

Telemetry MUST NOT expose restricted context or raw secrets.

---

## 22.0 Model Interaction Records and Retention

Model interaction records support audit, debugging, quality evaluation, and replay analysis.

### 22.1 Required Records

The platform MUST retain:

* request envelope
* response envelope
* context package summary
* routing decision reference
* output contract reference
* normalization record
* response validation record
* correction records where applicable
* retry and fallback records where applicable
* governance records where applicable
* traceability links
* telemetry summary

### 22.2 Retention Rules

Raw provider responses MAY be retained only when policy permits.

Restricted context MUST NOT be retained in raw form unless governance permits it.

High and Critical risk systems SHOULD retain model interaction metadata as audit-support evidence.

Records MUST be queryable by request-id, task-id, agent-invocation-id, specification-id, and model-profile-id.

---

## 23.0 Protocol Error Classes

This section defines protocol-specific error classes.

| Error Class                              | Description                                | Default Handling                      |
| ---------------------------------------- | ------------------------------------------ | ------------------------------------- |
| protocol-request-missing-required-field  | Required request field missing             | reject                                |
| protocol-request-invalid-version         | Unsupported protocol version               | reject                                |
| protocol-context-not-bound               | Context package missing or unavailable     | reject                                |
| protocol-context-policy-violation        | Context violates sensitivity or policy     | block                                 |
| protocol-output-contract-missing         | Output contract missing                    | reject                                |
| protocol-output-contract-disabled        | Output contract disabled or revoked        | reject                                |
| protocol-instruction-template-missing    | Instruction template missing               | reject                                |
| protocol-model-routing-missing           | Routing decision missing                   | reject                                |
| protocol-governance-blocked              | Governance blocks invocation               | block                                 |
| protocol-provider-timeout                | Provider invocation timed out              | retry                                 |
| protocol-provider-error                  | Provider returned error                    | retry or fallback                     |
| protocol-response-empty                  | No response content returned               | retry                                 |
| protocol-response-malformed              | Response cannot be parsed                  | correct or retry                      |
| protocol-normalization-failed            | Response normalization failed              | correct, retry, or fallback           |
| protocol-validation-failed               | Response failed output contract validation | correct, retry, fallback, or escalate |
| protocol-traceability-missing            | Required traceability missing              | block downstream use                  |
| protocol-deterministic-follow-up-missing | Required follow-up not executed            | block completion                      |
| protocol-security-injection-detected     | Prompt or context injection detected       | block or escalate                     |
| protocol-fallback-exhausted              | No valid fallback remains                  | escalate                              |
| protocol-retry-limit-reached             | Retry limit reached                        | escalate                              |
| protocol-record-retention-failed         | Required record retention failed           | escalate                              |

### 23.1 Protocol Error Record Schema

| Field                         | Type     | Required    | Description                                             |
| ----------------------------- | -------- | ----------- | ------------------------------------------------------- |
| protocol-error-id             | string   | REQUIRED    | Unique error identifier                                 |
| error-class                   | enum     | REQUIRED    | Error class                                             |
| severity                      | enum     | REQUIRED    | blocking, high, medium, low                             |
| model-interaction-request-id  | string   | CONDITIONAL | Request affected                                        |
| model-interaction-response-id | string   | CONDITIONAL | Response affected                                       |
| agent-invocation-id           | string   | CONDITIONAL | Agent invocation affected                               |
| task-id                       | string   | CONDITIONAL | Task affected                                           |
| output-contract-id            | string   | CONDITIONAL | Contract affected                                       |
| message                       | string   | REQUIRED    | Human-readable explanation                              |
| default-handling              | enum     | REQUIRED    | reject, block, correct, retry, fallback, escalate, halt |
| required-action               | string   | REQUIRED    | Required remediation                                    |
| detected-at                   | ISO 8601 | REQUIRED    | Detection time                                          |

---

## 24.0 Protocol Versioning and Compatibility

The protocol MUST be versioned.

### 24.1 Versioning Rules

Protocol versions MUST use semantic versioning.

A breaking change to request envelope, response envelope, output contract schema, or validation semantics MUST increment major version.

A backward-compatible field addition SHOULD increment minor version.

A clarification or non-structural correction MAY increment patch version.

Requests and responses MUST include protocol-version.

### 24.2 Compatibility Rules

A provider adapter MUST declare supported protocol versions.

An output contract MUST declare supported protocol versions where compatibility is limited.

A model interaction MUST be rejected if no compatible protocol version is available.

Protocol migrations MUST preserve traceability across versions.

---

## 25.0 Protocol Testing Requirements

The Model Interaction Protocol MUST be testable.

### 25.1 Required Test Categories

| Test Category           | Purpose                                              |
| ----------------------- | ---------------------------------------------------- |
| request-schema          | Validate request envelope required fields            |
| response-schema         | Validate response envelope required fields           |
| context-control         | Validate context binding, sensitivity, and redaction |
| instruction-template    | Validate instruction structure                       |
| output-contract         | Validate output contract schema                      |
| normalization           | Validate raw response to normalized output mapping   |
| response-validation     | Validate pass/fail behavior                          |
| correction              | Validate correction request behavior                 |
| retry-fallback          | Validate bounded retry and fallback                  |
| security-injection      | Validate injection detection and handling            |
| traceability            | Validate required traceability links                 |
| deterministic-follow-up | Validate downstream validation requirements          |
| protocol-versioning     | Validate version compatibility                       |

### 25.2 Testing Rules

Every active output contract MUST have validation tests.

Every provider adapter SHOULD have protocol compatibility tests.

Every instruction template SHOULD have contract conformance tests.

Security injection tests MUST verify that untrusted context cannot override platform instructions.

Retry and fallback tests MUST verify termination limits.

---

## 26.0 Conformance Requirements

### 26.1 Request Conformance

A model interaction request conforms to ISL v3.8 if it:

* includes required request envelope fields
* references an agent invocation
* references a task
* references a context package
* references an active output contract
* references a selected model profile and routing decision
* declares sensitivity classification
* declares timeout
* includes correlation identifier
* passes context and governance checks

### 26.2 Response Conformance

A model interaction response conforms to ISL v3.8 if it:

* references the original request
* identifies selected model and provider
* references output contract
* records response outcome
* records normalization status
* records validation status
* declares next action
* preserves correlation identifier
* does not treat timeout or invalid output as success

### 26.3 Output Contract Conformance

An output contract conforms to ISL v3.8 if it:

* declares contract identity and version
* declares applicable agent roles and task types
* declares output type and required format
* declares required fields
* declares required traceability fields
* declares validation rules
* declares deterministic follow-up requirements
* declares correction, fallback, and human-review behavior
* is active and versioned

### 26.4 Protocol Implementation Conformance

A protocol implementation conforms to ISL v3.8 if it:

* creates request envelopes
* controls context packages
* binds output contracts
* routes through Model Integration Layer
* invokes providers through adapters
* normalizes responses
* validates responses
* supports correction, retry, fallback, escalation, and rejection
* records traceability
* emits telemetry
* enforces governance and security controls
* preserves required records
* enforces deterministic follow-up

### 26.5 Platform Conformance

A platform conforms to ISL v3.8 if it:

* prevents agents from issuing informal or untracked model calls
* prevents provider-specific responses from bypassing normalization
* prevents invalid model outputs from downstream use
* prevents model outputs from directly promoting artifacts, passing validation, approving governance, or authorizing deployment
* supports local and remote model providers through the same logical protocol
* maintains model interaction evidence for audit, debugging, and quality improvement

---

## 27.0 Summary

ISL v3.8 defines the Model Interaction Protocol for the IMHOTEP platform. It establishes the formal protocol through which agents and runtime services interact with reasoning models.

This revision turns the prior conceptual protocol into an implementable contract. It defines reasoning transaction lifecycle, request envelopes, response envelopes, context package references, instruction structure, output contracts, standard output types, normalization, validation, correction, retry, fallback, deterministic follow-up, governance, security, streaming, batch interaction, traceability, telemetry, records, retention, error classes, versioning, testing, and conformance.

A conforming Model Interaction Protocol MUST make model reasoning structured, bounded, provider-neutral, context-controlled, output-contracted, normalized, validated, traceable, governable, secure, observable, and unable to bypass deterministic platform controls.
