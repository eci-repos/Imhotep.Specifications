# IMHOTEP Specification Language (ISL) v1.3

# Specification Readiness Levels

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2
**Supersedes:** ISL r1 v1.3 Specification Readiness Levels
**Document Type:** Readiness and Gating Specification

---

## 1.0 Scope

This document defines the Specification Readiness Levels used by the IMHOTEP Specification Language. Readiness levels establish the maturity, completeness, validation state, and authorization posture of an ISL specification. They determine whether a specification may remain in authoring, proceed to review, undergo machine validation, or authorize autonomous construction.

This document applies to authored specifications, canonical specifications, readiness evaluators, governance workflows, and execution runtimes. It defines readiness states, progression criteria, regression rules, readiness records, evaluation reports, approval requirements, and conformance obligations.

This document does not define the authored specification syntax, canonical semantic schemas, construction planning model, execution lifecycle, or governance role definitions. Those are defined elsewhere in the ISL corpus. This document defines when a specification is mature enough to move between lifecycle states.

A conforming implementation MUST use this model to ensure that:

* incomplete specifications do not proceed to construction
* human review occurs before machine validation
* semantic validation occurs before autonomous authorization
* governance approval occurs before execution
* changes trigger readiness regression when applicable
* all readiness decisions are auditable

---

## 2.0 Purpose of Specification Readiness Levels

Specification Readiness Levels provide a controlled maturity model for ISL specifications. They prevent the platform from treating every specification document as equally reliable, complete, or executable. Instead, each specification must progress through defined gates before autonomous construction is permitted.

The purpose of readiness levels is to make specification maturity measurable and enforceable. A specification that is readable by humans is not necessarily machine-valid. A specification that is machine-valid is not necessarily authorized for construction. A specification that is authorized for construction may later regress if meaningful changes are introduced.

Readiness levels exist to ensure that:

* authors can draft specifications without prematurely triggering construction
* reviewers can evaluate specifications before automation depends on them
* canonical validation can determine structural and semantic correctness
* governance authorities can authorize construction explicitly
* execution runtimes can rely on readiness as a hard precondition
* specification changes are controlled through regression and reauthorization

---

## 3.0 Normative References

This section identifies the documents required to interpret and apply readiness levels. Readiness is not a standalone concept. It depends on authored structure, canonical validation, execution preconditions, traceability, governance approval, and policy enforcement.

| Reference | Purpose                                                          |
| --------- | ---------------------------------------------------------------- |
| ISL v0.0  | Corpus-level principles, conformance, and system integrity rules |
| ISL v1.0  | Authored specification structure and language-level validation   |
| ISL v1.1  | Canonical semantic model and semantic validation                 |
| ISL v1.2  | Execution preconditions and execution gating                     |
| ISL v1.4  | Traceability requirements and impact analysis                    |
| ISL v1.7  | Governance roles, approval gates, waivers, and overrides         |
| RFC 2119  | Normative terminology                                            |

---

## 4.0 Terms and Definitions

This section defines terms specific to readiness evaluation. These terms are used by authoring tools, canonical validators, governance engines, and execution runtimes.

**Readiness Level**
A defined maturity state assigned to a specification based on satisfaction of required criteria.

**Readiness Transition**
A movement from one readiness level to another.

**Exit Criterion**
A measurable condition that must be satisfied before a specification may advance from one readiness level to the next.

**Regression**
A readiness downgrade caused by a specification change that invalidates previously satisfied criteria.

**Readiness Evaluation**
A structured process that checks whether a specification satisfies readiness criteria.

**Readiness Report**
The structured output of readiness evaluation.

**Readiness Override**
A governance-authorized exception allowing progression despite one or more unsatisfied criteria.

**Construction Authorization**
Governance approval allowing an Autonomous-Ready specification to enter execution.

---

## 5.0 Readiness Design Principles

This section defines the principles governing readiness evaluation. These principles ensure that readiness is not symbolic or subjective, but a controlled state derived from evidence.

### 5.1 Readiness Is a Gate

Readiness levels MUST control platform behavior. They MUST NOT be treated as descriptive labels only.

A platform MUST enforce readiness restrictions when authoring, validating, planning, executing, or deploying specification-derived systems.

### 5.2 Criteria Must Be Verifiable

Every readiness transition MUST be based on explicit criteria. Criteria MUST be verifiable through automated checks, governance records, or documented human review.

A criterion that cannot be verified MUST NOT be used as a mandatory readiness gate unless it is converted into a structured review requirement.

### 5.3 Higher Readiness Requires Lower Readiness

A specification MUST satisfy all lower-level criteria before it may advance to a higher readiness level.

A specification MUST NOT skip readiness levels.

### 5.4 Readiness Is Version-Specific

Readiness applies to a specific specification version. When a specification changes, the previous readiness decision does not automatically apply to the new version.

Every readiness record MUST identify the specification version to which it applies.

### 5.5 Readiness Is Governed

Readiness transitions that affect construction authorization MUST be governed. Human approval, separation of duties, waivers, and overrides MUST be enforced according to ISL v1.7.

### 5.6 Regression Is Mandatory When Criteria Are Invalidated

If a change invalidates criteria required for a readiness level, the specification MUST regress. Regression is not optional.

---

## 6.0 Readiness Level Overview

This section defines the four readiness levels. These levels provide a simple but strict progression from active authoring to autonomous construction authorization.

