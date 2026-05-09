# IMHOTEP Specification Language (ISL) v3.10

# The Platform Security Architecture

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1, ISL v3.4, ISL v3.5, ISL v3.6, ISL v3.7, ISL v3.8, ISL v3.9
**Supersedes:** ISL r0 v3.10 The Platform Security Architecture
**Document Type:** Platform Security, Trust Boundary, Threat Control, and Assurance Specification

---

## 1.0 Scope

This document defines the Platform Security Architecture for the IMHOTEP platform. It specifies the security controls, trust boundaries, identity controls, authorization rules, secret handling, isolation requirements, prompt and context injection defenses, model security, tool security, artifact integrity, audit requirements, monitoring requirements, incident response expectations, and conformance requirements for secure autonomous software construction.

This document applies to all IMHOTEP platform components, including the Specification Engine, Semantic Model Engine, Planning Engine, Runtime Orchestrator, Agent Orchestrator, Model Integration Layer, Tool Integration Layer, Artifact Repository, Traceability Engine, Governance Engine, State Manager, Observability Layer, deployment services, plugins, workers, and extension points.

This document defines:

* security principles
* trust zones and trust boundaries
* identity and access control
* service-to-service security
* project and tenant isolation
* data classification
* secret handling
* context protection
* prompt and context injection controls
* model interaction security
* tool and plugin execution security
* generated-code execution security
* artifact integrity and repository security
* governance security
* audit and evidence security
* telemetry security
* vulnerability management
* incident response
* security testing
* security conformance requirements

---

## 2.0 Purpose

The purpose of the Platform Security Architecture is to ensure that autonomous construction is safe, controlled, auditable, and appropriate for enterprise use. IMHOTEP ingests structured specifications, uses reasoning agents and models, invokes deterministic tools, generates artifacts, executes validation workflows, and prepares deployment assets. Each of these activities introduces security risk.

The security architecture exists to ensure that:

* untrusted specifications cannot control platform behavior
* model prompts and context packages cannot bypass governance or validation
* agents cannot exceed their role authority
* generated artifacts cannot become stable without validation and traceability
* tools and plugins cannot compromise the platform environment
* secrets are never exposed through prompts, logs, telemetry, artifacts, or model context
* project and tenant boundaries are enforced
* security findings affect runtime and lifecycle progression
* audit and governance evidence remains protected
* platform behavior can be investigated after security-relevant events

---

## 3.0 Security Design Principles

### 3.1 Zero Trust for Inputs

All externally supplied input MUST be treated as untrusted until validated. This includes specifications, uploaded files, prior artifacts, model responses, tool outputs, external API responses, repository content, telemetry payloads, and user instructions.

### 3.2 Least Privilege

Users, agents, services, tools, plugins, workers, and model providers MUST receive only the minimum access required to perform their authorized function.

### 3.3 Role-Bounded Autonomy

Autonomous agents MUST operate only within their declared role contracts. No agent MAY approve governance gates, mark validation as passed, promote artifacts, access secrets, or authorize deployment unless a separate specification explicitly grants that authority and governance permits it.

### 3.4 Defense in Depth

Security MUST be enforced at multiple layers: identity, authorization, context control, model protocol, tool sandboxing, repository controls, runtime gates, governance decisions, audit records, and telemetry monitoring.

### 3.5 Deterministic Security Validation

Where security can be checked deterministically, the platform MUST use deterministic tools or policy checks rather than relying only on model reasoning.

### 3.6 Fail Closed

Security control failure MUST default to blocking the affected action unless a governance-approved degraded-mode policy explicitly permits continuation.

### 3.7 Auditability

Security-relevant decisions MUST be recorded. The platform MUST preserve enough evidence to reconstruct who acted, what was evaluated, what was permitted, what was blocked, and why.

---

## 4.0 Security Architecture Overview

The Platform Security Architecture is implemented as a cross-cutting control layer across all platform subsystems.

| Security Control Area    | Primary Enforcement Components                                     |
| ------------------------ | ------------------------------------------------------------------ |
| Identity                 | Identity Provider, Security Module, Governance Engine              |
| Authorization            | Authorization Manager, Governance Engine, Runtime Orchestrator     |
| Context Protection       | Context Controller, Agent Orchestrator, Model Integration Layer    |
| Prompt Injection Defense | Context Controller, Model Interaction Protocol, Security Validator |
| Secret Protection        | Secret Manager Adapter, Runtime, Tool Gateway, Model Gateway       |
| Tool Isolation           | Tool Gateway, Plugin Runtime, Sandbox Manager                      |
| Artifact Integrity       | Artifact Repository, Traceability Engine, Validation Tools         |
| Governance Integrity     | Governance Engine, Audit Writer, State Manager                     |
| Telemetry Protection     | Observability Layer, Telemetry Processor                           |
| Incident Handling        | Security Finding Manager, Governance Engine, Runtime Orchestrator  |

### 4.1 Architecture Rules

Security controls MUST be enforced before actions occur, not merely reported afterward.

Security checks that affect lifecycle progression MUST create structured records.

Security findings MUST be linked to affected tasks, artifacts, agents, tools, models, contexts, or governance records.

---

## 5.0 Trust Zones

The platform MUST define trust zones to separate responsibilities and exposure.

