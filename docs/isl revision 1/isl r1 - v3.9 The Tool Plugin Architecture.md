# IMHOTEP Specification Language (ISL) v3.9

# The Tool Plugin Architecture

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1, ISL v3.5, ISL v3.6, ISL v3.7
**Supersedes:** ISL r0 v3.9 The Tool Plugin Architecture
**Document Type:** Tool Plugin Packaging, Registration, Invocation, Isolation, and Governance Specification

---

## 1.0 Scope

This document defines the Tool Plugin Architecture for the IMHOTEP platform. It specifies how deterministic engineering tools are packaged, discovered, registered, described, selected, invoked, isolated, governed, observed, versioned, tested, and retired.

This document applies to all tool plugins used for compilation, build, dependency resolution, testing, static analysis, security scanning, policy checking, infrastructure validation, packaging, deployment preparation, repository operations, formatting, documentation validation, and other deterministic engineering activities.

This document defines:

* tool plugin principles
* plugin package structure
* plugin manifest schema
* tool registry records
* capability declarations
* discovery and registration
* lifecycle states
* invocation request and result contracts
* structured findings
* evidence capture
* execution isolation
* trust profiles
* governance controls
* traceability and telemetry
* versioning and compatibility
* testing and certification
* error classes
* conformance requirements

---

## 2.0 Purpose

The purpose of the Tool Plugin Architecture is to provide a controlled mechanism for integrating deterministic engineering tools into autonomous construction. Reasoning agents may generate, interpret, review, and repair, but generated artifacts MUST be validated by deterministic tools when deterministic validation is applicable.

Tool plugins form the adapter boundary between IMHOTEP and external tools. A plugin translates platform requests into tool-specific execution, then normalizes tool results into structured platform records. This allows IMHOTEP to use compilers, test runners, scanners, formatters, validators, packagers, and deployment systems without hardcoding every tool into the runtime.

The Tool Plugin Architecture exists to ensure that:

* tools are integrated through standard contracts
* tool capabilities are declared explicitly
* the runtime can select tools by capability
* tool execution is isolated and governed
* tool outputs are normalized before platform use
* validation evidence is preserved
* tool activity is traceable to tasks, artifacts, validations, and repairs
* tool failures have defined handling paths
* plugins can evolve without destabilizing platform architecture

---

## 3.0 Tool Plugin Design Principles

### 3.1 Deterministic Validation Boundary

Tool plugins MUST represent deterministic or externally verifiable operations. A tool may produce warnings, findings, reports, or deployment status, but it MUST return structured evidence that the platform can evaluate.

### 3.2 Plugin as Adapter

A plugin MUST act as an adapter between the platform and an underlying tool. The plugin MAY invoke a CLI, library, service, container, API, or script, but the platform-facing contract MUST remain stable and provider-neutral.

### 3.3 Explicit Capability Declaration

A plugin MUST declare its capabilities before it may be selected by the runtime. The platform MUST NOT infer capability from tool name alone.

### 3.4 Structured Results

A plugin MUST return structured results. Raw logs MAY be preserved as evidence, but raw logs MUST NOT be the only output used for runtime decisions.

### 3.5 Isolation by Default

Tool execution SHOULD be isolated from platform control state, unrelated artifacts, secrets, and other project workspaces. Tools that execute generated code MUST run in sandboxed or otherwise restricted environments.

### 3.6 Governance-Aware Execution

Tool use MUST obey governance policy. Restricted tools, external tool services, deployment-affecting tools, destructive tools, and high-risk security tools MAY require approval before invocation.

### 3.7 Traceable and Observable Activity

Every lifecycle-relevant tool invocation MUST be traceable and observable. Tool results that affect validation, repair, artifact promotion, release readiness, deployment preparation, or governance MUST be linked to the relevant task, artifact, validation record, and execution graph.

---

## 4.0 Tool Plugin Types

Tool plugins are categorized by the operation they expose to IMHOTEP.

| Plugin Type           | Purpose                                                                                        |
| --------------------- | ---------------------------------------------------------------------------------------------- |
| compiler-plugin       | Compiles or builds source artifacts                                                            |
| test-plugin           | Executes automated tests                                                                       |
| analysis-plugin       | Performs static analysis, quality analysis, complexity analysis, or duplication detection      |
| security-plugin       | Performs vulnerability scanning, dependency audit, secret detection, or security policy checks |
| infrastructure-plugin | Validates infrastructure definitions, environment configuration, or provisioning plans         |
| packaging-plugin      | Builds packages, bundles, manifests, containers, or release artifacts                          |
| deployment-plugin     | Performs deployment preparation or controlled deployment actions                               |
| repository-plugin     | Performs repository operations, branch validation, merge checks, or metadata checks            |
| documentation-plugin  | Validates documentation, generated API references, or documentation completeness               |
| format-plugin         | Formats or validates formatting of generated artifacts                                         |
| custom-plugin         | Provides an approved domain-specific deterministic capability                                  |

### 4.1 Plugin Type Rules

A plugin MUST declare one primary plugin type.

A plugin MAY declare secondary capabilities.

A deployment-plugin MUST be governed before production or regulated use.

A security-plugin used for release gates MUST have an active trust profile and validation record.

---

## 5.0 Standard Tool Capability Catalog

The platform SHOULD support the following standard tool capabilities.

| Category       | Capabilities                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------------ |
| development    | compile, build, package, dependency-resolve, lint, format-check                                  |
| testing        | unit-test, integration-test, acceptance-test, coverage-report, regression-test                   |
| analysis       | static-analysis, complexity-analysis, duplication-detection, api-compatibility-check             |
| security       | vulnerability-scan, dependency-audit, secret-detection, license-check, policy-check              |
| infrastructure | infrastructure-validate, manifest-validate, configuration-check, environment-compatibility-check |
| deployment     | artifact-package, container-build, deployment-plan-validate, service-deploy                      |
| repository     | repository-integrity-check, branch-policy-check, merge-conflict-check, metadata-validate         |
| documentation  | documentation-build, documentation-link-check, api-doc-validate                                  |

