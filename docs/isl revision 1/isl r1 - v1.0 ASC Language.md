# IMHOTEP Specification Language (ISL) v1.0

# The Specification Language

**Status:** Normative
**Depends On:** ISL v0.0
**Supersedes:** ISL r1 v1.0 ASC Language
**Document Type:** Language Specification

---

## 1.0 Scope

This document defines the IMHOTEP Specification Language (ISL) as the authored specification language used to describe software systems for autonomous construction. It establishes the required structure, authored representation rules, minimum content requirements, and validation expectations for ISL specifications before they are normalized into the canonical semantic model defined in ISL v1.1.

This document governs the **language surface** of ISL. It defines what a valid authored ISL specification MUST contain, how specification sections MUST be organized, and how human-authored content MUST map to machine-interpretable structures. It does not define the full internal canonical entity schema, execution lifecycle, construction task graph, or platform runtime behavior; those responsibilities are assigned to later ISL documents.

An ISL v1.0 conforming specification MUST be capable of being:

* read and reviewed by human stakeholders
* parsed into structured sections
* normalized into the ISL canonical semantic model
* evaluated for readiness progression
* traced to generated artifacts in later construction phases

---

## 2.0 Normative References

This section identifies standards and ISL documents that are required to correctly interpret this language specification. These references establish the external and internal authority chain for interpreting ISL terminology, quality classifications, security policy references, and readiness behavior.

Where this document references another ISL document, the referenced document governs the specialized model assigned to it. Where this document references an external standard, the external standard provides terminology or classification guidance unless ISL explicitly defines a stricter rule.

| Reference      | Purpose                                                               |
| -------------- | --------------------------------------------------------------------- |
| ISL v0.0       | Corpus-level conformance, terminology, and governing principles       |
| ISL v1.1       | Canonical semantic model and normalization rules                      |
| ISL v1.3       | Specification readiness levels and progression criteria               |
| ISL v1.4       | Traceability model and identifier behavior                            |
| ISL v1.7       | Governance roles, approval gates, and policy enforcement              |
| RFC 2119       | Normative requirement terminology                                     |
| ISO/IEC 25010  | Quality characteristic classification for non-functional requirements |
| NIST SP 800-53 | Security and privacy control reference model                          |
| W3C PROV-DM    | Provenance and traceability alignment model                           |

---

## 3.0 Terms and Definitions

This section defines terms used specifically within the ISL language layer. These definitions are intended to avoid ambiguity during authoring, review, parsing, and normalization.

Terms defined here apply to ISL specifications before they are converted into the canonical semantic model. More detailed entity-level definitions are provided in ISL v1.1.

**Authored Specification**
A human-authored ISL document written using the structured Markdown representation defined by this document.

**Canonical Specification**
The normalized machine-interpretable representation of an authored specification, governed by ISL v1.1.

**Specification Section**
A required or conditional top-level portion of an ISL document that captures one class of system intent.

**Specification Element**
A structured item within a section, such as a requirement, actor, service, data entity, policy, workflow, or validation criterion.

**Machine-Valid**
A readiness condition in which an ISL specification has passed structural, semantic, and reference validation as defined by ISL v1.3.

**Autonomous-Ready**
A readiness condition in which an ISL specification is approved for autonomous construction under governance control.

**Authoring Representation**
The human-facing surface format used to write an ISL specification.

**Canonical Mapping**
The required transformation from authored specification content into canonical entities and relationships.

---

## 4.0 Conventions

This section defines conventions used throughout the language specification. These conventions are required to ensure that authored specifications are readable, parseable, and enforceable.

The conventions in this section apply to all authored ISL specifications unless a later ISL document defines a stricter rule.

### 4.1 Normative Language

The following terms carry precise normative meaning:

* **MUST** and **SHALL** indicate mandatory requirements.
* **MUST NOT** and **SHALL NOT** indicate mandatory prohibitions.
* **SHOULD** indicates a recommended practice; deviations MUST be justified.
* **MAY** indicates permitted behavior.

Normative requirements apply to both specification authors and conforming ISL tooling unless explicitly scoped to one or the other.

### 4.2 Field Requirement Levels

ISL uses the following field requirement levels:

| Requirement Level | Meaning                                                            |
| ----------------- | ------------------------------------------------------------------ |
| REQUIRED          | Field or section MUST be present                                   |
| CONDITIONAL       | Field or section MUST be present when the stated condition applies |
| OPTIONAL          | Field or section MAY be present                                    |
| DERIVED           | Field is produced during normalization or platform processing      |

### 4.3 Identifier Style

Identifiers in authored ISL specifications MUST be stable, unique within their entity type, and suitable for traceability. Identifier formats are defined normatively in ISL v1.1 and applied by the traceability model in ISL v1.4.

An authored specification MAY use provisional identifiers during Draft readiness, but it MUST use valid traceability identifiers before reaching Machine-Valid readiness.

### 4.4 Date, Time, and Version Format

Date and time values in ISL specifications MUST use ISO 8601 format.

Version values MUST use semantic versioning in the following form:

| Component | Meaning                                 |
| --------- | --------------------------------------- |
| major     | Breaking specification change           |
| minor     | Backward-compatible functional addition |
| patch     | Correction or clarification             |

Example semantic version values include:

* 1.0.0
* 1.2.0
* 2.0.1

---

## 5.0 Purpose of the IMHOTEP Specification Language

The IMHOTEP Specification Language defines the structured blueprint from which the IMHOTEP platform constructs software systems. Its purpose is to allow business intent, system requirements, architecture, policies, operational constraints, and validation expectations to be expressed in a form that can be understood by humans and processed by autonomous construction systems.

Within the IMHOTEP lifecycle, the specification is the authoritative source of truth. Generated source code, configuration files, infrastructure definitions, tests, deployment artifacts, and documentation are derived from the ISL specification. A conforming implementation MUST treat the specification as the controlling artifact for planning, execution, validation, traceability, and governance.

ISL exists to support the following objectives:

* express system intent in a structured and reviewable form
* reduce ambiguity before construction begins
* enable automated semantic normalization
* support autonomous construction planning
* enforce traceability from intent to artifact
* support deterministic validation of generated systems
* provide governance-ready evidence for review and authorization

---

## 6.0 Language Design Principles

This section defines the design principles that govern ISL. These principles guide both specification authors and tool implementers. They also establish the boundaries between human-readable authoring and machine-enforceable interpretation.

A specification that violates these principles MAY be readable as documentation, but it SHALL NOT be considered a valid ISL specification.

### 6.1 Human-Readable Authoring

ISL specifications MUST be understandable by business analysts, architects, engineers, security reviewers, operators, and governance stakeholders. The authored representation MUST resemble structured architectural documentation rather than source code.

This principle does not permit informal or vague content. Human readability MUST coexist with machine interpretation. Authors MUST express intent using structured sections, explicit identifiers, typed elements, and verifiable statements.

### 6.2 Machine-Interpretable Structure

Every required section of an ISL specification MUST map to one or more canonical entities defined by ISL v1.1. The mapping MUST be deterministic enough for conforming tooling to parse, validate, normalize, and reject invalid content.