| Trust Zone                | Description                                                               |
| ------------------------- | ------------------------------------------------------------------------- |
| user-access-zone          | User interfaces, APIs, CLIs, dashboards                                   |
| control-plane-zone        | Governance, scheduling, policy, configuration, orchestration              |
| reasoning-zone            | Agents, context packages, model requests, model responses                 |
| tool-execution-zone       | Tool plugins, compilers, scanners, tests, generated-code execution        |
| artifact-zone             | Artifact content, metadata, packages, repository branches                 |
| state-zone                | Runtime state, memory, checkpoints, recovery records                      |
| audit-zone                | Governance audit records and immutable evidence                           |
| telemetry-zone            | Logs, traces, metrics, alerts, dashboards                                 |
| external-integration-zone | External model providers, tool services, repositories, identity providers |
| sandbox-zone              | Isolated execution for generated code and untrusted tool activity         |

### 5.1 Trust Zone Rules

Data moving between trust zones MUST pass through an approved interface.

External integrations MUST terminate in gateway components or approved adapters.

Sandbox-zone outputs MUST be validated before entering artifact-zone or state-zone.

Audit-zone records MUST be immutable or correction-only.

---

## 6.0 Identity Architecture

Identity controls establish who or what is acting.

### 6.1 Identity Types

| Identity Type              | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| human-user                 | Human interacting with the platform                          |
| service-identity           | Platform service identity                                    |
| worker-identity            | Runtime worker identity                                      |
| agent-identity             | Logical identity for an agent implementation                 |
| tool-plugin-identity       | Logical identity for a tool plugin                           |
| automation-identity        | CI/CD, deployment, or scheduled automation                   |
| external-provider-identity | External system, model provider, repository, or tool service |

### 6.2 Identity Rules

Every security-relevant action MUST be attributable to an identity.

Enterprise deployments MUST authenticate users through an approved identity provider.

Service-to-service calls in enterprise and distributed deployments MUST use service identities.

Disabled or revoked identities MUST NOT authorize platform actions.

Human approval records MUST identify authenticated human or authorized role identity.

Platform automation MUST NOT impersonate human approval roles.

---

## 7.0 Authorization Model

Authorization determines whether an authenticated identity may perform an action.

### 7.1 Authorization Dimensions

Authorization decisions MUST consider:

* identity
* role
* project scope
* tenant scope
* environment
* risk tier
* action type
* target object
* lifecycle phase
* governance policy
* sensitivity classification
* approval or waiver state

### 7.2 Standard Authorization Actions

| Action               | Description                   |
| -------------------- | ----------------------------- |
| specification-read   | Read specification content    |
| specification-write  | Modify specification content  |
| readiness-transition | Change readiness level        |
| construction-start   | Start autonomous construction |
| task-dispatch        | Dispatch runtime task         |
| agent-invoke         | Invoke agent                  |
| model-invoke         | Invoke model                  |
| tool-invoke          | Invoke tool plugin            |
| artifact-admit       | Admit artifact candidate      |
| artifact-promote     | Promote artifact to stable    |
| governance-approve   | Approve governance gate       |
| waiver-grant         | Grant waiver                  |
| override-apply       | Apply override                |
| deployment-authorize | Authorize deployment          |
| audit-read           | Read audit records            |
| configuration-change | Modify platform configuration |
| secret-access        | Access secret reference       |

### 7.3 Authorization Rules

Authorization MUST be evaluated before governed actions.

Authorization denial MUST block the action.

High and Critical risk systems MUST enforce separation of duties for approval roles.

A user or service MUST NOT gain access to another project or tenant unless cross-scope access is explicitly governed.

---

## 8.0 Project and Tenant Isolation

Shared deployments MUST prevent cross-project and cross-tenant contamination.

### 8.1 Isolation Scope

The platform MUST isolate:

* specifications
* canonical models
* construction plans
* execution graphs
* task queues
* artifacts
* traceability graphs
* governance records
* model context packages
* tool workspaces
* secrets
* telemetry
* evidence packages
* deployment preparation outputs

### 8.2 Isolation Rules

A task from one project MUST NOT read or write another project’s artifacts unless governed cross-project access is explicitly authorized.

A tenant’s context package MUST NOT include another tenant’s data unless cross-tenant access is explicitly authorized.

Telemetry dashboards MUST enforce project and tenant access controls.

Shared workers MUST clean workspaces between tasks.

Shared model and tool providers MUST enforce context and artifact isolation.

---

## 9.0 Data Classification

The platform MUST classify data to determine allowed handling.

### 9.1 Classification Levels

| Classification | Description                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------- |
| public         | Approved for public disclosure                                                                                         |
| internal       | Internal platform or organization information                                                                          |
| confidential   | Sensitive business, architecture, artifact, or operational information                                                 |
| restricted     | Highly sensitive information including secrets, regulated data, tenant-isolated data, or security-critical information |

### 9.2 Classification Rules

Every context package MUST declare sensitivity classification.

Every artifact MUST declare sensitivity classification.

Every telemetry event MUST declare redaction status and retention class.

Restricted data MUST NOT be sent to external model or tool providers unless governance explicitly permits it.

Classification downgrade MUST require governance approval.

---

## 10.0 Secret Management

Secrets require strict handling.

### 10.1 Secret Types

Secrets MAY include:

* model provider credentials
* repository credentials
* tool service credentials
* database credentials
* identity provider credentials
* signing keys
* encryption keys
* deployment credentials
* access tokens
* private keys

