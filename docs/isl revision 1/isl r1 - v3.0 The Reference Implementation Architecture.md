# IMHOTEP Specification Language (ISL) v3.0

# The Reference Implementation Architecture

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7
**Supersedes:** ISL r0 v3.0 The Reference Implementation Architecture
**Document Type:** Reference Implementation Architecture Specification

---

## 1.0 Scope

This document defines the Reference Implementation Architecture for the IMHOTEP platform. It specifies the implementation-level components, service boundaries, module responsibilities, internal contracts, persistence boundaries, integration points, runtime deployment assumptions, and conformance requirements for a reference implementation of the platform architecture defined in ISL v2.0.

This document does not require a single programming language, framework, database, model provider, cloud provider, or deployment platform. It defines a technology-neutral reference architecture that implementations MAY realize using different stacks while preserving the required logical boundaries and behavioral contracts.

This document defines:

* reference implementation principles
* implementation layers
* core modules
* service boundaries
* internal API categories
* persistence components
* runtime components
* model and tool gateways
* artifact repository integration
* governance and traceability integration
* observability integration
* configuration and dependency rules
* testing expectations
* implementation conformance requirements

---

## 2.0 Purpose of the Reference Implementation Architecture

The purpose of the Reference Implementation Architecture is to translate the platform architecture into an implementable system structure. The v2.x corpus defines what the IMHOTEP platform must do. This document defines how a reference implementation should organize those responsibilities into concrete modules, services, interfaces, stores, workers, and integration boundaries.

The reference implementation is not merely a prototype. It is the baseline architecture from which proof-of-concept, local-first, team, enterprise, distributed, and regulated deployments may evolve. It must therefore be simple enough to implement incrementally while remaining structurally sound enough to support enterprise hardening.

The Reference Implementation Architecture exists to ensure that:

* platform subsystems have clear implementation boundaries
* implementation modules align with the normative v2.x architecture
* local-first operation can evolve into distributed deployment
* agent, model, tool, repository, governance, traceability, state, and telemetry concerns remain separated
* deterministic validation and governance cannot be bypassed
* persistence and state are explicit
* implementation components are testable
* future reference pipelines can be built without architectural rework

---

## 3.0 Normative References

| Reference     | Purpose                                                          |
| ------------- | ---------------------------------------------------------------- |
| ISL v0.0      | Corpus principles, conformance language, and standards alignment |
| ISL v1.0      | Authored specification language                                  |
| ISL v1.1      | Canonical semantic model                                         |
| ISL v1.2      | Execution phases and construction lifecycle                      |
| ISL v1.3      | Readiness levels and execution authorization                     |
| ISL v1.4      | Traceability graph and provenance requirements                   |
| ISL v1.5      | Construction planning and task graph model                       |
| ISL v1.6      | Tool integration and deterministic validation                    |
| ISL v1.7      | Governance, approval gates, waivers, overrides, and audit        |
| ISL v2.0      | Platform architecture model                                      |
| ISL v2.1      | Agent collaboration and role contracts                           |
| ISL v2.2      | State and memory model                                           |
| ISL v2.3      | Artifact repository model                                        |
| ISL v2.4      | Execution runtime model                                          |
| ISL v2.5      | Model integration and abstraction layer                          |
| ISL v2.6      | Observability and telemetry model                                |
| ISL v2.7      | Enterprise deployment and scaling model                          |
| RFC 2119      | Normative terminology                                            |
| OpenTelemetry | Telemetry alignment                                              |
| W3C PROV-DM   | Traceability and provenance alignment                            |

---

## 4.0 Terms and Definitions

**Reference Implementation**
A concrete implementation architecture intended to demonstrate and guide construction of a conforming IMHOTEP platform.

**Implementation Module**
A library, package, project, assembly, service module, or equivalent unit implementing a bounded platform responsibility.

**Reference Service**
A deployable service exposing or coordinating one or more platform capabilities.

**Gateway Service**
A boundary service that mediates access to external or provider-specific systems, such as models or deterministic tools.

**Internal Contract**
A structured API, schema, interface, message, or data record used between reference implementation components.

**Local Monolith Mode**
A deployment mode in which multiple logical subsystems run in one process while preserving logical boundaries.

**Distributed Service Mode**
A deployment mode in which platform subsystems run as separate services or worker pools.

**Reference Store**
A persistence component used to store state, memory, artifacts, telemetry, governance records, or traceability data.

---

## 5.0 Reference Implementation Design Principles

### 5.1 Preserve Logical Architecture

The reference implementation MUST preserve the logical subsystem boundaries defined in ISL v2.0, even when implemented as a single process or local application.

Combining modules for convenience MUST NOT allow bypass of specification parsing, canonical normalization, planning validation, runtime governance, deterministic validation, traceability recording, artifact repository admission, or state consistency.

### 5.2 Prefer Modular Monolith Before Distributed Complexity

The first reference implementation SHOULD begin as a modular monolith or small set of services with strong internal boundaries. This supports local-first development and reduces operational complexity while preserving a path toward distributed deployment.

The implementation MUST avoid hard dependencies that prevent later extraction of services, workers, gateways, or stores.

### 5.3 Contracts Before Implementation Detail