Free-form prose MAY be used for explanation, but the normative content of a specification MUST be expressed through structured fields, tables, lists, or explicitly parseable statements.

### 6.3 Specification as Source of Truth

The ISL specification SHALL be the authoritative description of the system. No generated artifact MAY supersede, contradict, or silently extend the specification.

When an implementation requires behavior not present in the specification, the specification MUST be updated or the behavior MUST be treated as inferred and governed by traceability and justification rules.

### 6.4 Layered Completeness

Specifications evolve from high-level intent to construction-ready detail. A specification MAY begin as a partial Draft, but it MUST satisfy progressively stricter requirements before reaching Reviewable, Machine-Valid, and Autonomous-Ready readiness levels.

No required specification layer MAY be skipped when progressing to Autonomous-Ready status.

### 6.5 Explicit Validation

Every requirement, policy, workflow, interface, and operational expectation MUST be connected to a validation mechanism before the specification can reach Machine-Valid readiness.

Validation MUST be expressed as testable, observable, or reviewable criteria with a clear pass/fail or compliant/non-compliant outcome.

### 6.6 Extensibility Without Semantic Breakage

ISL MUST support extension without breaking existing specifications. Extensions MUST be additive, versioned, and explicitly declared.

An extension MUST NOT redefine the meaning of a core ISL section, canonical entity, required field, readiness criterion, or governance control.

---

## 7.0 ISL Representations

This section defines the supported surface representations of ISL. A single ISL specification may exist in multiple forms, but all valid representations MUST resolve to the same canonical semantic structure.

The representation model exists to support different users and tools. Humans typically author specifications in structured Markdown. Platforms process normalized JSON. Toolchains and integration pipelines MAY exchange YAML when appropriate.

### 7.1 Representation Types

| Representation | Format              | Primary Use                                 | Normative Role                       |
| -------------- | ------------------- | ------------------------------------------- | ------------------------------------ |
| Authored       | Structured Markdown | Human authoring and review                  | Primary human-facing representation  |
| Canonical      | JSON                | Platform processing and validation          | Authoritative machine representation |
| Exchange       | YAML                | Toolchain exchange and pipeline integration | Interoperability representation      |

The canonical JSON representation SHALL govern when two representations disagree.

### 7.2 Authored Markdown Representation

The authored representation MUST use structured Markdown. This representation is intended for human authoring, review, governance approval, and version control.

Authored Markdown MUST follow these rules:

* Top-level sections MUST use numbered headings.
* Required sections MUST appear in the order defined by this document.
* Structured elements MUST be represented using tables or labeled field lists.
* Each specification element requiring traceability MUST include an identifier.
* Prose MAY explain intent but MUST NOT replace required structured fields.

### 7.3 Canonical JSON Representation

The canonical JSON representation is the authoritative internal form used by conforming ISL processors. It is derived from the authored specification through normalization.

The canonical representation MUST:

* conform to the canonical semantic model defined in ISL v1.1
* preserve all identifiers from the authored specification
* preserve all relationships required for traceability
* reject ambiguous or unmappable authored content
* record normalization errors when mapping fails

### 7.4 YAML Exchange Representation

The YAML exchange representation MAY be used for toolchain integration, pipeline transfer, or configuration-based workflows. YAML MUST NOT introduce semantics that are absent from the canonical JSON representation.

A YAML exchange document MUST be transformable into canonical JSON without semantic loss.

### 7.5 Graphical and Visual Representations

Graphical editors, diagramming tools, or visual modeling environments MAY be used as authoring surfaces if they produce output that conforms to the authored or canonical representation rules.

A graphical representation SHALL NOT be considered authoritative unless it can be exported into a valid ISL representation.

---

## 8.0 Authored Markdown Syntax

This section defines the minimum syntax rules for authored ISL Markdown documents. This closes the language-level gap between descriptive documentation and parseable specification content.

The authored syntax is intentionally simple. It uses numbered sections, tables, enumerated values, and stable identifiers rather than a custom programming-language grammar. This allows ISL to remain accessible to non-programmers while still supporting machine validation.

### 8.1 Document Header Syntax

Every authored ISL specification MUST begin with a document header.

The header MUST include:

| Field                 | Required | Description                           |
| --------------------- | -------- | ------------------------------------- |
| Document Title        | REQUIRED | Human-readable specification title    |
| System ID             | REQUIRED | Stable system identifier              |
| Specification Version | REQUIRED | Semantic version of the specification |
| ISL Version           | REQUIRED | ISL language version used             |
| Status                | REQUIRED | Current readiness status              |
| Owner                 | REQUIRED | Responsible organizational unit       |
| Last Modified         | REQUIRED | ISO 8601 date                         |

Recommended authored Markdown form:

# System Name Specification

**System ID:** example-system
**Specification Version:** 1.0.0
**ISL Version:** 1.0
**Status:** Draft
**Owner:** Example Organization
**Last Modified:** 2026-05-02

### 8.2 Top-Level Section Syntax

Each required ISL section MUST be represented as a numbered second-level Markdown heading.

Required heading form:

## 1. System Identity and Context

## 2. Business Objectives and Scope

## 3. Actors and Stakeholders

## 4. Functional Requirements

## 5. Non-Functional Requirements

## 6. Data Model and Information Structures

## 7. Service Boundaries and Interfaces

## 8. Workflows and Behavioral Processes

## 9. Security and Policy Constraints

## 10. Operational and Deployment Expectations

## 11. Validation and Acceptance Criteria

## 12. Change History

A conforming parser MUST reject a Machine-Valid candidate specification if any REQUIRED section is missing.

### 8.3 Structured Element Syntax

Structured elements SHOULD be represented as Markdown tables when multiple elements of the same type appear in a section.

A structured element table MUST include:

* an identifier column when the element maps to a canonical entity
* a name or statement column when the element has human-readable content
* a type or classification column when the element is typed
* a validation or reference column when the element must be verified

Field-list syntax MAY be used when a section contains a single complex element.

### 8.4 Required Field Syntax

Required fields MUST be explicitly labeled. A parser MUST NOT infer a required field from surrounding prose unless the field appears in a defined structured location.

Valid field-list form:

* **id:** REQ-example-00001
* **statement:** The system MUST authenticate users before granting access.
* **priority:** must-have
* **source:** STK-example-00001
* **validation:** VAL-example-00001

### 8.5 Controlled Vocabulary Syntax

Where this document defines enum values, authored specifications MUST use the exact lowercase enum value unless a later ISL document explicitly permits aliases.

For example, requirement priority MUST use one of:

* must-have
* should-have
* nice-to-have

A conforming processor MUST reject unrecognized enum values during Machine-Valid evaluation.

### 8.6 Prose Usage

Prose MAY be used to explain rationale, context, assumptions, design intent, or business background. Prose MUST NOT be used as the only representation of required specification content.

If prose conflicts with structured fields, the structured fields SHALL govern.

---

## 9.0 Required Specification Structure

This section defines the complete structure of an authored ISL specification. These sections form the minimum complete language surface required to describe a system for review, semantic normalization, and eventual autonomous construction.

