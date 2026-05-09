# IMHOTEP Specification Language (ISL) v1.1

# The Canonical Semantic Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0
**Supersedes:** ISL r1 v1.1 Canonical Semantic Model
**Document Type:** Canonical Model Specification

---

## 1.0 Scope

This document defines the canonical semantic model used by the IMHOTEP Specification Language. The canonical semantic model is the authoritative machine-interpretable representation of an authored ISL specification after parsing and normalization. It defines the entity types, required fields, relationships, identifier rules, validation constraints, and normalization behavior required for a specification to become Machine-Valid.

This document does not define the human-authored Markdown syntax of ISL; that responsibility belongs to ISL v1.0. It also does not define runtime execution, construction planning, artifact generation, repair cycles, or platform deployment. Its responsibility is narrower and more foundational: it defines the internal semantic graph that all later ISL processes rely on.

A conforming implementation MUST use this canonical model as the authoritative representation for:

* semantic validation
* readiness evaluation
* construction planning
* traceability
* governance policy evaluation
* impact analysis
* artifact derivation
* model and agent context assembly

---

## 2.0 Purpose of the Canonical Semantic Model

The purpose of the canonical semantic model is to transform authored specifications into a structured, normalized, machine-checkable representation. Human-authored specifications may contain prose, tables, headings, and explanatory material. The canonical model removes ambiguity by converting that content into typed entities, explicit fields, and declared relationships.

The canonical model is the semantic bridge between specification authoring and autonomous construction. Without a canonical model, the platform would have to reason directly over authored documents, which would make validation inconsistent, traceability unreliable, and construction planning non-deterministic.

The canonical model SHALL serve as:

* the authoritative internal representation of the system specification
* the source graph used by the Planning Engine
* the source graph used by the Traceability Engine
* the source graph used by readiness evaluation
* the source graph used by governance and policy validation
* the semantic context source for reasoning agents

---

## 3.0 Normative References

This section identifies the ISL documents and external standards required to interpret the canonical semantic model. The model depends directly on ISL v1.0 because v1.0 defines the authored language surface that is normalized into this model.

The model also depends on external standards where those standards provide controlled vocabulary or conceptual alignment. ISL may impose stricter requirements than those standards when required for autonomous construction.

| Reference      | Purpose                                                         |
| -------------- | --------------------------------------------------------------- |
| ISL v0.0       | Corpus-level conformance, terminology, and governing principles |
| ISL v1.0       | Authored specification language and section structure           |
| ISL v1.3       | Readiness levels and canonical validation requirements          |
| ISL v1.4       | Traceability graph and artifact association rules               |
| ISL v1.7       | Governance roles, policies, approvals, and auditability         |
| RFC 2119       | Normative requirement terminology                               |
| ISO/IEC 25010  | Quality characteristic classification                           |
| NIST SP 800-53 | Security and privacy control references                         |
| W3C PROV-DM    | Provenance and traceability alignment                           |

---

## 4.0 Terms and Definitions

This section defines terminology specific to the canonical model. These definitions are required so that implementations use consistent semantics when parsing, validating, storing, and exchanging canonical ISL specifications.

The terms in this section apply to normalized specification content. Authored-language terminology is defined in ISL v1.0.

**Canonical Model**
The normalized, machine-interpretable representation of an ISL specification.

**Canonical Entity**
A typed object in the canonical model representing a specification concept such as a Requirement, Service, Interface, Policy, or Validation.

**Canonical Relationship**
A typed link between two canonical entities.

**Normalization**
The process of transforming an authored ISL specification into the canonical model.

**Specification Graph**
The directed graph formed by canonical entities and their relationships.

**Semantic Validation**
Validation that checks whether canonical entities, fields, identifiers, relationships, and constraints satisfy ISL requirements.

**Entity Type**
A defined category of canonical entity.

**Traceability Identifier**
A stable identifier assigned to a canonical entity and used for lifecycle traceability.

**Reference Resolution**
The process of verifying that all declared relationships and identifier references point to existing canonical entities.

**Canonical Error**
A validation or normalization failure produced while constructing or evaluating the canonical model.

---

## 5.0 Model Design Principles

This section defines the design principles that govern the canonical semantic model. These principles ensure that the model can support autonomous construction rather than merely represent documentation.

A canonical model that violates these principles MUST NOT be accepted as Machine-Valid.

### 5.1 Explicit Semantics

Every canonical entity MUST have an explicit type, identifier, name, description, version, and applicable type-specific fields. The platform MUST NOT infer core entity type or identity from prose.

Explicit semantics allow tools, agents, and governance processes to operate over the specification without relying on ambiguous natural-language interpretation.

### 5.2 Machine Checkability

Every required field, enum value, relationship, and constraint MUST be checkable by deterministic validation logic. A canonical model MUST NOT depend on subjective interpretation for readiness progression.

Reasoning agents MAY assist with interpretation, but their conclusions MUST be represented as structured canonical entities or validation records before being used by the platform.

### 5.3 Stable Identity

Every canonical entity MUST carry a stable traceability identifier. This identifier is used across specification versions, construction tasks, generated artifacts, validation results, governance records, and audit history.

Entity identifiers MUST remain stable even when entity names or descriptions change.

### 5.4 Relationship Integrity

Relationships between canonical entities MUST be explicit, typed, and resolvable. A relationship that references a missing or invalid entity MUST cause semantic validation failure.

The canonical graph MUST be internally consistent before it can be used for construction planning.

### 5.5 Specification Completeness

The canonical model MUST represent enough system intent to support readiness evaluation, planning, validation, and governance. Partial canonical models MAY exist during Draft readiness, but a specification MUST satisfy the required entity and relationship rules before Machine-Valid readiness.

### 5.6 Extensibility Under Control

The canonical model MAY be extended through declared extensions. Extensions MUST NOT redefine core entity types, weaken required fields, bypass validation requirements, or break canonical relationship semantics.

---

## 6.0 Canonical Model Overview

The canonical semantic model is organized as a graph of typed entities. Each entity describes a specific aspect of the system being specified. Relationships define how these aspects depend on, govern, validate, implement, expose, or group one another.

The model uses thirteen core entity types. These entity types collectively represent the minimum semantic vocabulary required to describe a system for autonomous construction.

| Entity Type    | Purpose                                                      |
| -------------- | ------------------------------------------------------------ |
| Project        | Root container for the specification                         |
| Context        | External environment, assumptions, and dependencies          |
| Stakeholder    | Party with interest, responsibility, or approval authority   |
| Actor          | Runtime participant that interacts with the system           |
| Requirement    | Verifiable behavior or constraint                            |
| Capability     | Higher-level functional grouping                             |
| Service        | Deployable or logical system component                       |
| DataEntity     | Structured information managed by the system                 |
| Workflow       | Multi-step behavior involving actors, services, or decisions |
| Interface      | Communication boundary exposed or consumed by services       |
| Policy         | Rule governing security, compliance, operation, or risk      |
| Infrastructure | Runtime and deployment expectation                           |
| Validation     | Criteria proving that specification intent is satisfied      |