The reference implementation MUST define explicit contracts between major modules before relying on implementation-specific coupling.

Interfaces, request records, result records, state records, and event records SHOULD be defined independently of concrete adapters and storage mechanisms.

### 5.4 Deterministic Validation Is a Hard Boundary

Generated artifacts MUST pass through deterministic validation or governance waiver before promotion. The implementation MUST NOT allow model output or agent output to bypass tool validation when validation is applicable.

### 5.5 Governance Is Runtime-Enforced

Governance MUST be implemented as an enforceable runtime decision service or module. It MUST NOT be limited to passive metadata, comments, or post-execution reporting.

### 5.6 Traceability Is Written During Execution

Traceability links MUST be created as lifecycle events occur. The implementation MUST NOT rely solely on after-the-fact reconstruction.

### 5.7 Local-First, Enterprise-Ready

The reference implementation SHOULD run locally with local stores, local tools, and local or configured model providers. The same logical architecture MUST also support migration to enterprise infrastructure.

---

## 6.0 Reference Implementation Layers

The reference implementation SHALL be organized into implementation layers.

| Layer               | Responsibility                                                                  |
| ------------------- | ------------------------------------------------------------------------------- |
| Interface Layer     | APIs, CLIs, UI endpoints, command handlers, external-facing contracts           |
| Application Layer   | Use cases, orchestration flows, service coordination                            |
| Domain Layer        | ISL entities, task graphs, policies, state machines, validation rules           |
| Runtime Layer       | Scheduling, workers, queues, leases, locks, execution graph                     |
| Integration Layer   | Model adapters, tool adapters, repository adapters, identity adapters           |
| Persistence Layer   | State stores, traceability graph stores, artifact metadata stores, audit stores |
| Observability Layer | Telemetry emission, logs, traces, metrics, alerts                               |
| Configuration Layer | Runtime, model, tool, governance, deployment, and environment configuration     |

### 6.1 Layering Rules

Interface components MUST NOT directly mutate persistence stores.

Integration adapters MUST NOT contain domain policy logic except adapter-specific validation.

Runtime components MUST use domain and application contracts to perform lifecycle actions.

Persistence components MUST preserve records defined by ISL v2.2, v2.3, and v1.7.

Observability components MUST observe but not decide lifecycle progression.

Governance decisions MUST be enforced by application and runtime layers before controlled actions proceed.

---

## 7.0 Core Implementation Modules

This section defines the core modules of the reference implementation. Module names are illustrative, but the responsibilities are normative.

| Module                | Responsibility                                                                      |
| --------------------- | ----------------------------------------------------------------------------------- |
| Imhotep.Specification | Parse, validate, and manage authored ISL specifications                             |
| Imhotep.SemanticModel | Normalize specifications into canonical entities and relationships                  |
| Imhotep.Readiness     | Evaluate readiness levels and transition evidence                                   |
| Imhotep.Planning      | Generate and validate construction task graphs                                      |
| Imhotep.Runtime       | Execute construction plans and manage runtime state                                 |
| Imhotep.Agents        | Implement agent role invocation and collaboration contracts                         |
| Imhotep.Models        | Provide model routing, invocation, context control, and output normalization        |
| Imhotep.Tools         | Provide deterministic tool registry, invocation, and result normalization           |
| Imhotep.Repository    | Manage generated artifacts, metadata, branches, packages, and evidence              |
| Imhotep.Traceability  | Maintain traceability graph, snapshots, and impact analysis                         |
| Imhotep.Governance    | Enforce policies, gates, waivers, overrides, risk tiers, and audit                  |
| Imhotep.State         | Manage state records, events, transactions, snapshots, and recovery                 |
| Imhotep.Observability | Emit telemetry, logs, traces, metrics, and alerts                                   |
| Imhotep.Configuration | Manage versioned configuration and environment profiles                             |
| Imhotep.Security      | Provide security utilities, context protection, access control, and secret handling |

### 7.1 Module Rules

Each module MUST expose a defined public interface.

Each module MUST hide internal implementation details behind contracts.

Modules MUST NOT access another module’s persistence tables or files directly unless explicitly authorized by the architecture.

Modules SHOULD be independently testable.

Modules SHOULD support dependency injection or equivalent composition to allow local and enterprise deployment profiles.

---

## 8.0 Reference Services

This section defines the deployable services that may expose or coordinate platform capabilities.

| Service               | Responsibility                                                                         |
| --------------------- | -------------------------------------------------------------------------------------- |
| Execution Service     | Coordinates construction execution and runtime workflows                               |
| Specification Service | Accepts specifications, validates structure, and exposes canonicalization entry points |
| Planning Service      | Produces and validates construction task graphs                                        |
| Model Gateway         | Provides model abstraction, routing, invocation, and output normalization              |
| Tool Gateway          | Provides deterministic tool invocation and result normalization                        |
| Artifact Service      | Manages artifact admission, metadata, promotion, branching, and packaging              |
| Traceability Service  | Manages traceability graph, snapshots, and impact analysis                             |
| Governance Service    | Evaluates policies, gates, approvals, waivers, overrides, and audit events             |
| Telemetry Service     | Collects, processes, and exposes telemetry signals                                     |
| Configuration Service | Serves versioned runtime, model, tool, governance, and deployment configuration        |

