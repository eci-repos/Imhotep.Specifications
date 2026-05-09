# IMHOTEP Specification Language (ISL) v2.0

# The IMHOTEP Platform Architecture Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7
**Supersedes:** ISL r0 v2.0 Platform Architecture Model
**Document Type:** Platform Architecture Specification

---

## 1.0 Scope

This document defines the architecture of the IMHOTEP platform: the system responsible for interpreting ISL specifications, normalizing them into canonical semantic models, generating construction plans, orchestrating agents, invoking deterministic tools, managing artifacts, preserving traceability, enforcing governance, and supporting autonomous software construction.

This document applies to conforming implementations of the IMHOTEP platform. It defines required platform subsystems, subsystem responsibilities, architectural boundaries, data flow rules, control points, integration contracts, runtime coordination responsibilities, and conformance expectations. It does not prescribe a specific programming language, cloud provider, database, model provider, operating system, or deployment topology. Those are implementation choices, provided the resulting platform satisfies this architecture.

This document defines:

* platform architecture principles
* core platform subsystems
* subsystem responsibility boundaries
* subsystem interface expectations
* platform data flows
* governance and traceability integration
* tool and model abstraction boundaries
* artifact repository responsibilities
* observability and monitoring expectations
* local-first and enterprise scalability requirements
* conformance requirements

---

## 2.0 Purpose of the Platform Architecture Model

The purpose of the Platform Architecture Model is to define the structural organization of the IMHOTEP platform itself. Earlier ISL documents define how systems are specified, validated, planned, traced, governed, and executed. This document defines the architecture of the platform that performs those activities.

The IMHOTEP platform is not merely an authoring tool, a code generator, or an agent orchestration framework. It is an autonomous construction environment that coordinates formal specifications, canonical semantic models, task graphs, reasoning agents, deterministic tools, artifact repositories, governance services, and observability systems. Its architecture must therefore provide strong subsystem boundaries and explicit control paths.

This architecture exists to ensure that:

* ISL specifications are processed consistently
* semantic interpretation is separated from execution
* planning is traceable to canonical specification entities
* agents operate under runtime and governance control
* generated artifacts are validated deterministically
* repair cycles are bounded and auditable
* governance is enforced continuously
* traceability is preserved across the lifecycle
* platform behavior can be observed, audited, and scaled

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the platform architecture. The platform architecture depends on all v1.x foundation models because those documents define the language, semantic model, execution lifecycle, readiness gates, traceability, planning, tools, and governance controls the platform must implement.

| Reference     | Purpose                                                         |
| ------------- | --------------------------------------------------------------- |
| ISL v0.0      | Corpus-level principles, conformance, and architecture layering |
| ISL v1.0      | Authored specification language                                 |
| ISL v1.1      | Canonical semantic model                                        |
| ISL v1.2      | Execution model                                                 |
| ISL v1.3      | Specification readiness levels                                  |
| ISL v1.4      | Traceability model                                              |
| ISL v1.5      | Construction planning model                                     |
| ISL v1.6      | Tool integration model                                          |
| ISL v1.7      | Governance and control model                                    |
| RFC 2119      | Normative terminology                                           |
| W3C PROV-DM   | Provenance alignment for traceability                           |
| OpenTelemetry | Observability and telemetry alignment                           |

---

## 4.0 Terms and Definitions

This section defines platform architecture terms used throughout this document. These terms describe architectural components and boundaries rather than specification-language concepts.

**IMHOTEP Platform**
The integrated system that processes ISL specifications and performs governed autonomous software construction.

**Platform Subsystem**
A logical component responsible for one bounded area of platform functionality.

**Specification Engine**
The subsystem that receives, parses, validates, and manages authored ISL specifications.

**Semantic Model Engine**
The subsystem that normalizes authored specifications into canonical semantic models and manages the canonical graph.

**Planning Engine**
The subsystem that transforms the canonical model into a construction task graph.

**Agent Orchestrator**
The subsystem that coordinates reasoning agents used for interpretation, planning, generation, review, repair, testing, security validation, and deployment preparation.

**Construction Engine**
The subsystem that executes construction tasks and coordinates artifact generation, validation, repair, and completion.

**Tool Integration Layer**
The subsystem that registers, selects, invokes, and normalizes deterministic engineering tools.

**Traceability Engine**
The subsystem that maintains traceability relationships across specifications, tasks, artifacts, validations, repairs, and governance events.

**Governance Engine**
The subsystem that evaluates policies, manages approval gates, enforces runtime governance decisions, and records governance events.

**Execution Monitor**
The subsystem that observes platform execution, captures telemetry, and exposes operational state.

**Artifact Repository**
The subsystem that stores generated artifacts, metadata, versions, validation evidence, and repository state.

---