### 5.1 Capability Rules

A plugin MUST declare each capability it supports.

A capability declaration MUST include input types, output types, supported artifact types, and execution requirements.

A task requiring a tool capability MUST NOT be dispatched unless at least one active, permitted plugin supports the capability.

---

## 6.0 Tool Plugin Package Structure

A tool plugin SHOULD be packaged using a predictable structure.

| Path               | Purpose                                                    |
| ------------------ | ---------------------------------------------------------- |
| `/plugin.manifest` | Plugin manifest                                            |
| `/contracts`       | Plugin-specific request/result contract extensions, if any |
| `/adapter`         | Adapter implementation                                     |
| `/schemas`         | Result, finding, and configuration schemas                 |
| `/config`          | Non-secret configuration templates                         |
| `/tests`           | Plugin tests and fixtures                                  |
| `/docs`            | Plugin documentation                                       |
| `/examples`        | Example invocations and expected results                   |
| `/evidence`        | Certification or validation evidence where applicable      |

### 6.1 Package Rules

A plugin package MUST include a manifest.

A plugin package MUST NOT include plaintext secrets.

A plugin package MUST declare supported platform contract versions.

A plugin package SHOULD include tests and example invocations.

A plugin package used in enterprise or regulated execution SHOULD include validation evidence.

---

## 7.0 Tool Plugin Manifest

Each plugin MUST provide a Tool Plugin Manifest.

### 7.1 Tool Plugin Manifest Schema

| Field                         | Type     | Required    | Description                                                               |
| ----------------------------- | -------- | ----------- | ------------------------------------------------------------------------- |
| tool-plugin-id                | string   | REQUIRED    | Unique plugin identifier                                                  |
| plugin-name                   | string   | REQUIRED    | Human-readable plugin name                                                |
| plugin-version                | semver   | REQUIRED    | Plugin version                                                            |
| plugin-type                   | enum     | REQUIRED    | Plugin type from §4.0                                                     |
| tool-name                     | string   | REQUIRED    | Underlying tool name                                                      |
| tool-version                  | string   | REQUIRED    | Underlying tool version or version range                                  |
| provider                      | string   | CONDITIONAL | Tool provider or vendor                                                   |
| supported-capabilities        | array    | REQUIRED    | Capability declarations                                                   |
| supported-artifact-types      | array    | REQUIRED    | Artifact types supported                                                  |
| supported-input-formats       | array    | REQUIRED    | Input formats supported                                                   |
| supported-output-contracts    | array    | REQUIRED    | Output contracts supported                                                |
| invocation-method             | enum     | REQUIRED    | cli, library, http-api, grpc, container, script, message-bus              |
| execution-environment         | enum     | REQUIRED    | host, process, container, vm, remote-service, sandbox                     |
| isolation-profile-id          | string   | REQUIRED    | Isolation profile                                                         |
| trust-profile-id              | string   | REQUIRED    | Trust profile                                                             |
| required-permissions          | array    | REQUIRED    | Filesystem, network, process, secret, repository, deployment permissions  |
| required-secrets              | array    | CONDITIONAL | Secret references by name only                                            |
| network-access-required       | boolean  | REQUIRED    | Whether network access is required                                        |
| destructive-operation-capable | boolean  | REQUIRED    | Whether plugin can modify external state destructively                    |
| supports-dry-run              | boolean  | REQUIRED    | Whether dry-run is supported                                              |
| supports-timeout              | boolean  | REQUIRED    | Whether timeout can be enforced                                           |
| supports-cancellation         | boolean  | REQUIRED    | Whether cancellation can be enforced                                      |
| supports-evidence-capture     | boolean  | REQUIRED    | Whether evidence capture is supported                                     |
| supported-platform-versions   | array    | REQUIRED    | Supported IMHOTEP platform versions                                       |
| supported-isl-versions        | array    | REQUIRED    | Supported ISL versions                                                    |
| status                        | enum     | REQUIRED    | proposed, active, restricted, experimental, deprecated, disabled, revoked |
| registered-at                 | ISO 8601 | CONDITIONAL | Registration time                                                         |
| registered-by                 | string   | CONDITIONAL | Registering authority or subsystem                                        |

### 7.2 Manifest Rules

A plugin MUST NOT be registered without a valid manifest.

A plugin with status revoked MUST NOT be invoked.

A plugin with status experimental MUST NOT be used for High or Critical risk systems unless governance explicitly permits it.

A plugin that declares destructive-operation-capable true MUST require governance review before use in production environments.

---

## 8.0 Capability Declaration Schema

Each supported capability MUST be declared.

| Field                    | Type   | Required    | Description                                                                                             |
| ------------------------ | ------ | ----------- | ------------------------------------------------------------------------------------------------------- |
| capability-id            | string | REQUIRED    | Unique capability declaration identifier                                                                |
| capability-name          | string | REQUIRED    | Capability name                                                                                         |
| capability-category      | enum   | REQUIRED    | development, testing, analysis, security, infrastructure, deployment, repository, documentation, custom |
| capability-level         | enum   | REQUIRED    | basic, standard, advanced, enterprise, regulated                                                        |
| supported-task-types     | array  | REQUIRED    | Task types supported                                                                                    |
| supported-artifact-types | array  | REQUIRED    | Artifact types supported                                                                                |
| required-inputs          | array  | REQUIRED    | Required invocation inputs                                                                              |
| optional-inputs          | array  | CONDITIONAL | Optional invocation inputs                                                                              |
| output-contract-id       | string | REQUIRED    | Result contract produced                                                                                |
| evidence-types-produced  | array  | CONDITIONAL | Logs, reports, coverage files, SBOM, scan reports, manifests                                            |
| isolation-requirements   | array  | REQUIRED    | Required isolation controls                                                                             |
| governance-requirements  | array  | CONDITIONAL | Required governance checks                                                                              |
| limitations              | array  | CONDITIONAL | Known limitations                                                                                       |
| validation-record-id     | string | CONDITIONAL | Capability validation record                                                                            |