### 8.1 Service Deployment Rules

A local-first implementation MAY host all services in one process.

An enterprise implementation SHOULD allow services to be deployed independently.

A service boundary MUST NOT weaken the module boundary beneath it.

Service APIs MUST enforce authentication and authorization when exposed outside a local trusted process.

Service calls that affect lifecycle state MUST be traceable and auditable.

---

## 9.0 Reference Service Topology

The reference service topology defines how services interact during construction.

The Execution Service is the operational coordinator. It consumes an executable construction plan, schedules tasks, invokes agents through the Agent module and Model Gateway, invokes deterministic tools through the Tool Gateway, writes artifacts through the Artifact Service, records traceability through the Traceability Service, consults governance through the Governance Service, and emits telemetry through the Telemetry Service.

### 9.1 Primary Service Interactions

| Source                | Target               | Purpose                                 |
| --------------------- | -------------------- | --------------------------------------- |
| Specification Service | SemanticModel Module | Normalize parsed specifications         |
| Specification Service | Readiness Module     | Evaluate readiness                      |
| Planning Service      | SemanticModel Module | Read canonical graph                    |
| Planning Service      | Traceability Service | Initialize planning traceability        |
| Planning Service      | Governance Service   | Evaluate planning constraints           |
| Execution Service     | Planning Service     | Accept executable plan                  |
| Execution Service     | Governance Service   | Check runtime authorization             |
| Execution Service     | Model Gateway        | Invoke reasoning models                 |
| Execution Service     | Tool Gateway         | Invoke deterministic tools              |
| Execution Service     | Artifact Service     | Admit and update artifacts              |
| Execution Service     | Traceability Service | Record runtime links                    |
| Execution Service     | State Module         | Commit runtime state transitions        |
| Execution Service     | Telemetry Service    | Emit execution telemetry                |
| Artifact Service      | Tool Gateway         | Validate artifact content when required |
| Governance Service    | Telemetry Service    | Emit governance telemetry               |
| Traceability Service  | State Module         | Record snapshots and integrity state    |

### 9.2 Topology Rules

The Execution Service MUST NOT directly invoke external model providers.

The Execution Service MUST NOT directly invoke external deterministic tools.

The Execution Service MUST NOT write stable artifacts without Artifact Service promotion.

The Execution Service MUST NOT bypass Governance Service decisions.

The Artifact Service MUST NOT promote artifacts without validation, traceability, and governance checks.

---

## 10.0 Internal API Categories

The reference implementation MUST define internal APIs or equivalent contracts for major platform operations.

| API Category        | Purpose                                                            |
| ------------------- | ------------------------------------------------------------------ |
| Specification API   | Submit, retrieve, parse, and validate specifications               |
| Canonical Model API | Normalize, retrieve, query, and version canonical models           |
| Readiness API       | Evaluate readiness, transition readiness, record regression        |
| Planning API        | Generate plans, validate plans, adapt plans                        |
| Execution API       | Start, pause, resume, halt, recover, and query execution           |
| Agent API           | Invoke agent roles and validate agent outputs                      |
| Model API           | Route and invoke models through provider abstraction               |
| Tool API            | Select and invoke tools through tool abstraction                   |
| Artifact API        | Admit, update, validate, promote, branch, merge, package artifacts |
| Traceability API    | Create links, snapshots, queries, and impact analysis              |
| Governance API      | Evaluate policies, gates, waivers, overrides, approvals            |
| State API           | Commit transitions, emit state events, create snapshots, recover   |
| Observability API   | Emit telemetry, query telemetry, evaluate alerts                   |
| Configuration API   | Read and update versioned configuration                            |

### 10.1 API Rules

APIs MUST use structured request and response records.

APIs MUST include correlation identifiers where lifecycle state may be affected.

APIs that mutate state MUST return success, failure, and error information in structured form.

APIs that perform governed actions MUST include or request governance checks.

APIs SHOULD be versioned.

---

## 11.0 Reference Persistence Architecture

The reference implementation MUST define persistence boundaries for durable state and memory.

| Store                   | Required Contents                                                  |
| ----------------------- | ------------------------------------------------------------------ |
| Specification Store     | Authored specifications, versions, source metadata                 |
| Canonical Model Store   | Canonical entities, relationships, validation reports              |
| Planning Store          | Construction plans, task graphs, dependencies, validation reports  |
| Runtime State Store     | Execution graphs, task states, work items, leases, locks           |
| Artifact Metadata Store | Artifact records, versions, states, evidence references            |
| Artifact Content Store  | Artifact files, packages, deployment bundles                       |
| Traceability Store      | Traceability graph, snapshots, impact analysis records             |
| Governance Store        | Policies, approvals, waivers, overrides, escalations, audit logs   |
| Model Store             | Provider profiles, routing decisions, invocation records           |
| Tool Store              | Tool records, invocations, results, findings                       |
| Telemetry Store         | Events, metrics, traces, logs, alerts                              |
| Configuration Store     | Versioned platform, runtime, tool, model, governance configuration |

### 11.1 Persistence Rules

Lifecycle-critical records MUST be persisted durably.

Governance audit records MUST be immutable or correction-only.

Traceability records MUST preserve historical relationships across versions.

