# IMHOTEP Specification Language (ISL) v1.4

# The Traceability Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3
**Supersedes:** ISL r1 v1.4 Traceability Model
**Document Type:** Traceability and Provenance Specification

---

## 1.0 Scope

This document defines the traceability model used by the IMHOTEP Specification Language and conforming IMHOTEP platform implementations. Traceability establishes the required relationships between specification entities, construction tasks, generated artifacts, validation results, repair actions, governance events, and lifecycle changes.

This document applies to all lifecycle phases beginning with canonical specification normalization and continuing through planning, execution, validation, repair, artifact consolidation, deployment preparation, and later specification evolution. Traceability is not limited to documentation or reporting. It is a required system property that governs whether generated artifacts can be accepted, audited, maintained, or deployed.

This document defines:

* traceability principles
* traceability identifiers
* traceability graph structure
* artifact association rules
* derivation relationships
* bidirectional traceability requirements
* impact analysis requirements
* decision record requirements
* integrity checks
* audit queries
* lifecycle and versioning behavior
* conformance requirements

---

## 2.0 Purpose of the Traceability Model

The purpose of the Traceability Model is to ensure that every meaningful output of the autonomous construction process can be explained in relation to the specification that caused it to exist. In a conventional software project, traceability is often maintained through documentation, ticket links, commit references, or manual review. In IMHOTEP, traceability is a first-class structural property of the platform.

Autonomous construction requires stronger traceability than ordinary development because artifacts may be generated, validated, repaired, and replaced by automated systems. Without enforced traceability, the platform cannot determine why an artifact exists, which requirement it satisfies, which policy governs it, which validation proves it, or whether it must be regenerated when the specification changes.

The Traceability Model exists to ensure that:

* specification intent remains connected to implementation artifacts
* construction tasks can be traced to source entities
* generated artifacts can be traced back to requirements, policies, services, data entities, and interfaces
* validation results can be traced to the artifacts and specification elements they prove
* repair decisions can be audited
* governance decisions can be associated with affected entities and artifacts
* specification changes can trigger precise impact analysis
* no untraced artifact can enter the stable artifact repository

---

## 3.0 Normative References

This section identifies the ISL documents and external standards required to interpret and implement the Traceability Model. Traceability depends directly on canonical identifiers, readiness rules, execution behavior, artifact generation, validation outcomes, and governance control.

| Reference   | Purpose                                                                        |
| ----------- | ------------------------------------------------------------------------------ |
| ISL v0.0    | Corpus-level principles, conformance, and external standards alignment         |
| ISL v1.0    | Authored specification structure and section-origin metadata                   |
| ISL v1.1    | Canonical semantic model, entity identifiers, and relationships                |
| ISL v1.2    | Execution phases, artifact metadata, validation, repair, and execution records |
| ISL v1.3    | Readiness levels, regression, and readiness evidence                           |
| ISL v1.5    | Construction task graph and planning relationships                             |
| ISL v1.6    | Tool invocation records and deterministic validation outcomes                  |
| ISL v1.7    | Governance records, approval gates, waivers, and audit events                  |
| W3C PROV-DM | Provenance alignment model for entity, activity, and agent relationships       |

---

## 4.0 Terms and Definitions

This section defines terminology specific to traceability. These terms are used by traceability engines, planning engines, execution runtimes, governance systems, and audit tools.

**Traceability**
The ability to link specification entities, construction tasks, artifacts, validations, repairs, decisions, and governance events through explicit relationships.

**Traceability Graph**
The directed graph representing traceability nodes and edges across the construction lifecycle.

**Traceability Node**
A graph node representing a specification entity, construction task, artifact, validation result, decision record, repair record, tool invocation, model invocation, governance event, or deployment artifact.

**Traceability Edge**
A typed relationship between two traceability nodes.

**Forward Traceability**
Navigation from specification intent to the artifacts, tasks, validations, or governance records derived from it.

**Reverse Traceability**
Navigation from an artifact or execution record back to the specification entities, tasks, decisions, and validations that caused or affected it.

**Derivation**
A relationship indicating that one object was created, modified, inferred, or replaced because of another object.

**Impact Analysis**
The process of identifying artifacts, tasks, validations, policies, and execution records affected by a specification change.

**Untraced Artifact**
An artifact lacking a valid traceability relationship to one or more specification entities or approved inferred derivation records.

**Decision Record**
A traceability record describing a significant construction, repair, validation, or governance decision.

---

## 5.0 Traceability Design Principles

This section defines the principles that govern traceability across the ISL lifecycle. A conforming implementation MUST enforce these principles rather than treating traceability as optional metadata.

### 5.1 Traceability Is Structural

Traceability MUST be implemented as part of the platform’s persistent state. It MUST NOT be reconstructed solely from commit messages, file names, comments, or informal documentation.

The traceability graph is a required structural representation used by planning, execution, governance, audit, and impact analysis.

### 5.2 Traceability Is Created at the Time of Action

Traceability relationships MUST be recorded when the related action occurs. Artifact relationships MUST be recorded when artifacts are created. Validation relationships MUST be recorded when validation runs. Repair relationships MUST be recorded when repair actions occur.

The platform MUST NOT rely on retrospective reconstruction as the primary traceability mechanism.

### 5.3 Traceability Is Bidirectional

The platform MUST support navigation in both directions:

* from specification entity to derived tasks, artifacts, validations, decisions, and governance events
* from artifact, task, validation, or decision back to the originating specification entities

Bidirectional traceability is required for audit, debugging, maintenance, and impact analysis.

### 5.4 Traceability Is Versioned

Traceability MUST preserve historical relationships across specification versions. When a specification changes, prior traceability graphs MUST remain queryable. New traceability relationships MUST be added rather than overwriting history.

### 5.5 Traceability Is Integrity-Enforced

An artifact, task, validation result, or governance event that lacks required traceability MUST be treated as a traceability integrity violation.

A stable artifact repository MUST NOT accept untraced artifacts as valid construction outputs.

### 5.6 Traceability Supports Accountability

Traceability records MUST preserve enough information to determine what action occurred, what caused it, who or what performed it, what artifacts were affected, and what evidence supports the result.

---

## 6.0 Traceability Identifier Rules

This section defines the identifier rules used by traceability records. Entity identifiers are defined by ISL v1.1, but traceability also requires identifiers for tasks, artifacts, validations, repairs, decisions, tool invocations, model invocations, and governance events.

Stable identifiers are essential because traceability depends on persistent references across lifecycle events.

### 6.1 Canonical Entity Identifiers

Canonical entity identifiers MUST follow the format defined by ISL v1.1:

`{PREFIX}-{system-id}-{sequence}`

Examples:

* REQ-acme-order-00001
* SVC-acme-order-00001
* VAL-acme-order-00001

Canonical entity identifiers MUST be immutable and MUST NOT be reused after deprecation.

### 6.2 Traceability Object Identifier Prefixes

The following prefixes SHALL be used for traceability objects that are not canonical specification entities.

| Object Type           | Prefix |
| --------------------- | ------ |
| Construction Task     | TSK    |
| Artifact              | ART    |
| Validation Result     | VRS    |
| Repair Record         | RPR    |
| Decision Record       | DEC    |
| Tool Invocation       | TIV    |
| Model Invocation      | MIV    |
| Governance Event      | GOV    |
| Impact Analysis       | IAN    |
| Traceability Snapshot | TRS    |
| Deployment Artifact   | DPA    |
| Execution Event       | EXE    |

### 6.3 Traceability Object Identifier Format

Traceability object identifiers MUST conform to:

`{PREFIX}-{system-id}-{sequence}`

Where:

| Component | Description                                    |
| --------- | ---------------------------------------------- |
| PREFIX    | Object prefix from §6.2                        |
| system-id | Normalized Project system identifier           |
| sequence  | Zero-padded sequence unique within object type |

Example:

`ART-acme-order-00042`

### 6.4 Identifier Rules

Traceability identifiers MUST be unique within their object type and system.

Traceability identifiers MUST remain stable after assignment.

Traceability identifiers MUST NOT be reused after deletion, deprecation, supersession, or archival.

A traceability object MUST NOT be accepted into persistent state without a valid identifier.

---

## 7.0 Traceability Graph Overview

This section defines the traceability graph as the primary structure used to represent traceability. The graph connects specification entities to construction and lifecycle records through typed relationships.

The traceability graph extends the canonical semantic graph. The canonical graph represents specification meaning. The traceability graph represents lifecycle derivation and evidence.

### 7.1 Graph Definition

The traceability graph SHALL be a directed graph.

| Graph Element | Description                                                                                                                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Node          | Specification entity, task, artifact, validation result, decision, repair, invocation, governance event, or deployment artifact |
| Edge          | Typed traceability relationship                                                                                                 |
| Root          | Project entity                                                                                                                  |
| Snapshot      | Versioned view of the traceability graph at a point in lifecycle time                                                           |

### 7.2 Required Graph Properties

The traceability graph MUST support:

* directed relationships
* typed edges
* bidirectional traversal
* versioned snapshots
* immutable historical records
* query by node identifier
* query by relationship type
* query by specification version
* query by artifact identifier
* query by governance event
* query by validation outcome

### 7.3 Relationship to Canonical Graph

The traceability graph MUST include or reference canonical entity identifiers from ISL v1.1.

The traceability graph MAY store canonical graph relationships directly or may reference a canonical model version.

A traceability graph MUST identify the canonical model version from which its specification entity nodes originate.

---

## 8.0 Traceability Node Types

This section defines node types used in the traceability graph. Node types allow the platform to distinguish specification concepts from construction activities, generated outputs, evidence, and governance actions.

### 8.1 Required Node Types

| Node Type            | Description                                                    |
| -------------------- | -------------------------------------------------------------- |
| SpecificationEntity  | Canonical entity from ISL v1.1                                 |
| ConstructionTask     | Task from the Construction Task Graph                          |
| Artifact             | Generated or managed construction output                       |
| ValidationResult     | Result of deterministic validation or test execution           |
| RepairRecord         | Record of repair activity                                      |
| DecisionRecord       | Record explaining significant construction or repair decisions |
| ToolInvocation       | Deterministic tool invocation record                           |
| ModelInvocation      | Reasoning model invocation record                              |
| GovernanceEvent      | Approval, waiver, escalation, or policy event                  |
| ImpactAnalysis       | Impact analysis result                                         |
| TraceabilitySnapshot | Versioned graph snapshot                                       |
| DeploymentArtifact   | Deployment-related artifact                                    |
| ExecutionEvent       | Runtime event from execution lifecycle                         |