A canonical model MUST contain exactly one Project entity. Other entity types MAY occur one or more times depending on the scope of the system and readiness level.

---

## 7.0 Canonical Graph Structure

This section defines how canonical entities form a graph. The graph structure is critical because downstream systems do not process isolated entities; they process relationships between system intent, architecture, behavior, policy, validation, and operational expectations.

The canonical graph enables construction planning, traceability, governance analysis, and impact analysis. It also allows the platform to determine whether requirements are implemented, whether policies govern the right targets, and whether validation covers the system adequately.

### 7.1 Graph Definition

The canonical model SHALL be represented as a directed graph.

| Graph Element | Description                                                                           |
| ------------- | ------------------------------------------------------------------------------------- |
| Node          | A canonical entity                                                                    |
| Edge          | A canonical relationship                                                              |
| Root Node     | The Project entity                                                                    |
| Subgraph      | A connected subset of entities relevant to a capability, service, workflow, or policy |

### 7.2 Graph Rules

A canonical graph MUST satisfy the following rules:

* Exactly one Project entity MUST exist.
* Every non-Project entity MUST be reachable from the Project entity through one or more relationships.
* Every relationship target MUST resolve to an existing entity.
* Relationship types MUST be drawn from the relationship types defined in this document.
* The graph MUST NOT contain circular dependencies unless the relationship type explicitly permits cyclical structure.
* The graph MUST support traversal from requirements to implementing services and validations.
* The graph MUST support traversal from policies to governed entities.
* The graph MUST support traversal from data entities to owning or managing services.

### 7.3 Root Relationship Requirement

Every canonical entity MUST be associated with the Project entity either directly or indirectly. This ensures that no orphan semantic structures exist in the specification.

A canonical processor MUST reject a Machine-Valid candidate if any entity is disconnected from the Project graph.

---

## 8.0 Base Entity Schema

This section defines the schema that all canonical entities MUST satisfy. The base schema provides uniform identity, typing, naming, versioning, and relationship behavior across the entire model.

Type-specific schemas extend this base schema. They do not replace it.

### 8.1 Base Entity Fields

| Field          | Type   | Required    | Description                                            |
| -------------- | ------ | ----------- | ------------------------------------------------------ |
| id             | string | REQUIRED    | Traceability identifier conforming to §22.0            |
| type           | enum   | REQUIRED    | One of the thirteen canonical entity types             |
| name           | string | REQUIRED    | Human-readable entity name                             |
| description    | string | REQUIRED    | Purpose or meaning of the entity                       |
| version        | semver | REQUIRED    | Entity version                                         |
| relationships  | array  | CONDITIONAL | Explicit relationships to other entities               |
| source-section | string | REQUIRED    | Authored ISL section from which the entity was derived |
| status         | enum   | REQUIRED    | active, deprecated, superseded, draft                  |
| metadata       | object | OPTIONAL    | Implementation-neutral supplemental metadata           |

### 8.2 Base Entity Rules

Every canonical entity MUST include all REQUIRED base fields.

The `id` field MUST be immutable after assignment.

The `type` field MUST match one of the canonical entity types defined in §6.0.

The `description` field MUST be meaningful enough to support human review and audit. A description consisting only of a repeated name, placeholder, or empty string MUST fail semantic validation.

The `status` field MUST be set to `active` unless the entity is explicitly marked otherwise.

### 8.3 Entity Status Values

| Status     | Meaning                                                            |
| ---------- | ------------------------------------------------------------------ |
| active     | Entity is current and valid for use                                |
| draft      | Entity exists but is not complete                                  |
| deprecated | Entity remains traceable but MUST NOT be used for new construction |
| superseded | Entity has been replaced by a newer entity                         |

A deprecated or superseded entity MUST retain its identifier and MUST NOT be deleted from version history.

---

## 9.0 Base Relationship Schema

This section defines the common schema for relationships between canonical entities. Relationships are first-class semantic structures because they define how system intent becomes architecture, validation, governance, and construction behavior.

Relationships MUST be explicit and typed. A platform MUST NOT depend on implicit references hidden in prose once the specification has been normalized.

### 9.1 Relationship Fields

| Field             | Type    | Required    | Description                                                     |
| ----------------- | ------- | ----------- | --------------------------------------------------------------- |
| relationship-id   | string  | REQUIRED    | Unique identifier for the relationship                          |
| source-id         | string  | REQUIRED    | Entity identifier where the relationship originates             |
| target-id         | string  | REQUIRED    | Entity identifier where the relationship points                 |
| relationship-type | enum    | REQUIRED    | Relationship type defined in §9.2                               |
| cardinality       | enum    | CONDITIONAL | one-to-one, one-to-many, many-to-one, many-to-many              |
| required          | boolean | REQUIRED    | Whether the relationship is mandatory                           |
| rationale         | string  | CONDITIONAL | Required when relationship is inferred                          |
| source-section    | string  | REQUIRED    | Authored section or canonical process creating the relationship |

### 9.2 Relationship Types

| Relationship Type | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| contains          | Project includes another canonical entity           |
| depends-on        | Source requires target to be present or complete    |
| implements        | Source provides behavior required by target         |
| validates         | Source verifies target                              |
| governs           | Source constrains target                            |
| exposes           | Source makes target available through a boundary    |
| consumes          | Source uses target as input                         |
| produces          | Source creates target as output                     |
| groups            | Source aggregates target entities                   |
| manages           | Source owns or controls target data                 |
| participates-in   | Source participates in target workflow              |
| triggered-by      | Source begins when target event or condition occurs |
| constrained-by    | Source is limited by target                         |
| derived-from      | Source was normalized or inferred from target       |
| supersedes        | Source replaces target                              |

### 9.3 Relationship Rules

All relationship references MUST resolve to existing canonical entity identifiers.

Relationship direction MUST be meaningful. For example, a Service may implement a Requirement, but a Requirement MUST NOT implement a Service.

A relationship marked `required` MUST be satisfied before the entity can be considered semantically complete.

Inferred relationships MUST include a rationale.

A relationship that cannot be validated MUST be recorded as a canonical validation error.

---

## 10.0 Project Entity

The Project entity is the root container for the entire canonical specification. It represents the system as a governed specification artifact and provides the identity anchor for all other entities.

Exactly one Project entity MUST exist in every canonical model. All other entities are interpreted within the context of this Project entity.

### 10.1 Purpose

The Project entity establishes the specification’s identity, ownership, readiness level, version, governance posture, and lifecycle state. It is the entity most directly connected to readiness evaluation and governance approval.

Without a valid Project entity, the platform cannot determine what system is being described, who owns it, which ISL version applies, or whether construction may proceed.

### 10.2 Schema

