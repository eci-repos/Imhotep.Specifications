# IMHOTEP Specification Language (ISL) v1.6

# The Tool Integration Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.5
**Supersedes:** ISL r1 v1.6 Tool Integration Model
**Document Type:** Deterministic Tool Integration Specification

---

## 1.0 Scope

This document defines the Tool Integration Model used by the IMHOTEP platform to invoke, manage, interpret, and govern deterministic engineering tools during autonomous software construction. It establishes the requirements for tool registration, capability declaration, tool invocation, result normalization, validation outcome handling, security boundaries, telemetry, conflict resolution, tool lifecycle management, and conformance.

This document applies to all deterministic tools used during planning, generation support, validation, testing, repair, security analysis, infrastructure validation, packaging, deployment preparation, and operational readiness checks. It does not define the internal implementation of specific tools, the execution runtime itself, or the full plugin architecture; those are defined in later platform and implementation documents. This document defines the contract by which tools participate in the ISL construction lifecycle.

A conforming implementation MUST use this model to ensure that:

* reasoning outputs are validated by deterministic processes
* tools are registered before use
* tool capabilities are declared explicitly
* tool invocations are structured and auditable
* tool results are normalized before runtime consumption
* validation outcomes drive repair, escalation, or progression
* tool execution remains secure and controlled
* tool activity is traceable and observable
* tool conflicts and failures are handled predictably

---

## 2.0 Purpose of the Tool Integration Model

The purpose of the Tool Integration Model is to ensure that autonomous software construction does not rely on reasoning alone. Reasoning agents may interpret specifications, propose designs, generate code, create tests, and recommend repairs, but they cannot by themselves prove that artifacts compile, satisfy contracts, pass tests, comply with policies, or are deployable. Deterministic tools provide the objective feedback required to establish trust in generated artifacts.

In IMHOTEP, deterministic tools are not optional accessories. They are part of the system’s reliability foundation. A generated artifact is not valid merely because an agent produced it. It becomes valid only after it satisfies the applicable deterministic checks defined by the construction plan, validation entities, governance policies, and execution model.

This model exists to ensure that:

* tools are invoked through consistent interfaces
* tool results are machine-readable and comparable
* validation outcomes are authoritative for objective checks
* tool failures are not mistaken for successful validation
* tool versions and capabilities are controlled
* tool activity is traceable to tasks, artifacts, and policies
* repair cycles receive structured diagnostic feedback
* governance can restrict, approve, or audit tool usage

---

## 3.0 Normative References

This section identifies the documents required to interpret and implement the Tool Integration Model. Tool integration depends on canonical entities, execution behavior, construction tasks, traceability, and governance controls.

| Reference                   | Purpose                                                                                     |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| ISL v0.0                    | Corpus-level principles, conformance, and system integrity rules                            |
| ISL v1.1                    | Canonical semantic model, Validation entities, Policy entities, and Infrastructure entities |
| ISL v1.2                    | Execution phases, validation outcomes, repair cycles, telemetry, and completion rules       |
| ISL v1.4                    | Traceability requirements for tool invocations and validation results                       |
| ISL v1.5                    | Construction tasks, verification tasks, tool capability requirements, and planning handoff  |
| ISL v1.7                    | Governance policies, tool authorization, waivers, and auditability                          |
| RFC 2119                    | Normative terminology                                                                       |
| OpenTelemetry Specification | Telemetry alignment for tool execution observability                                        |

---

## 4.0 Terms and Definitions

This section defines terms specific to deterministic tool integration. These terms are used by the Planning Engine, Execution Runtime, Tool Integration Layer, Traceability Engine, Governance Engine, and Observability components.

**Deterministic Tool**
A tool that produces repeatable results for equivalent inputs under equivalent configuration, version, and environment conditions.

**Tool Integration**
The adapter, plugin, wrapper, or service through which the IMHOTEP platform invokes a deterministic tool.

**Tool Record**
A registered description of a tool, including identity, version, capabilities, supported artifact types, invocation type, and execution constraints.

**Tool Capability**
A declared operation a tool can perform, such as compile, lint, unit-test, vulnerability-scan, contract-validate, or infrastructure-validate.

**Tool Invocation**
A structured request from the platform to execute a tool capability against one or more artifacts or inputs.

**Tool Invocation Result**
A normalized structured response describing the outcome, findings, duration, confirmed tool version, and evidence produced by a tool invocation.

**Finding**
A structured issue, warning, error, vulnerability, policy violation, or observation produced by a tool.

**Tool Category**
A classification grouping tools by their primary role, such as development, testing, analysis, security, infrastructure, deployment, or documentation.

**Tool Version Drift**
A condition in which the version of the tool that executed differs from the version declared in the Tool Record.

**Tool Trust Profile**
A governance-controlled profile describing the trust level, approval status, restrictions, and allowed use cases for a tool.

---

## 5.0 Tool Integration Design Principles

This section defines the principles governing tool integration. These principles ensure that deterministic tools can be used reliably across local, enterprise, and distributed environments without hardcoding tool-specific behavior into the construction runtime.

### 5.1 Deterministic Validation Supremacy

Deterministic validation MUST govern artifact acceptance for objective checks. An artifact MUST NOT be marked valid based solely on reasoning-agent output, model confidence, or human-readable explanation.

Where deterministic validation is applicable, at least one required deterministic tool check MUST pass before an artifact may be accepted as valid.

### 5.2 Tool Abstraction

The Construction Engine and Execution Runtime MUST interact with tools through a Tool Abstraction Layer. Runtime components MUST NOT contain tool-specific command logic except through registered integrations.

This abstraction allows tools to be replaced, upgraded, configured, sandboxed, or governed without changing construction logic.

### 5.3 Explicit Capability Declaration

A tool MUST declare the capabilities it supports before it may be selected for a task. The platform MUST NOT infer tool capabilities from tool names, file extensions, or informal descriptions.

Capability declarations are required for planning, verification coverage, governance approval, and runtime routing.

### 5.4 Structured Results

Tool outputs MUST be normalized into structured result records before the runtime consumes them. Raw logs MAY be preserved as evidence, but runtime decisions MUST be based on structured outcomes and findings.

### 5.5 Version Awareness

Tool versions MUST be declared, confirmed during invocation, and recorded in results. Version drift MUST be detected and handled according to this document and governance policy.

### 5.6 Secure Execution