Artifact metadata MUST remain consistent with artifact content.

Runtime state MUST support recovery after interruption.

Telemetry MUST NOT be treated as authoritative state.

---

## 12.0 Reference Data Ownership

Each implementation module MUST own its data schema or store boundary.

| Data Owner            | Owns                                                             |
| --------------------- | ---------------------------------------------------------------- |
| Specification Module  | Authored specification records and structural validation results |
| Semantic Model Module | Canonical entities and relationship graph                        |
| Planning Module       | Plans, task graphs, planning validation reports                  |
| Runtime Module        | Execution graph, work items, leases, locks, runtime errors       |
| Agent Module          | Agent invocations, context packages, outputs, handoffs           |
| Model Module          | Model profiles, routing decisions, invocation records            |
| Tool Module           | Tool registry, invocations, normalized results, findings         |
| Artifact Module       | Artifact metadata, repository operations, packages               |
| Traceability Module   | Traceability graph and snapshots                                 |
| Governance Module     | Governance state and audit records                               |
| State Module          | State events, transactions, snapshots, recovery records          |
| Observability Module  | Telemetry records and alert records                              |
| Configuration Module  | Versioned configuration records                                  |

### 12.1 Data Ownership Rules

A module MUST NOT modify another module’s owned records directly.

Cross-module updates MUST occur through API calls, events, or defined transactions.

State consistency across modules MUST be enforced through the State Module or equivalent transaction mechanism.

---

## 13.0 Runtime Implementation Architecture

The runtime implementation MUST provide task execution according to ISL v2.4.

### 13.1 Runtime Components

| Component                    | Responsibility                                                                          |
| ---------------------------- | --------------------------------------------------------------------------------------- |
| Runtime Admission Controller | Validates execution admission                                                           |
| Scheduler                    | Evaluates task eligibility and queues work                                              |
| Queue Manager                | Maintains ready, delayed, retry, repair, validation, governance, and dead-letter queues |
| Worker Manager               | Registers workers and monitors heartbeats                                               |
| Lease Manager                | Manages task and work item leases                                                       |
| Lock Manager                 | Manages artifact, path, branch, package, state, and governance locks                    |
| Execution Graph Manager      | Maintains runtime graph state                                                           |
| Task Executor                | Executes work item lifecycle                                                            |
| Retry Manager                | Applies retry policies                                                                  |
| Repair Coordinator           | Coordinates repair cycles                                                               |
| Checkpoint Manager           | Creates recovery checkpoints                                                            |
| Recovery Manager             | Performs recovery after interruption                                                    |
| Runtime Error Manager        | Records runtime errors and handling status                                              |

### 13.2 Runtime Rules

Runtime components MUST use durable state for execution progress.

Workers MUST NOT execute tasks without leases.

Artifact-modifying tasks MUST acquire locks.

Runtime errors MUST be recorded using the error taxonomy defined in ISL v2.4.

Runtime recovery MUST validate state, repository, traceability, and governance consistency before resuming execution.

---

## 14.0 Agent Implementation Architecture

The Agent module implements the role contracts defined in ISL v2.1.

### 14.1 Agent Components

| Component                     | Responsibility                                   |
| ----------------------------- | ------------------------------------------------ |
| Agent Role Registry           | Registers supported agent roles                  |
| Agent Invocation Manager      | Creates and tracks agent invocations             |
| Context Package Builder       | Assembles task-scoped context                    |
| Agent Contract Validator      | Validates role inputs and outputs                |
| Agent Output Normalizer       | Converts model outputs into agent output records |
| Collaboration Session Manager | Manages multi-agent collaboration sessions       |
| Handoff Manager               | Records and routes handoffs                      |
| Agent Failure Manager         | Classifies and handles agent failures            |

### 14.2 Agent Rules

Agents MUST be invoked by role.

Agents MUST receive structured context packages.

Agents MUST produce structured outputs.

Agent outputs MUST be validated before downstream use.

Agents MUST NOT directly mutate platform state, stable artifacts, governance records, or traceability records.

---

## 15.0 Model Gateway Reference Architecture

The Model Gateway implements the model abstraction defined in ISL v2.5.

### 15.1 Model Gateway Components

| Component                | Responsibility                                           |
| ------------------------ | -------------------------------------------------------- |
| Provider Registry        | Stores model provider records                            |
| Model Profile Registry   | Stores model profiles and capabilities                   |
| Routing Policy Engine    | Selects models for invocations                           |
| Context Controller       | Applies context minimization, sensitivity, and redaction |
| Provider Adapter Host    | Hosts provider-specific adapters                         |
| Invocation Manager       | Executes model invocations                               |
| Output Normalizer        | Normalizes provider outputs                              |
| Fallback Manager         | Selects fallback models when permitted                   |
| Model Telemetry Emitter  | Emits model telemetry                                    |
| Model Governance Adapter | Requests governance checks for restricted model use      |

### 15.2 Model Gateway Rules

The gateway MUST reject unregistered providers.

The gateway MUST reject models lacking required capabilities.

The gateway MUST enforce trust profiles and context sensitivity rules.

The gateway MUST record routing decisions.

The gateway MUST normalize outputs before returning them to agent components.

---