### 8.1 Capability Declaration Rules

A capability MUST identify the output contract it produces.

A capability MUST identify required inputs.

A capability MUST identify supported artifact types.

Security and deployment capabilities SHOULD include governance requirements.

A failed capability validation MUST prevent runtime selection for that capability.

---

## 9.0 Tool Registry

The Tool Registry stores registered plugins and their capabilities.

### 9.1 Tool Registry Record Schema

| Field                   | Type     | Required    | Description                                                     |
| ----------------------- | -------- | ----------- | --------------------------------------------------------------- |
| tool-registry-record-id | string   | REQUIRED    | Unique registry record                                          |
| tool-plugin-id          | string   | REQUIRED    | Plugin identifier                                               |
| plugin-version          | semver   | REQUIRED    | Plugin version                                                  |
| tool-name               | string   | REQUIRED    | Underlying tool name                                            |
| tool-version            | string   | REQUIRED    | Tool version                                                    |
| manifest-reference      | string   | REQUIRED    | Manifest location or record                                     |
| capabilities            | array    | REQUIRED    | Capability declarations                                         |
| trust-profile-id        | string   | REQUIRED    | Trust profile                                                   |
| isolation-profile-id    | string   | REQUIRED    | Isolation profile                                               |
| registration-status     | enum     | REQUIRED    | active, restricted, experimental, deprecated, disabled, revoked |
| health-status           | enum     | REQUIRED    | healthy, degraded, unavailable, unknown                         |
| registered-at           | ISO 8601 | REQUIRED    | Registration time                                               |
| registered-by           | string   | REQUIRED    | Registering component or authority                              |
| last-validated-at       | ISO 8601 | CONDITIONAL | Last validation time                                            |

### 9.2 Registry Rules

The runtime MUST select tools from the Tool Registry.

A disabled or revoked plugin MUST NOT be selected.

A degraded plugin MAY be selected only when runtime policy permits it.

Registry updates during active execution MUST trigger impact evaluation when they affect active or planned validations.

---

## 10.0 Discovery and Registration

Tool discovery identifies available plugins. Registration makes them available for runtime selection.

### 10.1 Discovery Sources

The platform MAY discover plugins from:

* configured plugin directories
* service registries
* package registries
* repository manifests
* environment profiles
* enterprise tool catalogs
* deployment manifests
* administrator registration

### 10.2 Registration Process

Registration MUST:

1. locate plugin package
2. read plugin manifest
3. validate manifest schema
4. verify supported platform and ISL versions
5. validate capability declarations
6. validate required isolation profile
7. validate trust profile
8. verify required permissions and secrets references
9. run health check where available
10. create Tool Registry Record
11. emit registration telemetry

### 10.3 Registration Rules

A plugin MUST NOT be active until registration validation passes.

Registration failure MUST produce a structured plugin error.

A plugin requiring unavailable isolation controls MUST be rejected or restricted.

A plugin requiring secrets MUST reference secret identifiers, not secret values.

---

## 11.0 Tool Plugin Lifecycle

Tool plugins MUST have explicit lifecycle states.

| State              | Meaning                                           |
| ------------------ | ------------------------------------------------- |
| discovered         | Plugin package found                              |
| manifest-validated | Manifest passed structural validation             |
| registered         | Registry record created                           |
| health-checked     | Health check completed                            |
| active             | Plugin available for runtime selection            |
| restricted         | Plugin available only under constraints           |
| experimental       | Plugin available only for governed evaluation     |
| deprecated         | Plugin should not be selected for new work        |
| disabled           | Plugin temporarily unavailable                    |
| revoked            | Plugin prohibited from use                        |
| retired            | Plugin no longer active but retained historically |

### 11.1 Lifecycle Rules

A plugin MUST NOT be invoked unless active or restricted with satisfied conditions.

A deprecated plugin SHOULD NOT be selected for new tasks.

A revoked plugin MUST NOT be invoked.

A lifecycle change MUST be recorded.

A lifecycle change affecting active execution MUST trigger impact evaluation.

---

## 12.0 Tool Selection

Tool selection maps runtime task requirements to registered plugin capabilities.

### 12.1 Tool Selection Inputs

The selector MUST consider:

* required capability
* task type
* artifact type
* input format
* output contract
* tool version constraints
* plugin status
* trust profile
* isolation profile
* risk tier
* project or tenant constraints
* governance policy
* environment compatibility
* historical reliability where available
* resource availability

### 12.2 Tool Selection Record Schema

| Field                     | Type     | Required    | Description                          |
| ------------------------- | -------- | ----------- | ------------------------------------ |
| tool-selection-id         | string   | REQUIRED    | Unique selection decision            |
| task-id                   | string   | REQUIRED    | Task requiring tool                  |
| execution-graph-id        | string   | CONDITIONAL | Execution graph                      |
| required-capability       | string   | REQUIRED    | Capability required                  |
| candidate-tool-plugin-ids | array    | REQUIRED    | Plugins considered                   |
| rejected-candidates       | array    | CONDITIONAL | Rejected plugins and reasons         |
| selected-tool-plugin-id   | string   | REQUIRED    | Selected plugin                      |
| selected-plugin-version   | semver   | REQUIRED    | Selected plugin version              |
| selected-tool-version     | string   | REQUIRED    | Selected underlying tool version     |
| selection-rationale       | string   | REQUIRED    | Reason selected                      |
| governance-check-id       | string   | CONDITIONAL | Governance check affecting selection |
| selected-at               | ISO 8601 | REQUIRED    | Selection time                       |
| selected-by               | string   | REQUIRED    | Selector component                   |

### 12.3 Selection Rules

The selector MUST NOT select a plugin that lacks the required capability.

The selector MUST NOT select a plugin whose trust profile disallows the risk tier or context.