## 5.0 Platform Architecture Design Principles

This section defines the design principles that govern the IMHOTEP platform architecture. These principles ensure the platform remains modular, auditable, secure, scalable, and adaptable.

### 5.1 Layered Architecture

The platform MUST be organized as a layered architecture with clear responsibility boundaries. Each subsystem MUST perform a defined architectural role and MUST NOT bypass other subsystems’ control responsibilities.

Layering ensures that specification processing, semantic normalization, planning, execution, validation, governance, traceability, and artifact storage remain understandable and independently evolvable.

### 5.2 Specification-Driven Operation

The platform MUST operate from ISL specifications and their canonical semantic models. Construction activity MUST NOT be initiated from informal prompts, unnormalized prose, or ungoverned instructions.

The specification is the source of truth. The platform’s architecture MUST preserve this principle at every layer.

### 5.3 Local-First Capability

The platform MUST be capable of operating in a local-first mode using local infrastructure, local artifact repositories, local tools, and locally hosted or locally available model providers where configured.

Local-first operation supports privacy, restricted environments, offline development, enterprise experimentation, and operational independence.

### 5.4 Abstraction of External Dependencies

External model providers, deterministic tools, artifact repositories, identity systems, observability systems, and deployment environments MUST be integrated through abstraction layers.

The platform MUST NOT hardcode dependency-specific behavior into core orchestration or construction logic where an abstraction boundary is required.

### 5.5 Governed Autonomy

The platform MUST treat governance as an active control layer. Governance decisions MUST influence readiness, planning, execution, tool usage, repair, deployment preparation, and artifact acceptance.

A subsystem MUST NOT continue a governed action when the Governance Engine returns block, escalation, approval-required, waiver-required, or override-required.

### 5.6 Traceability by Construction

The platform MUST record traceability as actions occur. Traceability MUST NOT be treated as an optional report produced after execution.

Every construction task, generated artifact, validation result, repair action, governance event, and deployment preparation output MUST be traceable according to ISL v1.4.

### 5.7 Deterministic Validation Before Trust

Generated artifacts MUST NOT be accepted as valid until required deterministic validation has passed or a governance-approved waiver exists.

The platform architecture MUST ensure that reasoning outputs pass through validation and governance boundaries before acceptance.

### 5.8 Observability and Auditability

The platform MUST expose sufficient observability and audit records to explain what the system did, why it did it, what evidence supports the result, and which governance decisions were applied.

Operational transparency is required for enterprise adoption and trust.

---

## 6.0 Platform Architecture Overview

This section provides the high-level architecture of the IMHOTEP platform. The platform consists of cooperating subsystems that together implement the autonomous construction lifecycle.

Each subsystem has a bounded responsibility. Subsystems may be implemented as separate services, libraries, modules, processes, containers, or distributed components. The implementation form is not prescribed, but the logical architecture and control responsibilities are normative.

### 6.1 Core Platform Subsystems

The platform SHALL include the following core subsystems:

| Subsystem              | Primary Responsibility                                                               |
| ---------------------- | ------------------------------------------------------------------------------------ |
| Specification Engine   | Intake, parse, validate, and manage authored specifications                          |
| Semantic Model Engine  | Normalize and manage canonical semantic models                                       |
| Planning Engine        | Generate and validate construction task graphs                                       |
| Agent Orchestrator     | Coordinate reasoning agents and model-backed reasoning tasks                         |
| Construction Engine    | Execute construction tasks and manage generation, validation, repair, and completion |
| Tool Integration Layer | Register, select, invoke, and normalize deterministic tools                          |
| Traceability Engine    | Maintain lifecycle traceability graph                                                |
| Governance Engine      | Enforce policies, approval gates, waivers, overrides, and runtime decisions          |
| Execution Monitor      | Observe execution, collect telemetry, expose operational state                       |
| Artifact Repository    | Store generated artifacts, metadata, versions, and evidence                          |

### 6.2 Subsystem Interaction Summary

At a high level, the platform operates as follows:

1. The Specification Engine receives an authored ISL specification.
2. The Semantic Model Engine normalizes the specification into the canonical model.
3. The Readiness process evaluates whether the specification may progress.
4. The Planning Engine generates a construction task graph.
5. The Governance Engine evaluates approval and policy constraints.
6. The Construction Engine executes the task graph.
7. The Agent Orchestrator invokes reasoning agents when reasoning or generation is required.
8. The Tool Integration Layer invokes deterministic tools for validation.
9. The Artifact Repository stores generated outputs and evidence.
10. The Traceability Engine records relationships across all lifecycle objects.
11. The Execution Monitor records telemetry and operational state.

### 6.3 Architecture Rule

A conforming platform MUST preserve these logical responsibilities even if implementation packaging combines multiple subsystems into a single process or service.