Tools MUST execute within controlled environments appropriate to their risk. Tool execution MUST NOT grant unrestricted access to platform state, secrets, network resources, or artifact repositories unless explicitly authorized.

### 5.7 Traceable Tool Activity

Every tool invocation MUST be traceable to the task, artifact, validation criterion, policy, or execution event that caused it.

Tool activity MUST be auditable and reproducible to the extent supported by the tool and execution environment.

---

## 6.0 Tool Integration Architecture

This section defines the architectural role of tool integration within the IMHOTEP platform. The Tool Integration Layer acts as the boundary between autonomous construction and deterministic engineering systems.

The Tool Integration Layer receives structured invocation requests from the runtime, routes them to registered tool integrations, captures raw and structured outputs, normalizes findings, and returns a standardized invocation result.

### 6.1 Tool Integration Components

| Component         | Responsibility                                              |
| ----------------- | ----------------------------------------------------------- |
| Tool Registry     | Stores Tool Records and capability declarations             |
| Tool Selector     | Selects appropriate tools for planned capabilities          |
| Tool Adapter      | Translates platform requests into tool-specific invocations |
| Execution Sandbox | Provides controlled execution environment                   |
| Result Normalizer | Converts tool output into standard result schema            |
| Evidence Store    | Preserves raw logs, reports, and machine-readable outputs   |
| Governance Filter | Applies policy restrictions to tool usage                   |
| Telemetry Emitter | Records tool execution events and metrics                   |

### 6.2 Architectural Rules

All tool invocations MUST pass through the Tool Integration Layer.

The runtime MUST NOT directly invoke unregistered tools.

The Tool Integration Layer MUST enforce governance restrictions before invoking a tool.

The Tool Integration Layer MUST normalize tool output before returning results to the runtime.

The Tool Integration Layer MUST record invocation telemetry and traceability links.

---

## 7.0 Tool Categories and Capabilities

This section defines standard tool categories and capabilities. Categories allow tools to be grouped by purpose, while capabilities describe the specific operations a tool can perform.

Capabilities are used by the Planning Engine when generating verification tasks and by the Execution Runtime when selecting tools.

### 7.1 Standard Tool Categories

| Category       | Purpose                                                                        |
| -------------- | ------------------------------------------------------------------------------ |
| development    | Compilation, packaging, dependency resolution, linting                         |
| testing        | Unit, integration, acceptance, contract, and coverage testing                  |
| analysis       | Static analysis, complexity analysis, duplication detection                    |
| security       | Vulnerability scanning, dependency auditing, secret detection, policy checking |
| infrastructure | Infrastructure validation, configuration checks, environment provisioning      |
| deployment     | Artifact packaging, container build, manifest validation, deployment support   |
| documentation  | Documentation generation or validation                                         |
| governance     | Policy evaluation, waiver checks, approval validation                          |
| observability  | Telemetry validation, metrics schema validation, alert rule checks             |

### 7.2 Standard Capabilities

| Category       | Standard Capabilities                                                              |
| -------------- | ---------------------------------------------------------------------------------- |
| development    | compile, package, dependency-resolve, lint, format-check                           |
| testing        | unit-test, integration-test, acceptance-test, contract-test, coverage-report       |
| analysis       | static-analysis, complexity-analysis, duplication-detection, type-check            |
| security       | vulnerability-scan, dependency-audit, secret-detection, policy-check, license-scan |
| infrastructure | infrastructure-validate, configuration-check, environment-provision-check          |
| deployment     | artifact-package, container-build, manifest-validate, deployment-dry-run           |
| documentation  | documentation-generate, documentation-validate, api-doc-check                      |
| governance     | governance-policy-evaluate, waiver-validate, approval-check                        |
| observability  | telemetry-schema-check, alert-rule-validate, metrics-contract-check                |

### 7.3 Capability Declaration Rules

A tool MAY declare capabilities from multiple categories.

A tool MUST NOT declare a capability unless it can perform that capability for every artifact type and environment profile declared in its Tool Record.

A tool capability MUST include supported artifact types.

A tool capability MAY include constraints such as supported languages, file formats, platforms, or runtime versions.

---

## 8.0 Tool Registration

This section defines how tools become available to the platform. A tool MUST be registered before it can participate in construction planning or execution.

Tool registration provides the platform with the information needed to select tools, enforce governance, route invocations, validate results, and detect version drift.

### 8.1 Tool Record Schema

A registered tool MUST produce a Tool Record.

| Field                    | Type     | Required    | Description                                             |
| ------------------------ | -------- | ----------- | ------------------------------------------------------- |
| tool-id                  | string   | REQUIRED    | Unique identifier for this tool integration             |
| tool-name                | string   | REQUIRED    | Human-readable tool name                                |
| tool-version             | string   | REQUIRED    | Approved tool version or version range                  |
| tool-category            | array    | REQUIRED    | One or more categories from §7.1                        |
| capabilities             | array    | REQUIRED    | Capability declarations                                 |
| invocation-type          | enum     | REQUIRED    | cli, api, container, plugin, service                    |
| execution-environment    | string   | REQUIRED    | Required runtime or sandbox environment                 |
| timeout-seconds          | integer  | REQUIRED    | Default timeout for invocation                          |
| supported-artifact-types | array    | REQUIRED    | Artifact types supported                                |
| supported-platforms      | array    | CONDITIONAL | Platforms or OS profiles supported                      |
| configuration-schema     | string   | CONDITIONAL | Schema or reference for tool configuration              |
| result-schema            | string   | CONDITIONAL | Native or normalized result schema reference            |
| trust-profile-id         | string   | REQUIRED    | Tool trust profile reference                            |
| registered-at            | ISO 8601 | REQUIRED    | Registration timestamp                                  |
| registered-by            | string   | REQUIRED    | Authority, component, or administrator registering tool |
| status                   | enum     | REQUIRED    | active, deprecated, disabled, experimental, revoked     |

### 8.2 Capability Declaration Schema

Each capability declaration MUST include:

| Field                    | Type   | Required    | Description                                    |
| ------------------------ | ------ | ----------- | ---------------------------------------------- |
| capability               | string | REQUIRED    | Capability name                                |
| category                 | string | REQUIRED    | Tool category                                  |
| supported-artifact-types | array  | REQUIRED    | Artifact types capability can process          |
| supported-languages      | array  | CONDITIONAL | Languages supported when applicable            |
| supported-formats        | array  | CONDITIONAL | File formats supported when applicable         |
| required-parameters      | array  | CONDITIONAL | Parameters required for invocation             |
| optional-parameters      | array  | OPTIONAL    | Optional invocation parameters                 |
| output-types             | array  | CONDITIONAL | Reports, findings, logs, or artifacts produced |