| Field                 | Type     | Required    | Description                                        |
| --------------------- | -------- | ----------- | -------------------------------------------------- |
| system-id             | string   | REQUIRED    | Stable system identifier                           |
| readiness-level       | enum     | REQUIRED    | draft, reviewable, machine-valid, autonomous-ready |
| isl-version           | string   | REQUIRED    | ISL version governing the specification            |
| owner                 | string   | REQUIRED    | Responsible organization or unit                   |
| domain                | string   | REQUIRED    | Business or technical domain                       |
| created               | ISO 8601 | REQUIRED    | Date the specification was first created           |
| last-modified         | ISO 8601 | REQUIRED    | Date of most recent revision                       |
| risk-tier             | enum     | CONDITIONAL | Required before Autonomous-Ready                   |
| governance-profile    | string   | CONDITIONAL | Required when governance policies apply            |
| specification-version | semver   | REQUIRED    | Version of the specification                       |

### 10.3 Rules

The Project entity MUST be the root of the canonical graph.

The `system-id` MUST match the authored System Identity section defined in ISL v1.0.

The `readiness-level` MUST use the readiness values defined in ISL v1.3.

The Project entity MUST contain or reference every other entity in the specification graph.

The Project entity MUST NOT be deprecated while the specification remains active.

---

## 11.0 Context Entity

The Context entity describes the external environment in which the system operates. It captures assumptions, dependencies, constraints, external systems, organizational boundaries, and operational conditions that influence interpretation of the specification.

Context is essential because autonomous construction cannot safely infer environmental conditions. If the specification depends on external identity providers, third-party APIs, regulatory boundaries, legacy systems, or organizational constraints, those dependencies MUST be explicit.

### 11.1 Schema

| Field                   | Type   | Required    | Description                                                        |
| ----------------------- | ------ | ----------- | ------------------------------------------------------------------ |
| external-systems        | array  | CONDITIONAL | External systems depended on or integrated with                    |
| integration-patterns    | array  | CONDITIONAL | sync-request-reply, async-event, batch, streaming                  |
| environment-constraints | array  | CONDITIONAL | Regulatory, geographic, operational, or organizational constraints |
| assumptions             | array  | CONDITIONAL | Assumptions required for interpretation                            |
| exclusions              | array  | CONDITIONAL | Explicit responsibilities outside the system boundary              |
| domain-context          | string | CONDITIONAL | Business or technical context affecting system behavior            |

### 11.2 Rules

A Context entity MUST be present when the authored specification declares external dependencies, assumptions, exclusions, or environmental constraints.

External systems referenced by actors, services, interfaces, or workflows MUST be represented in the Context entity or as Actor entities of type `external`.

Assumptions MUST NOT be used to bypass required fields in other canonical entities.

An assumption that materially affects construction, validation, governance, or deployment MUST be linked to the affected entities through relationships.

---

## 12.0 Stakeholder Entity

The Stakeholder entity represents a person, role, organization, governance authority, or group with an interest in the system. Stakeholders may drive requirements, approve readiness transitions, define policy constraints, or own operational outcomes.

Stakeholders are not necessarily runtime users. Their primary importance is that they establish source authority, review responsibility, governance participation, and traceable business motivation.

### 12.1 Schema

| Field              | Type    | Required    | Description                                                                                                            |
| ------------------ | ------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- |
| role               | string  | REQUIRED    | Organizational, business, or governance role                                                                           |
| concerns           | array   | REQUIRED    | Outcomes, quality attributes, risks, or responsibilities                                                               |
| approval-authority | boolean | REQUIRED    | Whether the stakeholder can approve readiness or governance actions                                                    |
| authority-type     | enum    | CONDITIONAL | reviewer, security-reviewer, architecture-reviewer, authorizing-official, governance-administrator, override-authority |
| organization       | string  | CONDITIONAL | Organization or unit represented                                                                                       |
| contact-reference  | string  | OPTIONAL    | Non-sensitive reference to responsible party or role                                                                   |

### 12.2 Rules

Every Requirement MUST trace to at least one Stakeholder, Actor, or Objective-derived source.

A Stakeholder with `approval-authority` set to true MUST map to a governance role defined in ISL v1.7.

A single Stakeholder MUST NOT be assigned incompatible approval roles for the same specification when separation-of-duties rules apply.

Stakeholder concerns SHOULD be used to classify requirements, policies, and validation criteria.

---

## 13.0 Actor Entity

The Actor entity represents an entity that interacts with the system at runtime. Actors may be humans, systems, processes, or external parties.

Actors are necessary for defining permissions, interfaces, workflows, use cases, and access control policies. A system behavior involving an external participant MUST identify that participant as an Actor unless the participant is represented as an internal Service.

### 13.1 Schema

| Field                   | Type    | Required    | Description                                            |
| ----------------------- | ------- | ----------- | ------------------------------------------------------ |
| actor-type              | enum    | REQUIRED    | human, system, process, external                       |
| permissions             | array   | REQUIRED    | Operations the actor is permitted to perform           |
| interfaces-used         | array   | CONDITIONAL | Interface identifiers used by the actor                |
| authentication-required | boolean | REQUIRED    | Whether authenticated identity is required             |
| authorization-scope     | array   | CONDITIONAL | Permission scopes, roles, or policies governing access |
| data-access             | array   | CONDITIONAL | DataEntity identifiers the actor may access            |

### 13.2 Rules

Every Actor MUST define at least one permission.

If an Actor uses an Interface, the referenced Interface entity MUST exist.

If `authentication-required` is true, at least one Policy or Interface authentication rule MUST govern the Actor’s access path.

Actor permissions MUST be consistent with security policies and workflow actions.

An Actor of type `external` MUST be represented in Context or linked to an external system dependency.

---

## 14.0 Requirement Entity

The Requirement entity represents a verifiable statement of system behavior, quality, constraint, regulation, or operational expectation. Requirements are among the most important canonical entities because they drive capabilities, services, tests, validation, traceability, and governance decisions.

A Requirement MUST be specific enough to validate. If it cannot be validated, it cannot reliably guide autonomous construction.

### 14.1 Schema

| Field                    | Type   | Required    | Description                                                   |
| ------------------------ | ------ | ----------- | ------------------------------------------------------------- |
| requirement-type         | enum   | REQUIRED    | functional, non-functional, regulatory, operational           |
| statement                | string | REQUIRED    | Active-voice verifiable requirement statement                 |
| priority                 | enum   | REQUIRED    | must-have, should-have, nice-to-have                          |
| source                   | array  | REQUIRED    | Actor, Stakeholder, Objective, Policy, or Context identifiers |
| acceptance-criteria      | array  | REQUIRED    | Validation entity identifiers                                 |
| iso-25010-characteristic | string | CONDITIONAL | Required for non-functional requirements                      |
| metric                   | string | CONDITIONAL | Required when measurable                                      |
| target                   | string | CONDITIONAL | Required when metric is present                               |
| rationale                | string | OPTIONAL    | Explanation of why the requirement exists                     |

### 14.2 Requirement Types

| Type           | Meaning                                                 |
| -------------- | ------------------------------------------------------- |
| functional     | Behavior the system MUST provide                        |
| non-functional | Quality attribute or constraint                         |
| regulatory     | Compliance or legal requirement                         |
| operational    | Deployment, runtime, monitoring, or support requirement |

### 14.3 Rules

Every Requirement MUST be verifiable.

Every Requirement MUST have at least one source.

Every must-have Requirement MUST link to at least one Validation entity before Autonomous-Ready readiness.