The structure is intentionally ordered from broad context to validation. This order allows humans and machines to understand the system progressively: identity first, objectives second, actors and requirements next, then architecture, policy, operation, and validation.

| Section | Title                                   | Requirement Level | Canonical Mapping                    |
| ------- | --------------------------------------- | ----------------- | ------------------------------------ |
| 1       | System Identity and Context             | REQUIRED          | Project, Context                     |
| 2       | Business Objectives and Scope           | REQUIRED          | Requirement, Capability, Context     |
| 3       | Actors and Stakeholders                 | REQUIRED          | Actor, Stakeholder                   |
| 4       | Functional Requirements                 | REQUIRED          | Requirement                          |
| 5       | Non-Functional Requirements             | REQUIRED          | Requirement, Policy                  |
| 6       | Data Model and Information Structures   | REQUIRED          | DataEntity                           |
| 7       | Service Boundaries and Interfaces       | REQUIRED          | Service, Interface                   |
| 8       | Workflows and Behavioral Processes      | CONDITIONAL       | Workflow                             |
| 9       | Security and Policy Constraints         | REQUIRED          | Policy                               |
| 10      | Operational and Deployment Expectations | REQUIRED          | Infrastructure, Policy               |
| 11      | Validation and Acceptance Criteria      | REQUIRED          | Validation                           |
| 12      | Change History                          | REQUIRED          | Project metadata, governance records |

A specification MAY include appendices, examples, diagrams, and explanatory material, but such material MUST NOT replace required sections.

---

## 10.0 Section 1 — System Identity and Context

This section identifies the system being specified and establishes its operating context. It is the entry point for the specification and provides the root identity used by later semantic normalization, traceability, readiness evaluation, and governance approval.

The System Identity and Context section MUST be complete before a specification can progress beyond Draft readiness. Without a stable identity, later identifiers, traceability relationships, ownership records, and governance events cannot be reliably assigned.

### 10.1 Required Fields

| Field            | Type   | Required    | Description                                        |
| ---------------- | ------ | ----------- | -------------------------------------------------- |
| system-id        | string | REQUIRED    | Stable identifier for the system                   |
| name             | string | REQUIRED    | Human-readable system name                         |
| version          | semver | REQUIRED    | Specification version                              |
| domain           | string | REQUIRED    | Business or technical domain                       |
| owner            | string | REQUIRED    | Responsible organization or unit                   |
| description      | string | REQUIRED    | Purpose of the system                              |
| isl-version      | string | REQUIRED    | ISL version used to author the specification       |
| readiness-level  | enum   | REQUIRED    | draft, reviewable, machine-valid, autonomous-ready |
| external-context | array  | CONDITIONAL | External systems, constraints, or dependencies     |

### 10.2 Rules

The system-id MUST remain stable across specification versions unless the system is formally replaced by a new system. The name MAY change for readability, but the system-id MUST remain the traceability anchor.

The description MUST explain the purpose of the system in two to five sentences. Marketing language, aspirational claims, or vague statements MUST NOT substitute for a clear statement of purpose.

### 10.3 Minimum Authored Form

| Field           | Value                                                                                |
| --------------- | ------------------------------------------------------------------------------------ |
| system-id       | example-system                                                                       |
| name            | Example System                                                                       |
| version         | 1.0.0                                                                                |
| domain          | order-management                                                                     |
| owner           | Enterprise Architecture                                                              |
| description     | The system manages order intake, validation, and status tracking for internal users. |
| isl-version     | 1.0                                                                                  |
| readiness-level | draft                                                                                |

---

## 11.0 Section 2 — Business Objectives and Scope

This section defines why the system exists and what boundaries constrain its responsibility. Business objectives provide the measurable outcomes the system is intended to achieve. Scope statements prevent the platform from inferring responsibilities that were not explicitly assigned.

Enterprise specifications frequently fail because objectives are vague and scope boundaries are implicit. ISL requires measurable objectives and explicit exclusions so that downstream planning, validation, and governance can reason about what the system must and must not do.

### 11.1 Business Objective Fields

| Field     | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| id        | string | REQUIRED | Objective identifier                 |
| statement | string | REQUIRED | Measurable outcome statement         |
| metric    | string | REQUIRED | Measurement used to evaluate success |
| target    | string | REQUIRED | Required target value                |
| source    | string | REQUIRED | Stakeholder or business driver       |
| priority  | enum   | REQUIRED | must-have, should-have, nice-to-have |

### 11.2 Scope Fields

| Field        | Type  | Required    | Description                             |
| ------------ | ----- | ----------- | --------------------------------------- |
| in-scope     | array | REQUIRED    | Responsibilities included in the system |
| out-of-scope | array | REQUIRED    | Responsibilities explicitly excluded    |
| assumptions  | array | CONDITIONAL | Assumptions required for interpretation |
| dependencies | array | CONDITIONAL | External dependencies affecting scope   |

### 11.3 Rules

Each business objective MUST be measurable. Statements such as “improve user experience,” “increase efficiency,” or “modernize the platform” MUST NOT be accepted unless accompanied by measurable targets.

Each specification MUST include at least one explicit scope exclusion before reaching Reviewable readiness. Implicit exclusions MUST NOT be assumed.

### 11.4 Minimum Authored Form

| id                | statement                             | metric             | target                            | source            | priority  |
| ----------------- | ------------------------------------- | ------------------ | --------------------------------- | ----------------- | --------- |
| OBJ-example-00001 | Reduce manual order validation effort | manual review rate | less than 10% of submitted orders | STK-example-00001 | must-have |

Scope:

* **in-scope:** order intake, order validation, order status tracking
* **out-of-scope:** payment processing, shipment execution
* **assumptions:** customer identity is provided by an external identity provider

---

## 12.0 Section 3 — Actors and Stakeholders

This section identifies the human, system, process, and organizational participants relevant to the system. Actors interact directly with the system at runtime. Stakeholders have an interest in the system’s outcomes, governance, funding, compliance, or operation.

Actors and stakeholders must be explicit because they drive requirements, permissions, interfaces, workflows, and governance decisions. A requirement without a source is not sufficiently grounded for enterprise traceability.

### 12.1 Actor Fields

| Field           | Type   | Required    | Description                             |
| --------------- | ------ | ----------- | --------------------------------------- |
| id              | string | REQUIRED    | Actor identifier                        |
| name            | string | REQUIRED    | Human-readable actor name               |
| actor-type      | enum   | REQUIRED    | human, system, process, external        |
| description     | string | REQUIRED    | Runtime role of the actor               |
| permissions     | array  | REQUIRED    | Operations the actor may perform        |
| interfaces-used | array  | CONDITIONAL | Interface identifiers used by the actor |

### 12.2 Stakeholder Fields

| Field              | Type    | Required | Description                                                    |
| ------------------ | ------- | -------- | -------------------------------------------------------------- |
| id                 | string  | REQUIRED | Stakeholder identifier                                         |
| name               | string  | REQUIRED | Human-readable stakeholder name                                |
| role               | string  | REQUIRED | Organizational or governance role                              |
| concerns           | array   | REQUIRED | Outcomes or qualities the stakeholder cares about              |
| approval-authority | boolean | REQUIRED | Whether this stakeholder can approve specification progression |