### 8.3 Registration Rules

A tool MUST NOT be invoked unless it has an active Tool Record.

A disabled, revoked, or deprecated tool MUST NOT be selected for new execution unless governance explicitly authorizes its use.

A tool with experimental status MAY be used only in environments or risk tiers where governance permits experimental tools.

A Tool Record MUST be version-controlled or auditable.

Changes to Tool Records MUST be recorded as governance-relevant configuration changes when they affect validation, security, deployment, or readiness.

---

## 9.0 Tool Trust Profiles

This section defines trust profiles for tools. Tool trust profiles allow governance to control which tools may be used, where they may run, what they may access, and what outcomes they are authorized to influence.

Tool trust matters because tools may execute code, inspect artifacts, access dependencies, analyze security findings, or produce outputs that influence construction decisions.

### 9.1 Trust Profile Schema

| Field                       | Type    | Required    | Description                                               |
| --------------------------- | ------- | ----------- | --------------------------------------------------------- |
| trust-profile-id            | string  | REQUIRED    | Unique trust profile identifier                           |
| trust-level                 | enum    | REQUIRED    | approved, restricted, experimental, prohibited            |
| allowed-environments        | array   | REQUIRED    | Environments where tool may run                           |
| allowed-risk-tiers          | array   | REQUIRED    | Project risk tiers where tool may be used                 |
| network-access              | enum    | REQUIRED    | none, restricted, approved-endpoints, unrestricted        |
| filesystem-access           | enum    | REQUIRED    | read-only, workspace-only, restricted-write, unrestricted |
| secret-access               | enum    | REQUIRED    | none, scoped, unrestricted                                |
| approval-required           | boolean | REQUIRED    | Whether invocation requires approval                      |
| evidence-retention-required | boolean | REQUIRED    | Whether raw evidence must be retained                     |
| review-required             | boolean | REQUIRED    | Whether human review is required for results              |
| policy-reference            | string  | CONDITIONAL | Governance policy controlling trust profile               |

### 9.2 Trust Level Rules

| Trust Level  | Meaning                                                         |
| ------------ | --------------------------------------------------------------- |
| approved     | Tool is approved for declared use cases                         |
| restricted   | Tool may be used only under defined constraints                 |
| experimental | Tool may be used only for non-production or governed evaluation |
| prohibited   | Tool MUST NOT be used                                           |

### 9.3 Trust Profile Enforcement

The Tool Integration Layer MUST enforce trust profiles before invocation.

A prohibited tool MUST NOT be invoked.

A restricted tool MUST be invoked only within its declared constraints.

If a tool requires approval, the invocation MUST NOT proceed until approval exists.

A tool trust profile change MUST be recorded as a governance event.

---

## 10.0 Tool Selection

This section defines how tools are selected for planned verification, validation, testing, security, infrastructure, and deployment tasks.

Tool selection must be deterministic enough to support repeatable execution and auditable enough to explain why a tool was chosen.

### 10.1 Tool Selection Inputs

Tool selection SHOULD consider:

* required capability
* artifact type
* language or format
* specification risk tier
* governance policy
* tool trust profile
* environment compatibility
* tool version
* timeout and resource constraints
* prior tool performance or reliability telemetry
* configured tool preference order

### 10.2 Tool Selection Record Schema

| Field                  | Type     | Required    | Description                        |
| ---------------------- | -------- | ----------- | ---------------------------------- |
| selection-id           | string   | REQUIRED    | Unique selection record identifier |
| task-id                | string   | REQUIRED    | Task requiring tool selection      |
| required-capability    | string   | REQUIRED    | Capability required                |
| candidate-tools        | array    | REQUIRED    | Tools considered                   |
| selected-tool-id       | string   | REQUIRED    | Tool selected                      |
| selection-rationale    | string   | REQUIRED    | Reason tool was selected           |
| rejected-candidates    | array    | CONDITIONAL | Tools rejected and reasons         |
| governance-constraints | array    | CONDITIONAL | Policies affecting selection       |
| selected-at            | ISO 8601 | REQUIRED    | Time selection occurred            |

### 10.3 Selection Rules

A tool MUST NOT be selected unless it declares the required capability.

A tool MUST NOT be selected if its trust profile prohibits use for the specification risk tier or environment.

When multiple tools satisfy a capability, the Tool Selector MUST apply a deterministic selection policy.

The selected tool MUST be recorded in a Tool Selection Record.

---

## 11.0 Tool Invocation Contract

This section defines the standardized invocation contract. Every tool invocation MUST use this contract or an implementation-equivalent structure preserving all required fields.

The invocation contract ensures that tool execution is bounded, traceable, reproducible, and interpretable by the runtime.

### 11.1 Invocation Request Schema

| Field                       | Type     | Required    | Description                                             |
| --------------------------- | -------- | ----------- | ------------------------------------------------------- |
| invocation-id               | string   | REQUIRED    | Unique invocation identifier                            |
| tool-id                     | string   | REQUIRED    | Registered tool identifier                              |
| capability                  | string   | REQUIRED    | Capability being invoked                                |
| task-id                     | string   | REQUIRED    | Construction or verification task triggering invocation |
| specification-id            | string   | REQUIRED    | System identifier                                       |
| specification-version       | semver   | REQUIRED    | Specification version                                   |
| artifact-ids                | array    | REQUIRED    | Artifacts to process                                    |
| input-locations             | array    | CONDITIONAL | File paths, repository locations, or object references  |
| parameters                  | object   | CONDITIONAL | Tool-specific parameters                                |
| environment-profile         | string   | REQUIRED    | Execution environment profile                           |
| timeout-seconds             | integer  | REQUIRED    | Timeout for this invocation                             |
| timeout-override-reason     | string   | CONDITIONAL | Required when timeout differs from Tool Record          |
| expected-output-types       | array    | CONDITIONAL | Expected reports, findings, logs, or artifacts          |
| invoked-at                  | ISO 8601 | REQUIRED    | Invocation start timestamp                              |
| invoked-by                  | string   | REQUIRED    | Runtime, service, or component invoking tool            |
| governance-authorization-id | string   | CONDITIONAL | Required when invocation needs approval                 |