### 8.2 Node Base Schema

Every traceability node MUST include:

| Field                 | Type     | Required | Description                                                 |
| --------------------- | -------- | -------- | ----------------------------------------------------------- |
| node-id               | string   | REQUIRED | Unique identifier                                           |
| node-type             | enum     | REQUIRED | Node type from §8.1                                         |
| specification-id      | string   | REQUIRED | System identifier                                           |
| specification-version | semver   | REQUIRED | Specification version associated with the node              |
| created-at            | ISO 8601 | REQUIRED | Time node was created                                       |
| created-by            | string   | REQUIRED | Platform component, agent, tool, or authority creating node |
| status                | enum     | REQUIRED | active, deprecated, superseded, failed, archived            |
| metadata              | object   | OPTIONAL | Supplemental metadata                                       |

### 8.3 Node Rules

A node MUST NOT exist without a valid node-id.

A node MUST identify the specification version to which it relates.

A node representing an artifact MUST include artifact metadata as defined in ISL v1.2.

A node representing a governance event MUST reference governance records defined in ISL v1.7.

A node representing a tool invocation MUST reference tool records defined in ISL v1.6.

---

## 9.0 Traceability Edge Types

This section defines the edge types used in the traceability graph. Edge types describe how objects are related across the lifecycle.

Edges are semantically meaningful. An implementation MUST NOT collapse all traceability relationships into generic links.

### 9.1 Edge Types

| Edge Type    | Source                                 | Target                                         | Meaning                                               |
| ------------ | -------------------------------------- | ---------------------------------------------- | ----------------------------------------------------- |
| originates   | SpecificationEntity                    | ConstructionTask                               | Specification entity caused task creation             |
| produces     | ConstructionTask                       | Artifact                                       | Task produced artifact                                |
| implements   | Artifact                               | SpecificationEntity                            | Artifact implements specification entity              |
| validates    | ValidationResult                       | Artifact or SpecificationEntity                | Validation result verifies target                     |
| evaluates    | ValidationResult                       | Policy or Validation                           | Result evaluates rule or criterion                    |
| invokes      | ConstructionTask or ExecutionEvent     | ToolInvocation or ModelInvocation              | Task or event invoked tool/model                      |
| generated-by | Artifact                               | ModelInvocation or ToolInvocation              | Artifact was generated by invocation                  |
| reviewed-by  | Artifact or DecisionRecord             | GovernanceEvent                                | Governance event reviewed target                      |
| governed-by  | Artifact, Task, or SpecificationEntity | Policy                                         | Target is constrained by policy                       |
| repairs      | RepairRecord                           | Artifact                                       | Repair record modifies or attempts to modify artifact |
| supersedes   | Artifact                               | Artifact                                       | New artifact replaces prior artifact                  |
| depends-on   | Artifact or Task                       | Artifact or Task                               | Source requires target                                |
| derived-from | Artifact, Task, or DecisionRecord      | SpecificationEntity or Artifact                | Source was derived from target                        |
| affects      | ImpactAnalysis                         | Artifact, Task, or SpecificationEntity         | Impact analysis identified affected target            |
| unaffected   | ImpactAnalysis                         | Artifact, Task, or SpecificationEntity         | Impact analysis identified target as unaffected       |
| escalates-to | Task, Artifact, or RepairRecord        | GovernanceEvent                                | Target was escalated to governance                    |
| authorizes   | GovernanceEvent                        | Readiness, Task, Deployment, or ExecutionEvent | Governance event authorizes target                    |
| blocks       | GovernanceEvent or Policy              | Task, Artifact, or ExecutionEvent              | Governance or policy blocks target                    |
| waives       | GovernanceEvent                        | PolicyEvaluation or ValidationResult           | Governance event waives finding                       |

### 9.2 Edge Base Schema

Every traceability edge MUST include:

| Field                 | Type     | Required    | Description                                                 |
| --------------------- | -------- | ----------- | ----------------------------------------------------------- |
| edge-id               | string   | REQUIRED    | Unique edge identifier                                      |
| edge-type             | enum     | REQUIRED    | Edge type from §9.1                                         |
| source-node-id        | string   | REQUIRED    | Source node identifier                                      |
| target-node-id        | string   | REQUIRED    | Target node identifier                                      |
| specification-id      | string   | REQUIRED    | System identifier                                           |
| specification-version | semver   | REQUIRED    | Specification version                                       |
| created-at            | ISO 8601 | REQUIRED    | Time edge was created                                       |
| created-by            | string   | REQUIRED    | Platform component, tool, agent, or authority creating edge |
| rationale             | string   | CONDITIONAL | Required when edge is inferred                              |
| confidence            | enum     | CONDITIONAL | explicit, inferred, imported, repaired                      |

### 9.3 Edge Rules

Every edge source and target MUST resolve to existing traceability nodes.

An edge MUST use a defined edge type.

Inferred edges MUST include a rationale.

Edges MUST NOT be deleted when historical traceability must be preserved. Corrections MUST be represented through supersession, deprecation, or corrective records.

---

## 10.0 Artifact Association

This section defines the required association between generated artifacts and their originating specification entities. Artifact association is the minimum traceability requirement for construction outputs.

An artifact without a specification association cannot be trusted as a product of the ISL construction process.