| Level | Name             | Meaning                                                               | Autonomous Construction Permitted |
| ----- | ---------------- | --------------------------------------------------------------------- | --------------------------------- |
| 0     | Draft            | Specification is incomplete or actively authored                      | NO                                |
| 1     | Reviewable       | Specification is structurally coherent and ready for human review     | NO                                |
| 2     | Machine-Valid    | Specification is structurally and semantically valid                  | NO                                |
| 3     | Autonomous-Ready | Specification is validated, approved, and authorized for construction | YES                               |

The readiness level MUST be recorded in the Project entity’s `readiness-level` field as defined by ISL v1.1.

---

## 7.0 Readiness State Rules

This section defines general state rules that apply to all readiness levels. These rules prevent inconsistent lifecycle behavior and ensure readiness can be audited.

### 7.1 Single Current Readiness Level

A specification version MUST have exactly one current readiness level.

A platform MUST NOT mark the same specification version as both Machine-Valid and Autonomous-Ready unless Autonomous-Ready is represented as the current state and Machine-Valid is preserved as a satisfied historical transition.

### 7.2 Forward Progression

Standard forward progression MUST follow this sequence:

Draft → Reviewable → Machine-Valid → Autonomous-Ready

No other forward progression path is permitted.

### 7.3 Backward Regression

Regression MAY move a specification backward by one or more levels depending on the impact of a change. Regression MUST be recorded as a readiness transition.

### 7.4 Readiness Immutability for Historical Versions

A readiness decision for a prior specification version MUST remain auditable. A platform MUST NOT overwrite historical readiness records when a later version regresses.

### 7.5 Construction Blocking

The platform MUST NOT initiate construction planning or execution unless the specification version has reached the readiness level required by the applicable ISL document.

Autonomous artifact generation MUST NOT begin unless the specification is Autonomous-Ready.

---

## 8.0 Level 0 — Draft

### 8.1 Purpose

The Draft level represents a specification in active authoring. At this level, the specification may be incomplete, ambiguous, internally inconsistent, or missing required details. Draft status allows authors to capture intent without prematurely subjecting the specification to full validation or governance burden.

Draft is intentionally permissive, but it is not meaningless. A Draft specification should still follow ISL structure where possible so that it can progress efficiently toward review and validation.

### 8.2 Platform Behavior

At Draft readiness, the platform MAY:

* provide authoring assistance
* detect missing sections
* identify ambiguous language
* recommend identifiers
* perform advisory structural checks
* generate preliminary quality warnings

At Draft readiness, the platform MUST NOT:

* initiate construction planning
* generate implementation artifacts
* mark canonical entities as construction-ready
* authorize autonomous execution
* treat provisional identifiers as stable traceability identifiers

### 8.3 Draft Minimum Conditions

A specification may exist in Draft when it contains at minimum:

| Condition                                              | Required for Draft Recognition |
| ------------------------------------------------------ | ------------------------------ |
| Document title exists                                  | YES                            |
| System identity is partially present                   | YES                            |
| Specification version is present or marked provisional | YES                            |
| ISL version is declared or marked unknown              | YES                            |
| Owner or authoring unit is identified                  | SHOULD                         |

A document that lacks enough structure to identify the system MUST NOT be treated as an ISL Draft specification. It MAY be treated as informal source material.

### 8.4 Draft Exit Criteria

A specification MUST satisfy all of the following before it may advance from Draft to Reviewable.

| Criterion                                             | Verification Method     | Blocking |
| ----------------------------------------------------- | ----------------------- | -------- |
| System Identity and Context section is present        | Automated section check | YES      |
| All required System Identity fields are populated     | Automated field check   | YES      |
| Specification version is valid semantic version       | Automated format check  | YES      |
| ISL version references a supported ISL version        | Automated lookup        | YES      |
| At least one measurable business objective is defined | Automated content check | YES      |
| At least one explicit scope exclusion is defined      | Automated content check | YES      |
| At least one actor or stakeholder is defined          | Automated field check   | YES      |
| At least one functional requirement is defined        | Automated field check   | YES      |
| Change History section exists                         | Automated section check | YES      |

### 8.5 Draft Exit Record

When a specification exits Draft, the platform MUST produce a readiness transition record identifying the criteria evaluated and the evidence used.

---

## 9.0 Level 1 — Reviewable

### 9.1 Purpose

The Reviewable level represents a specification that is sufficiently structured and complete for meaningful human evaluation. It does not mean the specification is machine-valid or ready for construction. It means the document has enough content for domain experts, architects, security reviewers, operators, and governance stakeholders to evaluate correctness, intent, and completeness.

Reviewable status is an important enterprise control point. It ensures that human judgment is applied before the platform performs deeper semantic validation or prepares the specification for automation.

### 9.2 Platform Behavior

At Reviewable readiness, the platform MAY:

* perform structural validation
* perform preliminary canonical mapping
* generate review reports
* identify unresolved references
* identify missing validation links
* identify policy gaps
* identify standards alignment gaps
* route the specification for human review

At Reviewable readiness, the platform MUST NOT:

* initiate autonomous construction
* execute artifact generation
* treat preliminary canonical output as authoritative
* bypass required reviewer sign-off

### 9.3 Reviewable Exit Criteria

A specification MUST satisfy all of the following before it may advance from Reviewable to Machine-Valid.