### 12.3 Rules

Every functional requirement MUST reference at least one actor, stakeholder, or business objective as its source.

An actor MUST NOT be assigned permissions that are not later reflected in interface, workflow, or policy definitions.

A stakeholder with approval-authority set to true MUST correspond to a governance role defined under ISL v1.7 before the specification can reach Machine-Valid readiness.

### 12.4 Minimum Authored Form

Actors:

| id                | name        | actor-type | description                                  | permissions                     |
| ----------------- | ----------- | ---------- | -------------------------------------------- | ------------------------------- |
| ACT-example-00001 | Order Clerk | human      | Internal user who submits and reviews orders | create-order, view-order-status |

Stakeholders:

| id                | name               | role           | concerns                         | approval-authority |
| ----------------- | ------------------ | -------------- | -------------------------------- | ------------------ |
| STK-example-00001 | Operations Manager | Business Owner | order accuracy, processing speed | true               |

---

## 13.0 Section 4 — Functional Requirements

This section defines the behaviors the system must provide. Functional requirements are the primary source for implementation behavior, acceptance criteria, generated tests, and traceability to code artifacts.

Functional requirements MUST be specific, active-voice, and verifiable. Ambiguous language causes downstream uncertainty in planning, implementation, testing, and governance review.

### 13.1 Required Fields

| Field      | Type   | Required    | Description                                 |
| ---------- | ------ | ----------- | ------------------------------------------- |
| id         | string | REQUIRED    | Requirement identifier                      |
| statement  | string | REQUIRED    | Active-voice behavioral requirement         |
| priority   | enum   | REQUIRED    | must-have, should-have, nice-to-have        |
| source     | string | REQUIRED    | Actor, stakeholder, or objective identifier |
| validation | string | REQUIRED    | Validation identifier or method             |
| capability | string | CONDITIONAL | Capability grouping identifier              |
| rationale  | string | OPTIONAL    | Explanation of why the requirement exists   |

### 13.2 Requirement Statement Rules

A functional requirement statement MUST:

* describe one behavior only
* use active voice
* identify the system responsibility
* avoid implementation details unless architecturally required
* avoid ambiguous terms such as fast, easy, robust, seamless, normal, appropriate, or user-friendly unless quantified elsewhere

A functional requirement statement MUST NOT use:

* should
* might
* could
* as needed
* where appropriate
* etc.

### 13.3 Priority Rules

| Priority     | Meaning                                          |
| ------------ | ------------------------------------------------ |
| must-have    | Required for system acceptance                   |
| should-have  | Expected unless formally deferred                |
| nice-to-have | Optional enhancement not required for acceptance |

A must-have functional requirement MUST have at least one linked Validation entity before the specification can reach Autonomous-Ready readiness.

### 13.4 Minimum Authored Form

| id                | statement                                                                                                 | priority  | source            | validation        |
| ----------------- | --------------------------------------------------------------------------------------------------------- | --------- | ----------------- | ----------------- |
| REQ-example-00001 | The system MUST allow an Order Clerk to submit a new order with customer, item, and quantity information. | must-have | ACT-example-00001 | VAL-example-00001 |

---

## 14.0 Section 5 — Non-Functional Requirements

This section defines quality attributes, constraints, and measurable expectations that govern how the system must behave. Non-functional requirements are critical for enterprise readiness because they define performance, reliability, security, maintainability, portability, usability, compatibility, and scalability expectations.

Non-functional requirements MUST be measurable wherever measurement is possible. A non-functional requirement that cannot be measured MUST define a reviewable compliance method.

### 14.1 Required Fields

| Field          | Type   | Required    | Description                                  |
| -------------- | ------ | ----------- | -------------------------------------------- |
| id             | string | REQUIRED    | Requirement identifier                       |
| characteristic | enum   | REQUIRED    | ISO/IEC 25010-aligned quality characteristic |
| statement      | string | REQUIRED    | Quality or constraint statement              |
| metric         | string | CONDITIONAL | Measurement used to verify the requirement   |
| target         | string | CONDITIONAL | Required target value                        |
| source         | string | REQUIRED    | Stakeholder, policy, or objective identifier |
| validation     | string | REQUIRED    | Validation identifier or method              |

### 14.2 Quality Characteristics

ISL specifications SHOULD classify non-functional requirements using ISO/IEC 25010-aligned characteristics.

| Characteristic         | Example Measures                               |
| ---------------------- | ---------------------------------------------- |
| functional-suitability | completeness, correctness                      |
| performance-efficiency | response time, throughput, latency             |
| compatibility          | interoperability, coexistence                  |
| usability              | task completion rate, accessibility compliance |
| reliability            | availability, error rate, recovery time        |
| security               | authentication, authorization, encryption      |
| maintainability        | code coverage, complexity, modularity          |
| portability            | supported runtime environments                 |

### 14.3 Rules

A non-functional requirement MUST NOT use subjective quality statements without a measurable or reviewable target.

Examples of invalid statements:

* The system must be fast.
* The system must be secure.
* The system must be easy to maintain.

Examples of valid statements:

* The system MUST respond to 95% of order status requests within 500 milliseconds under normal operating load.
* The system MUST encrypt customer identifiers at rest using an approved enterprise encryption standard.
* The system MUST maintain unit test coverage of at least 80% for generated business logic.

### 14.4 Minimum Authored Form

| id                | characteristic         | statement                                                                           | metric      | target | source            | validation        |
| ----------------- | ---------------------- | ----------------------------------------------------------------------------------- | ----------- | ------ | ----------------- | ----------------- |
| REQ-example-00002 | performance-efficiency | The system MUST respond to order status requests within the defined latency target. | p95 latency | 500 ms | STK-example-00001 | VAL-example-00002 |

---

## 15.0 Section 6 — Data Model and Information Structures

This section defines the information managed by the system. Data entities provide the foundation for generated schemas, persistence models, validation logic, API contracts, and traceability from requirements to implementation artifacts.

A data model that lacks explicit attributes, constraints, and relationships cannot be reliably generated, validated, or evolved. ISL therefore requires every data entity to be structurally described.

### 15.1 Data Entity Fields

| Field         | Type   | Required    | Description                                 |
| ------------- | ------ | ----------- | ------------------------------------------- |
| id            | string | REQUIRED    | Data entity identifier                      |
| name          | string | REQUIRED    | Human-readable entity name                  |
| description   | string | REQUIRED    | Purpose of the data entity                  |
| attributes    | array  | REQUIRED    | Attribute definitions                       |
| relationships | array  | CONDITIONAL | Relationships to other data entities        |
| owner-service | string | CONDITIONAL | Service responsible for managing the entity |

### 15.2 Attribute Fields