### 11.2 Invocation Rules

Every invocation MUST reference a registered active Tool Record.

Every invocation MUST reference a task-id.

Every invocation MUST reference one or more artifacts or explicitly justify artifact-free invocation.

Invocation parameters MUST satisfy the tool’s configuration schema when one is declared.

Timeout overrides MUST be recorded with justification.

The invocation MUST be linked to traceability state.

### 11.3 Artifact-Free Invocation

Some tools may operate without direct artifact input, such as environment checks or governance approval checks. In such cases, artifact-ids MAY be empty only if:

* the capability permits artifact-free invocation
* the task-id is present
* the invocation target is represented in parameters or context
* the rationale is recorded

### 11.4 Boundary-Aware Tool Execution

Deterministic tool execution MUST respect construction boundaries and Connection Contexts defined in ISL v1.5.

A tool integration MUST NOT consume artifacts, validation outputs, or execution context originating from another construction boundary unless a valid Connection Context authorizes that interaction.

Tool invocation results MUST remain traceable to the construction boundary and task that initiated the invocation.

---

## 12.0 Tool Invocation Result Contract

This section defines the standardized result contract. Tool outputs vary widely across ecosystems, so the platform must normalize them into a common schema.

Runtime decisions MUST be based on normalized results, not raw logs.

### 12.1 Invocation Result Schema

| Field                  | Type     | Required    | Description                                                 |
| ---------------------- | -------- | ----------- | ----------------------------------------------------------- |
| invocation-result-id   | string   | REQUIRED    | Unique result identifier                                    |
| invocation-id          | string   | REQUIRED    | Matching invocation request identifier                      |
| tool-id                | string   | REQUIRED    | Tool that produced result                                   |
| capability             | string   | REQUIRED    | Capability invoked                                          |
| task-id                | string   | REQUIRED    | Task associated with invocation                             |
| artifact-ids           | array    | REQUIRED    | Artifacts evaluated or produced                             |
| outcome                | enum     | REQUIRED    | passed, failed, warning, timeout, error, not-applicable     |
| findings               | array    | CONDITIONAL | Required when outcome is failed, warning, timeout, or error |
| output-artifacts       | array    | CONDITIONAL | Artifacts produced by tool                                  |
| evidence-locations     | array    | CONDITIONAL | Raw logs, reports, or evidence references                   |
| duration-ms            | integer  | REQUIRED    | Invocation duration                                         |
| completed-at           | ISO 8601 | REQUIRED    | Completion timestamp                                        |
| tool-version-confirmed | string   | REQUIRED    | Actual tool version executed                                |
| environment-confirmed  | string   | CONDITIONAL | Confirmed execution environment                             |
| normalized-by          | string   | REQUIRED    | Component that normalized result                            |
| next-action            | enum     | REQUIRED    | proceed, repair, escalate, retry, halt, record-only         |

### 12.2 Outcome Values

| Outcome        | Meaning                                               |
| -------------- | ----------------------------------------------------- |
| passed         | Tool completed and validation criteria passed         |
| failed         | Tool completed and validation criteria failed         |
| warning        | Tool completed with non-blocking findings             |
| timeout        | Tool failed to complete before timeout                |
| error          | Tool invocation failed due to execution or tool error |
| not-applicable | Tool determined target does not apply to capability   |

### 12.3 Result Rules

Every invocation MUST produce an Invocation Result or a Tool Invocation Error Record.

The tool-version-confirmed field MUST be populated when the tool starts successfully.

A timeout MUST NOT be converted to passed.

An error MUST NOT be converted to passed.

A warning MAY permit progression only when validation criteria and governance policy allow warnings.

Raw outputs SHOULD be preserved when evidence retention is required.

---

## 13.0 Finding Model

This section defines the standard structure for tool findings. Findings provide actionable detail for validation failure, repair analysis, security review, governance evaluation, and audit.

A tool result without structured findings is insufficient when the result is failed, warning, timeout, or error.

### 13.1 Finding Schema

| Field                 | Type   | Required    | Description                                     |
| --------------------- | ------ | ----------- | ----------------------------------------------- |
| finding-id            | string | REQUIRED    | Unique finding identifier                       |
| severity              | enum   | REQUIRED    | critical, high, medium, low, informational      |
| category              | string | REQUIRED    | Finding category                                |
| description           | string | REQUIRED    | Human-readable finding description              |
| artifact-id           | string | CONDITIONAL | Artifact affected                               |
| location              | string | CONDITIONAL | File, line, path, symbol, endpoint, or resource |
| rule-id               | string | CONDITIONAL | Tool rule or policy identifier                  |
| related-policy-id     | string | CONDITIONAL | ISL Policy entity when applicable               |
| related-validation-id | string | CONDITIONAL | ISL Validation entity when applicable           |
| suggested-remediation | string | CONDITIONAL | Remediation guidance when available             |
| raw-reference         | string | CONDITIONAL | Pointer to raw tool output                      |
| confidence            | enum   | CONDITIONAL | high, medium, low, unknown                      |

### 13.2 Severity Rules

Critical and high severity security findings MUST be treated as failed outcomes unless governance policy explicitly allows another response and a valid waiver exists.

Findings with severity informational MUST NOT block progression unless a policy defines them as blocking.

A finding affecting a must-have requirement validation MUST be blocking unless waived.

### 13.3 Finding Traceability

Each finding SHOULD be linked to the affected artifact, specification entity, policy, validation criterion, or task where such mapping is possible.

Security findings MUST map to Policy entities when applicable.

---

## 14.0 Result Normalization

This section defines how raw tool output is transformed into standardized invocation results and findings.

Normalization is required because tools produce different output formats. Without normalization, the runtime cannot make consistent decisions across compilers, test frameworks, scanners, linters, and infrastructure validators.

### 14.1 Normalization Requirements

The Result Normalizer MUST:

* parse tool output
* determine normalized outcome
* extract findings
* map findings to artifacts where possible
* map findings to policies or validations where possible
* preserve raw output references
* record normalization errors
* confirm tool version where possible
* produce Invocation Result schema

### 14.2 Normalization Failure

If tool execution succeeds but output cannot be normalized, the result MUST be treated as error unless governance permits manual interpretation.