### 10.2 Secret Rules

Secrets MUST NOT be embedded in specifications.

Secrets MUST NOT be included in agent prompts or model context unless explicitly authorized by governance.

Secrets MUST NOT be written to logs, telemetry, generated artifacts, test fixtures, or evidence bundles.

Secrets MUST be referenced by secret identifiers rather than secret values.

Secret access MUST be scoped by project, tenant, environment, and service identity.

Secret exposure MUST create a critical security finding and halt or isolate affected execution.

---

## 11.0 Context Protection

Context packages are security-critical because they control what agents and models can see.

### 11.1 Context Control Checks

Before an agent or model receives context, the platform MUST check:

* context purpose
* task relevance
* source object identifiers
* sensitivity classification
* tenant and project scope
* secret presence
* untrusted content markers
* allowed model or provider trust profile
* allowed agent role
* governance restrictions
* context size and minimization
* redaction requirements

### 11.2 Context Protection Record Schema

| Field                        | Type     | Required    | Description                                |
| ---------------------------- | -------- | ----------- | ------------------------------------------ |
| context-protection-record-id | string   | REQUIRED    | Unique context protection record           |
| context-package-id           | string   | REQUIRED    | Context package evaluated                  |
| agent-invocation-id          | string   | CONDITIONAL | Agent invocation                           |
| model-interaction-request-id | string   | CONDITIONAL | Model request                              |
| task-id                      | string   | REQUIRED    | Task context                               |
| sensitivity-classification   | enum     | REQUIRED    | public, internal, confidential, restricted |
| secret-scan-outcome          | enum     | REQUIRED    | passed, failed, warning                    |
| scope-check-outcome          | enum     | REQUIRED    | passed, failed, warning                    |
| injection-risk-outcome       | enum     | REQUIRED    | passed, failed, warning                    |
| redactions-applied           | array    | CONDITIONAL | Redactions applied                         |
| protection-outcome           | enum     | REQUIRED    | passed, blocked, redacted, escalated       |
| evaluated-at                 | ISO 8601 | REQUIRED    | Evaluation time                            |
| evaluated-by                 | string   | REQUIRED    | Context controller                         |

### 11.3 Context Protection Rules

A context package containing unauthorized secrets MUST be blocked.

A context package containing cross-tenant data without authorization MUST be blocked.

A context package containing untrusted content MUST mark that content explicitly.

Redaction that removes required task context MUST trigger rejection or escalation.

---

## 12.0 Prompt and Context Injection Defense

Prompt injection and context injection occur when untrusted content attempts to override platform instructions, role boundaries, output contracts, governance controls, validation rules, or security policy.

### 12.1 Injection Classes

| Injection Class      | Description                                                                |
| -------------------- | -------------------------------------------------------------------------- |
| instruction-override | Attempts to override platform, role, or task instructions                  |
| role-escalation      | Attempts to change agent role or authority                                 |
| contract-bypass      | Attempts to avoid output contract requirements                             |
| governance-bypass    | Attempts to ignore approval, waiver, or policy requirements                |
| validation-bypass    | Attempts to mark work valid without deterministic validation               |
| secret-exfiltration  | Attempts to reveal or infer secrets                                        |
| tool-abuse           | Attempts to invoke unauthorized tools or commands                          |
| repository-abuse     | Attempts to write outside approved artifact paths                          |
| data-exfiltration    | Attempts to reveal restricted context or tenant data                       |
| persistence-abuse    | Attempts to store malicious instructions in artifacts, memory, or metadata |

### 12.2 Sanitization Definition

For ISL, sanitization means applying deterministic transformations or markings to untrusted content before it is used in agent, model, tool, repository, or runtime contexts.

Sanitization MAY include:

* separating untrusted content from platform instructions
* escaping or quoting untrusted text
* removing executable control sequences
* redacting secrets
* replacing restricted values with references
* stripping unauthorized tool commands
* removing unsupported file links or external references
* marking content as untrusted
* summarizing content when raw inclusion is unnecessary
* blocking content that cannot be safely represented

### 12.3 Injection Validation Rules

The platform MUST validate that:

* untrusted content is not placed in instruction sections
* untrusted content cannot redefine agent role
* untrusted content cannot change output contract
* untrusted content cannot alter governance decision
* untrusted content cannot declare validation success
* untrusted content cannot request unauthorized tools
* untrusted content cannot access secrets
* untrusted content cannot write to stable artifact state
* untrusted content cannot modify traceability or audit records
* untrusted content cannot override system or platform constraints

### 12.4 Injection Finding Schema

| Field                | Type     | Required    | Description                                                                                             |
| -------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------------------- |
| injection-finding-id | string   | REQUIRED    | Unique injection finding                                                                                |
| injection-class      | enum     | REQUIRED    | Class from §12.1                                                                                        |
| affected-object-id   | string   | REQUIRED    | Context, artifact, specification, model request, or tool invocation affected                            |
| affected-object-type | enum     | REQUIRED    | specification, context-package, artifact, model-request, model-response, tool-output, telemetry, memory |
| severity             | enum     | REQUIRED    | critical, high, medium, low                                                                             |
| detection-method     | enum     | REQUIRED    | pattern, schema, policy, model-assisted, human-review, deterministic-tool                               |
| evidence-reference   | string   | CONDITIONAL | Supporting evidence                                                                                     |
| handling             | enum     | REQUIRED    | block, sanitize, redact, quarantine, escalate, warn                                                     |
| detected-at          | ISO 8601 | REQUIRED    | Detection time                                                                                          |
| detected-by          | string   | REQUIRED    | Detector component                                                                                      |