## 16.0 Tool Gateway Reference Architecture

The Tool Gateway implements deterministic tool integration as defined in ISL v1.6.

### 16.1 Tool Gateway Components

| Component              | Responsibility                                               |
| ---------------------- | ------------------------------------------------------------ |
| Tool Registry          | Stores tool records and capability declarations              |
| Tool Selector          | Selects tools by capability and governance constraints       |
| Tool Adapter Host      | Hosts CLI, container, service, plugin, or API adapters       |
| Sandbox Manager        | Applies tool execution isolation                             |
| Invocation Manager     | Executes tool invocations                                    |
| Result Normalizer      | Normalizes tool outputs                                      |
| Finding Mapper         | Maps findings to artifacts, validations, policies, and tasks |
| Evidence Manager       | Preserves tool evidence                                      |
| Tool Telemetry Emitter | Emits tool telemetry                                         |

### 16.2 Tool Gateway Rules

The gateway MUST reject unregistered tools.

The gateway MUST enforce tool trust profiles.

The gateway MUST not treat tool timeout or execution error as success.

The gateway MUST normalize tool results before returning them to runtime.

The gateway MUST preserve evidence where required by governance or validation policy.

---

## 17.0 Artifact Repository Implementation Architecture

The Artifact module implements the artifact repository model defined in ISL v2.3.

### 17.1 Artifact Components

| Component                     | Responsibility                                  |
| ----------------------------- | ----------------------------------------------- |
| Artifact Admission Controller | Accepts or rejects candidate artifacts          |
| Artifact Metadata Manager     | Maintains artifact metadata and lifecycle state |
| Content Store Adapter         | Stores artifact content                         |
| Branch Manager                | Manages branch/workspace isolation              |
| Merge Manager                 | Detects and resolves merge conflicts            |
| Promotion Manager             | Promotes artifacts to stable state              |
| Evidence Manager              | Associates validation evidence with artifacts   |
| Package Manager               | Creates package manifests and bundles           |
| Repository Integrity Checker  | Detects repository inconsistencies              |
| Retention Manager             | Archives or deletes artifacts under policy      |

### 17.2 Artifact Rules

All generated artifacts MUST pass through artifact admission.

Stable artifact promotion MUST require validation, traceability, and governance eligibility.

Artifact metadata MUST be authoritative for artifact lifecycle state.

Repository content and metadata MUST be checked for consistency.

---

## 18.0 Traceability Implementation Architecture

The Traceability module implements ISL v1.4 and the traceability portions of v2.x.

### 18.1 Traceability Components

| Component                  | Responsibility                                      |
| -------------------------- | --------------------------------------------------- |
| Traceability Graph Manager | Maintains nodes and edges                           |
| Link Writer                | Creates lifecycle traceability links                |
| Snapshot Manager           | Creates graph snapshots                             |
| Integrity Checker          | Validates traceability completeness and consistency |
| Impact Analyzer            | Identifies affected artifacts, tasks, and records   |
| Query Engine               | Supports bidirectional traceability queries         |
| Provenance Exporter        | Exports provenance-aligned records where required   |

### 18.2 Traceability Rules

Traceability links MUST be written during lifecycle actions.

Untraced stable artifacts MUST be rejected or escalated.

Traceability integrity failure MUST block execution completion and deployment preparation where required.

---

## 19.0 Governance Implementation Architecture

The Governance module implements ISL v1.7.

### 19.1 Governance Components

| Component                        | Responsibility                             |
| -------------------------------- | ------------------------------------------ |
| Policy Engine                    | Evaluates policies and violation responses |
| Gate Manager                     | Manages approval gates                     |
| Role Manager                     | Manages governance role assignments        |
| Separation Validator             | Enforces separation of duties              |
| Waiver Manager                   | Manages waivers                            |
| Override Manager                 | Manages overrides                          |
| Escalation Manager               | Opens and resolves escalations             |
| Risk Tier Manager                | Assigns and evaluates risk tiers           |
| Deployment Authorization Manager | Manages deployment authorization           |
| Audit Writer                     | Writes immutable governance audit events   |

### 19.2 Governance Rules

Governance decisions MUST be explicit.

Governance audit records MUST be immutable or correction-only.

Expired approvals, waivers, and overrides MUST NOT authorize runtime actions.

Governance audit write failure MUST block governed actions.

---

## 20.0 State and Memory Implementation Architecture

The State module implements ISL v2.2.

### 20.1 State Components

| Component                | Responsibility                                |
| ------------------------ | --------------------------------------------- |
| State Record Manager     | Maintains current state records               |
| State Event Writer       | Emits durable state events                    |
| Transaction Manager      | Coordinates multi-state updates               |
| Snapshot Manager         | Creates state snapshots                       |
| Checkpoint Manager       | Creates runtime checkpoints                   |
| Consistency Checker      | Validates cross-state invariants              |
| Recovery Manager         | Restores consistent state after interruption  |
| Memory Access Controller | Controls agent and subsystem access to memory |
| Retention Manager        | Enforces retention and archival rules         |

### 20.2 State Rules

Lifecycle state MUST be explicit.

State transitions MUST follow defined state machines.

Multi-record updates MUST be atomic or recoverable.

Recovery MUST validate consistency before execution resumes.