A normalization failure MUST produce a Tool Normalization Error Record.

### 14.3 Normalization Error Record Schema

| Field                  | Type     | Required    | Description                           |
| ---------------------- | -------- | ----------- | ------------------------------------- |
| normalization-error-id | string   | REQUIRED    | Unique normalization error identifier |
| invocation-id          | string   | REQUIRED    | Related invocation                    |
| tool-id                | string   | REQUIRED    | Tool producing unnormalized output    |
| error-message          | string   | REQUIRED    | Explanation of failure                |
| raw-output-location    | string   | CONDITIONAL | Evidence location                     |
| severity               | enum     | REQUIRED    | blocking, high, medium, low           |
| required-action        | string   | REQUIRED    | Remediation required                  |
| recorded-at            | ISO 8601 | REQUIRED    | Time error was recorded               |

---

## 15.0 Outcome Handling

This section defines how the runtime handles normalized tool outcomes. Outcome handling connects tool feedback to execution progression, repair cycles, escalation, or halt.

The runtime MUST apply both this model and any stricter governance policy.

### 15.1 Default Outcome Handling

| Outcome        | Default Runtime Action                                                          |
| -------------- | ------------------------------------------------------------------------------- |
| passed         | Mark validation satisfied and proceed                                           |
| failed         | Initiate repair cycle or fail task according to task configuration              |
| warning        | Record finding and proceed unless policy escalates                              |
| timeout        | Retry once; if timeout recurs, treat as failed or escalate                      |
| error          | Retry, substitute tool, escalate, or fail according to policy                   |
| not-applicable | Record justification and proceed only if validation coverage remains sufficient |

### 15.2 Failure Handling Rules

A failed outcome for an artifact validation task MUST prevent the artifact from being marked valid.

A failed outcome MAY trigger repair if repair is permitted and termination limits have not been reached.

A failed outcome MUST trigger escalation when repair is not permitted, repair limits are reached, or governance requires human review.

### 15.3 Warning Handling Rules

A warning outcome MUST be recorded.

A warning MAY permit progression if:

* expected-outcome allows warning-acceptable
* no policy requires escalation
* no finding severity is critical or high unless explicitly permitted
* the finding does not affect must-have validation

### 15.4 Timeout Handling Rules

The runtime MAY retry a timed-out invocation once using the same tool and equivalent input.

If the retry times out, the runtime MUST treat the outcome as failed or escalate according to policy.

Timeouts MUST be recorded in telemetry.

### 15.5 Error Handling Rules

Tool errors MUST be recorded.

A tool error MAY trigger:

* retry
* substitution with another approved tool
* escalation
* task failure
* execution halt

A repeated tool error across two consecutive invocations for the same capability and artifact SHOULD trigger governance or operational alerting.

---

## 16.0 Tool Conflict Handling

This section defines how the platform handles conflicting results from multiple tools. Tool conflicts occur when two or more tools evaluate the same artifact, capability, policy, or validation criterion and produce incompatible outcomes.

Enterprise construction environments often use overlapping tools. Conflict handling must be explicit because silent selection of the most convenient result undermines trust.

### 16.1 Conflict Types

| Conflict Type     | Description                                            |
| ----------------- | ------------------------------------------------------ |
| outcome-conflict  | One tool passes while another fails                    |
| severity-conflict | Tools agree on issue but disagree on severity          |
| policy-conflict   | Tool result conflicts with governance policy           |
| version-conflict  | Results differ due to tool version differences         |
| scope-conflict    | Tools evaluate different target scopes unintentionally |
| finding-conflict  | Tools report incompatible findings for same target     |

### 16.2 Tool Conflict Record Schema

| Field                 | Type     | Required    | Description                                                    |
| --------------------- | -------- | ----------- | -------------------------------------------------------------- |
| conflict-id           | string   | REQUIRED    | Unique conflict identifier                                     |
| task-id               | string   | REQUIRED    | Task associated with conflict                                  |
| artifact-id           | string   | CONDITIONAL | Artifact affected                                              |
| capability            | string   | REQUIRED    | Capability evaluated                                           |
| invocation-result-ids | array    | REQUIRED    | Conflicting result identifiers                                 |
| conflict-type         | enum     | REQUIRED    | Conflict type from §16.1                                       |
| description           | string   | REQUIRED    | Explanation of conflict                                        |
| default-resolution    | enum     | REQUIRED    | fail-closed, precedence-rule, governance-review, manual-review |
| resolved-by           | string   | CONDITIONAL | Tool, rule, role, or authority resolving conflict              |
| resolution            | string   | CONDITIONAL | Resolution applied                                             |
| resolved-at           | ISO 8601 | CONDITIONAL | Time resolved                                                  |

### 16.3 Default Conflict Policy

Unless a governance policy defines a different rule, tool conflicts MUST be handled using fail-closed behavior.

Fail-closed means:

* a passed result MUST NOT override a failed result
* security or policy failures MUST remain blocking
* conflicting results MUST be escalated or resolved before artifact acceptance
* the artifact MUST NOT be marked valid until conflict resolution completes

### 16.4 Tool Precedence

A governance profile MAY define tool precedence rules.

Tool precedence rules MUST specify:

* applicable capability
* applicable artifact type
* tool order of authority
* conditions where precedence applies
* whether human review is required

Precedence MUST NOT allow a lower-trust tool to override a higher-trust blocking result unless governance explicitly permits it.

---

## 17.0 Tool Version Management

This section defines how tool versions are declared, confirmed, and controlled. Tool version management is required because deterministic results may differ across tool versions.

A validation result is not fully interpretable unless the platform knows which tool version produced it.

### 17.1 Version Declaration

Every Tool Record MUST declare an approved tool version or version range.

Tool integrations SHOULD support version discovery.

### 17.2 Version Confirmation

Every Invocation Result MUST include tool-version-confirmed when the tool starts successfully.

If tool version cannot be confirmed, the result MUST include a finding or warning unless governance permits unknown versions.

### 17.3 Version Drift

Version drift occurs when tool-version-confirmed differs from the registered tool-version or approved range.

Version drift MUST be recorded.

Version drift MUST be handled according to governance policy.

By default, version drift in validation, security, infrastructure, or deployment tools MUST produce warning or failed outcome depending on risk tier.

### 17.4 Version Drift Record Schema