The selector MUST NOT select a plugin whose isolation requirements cannot be satisfied.

When multiple plugins satisfy requirements, the selector SHOULD prefer active, validated, least-privilege, environment-compatible tools.

---

## 13.0 Tool Invocation Request

The Tool Invocation Request is the standard runtime request sent to a plugin.

### 13.1 Tool Invocation Request Schema

| Field                  | Type     | Required    | Description                                        |
| ---------------------- | -------- | ----------- | -------------------------------------------------- |
| tool-invocation-id     | string   | REQUIRED    | Unique invocation identifier                       |
| tool-selection-id      | string   | REQUIRED    | Tool selection decision                            |
| tool-plugin-id         | string   | REQUIRED    | Plugin invoked                                     |
| plugin-version         | semver   | REQUIRED    | Plugin version                                     |
| tool-name              | string   | REQUIRED    | Underlying tool name                               |
| tool-version           | string   | REQUIRED    | Underlying tool version                            |
| capability-name        | string   | REQUIRED    | Capability being invoked                           |
| task-id                | string   | REQUIRED    | Runtime task                                       |
| work-item-id           | string   | CONDITIONAL | Runtime work item                                  |
| execution-graph-id     | string   | CONDITIONAL | Execution graph                                    |
| specification-id       | string   | REQUIRED    | Specification identifier                           |
| specification-version  | semver   | REQUIRED    | Specification version                              |
| artifact-ids           | array    | CONDITIONAL | Artifacts evaluated or modified                    |
| artifact-version-ids   | array    | CONDITIONAL | Exact artifact versions                            |
| input-references       | array    | REQUIRED    | Files, directories, records, or payload references |
| parameters             | object   | CONDITIONAL | Tool parameters                                    |
| environment-profile-id | string   | REQUIRED    | Execution environment profile                      |
| isolation-profile-id   | string   | REQUIRED    | Isolation controls applied                         |
| timeout-seconds        | integer  | REQUIRED    | Invocation timeout                                 |
| dry-run                | boolean  | REQUIRED    | Whether dry-run is requested                       |
| governance-check-id    | string   | CONDITIONAL | Governance authorization where required            |
| correlation-id         | string   | REQUIRED    | Correlation identifier                             |
| requested-at           | ISO 8601 | REQUIRED    | Request time                                       |
| requested-by           | string   | REQUIRED    | Runtime or service requesting invocation           |

### 13.2 Invocation Request Rules

A tool invocation MUST reference a selected tool plugin.

A tool invocation MUST declare capability-name.

A tool invocation MUST declare exact artifact versions when validating artifacts.

A tool invocation MUST declare timeout-seconds.

A tool invocation MUST declare isolation-profile-id.

A governed tool invocation MUST NOT proceed until governance permits it.

---

## 14.0 Tool Invocation Result

The Tool Invocation Result is the normalized result returned by the plugin.

### 14.1 Tool Invocation Result Schema

| Field                          | Type     | Required    | Description                                                        |
| ------------------------------ | -------- | ----------- | ------------------------------------------------------------------ |
| tool-invocation-result-id      | string   | REQUIRED    | Unique result identifier                                           |
| tool-invocation-id             | string   | REQUIRED    | Invocation request                                                 |
| tool-plugin-id                 | string   | REQUIRED    | Plugin invoked                                                     |
| tool-version                   | string   | REQUIRED    | Tool version used                                                  |
| capability-name                | string   | REQUIRED    | Capability invoked                                                 |
| outcome                        | enum     | REQUIRED    | passed, failed, warning, timeout, error, cancelled, not-applicable |
| exit-code                      | integer  | CONDITIONAL | Tool process exit code where applicable                            |
| findings                       | array    | CONDITIONAL | Structured findings                                                |
| evidence-references            | array    | CONDITIONAL | Logs, reports, output files, coverage, scan reports                |
| affected-artifact-ids          | array    | CONDITIONAL | Artifacts affected                                                 |
| evaluated-artifact-version-ids | array    | CONDITIONAL | Artifact versions evaluated                                        |
| normalized-output-reference    | string   | CONDITIONAL | Normalized result payload                                          |
| raw-output-reference           | string   | CONDITIONAL | Raw output if retained                                             |
| duration-ms                    | integer  | REQUIRED    | Execution duration                                                 |
| resource-usage                 | object   | CONDITIONAL | CPU, memory, disk, network usage where available                   |
| started-at                     | ISO 8601 | REQUIRED    | Start time                                                         |
| completed-at                   | ISO 8601 | REQUIRED    | Completion time                                                    |
| next-action                    | enum     | REQUIRED    | proceed, repair, retry, escalate, fail-task, halt, ignore-warning  |

### 14.2 Result Rules

A timeout MUST NOT be treated as passed.

An error MUST NOT be treated as passed.

A result with outcome failed MUST trigger repair, retry, escalation, failure, or waiver handling according to policy.

A result MUST reference evaluated artifact versions when used for artifact validation.

Raw output retention MUST follow governance and sensitivity rules.

---

## 15.0 Tool Findings

Tool findings provide structured details from tool execution.

### 15.1 Tool Finding Schema

| Field                        | Type    | Required    | Description                                                                                                                                |
| ---------------------------- | ------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| tool-finding-id              | string  | REQUIRED    | Unique finding identifier                                                                                                                  |
| tool-invocation-result-id    | string  | REQUIRED    | Tool result containing finding                                                                                                             |
| finding-category             | enum    | REQUIRED    | compile, test, lint, format, static-analysis, security, dependency, license, infrastructure, deployment, repository, documentation, policy |
| severity                     | enum    | REQUIRED    | critical, high, medium, low, informational                                                                                                 |
| finding-code                 | string  | CONDITIONAL | Tool-specific or normalized code                                                                                                           |
| message                      | string  | REQUIRED    | Finding message                                                                                                                            |
| affected-artifact-id         | string  | CONDITIONAL | Artifact affected                                                                                                                          |
| affected-artifact-version-id | string  | CONDITIONAL | Artifact version affected                                                                                                                  |
| location-reference           | string  | CONDITIONAL | File path, line, symbol, config path, resource id                                                                                          |
| rule-reference               | string  | CONDITIONAL | Rule, policy, test, or check reference                                                                                                     |
| evidence-reference           | string  | CONDITIONAL | Supporting evidence                                                                                                                        |
| recommended-action           | enum    | REQUIRED    | proceed, repair, reconfigure, waive, escalate, fail                                                                                        |
| blocks-progression           | boolean | REQUIRED    | Whether finding blocks lifecycle progression                                                                                               |