A non-functional Requirement MUST include an ISO/IEC 25010-aligned characteristic unless explicitly justified as outside that model.

Requirement statements MUST NOT contain ambiguous language that prevents validation.

A Requirement with priority `must-have` MUST NOT be ignored, deferred, or treated as optional during planning.

---

## 15.0 Capability Entity

The Capability entity groups related requirements into a higher-level function or business capability. Capabilities are useful for planning, architecture, traceability, prioritization, and communication with stakeholders.

Capabilities help bridge the gap between individual requirements and system architecture. A Service may implement one or more Capabilities, and a Capability may group many Requirements.

### 15.1 Schema

| Field           | Type   | Required    | Description                                           |
| --------------- | ------ | ----------- | ----------------------------------------------------- |
| requirements    | array  | REQUIRED    | Requirement identifiers grouped under this capability |
| implemented-by  | array  | CONDITIONAL | Service identifiers implementing the capability       |
| capability-type | enum   | OPTIONAL    | business, technical, operational, governance          |
| priority        | enum   | CONDITIONAL | must-have, should-have, nice-to-have                  |
| owner           | string | CONDITIONAL | Stakeholder or organizational owner                   |

### 15.2 Rules

A Capability MUST group at least one Requirement.

A Capability SHOULD be implemented by at least one Service before Autonomous-Ready readiness.

A Capability MUST NOT introduce behavior that is not represented by one or more Requirements.

Capability priority MUST NOT be lower than the highest priority Requirement it contains unless explicitly justified.

---

## 16.0 Service Entity

The Service entity represents a logical or deployable component responsible for implementing requirements, exposing interfaces, managing data, and participating in workflows.

Service definitions are critical to construction planning because they determine system decomposition, artifact generation, dependencies, interface contracts, and deployment preparation.

### 16.1 Schema

| Field           | Type   | Required    | Description                                      |
| --------------- | ------ | ----------- | ------------------------------------------------ |
| responsibility  | string | REQUIRED    | Single-sentence responsibility statement         |
| interfaces      | array  | REQUIRED    | Interface identifiers exposed or consumed        |
| dependencies    | array  | CONDITIONAL | Service, Context, or external system identifiers |
| data-entities   | array  | CONDITIONAL | DataEntity identifiers managed or used           |
| capabilities    | array  | CONDITIONAL | Capability identifiers implemented               |
| requirements    | array  | REQUIRED    | Requirement identifiers implemented              |
| deployment-unit | string | CONDITIONAL | Logical deployment grouping                      |
| statefulness    | enum   | REQUIRED    | stateless, stateful, hybrid                      |

### 16.2 Rules

A Service MUST have exactly one primary responsibility statement.

A Service MUST implement or support at least one Requirement.

A Service MUST NOT declare a responsibility that substantially overlaps another Service unless the overlap is explicitly justified.

A Service exposing an Interface MUST reference a valid Interface entity.

A Service managing a DataEntity MUST be linked using a `manages` relationship.

A Service depending on another Service MUST use a `depends-on` relationship.

---

## 17.0 DataEntity Entity

The DataEntity entity represents structured information managed, stored, transmitted, validated, or transformed by the system. DataEntity definitions support schema generation, API contracts, validation logic, persistence design, security policy enforcement, and impact analysis.

A DataEntity MUST be sufficiently structured for deterministic validation. Vague data descriptions are not acceptable in a Machine-Valid specification.

### 17.1 Schema

| Field            | Type   | Required    | Description                                            |
| ---------------- | ------ | ----------- | ------------------------------------------------------ |
| attributes       | array  | REQUIRED    | Attribute definitions                                  |
| relationships    | array  | CONDITIONAL | Relationships to other DataEntity instances            |
| sensitivity      | enum   | REQUIRED    | public, internal, confidential, restricted             |
| lifecycle        | enum   | OPTIONAL    | transient, persisted, archived, derived                |
| owner-service    | string | CONDITIONAL | Service identifier responsible for managing the entity |
| retention-policy | string | CONDITIONAL | Required for persisted sensitive data                  |

### 17.2 Attribute Schema

| Field       | Type    | Required    | Description                                    |
| ----------- | ------- | ----------- | ---------------------------------------------- |
| name        | string  | REQUIRED    | Attribute name                                 |
| data-type   | string  | REQUIRED    | Primitive, structured, enum, or reference type |
| nullable    | boolean | REQUIRED    | Whether null values are permitted              |
| constraints | array   | CONDITIONAL | Validation rules                               |
| default     | string  | OPTIONAL    | Default value when applicable                  |
| description | string  | CONDITIONAL | Attribute meaning                              |

### 17.3 Relationship Schema

| Field             | Type    | Required | Description                                        |
| ----------------- | ------- | -------- | -------------------------------------------------- |
| target-entity-id  | string  | REQUIRED | Related DataEntity identifier                      |
| relationship-type | string  | REQUIRED | Nature of the relationship                         |
| cardinality       | enum    | REQUIRED | one-to-one, one-to-many, many-to-one, many-to-many |
| required          | boolean | REQUIRED | Whether relationship must exist                    |

### 17.4 Rules

A DataEntity MUST contain at least one attribute.

Every attribute MUST declare `data-type` and `nullable`.

Relationships between DataEntity entities MUST specify cardinality.

A DataEntity with sensitivity `confidential` or `restricted` MUST be governed by at least one Policy entity.

A persisted DataEntity with sensitivity `confidential` or `restricted` MUST define or reference a retention policy before Machine-Valid readiness.

---

## 18.0 Workflow Entity

The Workflow entity represents ordered behavior involving actors, services, decisions, events, and exception paths. Workflows define how the system behaves over time and across participants.

Workflows are required when behavior cannot be fully described by isolated requirements. They are especially important for process-heavy systems, multi-service systems, approval flows, asynchronous behavior, and exception handling.

### 18.1 Schema

| Field           | Type   | Required    | Description                                        |
| --------------- | ------ | ----------- | -------------------------------------------------- |
| trigger         | string | REQUIRED    | Event or condition initiating the workflow         |
| actors          | array  | CONDITIONAL | Actor identifiers participating                    |
| services        | array  | CONDITIONAL | Service identifiers participating                  |
| steps           | array  | REQUIRED    | Ordered step definitions                           |
| decision-points | array  | CONDITIONAL | Branching conditions                               |
| exception-paths | array  | REQUIRED    | Failure and exception handling paths               |
| validations     | array  | CONDITIONAL | Validation identifiers verifying workflow behavior |
| terminal-states | array  | REQUIRED    | Valid end states of the workflow                   |

### 18.2 Step Schema

| Field             | Type    | Required    | Description                         |
| ----------------- | ------- | ----------- | ----------------------------------- |
| sequence          | integer | REQUIRED    | Step order                          |
| participant-id    | string  | REQUIRED    | Actor or Service responsible        |
| action            | string  | REQUIRED    | Action performed                    |
| input             | string  | CONDITIONAL | Input required                      |
| output            | string  | CONDITIONAL | Output produced                     |
| success-condition | string  | REQUIRED    | Condition for step completion       |
| failure-condition | string  | CONDITIONAL | Condition triggering exception path |