| Criterion                                                                             | Verification Method                  | Blocking |
| ------------------------------------------------------------------------------------- | ------------------------------------ | -------- |
| All required ISL v1.0 sections are present                                            | Automated section check              | YES      |
| All required fields are populated                                                     | Automated schema check               | YES      |
| Functional requirements carry identifiers, priority, source, and validation reference | Automated schema check               | YES      |
| Functional requirements use active voice and avoid prohibited ambiguous terms         | Automated language rule check        | YES      |
| Non-functional requirements include measurable targets where measurable               | Automated/schema plus reviewer check | YES      |
| Non-functional requirements are classified by quality characteristic                  | Automated schema check               | YES      |
| Actors are typed and define permissions                                               | Automated schema check               | YES      |
| Stakeholders with approval authority map to governance roles                          | Governance configuration check       | YES      |
| Data entities define attributes, types, nullability, and constraints where applicable | Automated schema check               | YES      |
| Data relationships define cardinality                                                 | Automated schema check               | YES      |
| Services define single responsibility statements                                      | Automated schema check               | YES      |
| Interfaces define contract and authentication                                         | Automated schema check               | YES      |
| Workflows define steps and exception paths when workflows are required                | Automated schema check               | YES      |
| Policies are expressed as verifiable rules                                            | Automated/schema plus reviewer check | YES      |
| Acceptance criteria are linked to specification identifiers                           | Automated reference check            | YES      |
| Change history is complete for current version                                        | Automated field check                | YES      |
| Reviewer sign-off has been recorded                                                   | Governance record check              | YES      |

### 9.4 Reviewer Sign-Off

At least one designated Reviewer MUST approve the transition from Reviewable to Machine-Valid. The Reviewer role MUST be defined in governance configuration according to ISL v1.7.

Reviewer sign-off MUST confirm that:

* the specification is coherent
* business objectives are understandable
* requirements are reviewable
* scope boundaries are explicit
* no obvious domain contradictions remain
* the specification is ready for formal machine validation

Reviewer sign-off does not replace automated validation.

### 9.5 Review Findings

Review findings SHOULD be recorded as structured records.

| Field       | Type     | Required    | Description                             |
| ----------- | -------- | ----------- | --------------------------------------- |
| finding-id  | string   | REQUIRED    | Unique review finding identifier        |
| section     | string   | REQUIRED    | Section reviewed                        |
| entity-id   | string   | CONDITIONAL | Related entity identifier               |
| severity    | enum     | REQUIRED    | blocking, high, medium, low, advisory   |
| finding     | string   | REQUIRED    | Description of issue or observation     |
| disposition | enum     | REQUIRED    | open, resolved, accepted-risk, deferred |
| reviewer    | string   | REQUIRED    | Reviewer role or identity reference     |
| recorded-at | ISO 8601 | REQUIRED    | Time finding was recorded               |

Blocking findings MUST be resolved or formally waived before Machine-Valid readiness.

---

## 10.0 Level 2 — Machine-Valid

### 10.1 Purpose

The Machine-Valid level represents a specification that has passed structural and semantic validation. At this level, the specification has been normalized into the canonical semantic model, required references resolve, required relationships exist, policies are verifiable, and validation coverage is sufficient for the next readiness gate.

Machine-Valid does not authorize autonomous construction. It means the specification is technically valid enough to support planning, traceability analysis, impact analysis, and governance review for construction authorization.

### 10.2 Platform Behavior

At Machine-Valid readiness, the platform MAY:

* generate or update a construction plan
* perform impact analysis
* evaluate traceability coverage
* perform governance readiness review
* prepare construction authorization materials
* validate tool capability requirements

At Machine-Valid readiness, the platform MUST NOT:

* begin artifact generation
* initiate autonomous execution
* treat construction authorization as implied
* bypass final approval gates

### 10.3 Machine-Valid Entry Requirements

A specification MUST NOT be marked Machine-Valid unless:

* Reviewable exit criteria are satisfied
* canonical normalization completed successfully
* canonical validation produced no blocking errors
* reviewer sign-off is recorded
* structural validation produced no blocking errors

### 10.4 Machine-Valid Exit Criteria

A specification MUST satisfy all of the following before it may advance from Machine-Valid to Autonomous-Ready.

| Criterion                                                                                                       | Verification Method            | Blocking |
| --------------------------------------------------------------------------------------------------------------- | ------------------------------ | -------- |
| Canonical normalization completes without blocking errors                                                       | Canonical validation report    | YES      |
| All entity identifiers conform to ISL v1.1 identifier format                                                    | Automated format check         | YES      |
| All relationship references resolve                                                                             | Automated reference resolution | YES      |
| Every non-Project entity is reachable from Project                                                              | Graph validation               | YES      |
| All must-have functional requirements have linked Validation entities                                           | Automated coverage check       | YES      |
| Must-have functional requirements trace to at least one Service or Capability                                   | Traceability check             | YES      |
| Non-functional requirements have validation methods or reviewable compliance methods                            | Automated plus reviewer check  | YES      |
| All DataEntity attributes define type and nullability                                                           | Automated schema check         | YES      |
| Sensitive or regulated DataEntity entities are governed by Policy entities                                      | Policy coverage check          | YES      |
| All Workflow entities define exception paths and terminal states                                                | Automated schema check         | YES      |
| All Interface entities define contract, authentication, and versioning strategy                                 | Automated schema check         | YES      |
| All Infrastructure entities define runtime platform, deployment model, resource requirements, and scaling model | Automated schema check         | YES      |
| All Policy entities define verifiable rules and enforcement points                                              | Automated schema check         | YES      |
| Security Reviewer sign-off has been recorded                                                                    | Governance record check        | YES      |
| Architecture Reviewer sign-off has been recorded                                                                | Governance record check        | YES      |
| Risk tier has been assigned                                                                                     | Governance/risk check          | YES      |
| Required deterministic tool capabilities have been identified                                                   | Tool capability analysis       | YES      |
| No open blocking review findings remain                                                                         | Review finding check           | YES      |