### 15.2 Finding Rules

A blocking finding MUST prevent affected artifact promotion unless waived.

A critical security finding MUST escalate according to governance policy.

A finding used to trigger repair MUST link to the repair record.

Tool-specific finding codes SHOULD be mapped to normalized finding categories.

---

## 16.0 Evidence Capture

Tool plugins MUST preserve evidence when results affect lifecycle decisions.

### 16.1 Evidence Types

Tool evidence MAY include:

* raw stdout/stderr logs
* build reports
* test result files
* coverage reports
* static analysis reports
* vulnerability scan reports
* dependency manifests
* SBOM files
* package manifests
* deployment plan outputs
* infrastructure validation reports
* screenshots or generated diagnostic files where applicable

### 16.2 Evidence Record Schema

| Field                      | Type     | Required    | Description                                                                       |
| -------------------------- | -------- | ----------- | --------------------------------------------------------------------------------- |
| tool-evidence-id           | string   | REQUIRED    | Unique evidence identifier                                                        |
| tool-invocation-id         | string   | REQUIRED    | Invocation producing evidence                                                     |
| evidence-type              | enum     | REQUIRED    | log, report, coverage, scan, sbom, manifest, package, deployment-plan, diagnostic |
| evidence-reference         | string   | REQUIRED    | Evidence storage reference                                                        |
| evidence-format            | string   | REQUIRED    | Format of evidence                                                                |
| sensitivity-classification | enum     | REQUIRED    | public, internal, confidential, restricted                                        |
| retention-class            | enum     | REQUIRED    | transient, operational, audit-support, archival                                   |
| hash                       | string   | CONDITIONAL | Integrity hash where available                                                    |
| captured-at                | ISO 8601 | REQUIRED    | Capture time                                                                      |

### 16.3 Evidence Rules

Evidence required for validation, release, deployment authorization, or governance MUST be retained.

Evidence MUST be linked to tool invocation result.

Evidence containing sensitive data MUST be classified and protected.

Evidence used for High or Critical systems SHOULD include integrity markers.

---

## 17.0 Execution Isolation

Tool plugins MUST run under declared isolation controls.

### 17.1 Isolation Profiles

| Isolation Profile        | Description                                                          |
| ------------------------ | -------------------------------------------------------------------- |
| host-limited             | Runs on host with restricted workspace and permissions               |
| process-isolated         | Runs in restricted process environment                               |
| container-isolated       | Runs in container with controlled filesystem, network, and resources |
| vm-isolated              | Runs in virtual machine or equivalent strong isolation               |
| remote-service           | Runs through external service boundary                               |
| sandboxed-generated-code | Runs generated code under strict sandbox controls                    |

### 17.2 Isolation Profile Schema

| Field                | Type    | Required    | Description                                                                                               |
| -------------------- | ------- | ----------- | --------------------------------------------------------------------------------------------------------- |
| isolation-profile-id | string  | REQUIRED    | Unique isolation profile                                                                                  |
| isolation-type       | enum    | REQUIRED    | host-limited, process-isolated, container-isolated, vm-isolated, remote-service, sandboxed-generated-code |
| filesystem-scope     | enum    | REQUIRED    | none, read-only-artifacts, write-workspace, repository-scoped                                             |
| network-access       | enum    | REQUIRED    | none, internal-only, restricted-egress, unrestricted                                                      |
| process-execution    | enum    | REQUIRED    | none, restricted, unrestricted                                                                            |
| secret-access        | enum    | REQUIRED    | none, scoped, unrestricted                                                                                |
| resource-limits      | object  | CONDITIONAL | CPU, memory, disk, time, network limits                                                                   |
| cleanup-required     | boolean | REQUIRED    | Whether workspace cleanup is required                                                                     |
| status               | enum    | REQUIRED    | active, deprecated, disabled                                                                              |

### 17.3 Isolation Rules

Tools that execute generated code MUST use sandboxed-generated-code or equivalent strong isolation.

A plugin MUST NOT receive broader filesystem, network, process, or secret access than its manifest declares.

A plugin requiring unavailable isolation controls MUST NOT be invoked.

Transient workspaces SHOULD be cleaned after execution unless evidence retention requires preservation.

---

## 18.0 Trust Profiles

Trust profiles define where and how a plugin may be used.

### 18.1 Tool Trust Profile Schema

| Field                  | Type    | Required    | Description                                        |
| ---------------------- | ------- | ----------- | -------------------------------------------------- |
| trust-profile-id       | string  | REQUIRED    | Unique trust profile                               |
| trust-level            | enum    | REQUIRED    | approved, restricted, experimental, prohibited     |
| allowed-risk-tiers     | array   | REQUIRED    | Risk tiers where tool may be used                  |
| allowed-environments   | array   | REQUIRED    | Environments where tool may run                    |
| allowed-project-scopes | array   | CONDITIONAL | Project or tenant scopes                           |
| external-service       | boolean | REQUIRED    | Whether tool executes outside platform environment |
| data-retention-profile | enum    | REQUIRED    | none, transient, retained, unknown                 |
| human-review-required  | boolean | REQUIRED    | Whether output requires human review               |
| approval-required      | boolean | REQUIRED    | Whether invocation requires approval               |
| policy-reference       | string  | CONDITIONAL | Governance policy controlling profile              |
| status                 | enum    | REQUIRED    | active, superseded, revoked                        |