| Field              | Type     | Required | Description                         |
| ------------------ | -------- | -------- | ----------------------------------- |
| drift-id           | string   | REQUIRED | Unique drift record identifier      |
| tool-id            | string   | REQUIRED | Tool with drift                     |
| registered-version | string   | REQUIRED | Approved or expected version        |
| confirmed-version  | string   | REQUIRED | Actual executed version             |
| invocation-id      | string   | REQUIRED | Invocation where drift was detected |
| severity           | enum     | REQUIRED | blocking, high, medium, low         |
| governance-action  | enum     | REQUIRED | allow, warn, escalate, block        |
| recorded-at        | ISO 8601 | REQUIRED | Time drift was recorded             |

---

## 18.0 Tool Configuration Management

This section defines how tool configuration is handled. Tool behavior can vary significantly based on configuration, rulesets, plugins, environment variables, and profiles.

A deterministic tool is only repeatable when its version and configuration are known.

### 18.1 Configuration Requirements

A tool invocation MUST record configuration inputs sufficient to reproduce or explain the result.

Configuration may include:

* config file references
* rule set versions
* severity thresholds
* target language settings
* environment variables
* plugin versions
* policy profiles
* execution flags

### 18.2 Configuration Record Schema

| Field                  | Type     | Required    | Description                       |
| ---------------------- | -------- | ----------- | --------------------------------- |
| configuration-id       | string   | REQUIRED    | Unique configuration identifier   |
| tool-id                | string   | REQUIRED    | Tool configured                   |
| configuration-version  | string   | CONDITIONAL | Version of configuration          |
| configuration-location | string   | CONDITIONAL | File or repository location       |
| configuration-hash     | string   | CONDITIONAL | Integrity hash when supported     |
| parameters             | object   | CONDITIONAL | Runtime parameters                |
| ruleset-reference      | string   | CONDITIONAL | Ruleset or profile reference      |
| created-at             | ISO 8601 | REQUIRED    | Time configuration record created |

### 18.3 Configuration Rules

Tool configurations used for validation, security, infrastructure, or deployment MUST be versioned or otherwise identifiable.

Configuration changes affecting validation results SHOULD trigger revalidation of affected artifacts.

Unrecorded configuration changes MUST be treated as reproducibility risks.

---

## 19.0 Execution Environment and Sandboxing

This section defines requirements for controlled tool execution environments. Tools may compile code, execute tests, inspect files, access dependencies, or run infrastructure validation. These activities must occur within security boundaries.

Sandboxing protects the platform from tool failures, malicious dependencies, untrusted generated code, and unintended side effects.

### 19.1 Execution Environment Types

| Environment Type | Description                                         |
| ---------------- | --------------------------------------------------- |
| local-process    | Runs as a local process with controlled permissions |
| container        | Runs in isolated container environment              |
| remote-service   | Runs through approved external or internal service  |
| restricted-vm    | Runs in virtualized isolated environment            |
| plugin-host      | Runs inside controlled plugin host                  |

### 19.2 Sandbox Requirements

Tool execution environments MUST define:

* filesystem access boundaries
* network access policy
* environment variable access
* secret access policy
* resource limits
* timeout controls
* artifact input and output locations
* cleanup behavior after execution

### 19.3 Security Rules

Tools MUST NOT receive unrestricted secrets by default.

Tools MUST NOT write outside approved workspace locations unless explicitly authorized.

Tools executing generated code SHOULD run in isolated environments.

Security tools requiring network access MUST operate under approved endpoint policies.

Sandbox violations MUST be treated as tool errors and governance-relevant security events.

---

## 20.0 Testing Tool Integration

This section defines requirements for tools that execute tests or produce test evidence. Testing tools validate whether generated artifacts satisfy requirements and acceptance criteria.

Testing results must be granular enough to support traceability to Validation entities and requirements.

### 20.1 Testing Tool Requirements

Testing tools MUST report individual test case outcomes.

Aggregate pass/fail results without individual case detail MUST NOT satisfy test execution requirements for must-have requirements.

Testing tools MUST map results to:

* test artifact identifiers
* Validation entity identifiers
* Requirement identifiers where applicable
* artifacts under test
* execution timestamp

### 20.2 Test Result Mapping

Testing tool results MUST be converted into Test Result records defined by ISL v1.2.

A failed test linked to a must-have requirement MUST block progression unless waived.

A skipped test linked to a must-have requirement MUST require justification.

---

## 21.0 Security Tool Integration

This section defines requirements for security tools. Security tools evaluate artifacts and dependencies for vulnerabilities, secrets, policy violations, access control defects, and other security-relevant findings.

Security tool results directly affect governance and policy enforcement.

### 21.1 Required Security Capabilities

Platforms operating beyond advisory planning SHOULD support tools with the following capabilities:

* vulnerability-scan
* dependency-audit
* secret-detection
* policy-check
* license-scan

Regulated, high-risk, or critical systems SHOULD include additional tools appropriate to their governance profile.

### 21.2 Security Finding Rules

Security findings MUST include severity.

Security findings SHOULD map to Policy entities when applicable.

Critical and high severity findings MUST be treated as failed outcomes unless governance explicitly waives or reclassifies the finding.

A security tool result MUST NOT be downgraded without a governance record.

### 21.3 Security Evidence

Security tool evidence MUST be retained when:

* the system risk tier is Elevated, High, or Critical
* governance requires audit evidence
* findings are waived
* findings affect deployment authorization

---

## 22.0 Infrastructure Tool Integration

This section defines requirements for infrastructure validation tools. Infrastructure tools validate deployment definitions, environment configuration, resource declarations, network policies, and runtime compatibility.

Infrastructure validation ensures that generated systems can be deployed in the target environment without violating operational constraints.

### 22.1 Infrastructure Validation Requirements

Infrastructure validation MUST verify, at minimum:

* deployment model compatibility
* runtime platform compatibility
* declared resource requirement satisfiability
* network configuration correctness where applicable
* environment configuration completeness
* manifest syntax validity
* health check configuration where applicable

### 22.2 Infrastructure Finding Rules

Infrastructure findings affecting deployment feasibility MUST block deployment preparation.

Infrastructure findings affecting security or access control MUST be treated according to security policy rules.

Infrastructure warnings MAY proceed only if deployment preparation criteria and governance policy allow them.

---

## 23.0 Development and Build Tool Integration

This section defines requirements for development tools such as compilers, linters, package managers, formatters, and dependency resolvers. These tools validate basic artifact correctness and build viability.