### 10.5 Machine-Valid Evidence

Machine-Valid status MUST be supported by evidence records including:

* structural validation report
* canonical validation report
* reference resolution report
* validation coverage report
* policy coverage report
* traceability coverage report
* reviewer sign-off record
* security reviewer sign-off record, when applicable
* architecture reviewer sign-off record, when applicable

---

## 11.0 Level 3 — Autonomous-Ready

### 11.1 Purpose

The Autonomous-Ready level represents a specification that is complete, valid, governed, and authorized for autonomous construction. This is the only readiness level at which the platform may begin full autonomous construction execution.

Autonomous-Ready status is a high-control state. It means the specification has satisfied all required authoring, structural, semantic, validation, traceability, policy, risk, and governance criteria. It also means the organization has accepted the risk of allowing the platform to construct artifacts from the specification.

### 11.2 Platform Behavior

At Autonomous-Ready readiness, the platform MAY:

* initiate construction planning if not already generated
* confirm construction plan executability
* begin execution under ISL v1.2
* generate artifacts
* run deterministic validation
* enter bounded repair cycles
* perform policy validation
* prepare deployment artifacts

At Autonomous-Ready readiness, the platform MUST:

* enforce execution preconditions
* preserve traceability
* enforce governance checkpoints
* enforce repair termination rules
* record execution activity

### 11.3 Autonomous-Ready Entry Criteria

A specification MUST NOT be marked Autonomous-Ready unless all Machine-Valid exit criteria are satisfied and the following additional criteria are met.

| Criterion                                                                     | Verification Method            | Blocking |
| ----------------------------------------------------------------------------- | ------------------------------ | -------- |
| Construction authorization has been recorded by Authorizing Official          | Governance approval record     | YES      |
| Risk tier is approved for autonomous construction                             | Governance/risk check          | YES      |
| Required Tool Records exist for planned deterministic validation capabilities | Tool registry check            | YES      |
| Construction plan exists or can be generated from canonical model             | Planning engine check          | YES      |
| No unresolved interpretation ambiguities are recorded                         | Interpretation/ambiguity check | YES      |
| All blocking policies have validation or enforcement mechanisms               | Policy validation check        | YES      |
| Required waivers, if any, are approved and unexpired                          | Governance waiver check        | YES      |
| Separation-of-duties rules are satisfied                                      | Governance role check          | YES      |
| Execution runtime configuration is compatible with specification risk tier    | Runtime configuration check    | YES      |
| Artifact repository target is configured                                      | Repository configuration check | YES      |
| Traceability graph is initialized                                             | Traceability state check       | YES      |

### 11.4 Autonomous Construction Permission

Autonomous construction is permitted only for the exact specification version marked Autonomous-Ready.

A later modification to the specification MUST NOT inherit Autonomous-Ready status unless the change is classified as non-impacting and governance rules allow preservation of readiness.

### 11.5 Autonomous-Ready Evidence

Autonomous-Ready status MUST be supported by:

* Machine-Valid evidence package
* construction authorization record
* risk tier record
* construction plan or planning readiness evidence
* governance approval records
* waiver records, if applicable
* tool capability readiness record
* traceability initialization record
* execution precondition readiness record

---

## 12.0 Readiness Transition Records

This section defines the required record produced for every readiness transition. Readiness transitions are governance-relevant events and must be auditable.

A transition record proves that a specification moved from one maturity state to another based on evidence.

### 12.1 Transition Record Schema

| Field                   | Type     | Required    | Description                                   |
| ----------------------- | -------- | ----------- | --------------------------------------------- |
| transition-id           | string   | REQUIRED    | Unique transition identifier                  |
| specification-id        | string   | REQUIRED    | System identifier                             |
| specification-version   | semver   | REQUIRED    | Specification version                         |
| from-level              | integer  | REQUIRED    | Previous readiness level                      |
| to-level                | integer  | REQUIRED    | New readiness level                           |
| transition-type         | enum     | REQUIRED    | progression, regression, override, correction |
| criteria-evaluated      | array    | REQUIRED    | Criteria evaluated                            |
| criteria-passed         | array    | REQUIRED    | Criteria satisfied                            |
| criteria-failed         | array    | CONDITIONAL | Criteria not satisfied                        |
| evidence-records        | array    | REQUIRED    | Evidence supporting transition                |
| authorized-by           | string   | CONDITIONAL | Required when transition requires approval    |
| automated-checks-passed | boolean  | REQUIRED    | Whether automated checks passed               |
| override-id             | string   | CONDITIONAL | Required when transition uses override        |
| transitioned-at         | ISO 8601 | REQUIRED    | Time transition occurred                      |
| notes                   | string   | CONDITIONAL | Explanation or conditions                     |