### 18.2 Trust Rules

A prohibited tool MUST NOT be invoked.

A restricted tool MUST be used only under its declared constraints.

A tool with unknown data-retention-profile MUST NOT receive confidential or restricted artifacts unless governance authorizes it.

Trust profile changes MUST be audited.

---

## 19.0 Governance of Tool Usage

Tool usage may be controlled by governance policies.

### 19.1 Governed Tool Actions

Governance MAY control:

* plugin registration
* plugin activation
* restricted tool invocation
* external tool service use
* destructive operations
* deployment tool invocation
* security scan waiver
* use of experimental plugins
* use of tools on High or Critical systems
* acceptance of failed or warning results
* evidence retention exceptions

### 19.2 Governance Rules

A governance block MUST prevent tool invocation.

An approval-required decision MUST pause tool invocation until approval exists.

A waiver MUST be explicit when accepting a failed or blocking finding.

A tool used for deployment-affecting action MUST record deployment or environment scope.

Governance audit write failure MUST halt governed tool action.

---

## 20.0 Traceability of Tool Activity

Tool activity MUST be traceable.

### 20.1 Required Traceability Links

The platform MUST link:

* task to tool selection
* tool selection to tool invocation
* tool invocation to plugin manifest
* tool invocation to artifacts evaluated
* tool invocation result to validation record
* tool finding to artifact version
* tool finding to repair record where applicable
* tool evidence to validation result
* tool invocation to governance record where applicable
* tool invocation to execution graph

### 20.2 Traceability Rules

A validation task MUST NOT be completed if required tool traceability is missing.

An artifact MUST NOT be promoted based on a tool result that cannot be traced to the evaluated artifact version.

A repair triggered by tool finding MUST preserve the finding-to-repair link.

---

## 21.0 Tool Telemetry

Tool plugins MUST emit or enable telemetry.

### 21.1 Required Telemetry Events

The platform SHOULD emit:

* tool-plugin-discovered
* tool-plugin-registered
* tool-plugin-health-checked
* tool-selected
* tool-invocation-started
* tool-invocation-completed
* tool-invocation-failed
* tool-invocation-timeout
* tool-finding-recorded
* tool-evidence-captured
* tool-governance-blocked
* tool-plugin-disabled
* tool-plugin-revoked

### 21.2 Tool Metrics

The platform SHOULD collect:

* invocation count by plugin
* invocation duration by capability
* pass/fail/warning/error rate
* timeout rate
* finding count by severity
* evidence capture failure rate
* resource usage by plugin
* plugin health status
* tool version distribution

### 21.3 Telemetry Rules

Telemetry MUST include tool-plugin-id and tool-invocation-id when applicable.

Telemetry MUST include correlation-id.

Telemetry MUST NOT expose restricted artifact content or secrets.

---

## 22.0 Plugin Health Checks

Plugins SHOULD expose health checks.

### 22.1 Health Check Schema

| Field                | Type     | Required    | Description                                   |
| -------------------- | -------- | ----------- | --------------------------------------------- |
| tool-health-check-id | string   | REQUIRED    | Unique health check                           |
| tool-plugin-id       | string   | REQUIRED    | Plugin checked                                |
| plugin-version       | semver   | REQUIRED    | Plugin version                                |
| tool-version         | string   | REQUIRED    | Tool version detected                         |
| status               | enum     | REQUIRED    | healthy, degraded, unavailable, misconfigured |
| capability-checks    | array    | CONDITIONAL | Capability health results                     |
| environment-checks   | array    | CONDITIONAL | Environment compatibility checks              |
| permission-checks    | array    | CONDITIONAL | Permission checks                             |
| last-error           | string   | CONDITIONAL | Last error                                    |
| checked-at           | ISO 8601 | REQUIRED    | Check timestamp                               |

### 22.2 Health Rules

An unavailable plugin MUST NOT be selected.

A misconfigured plugin MUST be disabled or restricted until corrected.

A degraded plugin MAY be selected only when runtime policy permits.

Health check failures SHOULD emit telemetry.

---

## 23.0 Versioning and Compatibility

Tool plugins and underlying tools MUST be versioned.

### 23.1 Compatibility Record Schema

| Field                        | Type     | Required    | Description                                   |
| ---------------------------- | -------- | ----------- | --------------------------------------------- |
| tool-compatibility-record-id | string   | REQUIRED    | Unique compatibility record                   |
| tool-plugin-id               | string   | REQUIRED    | Plugin identifier                             |
| plugin-version               | semver   | REQUIRED    | Plugin version                                |
| tool-name                    | string   | REQUIRED    | Underlying tool                               |
| tool-version                 | string   | REQUIRED    | Tool version                                  |
| supported-platform-versions  | array    | REQUIRED    | Platform versions supported                   |
| supported-isl-versions       | array    | REQUIRED    | ISL versions supported                        |
| supported-contract-versions  | array    | REQUIRED    | Invocation/result contract versions supported |
| compatibility-status         | enum     | REQUIRED    | compatible, warning, incompatible             |
| findings                     | array    | CONDITIONAL | Compatibility findings                        |
| evaluated-at                 | ISO 8601 | REQUIRED    | Evaluation time                               |

### 23.2 Versioning Rules

Plugin versions SHOULD use semantic versioning.

A breaking change to invocation or result contract behavior MUST increment the major version.

A tool version change affecting validation results SHOULD trigger capability revalidation.

The tool version used for each invocation MUST be recorded.

---

## 24.0 Tool Plugin Configuration

Tool plugin configuration MUST be explicit and versioned.

### 24.1 Plugin Configuration Schema