### 18.3 Decision Point Schema

| Field       | Type   | Required | Description                       |
| ----------- | ------ | -------- | --------------------------------- |
| decision-id | string | REQUIRED | Unique decision identifier        |
| condition   | string | REQUIRED | Condition evaluated               |
| true-path   | string | REQUIRED | Step or terminal state when true  |
| false-path  | string | REQUIRED | Step or terminal state when false |

### 18.4 Rules

A Workflow MUST contain at least one step.

A Workflow MUST define exception paths.

A Workflow MUST define at least one terminal state.

Every participant referenced by a Workflow MUST resolve to an Actor or Service.

Decision points MUST define explicit conditions.

A Workflow without exception paths MUST fail Machine-Valid evaluation.

---

## 19.0 Interface Entity

The Interface entity represents a communication boundary exposed or consumed by a Service. Interfaces define how actors, services, external systems, or processes interact with system behavior.

Interfaces are required for implementation generation, security validation, contract testing, compatibility review, and deployment preparation.

### 19.1 Schema

| Field               | Type   | Required    | Description                                                      |
| ------------------- | ------ | ----------- | ---------------------------------------------------------------- |
| interface-type      | enum   | REQUIRED    | rest-api, grpc, message-queue, event-stream, batch-file, graphql |
| contract            | string | REQUIRED    | Inline contract or reference to contract document                |
| authentication      | string | REQUIRED    | Authentication mechanism                                         |
| authorization       | string | CONDITIONAL | Authorization policy or scope                                    |
| versioning-strategy | string | REQUIRED    | How breaking changes are handled                                 |
| exposed-by          | string | REQUIRED    | Service identifier exposing the interface                        |
| consumed-by         | array  | CONDITIONAL | Actors, services, or external systems using the interface        |
| data-entities       | array  | CONDITIONAL | DataEntity identifiers transmitted through interface             |

### 19.2 Rules

An Interface MUST define a contract.

An Interface MUST define authentication.

If authorization differs by actor or operation, the Interface MUST reference a Policy entity governing authorization.

An Interface MUST be exposed by exactly one Service unless it is an external interface represented by Context.

Interface contracts SHOULD use established formats where applicable, such as OpenAPI for REST APIs or AsyncAPI for event-driven interfaces.

Breaking-change behavior MUST be defined through the `versioning-strategy` field.

---

## 20.0 Policy Entity

The Policy entity represents a verifiable rule governing security, compliance, operations, technology standards, risk, data handling, or governance behavior. Policies constrain both the specified system and the autonomous construction process.

Policy entities are essential because autonomous construction must operate inside enterprise control boundaries. A policy that cannot be verified is not enforceable and therefore cannot serve as a governance control.

### 20.1 Schema

| Field              | Type    | Required    | Description                                                                 |
| ------------------ | ------- | ----------- | --------------------------------------------------------------------------- |
| policy-type        | enum    | REQUIRED    | security, compliance, operational, data-handling, technology-standard, risk |
| rule               | string  | REQUIRED    | Verifiable rule statement                                                   |
| control-reference  | string  | CONDITIONAL | External control reference such as NIST SP 800-53                           |
| enforcement-point  | enum    | REQUIRED    | authoring, planning, generation, validation, deployment, runtime            |
| applies-to         | array   | REQUIRED    | Entity identifiers governed by the policy                                   |
| violation-response | enum    | REQUIRED    | block, escalate, warn, audit-only                                           |
| waiver-permitted   | boolean | REQUIRED    | Whether waiver may be granted                                               |
| validation         | array   | CONDITIONAL | Validation identifiers proving compliance                                   |

### 20.2 Rules

A Policy MUST contain a verifiable rule.

Intent-only policy statements MUST fail semantic validation.

A Policy with `violation-response` set to `block` MUST prevent progression at its enforcement point until resolved or waived.

A security Policy SHOULD include a control-reference to NIST SP 800-53 or an equivalent control framework.

A Policy MUST reference at least one entity in `applies-to`.

If waiver-permitted is true, governance rules in ISL v1.7 determine approval requirements.

---

## 21.0 Infrastructure Entity

The Infrastructure entity represents runtime environment expectations, deployment constraints, scaling behavior, and operational resource needs. It does not require a specific implementation technology, but it defines the constraints that deployment preparation and validation must satisfy.

Infrastructure entities are necessary for construction because generated systems must be deployable and operable in their intended environments.

### 21.1 Schema

| Field                      | Type   | Required    | Description                                                 |
| -------------------------- | ------ | ----------- | ----------------------------------------------------------- |
| deployment-model           | enum   | REQUIRED    | container, serverless, vm, bare-metal, hybrid               |
| runtime-platform           | string | REQUIRED    | Target runtime platform and minimum version                 |
| resource-requirements      | object | REQUIRED    | CPU, memory, storage, network expectations                  |
| scaling-model              | enum   | REQUIRED    | fixed, horizontal, vertical, auto                           |
| regions                    | array  | CONDITIONAL | Deployment regions when geographic distribution is required |
| environments               | array  | REQUIRED    | dev, test, staging, prod, or equivalent                     |
| observability-requirements | array  | REQUIRED    | Metrics, logs, traces, alerts required                      |
| availability-target        | string | CONDITIONAL | Required when availability is specified                     |
| recovery-objectives        | object | CONDITIONAL | Required for persistent or critical systems                 |

### 21.2 Rules

Every canonical model intended for Autonomous-Ready readiness MUST include at least one Infrastructure entity.

Infrastructure entities MUST define runtime platform, deployment model, resource requirements, and scaling model.

Systems with persisted data MUST define recovery objectives or explicitly justify why recovery objectives are not applicable.

Infrastructure expectations MUST be consistent with non-functional requirements and operational policies.

---

## 22.0 Validation Entity

The Validation entity represents evidence-producing criteria used to verify that requirements, policies, workflows, interfaces, data models, services, and infrastructure expectations are satisfied.

Validation entities convert intent into proof. Without validation entities, the platform cannot determine whether generated artifacts satisfy the specification.

### 22.1 Schema

| Field               | Type   | Required    | Description                                                                                          |
| ------------------- | ------ | ----------- | ---------------------------------------------------------------------------------------------------- |
| validates           | array  | REQUIRED    | Entity identifiers being validated                                                                   |
| validation-type     | enum   | REQUIRED    | test, inspection, static-analysis, security-scan, policy-check, operational-check, governance-review |
| method              | string | REQUIRED    | How validation is performed                                                                          |
| pass-condition      | string | REQUIRED    | Condition required for success                                                                       |
| evidence            | string | CONDITIONAL | Evidence artifact or record expected                                                                 |
| automation-level    | enum   | REQUIRED    | manual, semi-automated, automated                                                                    |
| tool-capability     | string | CONDITIONAL | Tool capability required when automated                                                              |
| severity-on-failure | enum   | REQUIRED    | blocking, high, medium, low                                                                          |

### 22.2 Rules

A Validation entity MUST reference at least one entity through `validates`.

A Validation entity MUST define a pass-condition.