### 12.2 Transition Record Rules

Every readiness transition MUST produce a transition record.

A transition record MUST identify the specification version.

A transition record MUST NOT be overwritten.

A transition using an override MUST reference an approved override record.

A transition to Autonomous-Ready MUST reference governance authorization.

---

## 13.0 Readiness Evaluation Process

This section defines the process for evaluating readiness. Readiness evaluation is the structured activity that determines whether a specification satisfies the criteria for its current or next level.

Evaluation MAY be triggered manually by an author or reviewer, automatically by a platform, or as part of a governance workflow.

### 13.1 Evaluation Stages

Readiness evaluation SHOULD proceed through the following stages:

| Stage                       | Purpose                                               |
| --------------------------- | ----------------------------------------------------- |
| 1. Identify Current Level   | Determine current readiness level from Project entity |
| 2. Select Target Level      | Determine requested transition target                 |
| 3. Validate Transition Path | Ensure transition path is permitted                   |
| 4. Evaluate Criteria        | Check all criteria for target transition              |
| 5. Collect Evidence         | Record validation reports and approvals               |
| 6. Determine Outcome        | passed, failed, blocked, override-required            |
| 7. Produce Readiness Report | Emit structured result                                |
| 8. Record Transition        | Persist transition if approved                        |

### 13.2 Evaluation Rules

A readiness evaluator MUST evaluate all criteria applicable to the requested transition.

A readiness evaluator MUST distinguish between automated checks, manual review checks, and governance approval checks.

A readiness evaluator MUST NOT mark a transition passed if any blocking criterion fails unless an approved override exists.

A readiness evaluator MUST produce a Readiness Report whether evaluation passes or fails.

---

## 14.0 Readiness Report

This section defines the structured output of readiness evaluation. Readiness Reports provide the evidence needed for authors, reviewers, governance authorities, and execution runtimes to understand specification maturity.

A Readiness Report is not the same as a transition record. A report may show that a transition failed. A transition record is produced only when readiness state changes.

### 14.1 Readiness Report Schema

| Field                 | Type     | Required    | Description                                              |
| --------------------- | -------- | ----------- | -------------------------------------------------------- |
| report-id             | string   | REQUIRED    | Unique readiness report identifier                       |
| specification-id      | string   | REQUIRED    | System identifier                                        |
| specification-version | semver   | REQUIRED    | Specification version evaluated                          |
| current-level         | integer  | REQUIRED    | Current readiness level                                  |
| target-level          | integer  | CONDITIONAL | Requested target level                                   |
| evaluation-outcome    | enum     | REQUIRED    | passed, failed, blocked, override-required               |
| criteria-results      | array    | REQUIRED    | Result of each criterion                                 |
| blocking-failures     | array    | CONDITIONAL | Blocking criteria that failed                            |
| warnings              | array    | CONDITIONAL | Non-blocking concerns                                    |
| evidence-records      | array    | REQUIRED    | Validation, review, governance, or traceability evidence |
| evaluated-at          | ISO 8601 | REQUIRED    | Time evaluation occurred                                 |
| evaluated-by          | string   | REQUIRED    | Tool, role, or platform component performing evaluation  |

### 14.2 Criterion Result Schema

| Field               | Type    | Required    | Description                                   |
| ------------------- | ------- | ----------- | --------------------------------------------- |
| criterion-id        | string  | REQUIRED    | Unique criterion identifier                   |
| description         | string  | REQUIRED    | Criterion evaluated                           |
| result              | enum    | REQUIRED    | passed, failed, not-applicable, not-evaluated |
| verification-method | string  | REQUIRED    | How criterion was checked                     |
| evidence            | string  | CONDITIONAL | Supporting record                             |
| blocking            | boolean | REQUIRED    | Whether failure blocks transition             |
| remediation         | string  | CONDITIONAL | Required action when failed                   |

### 14.3 Report Rules

A Readiness Report MUST include all criteria applicable to the target transition.

A Readiness Report MUST identify blocking failures separately from warnings.

A failed Readiness Report MUST include remediation guidance for blocking failures where determinable.

---

## 15.0 Governance Approval Requirements

This section defines how readiness levels interact with governance approvals. Readiness progression is not purely technical; some transitions require human authority.

Governance approvals are defined in detail in ISL v1.7, but this document establishes which readiness transitions require approval.

### 15.1 Required Approval Mapping

| Transition                       | Required Approval                                                           |
| -------------------------------- | --------------------------------------------------------------------------- |
| Draft → Reviewable               | Reviewer acknowledgement MAY be required by governance profile              |
| Reviewable → Machine-Valid       | Reviewer sign-off REQUIRED                                                  |
| Machine-Valid → Autonomous-Ready | Security Reviewer, Architecture Reviewer, and Authorizing Official REQUIRED |
| Regression                       | Governance event REQUIRED                                                   |
| Override                         | Override Authority REQUIRED                                                 |
| Post-change reauthorization      | Authorizing Official REQUIRED when Autonomous-Ready is affected             |

### 15.2 Separation of Duties

For transitions to Autonomous-Ready, the Authorizing Official MUST be distinct from the Reviewer and Security Reviewer.

Where governance requires Architecture Reviewer approval, the Architecture Reviewer MUST be distinct from the Authorizing Official unless explicitly allowed by governance policy for low-risk systems.