| Field                        | Type     | Required    | Description                             |
| ---------------------------- | -------- | ----------- | --------------------------------------- |
| tool-plugin-configuration-id | string   | REQUIRED    | Unique configuration identifier         |
| tool-plugin-id               | string   | REQUIRED    | Plugin configured                       |
| configuration-version        | semver   | REQUIRED    | Configuration version                   |
| environment                  | string   | REQUIRED    | Environment where configuration applies |
| parameters                   | object   | CONDITIONAL | Non-secret parameters                   |
| secret-references            | array    | CONDITIONAL | Secret identifiers only                 |
| isolation-profile-id         | string   | REQUIRED    | Isolation profile                       |
| trust-profile-id             | string   | REQUIRED    | Trust profile                           |
| governance-profile-id        | string   | CONDITIONAL | Governance profile                      |
| status                       | enum     | REQUIRED    | draft, active, superseded, revoked      |
| configured-at                | ISO 8601 | REQUIRED    | Configuration time                      |
| configured-by                | string   | REQUIRED    | Configuring authority or subsystem      |

### 24.2 Configuration Rules

Configuration MUST NOT contain plaintext secrets.

Active configuration MUST be recorded for each invocation.

Configuration changes affecting behavior MUST trigger compatibility or capability revalidation when applicable.

A revoked configuration MUST NOT be used.

---

## 25.0 Plugin Testing and Certification

Plugins MUST be testable before production use.

### 25.1 Required Test Categories

| Test Category         | Purpose                                                      |
| --------------------- | ------------------------------------------------------------ |
| manifest-validation   | Validate plugin manifest schema                              |
| capability-validation | Verify declared capabilities                                 |
| invocation-contract   | Verify request handling                                      |
| result-contract       | Verify normalized result structure                           |
| finding-normalization | Verify finding mapping                                       |
| evidence-capture      | Verify required evidence is retained                         |
| timeout-handling      | Verify timeout behavior                                      |
| cancellation-handling | Verify cancellation behavior                                 |
| isolation             | Verify filesystem, network, process, and secret restrictions |
| security              | Verify no secret leakage and safe handling                   |
| governance            | Verify governed actions are blocked or paused                |
| compatibility         | Verify supported platform and contract versions              |
| failure-injection     | Verify error and recovery behavior                           |

### 25.2 Certification Record Schema

| Field                        | Type     | Required    | Description                            |
| ---------------------------- | -------- | ----------- | -------------------------------------- |
| tool-plugin-certification-id | string   | REQUIRED    | Unique certification record            |
| tool-plugin-id               | string   | REQUIRED    | Plugin certified                       |
| plugin-version               | semver   | REQUIRED    | Plugin version                         |
| tool-version                 | string   | REQUIRED    | Tool version                           |
| certification-level          | enum     | REQUIRED    | local, standard, enterprise, regulated |
| tests-performed              | array    | REQUIRED    | Tests performed                        |
| certification-outcome        | enum     | REQUIRED    | passed, failed, warning, expired       |
| limitations                  | array    | CONDITIONAL | Known limitations                      |
| certified-at                 | ISO 8601 | REQUIRED    | Certification time                     |
| expires-at                   | ISO 8601 | CONDITIONAL | Expiration time                        |
| certified-by                 | string   | REQUIRED    | Certifying role, tool, or authority    |

### 25.3 Testing Rules

A plugin used for enterprise release gates SHOULD have enterprise certification.

A plugin used for regulated systems SHOULD have regulated certification.

A failed certification MUST prevent activation for the certified scope.

Expired certification MUST trigger revalidation or governance review.

---

## 26.0 Plugin Extension Model

Organizations MAY develop custom plugins.

### 26.1 Extension Rules

A custom plugin MUST provide a manifest.

A custom plugin MUST declare capabilities and output contracts.

A custom plugin MUST implement standard invocation and result contracts.

A custom plugin MUST define isolation and trust profiles.

A custom plugin MUST include tests or certification evidence before active use.

A custom plugin MUST NOT bypass governance, traceability, artifact, state, or telemetry controls.

---

## 27.0 Tool Plugin Error Classes

This section defines tool-plugin-specific errors.

| Error Class                       | Description                                      | Default Handling         |
| --------------------------------- | ------------------------------------------------ | ------------------------ |
| tool-plugin-manifest-missing      | Plugin package has no manifest                   | reject registration      |
| tool-plugin-manifest-invalid      | Manifest fails schema validation                 | reject registration      |
| tool-plugin-version-incompatible  | Plugin incompatible with platform or ISL version | reject or disable        |
| tool-capability-missing           | Required capability not declared                 | reject selection         |
| tool-capability-validation-failed | Capability validation failed                     | disable capability       |
| tool-plugin-health-failed         | Plugin health check failed                       | disable or restrict      |
| tool-configuration-invalid        | Plugin configuration invalid                     | block invocation         |
| tool-required-secret-missing      | Required secret reference unavailable            | block invocation         |
| tool-isolation-unavailable        | Required isolation controls unavailable          | block invocation         |
| tool-governance-blocked           | Governance blocks tool action                    | block                    |
| tool-invocation-timeout           | Tool execution exceeded timeout                  | retry or fail            |
| tool-invocation-cancelled         | Tool execution cancelled                         | retry or fail            |
| tool-process-error                | Tool process or service returned error           | retry or fail            |
| tool-output-malformed             | Tool output cannot be normalized                 | retry, escalate, or fail |
| tool-result-contract-failed       | Tool result violates result contract             | escalate or fail         |
| tool-evidence-capture-failed      | Required evidence could not be captured          | escalate                 |
| tool-finding-normalization-failed | Findings could not be normalized                 | escalate                 |
| tool-security-boundary-violation  | Tool violated isolation or security boundary     | halt and escalate        |
| tool-destructive-action-blocked   | Destructive action not authorized                | block                    |
| tool-traceability-missing         | Required tool traceability missing               | block downstream use     |
| tool-certification-expired        | Required certification expired                   | restrict or revalidate   |

### 27.1 Tool Plugin Error Record Schema