### 12.5 Injection Handling Rules

A critical injection finding MUST block the affected action.

A secret-exfiltration finding MUST block the action and create a security incident record.

A governance-bypass or validation-bypass finding MUST block downstream use.

A sanitized context MUST retain a record of sanitization actions.

An injection finding that cannot be classified MUST escalate.

---

## 13.0 Model Security

Model interactions MUST follow the Model Interaction Protocol and Model Integration Layer controls.

### 13.1 Model Security Rules

Models MUST be invoked only through the Model Integration Layer.

Model requests MUST reference approved context packages.

Model providers MUST have trust profiles.

External model providers MUST be governed before receiving confidential or restricted context.

Raw model responses MUST be normalized and validated before use.

Model outputs MUST NOT directly approve governance gates, pass validation, promote artifacts, or authorize deployment.

Model output attempting to bypass controls MUST be rejected or escalated.

### 13.2 Model Trust Profile Requirements

A model trust profile MUST define:

* allowed risk tiers
* allowed sensitivity levels
* allowed environments
* data retention constraints
* provider location constraints
* human review requirements
* approval requirements
* prohibited use cases

---

## 14.0 Agent Security

Agents are autonomous reasoning components and MUST be security-bounded.

### 14.1 Agent Security Rules

Agents MUST be invoked only through the Agent Orchestrator or approved runtime path.

Agents MUST operate under a role manifest.

Agents MUST receive context only through controlled context packages.

Agents MUST NOT directly access secrets.

Agents MUST NOT directly mutate stable artifacts.

Agents MUST NOT directly write governance audit records.

Agents MUST NOT directly authorize deployment.

Agent outputs MUST be validated before downstream use.

Agent role violation MUST create a security finding.

---

## 15.0 Tool and Plugin Security

Tools and plugins can execute code, inspect artifacts, and interact with external systems.

### 15.1 Tool Security Rules

Tools MUST be registered before invocation.

Tool plugins MUST declare capabilities, trust profiles, isolation profiles, and required permissions.

Tool execution MUST use the least privileged isolation profile sufficient for the task.

Tools executing generated code MUST run in sandboxed-generated-code or equivalent isolation.

Tool outputs MUST be normalized before runtime decisions.

Tool findings affecting validation, security, or release MUST be retained as evidence.

A tool security boundary violation MUST halt or isolate affected execution.

---

## 16.0 Generated-Code Execution Security

Generated code is untrusted until validated.

### 16.1 Generated-Code Rules

Generated code MUST NOT execute in the control-plane-zone.

Generated code SHOULD execute only in sandbox-zone or tool-execution-zone.

Generated code MUST NOT receive platform secrets by default.

Generated code MUST NOT receive unrestricted network access by default.

Generated code MUST NOT write to stable artifact repositories directly.

Generated code execution MUST have time, filesystem, process, memory, and network limits.

Generated-code execution failure MUST NOT compromise runtime state.

---

## 17.0 Artifact Repository Security

The Artifact Repository protects generated and validated outputs.

### 17.1 Artifact Integrity Rules

Artifacts MUST have identifiers and metadata.

Artifact content MUST be associated with exact artifact versions.

Stable artifact promotion MUST require validation evidence, traceability, and governance eligibility.

Failed artifacts MUST NOT be promoted.

Quarantined artifacts MUST NOT be consumed by downstream lifecycle stages.

Artifact modification MUST require appropriate locks and authorization.

Artifact integrity failure MUST block release preparation.

### 17.2 Artifact Security Metadata

Each artifact SHOULD include:

| Field                      | Required    | Description                     |
| -------------------------- | ----------- | ------------------------------- |
| artifact-id                | YES         | Unique artifact identifier      |
| artifact-version           | YES         | Exact version                   |
| sensitivity-classification | YES         | Data classification             |
| provenance-reference       | YES         | Producing task or agent         |
| validation-status          | YES         | Validation state                |
| security-validation-status | CONDITIONAL | Security validation state       |
| traceability-status        | YES         | Traceability completeness       |
| integrity-hash             | SHOULD      | Integrity marker                |
| quarantine-status          | YES         | Whether artifact is quarantined |
| waiver-references          | CONDITIONAL | Waivers used                    |

---

## 18.0 Repository Path and Workspace Controls

Repository path controls prevent unauthorized writes and path traversal.

### 18.1 Path Control Rules

Artifact writes MUST occur only in approved repository workspaces.

Path traversal patterns MUST be rejected.

Generated artifacts MUST NOT overwrite platform source code unless operating in a governed platform-development context.

Tool outputs MUST NOT write outside declared workspace.

Package creation MUST use explicit artifact version references.

Repository merge operations MUST be governed when they affect stable branches.

---

## 19.0 State, Memory, and Persistence Security

State and memory are authoritative platform assets.

### 19.1 State Security Rules

State transitions MUST occur through authorized state interfaces.

Lifecycle-critical state MUST be durable.

State records MUST include actor or component identity.

State rollback MUST be governed and auditable.