| Field       | Type    | Required    | Description                              |
| ----------- | ------- | ----------- | ---------------------------------------- |
| name        | string  | REQUIRED    | Attribute name                           |
| data-type   | string  | REQUIRED    | Primitive, structured, or reference type |
| nullable    | boolean | REQUIRED    | Whether null is permitted                |
| constraints | array   | CONDITIONAL | Validation constraints                   |
| description | string  | CONDITIONAL | Attribute purpose                        |

### 15.3 Relationship Fields

| Field             | Type    | Required | Description                           |
| ----------------- | ------- | -------- | ------------------------------------- |
| target            | string  | REQUIRED | Target DataEntity identifier          |
| relationship-type | string  | REQUIRED | Nature of the relationship            |
| cardinality       | enum    | REQUIRED | one-to-one, one-to-many, many-to-many |
| required          | boolean | REQUIRED | Whether the relationship is mandatory |

### 15.4 Rules

Each data entity MUST include at least one attribute.

Each attribute MUST declare a data-type and nullability.

Relationships MUST declare cardinality explicitly. Implicit cardinality MUST NOT be assumed.

A data entity that stores sensitive, regulated, or personal information MUST be governed by at least one Policy entity before the specification can reach Machine-Valid readiness.

### 15.5 Minimum Authored Form

Data Entities:

| id                | name  | description                                       | owner-service     |
| ----------------- | ----- | ------------------------------------------------- | ----------------- |
| DAT-example-00001 | Order | Represents an order submitted by an internal user | SVC-example-00001 |

Attributes:

| entity-id         | name       | data-type | nullable | constraints                    |
| ----------------- | ---------- | --------- | -------- | ------------------------------ |
| DAT-example-00001 | orderId    | string    | false    | unique                         |
| DAT-example-00001 | customerId | string    | false    | required                       |
| DAT-example-00001 | status     | enum      | false    | submitted, validated, rejected |

---

## 16.0 Section 7 — Service Boundaries and Interfaces

This section defines the structural components that provide system behavior and the interfaces through which actors, services, or external systems interact with them. Services establish responsibility boundaries. Interfaces establish communication boundaries.

Clear service and interface definitions are necessary for construction planning, implementation generation, dependency analysis, security validation, and deployment preparation.

### 16.1 Service Fields

| Field          | Type   | Required    | Description                                        |
| -------------- | ------ | ----------- | -------------------------------------------------- |
| id             | string | REQUIRED    | Service identifier                                 |
| name           | string | REQUIRED    | Human-readable service name                        |
| responsibility | string | REQUIRED    | Single responsibility statement                    |
| capabilities   | array  | CONDITIONAL | Capability identifiers implemented by the service  |
| requirements   | array  | REQUIRED    | Requirement identifiers implemented by the service |
| data-entities  | array  | CONDITIONAL | DataEntity identifiers managed by the service      |
| dependencies   | array  | CONDITIONAL | Service or external dependencies                   |
| interfaces     | array  | REQUIRED    | Interface identifiers exposed by the service       |

### 16.2 Interface Fields

| Field               | Type   | Required    | Description                                                      |
| ------------------- | ------ | ----------- | ---------------------------------------------------------------- |
| id                  | string | REQUIRED    | Interface identifier                                             |
| name                | string | REQUIRED    | Human-readable interface name                                    |
| interface-type      | enum   | REQUIRED    | rest-api, grpc, message-queue, event-stream, batch-file, graphql |
| exposed-by          | string | REQUIRED    | Service identifier exposing the interface                        |
| used-by             | array  | CONDITIONAL | Actor or service identifiers using the interface                 |
| contract            | string | REQUIRED    | Inline or referenced interface contract                          |
| authentication      | string | REQUIRED    | Authentication mechanism                                         |
| authorization       | string | CONDITIONAL | Authorization rule or policy reference                           |
| versioning-strategy | string | REQUIRED    | Interface versioning approach                                    |

### 16.3 Rules

A service MUST have one clearly stated responsibility.

A service MUST NOT overlap responsibility with another service in the same specification unless the overlap is explicitly justified as redundancy, migration support, or failover behavior.

Each service MUST expose or consume at least one interface unless it is explicitly marked as internal infrastructure or background processing.

Each interface MUST define authentication before the specification can reach Machine-Valid readiness.

### 16.4 Minimum Authored Form

Services:

| id                | name          | responsibility                                            | requirements      | interfaces        |
| ----------------- | ------------- | --------------------------------------------------------- | ----------------- | ----------------- |
| SVC-example-00001 | Order Service | Manages order submission, validation, and status tracking | REQ-example-00001 | INT-example-00001 |

Interfaces:

| id                | name      | interface-type | exposed-by        | contract                             | authentication            | versioning-strategy |
| ----------------- | --------- | -------------- | ----------------- | ------------------------------------ | ------------------------- | ------------------- |
| INT-example-00001 | Order API | rest-api       | SVC-example-00001 | OpenAPI reference or inline contract | enterprise-identity-token | URI versioning      |

---

## 17.0 Section 8 — Workflows and Behavioral Processes

This section defines multi-step behavior involving actors, services, decisions, and exception paths. Workflows are required when system behavior cannot be fully understood from isolated requirements or service definitions.

Workflows are especially important for enterprise systems because failures often occur at process boundaries rather than within individual functions. ISL therefore requires decision points and exception paths when workflows are present.

### 17.1 When Workflows Are Required

A workflow section MUST be included when the system includes behavior involving any of the following:

* more than one actor
* more than one service
* ordered multi-step processing
* decision points
* approval paths
* exception handling
* asynchronous events
* state transitions

### 17.2 Workflow Fields

| Field           | Type   | Required    | Description                                 |
| --------------- | ------ | ----------- | ------------------------------------------- |
| id              | string | REQUIRED    | Workflow identifier                         |
| name            | string | REQUIRED    | Human-readable workflow name                |
| trigger         | string | REQUIRED    | Event or condition that starts the workflow |
| participants    | array  | REQUIRED    | Actors and services involved                |
| steps           | array  | REQUIRED    | Ordered workflow steps                      |
| decision-points | array  | CONDITIONAL | Branching conditions                        |
| exception-paths | array  | REQUIRED    | Failure and exception handling              |
| validation      | string | REQUIRED    | Validation identifier or method             |

### 17.3 Workflow Step Fields

| Field             | Type    | Required    | Description                         |
| ----------------- | ------- | ----------- | ----------------------------------- |
| sequence          | integer | REQUIRED    | Step order                          |
| actor-or-service  | string  | REQUIRED    | Responsible participant             |
| action            | string  | REQUIRED    | Action performed                    |
| input             | string  | CONDITIONAL | Input required                      |
| output            | string  | CONDITIONAL | Output produced                     |
| success-condition | string  | REQUIRED    | Condition for successful completion |

### 17.4 Rules

A workflow MUST include at least one step.

A workflow with no exception paths MUST NOT be considered complete.

Decision points MUST include explicit conditions. A decision point MUST NOT rely on unstated judgment or implicit interpretation.

### 17.5 Minimum Authored Form

Workflows:

| id                | name         | trigger                   | participants                         | validation        |
| ----------------- | ------------ | ------------------------- | ------------------------------------ | ----------------- |
| WFL-example-00001 | Submit Order | Order Clerk submits order | ACT-example-00001, SVC-example-00001 | VAL-example-00003 |