### 15.3 Approval Record Dependency

A readiness transition requiring approval MUST NOT complete unless the approval record exists and references the same specification version.

---

## 16.0 Readiness Regression

This section defines when a specification must move backward to an earlier readiness level. Regression ensures that previously valid approval or validation decisions are not improperly reused after meaningful changes.

Regression is essential because ISL specifications are living artifacts. A change to requirements, policies, data models, interfaces, or infrastructure expectations may invalidate prior review, validation, or authorization.

### 16.1 Regression Principles

A specification MUST regress when a change invalidates criteria required for its current readiness level.

Regression applies to the modified specification version, not the historical version.

Regression MUST be recorded as a readiness transition.

Regression MUST trigger impact analysis when traceability exists.

### 16.2 Mandatory Regression Rules

| Change Type                                                | Mandatory Regression To | Reason                                           |
| ---------------------------------------------------------- | ----------------------- | ------------------------------------------------ |
| Change to System Identity required fields                  | Reviewable              | Identity and governance context must be reviewed |
| Addition of must-have functional requirement               | Reviewable              | Business and validation review required          |
| Removal of must-have functional requirement                | Reviewable              | Scope and acceptance impact                      |
| Change to functional requirement statement                 | Reviewable              | Behavior changed                                 |
| Change to non-functional requirement target                | Reviewable              | Quality expectation changed                      |
| Change to security policy rule                             | Machine-Valid           | Policy validation and sign-off required          |
| Change to policy violation response                        | Machine-Valid           | Governance enforcement changed                   |
| Change to DataEntity attribute type or nullability         | Machine-Valid           | Semantic and schema impact                       |
| Change to sensitive data classification                    | Machine-Valid           | Policy coverage impact                           |
| Change to Interface contract reference                     | Machine-Valid           | Integration impact                               |
| Change to authentication or authorization rule             | Machine-Valid           | Security impact                                  |
| Change to Infrastructure deployment model                  | Machine-Valid           | Operational impact                               |
| Change invalidating canonical normalization                | Draft or Reviewable     | Structural correction required                   |
| Change after Autonomous-Ready affecting construction scope | Machine-Valid           | Reauthorization required                         |
| Removal of validation for must-have requirement            | Machine-Valid           | Validation coverage invalidated                  |

### 16.3 Regression Severity

Regression severity SHOULD be classified as:

| Severity | Meaning                                                         |
| -------- | --------------------------------------------------------------- |
| minor    | No readiness regression required, but review note recorded      |
| moderate | Regression to Reviewable or Machine-Valid required              |
| major    | Autonomous-Ready authorization invalidated                      |
| critical | Specification must return to Draft due to structural invalidity |

### 16.4 Regression Record

A regression MUST produce a readiness transition record with transition-type `regression`.

The record MUST include:

* changed elements
* prior readiness level
* new readiness level
* reason for regression
* impact analysis reference when available
* governance event reference

---

## 17.0 Post-Change Reauthorization

This section defines the requirements for restoring readiness after a specification change. Reauthorization is the process by which a modified specification regains a readiness level that was invalidated by regression.

A specification that has regressed MUST satisfy all criteria for the target readiness level again. Prior approvals MAY be referenced as historical evidence, but they MUST NOT substitute for required approvals when the change affects the approved content.

### 17.1 Reauthorization Rules

A specification that regresses from Autonomous-Ready MUST NOT resume execution until:

* impact analysis is complete
* canonical validation passes
* affected validation coverage is restored
* affected governance approvals are renewed
* construction authorization is reissued when required

### 17.2 Reuse of Evidence

Evidence from prior versions MAY be reused only when:

* the relevant specification elements are unchanged
* the evidence applies to the same semantic content
* governance policy permits reuse
* the Readiness Report identifies reused evidence explicitly

### 17.3 Reauthorization Record

Post-change reauthorization MUST produce a readiness transition record and governance approval record when returning to Autonomous-Ready.

---

## 18.0 Overrides and Waivers

This section defines how exceptional readiness progression may occur when a criterion cannot be satisfied. Overrides and waivers are governance-controlled exceptions, not informal shortcuts.

Readiness overrides are risky because they allow progression despite incomplete criteria. Therefore, they require explicit authority, justification, compensating controls, and audit records.

### 18.1 Override Conditions

An override MAY be requested when:

* a criterion cannot be satisfied due to temporary external constraint
* an automated check produces a known false positive
* governance accepts documented risk
* compensating controls are defined
* the Override Authority approves the exception

### 18.2 Override Prohibitions

An override MUST NOT be used to bypass:

* missing system identity
* invalid specification version
* absence of canonical model for Machine-Valid readiness
* unresolved critical security policy violation without compensating controls
* missing construction authorization for Autonomous-Ready readiness
* separation-of-duties requirements

### 18.3 Override Record Schema

| Field                 | Type     | Required | Description                      |
| --------------------- | -------- | -------- | -------------------------------- |
| override-id           | string   | REQUIRED | Unique override identifier       |
| specification-id      | string   | REQUIRED | System identifier                |
| specification-version | semver   | REQUIRED | Specification version            |
| criterion-id          | string   | REQUIRED | Criterion overridden             |
| justification         | string   | REQUIRED | Reason override is necessary     |
| compensating-controls | string   | REQUIRED | Controls mitigating the risk     |
| approved-by           | string   | REQUIRED | Override Authority               |
| approved-at           | ISO 8601 | REQUIRED | Time approved                    |
| expiry                | ISO 8601 | REQUIRED | Expiration date                  |
| review-date           | ISO 8601 | REQUIRED | Date for reassessment            |
| status                | enum     | REQUIRED | active, expired, revoked, closed |