---

## 21.0 Observability Implementation Architecture

The Observability module implements ISL v2.6.

### 21.1 Observability Components

| Component           | Responsibility                                        |
| ------------------- | ----------------------------------------------------- |
| Telemetry Emitter   | Emits structured events, logs, metrics, and traces    |
| Telemetry Collector | Receives telemetry                                    |
| Telemetry Processor | Redacts, enriches, filters, and validates telemetry   |
| Metrics Engine      | Aggregates metrics                                    |
| Trace Engine        | Correlates traces and spans                           |
| Alert Engine        | Evaluates alert rules                                 |
| Dashboard API       | Exposes dashboard queries                             |
| Exporter            | Exports telemetry to external observability platforms |

### 21.2 Observability Rules

Telemetry MUST include correlation identifiers.

Telemetry MUST NOT replace audit or traceability records.

Sensitive telemetry MUST be redacted or access-controlled.

Telemetry export MUST obey governance and privacy rules.

---

## 22.0 Configuration Implementation Architecture

The Configuration module manages versioned platform configuration.

### 22.1 Configuration Domains

| Domain        | Examples                                                    |
| ------------- | ----------------------------------------------------------- |
| runtime       | concurrency, retry, timeout, queue, worker settings         |
| model         | providers, routing policies, model profiles, trust profiles |
| tool          | tool records, sandbox profiles, capability mappings         |
| governance    | policies, roles, approval rules, risk tiers                 |
| repository    | branch rules, path rules, promotion rules                   |
| observability | telemetry profiles, alert rules, export settings            |
| security      | secret profiles, access policies, context controls          |
| deployment    | local, team, enterprise, distributed profile settings       |

### 22.2 Configuration Rules

Configuration MUST be versioned.

Configuration active during execution MUST be recorded.

Configuration changes affecting execution behavior MUST trigger impact evaluation.

Secrets MUST NOT be stored in plain configuration records.

---

## 23.0 Security Implementation Architecture

Security must be embedded across the reference implementation.

### 23.1 Security Components

| Component                  | Responsibility                                          |
| -------------------------- | ------------------------------------------------------- |
| Identity Adapter           | Integrates user and service identity                    |
| Authorization Manager      | Enforces role, project, tenant, and environment access  |
| Secret Manager Adapter     | Retrieves and scopes secrets                            |
| Context Protection Service | Prevents unauthorized context exposure                  |
| Sandbox Policy Manager     | Controls tool and generated-code execution environments |
| Security Finding Manager   | Records security-relevant findings                      |
| Audit Security Adapter     | Supports security-relevant audit records                |

### 23.2 Security Rules

Service-to-service calls SHOULD authenticate in enterprise deployments.

Secrets MUST NOT be exposed to agent context unless explicitly governed.

Tools executing generated code SHOULD run in sandboxed environments.

Model context transmission MUST obey sensitivity and trust profile rules.

Security boundary violations MUST halt or isolate affected execution.

---

## 24.0 Reference Implementation Modes

The reference implementation MUST support progressive deployment modes.

| Mode                | Description                                                                     |
| ------------------- | ------------------------------------------------------------------------------- |
| local-library       | All modules invoked in-process by tests, CLI, or local host                     |
| local-service       | Core services run locally as one process or small service set                   |
| containerized-local | Services run locally in containers                                              |
| team-service        | Shared services support multiple users or projects                              |
| distributed-workers | Runtime workers scale independently                                             |
| enterprise-services | Services integrate with enterprise identity, telemetry, storage, and governance |
| regulated-profile   | Deployment applies high-assurance audit, security, and retention controls       |

### 24.1 Mode Rules

Each mode MUST preserve logical boundaries.

Moving from one mode to another MUST NOT require rewriting domain contracts.

Distributed modes MUST use durable queues, leases, locks, and state stores.

Enterprise and regulated modes MUST enforce identity, governance, audit, traceability, and retention controls.

---

## 25.0 Reference Implementation Workflow

The reference implementation MUST support the following workflow.

1. Receive authored specification.
2. Parse and structurally validate specification.
3. Normalize into canonical semantic model.
4. Evaluate readiness.
5. Generate construction task graph.
6. Validate construction plan.
7. Record planning traceability.
8. Perform governance execution admission check.
9. Initialize execution graph.
10. Schedule eligible tasks.
11. Invoke agents through agent and model contracts.
12. Admit generated artifacts into repository.
13. Invoke deterministic tools for validation.
14. Record validation results.
15. Trigger bounded repair when validation fails.
16. Promote valid and traced artifacts to stable state.
17. Create traceability snapshots.
18. Produce telemetry and audit records.
19. Prepare deployment artifacts when required.
20. Produce completion report.

### 25.1 Workflow Rules

The workflow MUST stop or pause when readiness, governance, validation, traceability, or state consistency conditions fail.

The workflow MUST preserve evidence for every lifecycle decision.

The workflow MUST be recoverable after interruption.

---

## 26.0 Reference Implementation Contracts

This section defines required contract categories.