Steps:

| workflow-id       | sequence | actor-or-service  | action               | success-condition                |
| ----------------- | -------- | ----------------- | -------------------- | -------------------------------- |
| WFL-example-00001 | 1        | ACT-example-00001 | Submit order request | Request contains required fields |
| WFL-example-00001 | 2        | SVC-example-00001 | Validate order       | Order is accepted or rejected    |

Exception Paths:

| workflow-id       | condition              | handling                                   |
| ----------------- | ---------------------- | ------------------------------------------ |
| WFL-example-00001 | Required field missing | Reject request and return validation error |

---

## 18.0 Section 9 — Security and Policy Constraints

This section defines rules that constrain system behavior for security, compliance, operational control, data protection, and governance. Policies translate enterprise control expectations into verifiable rules.

Security and policy constraints MUST be expressed as rules, not aspirations. A policy that merely states intent cannot be enforced by deterministic tools or governance engines.

### 18.1 Policy Fields

| Field              | Type   | Required    | Description                                                                 |
| ------------------ | ------ | ----------- | --------------------------------------------------------------------------- |
| id                 | string | REQUIRED    | Policy identifier                                                           |
| name               | string | REQUIRED    | Human-readable policy name                                                  |
| policy-type        | enum   | REQUIRED    | security, compliance, operational, data-handling, technology-standard, risk |
| rule               | string | REQUIRED    | Verifiable policy rule                                                      |
| enforcement-point  | enum   | REQUIRED    | authoring, planning, generation, validation, deployment, runtime            |
| applies-to         | array  | REQUIRED    | Entity identifiers governed by the policy                                   |
| control-reference  | string | CONDITIONAL | External control reference                                                  |
| violation-response | enum   | REQUIRED    | block, escalate, warn, audit-only                                           |
| validation         | string | REQUIRED    | Validation identifier or method                                             |

### 18.2 Rules

Each policy MUST contain a verifiable rule.

Intent-only policy statements MUST be rejected during Machine-Valid evaluation.

Where applicable, security policies SHOULD reference NIST SP 800-53 or an equivalent enterprise control framework.

A policy with violation-response set to block MUST prevent readiness progression, construction, deployment, or runtime continuation at its enforcement point until resolved or waived under governance rules.

### 18.3 Invalid and Valid Policy Statements

Invalid:

* The system should be secure.
* Access should be controlled appropriately.
* Sensitive data should be protected.

Valid:

* The system MUST require authenticated identity tokens for all order submission requests.
* The system MUST encrypt customer identifiers at rest.
* The system MUST reject requests from actors lacking create-order permission.

### 18.4 Minimum Authored Form

| id                | name                           | policy-type | rule                                                                        | enforcement-point | applies-to        | violation-response | validation        |
| ----------------- | ------------------------------ | ----------- | --------------------------------------------------------------------------- | ----------------- | ----------------- | ------------------ | ----------------- |
| POL-example-00001 | Authenticated Order Submission | security    | The system MUST require authenticated identity tokens for order submission. | validation        | INT-example-00001 | block              | VAL-example-00004 |

---

## 19.0 Section 10 — Operational and Deployment Expectations

This section defines the environment in which the system is expected to operate. Operational expectations are required because autonomous construction cannot produce deployable artifacts without knowing runtime targets, infrastructure constraints, resource expectations, and monitoring obligations.

The goal of this section is not to prescribe a specific deployment technology. Instead, it provides enough structured expectation for the platform to generate, validate, or prepare deployment artifacts later in the lifecycle.

### 19.1 Required Fields

| Field                 | Type   | Required    | Description                                      |
| --------------------- | ------ | ----------- | ------------------------------------------------ |
| runtime-platform      | string | REQUIRED    | Target runtime platform and minimum version      |
| deployment-model      | enum   | REQUIRED    | container, serverless, vm, bare-metal, hybrid    |
| environments          | array  | REQUIRED    | Target environments such as dev, test, prod      |
| resource-requirements | object | REQUIRED    | CPU, memory, storage, network expectations       |
| scaling-model         | enum   | REQUIRED    | fixed, horizontal, vertical, auto                |
| availability-target   | string | CONDITIONAL | Required when availability is a stated objective |
| monitoring            | array  | REQUIRED    | Metrics, logs, traces, or alerts required        |
| backup-recovery       | string | CONDITIONAL | Required when persistent data is managed         |
| operational-owner     | string | REQUIRED    | Team or role responsible for operation           |

### 19.2 Rules

Operational expectations MUST define the target runtime platform and deployment model.

Systems managing persistent data MUST define backup or recovery expectations.

Systems with availability or reliability requirements MUST define monitoring and alerting expectations.

Deployment expectations MUST NOT contradict infrastructure constraints defined in the canonical Infrastructure entity.

### 19.3 Minimum Authored Form

| Field                 | Value                                            |
| --------------------- | ------------------------------------------------ |
| runtime-platform      | .NET 8 or later                                  |
| deployment-model      | container                                        |
| environments          | dev, test, prod                                  |
| resource-requirements | 1 CPU minimum, 512 MB memory minimum             |
| scaling-model         | horizontal                                       |
| monitoring            | request latency, error rate, health check status |
| operational-owner     | Platform Operations                              |

---

## 20.0 Section 11 — Validation and Acceptance Criteria

This section defines how the specification proves that the constructed system satisfies the stated requirements, policies, workflows, and operational expectations. Validation is the bridge between intent and evidence.

No requirement should be trusted simply because it is written. ISL requires acceptance criteria that can be evaluated by tests, deterministic tools, inspections, governance review, or operational checks.

### 20.1 Validation Fields

| Field            | Type   | Required    | Description                                                                                          |
| ---------------- | ------ | ----------- | ---------------------------------------------------------------------------------------------------- |
| id               | string | REQUIRED    | Validation identifier                                                                                |
| name             | string | REQUIRED    | Human-readable validation name                                                                       |
| validates        | array  | REQUIRED    | Requirement, policy, workflow, interface, or infrastructure identifiers                              |
| validation-type  | enum   | REQUIRED    | test, inspection, static-analysis, security-scan, policy-check, operational-check, governance-review |
| method           | string | REQUIRED    | How validation is performed                                                                          |
| pass-condition   | string | REQUIRED    | Condition required for success                                                                       |
| evidence         | string | CONDITIONAL | Evidence artifact or record expected                                                                 |
| automation-level | enum   | REQUIRED    | manual, semi-automated, automated                                                                    |

### 20.2 Rules

Each must-have functional requirement MUST be linked to at least one validation criterion.

Each policy with violation-response set to block MUST be linked to at least one validation criterion.

Each acceptance criterion MUST define a pass-condition.

Acceptance criteria without linked specification identifiers MUST be rejected during Machine-Valid evaluation.

### 20.3 Minimum Authored Form

| id                | name              | validates         | validation-type | method                                      | pass-condition                    | automation-level |
| ----------------- | ----------------- | ----------------- | --------------- | ------------------------------------------- | --------------------------------- | ---------------- |
| VAL-example-00001 | Submit Order Test | REQ-example-00001 | test            | Execute API test for valid order submission | API returns accepted order status | automated        |