---

## 7.0 Platform Layers

This section defines the major architectural layers of the platform. Layering clarifies which responsibilities belong together and where control boundaries must be enforced.

### 7.1 Layer Overview

| Layer               | Subsystems                                                                                        |
| ------------------- | ------------------------------------------------------------------------------------------------- |
| Specification Layer | Specification Engine, Semantic Model Engine                                                       |
| Planning Layer      | Planning Engine, Traceability Engine                                                              |
| Execution Layer     | Construction Engine, Agent Orchestrator, Tool Integration Layer                                   |
| Control Layer       | Governance Engine, Execution Monitor                                                              |
| Artifact Layer      | Artifact Repository, evidence stores, versioned outputs                                           |
| Integration Layer   | Model providers, deterministic tools, identity systems, observability systems, deployment targets |

### 7.2 Layer Rules

The Specification Layer MUST produce validated semantic inputs before formal planning.

The Planning Layer MUST produce valid construction plans before execution.

The Execution Layer MUST operate under governance and traceability control.

The Control Layer MUST be able to observe and influence readiness, planning, execution, repair, and deployment preparation.

The Artifact Layer MUST preserve generated outputs, validation evidence, metadata, and lifecycle history.

The Integration Layer MUST be accessed through approved abstraction interfaces.

---

## 8.0 Specification Engine

This section defines the Specification Engine. The Specification Engine is the entry point through which authored ISL specifications enter the platform.

The Specification Engine protects the platform from malformed, incomplete, unsupported, or unstructured specifications. It converts human-authored documents into structured input suitable for canonical normalization.

### 8.1 Responsibilities

The Specification Engine MUST:

* accept authored ISL specifications
* validate document structure according to ISL v1.0
* verify required sections and required fields
* detect language-level errors
* identify declared ISL version
* extract structured elements for normalization
* preserve source location metadata
* produce structural validation reports
* route valid structured input to the Semantic Model Engine

### 8.2 Inputs

| Input                  | Description                                |
| ---------------------- | ------------------------------------------ |
| Authored ISL Markdown  | Human-authored specification               |
| Specification Metadata | Version, owner, status, ISL version        |
| Extension Declarations | Declared language extensions               |
| Authoring Context      | Optional context from editor or repository |

### 8.3 Outputs

| Output                       | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| Parsed Specification         | Structured representation of authored document    |
| Structural Validation Report | Language-level validation results                 |
| Source Location Map          | Mapping from extracted content to authored source |
| Normalization Input Package  | Input package for Semantic Model Engine           |

### 8.4 Rules

The Specification Engine MUST reject specifications with blocking ISL v1.0 structural errors when formal validation is requested.

The Specification Engine MUST preserve source location information sufficient to report validation failures to authors.

The Specification Engine MUST NOT perform autonomous construction or artifact generation.

---

## 9.0 Semantic Model Engine

This section defines the Semantic Model Engine. This subsystem converts structured specification input into the canonical semantic model defined by ISL v1.1.

The Semantic Model Engine is responsible for producing the authoritative machine representation used by readiness evaluation, planning, governance, traceability, and execution.

### 9.1 Responsibilities

The Semantic Model Engine MUST:

* normalize parsed specification content into canonical entities
* assign or validate canonical identifiers
* construct canonical relationships
* resolve references
* validate entity schemas
* validate relationship constraints
* detect semantic ambiguity
* produce canonical validation reports
* maintain canonical model versions
* expose canonical graph queries to authorized subsystems

### 9.2 Inputs

| Input                       | Description                                                |
| --------------------------- | ---------------------------------------------------------- |
| Normalization Input Package | Structured specification content from Specification Engine |
| ISL v1.1 Schema Rules       | Canonical entity and relationship rules                    |
| Extension Schemas           | Approved extension schemas                                 |
| Prior Canonical Model       | Required for version comparison or change analysis         |

### 9.3 Outputs

| Output                      | Description                                    |
| --------------------------- | ---------------------------------------------- |
| Canonical Model             | Normalized specification graph                 |
| Canonical Validation Report | Semantic validation results                    |
| Reference Resolution Report | Identifier and relationship resolution results |
| Model Version Record        | Canonical model version metadata               |

### 9.4 Rules

The Semantic Model Engine MUST produce exactly one Project entity for each canonical model.

The Semantic Model Engine MUST reject canonical models with blocking semantic errors before Machine-Valid readiness.

The Semantic Model Engine MUST NOT infer missing required business semantics.

The Semantic Model Engine MUST preserve prior canonical model versions for traceability and impact analysis.

---

## 10.0 Readiness Evaluation Capability