Build tools are often the first deterministic check applied to generated artifacts.

### 23.1 Development Tool Requirements

Development tools SHOULD support applicable capabilities including:

* compile
* lint
* type-check
* format-check
* dependency-resolve
* package

### 23.2 Build Validation Rules

A source artifact MUST NOT be accepted as valid if compilation or equivalent language validation fails.

Dependency resolution failures MUST block packaging unless governance explicitly permits deferred resolution.

Lint or formatting failures MAY be warning-level unless policy defines them as blocking.

---

## 24.0 Deployment Tool Integration

This section defines requirements for deployment-related tools. Deployment tools package artifacts, build containers, validate manifests, perform dry runs, or prepare deployment outputs.

Deployment tool integration supports the final execution stages but does not itself imply production deployment authorization.

### 24.1 Deployment Tool Capabilities

Deployment tools MAY provide:

* artifact-package
* container-build
* manifest-validate
* deployment-dry-run
* release-bundle-generate
* environment-compatibility-check

### 24.2 Deployment Tool Rules

Deployment tool outputs MUST trace to Infrastructure entities or deployment preparation tasks.

Deployment validation failures MUST block deployment preparation readiness.

Deployment tool success MUST NOT override governance deployment authorization requirements.

---

## 25.0 Tool Feedback and Repair Loop Integration

This section defines how tool findings feed repair cycles. Repair analysts and implementation generators require structured diagnostic feedback to correct failed artifacts.

Tool feedback must be preserved in enough detail to support root cause analysis and later audit.

### 25.1 Repair Feedback Requirements

When a tool result triggers repair, the runtime MUST provide the Repair Analyst with:

* failed invocation result
* findings
* affected artifacts
* related task
* related specification entities
* prior repair history for the artifact
* tool version and configuration
* raw evidence references when available

### 25.2 Repair Loop Rules

A repair cycle MUST reference the tool result that triggered it.

A repaired artifact MUST be revalidated by applicable deterministic tools.

A repair MUST NOT close a failed tool result without successful validation or governance waiver.

Repeated tool findings SHOULD be used to detect non-converging repair cycles.

---

## 26.0 Tool Telemetry and Observability

This section defines tool telemetry requirements. Tool telemetry allows operators to monitor reliability, performance, failure patterns, version drift, and validation bottlenecks.

Tool telemetry also supports enterprise audit and continuous platform improvement.

### 26.1 Required Telemetry Events

The platform MUST emit telemetry for:

* tool selection
* invocation start
* invocation completion
* invocation failure
* timeout
* version drift
* normalization failure
* result conflict
* security finding
* repeated failure
* sandbox violation

### 26.2 Minimum Telemetry Fields

Each tool telemetry event SHOULD include:

| Field                  | Description                         |
| ---------------------- | ----------------------------------- |
| event-id               | Unique event identifier             |
| event-type             | Type of telemetry event             |
| timestamp              | Event time                          |
| tool-id                | Tool involved                       |
| tool-version-confirmed | Actual tool version                 |
| capability             | Capability invoked                  |
| task-id                | Related task                        |
| artifact-ids           | Related artifacts                   |
| outcome                | Invocation or event outcome         |
| duration-ms            | Invocation duration when applicable |
| severity               | Event severity when applicable      |

### 26.3 Alert Conditions

The platform SHOULD alert governance or operations when:

| Condition                    | Detection Rule                                            |
| ---------------------------- | --------------------------------------------------------- |
| Persistent tool failure      | Same tool fails same capability three consecutive times   |
| Tool performance degradation | Duration exceeds timeout or baseline threshold repeatedly |
| Tool version drift           | Confirmed version differs from approved version           |
| Sandbox violation            | Tool attempts unauthorized access                         |
| Security finding surge       | Findings exceed configured threshold                      |
| Normalization failure        | Tool output cannot be normalized repeatedly               |

---

## 27.0 Traceability Requirements for Tools

This section defines traceability requirements for tool activity. Tool invocations influence artifact status, repair, governance, and execution completion, so they must be traceable.

### 27.1 Required Traceability Links

The platform MUST record links between:

* construction task and tool invocation
* tool invocation and invocation result
* invocation result and artifacts evaluated
* invocation result and Validation entity where applicable
* invocation result and Policy entity where applicable
* failed result and Repair Record where applicable
* tool conflict and conflicting invocation results
* version drift and invocation result

### 27.2 Traceability Rules

A tool invocation MUST NOT be discarded even if it fails.

A validation result MUST reference the tool invocation that produced it.

A repair triggered by tool output MUST reference the invocation result and findings.

A governance waiver for a tool finding MUST reference the finding and invocation result.

---

## 28.0 Governance of Tool Usage

This section defines governance requirements for tool usage. Tools may introduce risk through external access, execution of generated code, dependency resolution, security scanning, or deployment operations.

Governance must be able to control what tools are used, where, by whom, and for which risk tiers.

### 28.1 Governance Control Points

Governance MAY control:

* tool registration
* tool activation or revocation
* allowed environments
* allowed risk tiers
* network access
* secret access
* tool version approval
* use of experimental tools
* waiver of tool findings
* conflict resolution
* deployment tool authorization

### 28.2 Governance Rules

A tool prohibited by governance MUST NOT be invoked.

A governance-restricted tool MUST satisfy approval and environment constraints before invocation.

A waiver of a tool finding MUST produce a governance waiver record.

A governance policy MAY require human review of certain tool results before progression.

---

## 29.0 Evidence Retention

This section defines retention requirements for tool evidence. Evidence includes raw logs, reports, normalized findings, output artifacts, and configuration records.

Evidence retention is required for auditability, debugging, reproducibility, security review, and governance.

### 29.1 Evidence Types

Tool evidence MAY include:

* raw stdout and stderr logs
* structured result files
* test reports
* coverage reports
* vulnerability reports
* dependency manifests
* infrastructure validation reports
* generated package manifests
* configuration files
* screenshots or external reports when applicable

### 29.2 Evidence Retention Rules

Evidence MUST be retained when:

* the tool result is failed
* the result contains security findings
* the result is waived
* the result affects deployment readiness
* governance policy requires retention
* the system risk tier requires audit evidence

Evidence SHOULD be retained for passed validation results when needed for reproducibility or compliance.

---

## 30.0 Tool Lifecycle Management