Memory used by agents MUST be scoped, versioned, and traceable.

Hidden persistent memory that affects agent behavior MUST NOT be used.

State inconsistency affecting security MUST trigger recovery or halt.

---

## 20.0 Governance Security

Governance decisions protect autonomous activity from unsafe progression.

### 20.1 Governance Security Rules

Governance policies MUST be versioned.

Governance decisions MUST be attributable.

Governance approvals MUST enforce separation of duties.

Waivers MUST be scoped and time-bound where policy requires it.

Overrides MUST require override authority.

Expired waivers, approvals, and overrides MUST NOT authorize actions.

Governance audit write failure MUST halt governed action.

---

## 21.0 Audit Security

Audit records provide non-repudiation and accountability.

### 21.1 Audit Record Requirements

Security-relevant audit records MUST include:

* actor identity
* actor role
* action
* target object
* decision
* policy reference
* timestamp
* correlation-id
* rationale or finding reference
* approval, waiver, or override reference where applicable

### 21.2 Audit Rules

Audit records MUST be immutable or correction-only.

Audit records MUST NOT be deleted by platform components.

Audit records MUST be protected from unauthorized reading and modification.

Audit records for High and Critical risk systems MUST be retained according to lifecycle retention profile.

---

## 22.0 Telemetry and Log Security

Telemetry supports monitoring but may expose sensitive details if uncontrolled.

### 22.1 Telemetry Security Rules

Telemetry MUST NOT contain secrets.

Telemetry containing restricted information MUST be redacted, summarized, or access-controlled.

Security events MUST include correlation identifiers.

Critical security findings MUST generate alerts.

Telemetry failure MUST NOT hide security failures.

Telemetry export MUST obey classification and governance policy.

---

## 23.0 Network Security

Network security controls communication between platform components and external systems.

### 23.1 Network Rules

External model providers MUST be accessed only through the Model Integration Layer.

External tool services MUST be accessed only through the Tool Integration Layer.

Repository services MUST require authenticated access.

Service-to-service calls SHOULD use encrypted channels in enterprise deployments.

Restricted environments SHOULD use network segmentation.

Generated code MUST NOT receive unrestricted network access unless explicitly governed.

---

## 24.0 Supply Chain Security

The platform depends on models, tools, plugins, libraries, templates, and generated artifacts.

### 24.1 Supply Chain Controls

The platform SHOULD support:

* dependency scanning
* plugin certification
* model provider trust profiles
* tool version recording
* artifact integrity hashes
* package manifests
* SBOM generation where configured
* template version recording
* provenance records
* vulnerability findings
* license findings

### 24.2 Supply Chain Rules

Tool and model versions used for construction MUST be recorded.

Plugins used in lifecycle-critical validation SHOULD have certification evidence.

Package manifests MUST reference exact artifact versions.

Critical supply-chain findings MUST block release readiness unless waived.

---

## 25.0 Security Validation

Security validation applies deterministic and governed checks to specifications, artifacts, tools, and deployment preparation.

### 25.1 Security Validation Categories

| Category               | Description                                                                |
| ---------------------- | -------------------------------------------------------------------------- |
| specification-security | Security requirements, policies, authentication, authorization definitions |
| context-security       | Secret detection, injection detection, sensitivity validation              |
| artifact-security      | Static analysis, secret scanning, dependency audit, vulnerability scan     |
| model-security         | Provider trust, data retention, restricted context checks                  |
| tool-security          | Plugin trust, isolation, permission review                                 |
| repository-security    | Path control, branch control, artifact integrity                           |
| deployment-security    | Manifest validation, environment compatibility, secrets policy             |
| governance-security    | Approval, waiver, override, audit integrity                                |

### 25.2 Security Validation Rules

Security validation MUST run before release readiness for High and Critical systems.

A critical security finding MUST block artifact promotion, release readiness, or deployment authorization unless a valid waiver exists.

Security validation evidence MUST be retained.

Security validation failure MUST produce structured findings.

---

## 26.0 Security Finding Model

Security findings record detected risks.

### 26.1 Security Finding Schema

| Field                | Type     | Required    | Description                                                                                                                                                |
| -------------------- | -------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| security-finding-id  | string   | REQUIRED    | Unique finding identifier                                                                                                                                  |
| finding-category     | enum     | REQUIRED    | specification, context, prompt-injection, model, agent, tool, artifact, repository, deployment, governance, telemetry, network, supply-chain               |
| severity             | enum     | REQUIRED    | critical, high, medium, low, informational                                                                                                                 |
| affected-object-id   | string   | REQUIRED    | Affected object                                                                                                                                            |
| affected-object-type | enum     | REQUIRED    | specification, context-package, model-request, model-response, agent-output, tool-invocation, artifact, repository, package, deployment, governance-record |
| policy-reference     | string   | CONDITIONAL | Policy or control reference                                                                                                                                |
| evidence-reference   | string   | CONDITIONAL | Evidence                                                                                                                                                   |
| recommended-action   | enum     | REQUIRED    | block, repair, redact, quarantine, revalidate, waive, escalate, monitor                                                                                    |
| blocks-progression   | boolean  | REQUIRED    | Whether finding blocks lifecycle progression                                                                                                               |
| detected-at          | ISO 8601 | REQUIRED    | Detection time                                                                                                                                             |
| detected-by          | string   | REQUIRED    | Detector                                                                                                                                                   |