This section defines platform support for readiness evaluation. Readiness evaluation may be implemented as a separate subsystem or as a coordinated capability across the Specification Engine, Semantic Model Engine, Governance Engine, and Traceability Engine.

Readiness evaluation determines whether a specification may progress from Draft to Reviewable, Machine-Valid, or Autonomous-Ready.

### 10.1 Responsibilities

The platform MUST provide readiness evaluation capability that can:

* evaluate readiness criteria defined in ISL v1.3
* produce readiness reports
* produce readiness transition records
* detect readiness regression
* verify evidence packages
* consult governance approval gates
* prevent unauthorized readiness progression
* expose current readiness state

### 10.2 Rules

A specification MUST NOT be marked Machine-Valid without passing canonical validation.

A specification MUST NOT be marked Autonomous-Ready without required governance authorization.

Readiness state MUST be version-specific.

Readiness history MUST be preserved.

---

## 11.0 Planning Engine

This section defines the Planning Engine. The Planning Engine transforms the canonical semantic model into a construction task graph.

The Planning Engine is the bridge between specification meaning and executable work. It determines what must be done, in what order, by which role or tool capability, and with what expected outputs.

### 11.1 Responsibilities

The Planning Engine MUST:

* validate planning preconditions
* generate construction tasks from canonical entities
* assign task types and responsible roles
* resolve dependencies
* detect circular dependencies
* generate verification tasks
* generate artifact production plans
* identify parallel groups
* calculate critical path
* identify governance constraints
* validate construction plans
* adapt plans using impact analysis
* produce execution handoff records

### 11.2 Inputs

| Input                   | Description                                     |
| ----------------------- | ----------------------------------------------- |
| Canonical Model         | ISL v1.1 semantic graph                         |
| Readiness Record        | Planning permission state                       |
| Traceability State      | Existing traceability context                   |
| Governance State        | Policies, risk tier, constraints                |
| Tool Capability Catalog | Available deterministic validation capabilities |
| Planning Configuration  | Granularity, templates, platform rules          |

### 11.3 Outputs

| Output                     | Description                              |
| -------------------------- | ---------------------------------------- |
| Construction Task Graph    | Executable or advisory construction plan |
| Planning Validation Report | Validation result for the plan           |
| Critical Path Record       | Longest dependency chain                 |
| Parallel Group Records     | Safe concurrency recommendations         |
| Artifact Production Plan   | Expected artifacts and associations      |
| Execution Handoff Record   | Handoff package for runtime              |

### 11.4 Rules

The Planning Engine MUST NOT produce an executable plan for a specification below the readiness level permitted by ISL v1.3.

Every construction task MUST trace to a specification entity or platform rule.

A construction plan MUST NOT be handed to execution unless it passes planning validation.

---

## 12.0 Agent Orchestrator

This section defines the Agent Orchestrator. The Agent Orchestrator coordinates reasoning agents used during interpretation, planning, generation, review, repair, testing, security validation, and deployment preparation.

The Agent Orchestrator does not replace governance, validation, or execution control. It operates under the Construction Engine and Governance Engine.

### 12.1 Responsibilities

The Agent Orchestrator MUST:

* manage agent roles and assignments
* receive structured task requests from the Construction Engine
* assemble permitted task context
* route reasoning tasks to model providers through the model abstraction boundary
* capture agent outputs
* validate agent outputs against expected structure
* return outputs to the Construction Engine
* record model invocation metadata where applicable
* preserve traceability from task to agent output

### 12.2 Standard Agent Roles

| Agent Role                | Primary Responsibility                               |
| ------------------------- | ---------------------------------------------------- |
| Specification Interpreter | Produces execution-oriented interpretation           |
| Architecture Planner      | Interprets architectural structure and decomposition |
| Construction Planner      | Supports task planning and adaptation                |
| Implementation Generator  | Produces implementation artifacts                    |
| Review Agent              | Reviews generated outputs                            |
| Repair Analyst            | Analyzes validation failures and proposes repairs    |
| Test Generator            | Generates test artifacts and acceptance tests        |
| Security Validator        | Evaluates security and policy implications           |
| Deployment Preparer       | Produces deployment preparation artifacts            |

### 12.3 Rules

Agents MUST operate on structured task context.

Agents MUST NOT directly mutate the canonical model, task graph, artifact repository, or governance state unless mediated by authorized platform interfaces.

Agent outputs MUST NOT be accepted as valid artifacts without deterministic validation where validation is applicable.

Agent activity MUST be traceable to tasks and model invocations.

---

## 13.0 Model Abstraction Boundary

This section defines the architectural boundary for AI model providers. The platform may use local models, enterprise model services, or external model APIs, but agent logic must remain decoupled from provider-specific behavior.

This boundary protects portability, governance, security, and future model evolution.

### 13.1 Responsibilities