---

## 21.0 Section 12 — Change History

This section records the version history of the specification. Because ISL treats the specification as the source of truth, every change to that source must be visible, attributable, and reviewable.

Change history supports governance review, readiness regression, impact analysis, and traceability across versions. A conforming platform MUST NOT process a specification that lacks a valid version identifier and change history after Draft readiness.

### 21.1 Required Fields

| Field             | Type     | Required | Description                                |
| ----------------- | -------- | -------- | ------------------------------------------ |
| version           | semver   | REQUIRED | Specification version                      |
| date              | ISO 8601 | REQUIRED | Date of change                             |
| author            | string   | REQUIRED | Person, role, or system making the change  |
| change-summary    | string   | REQUIRED | Summary of the change                      |
| affected-sections | array    | REQUIRED | Sections modified                          |
| readiness-impact  | enum     | REQUIRED | none, regression-required, review-required |

### 21.2 Rules

Every specification revision MUST increment the semantic version.

A change to requirements, policies, interfaces, data entities, or operational expectations MUST trigger readiness impact analysis as defined by ISL v1.3.

A specification version authorized for autonomous construction MUST be recorded by the governance system.

### 21.3 Minimum Authored Form

| version | date       | author                  | change-summary                 | affected-sections | readiness-impact |
| ------- | ---------- | ----------------------- | ------------------------------ | ----------------- | ---------------- |
| 1.0.0   | 2026-05-02 | Enterprise Architecture | Initial specification baseline | all               | review-required  |

---

## 22.0 Canonical Mapping Requirements

This section defines the mapping obligation between authored ISL sections and canonical semantic entities. It does not define the full canonical schema; that responsibility belongs to ISL v1.1. It does define what an ISL v1.0 processor must be able to extract from the authored language.

Canonical mapping is essential because the authored specification must become executable. If the platform cannot normalize the authored document into canonical entities and relationships, it cannot reliably plan, construct, validate, or govern the system.

### 22.1 Section-to-Entity Mapping

| Authored Section                        | Canonical Entity Types              |
| --------------------------------------- | ----------------------------------- |
| System Identity and Context             | Project, Context                    |
| Business Objectives and Scope           | Requirement, Capability, Context    |
| Actors and Stakeholders                 | Actor, Stakeholder                  |
| Functional Requirements                 | Requirement                         |
| Non-Functional Requirements             | Requirement, Policy                 |
| Data Model and Information Structures   | DataEntity                          |
| Service Boundaries and Interfaces       | Service, Interface                  |
| Workflows and Behavioral Processes      | Workflow                            |
| Security and Policy Constraints         | Policy                              |
| Operational and Deployment Expectations | Infrastructure, Policy              |
| Validation and Acceptance Criteria      | Validation                          |
| Change History                          | Project metadata, governance record |

### 22.2 Mapping Rules

A conforming ISL processor MUST map every required structured element to at least one canonical entity or canonical relationship.

If an authored element cannot be mapped, the processor MUST record a normalization error.

If a required relationship cannot be resolved, the specification MUST NOT reach Machine-Valid readiness.

### 22.3 Ambiguity Handling

When authored prose is ambiguous, the processor MUST NOT infer hidden requirements, policies, services, or validations.

Ambiguities MUST be recorded and resolved before the specification progresses to Autonomous-Ready readiness.

---

## 23.0 Structural Validation Rules

This section defines the minimum validation rules that apply to authored ISL specifications. These rules are language-level checks and are separate from deeper semantic validation performed by ISL v1.1.

Structural validation ensures that an authored specification is complete enough to be normalized and reviewed. It does not guarantee that the specification is semantically correct or construction-ready.

### 23.1 Required Structural Checks

A conforming ISL processor MUST verify:

* all required sections are present
* required sections appear in the defined order
* all required fields are populated
* all identifiers are unique within their entity type
* all enum values are valid
* all references point to defined identifiers
* all must-have requirements have validation references
* all policies have verifiable rules
* all workflows have exception paths when workflows are present
* all data relationships define cardinality

### 23.2 Failure Behavior

If structural validation fails, the processor MUST produce a validation report identifying:

| Field           | Description                                |
| --------------- | ------------------------------------------ |
| error-id        | Unique validation error identifier         |
| section         | Section where the failure occurred         |
| element-id      | Related element identifier, when available |
| severity        | error, warning, advisory                   |
| message         | Human-readable explanation                 |
| required-action | Action required to resolve the issue       |

A specification with one or more structural validation errors MUST NOT reach Machine-Valid readiness.

---

## 24.0 Language-Level Error Classes

This section defines language-level error classes. A broader runtime and platform error taxonomy may be defined in later ISL documents, but v1.0 must classify errors that occur during authoring, parsing, and structural validation.

Error classes allow tools to report failures consistently and allow governance reviewers to understand whether a specification failure is cosmetic, structural, semantic, or readiness-blocking.

| Error Class              | Description                                         | Blocks Machine-Valid |
| ------------------------ | --------------------------------------------------- | -------------------- |
| missing-section          | Required section is absent                          | YES                  |
| missing-field            | Required field is absent                            | YES                  |
| invalid-enum             | Field contains unsupported enum value               | YES                  |
| duplicate-identifier     | Identifier is reused incorrectly                    | YES                  |
| unresolved-reference     | Referenced element does not exist                   | YES                  |
| ambiguous-statement      | Required content is not clear enough to normalize   | YES                  |
| unverifiable-requirement | Requirement lacks validation method                 | YES                  |
| unverifiable-policy      | Policy lacks enforceable rule or validation         | YES                  |
| incomplete-workflow      | Workflow lacks steps or exception paths             | YES                  |
| advisory-quality         | Content is valid but lower quality than recommended | NO                   |

---

## 25.0 Readiness Relationship

This section defines how ISL v1.0 relates to the readiness model defined in ISL v1.3. ISL v1.0 does not itself assign final readiness levels, but it defines the authored content that readiness evaluation depends on.

A specification cannot become reviewable, machine-valid, or autonomous-ready unless the language-level requirements in this document are satisfied.

### 25.1 Draft Readiness

A Draft specification MAY contain incomplete sections, provisional identifiers, and unresolved questions.

A Draft specification SHOULD still use the required section structure to reduce future normalization effort.

### 25.2 Reviewable Readiness

To become Reviewable, a specification MUST include:

* complete System Identity and Context
* at least one measurable business objective
* at least one explicit scope exclusion
* at least one actor or stakeholder
* at least one functional requirement
* a valid semantic version
* a declared ISL version

### 25.3 Machine-Valid Readiness

To become Machine-Valid, a specification MUST satisfy all required structural validation rules in this document and all canonical normalization rules in ISL v1.1.

### 25.4 Autonomous-Ready Readiness

To become Autonomous-Ready, a specification MUST satisfy Machine-Valid readiness and all governance, traceability, validation, and authorization requirements defined by later ISL documents.

---

## 26.0 Extension Rules