| Contract                   | Required Fields or Basis                              |
| -------------------------- | ----------------------------------------------------- |
| SpecificationParseRequest  | specification source, format, version, correlation-id |
| SpecificationParseResult   | parsed structure, findings, validation outcome        |
| CanonicalNormalizeRequest  | parsed specification, ISL version, extensions         |
| CanonicalNormalizeResult   | canonical model, validation report, source map        |
| ReadinessEvaluationRequest | specification version, canonical model, evidence      |
| ReadinessEvaluationResult  | readiness level, findings, transition eligibility     |
| PlanGenerationRequest      | canonical model, readiness state, governance state    |
| PlanGenerationResult       | construction plan, task graph, validation report      |
| ExecutionStartRequest      | plan-id, readiness state, governance state            |
| ExecutionStartResult       | execution-graph-id, admission outcome                 |
| AgentInvocationRequest     | role, task-id, context package, output contract       |
| AgentInvocationResult      | output, validation status, findings                   |
| ModelInvocationRequest     | model profile, context package, output contract       |
| ModelInvocationResult      | normalized output, outcome, findings                  |
| ToolInvocationRequest      | tool-id, capability, artifacts, parameters            |
| ToolInvocationResult       | outcome, findings, evidence                           |
| ArtifactAdmissionRequest   | artifact candidate, metadata, source task             |
| ArtifactAdmissionResult    | artifact-id, state, findings                          |
| GovernanceCheckRequest     | target, action, policy context                        |
| GovernanceCheckResult      | allow, block, warn, escalate, approval-required       |
| TraceabilityLinkRequest    | source, target, relationship type                     |
| StateTransitionRequest     | object, prior state, new state, reason                |
| TelemetryEvent             | event name, category, timestamp, correlation-id       |

### 26.1 Contract Rules

Contracts MUST be versioned.

Contracts MUST include correlation identifiers when used in lifecycle operations.

Contracts MUST use structured error responses.

Contracts SHOULD be serializable.

Contracts MUST NOT depend on provider-specific model or tool response formats.

---

## 27.0 Error Handling Architecture

The reference implementation MUST implement error handling across modules.

### 27.1 Error Handling Components

| Component          | Responsibility                                               |
| ------------------ | ------------------------------------------------------------ |
| Error Classifier   | Maps failures to defined error classes                       |
| Error Recorder     | Persists error records                                       |
| Error Router       | Routes errors to retry, repair, escalation, halt, or failure |
| Retry Coordinator  | Applies retry policy                                         |
| Escalation Adapter | Opens governance or operational escalation                   |
| Recovery Adapter   | Initiates recovery where required                            |

### 27.2 Error Handling Rules

Runtime errors MUST follow ISL v2.4 taxonomy.

Model errors MUST follow ISL v2.5 taxonomy.

Tool errors MUST follow ISL v1.6 taxonomy.

Repository errors MUST follow ISL v2.3 taxonomy.

Governance errors MUST follow ISL v1.7 taxonomy.

Unhandled exceptions MUST be converted into structured error records before affecting lifecycle state.

---

## 28.0 Testing Architecture

The reference implementation MUST be testable at multiple levels.

### 28.1 Required Test Categories

| Test Category | Purpose                                                                  |
| ------------- | ------------------------------------------------------------------------ |
| unit          | Validate individual modules and domain rules                             |
| contract      | Validate API and record schemas                                          |
| integration   | Validate interactions between modules                                    |
| runtime       | Validate scheduling, queues, workers, retries, locks, recovery           |
| agent         | Validate agent role contracts and output validation                      |
| model         | Validate routing, context control, normalization, fallback               |
| tool          | Validate tool adapters and result normalization                          |
| repository    | Validate artifact admission, versioning, branching, promotion            |
| traceability  | Validate graph links, snapshots, impact analysis                         |
| governance    | Validate policy checks, gates, waivers, overrides, audit                 |
| security      | Validate access control, secret handling, sandboxing, context protection |
| observability | Validate telemetry correlation and alerting                              |
| scenario      | Validate end-to-end construction workflows                               |

### 28.2 Testing Rules

Every module SHOULD include unit tests.

Every contract MUST have schema or compatibility tests.

Every external adapter SHOULD have mock or simulator tests.

Runtime recovery MUST have failure-injection tests.

The proof-of-concept pipeline MUST have scenario tests.

---

## 29.0 Reference Implementation Validation

The reference implementation MUST include validation criteria that prove architectural conformance.

### 29.1 Validation Criteria

A reference implementation is valid when it can demonstrate:

* specification parsing
* canonical normalization
* readiness evaluation
* construction planning
* execution admission
* task scheduling
* agent invocation
* model routing and output normalization
* artifact admission
* deterministic validation
* bounded repair
* traceability link creation
* governance check enforcement
* telemetry emission
* state persistence
* recovery after interruption
* artifact promotion
* completion reporting

### 29.2 Validation Rules

Validation evidence MUST be recorded.

Scenario validation SHOULD use a minimal system before expanding to complex systems.

A failed validation criterion MUST produce an implementation conformance finding.

---

## 30.0 Implementation Extension Model

The reference implementation MAY be extended.

### 30.1 Extension Points