A Validation entity with automation-level `automated` SHOULD identify the tool capability required.

Every must-have Requirement MUST have at least one linked Validation entity before Autonomous-Ready readiness.

Every blocking Policy MUST have at least one linked Validation entity before Machine-Valid readiness.

---

## 23.0 Traceability Identifier Format

This section defines the normative identifier format for canonical entities. Identifiers provide the foundation for traceability across specifications, tasks, artifacts, validations, policies, decisions, governance events, and version history.

Identifier stability is mandatory. Names may change. Descriptions may change. Implementation artifacts may change. The identifier remains the durable semantic anchor.

### 23.1 Identifier Format

Every canonical entity identifier MUST conform to the following format:

`{PREFIX}-{system-id}-{sequence}`

Where:

| Component | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| PREFIX    | Three-character uppercase entity prefix                                  |
| system-id | System identifier from the Project entity, normalized for identifier use |
| sequence  | Zero-padded five-digit integer unique within the entity type             |

Example:

`REQ-acme-order-00001`

### 23.2 Entity Prefixes

| Entity Type    | Prefix |
| -------------- | ------ |
| Project        | PRJ    |
| Context        | CTX    |
| Stakeholder    | STK    |
| Actor          | ACT    |
| Requirement    | REQ    |
| Capability     | CAP    |
| Service        | SVC    |
| DataEntity     | DAT    |
| Workflow       | WFL    |
| Interface      | INT    |
| Policy         | POL    |
| Infrastructure | INF    |
| Validation     | VAL    |

### 23.3 Identifier Rules

Identifiers MUST be immutable after assignment.

Identifiers MUST NOT be reused after deprecation.

Identifiers MUST remain stable across specification versions.

No two active entities of the same type within the same specification MAY share an identifier.

A conforming processor MUST reject a Machine-Valid candidate if any identifier violates the required format.

### 23.4 System ID Normalization

The `system-id` component MUST be derived from the Project `system-id`.

The normalized system-id used in identifiers MUST:

* be lowercase
* contain only letters, numbers, and hyphens
* be no more than twelve characters unless the implementation explicitly supports longer identifiers
* remain stable once identifiers have been issued

---

## 24.0 Canonical Relationship Constraints

This section defines which relationships are permitted between entity types. These constraints prevent invalid semantic graphs, such as requirements implementing services or policies being implemented by data attributes without an intervening service or interface.

Relationship constraints are necessary for deterministic planning and validation.

### 24.1 Permitted Relationship Matrix

| Source Entity  | Permitted Relationship                      | Target Entity                                                                 |
| -------------- | ------------------------------------------- | ----------------------------------------------------------------------------- |
| Project        | contains                                    | Any canonical entity                                                          |
| Context        | constrains, depends-on                      | Project, Service, Interface, Infrastructure                                   |
| Stakeholder    | depends-on, governs                         | Requirement, Policy, Project                                                  |
| Actor          | participates-in, consumes                   | Workflow, Interface                                                           |
| Requirement    | validates, depends-on, constrained-by       | Validation, Requirement, Policy                                               |
| Capability     | groups                                      | Requirement                                                                   |
| Service        | implements, exposes, manages, depends-on    | Requirement, Interface, DataEntity, Service, Context                          |
| DataEntity     | depends-on, constrained-by                  | DataEntity, Policy                                                            |
| Workflow       | triggered-by, consumes, produces, validates | Actor, Service, DataEntity, Validation                                        |
| Interface      | exposes, consumes, constrained-by           | Service, DataEntity, Policy                                                   |
| Policy         | governs, validates                          | Requirement, Service, Interface, DataEntity, Infrastructure, Validation       |
| Infrastructure | constrained-by, supports                    | Service, Policy                                                               |
| Validation     | validates, depends-on                       | Requirement, Policy, Workflow, Interface, Service, DataEntity, Infrastructure |

### 24.2 Required Relationship Rules

The following relationships MUST exist before Machine-Valid readiness:

| Entity      | Required Relationship                                                          |
| ----------- | ------------------------------------------------------------------------------ |
| Requirement | MUST be validated by at least one Validation entity when priority is must-have |
| Service     | MUST implement at least one Requirement or Capability                          |
| Interface   | MUST be exposed by one Service or represented as external Context              |
| Policy      | MUST govern at least one entity                                                |
| DataEntity  | MUST be managed by at least one Service when persisted                         |
| Workflow    | MUST reference at least one Actor or Service                                   |
| Validation  | MUST validate at least one entity                                              |

### 24.3 Invalid Relationship Handling

A relationship that violates the permitted relationship matrix MUST be reported as a canonical validation error.

A processor MUST NOT silently drop invalid relationships.

A processor MAY provide repair suggestions, but the canonical model MUST NOT be marked Machine-Valid until invalid relationships are corrected.

---

## 25.0 Normalization Process

Normalization is the process by which an authored ISL specification becomes a canonical semantic model. This process is essential because authored specifications are designed for human readability, while canonical models are designed for deterministic validation and autonomous construction.

A conforming normalization process MUST be repeatable. The same authored specification and the same ISL version MUST produce the same canonical model unless declared extensions or configuration profiles alter processing behavior.

### 25.1 Normalization Stages

Normalization MUST proceed through the following stages:

| Stage | Name                    | Purpose                                                                          |
| ----- | ----------------------- | -------------------------------------------------------------------------------- |
| 1     | Parse                   | Read authored representation and identify sections, tables, fields, and elements |
| 2     | Extract                 | Extract structured specification elements                                        |
| 3     | Classify                | Assign canonical entity types                                                    |
| 4     | Assign Identifiers      | Validate or assign traceability identifiers                                      |
| 5     | Map Fields              | Populate canonical entity fields                                                 |
| 6     | Resolve References      | Resolve identifiers and relationships                                            |
| 7     | Validate Semantics      | Apply canonical schema and relationship rules                                    |
| 8     | Produce Canonical Graph | Emit canonical model or validation failure report                                |

### 25.2 Parse Stage

The parser MUST identify all required authored sections defined by ISL v1.0.

The parser MUST preserve source location metadata sufficient to report errors back to the authored specification.

If required sections are missing, normalization MUST fail before semantic validation.

### 25.3 Extract Stage

The extractor MUST identify structured elements such as requirements, actors, services, policies, workflows, and validations.

Required fields MUST be extracted from structured fields, not inferred from prose.

Prose MAY be retained as descriptive metadata but MUST NOT replace required canonical fields.

### 25.4 Classify Stage

Each extracted element MUST be assigned one canonical entity type.

If an extracted element could map to more than one entity type, the processor MUST record an ambiguity unless the authored structure clearly resolves the type.

### 25.5 Identifier Stage

Identifiers provided in the authored specification MUST be validated against §23.0.

If Draft readiness permits provisional identifiers, the processor MAY assign temporary identifiers, but those identifiers MUST be replaced with valid traceability identifiers before Machine-Valid readiness.

### 25.6 Field Mapping Stage

Every required canonical field MUST be populated from authored content, derived metadata, or explicitly defined defaults.

The processor MUST NOT invent missing required business semantics.