### 26.2 Finding Rules

Critical findings MUST block affected progression unless waived.

Findings used for waiver decisions MUST be linked to waiver records.

Findings triggering repair MUST be linked to repair records.

Findings affecting release or deployment MUST be included in lifecycle evidence packages.

---

## 27.0 Quarantine Model

Quarantine isolates unsafe or uncertain content.

### 27.1 Quarantine Conditions

The platform MUST quarantine artifacts, contexts, outputs, or workspaces when:

* provenance is unclear
* traceability is missing
* security validation fails critically
* prompt injection is detected
* secret exposure is detected
* generated code performs unauthorized behavior
* repository integrity is uncertain
* interrupted execution produced ambiguous side effects

### 27.2 Quarantine Record Schema

| Field                   | Type     | Required    | Description                                                                |
| ----------------------- | -------- | ----------- | -------------------------------------------------------------------------- |
| quarantine-record-id    | string   | REQUIRED    | Unique quarantine record                                                   |
| quarantined-object-id   | string   | REQUIRED    | Object quarantined                                                         |
| quarantined-object-type | enum     | REQUIRED    | artifact, context-package, model-response, tool-output, workspace, package |
| reason                  | string   | REQUIRED    | Quarantine reason                                                          |
| triggering-finding-id   | string   | CONDITIONAL | Finding that triggered quarantine                                          |
| allowed-actions         | array    | REQUIRED    | inspect, delete, repair, revalidate, release, archive                      |
| release-requires        | enum     | REQUIRED    | governance-approval, security-review, revalidation, forbidden              |
| quarantined-at          | ISO 8601 | REQUIRED    | Quarantine time                                                            |
| quarantined-by          | string   | REQUIRED    | Component or actor                                                         |

### 27.3 Quarantine Rules

Quarantined artifacts MUST NOT be promoted.

Quarantined context packages MUST NOT be sent to models.

Quarantined outputs MUST NOT be consumed downstream.

Release from quarantine MUST be recorded and governed.

---

## 28.0 Incident Response

Security incidents require controlled response.

### 28.1 Security Incident Classes

| Incident Class                | Description                                                            |
| ----------------------------- | ---------------------------------------------------------------------- |
| secret-exposure               | Secret appeared in context, logs, artifact, model output, or telemetry |
| unauthorized-access           | Identity accessed unauthorized object                                  |
| cross-tenant-exposure         | Tenant boundary violation                                              |
| prompt-injection-critical     | Injection attempt affected or could affect control behavior            |
| tool-sandbox-escape           | Tool or generated code violated sandbox boundary                       |
| artifact-integrity-compromise | Artifact content or metadata integrity failed                          |
| governance-bypass-attempt     | Attempt to bypass approval, waiver, or override                        |
| audit-integrity-failure       | Audit write, retention, or immutability failure                        |
| supply-chain-critical         | Critical dependency, plugin, or package compromise                     |
| deployment-security-block     | Deployment preparation or authorization blocked by security condition  |

### 28.2 Incident Record Schema

| Field                  | Type     | Required    | Description                                                       |
| ---------------------- | -------- | ----------- | ----------------------------------------------------------------- |
| security-incident-id   | string   | REQUIRED    | Unique incident identifier                                        |
| incident-class         | enum     | REQUIRED    | Incident class                                                    |
| severity               | enum     | REQUIRED    | critical, high, medium, low                                       |
| affected-project-id    | string   | CONDITIONAL | Project affected                                                  |
| affected-tenant-id     | string   | CONDITIONAL | Tenant affected                                                   |
| affected-object-ids    | array    | REQUIRED    | Objects affected                                                  |
| triggering-finding-ids | array    | CONDITIONAL | Findings that triggered incident                                  |
| containment-action     | enum     | REQUIRED    | halt, isolate, quarantine, revoke, rotate-secrets, block, monitor |
| status                 | enum     | REQUIRED    | opened, contained, investigating, resolved, closed                |
| opened-at              | ISO 8601 | REQUIRED    | Open time                                                         |
| resolved-at            | ISO 8601 | CONDITIONAL | Resolution time                                                   |
| owner-role             | string   | REQUIRED    | Responsible role                                                  |

### 28.3 Incident Response Rules

Critical incidents MUST halt or isolate affected execution.

Secret exposure incidents MUST trigger secret rotation or governance-approved compensating action.

Cross-tenant exposure MUST escalate.

Audit integrity failure MUST halt governed actions.

Incident closure MUST include resolution evidence.

---

## 29.0 Security Monitoring and Alerting

Security monitoring detects abnormal or unsafe behavior.

### 29.1 Required Security Alerts

The platform SHOULD alert on:

* critical security finding
* secret exposure
* prompt injection critical finding
* governance bypass attempt
* unauthorized access attempt
* tool sandbox violation
* generated-code network violation
* cross-tenant access attempt
* artifact integrity failure
* audit write failure
* repeated model output contract failures
* repeated tool security failures
* deployment authorization security block

### 29.2 Alert Rules

Critical alerts MUST include affected object identifiers.

Security alerts MUST include correlation identifiers.

Security alert suppression MUST NOT hide unresolved critical incidents.

Alerts MUST be routed according to governance and operations policy.

---

## 30.0 Vulnerability Management

The platform MUST support vulnerability detection and remediation.

### 30.1 Vulnerability Sources