### 18.4 Override Rules

An override MUST have an expiry date.

An override MUST be reviewed by its review date.

An expired override MUST NOT support readiness progression.

A readiness transition relying on an override MUST reference the override record.

---

## 19.0 Automated Readiness Evaluation

This section defines the requirements for automated readiness evaluation. Automated evaluation enables repeatability, consistency, and integration with authoring tools, governance workflows, and execution preconditions.

Automated evaluation does not eliminate human review. Instead, it ensures that objective criteria are checked consistently and that human reviewers focus on judgment-based concerns.

### 19.1 Required Automated Checks

A conforming readiness evaluator MUST support automated checks for:

* required section presence
* required field presence
* semantic version validity
* ISL version support
* identifier format
* duplicate identifiers
* enum validity
* unresolved references
* canonical normalization outcome
* required relationship presence
* validation coverage
* policy verifiability
* interface completeness
* workflow completeness
* data model completeness
* readiness regression triggers

### 19.2 Manual or Governance Checks

The following checks MAY require human or governance evaluation:

* business objective quality
* domain correctness
* architecture suitability
* security risk acceptability
* policy adequacy
* waiver justification
* construction authorization
* risk tier approval

### 19.3 Evaluation Determinism

For the same specification version, same ISL version, same governance profile, and same evaluator version, automated readiness evaluation SHOULD produce the same result.

If results differ, the evaluator MUST record version or configuration differences explaining the divergence.

---

## 20.0 Risk Tier Relationship

This section defines the relationship between readiness and risk tier. Risk tier determines the level of governance scrutiny required before a specification may become Autonomous-Ready.

Risk tiers are defined in ISL v1.7, but this document establishes that risk tier assignment is a readiness dependency.

### 20.1 Risk Tier Requirement

A specification MUST have an assigned risk tier before it may reach Autonomous-Ready.

### 20.2 Risk-Based Readiness Constraints

| Risk Tier | Additional Readiness Expectation                                            |
| --------- | --------------------------------------------------------------------------- |
| Standard  | Default readiness criteria apply                                            |
| Elevated  | Security policy coverage and Security Reviewer sign-off required            |
| High      | Independent security or architecture review SHOULD be required              |
| Critical  | External or specialized authorization MAY be required by governance profile |

### 20.3 Risk Tier Change

A risk tier increase MUST trigger readiness reevaluation.

A risk tier reduction MUST require governance approval.

---

## 21.0 Readiness and Construction Planning

This section defines how readiness affects construction planning. Planning is not the same as execution. Some planning activities may be permitted earlier than artifact generation, depending on readiness level.

The platform must distinguish advisory planning, formal construction planning, and executable planning.

### 21.1 Planning Permissions by Readiness Level

| Readiness Level  | Planning Permission                                              |
| ---------------- | ---------------------------------------------------------------- |
| Draft            | Advisory gap analysis only                                       |
| Reviewable       | Preliminary planning MAY be performed but MUST NOT be executable |
| Machine-Valid    | Formal construction planning MAY be performed                    |
| Autonomous-Ready | Executable construction planning and execution MAY proceed       |

### 21.2 Planning Restrictions

A construction plan generated before Autonomous-Ready MUST NOT be executed until the specification reaches Autonomous-Ready and execution preconditions are satisfied.

If a specification changes after a construction plan is generated, readiness regression and impact analysis MUST determine whether the plan remains valid.

---

## 22.0 Readiness and Execution

This section defines how readiness gates execution. The execution model depends on readiness because autonomous construction must not begin from incomplete or unauthorized specifications.

Execution preconditions are defined in ISL v1.2, but this document establishes the readiness prerequisite.

### 22.1 Execution Permission

Autonomous execution MUST NOT begin unless:

* the specification version is Autonomous-Ready
* construction authorization is recorded
* execution preconditions are satisfied
* no blocking regression has occurred

### 22.2 Execution Invalidation

If a specification changes while execution is active, the runtime MUST determine whether the change affects executing tasks or generated artifacts.

A change that invalidates Autonomous-Ready status MUST cause affected execution activities to pause, halt, or re-plan according to ISL v1.2 and ISL v1.5.

---

## 23.0 Readiness Evidence Package

This section defines the evidence package required to support readiness decisions. Enterprise governance requires more than a readiness label; it requires evidence that can be reviewed and audited.

A readiness evidence package collects the records supporting the current readiness state.

### 23.1 Evidence Package Contents

A readiness evidence package SHOULD include:

| Evidence Type                    | Required For                       |
| -------------------------------- | ---------------------------------- |
| Authored specification version   | All readiness levels               |
| Structural validation report     | Reviewable and above               |
| Canonical validation report      | Machine-Valid and above            |
| Reference resolution report      | Machine-Valid and above            |
| Validation coverage report       | Machine-Valid and above            |
| Policy coverage report           | Machine-Valid and above            |
| Traceability coverage report     | Machine-Valid and above            |
| Review findings and dispositions | Reviewable and above               |
| Governance approval records      | Machine-Valid and Autonomous-Ready |
| Risk tier record                 | Autonomous-Ready                   |
| Construction authorization       | Autonomous-Ready                   |
| Override or waiver records       | When applicable                    |
| Readiness transition records     | All transitions                    |