| Extension Point             | Examples                               |
| --------------------------- | -------------------------------------- |
| specification parser        | new representation formats             |
| canonical entity extensions | domain-specific entities               |
| planning strategy           | alternative task generation algorithms |
| agent role                  | specialized agents                     |
| model provider              | new model adapters                     |
| tool provider               | new deterministic tool adapters        |
| artifact repository         | new storage backends                   |
| governance policy           | new policy engines                     |
| telemetry exporter          | new observability systems              |
| deployment profile          | new runtime environments               |

### 30.2 Extension Rules

Extensions MUST register their capabilities.

Extensions MUST NOT bypass governance, traceability, state, validation, or artifact controls.

Extensions MUST declare supported ISL versions.

Extensions used in High or Critical risk systems SHOULD require governance approval.

---

## 31.0 Reference Implementation Security Requirements

The reference implementation MUST implement minimum security controls.

### 31.1 Minimum Requirements

The implementation MUST support:

* authenticated access for external APIs
* service identity for inter-service calls in distributed modes
* authorization checks for governed operations
* secret isolation
* model context control
* tool sandboxing
* artifact repository access control
* immutable or correction-only audit records
* restricted telemetry redaction
* security error recording

### 31.2 Security Rules

Secrets MUST NOT be written to logs, telemetry, specifications, or agent context packages.

Generated code execution SHOULD occur in a sandbox.

External model and tool integrations MUST be registered and governed.

Repository promotion and deployment preparation MUST be access-controlled.

---

## 32.0 Reference Implementation Observability Requirements

The implementation MUST provide observability hooks.

### 32.1 Required Observability

The implementation SHOULD emit telemetry for:

* specification parsing
* canonical normalization
* readiness evaluation
* plan generation
* execution admission
* task scheduling
* agent invocation
* model invocation
* tool invocation
* artifact admission
* validation result
* repair cycle
* governance check
* traceability link creation
* state transition
* checkpoint creation
* recovery action
* completion report

### 32.2 Observability Rules

Telemetry MUST include correlation identifiers.

Telemetry MUST distinguish operational telemetry from audit records.

Telemetry MUST not expose restricted data without authorization.

---

## 33.0 Reference Implementation Documentation

The reference implementation MUST include documentation sufficient for development, operation, and conformance review.

### 33.1 Required Documentation

| Document                 | Purpose                                                          |
| ------------------------ | ---------------------------------------------------------------- |
| Architecture Overview    | Explains modules, services, and boundaries                       |
| Module Contracts         | Defines public interfaces and data contracts                     |
| Deployment Guide         | Explains local and enterprise deployment modes                   |
| Configuration Guide      | Explains configuration domains and profiles                      |
| Adapter Guide            | Explains model and tool adapter implementation                   |
| Runtime Operations Guide | Explains scheduling, workers, recovery, and errors               |
| Governance Guide         | Explains policy, approval, waiver, override behavior             |
| Traceability Guide       | Explains graph structure and queries                             |
| Repository Guide         | Explains artifact lifecycle and branching                        |
| Testing Guide            | Explains test strategy and scenario validation                   |
| Security Guide           | Explains access control, secrets, sandboxing, context protection |

### 33.2 Documentation Rules

Documentation MUST be versioned with the implementation.

Documentation MUST identify the ISL versions implemented.

Documentation SHOULD include examples.

---

## 34.0 Implementation Conformance Requirements

### 34.1 Module Conformance

A module conforms to ISL v3.0 if it:

* implements a defined responsibility
* exposes structured contracts
* preserves logical boundaries
* records required state and telemetry
* enforces applicable governance, validation, traceability, and security rules
* is testable through unit or contract tests

### 34.2 Service Conformance

A service conforms to ISL v3.0 if it:

* exposes defined service responsibilities
* uses structured APIs
* enforces access control where required
* records lifecycle actions
* emits telemetry
* preserves correlation identifiers
* does not bypass lower-level module contracts

### 34.3 Persistence Conformance

A persistence implementation conforms to ISL v3.0 if it:

* stores lifecycle-critical records durably
* preserves version context
* supports state recovery
* supports traceability and governance retention
* protects audit records
* maintains artifact metadata and content consistency

### 34.4 Reference Implementation Conformance

A reference implementation conforms to ISL v3.0 if it:

* implements the required core modules or equivalent responsibilities
* supports local-first execution
* preserves logical subsystem boundaries
* supports model and tool gateways
* supports execution runtime behavior
* supports artifact repository lifecycle
* supports traceability graph recording
* supports governance enforcement
* supports state persistence and recovery
* supports observability and telemetry
* supports structured contracts
* passes reference implementation validation criteria

---

## 35.0 Summary

ISL v3.0 defines the Reference Implementation Architecture for the IMHOTEP platform. It translates the platform architecture from the v2.x corpus into implementation-level modules, services, APIs, stores, workers, gateways, contracts, and validation expectations.

This revision strengthens the original reference implementation concept by making implementation boundaries explicit. It defines the module architecture, service topology, persistence boundaries, runtime components, agent/model/tool gateway structure, artifact repository implementation, traceability implementation, governance implementation, state and memory implementation, observability implementation, configuration model, security requirements, testing architecture, extension points, and conformance requirements.

A conforming IMHOTEP reference implementation MUST be more than a demonstration. It MUST be a controlled, modular, testable, traceable, governed, validation-centered architecture that can begin locally and mature into enterprise deployment without violating the core ISL architecture.