### 25.7 Reference Resolution Stage

All entity references MUST resolve to known identifiers.

Unresolved references MUST be reported as canonical validation errors.

### 25.8 Semantic Validation Stage

The processor MUST validate:

* base entity schema compliance
* type-specific schema compliance
* required relationship presence
* enum values
* identifier format
* cardinality rules
* policy verifiability
* validation coverage
* readiness-relevant constraints

### 25.9 Canonical Graph Output Stage

If normalization succeeds, the processor MUST produce a canonical graph.

If normalization fails, the processor MUST produce a Normalization Report and MUST NOT emit a Machine-Valid canonical model.

---

## 26.0 Canonical Validation Rules

Canonical validation determines whether the normalized model satisfies the semantic requirements of ISL. Structural validation of the authored document is insufficient; semantic validation checks whether the model is internally meaningful and ready for downstream processing.

A canonical model MUST pass all required validation rules before the specification can reach Machine-Valid readiness.

### 26.1 Required Validation Checks

A conforming canonical validator MUST verify:

* exactly one Project entity exists
* every entity satisfies the base entity schema
* every entity satisfies its type-specific schema
* every identifier conforms to the identifier format
* every required relationship exists
* every relationship target resolves
* every enum value is valid
* every must-have Requirement has Validation coverage
* every Policy contains a verifiable rule
* every Interface defines authentication and contract
* every Workflow defines steps and exception paths
* every DataEntity defines attributes and nullability
* every Service implements a Requirement or Capability
* every persisted sensitive DataEntity is governed by a Policy
* every Infrastructure entity defines deployment model, runtime platform, and resource requirements

### 26.2 Validation Outcomes

Canonical validation MUST produce one of the following outcomes:

| Outcome       | Meaning                                                                  |
| ------------- | ------------------------------------------------------------------------ |
| passed        | Canonical model satisfies all required rules                             |
| failed        | Canonical model has one or more blocking errors                          |
| warning       | Canonical model is valid but contains non-blocking concerns              |
| not-evaluated | Validation could not run due to earlier parsing or normalization failure |

### 26.3 Blocking Conditions

The following conditions MUST block Machine-Valid readiness:

* missing Project entity
* duplicate canonical identifiers
* unresolved references
* missing required fields
* invalid enum values
* missing validation for must-have requirements
* unverifiable policies
* incomplete workflows
* missing interface contracts
* missing authentication rules
* invalid relationship types
* disconnected graph entities

---

## 27.0 Canonical Error Model

This section defines canonical error classes. ISL v1.0 defined authored-language error classes; this document defines canonical semantic error classes produced during normalization and semantic validation.

A consistent error model is necessary for authoring tools, readiness reports, governance review, and automated repair assistance.

### 27.1 Canonical Error Record

| Field           | Type     | Required    | Description                     |
| --------------- | -------- | ----------- | ------------------------------- |
| error-id        | string   | REQUIRED    | Unique error identifier         |
| error-class     | enum     | REQUIRED    | Error class defined in §27.2    |
| severity        | enum     | REQUIRED    | blocking, high, medium, low     |
| entity-id       | string   | CONDITIONAL | Related entity identifier       |
| relationship-id | string   | CONDITIONAL | Related relationship identifier |
| source-section  | string   | CONDITIONAL | Authored source section         |
| message         | string   | REQUIRED    | Human-readable explanation      |
| required-action | string   | REQUIRED    | Required remediation            |
| detected-at     | ISO 8601 | REQUIRED    | Timestamp of detection          |

### 27.2 Error Classes

| Error Class                         | Description                                                        | Blocks Machine-Valid |
| ----------------------------------- | ------------------------------------------------------------------ | -------------------- |
| canonical-missing-entity            | Required entity is absent                                          | YES                  |
| canonical-missing-field             | Required canonical field is absent                                 | YES                  |
| canonical-invalid-type              | Entity type is invalid                                             | YES                  |
| canonical-invalid-enum              | Enum value is unsupported                                          | YES                  |
| canonical-duplicate-id              | Identifier is duplicated                                           | YES                  |
| canonical-invalid-id-format         | Identifier violates §23.0                                          | YES                  |
| canonical-unresolved-reference      | Relationship or field references missing entity                    | YES                  |
| canonical-invalid-relationship      | Relationship type is not permitted                                 | YES                  |
| canonical-missing-relationship      | Required relationship is absent                                    | YES                  |
| canonical-disconnected-entity       | Entity is not reachable from Project                               | YES                  |
| canonical-unverifiable-requirement  | Requirement lacks validation coverage                              | YES                  |
| canonical-unverifiable-policy       | Policy lacks verifiable rule or validation                         | YES                  |
| canonical-incomplete-workflow       | Workflow lacks required steps, terminal states, or exception paths | YES                  |
| canonical-incomplete-interface      | Interface lacks contract or authentication                         | YES                  |
| canonical-sensitive-data-ungoverned | Sensitive data lacks governing policy                              | YES                  |
| canonical-quality-warning           | Model is valid but below recommended quality                       | NO                   |

### 27.3 Error Handling Rules

A canonical processor MUST produce a structured error record for every detected blocking error.

A canonical processor MUST NOT suppress blocking errors.

A canonical processor MAY continue validation after detecting errors in order to produce a complete report.

A specification with one or more blocking canonical errors MUST NOT progress to Machine-Valid readiness.

---

## 28.0 Canonical Model Versioning

This section defines how canonical entities and canonical models evolve over time. Versioning is required because specifications change, requirements evolve, and generated systems must remain traceable across those changes.

Canonical versioning supports readiness regression, impact analysis, artifact supersession, governance review, and reconstruction of historical states.

### 28.1 Model Version Fields

A canonical model MUST record:

| Field                   | Type     | Required | Description                                          |
| ----------------------- | -------- | -------- | ---------------------------------------------------- |
| model-version           | semver   | REQUIRED | Version of the canonical model schema                |
| specification-version   | semver   | REQUIRED | Version of the source specification                  |
| generated-at            | ISO 8601 | REQUIRED | Time canonical model was generated                   |
| generated-by            | string   | REQUIRED | Processor or platform component that generated model |
| source-document-version | semver   | REQUIRED | Authored specification version                       |
| isl-version             | string   | REQUIRED | ISL version used                                     |

### 28.2 Entity Versioning Rules

Each canonical entity MUST carry a version.

When an entity changes semantically, its version MUST be incremented.

When an entity is replaced, the new entity MUST use a new identifier only if it represents a distinct semantic concept. If it is the same concept revised, the identifier MUST remain stable and the version MUST change.

Deprecated and superseded entities MUST remain available for traceability.

### 28.3 Model Versioning Rules

A new canonical model version MUST be created whenever the authored specification version changes.

Previous canonical model versions MUST remain queryable.

A platform MUST NOT overwrite a previous canonical model in a way that destroys traceability.

---

## 29.0 Extension Model

This section defines how organizations may extend the canonical semantic model. Extensions are useful for industry-specific entities, compliance overlays, specialized architectural domains, or internal governance requirements.