This section defines how ISL specifications may be extended without breaking compatibility. Extensions are necessary because organizations may need domain-specific sections, industry-specific policies, or specialized architectural constructs.

Extensions are permitted only when they preserve the meaning of core ISL constructs and remain compatible with canonical normalization.

### 26.1 Extension Declaration

An extension MUST declare:

| Field         | Type   | Required | Description                       |
| ------------- | ------ | -------- | --------------------------------- |
| extension-id  | string | REQUIRED | Unique extension identifier       |
| name          | string | REQUIRED | Human-readable extension name     |
| version       | semver | REQUIRED | Extension version                 |
| owner         | string | REQUIRED | Extension owner                   |
| applies-to    | array  | REQUIRED | Sections or entity types extended |
| compatibility | string | REQUIRED | Supported ISL versions            |

### 26.2 Extension Constraints

An extension MUST NOT:

* remove required ISL sections
* redefine core enum values
* weaken validation requirements
* bypass readiness gates
* bypass governance controls
* break canonical mapping

### 26.3 Extension Processing

A conforming processor MAY reject specifications using unsupported extensions.

A processor that accepts an extension MUST preserve extension data during normalization or explicitly record unsupported fields as validation errors.

---

## 27.0 Minimum Complete Specification Checklist

This appendix-style section provides a practical checklist for authors and reviewers. It is not a substitute for automated validation, but it summarizes the minimum content expected before a specification is considered structurally complete.

A specification MUST satisfy all applicable checklist items before Machine-Valid readiness.

| Item                                                              | Required |
| ----------------------------------------------------------------- | -------- |
| System Identity block is complete                                 | YES      |
| Specification version uses semantic versioning                    | YES      |
| ISL version is declared                                           | YES      |
| At least one measurable business objective exists                 | YES      |
| At least one explicit scope exclusion exists                      | YES      |
| Actors and stakeholders are identified                            | YES      |
| Functional requirements are uniquely identified                   | YES      |
| Functional requirements use active voice                          | YES      |
| Functional requirements avoid ambiguous language                  | YES      |
| Non-functional requirements use measurable targets where possible | YES      |
| Non-functional requirements are quality-classified                | YES      |
| Data entities define attributes, types, and constraints           | YES      |
| Data relationships define cardinality                             | YES      |
| Services define single responsibilities                           | YES      |
| Interfaces define contracts and authentication                    | YES      |
| Workflows define exception paths when workflows exist             | YES      |
| Policies are expressed as verifiable rules                        | YES      |
| Operational expectations define runtime and deployment model      | YES      |
| Acceptance criteria are linked to requirements or policies        | YES      |
| Change history is present                                         | YES      |

---

## 28.0 Minimal Example Specification Fragment

This section provides a minimal authored example to clarify the expected language style. The example is intentionally small and does not represent a complete enterprise specification.

Examples are informative unless explicitly stated otherwise. However, the structure demonstrated here reflects normative syntax expectations.

# Example System Specification

**System ID:** example-system
**Specification Version:** 1.0.0
**ISL Version:** 1.0
**Status:** Draft
**Owner:** Enterprise Architecture
**Last Modified:** 2026-05-02

## 1. System Identity and Context

| Field           | Value                                                                                |
| --------------- | ------------------------------------------------------------------------------------ |
| system-id       | example-system                                                                       |
| name            | Example System                                                                       |
| version         | 1.0.0                                                                                |
| domain          | order-management                                                                     |
| owner           | Enterprise Architecture                                                              |
| description     | The system manages order intake, validation, and status tracking for internal users. |
| isl-version     | 1.0                                                                                  |
| readiness-level | draft                                                                                |

## 2. Business Objectives and Scope

| id                | statement                             | metric             | target        | source            | priority  |
| ----------------- | ------------------------------------- | ------------------ | ------------- | ----------------- | --------- |
| OBJ-example-00001 | Reduce manual order validation effort | manual review rate | less than 10% | STK-example-00001 | must-have |

* **in-scope:** order intake, order validation, order status tracking
* **out-of-scope:** payment processing, shipment execution

## 3. Actors and Stakeholders

| id                | name        | actor-type | description                                  | permissions                     |
| ----------------- | ----------- | ---------- | -------------------------------------------- | ------------------------------- |
| ACT-example-00001 | Order Clerk | human      | Internal user who submits and reviews orders | create-order, view-order-status |

| id                | name               | role           | concerns                         | approval-authority |
| ----------------- | ------------------ | -------------- | -------------------------------- | ------------------ |
| STK-example-00001 | Operations Manager | Business Owner | order accuracy, processing speed | true               |

## 4. Functional Requirements

| id                | statement                                                                                                 | priority  | source            | validation        |
| ----------------- | --------------------------------------------------------------------------------------------------------- | --------- | ----------------- | ----------------- |
| REQ-example-00001 | The system MUST allow an Order Clerk to submit a new order with customer, item, and quantity information. | must-have | ACT-example-00001 | VAL-example-00001 |

## 11. Validation and Acceptance Criteria

| id                | name              | validates         | validation-type | method                                      | pass-condition                    | automation-level |
| ----------------- | ----------------- | ----------------- | --------------- | ------------------------------------------- | --------------------------------- | ---------------- |
| VAL-example-00001 | Submit Order Test | REQ-example-00001 | test            | Execute API test for valid order submission | API returns accepted order status | automated        |

---

## 29.0 Conformance Requirements

This section defines what it means for an authored specification, parser, or authoring tool to conform to ISL v1.0.

Conformance is separated by role because a document, parser, and platform do not satisfy the same obligations. This separation allows partial tooling adoption while preserving a clear path toward full platform conformance.

### 29.1 Authored Specification Conformance

An authored specification conforms to ISL v1.0 if it:

* includes all required sections
* uses the required section order
* provides all required fields
* uses valid enum values
* links requirements to validation criteria
* expresses policies as verifiable rules
* declares version and ISL version
* can be normalized into ISL v1.1 canonical form

### 29.2 Parser Conformance

An ISL parser conforms to ISL v1.0 if it can:

* identify all required sections
* extract structured fields from authored Markdown
* validate required fields and enum values
* detect unresolved references
* report language-level errors using defined error classes
* produce canonical input suitable for ISL v1.1 normalization

### 29.3 Authoring Tool Conformance

An ISL authoring tool conforms to ISL v1.0 if it can:

* guide authors through required sections
* prevent or flag missing required fields
* validate enum values
* detect duplicate identifiers
* warn about ambiguous requirement language
* produce valid authored Markdown or canonical JSON

---

## 30.0 Summary

ISL v1.0 defines the authored specification language used to describe systems for autonomous construction. It establishes the required document structure, supported representations, authored Markdown syntax, section requirements, validation expectations, extension rules, and conformance requirements.

A valid ISL specification is not merely documentation. It is a structured system blueprint that must be capable of normalization, validation, traceability, governance review, and eventual autonomous construction.

The primary achievement of this revision is that the language surface is now explicit enough to support implementation. It defines not only what sections are required, but how those sections must be authored, validated, mapped, and rejected when incomplete.