This section defines how tools evolve in the platform. Tools may be added, updated, deprecated, disabled, or revoked. These lifecycle changes can affect validation reliability and must be controlled.

### 30.1 Tool Lifecycle States

| State        | Meaning                                                                                |
| ------------ | -------------------------------------------------------------------------------------- |
| proposed     | Tool is submitted for registration                                                     |
| active       | Tool is approved and available                                                         |
| experimental | Tool is available only under restricted conditions                                     |
| deprecated   | Tool remains available for existing workflows but should not be selected for new plans |
| disabled     | Tool is temporarily unavailable                                                        |
| revoked      | Tool must not be used                                                                  |

### 30.2 Lifecycle Rules

A tool MUST be active or explicitly approved for use before invocation.

A revoked tool MUST NOT be invoked.

A deprecated tool SHOULD NOT be selected for new construction plans.

A tool lifecycle state change MUST be recorded.

A tool update affecting validation outcomes SHOULD trigger assessment of affected plans or artifacts.

---

## 31.0 Tool Error Taxonomy

This section defines tool-specific error classes. These errors support consistent handling by the runtime, governance engine, observability layer, and repair process.

### 31.1 Tool Error Record Schema

| Field           | Type     | Required    | Description                 |
| --------------- | -------- | ----------- | --------------------------- |
| error-id        | string   | REQUIRED    | Unique error identifier     |
| error-class     | enum     | REQUIRED    | Error class                 |
| severity        | enum     | REQUIRED    | blocking, high, medium, low |
| tool-id         | string   | CONDITIONAL | Related tool                |
| invocation-id   | string   | CONDITIONAL | Related invocation          |
| task-id         | string   | CONDITIONAL | Related task                |
| artifact-id     | string   | CONDITIONAL | Related artifact            |
| message         | string   | REQUIRED    | Human-readable explanation  |
| required-action | string   | REQUIRED    | Remediation required        |
| recorded-at     | ISO 8601 | REQUIRED    | Time error was recorded     |

### 31.2 Error Classes

| Error Class                      | Description                                     | Default Handling                    |
| -------------------------------- | ----------------------------------------------- | ----------------------------------- |
| tool-not-registered              | Tool has no active Tool Record                  | halt or select alternative          |
| tool-capability-missing          | Tool lacks required capability                  | select alternative or fail planning |
| tool-trust-profile-blocked       | Governance trust profile prohibits use          | block                               |
| tool-configuration-invalid       | Tool configuration invalid or missing           | fail invocation                     |
| tool-invocation-timeout          | Tool invocation exceeded timeout                | retry once                          |
| tool-execution-error             | Tool failed during execution                    | retry, substitute, or escalate      |
| tool-output-normalization-failed | Tool output could not be normalized             | escalate or fail                    |
| tool-version-drift               | Confirmed version differs from approved version | warn, escalate, or block            |
| tool-result-conflict             | Results conflict across tools                   | fail-closed or governance review    |
| tool-sandbox-violation           | Tool violated execution boundary                | halt and escalate                   |
| tool-evidence-missing            | Required evidence was not retained              | warning or block by policy          |
| tool-unsupported-artifact        | Tool cannot process artifact type               | select alternative or fail          |

---

## 32.0 Tool Integration Security Requirements

This section consolidates security requirements for tool integration. Because tools operate on generated artifacts and may execute commands, security requirements must be explicit.

### 32.1 Minimum Security Requirements

A conforming Tool Integration Layer MUST:

* execute tools under least privilege
* restrict filesystem access
* restrict network access unless approved
* restrict secret access
* enforce timeout and resource limits
* isolate generated-code execution where applicable
* record sandbox violations
* validate tool registration before use
* enforce tool trust profiles
* preserve security-relevant evidence

### 32.2 Supply Chain Considerations

Tools that resolve dependencies, download packages, build artifacts, or scan supply-chain metadata MUST operate under governance-approved configuration.

Dependency and package operations SHOULD record:

* dependency sources
* resolved versions
* lockfiles or manifests
* package integrity metadata
* vulnerability scan results
* license findings where applicable

---

## 33.0 Conformance Requirements

This section defines conformance requirements for Tool Records, Tool Integration Layers, Tool Selectors, and platforms.

### 33.1 Tool Record Conformance

A Tool Record conforms to ISL v1.6 if it:

* includes all required registration fields
* declares capabilities explicitly
* declares supported artifact types
* declares invocation type
* declares execution environment
* declares timeout
* references a trust profile
* records registration metadata
* uses valid lifecycle status

### 33.2 Tool Integration Layer Conformance

A Tool Integration Layer conforms to ISL v1.6 if it can:

* maintain a Tool Registry
* enforce tool registration before invocation
* enforce trust profiles
* select tools by declared capability
* issue structured invocation requests
* execute tools in controlled environments
* normalize tool results
* produce structured findings
* handle outcomes according to this model
* detect version drift
* detect tool conflicts
* emit telemetry
* record traceability links
* preserve required evidence

### 33.3 Tool Selector Conformance

A Tool Selector conforms to ISL v1.6 if it can:

* identify candidate tools by capability
* filter candidates by artifact type and environment
* apply governance constraints
* apply deterministic selection rules
* produce Tool Selection Records
* reject prohibited tools
* detect absence of required capabilities

### 33.4 Platform Conformance

A platform conforms to ISL v1.6 if it:

* integrates tool capability requirements with planning
* integrates tool invocation with execution
* integrates tool results with repair cycles
* integrates findings with policy validation
* integrates tool records with governance
* integrates tool telemetry with observability
* prevents artifacts from being marked valid without required deterministic validation
* preserves tool traceability and evidence

---

## 34.0 Summary

ISL v1.6 defines the Tool Integration Model that connects the IMHOTEP autonomous construction lifecycle to deterministic engineering tools. It establishes how tools are registered, selected, invoked, governed, sandboxed, normalized, monitored, and used to validate generated artifacts.

This revision strengthens the model by adding explicit tool records, capability declarations, trust profiles, invocation contracts, result schemas, finding models, conflict handling, version management, sandboxing, testing/security/infrastructure-specific integration rules, evidence retention, lifecycle management, and conformance requirements.

A conforming IMHOTEP platform MUST treat deterministic tool feedback as the objective validation layer for autonomous construction. Reasoning may generate artifacts, but tools establish whether those artifacts can be trusted.