The model abstraction boundary MUST:

* provide a standard model invocation interface
* enforce model provider configuration
* enforce model governance restrictions
* route requests to approved providers
* apply context assembly rules
* capture invocation metadata
* normalize model outputs where required
* support error handling and fallback where approved

### 13.2 Rules

Agents MUST invoke models through the model abstraction boundary.

The platform MUST NOT expose unbounded specification, artifact, or governance context to models without context control.

External model usage MUST be governed where policy requires.

Model outputs MUST be treated as reasoning outputs, not deterministic validation results.

---

## 14.0 Construction Engine

This section defines the Construction Engine. The Construction Engine executes construction task graphs and coordinates artifact generation, validation, repair, testing, policy validation, consolidation, and deployment preparation.

The Construction Engine is the operational core of autonomous construction.

### 14.1 Responsibilities

The Construction Engine MUST:

* accept validated execution handoff records
* initialize execution state
* execute construction phases defined in ISL v1.2
* schedule eligible tasks
* invoke the Agent Orchestrator for reasoning tasks
* invoke the Tool Integration Layer for deterministic tool tasks
* write generated artifacts to the Artifact Repository
* initiate validation and repair cycles
* enforce repair termination rules
* consult the Governance Engine at control points
* update traceability during execution
* produce execution records and completion reports

### 14.2 Inputs

| Input                         | Description                                      |
| ----------------------------- | ------------------------------------------------ |
| Construction Task Graph       | Executable plan                                  |
| Execution Handoff Record      | Planning-to-runtime handoff                      |
| Runtime Configuration         | Scheduling, repair limits, timeouts, concurrency |
| Governance State              | Runtime constraints                              |
| Tool Registry                 | Registered tools and capabilities                |
| Artifact Repository Reference | Target artifact storage                          |

### 14.3 Outputs

| Output                         | Description                       |
| ------------------------------ | --------------------------------- |
| Execution Graph                | Runtime execution state graph     |
| Generated Artifacts            | Construction outputs              |
| Validation Results             | Deterministic validation evidence |
| Repair Records                 | Repair loop records               |
| Completion Report              | Final execution summary           |
| Deployment Preparation Records | Deployment readiness outputs      |

### 14.4 Rules

The Construction Engine MUST NOT begin execution unless ISL v1.2 preconditions are satisfied.

The Construction Engine MUST NOT mark artifacts valid without required validation.

The Construction Engine MUST enforce governance decisions.

The Construction Engine MUST maintain execution state sufficient for recovery.

---

## 15.0 Tool Integration Layer

This section defines the Tool Integration Layer. This layer connects the platform to deterministic engineering tools used for compilation, testing, static analysis, security scanning, infrastructure validation, packaging, deployment preparation, and policy checking.

The Tool Integration Layer is the validation boundary between generated artifacts and trusted outputs.

### 15.1 Responsibilities

The Tool Integration Layer MUST:

* maintain a Tool Registry
* manage Tool Records and trust profiles
* select tools by declared capability
* enforce tool governance restrictions
* invoke tools through structured requests
* execute tools in controlled environments
* normalize tool outputs
* produce structured findings
* detect tool failures, timeouts, conflicts, and version drift
* emit tool telemetry
* record tool traceability links

### 15.2 Rules

The platform MUST NOT invoke unregistered tools.

Tool outputs MUST be normalized before influencing execution state.

Tool failure MUST NOT be interpreted as success.

Security, deployment, and validation tool evidence MUST be retained where required by governance.

---

## 16.0 Traceability Engine

This section defines the Traceability Engine. The Traceability Engine maintains the persistent graph linking specification entities, tasks, artifacts, validations, repairs, decisions, tool invocations, model invocations, governance events, and deployment artifacts.

Traceability is required for auditability, impact analysis, repair explanation, and artifact trust.

### 16.1 Responsibilities

The Traceability Engine MUST:

* create traceability nodes and edges
* enforce traceability identifier rules
* associate artifacts with source specification entities
* record task, validation, repair, tool, model, and governance links
* create traceability snapshots
* perform traceability integrity checks
* support impact analysis
* support bidirectional traceability queries
* preserve historical traceability across versions

### 16.2 Rules

Traceability MUST be recorded at lifecycle event time.

Untraced artifacts MUST NOT be accepted as stable outputs.

Execution completion MUST be blocked if required traceability is incomplete.

Traceability snapshots MUST be created at required lifecycle points.

---

## 17.0 Governance Engine

This section defines the Governance Engine. The Governance Engine enforces the governance and control model across readiness, planning, execution, tool usage, model usage, repair, waiver, override, deployment, and audit.

Governance is an active runtime control layer, not a passive approval record.

### 17.1 Responsibilities

The Governance Engine MUST:

* manage role assignments
* evaluate policies
* enforce violation responses
* manage approval gates
* assign or validate risk tiers
* enforce separation-of-duties rules
* manage waivers and overrides
* open and resolve escalations
* evaluate governance runtime checks
* authorize or block deployment
* record immutable audit events
* expose governance audit queries

### 17.2 Rules

The Governance Engine MUST return explicit decisions to runtime governance checks.

Runtime components MUST obey governance decisions.

Governance audit events MUST be immutable.

Expired approvals, waivers, or overrides MUST NOT authorize progression.

---

## 18.0 Execution Monitor

This section defines the Execution Monitor. The Execution Monitor provides visibility into platform activity, execution state, task progress, artifact generation, validation outcomes, tool performance, agent activity, governance events, and runtime health.

Observability is essential because autonomous construction must be explainable and controllable.

### 18.1 Responsibilities

The Execution Monitor MUST:

* collect execution telemetry
* collect agent invocation telemetry
* collect tool invocation telemetry
* collect governance telemetry
* collect artifact lifecycle events
* expose task and phase progress
* expose validation and repair status
* expose errors and escalations
* support dashboards or query interfaces
* support operational alerts where configured

### 18.2 Rules

The platform MUST record telemetry for major lifecycle events.

Telemetry MUST be correlated with execution, task, artifact, tool, model, and governance identifiers where applicable.

Telemetry does not replace audit logs or traceability records.

---

## 19.0 Artifact Repository

This section defines the Artifact Repository. The Artifact Repository stores generated outputs and related metadata produced during autonomous construction.

The Artifact Repository bridges autonomous generation and conventional software engineering practices by preserving source code, configuration, schemas, tests, contracts, infrastructure definitions, documentation, packages, and deployment materials.

### 19.1 Responsibilities

The Artifact Repository MUST:

* store generated artifacts
* store artifact metadata
* preserve artifact versions
* support repository organization by system, module, service, or artifact type
* support artifact status tracking
* preserve validation evidence or references
* preserve deployment preparation outputs
* support artifact retrieval by platform subsystems
* integrate with traceability records
* prevent stable acceptance of untraced artifacts

### 19.2 Artifact States

The repository MUST support artifact states including:

* pending
* valid
* failed
* repaired
* escalated
* deprecated
* superseded

### 19.3 Rules

An artifact MUST NOT be marked stable unless it has required traceability and validation status.

Artifact version history MUST be preserved.

Superseded artifacts MUST remain available for audit or reconstruction where required.

---

## 20.0 Platform Data Flow

This section defines the primary data flows across the platform. Data flow clarity is required so that implementations do not bypass validation, governance, traceability, or artifact control boundaries.

### 20.1 Primary Data Flow

The standard platform data flow is:

1. Authored specification enters Specification Engine.
2. Parsed specification enters Semantic Model Engine.
3. Canonical model enters readiness evaluation.
4. Machine-Valid canonical model enters Planning Engine.
5. Construction Task Graph enters governance evaluation and execution handoff.
6. Execution Runtime initializes Execution Graph.
7. Agent Orchestrator produces reasoning outputs for generation and repair tasks.
8. Artifact outputs enter Artifact Repository with pending status.
9. Tool Integration Layer validates artifacts.
10. Validation results update execution, traceability, and governance state.
11. Repair cycles update artifacts and validation records.
12. Consolidated artifacts enter stable repository state.
13. Deployment preparation artifacts are generated and validated.
14. Completion report, traceability snapshot, and governance evidence are stored.

### 20.2 Data Flow Rules

The platform MUST NOT allow artifact generation before required readiness and governance conditions are met.

The platform MUST NOT allow artifact acceptance before traceability and validation requirements are satisfied.

The platform MUST NOT allow deployment authorization without governance approval.

The platform MUST preserve evidence and traceability across data flow boundaries.

---

## 21.0 Control Boundaries

This section defines the architecture’s control boundaries. Control boundaries prevent one subsystem from bypassing another subsystem’s responsibilities.

### 21.1 Required Control Boundaries

| Boundary                                          | Control Requirement                                 |
| ------------------------------------------------- | --------------------------------------------------- |
| Authored Specification → Canonical Model          | Must pass parsing and normalization                 |
| Canonical Model → Readiness                       | Must pass semantic validation                       |
| Machine-Valid → Planning                          | Must satisfy planning permissions                   |
| Plan → Execution                                  | Must pass planning validation and governance checks |
| Agent Output → Artifact Acceptance                | Must pass deterministic validation where applicable |
| Tool Result → Runtime Decision                    | Must be normalized                                  |
| Artifact → Stable Repository                      | Must have traceability and valid status             |
| Runtime Action → Governed Action                  | Must consult Governance Engine when required        |
| Deployment Preparation → Deployment Authorization | Must pass governance authorization                  |