Vulnerabilities MAY be detected in:

* platform source code
* generated artifacts
* dependencies
* tool plugins
* model providers
* deployment manifests
* infrastructure definitions
* container images
* configuration templates
* secrets management
* telemetry exports

### 30.2 Vulnerability Record Schema

| Field                   | Type     | Required    | Description                                                                                  |
| ----------------------- | -------- | ----------- | -------------------------------------------------------------------------------------------- |
| vulnerability-record-id | string   | REQUIRED    | Unique vulnerability record                                                                  |
| affected-object-id      | string   | REQUIRED    | Object affected                                                                              |
| affected-object-type    | enum     | REQUIRED    | platform, artifact, dependency, plugin, model-provider, container, deployment, configuration |
| severity                | enum     | REQUIRED    | critical, high, medium, low                                                                  |
| source                  | string   | REQUIRED    | Scanner, tool, advisory, reviewer, incident                                                  |
| finding-reference       | string   | CONDITIONAL | Finding reference                                                                            |
| remediation-status      | enum     | REQUIRED    | open, accepted, repaired, waived, false-positive, closed                                     |
| required-action         | string   | REQUIRED    | Remediation                                                                                  |
| detected-at             | ISO 8601 | REQUIRED    | Detection time                                                                               |
| due-at                  | ISO 8601 | CONDITIONAL | Remediation deadline                                                                         |

### 30.3 Vulnerability Rules

Critical vulnerabilities affecting release artifacts MUST block release readiness unless waived.

Critical vulnerabilities affecting platform control plane MUST trigger incident response.

Waived vulnerabilities MUST reference governance waiver records.

Remediation MUST trigger revalidation where artifacts or platform behavior changed.

---

## 31.0 Security Configuration

Security configuration MUST be versioned and governed.

### 31.1 Security Configuration Domains

| Domain              | Examples                                    |
| ------------------- | ------------------------------------------- |
| identity            | identity providers, MFA, service identities |
| authorization       | roles, permissions, policy bindings         |
| context-protection  | redaction rules, injection detection rules  |
| model-security      | trust profiles, provider allow lists        |
| tool-security       | plugin trust, isolation profiles            |
| repository-security | branch protection, path restrictions        |
| secret-management   | secret stores, rotation rules               |
| telemetry-security  | redaction, export restrictions              |
| incident-response   | alert routing, containment policies         |

### 31.2 Configuration Rules

Security configuration MUST be versioned.

Security configuration changes MUST be auditable.

Security configuration changes affecting active execution MUST trigger impact evaluation.

Revoked security configuration MUST NOT be used for new execution.

---

## 32.0 Security Error Classes

This section defines platform-security-specific error classes.

| Error Class                         | Description                                           | Default Handling           |
| ----------------------------------- | ----------------------------------------------------- | -------------------------- |
| security-authentication-failed      | Identity could not authenticate                       | block                      |
| security-authorization-denied       | Identity lacks permission                             | block                      |
| security-sod-violation              | Separation of duties violation                        | block                      |
| security-context-secret-detected    | Secret detected in context                            | block and incident         |
| security-context-scope-violation    | Context crosses project or tenant scope               | block                      |
| security-prompt-injection-detected  | Prompt or context injection detected                  | block or sanitize          |
| security-model-trust-violation      | Model trust profile disallows use                     | block                      |
| security-tool-trust-violation       | Tool trust profile disallows use                      | block                      |
| security-tool-sandbox-violation     | Tool violated sandbox controls                        | halt and incident          |
| security-generated-code-violation   | Generated code violated execution policy              | halt or quarantine         |
| security-artifact-integrity-failed  | Artifact integrity check failed                       | quarantine                 |
| security-repository-path-violation  | Unauthorized path access or traversal                 | block                      |
| security-secret-exposure            | Secret exposed in output, artifact, log, or telemetry | halt and incident          |
| security-audit-write-failed         | Security or governance audit write failed             | halt governed action       |
| security-telemetry-redaction-failed | Sensitive telemetry not redacted                      | block export               |
| security-cross-tenant-exposure      | Tenant boundary violated                              | halt and incident          |
| security-governance-bypass-attempt  | Attempt to bypass governance                          | block and escalate         |
| security-deployment-blocked         | Deployment blocked by security condition              | block deployment           |
| security-vulnerability-critical     | Critical vulnerability detected                       | block affected progression |
| security-quarantine-release-denied  | Quarantine release not authorized                     | block                      |

### 32.1 Security Error Record Schema

| Field                | Type     | Required    | Description                                                                   |
| -------------------- | -------- | ----------- | ----------------------------------------------------------------------------- |
| security-error-id    | string   | REQUIRED    | Unique security error identifier                                              |
| error-class          | enum     | REQUIRED    | Error class                                                                   |
| severity             | enum     | REQUIRED    | critical, high, medium, low                                                   |
| affected-object-id   | string   | CONDITIONAL | Object affected                                                               |
| affected-object-type | enum     | CONDITIONAL | Context, artifact, model, tool, repository, governance, telemetry, deployment |
| task-id              | string   | CONDITIONAL | Task affected                                                                 |
| execution-graph-id   | string   | CONDITIONAL | Execution affected                                                            |
| lifecycle-run-id     | string   | CONDITIONAL | Lifecycle affected                                                            |
| message              | string   | REQUIRED    | Human-readable explanation                                                    |
| default-handling     | enum     | REQUIRED    | block, halt, quarantine, sanitize, redact, escalate, incident                 |
| required-action      | string   | REQUIRED    | Required remediation                                                          |
| detected-at          | ISO 8601 | REQUIRED    | Detection time                                                                |
| detected-by          | string   | REQUIRED    | Detector component                                                            |