### 10.1 Artifact Association Record Schema

Each artifact association MUST include:

| Field                 | Type     | Required    | Description                                                           |
| --------------------- | -------- | ----------- | --------------------------------------------------------------------- |
| association-id        | string   | REQUIRED    | Unique association identifier                                         |
| artifact-id           | string   | REQUIRED    | Artifact identifier                                                   |
| artifact-type         | enum     | REQUIRED    | source, config, schema, infrastructure, contract, test, documentation |
| source-entity-ids     | array    | REQUIRED    | One or more originating specification entity identifiers              |
| task-id               | string   | REQUIRED    | Construction task that produced artifact                              |
| derivation-type       | enum     | REQUIRED    | direct, derived, inferred, repair                                     |
| created-at            | ISO 8601 | REQUIRED    | Time association was created                                          |
| specification-version | semver   | REQUIRED    | Specification version at artifact creation                            |
| rationale             | string   | CONDITIONAL | Required when derivation-type is inferred                             |
| validation-status     | enum     | REQUIRED    | pending, valid, failed, waived, escalated                             |

### 10.2 Derivation Types

| Derivation Type | Meaning                                                                      |
| --------------- | ---------------------------------------------------------------------------- |
| direct          | Artifact directly implements one or more specification entities              |
| derived         | Artifact is produced as a consequence of implementing specification entities |
| inferred        | Artifact was created to satisfy an implicit structural need                  |
| repair          | Artifact replaces or modifies another artifact due to repair                 |

### 10.3 Artifact Association Rules

Every artifact produced by the platform MUST have at least one Artifact Association Record.

An artifact with derivation-type `inferred` MUST include a rationale.

An artifact with derivation-type `repair` MUST reference the prior artifact and Repair Record.

An artifact MUST NOT be marked valid unless its traceability association exists.

An unassociated artifact MUST be treated as an untraced artifact and MUST NOT enter the stable artifact repository.

---

## 11.0 Traceability During Construction Planning

This section defines traceability requirements during construction planning. Planning creates the bridge between specification entities and executable construction tasks.

The Planning Engine MUST record why each task exists. Without planning traceability, later artifacts cannot be reliably traced back to specification intent.

### 11.1 Required Planning Traceability

For every generated construction task, the platform MUST record:

| Relationship                                                          | Required |
| --------------------------------------------------------------------- | -------- |
| SpecificationEntity originates ConstructionTask                       | YES      |
| ConstructionTask depends-on ConstructionTask, when dependencies exist | YES      |
| ConstructionTask governed-by Policy, when policies apply              | YES      |
| ConstructionTask derived-from Construction Plan                       | YES      |

### 11.2 Planning Traceability Rules

Every construction task MUST trace to at least one source specification entity unless it is a platform-required task such as consolidation, validation, or deployment preparation.

Platform-required tasks MUST include a rationale and reference the phase or rule that caused them to be generated.

A task without traceability MUST NOT be eligible for execution.

---

## 12.0 Traceability During Execution

This section defines traceability requirements during execution. Execution produces artifacts, validations, repairs, tool invocations, model invocations, decisions, and governance events. Each must be linked as it occurs.

Execution traceability ensures that runtime behavior can be reconstructed after the fact and that failures can be diagnosed.

### 12.1 Required Execution Traceability

The runtime MUST record traceability links for:

* task start and completion
* artifact generation
* deterministic validation
* test execution
* tool invocation
* model invocation
* repair cycle initiation
* repair modifications
* security and policy evaluation
* escalation events
* waivers and overrides
* artifact consolidation
* deployment preparation

### 12.2 Execution Timing Rule

Traceability links MUST be created at the time of the execution event.

If traceability recording fails, the runtime MUST mark the affected task or artifact as failed or escalated.

### 12.3 Execution Traceability Failure

A traceability failure during execution MUST produce an execution-traceability-failure error as defined by ISL v1.2.

Execution MUST NOT mark a construction plan completed while required execution traceability links are missing.

---

## 13.0 Validation Traceability

This section defines how validation results are linked to artifacts and specification entities. Validation traceability provides evidence that generated artifacts satisfy the requirements, policies, interfaces, workflows, or infrastructure expectations that justified their creation.

Validation without traceability is not sufficient evidence for readiness, completion, or governance.

### 13.1 Required Validation Links

Every validation result MUST link to:

| Target                          | Required                                        |
| ------------------------------- | ----------------------------------------------- |
| Artifact evaluated              | YES, when artifact-level validation             |
| Validation entity               | YES, when derived from specification validation |
| Tool invocation                 | YES, when deterministic tool used               |
| Requirement or Policy validated | YES, when applicable                            |
| Task that triggered validation  | YES                                             |

### 13.2 Validation Traceability Rules

A validation result MUST NOT be accepted without a target.

A passed validation result MUST reference the artifact or entity it proves.

A failed validation result MUST be linked to any Repair Record it triggers.

A waived validation result MUST be linked to the governance event authorizing the waiver.

---

## 14.0 Repair Traceability

This section defines traceability requirements for repair cycles. Repair actions are especially important to trace because they modify previously generated artifacts in response to validation failures.

Repair traceability makes it possible to understand why an artifact changed, what failure caused the change, who or what proposed the correction, and whether the correction succeeded.

### 14.1 Required Repair Links

Every Repair Record MUST link to:

| Target                       | Required           |
| ---------------------------- | ------------------ |
| Failed validation result     | YES                |
| Artifact being repaired      | YES                |
| Construction task            | YES                |
| Modified artifact version    | YES, when produced |
| Repair decision record       | CONDITIONAL        |
| Subsequent validation result | YES                |

### 14.2 Repair Traceability Rules

A repaired artifact MUST preserve linkage to the original source specification entities.

A repaired artifact MUST supersede the artifact version it replaces.

A repair that modifies an artifact MUST produce a `repairs` edge and a `supersedes` edge when a new artifact version is created.

A repair that fails MUST remain traceable and MUST NOT be deleted.

---

## 15.0 Decision Records

This section defines Decision Records. Decision Records capture significant reasoning, construction, repair, validation, or governance decisions that affect the generated system.

Decision Records are necessary because autonomous systems may make many choices that are not obvious from the final artifact. Enterprise auditability requires that important choices be explainable.

### 15.1 Decision Record Schema

A Decision Record MUST include:

| Field                   | Type     | Required    | Description                                                                                    |
| ----------------------- | -------- | ----------- | ---------------------------------------------------------------------------------------------- |
| decision-id             | string   | REQUIRED    | Unique decision identifier                                                                     |
| decision-type           | enum     | REQUIRED    | interpretation, planning, generation, modification, repair, validation, escalation, governance |
| agent-role              | string   | CONDITIONAL | Agent role making or proposing decision                                                        |
| task-id                 | string   | CONDITIONAL | Related construction task                                                                      |
| artifact-id             | string   | CONDITIONAL | Related artifact                                                                               |
| source-entity-ids       | array    | CONDITIONAL | Specification entities influencing decision                                                    |
| rationale               | string   | REQUIRED    | Explanation of why decision was made                                                           |
| alternatives-considered | array    | CONDITIONAL | Alternatives considered                                                                        |
| model-invocation-id     | string   | CONDITIONAL | Model invocation that produced reasoning                                                       |
| tool-invocation-id      | string   | CONDITIONAL | Tool invocation influencing decision                                                           |
| governance-event-id     | string   | CONDITIONAL | Governance event influencing decision                                                          |
| recorded-at             | ISO 8601 | REQUIRED    | Time decision was recorded                                                                     |
| recorded-by             | string   | REQUIRED    | Agent, tool, runtime, or authority recording decision                                          |

### 15.2 Decision Record Rules

Decision Records MUST be immutable after creation.

Corrections MUST be recorded as new Decision Records referencing the superseded record.

A Decision Record MUST be created for:

* inferred artifacts
* repair proposals
* escalations
* manual overrides
* governance waivers
* construction decisions that materially affect architecture, security, policy, or deployment

A Decision Record SHOULD be created for significant implementation choices when multiple valid alternatives exist.

---

## 16.0 Bidirectional Traceability

This section defines bidirectional navigation requirements. Traceability must support both forward and reverse questions.

Forward traceability answers: “What did this specification entity produce?” Reverse traceability answers: “Why does this artifact exist?”

### 16.1 Forward Traceability

Given a specification entity, the platform MUST be able to return:

* construction tasks derived from the entity
* artifacts implementing or derived from the entity
* validation results proving the entity
* policies governing the entity
* decisions involving the entity
* repairs affecting artifacts derived from the entity
* deployment artifacts derived from the entity

### 16.2 Reverse Traceability

Given an artifact, validation result, repair record, decision record, or governance event, the platform MUST be able to return:

* originating specification entities
* construction tasks involved
* model or tool invocations involved
* validation records affecting status
* repair records affecting current version
* policies governing the object
* governance events authorizing or blocking actions

### 16.3 Navigation Rules

Traceability queries MUST return results scoped to a specification version unless the query explicitly requests cross-version history.

A platform MUST support both direct and transitive traversal.

A platform MUST indicate whether returned relationships are direct, derived, inferred, repaired, superseded, or governed.

---

## 17.0 Impact Analysis

This section defines impact analysis. Impact analysis determines what tasks, artifacts, validations, policies, and deployment materials are affected when a specification changes.

Impact analysis allows the platform to avoid rebuilding everything when only part of the specification changes. It also prevents unsafe partial updates by identifying affected downstream objects.

### 17.1 Impact Analysis Trigger

The platform MUST perform impact analysis when:

* a canonical entity changes
* a requirement is added, removed, or modified
* a policy rule changes
* an interface contract changes
* a data entity attribute changes
* an infrastructure expectation changes
* readiness regression occurs
* governance requires change-impact review

### 17.2 Impact Analysis Record Schema

An Impact Analysis Record MUST include:

| Field                          | Type     | Required    | Description                                                   |
| ------------------------------ | -------- | ----------- | ------------------------------------------------------------- |
| analysis-id                    | string   | REQUIRED    | Unique impact analysis identifier                             |
| triggered-by                   | string   | REQUIRED    | Entity, change event, or governance event triggering analysis |
| specification-id               | string   | REQUIRED    | System identifier                                             |
| previous-specification-version | semver   | CONDITIONAL | Prior version                                                 |
| new-specification-version      | semver   | REQUIRED    | New version                                                   |
| changed-entities               | array    | REQUIRED    | Entities changed                                              |
| affected-artifacts             | array    | REQUIRED    | Artifacts requiring regeneration or review                    |
| affected-tasks                 | array    | REQUIRED    | Tasks requiring re-execution or review                        |
| affected-validations           | array    | CONDITIONAL | Validations requiring re-run or update                        |
| affected-policies              | array    | CONDITIONAL | Policies requiring re-evaluation                              |
| affected-deployment-artifacts  | array    | CONDITIONAL | Deployment artifacts requiring update                         |
| unaffected-artifacts           | array    | REQUIRED    | Artifacts confirmed unaffected                                |
| analysis-method                | string   | REQUIRED    | Traversal or comparison method used                           |
| analysis-timestamp             | ISO 8601 | REQUIRED    | Time analysis completed                                       |
| confidence                     | enum     | REQUIRED    | complete, partial, uncertain                                  |

### 17.3 Impact Analysis Rules

Impact analysis MUST traverse the traceability graph from changed entities through dependent tasks, artifacts, validations, repairs, and deployment artifacts.

Artifacts listed as unaffected MUST NOT be regenerated unless explicitly instructed by governance or runtime configuration.

If impact analysis confidence is `partial` or `uncertain`, the platform MUST escalate or require human review before proceeding with selective reconstruction.

Impact analysis results MUST be recorded as traceability nodes and edges.

---

## 18.0 Traceability Snapshots

This section defines traceability snapshots. A snapshot captures the state of the traceability graph at a specific lifecycle point.

Snapshots support audit, rollback analysis, comparison between specification versions, and reconstruction of historical construction state.

### 18.1 Snapshot Trigger Points

A Traceability Snapshot MUST be created at the following points:

* after canonical normalization reaches Machine-Valid
* before Autonomous-Ready authorization
* at execution start
* after artifact consolidation
* after deployment preparation
* after specification version change
* after impact analysis
* when governance requires audit preservation

### 18.2 Snapshot Schema

A Traceability Snapshot MUST include:

| Field | Type | Required | Description |
|---|---|---|
| snapshot-id | string | REQUIRED | Unique snapshot identifier |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| canonical-model-version | semver | REQUIRED | Canonical model version |
| created-at | ISO 8601 | REQUIRED | Time snapshot created |
| created-by | string | REQUIRED | Platform component creating snapshot |
| node-count | integer | REQUIRED | Number of nodes included |
| edge-count | integer | REQUIRED | Number of edges included |
| snapshot-purpose | enum | REQUIRED | readiness, execution-start, consolidation, deployment, audit, change-impact |
| storage-location | string | REQUIRED | Where snapshot is stored |
| integrity-hash | string | CONDITIONAL | Hash or integrity marker when supported |

### 18.3 Snapshot Rules

Snapshots MUST be immutable once created.

A later snapshot MUST NOT overwrite an earlier snapshot.

Snapshots SHOULD support graph comparison when implementations provide version-diff capability.

---

## 19.0 Traceability Integrity Checks

This section defines integrity checks required to ensure traceability quality. Integrity checks identify missing links, invalid references, untraced artifacts, orphan nodes, and inconsistent graph state.

A platform cannot claim reliable autonomous construction if traceability integrity is not enforced.

### 19.1 Required Integrity Checkpoints

The platform MUST perform traceability integrity checks at the following points:

| Checkpoint              | Check Required                                                            |
| ----------------------- | ------------------------------------------------------------------------- |
| Canonical normalization | Entities have valid identifiers                                           |
| Construction planning   | Tasks trace to specification entities                                     |
| Artifact creation       | Artifacts trace to tasks and source entities                              |
| Validation completion   | Validation results trace to artifacts and validation criteria             |
| Repair completion       | Repairs trace to failures and modified artifacts                          |
| Policy evaluation       | Evaluation records trace to policies and targets                          |
| Artifact consolidation  | Consolidated artifacts are traceable and valid                            |
| Deployment preparation  | Deployment artifacts trace to infrastructure and operational expectations |
| Readiness transition    | Required traceability evidence exists                                     |
| Execution completion    | Traceability graph is complete and consistent                             |

### 19.2 Integrity Check Rules

A traceability integrity check MUST verify:

* all node identifiers are valid
* all edge source and target identifiers resolve
* all artifacts have source associations
* all tasks trace to source entities or justified platform rules
* all validation results trace to targets
* all repairs trace to failures
* all superseded artifacts retain historical links
* no stable artifact is untraced
* no required traceability edge is missing

### 19.3 Integrity Check Record Schema

| Field                 | Type     | Required    | Description                         |
| --------------------- | -------- | ----------- | ----------------------------------- |
| integrity-check-id    | string   | REQUIRED    | Unique check identifier             |
| specification-id      | string   | REQUIRED    | System identifier                   |
| specification-version | semver   | REQUIRED    | Specification version               |
| checkpoint            | string   | REQUIRED    | Lifecycle checkpoint                |
| outcome               | enum     | REQUIRED    | passed, failed, warning             |
| findings              | array    | CONDITIONAL | Integrity findings                  |
| checked-at            | ISO 8601 | REQUIRED    | Time check occurred                 |
| checked-by            | string   | REQUIRED    | Platform component performing check |

### 19.4 Integrity Failure Rules

A failed traceability integrity check MUST block the lifecycle action associated with the checkpoint unless governance explicitly waives the failure.