### 21.2 Boundary Rules

Subsystems MUST NOT bypass required control boundaries.

A platform implementation that combines subsystems into one deployable component MUST still enforce logical control boundaries.

Control boundary violations MUST be recorded as platform integrity errors.

---

## 22.0 Platform State

This section defines platform state at an architectural level. Detailed state models are defined later, but the platform architecture must identify the state categories that must be preserved.

### 22.1 State Categories

The platform MUST maintain state for:

* specifications
* canonical models
* readiness levels
* construction plans
* execution graphs
* artifacts
* validations
* repairs
* traceability graphs
* governance records
* tool records
* model invocation records
* telemetry and monitoring records
* deployment preparation records

### 22.2 State Rules

Platform state MUST be persistent where required for audit, recovery, traceability, or governance.

Execution state MUST be recoverable after interruption.

Governance and traceability state MUST not be silently overwritten.

State updates affecting readiness, execution, artifacts, or governance MUST be auditable.

---

## 23.0 Security Architecture Requirements

This section defines baseline security requirements for the platform architecture. Detailed security architecture is defined later, but these minimum requirements apply to all conforming platform implementations.

Autonomous construction introduces security risk because the platform processes specifications, invokes models, executes tools, generates artifacts, and interacts with repositories and deployment systems.

### 23.1 Minimum Security Requirements

The platform architecture MUST support:

* identity and access control for users and services
* role-based governance authorization
* controlled model invocation
* controlled tool execution
* sandboxing for generated code execution where applicable
* artifact repository access control
* audit logging
* secret handling restrictions
* network access controls for tool and model integrations
* protection of governance and traceability records
* dependency and supply-chain validation where applicable

### 23.2 Security Boundary Rules

Tools MUST execute through the Tool Integration Layer.

Models MUST be invoked through the model abstraction boundary.

Artifacts MUST enter the repository through controlled interfaces.

Governance records MUST be protected from unauthorized modification.

---

## 24.0 Local-First Architecture Requirement

This section defines the local-first architecture requirement. Local-first operation means the platform can operate without requiring external cloud services, external model providers, or external tool services, provided local equivalents are configured.

Local-first does not prohibit cloud or enterprise integrations. It ensures the platform remains deployable in restricted or privacy-sensitive environments.

### 24.1 Local-First Requirements

A conforming platform SHOULD support local deployment of:

* Specification Engine
* Semantic Model Engine
* Planning Engine
* Agent Orchestrator
* Construction Engine
* Tool Integration Layer
* Traceability Engine
* Governance Engine
* Execution Monitor
* Artifact Repository

A local-first deployment SHOULD support local deterministic tools and local or self-hosted model providers where configured.

### 24.2 Local-First Rules

The platform MUST NOT require external services for core specification validation, planning, traceability, governance, and artifact repository functions unless the deployment profile explicitly declares such dependencies.

External provider usage MUST be configurable and governable.

---

## 25.0 Enterprise Scalability Requirements

This section defines baseline scalability expectations. The platform must support growth from local experiments to team, enterprise, distributed, and hybrid deployments.

The architecture must allow independent scaling of workload-heavy subsystems without changing the core semantic contracts.

### 25.1 Scalable Subsystems

The architecture SHOULD permit independent scaling of:

* Construction Engine workers
* Agent Orchestrator capacity
* model invocation infrastructure
* Tool Integration workers
* Artifact Repository storage
* Traceability query infrastructure
* telemetry and monitoring storage

### 25.2 Scalability Rules

Scaling MUST NOT weaken governance, traceability, validation, or audit requirements.

Distributed execution MUST preserve task state consistency.

Parallel execution MUST respect dependencies and governance constraints.

Multi-project operation MUST isolate specification, artifact, traceability, and governance contexts.

---

## 26.0 Platform Interface Requirements

This section defines required interface categories. The exact API technology is implementation-defined, but the platform must expose interfaces sufficient to support authoring, validation, planning, execution, governance, traceability, artifacts, and monitoring.

### 26.1 Required Interface Categories

A conforming platform SHOULD expose controlled interfaces for:

* specification submission and retrieval
* structural validation
* canonical model retrieval
* readiness evaluation
* construction planning
* execution initiation and monitoring
* artifact repository access
* traceability queries
* governance approvals and audit queries
* tool registry management
* telemetry and operational status

### 26.2 Interface Rules

Platform interfaces MUST enforce access control.

Platform interfaces MUST preserve specification version context.

Governance-controlled actions MUST require governance authorization.

Interfaces that expose artifacts, governance records, or traceability records MUST enforce appropriate permissions.

---

## 27.0 Platform Error Classes