---

## 33.0 Security Testing

Security controls MUST be testable.

### 33.1 Required Test Categories

| Test Category            | Purpose                                                             |
| ------------------------ | ------------------------------------------------------------------- |
| authentication           | Verify identity controls                                            |
| authorization            | Verify role, scope, and permission checks                           |
| separation-of-duties     | Verify approval role separation                                     |
| context-protection       | Verify context minimization, redaction, scope checks                |
| prompt-injection         | Verify injection detection and handling                             |
| secret-detection         | Verify secrets are blocked from prompts, logs, artifacts, telemetry |
| model-security           | Verify model trust profile enforcement                              |
| tool-security            | Verify plugin trust and isolation                                   |
| sandbox                  | Verify generated-code and tool execution boundaries                 |
| artifact-integrity       | Verify artifact hashes, metadata, promotion rules                   |
| repository-path          | Verify path traversal and workspace restrictions                    |
| telemetry-redaction      | Verify telemetry redaction and export controls                      |
| audit-integrity          | Verify immutable or correction-only audit records                   |
| incident-response        | Verify containment, escalation, and closure records                 |
| vulnerability-management | Verify findings block or route correctly                            |

### 33.2 Testing Rules

Prompt injection tests MUST include attempts to override role, output contract, validation, governance, and secret-handling instructions.

Secret detection tests MUST verify prompts, context packages, model responses, tool outputs, logs, telemetry, artifacts, and evidence bundles.

Sandbox tests MUST verify filesystem, process, network, memory, and timeout restrictions.

High and Critical deployments MUST run security tests before production authorization.

---

## 34.0 Security Conformance Requirements

### 34.1 Identity and Authorization Conformance

A platform conforms to identity and authorization requirements if it:

* authenticates users and service identities
* enforces role, project, tenant, environment, action, and risk-tier authorization
* records actor identity for security-relevant actions
* enforces separation of duties where required
* blocks disabled or revoked identities

### 34.2 Context and Prompt Security Conformance

A platform conforms to context and prompt security requirements if it:

* classifies context packages
* scans for secrets
* enforces project and tenant scope
* marks untrusted content
* separates untrusted content from platform instructions
* detects prompt and context injection classes
* blocks or sanitizes unsafe context
* records context protection and injection findings

### 34.3 Model Security Conformance

A platform conforms to model security requirements if it:

* invokes models only through the Model Integration Layer
* enforces model trust profiles
* blocks unauthorized restricted context transmission
* normalizes and validates model outputs
* prevents model outputs from bypassing governance, validation, traceability, artifact promotion, or deployment authorization

### 34.4 Tool Security Conformance

A platform conforms to tool security requirements if it:

* invokes only registered tools
* enforces tool trust and isolation profiles
* sandboxes generated-code execution
* records tool security findings
* blocks tool sandbox violations
* prevents tool outputs from bypassing normalization and validation

### 34.5 Artifact and Repository Security Conformance

A platform conforms to artifact and repository security requirements if it:

* controls artifact admission
* enforces metadata and versioning
* prevents failed or quarantined artifacts from promotion
* enforces path controls
* records integrity evidence
* links artifacts to validation and traceability records

### 34.6 Governance and Audit Security Conformance

A platform conforms to governance and audit security requirements if it:

* records governance decisions
* preserves immutable or correction-only audit records
* blocks governed actions on audit write failure
* enforces waiver and override rules
* records security-relevant approvals, waivers, overrides, and incidents

### 34.7 Platform Security Conformance

A platform conforms to ISL v3.10 if it:

* defines trust zones
* enforces identity and authorization
* protects secrets
* protects context packages
* defends against prompt and context injection
* secures model interactions
* secures tool and plugin execution
* sandboxes generated code
* protects artifacts and repositories
* enforces governance security
* preserves audit evidence
* protects telemetry
* supports vulnerability management
* supports incident response
* tests security controls
* fails closed when security controls cannot be enforced

---

## 35.0 Summary

ISL v3.10 defines the Platform Security Architecture for IMHOTEP. It establishes the security control layer that protects autonomous software construction across specifications, agents, models, tools, artifacts, repositories, runtime orchestration, governance, telemetry, deployment preparation, and lifecycle evidence.

This revision strengthens the original security architecture by making security mechanisms explicit. It defines trust zones, identity types, authorization dimensions, project and tenant isolation, classification, secret management, context protection, prompt and context injection classes, sanitization rules, validation rules, model security, agent security, tool security, generated-code sandboxing, artifact integrity, repository path controls, governance security, audit security, telemetry security, network security, supply-chain security, security validation, findings, quarantine, incident response, monitoring, vulnerability management, configuration, error classes, testing, and conformance.

A conforming IMHOTEP platform MUST treat security as an active construction control, not an after-the-fact review. Autonomous construction may be fast and generative, but it MUST remain identity-bound, role-bounded, context-controlled, least-privileged, sandboxed, traceable, governed, observable, auditable, and fail-closed.