Extensions must be controlled because uncontrolled extension would undermine interoperability and validation.

### 29.1 Extension Declaration Schema

| Field            | Type   | Required    | Description                                     |
| ---------------- | ------ | ----------- | ----------------------------------------------- |
| extension-id     | string | REQUIRED    | Unique extension identifier                     |
| name             | string | REQUIRED    | Human-readable extension name                   |
| version          | semver | REQUIRED    | Extension version                               |
| owner            | string | REQUIRED    | Extension owner                                 |
| extends          | array  | REQUIRED    | Entity types, fields, or relationships extended |
| compatibility    | array  | REQUIRED    | Supported ISL versions                          |
| schema-reference | string | CONDITIONAL | Reference to extension schema                   |
| validation-rules | array  | CONDITIONAL | Extension validation rules                      |

### 29.2 Extension Rules

An extension MUST NOT redefine core entity types.

An extension MUST NOT remove required core fields.

An extension MUST NOT weaken validation, governance, traceability, or readiness requirements.

An extension MUST declare compatibility with specific ISL versions.

A processor that does not support an extension MUST reject the extension or preserve it as opaque metadata while reporting unsupported semantic content.

### 29.3 Extension Field Rules

Extension fields MUST be namespaced.

Extension fields MUST NOT collide with core field names.

Extension fields MUST be ignored only when they are not required for semantic interpretation. If an extension field changes required behavior, unsupported processors MUST reject the specification.

---

## 30.0 Standards Alignment

This section defines how the canonical semantic model aligns with external standards. Standards alignment improves enterprise credibility, auditability, and integration with existing governance and architecture practices.

Alignment does not mean that ISL becomes identical to these standards. It means that ISL entities and relationships MUST be mappable where applicable, and divergence MUST be explicit when required.

### 30.1 Alignment Table

| Standard          | ISL Alignment                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------- |
| ISO/IEC 25010     | Requirement.iso-25010-characteristic classifies non-functional requirements                              |
| NIST SP 800-53    | Policy.control-reference may reference applicable security and privacy controls                          |
| W3C PROV-DM       | Canonical entities and relationships align with provenance entity/activity/agent concepts                |
| OpenTelemetry     | Validation, Infrastructure, and later telemetry models SHOULD align with observability conventions       |
| ArchiMate / TOGAF | Service, Capability, Interface, Actor, and Stakeholder MAY be mapped to enterprise architecture concepts |

### 30.2 Standards Mapping Requirements

A canonical model intended for enterprise governance SHOULD preserve standards references in structured fields rather than prose.

Where a security or compliance policy claims alignment to an external control, the Policy entity MUST include a control-reference.

Where a non-functional requirement is classified using a quality characteristic, the classification SHOULD align to ISO/IEC 25010 unless another governance-approved quality model is used.

---

## 31.0 Conformance Requirements

This section defines what it means for canonical models, processors, and validators to conform to ISL v1.1. Conformance is separated by artifact type because a canonical model, normalization processor, and validation engine have different obligations.

Conformance to this document is required before a platform can claim semantic model support.

### 31.1 Canonical Model Conformance

A canonical model conforms to ISL v1.1 if it:

* includes exactly one Project entity
* uses only defined core entity types unless extensions are declared
* satisfies the base entity schema
* satisfies all applicable type-specific schemas
* uses valid traceability identifiers
* uses valid relationship types
* resolves all references
* satisfies required relationship constraints
* passes canonical validation

### 31.2 Normalization Processor Conformance

A normalization processor conforms to ISL v1.1 if it can:

* parse structured input from ISL v1.0 processors
* classify authored elements into canonical entity types
* validate or assign identifiers according to §23.0
* populate required canonical fields
* construct canonical relationships
* detect ambiguity and unmappable content
* produce canonical graphs
* produce structured normalization errors

### 31.3 Canonical Validator Conformance

A canonical validator conforms to ISL v1.1 if it can:

* validate all base schema requirements
* validate all type-specific schema requirements
* validate identifier format
* validate relationship constraints
* validate reference resolution
* validate required validation coverage
* detect disconnected graph entities
* produce structured canonical error records

---

## 32.0 Minimal Canonical Model Example

This section provides a compact example of canonical model content. The example is informative but demonstrates the level of structure required for canonical processing.

A full canonical representation may be serialized as JSON, stored in a graph database, or represented using another implementation format, provided that all required semantics are preserved.

### 32.1 Example Entity Set

| id                | type        | name              | version | status |
| ----------------- | ----------- | ----------------- | ------- | ------ |
| PRJ-example-00001 | Project     | Example System    | 1.0.0   | active |
| ACT-example-00001 | Actor       | Order Clerk       | 1.0.0   | active |
| REQ-example-00001 | Requirement | Submit Order      | 1.0.0   | active |
| SVC-example-00001 | Service     | Order Service     | 1.0.0   | active |
| INT-example-00001 | Interface   | Order API         | 1.0.0   | active |
| VAL-example-00001 | Validation  | Submit Order Test | 1.0.0   | active |

### 32.2 Example Relationship Set

| relationship-id   | source-id         | relationship-type | target-id         |
| ----------------- | ----------------- | ----------------- | ----------------- |
| REL-example-00001 | PRJ-example-00001 | contains          | ACT-example-00001 |
| REL-example-00002 | PRJ-example-00001 | contains          | REQ-example-00001 |
| REL-example-00003 | SVC-example-00001 | implements        | REQ-example-00001 |
| REL-example-00004 | SVC-example-00001 | exposes           | INT-example-00001 |
| REL-example-00005 | VAL-example-00001 | validates         | REQ-example-00001 |
| REL-example-00006 | ACT-example-00001 | consumes          | INT-example-00001 |

---

## 33.0 Machine-Valid Readiness Dependency

This section clarifies the relationship between the canonical semantic model and Machine-Valid readiness. ISL v1.3 defines readiness levels, but canonical validation is the semantic foundation for Machine-Valid status.

A specification MUST NOT be marked Machine-Valid unless canonical normalization and validation complete without blocking errors.

### 33.1 Required Machine-Valid Conditions

Before Machine-Valid readiness, the canonical model MUST satisfy:

* all required entity schemas
* all required relationship constraints
* all identifier rules
* all reference resolution rules
* all validation coverage rules
* all policy verifiability rules
* all data entity completeness rules
* all interface completeness rules
* all workflow completeness rules

### 33.2 Readiness Failure Behavior

If canonical validation fails, readiness evaluation MUST record the failure.

The platform MUST NOT proceed to construction planning based on a failed canonical model.

The platform MAY provide advisory repair guidance to specification authors.

---

## 34.0 Summary

ISL v1.1 defines the canonical semantic model that transforms authored ISL specifications into a machine-interpretable graph. This graph is the authoritative representation used for validation, readiness evaluation, planning, traceability, governance, and downstream autonomous construction.

This revision strengthens the canonical model by making entity schemas, relationships, identifiers, normalization, validation, and errors explicit. The result is a model that can be implemented, tested, validated, audited, and evolved.

A conforming ISL platform MUST treat the canonical semantic model as the semantic source of truth after normalization.