This section defines platform-architecture-level error classes. These errors represent failures in subsystem boundaries, integration behavior, state consistency, and architecture control.

More specific error classes are defined in earlier ISL documents for language, canonical validation, readiness, execution, traceability, planning, tools, and governance.

### 27.1 Platform Error Record Schema

| Field                 | Type     | Required    | Description                                               |
| --------------------- | -------- | ----------- | --------------------------------------------------------- |
| error-id              | string   | REQUIRED    | Unique platform error identifier                          |
| error-class           | enum     | REQUIRED    | Platform error class                                      |
| severity              | enum     | REQUIRED    | blocking, high, medium, low                               |
| subsystem             | string   | REQUIRED    | Subsystem where error occurred                            |
| specification-id      | string   | CONDITIONAL | Related system identifier                                 |
| specification-version | semver   | CONDITIONAL | Related specification version                             |
| target-id             | string   | CONDITIONAL | Related task, artifact, model, tool, or governance target |
| message               | string   | REQUIRED    | Human-readable explanation                                |
| required-action       | string   | REQUIRED    | Remediation required                                      |
| recorded-at           | ISO 8601 | REQUIRED    | Time error was recorded                                   |

### 27.2 Error Classes

| Error Class                              | Description                                                              | Default Severity |
| ---------------------------------------- | ------------------------------------------------------------------------ | ---------------- |
| platform-boundary-violation              | Subsystem bypassed required control boundary                             | blocking         |
| platform-state-inconsistent              | Platform state is inconsistent across subsystems                         | blocking         |
| platform-subsystem-unavailable           | Required subsystem unavailable                                           | blocking or high |
| platform-version-mismatch                | Subsystems reference inconsistent specification or model versions        | blocking         |
| platform-governance-unavailable          | Governance decision required but Governance Engine unavailable           | blocking         |
| platform-traceability-unavailable        | Traceability recording required but unavailable                          | blocking         |
| platform-artifact-repository-unavailable | Artifact storage required but unavailable                                | blocking         |
| platform-tool-layer-unavailable          | Deterministic validation required but Tool Integration Layer unavailable | blocking         |
| platform-model-boundary-violation        | Agent or subsystem bypassed model abstraction boundary                   | high             |
| platform-observability-degraded          | Required telemetry or monitoring unavailable                             | medium or high   |
| platform-security-boundary-violation     | Security control boundary violated                                       | blocking         |

---

## 28.0 Conformance Requirements

This section defines conformance requirements for platform architecture implementations. A platform may vary in technology stack, deployment model, or scale, but it must preserve the logical architecture and control behavior defined here.

### 28.1 Platform Architecture Conformance

A platform conforms to ISL v2.0 if it includes or implements the required logical responsibilities for:

* Specification Engine
* Semantic Model Engine
* readiness evaluation
* Planning Engine
* Agent Orchestrator
* model abstraction boundary
* Construction Engine
* Tool Integration Layer
* Traceability Engine
* Governance Engine
* Execution Monitor
* Artifact Repository

### 28.2 Subsystem Boundary Conformance

A platform conforms to subsystem boundary requirements if it:

* enforces specification-to-canonical validation boundaries
* enforces readiness gates before planning and execution
* enforces planning validation before execution
* enforces deterministic validation before artifact acceptance
* enforces traceability before stable artifact acceptance
* enforces governance decisions at runtime
* enforces deployment authorization before deployment

### 28.3 Integration Conformance

A platform conforms to integration requirements if it:

* invokes models only through the model abstraction boundary
* invokes tools only through the Tool Integration Layer
* stores artifacts only through controlled repository interfaces
* records traceability through the Traceability Engine
* records governance decisions through the Governance Engine
* records telemetry through the Execution Monitor or observability system

### 28.4 Enterprise Architecture Conformance

A platform conforms to enterprise architecture expectations if it:

* supports auditability
* supports role-based governance
* preserves historical records
* supports versioned specifications and artifacts
* supports local-first deployment profile
* supports controlled scaling
* protects sensitive state and artifacts
* exposes required governance, traceability, and monitoring queries

---

## 29.0 Summary

ISL v2.0 defines the IMHOTEP Platform Architecture Model. It establishes the logical subsystems, architectural boundaries, data flows, control points, and conformance expectations required for an implementation of the IMHOTEP autonomous software construction platform.

This revision strengthens the platform architecture by making subsystem responsibilities explicit and by treating governance, traceability, deterministic validation, observability, security, and artifact control as architectural requirements rather than optional implementation details.

A conforming IMHOTEP platform MUST preserve the architecture’s control boundaries even when subsystems are implemented within a single process or service. The platform may vary in technology, deployment topology, and scale, but it MUST remain specification-driven, traceable, governed, observable, and validation-centered.