An untraced artifact finding MUST be treated as blocking for artifact consolidation and execution completion.

---

## 20.0 Traceability Error Classes

This section defines traceability-specific error classes. These errors allow tools, execution runtimes, and governance systems to report traceability failures consistently.

### 20.1 Traceability Error Record Schema

| Field                 | Type     | Required    | Description                 |
| --------------------- | -------- | ----------- | --------------------------- |
| error-id              | string   | REQUIRED    | Unique error identifier     |
| error-class           | enum     | REQUIRED    | Error class from §20.2      |
| severity              | enum     | REQUIRED    | blocking, high, medium, low |
| node-id               | string   | CONDITIONAL | Related node identifier     |
| edge-id               | string   | CONDITIONAL | Related edge identifier     |
| artifact-id           | string   | CONDITIONAL | Related artifact identifier |
| task-id               | string   | CONDITIONAL | Related task identifier     |
| specification-id      | string   | REQUIRED    | System identifier           |
| specification-version | semver   | REQUIRED    | Specification version       |
| message               | string   | REQUIRED    | Human-readable explanation  |
| required-action       | string   | REQUIRED    | Remediation required        |
| detected-at           | ISO 8601 | REQUIRED    | Time detected               |

### 20.2 Error Classes

| Error Class                             | Description                                        | Default Severity |
| --------------------------------------- | -------------------------------------------------- | ---------------- |
| traceability-invalid-identifier         | Identifier does not conform to required format     | blocking         |
| traceability-missing-node               | Required node is missing                           | blocking         |
| traceability-missing-edge               | Required edge is missing                           | blocking         |
| traceability-unresolved-reference       | Edge source or target cannot be resolved           | blocking         |
| traceability-untraced-artifact          | Artifact lacks required source association         | blocking         |
| traceability-orphan-node                | Node is disconnected from required graph context   | high             |
| traceability-invalid-edge-type          | Edge type is not permitted                         | blocking         |
| traceability-missing-rationale          | Inferred relationship lacks rationale              | high             |
| traceability-snapshot-missing           | Required snapshot was not produced                 | high             |
| traceability-history-overwrite          | Historical traceability record was overwritten     | blocking         |
| traceability-impact-analysis-incomplete | Impact analysis could not determine affected scope | high             |
| traceability-integrity-failed           | Integrity check failed                             | blocking         |

---

## 21.0 Traceability Audit Queries

This section defines the minimum audit queries that a conforming platform MUST support. These queries provide evidence for governance, compliance, debugging, and lifecycle management.

Audit queries must be structured and repeatable. A platform SHOULD expose them through an API, query interface, report generator, or governance dashboard.

### 21.1 Required Audit Queries

| Query                 | Description                                                                            |
| --------------------- | -------------------------------------------------------------------------------------- |
| requirements-coverage | For each requirement, list implementing artifacts and validations                      |
| must-have-coverage    | For each must-have requirement, list implementation and validation evidence            |
| policy-enforcement    | For each policy, list governed targets and evaluation records                          |
| untraced-artifacts    | List artifacts lacking required source associations                                    |
| artifact-origin       | Given an artifact, return originating entities, tasks, invocations, and decisions      |
| change-impact         | Given a specification change, list affected and unaffected artifacts                   |
| decision-history      | Given an artifact or task, list related Decision Records                               |
| repair-history        | Given an artifact, list repair attempts and outcomes                                   |
| validation-history    | Given an artifact or requirement, list validation results                              |
| governance-history    | Given a specification or artifact, list approvals, waivers, escalations, and overrides |
| supersession-history  | Given an artifact, list versions it superseded and versions superseding it             |
| deployment-trace      | Given a deployment artifact, list source infrastructure and operational expectations   |

### 21.2 Query Output Rules

Audit query results MUST include identifiers, relationship types, specification versions, and timestamps.

Audit query results SHOULD indicate whether relationships are direct, transitive, inferred, repaired, superseded, or waived.

Audit query results MUST be exportable in a machine-readable format when required by governance or audit.

---

## 22.0 Traceability and Readiness

This section defines how traceability supports readiness evaluation. Readiness levels depend on evidence, and traceability provides the relationships needed to prove that evidence applies to the correct specification version and entities.

### 22.1 Readiness Traceability Requirements

Before Machine-Valid readiness, the platform MUST verify that:

* canonical entities have valid identifiers
* requirements link to validation criteria
* policies link to governed targets
* interfaces link to services
* data entities link to owning or managing services where required
* workflows link to participating actors or services

Before Autonomous-Ready readiness, the platform MUST verify that:

* must-have requirements have implementation targets or planned construction tasks
* validation coverage is traceable
* governance approvals reference the correct specification version
* risk tier records reference the Project entity
* traceability snapshot exists

### 22.2 Readiness Failure

A readiness transition MUST fail if required traceability evidence is missing.

Readiness overrides involving traceability gaps MUST be approved by governance and MUST define compensating controls.

---

## 23.0 Traceability and Governance

This section defines how traceability integrates with governance. Governance decisions must be traceable to the specification, artifacts, tasks, policies, or lifecycle events they affect.

Traceability allows governance authorities to understand exactly what was approved, waived, blocked, escalated, or overridden.

### 23.1 Governance Traceability Requirements

Governance events MUST link to affected objects, including:

* specification versions
* readiness transitions
* policies
* construction tasks
* artifacts
* validation results
* repair records
* deployment artifacts

### 23.2 Waiver Traceability

A waiver MUST link to:

* policy or criterion waived
* affected specification entity or artifact
* justification
* compensating controls
* approving authority
* expiry date

### 23.3 Escalation Traceability

An escalation MUST link to:

* task, artifact, validation result, or repair record causing escalation
* governance event receiving escalation
* resolution decision
* resumed or halted execution state

---

## 24.0 Traceability and Standards Alignment

This section defines alignment with external provenance and audit concepts. The Traceability Model is designed to align with provenance principles without requiring a specific storage technology.

### 24.1 W3C PROV-DM Alignment

The traceability model SHOULD support mapping to W3C PROV-DM concepts.

| ISL Traceability Concept | PROV-DM Alignment |
| ------------------------ | ----------------- |
| Artifact                 | Entity            |
| SpecificationEntity      | Entity            |
| ConstructionTask         | Activity          |
| ToolInvocation           | Activity          |
| ModelInvocation          | Activity          |
| GovernanceEvent          | Activity          |
| Agent or Tool            | Agent             |
| produces                 | wasGeneratedBy    |
| derived-from             | wasDerivedFrom    |
| generated-by             | wasGeneratedBy    |
| reviewed-by              | wasAssociatedWith |
| authorizes               | wasAssociatedWith |

### 24.2 Standards Mapping Rule

Where a platform claims standards alignment, it MUST be able to export or map traceability records into the claimed standard’s conceptual structure.

If a platform diverges from a mapped standard, the divergence MUST be documented.

---

## 25.0 Traceability Storage Requirements

This section defines storage requirements for traceability records. Traceability records must be durable, queryable, and protected from unauthorized modification.

The storage implementation is not prescribed. A platform may use graph databases, relational databases, document stores, event logs, or hybrid mechanisms if the required semantics are preserved.

### 25.1 Storage Requirements

Traceability storage MUST support:

* durable persistence
* lookup by identifier
* graph traversal
* versioned snapshots
* immutable historical records
* query by specification version
* query by artifact
* query by requirement
* query by policy
* export for audit

### 25.2 Integrity Protection

Traceability records SHOULD include integrity mechanisms such as hashes, append-only event logs, signed records, or immutable storage where required by governance.

Historical traceability records MUST NOT be silently modified or deleted.

### 25.3 Retention

Traceability retention MUST satisfy governance and audit requirements.

At minimum, traceability records for a specification version that reached Autonomous-Ready MUST be retained for the life of the generated system unless governance defines a longer retention period.

---

## 26.0 Traceability Lifecycle and Evolution

This section defines how traceability evolves as specifications and artifacts change. Software systems evolve over time, and traceability must preserve both current and historical truth.

Traceability must show not only what exists now, but how it got there.

### 26.1 Specification Version Evolution

When a specification version changes, the platform MUST preserve traceability for the prior version.

The new version MUST produce an additive traceability layer or new snapshot.

Traceability for prior versions MUST remain queryable.

### 26.2 Artifact Evolution

When an artifact changes, the platform MUST preserve the prior artifact version or metadata sufficient to identify it.

New artifact versions MUST be linked to prior versions using `supersedes` relationships.

A repaired artifact MUST be traceable to the validation failure and repair record that caused the change.

### 26.3 Deprecation

Deprecated entities or artifacts MUST remain traceable.

Deprecation MUST NOT delete historical traceability records.

A deprecated artifact MUST identify its replacement when superseded.

---

## 27.0 Conformance Requirements

This section defines conformance requirements for traceability graphs, traceability engines, and platforms.

### 27.1 Traceability Graph Conformance

A traceability graph conforms to ISL v1.4 if it:

* contains required node and edge types
* uses valid identifiers
* resolves all required references
* supports bidirectional traversal
* preserves specification version context
* records artifact associations
* records validation, repair, and governance relationships
* passes required integrity checks

### 27.2 Traceability Engine Conformance

A traceability engine conforms to ISL v1.4 if it can:

* create traceability nodes and edges at lifecycle event time
* enforce required artifact associations
* enforce identifier rules
* perform bidirectional traceability queries
* perform impact analysis
* create traceability snapshots
* perform integrity checks
* report traceability errors
* preserve historical traceability
* support required audit queries

### 27.3 Platform Conformance

A platform conforms to ISL v1.4 if it:

* integrates traceability with readiness evaluation
* integrates traceability with construction planning
* integrates traceability with execution runtime
* integrates traceability with deterministic validation
* integrates traceability with governance events
* blocks stable artifact acceptance for untraced artifacts
* prevents execution completion when required traceability is incomplete
* preserves traceability across specification versions

---

## 28.0 Summary

ISL v1.4 defines traceability as a required structural property of the IMHOTEP autonomous construction lifecycle. It establishes how specification entities, construction tasks, artifacts, validations, repairs, decisions, tool invocations, model invocations, governance events, and deployment artifacts are connected through a persistent traceability graph.

This revision strengthens the traceability model by making identifiers, node types, edge types, artifact associations, impact analysis, decision records, integrity checks, audit queries, storage behavior, and lifecycle evolution explicit.

A conforming platform MUST treat traceability as a condition of trust. If an artifact cannot be traced, it cannot be accepted as a valid product of the ISL construction process.