| Field                | Type     | Required    | Description                                                   |
| -------------------- | -------- | ----------- | ------------------------------------------------------------- |
| tool-plugin-error-id | string   | REQUIRED    | Unique error identifier                                       |
| error-class          | enum     | REQUIRED    | Error class                                                   |
| severity             | enum     | REQUIRED    | blocking, high, medium, low                                   |
| tool-plugin-id       | string   | CONDITIONAL | Plugin affected                                               |
| tool-invocation-id   | string   | CONDITIONAL | Invocation affected                                           |
| task-id              | string   | CONDITIONAL | Task affected                                                 |
| artifact-id          | string   | CONDITIONAL | Artifact affected                                             |
| message              | string   | REQUIRED    | Human-readable explanation                                    |
| default-handling     | enum     | REQUIRED    | reject, block, retry, fail, restrict, disable, halt, escalate |
| required-action      | string   | REQUIRED    | Required remediation                                          |
| detected-at          | ISO 8601 | REQUIRED    | Detection time                                                |

---

## 28.0 Plugin Retirement

Plugins may be retired or superseded.

### 28.1 Retirement Rules

A retired plugin MUST NOT be selected for new invocations.

Historical invocation records MUST remain queryable.

Retirement MUST preserve manifest, compatibility, certification, and invocation records.

A retired plugin MAY remain available only for historical replay or evidence review when governance permits it.

### 28.2 Retirement Record Schema

| Field                      | Type     | Required    | Description                         |
| -------------------------- | -------- | ----------- | ----------------------------------- |
| tool-plugin-retirement-id  | string   | REQUIRED    | Unique retirement record            |
| tool-plugin-id             | string   | REQUIRED    | Plugin retired                      |
| plugin-version             | semver   | REQUIRED    | Plugin version                      |
| retirement-reason          | string   | REQUIRED    | Reason for retirement               |
| replacement-tool-plugin-id | string   | CONDITIONAL | Replacement plugin where applicable |
| effective-at               | ISO 8601 | REQUIRED    | Retirement effective time           |
| approved-by                | string   | CONDITIONAL | Approver where required             |

---

## 29.0 Documentation Requirements

Each plugin SHOULD include documentation.

### 29.1 Required Documentation

| Document               | Purpose                                                           |
| ---------------------- | ----------------------------------------------------------------- |
| Plugin Overview        | Explains plugin purpose and underlying tool                       |
| Capability Reference   | Lists supported capabilities                                      |
| Invocation Guide       | Explains request parameters and expected inputs                   |
| Result Guide           | Explains normalized outputs and findings                          |
| Evidence Guide         | Explains evidence captured and retained                           |
| Isolation Guide        | Explains execution boundaries                                     |
| Configuration Guide    | Explains non-secret configuration and secret references           |
| Failure Handling Guide | Explains known errors and remediation                             |
| Security Notes         | Explains permissions, network access, and sensitive data handling |

### 29.2 Documentation Rules

Documentation MUST identify supported plugin version and tool version.

Documentation MUST NOT contain secrets.

Documentation SHOULD include examples using non-sensitive fixtures.

---

## 30.0 Conformance Requirements

### 30.1 Tool Plugin Package Conformance

A plugin package conforms to ISL v3.9 if it:

* includes a valid manifest
* declares plugin type
* declares underlying tool and version
* declares capabilities
* declares invocation method
* declares output contracts
* declares isolation and trust profiles
* contains no plaintext secrets
* includes tests or validation evidence

### 30.2 Tool Registry Conformance

A Tool Registry conforms to ISL v3.9 if it:

* stores plugin registry records
* validates manifests
* records plugin lifecycle state
* records capabilities
* records trust and isolation profiles
* records health status
* prevents disabled or revoked plugins from selection
* records registration and lifecycle changes

### 30.3 Tool Invocation Conformance

A tool invocation conforms to ISL v3.9 if it:

* references a selected registered plugin
* declares capability invoked
* declares exact artifact versions when validating artifacts
* declares isolation profile
* declares timeout
* records governance check where required
* returns a structured result
* records findings and evidence where applicable
* emits telemetry
* creates traceability links

### 30.4 Tool Result Conformance

A tool result conforms to ISL v3.9 if it:

* references the invocation
* declares outcome
* records tool and plugin version
* records findings when present
* records evidence references where required
* records affected artifact versions
* declares next action
* does not treat timeout or error as passed

### 30.5 Enterprise Plugin Conformance

An enterprise plugin conforms to ISL v3.9 if it:

* has active trust and isolation profiles
* supports evidence capture
* has health checks
* has compatibility records
* has enterprise certification or validation evidence
* supports governed execution
* emits telemetry
* preserves traceability

### 30.6 Platform Conformance

A platform conforms to ISL v3.9 if it:

* integrates deterministic tools through registered plugins
* selects tools by declared capability
* invokes tools through standard request contracts
* normalizes tool results
* preserves evidence
* enforces isolation and trust profiles
* enforces governance controls
* records traceability
* emits tool telemetry
* handles plugin errors through defined error classes
* prevents unregistered, disabled, revoked, or untrusted tools from lifecycle-critical execution

---

## 31.0 Summary

ISL v3.9 defines the Tool Plugin Architecture for the IMHOTEP platform. It establishes how deterministic engineering tools are integrated through governed, traceable, isolated, versioned, and observable plugins.

This revision strengthens the original tool plugin concept by defining plugin types, package structure, manifests, capability declarations, registry records, discovery, registration, lifecycle states, selection, invocation requests, invocation results, structured findings, evidence capture, execution isolation, trust profiles, governance controls, traceability, telemetry, health checks, versioning, configuration, testing, certification, extension, error classes, retirement, documentation, and conformance requirements.

A conforming Tool Plugin Architecture MUST make deterministic tools reliable members of the autonomous construction system. Plugins are not arbitrary scripts; they are controlled engineering instruments that validate, inspect, package, and prepare artifacts under explicit capability contracts, isolation boundaries, governance policy, traceability, and evidence requirements.