### 23.2 Evidence Package Rules

The evidence package MUST identify the specification version.

The evidence package MUST be immutable after the readiness decision is recorded, except that later corrections MUST be appended rather than overwritten.

The evidence package MUST be accessible to governance and audit roles.

---

## 24.0 Readiness Error Classes

This section defines readiness-specific error classes. These errors allow tools and governance workflows to distinguish readiness failures from authored-language errors, canonical errors, execution errors, or governance policy violations.

### 24.1 Readiness Error Record Schema

| Field                 | Type     | Required    | Description                       |
| --------------------- | -------- | ----------- | --------------------------------- |
| error-id              | string   | REQUIRED    | Unique readiness error identifier |
| error-class           | enum     | REQUIRED    | Readiness error class             |
| severity              | enum     | REQUIRED    | blocking, high, medium, low       |
| readiness-level       | integer  | REQUIRED    | Level being evaluated             |
| criterion-id          | string   | CONDITIONAL | Failed criterion                  |
| specification-id      | string   | REQUIRED    | System identifier                 |
| specification-version | semver   | REQUIRED    | Specification version             |
| message               | string   | REQUIRED    | Human-readable explanation        |
| required-action       | string   | REQUIRED    | Remediation action                |
| detected-at           | ISO 8601 | REQUIRED    | Time detected                     |

### 24.2 Error Classes

| Error Class                              | Description                              | Blocks Progression             |
| ---------------------------------------- | ---------------------------------------- | ------------------------------ |
| readiness-missing-required-section       | Required authored section is absent      | YES                            |
| readiness-missing-required-field         | Required field is absent                 | YES                            |
| readiness-invalid-version                | Specification version is invalid         | YES                            |
| readiness-unsupported-isl-version        | ISL version is unsupported               | YES                            |
| readiness-review-not-approved            | Required review approval missing         | YES                            |
| readiness-canonical-validation-failed    | Canonical validation has blocking errors | YES                            |
| readiness-reference-resolution-failed    | References do not resolve                | YES                            |
| readiness-validation-coverage-incomplete | Required validation coverage missing     | YES                            |
| readiness-policy-coverage-incomplete     | Required policy coverage missing         | YES                            |
| readiness-risk-tier-missing              | Risk tier not assigned                   | YES for Autonomous-Ready       |
| readiness-authorization-missing          | Construction authorization missing       | YES                            |
| readiness-separation-of-duties-violation | Approval role conflict detected          | YES                            |
| readiness-regression-required            | Change invalidates current readiness     | YES                            |
| readiness-override-expired               | Override supporting readiness is expired | YES                            |
| readiness-evidence-incomplete            | Evidence package incomplete              | YES when evidence is mandatory |

---

## 25.0 Readiness Record Storage and Auditability

This section defines storage and audit expectations for readiness records. Readiness is a governance-relevant lifecycle state and must be preserved for later inspection.

Readiness records support compliance, troubleshooting, historical reconstruction, and accountability.

### 25.1 Storage Requirements

The platform MUST store:

* readiness reports
* readiness transition records
* evidence packages
* approval records
* regression records
* override records
* readiness error records

### 25.2 Audit Requirements

The platform MUST support audit queries for:

* current readiness state
* readiness history by specification version
* failed readiness evaluations
* readiness overrides
* readiness regressions
* approval history
* evidence package contents

### 25.3 Immutability

Readiness transition records MUST be immutable after creation.

Corrections MUST be recorded as new records referencing the original.

---

## 26.0 Conformance Requirements

This section defines what it means for readiness evaluators, specifications, and platforms to conform to ISL v1.3.

### 26.1 Specification Conformance

A specification conforms to ISL v1.3 if it:

* records readiness level in the Project entity
* satisfies all criteria required for its claimed readiness level
* includes required evidence for readiness transitions
* regresses when criteria are invalidated
* preserves readiness history by version

### 26.2 Readiness Evaluator Conformance

A readiness evaluator conforms to ISL v1.3 if it can:

* evaluate all readiness levels
* enforce permitted transition paths
* check automated readiness criteria
* distinguish blocking failures from warnings
* produce Readiness Reports
* produce or request transition records
* identify regression triggers
* detect override requirements
* validate readiness evidence packages

### 26.3 Platform Conformance

A platform conforms to ISL v1.3 if it can:

* enforce readiness restrictions
* prevent construction before Autonomous-Ready
* integrate readiness evaluation with governance approval
* store readiness records immutably
* enforce regression rules
* prevent use of expired overrides
* expose readiness audit queries
* integrate readiness state with execution preconditions

---

## 27.0 Summary

ISL v1.3 defines the readiness model that controls the maturity and authorization state of ISL specifications. It establishes four levels: Draft, Reviewable, Machine-Valid, and Autonomous-Ready. Each level imposes explicit criteria, permitted platform behavior, required evidence, and governance expectations.

This revision strengthens readiness by making progression measurable, regression mandatory, evidence auditable, overrides controlled, and execution gating explicit.

A conforming IMHOTEP platform MUST treat readiness as a hard control mechanism. A specification MUST NOT enter autonomous construction until it has reached Autonomous-Ready status for the exact specification version being executed.
