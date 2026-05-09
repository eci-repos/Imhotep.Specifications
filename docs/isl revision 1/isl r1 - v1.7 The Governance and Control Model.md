# IMHOTEP Specification Language (ISL) v1.7

# The Governance and Control Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6
**Supersedes:** ISL r1 v1.7 Governance and Control Model
**Document Type:** Governance, Policy, and Control Specification

---

## 1.0 Scope

This document defines the Governance and Control Model for the IMHOTEP Specification Language and conforming platform implementations. It establishes the governance roles, approval gates, policy evaluation rules, risk tiers, waiver handling, override handling, escalation behavior, audit requirements, runtime enforcement points, and conformance obligations required to control autonomous software construction.

This document applies to the full ISL lifecycle, including specification authoring, readiness progression, canonical validation, construction planning, execution, deterministic validation, repair cycles, artifact consolidation, deployment preparation, post-change reauthorization, and audit. Governance is not limited to final approval. Governance is a continuous control layer that constrains what the platform may do at every significant lifecycle point.

This document defines:

* governance principles
* governance roles
* separation-of-duties requirements
* policy evaluation behavior
* approval gate types
* runtime gate enforcement
* risk tier classification
* waiver and override controls
* escalation handling
* deployment authorization
* audit logging
* governance state
* governance error classes
* conformance requirements

---

## 2.0 Purpose of the Governance and Control Model

The purpose of the Governance and Control Model is to ensure that autonomous software construction remains accountable, authorized, policy-compliant, and auditable. The IMHOTEP platform may perform activities traditionally carried out by architects, developers, testers, reviewers, and operators. Because these activities may materially affect enterprise systems, they must occur under explicit governance constraints.

Autonomous construction without governance would create unacceptable operational, compliance, security, and accountability risk. A system that can interpret specifications, generate artifacts, repair failures, and prepare deployment outputs must also know when it is allowed to act, when it must stop, who may authorize continuation, and how its decisions are recorded.

The Governance and Control Model exists to ensure that:

* readiness transitions are explicitly approved where required
* autonomous construction cannot begin without authorization
* policy violations produce defined runtime responses
* high-risk systems receive stronger review and approval controls
* waivers and overrides are controlled and time-bound
* runtime execution can pause, resume, halt, or escalate under governance control
* governance decisions are traceable to affected specifications, tasks, artifacts, validations, and policies
* audit records are immutable and queryable

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the Governance and Control Model. Governance depends on canonical entities, readiness levels, execution phases, traceability, construction tasks, tool outcomes, and platform state.

| Reference      | Purpose                                                                               |
| -------------- | ------------------------------------------------------------------------------------- |
| ISL v0.0       | Corpus-level principles, conformance, and standards alignment                         |
| ISL v1.1       | Policy entities, Project readiness fields, canonical relationships                    |
| ISL v1.2       | Execution phases, repair cycles, escalation, completion, and runtime state            |
| ISL v1.3       | Readiness levels, transition records, regression, and authorization                   |
| ISL v1.4       | Traceability graph, audit queries, governance event traceability                      |
| ISL v1.5       | Construction task graph, governance constraints in planning                           |
| ISL v1.6       | Tool trust profiles, tool governance, findings, and deterministic validation outcomes |
| RFC 2119       | Normative terminology                                                                 |
| NIST SP 800-53 | Security and privacy control reference alignment                                      |
| OpenTelemetry  | Governance telemetry and audit event alignment where applicable                       |

---

## 4.0 Terms and Definitions

This section defines governance-specific terminology used throughout this document. These terms provide a common vocabulary for governance engines, readiness evaluators, execution runtimes, authoring tools, and audit systems.

**Governance**
The set of roles, policies, approvals, controls, records, and enforcement mechanisms that regulate autonomous construction activity.

**Governance Engine**
The platform component responsible for evaluating governance rules, enforcing approval gates, recording governance events, and controlling runtime authorization.

**Approval Gate**
A lifecycle checkpoint that requires approval by an authorized governance role before progression may continue.

**Policy Evaluation**
The process of checking a Policy entity or governance rule against a specification, task, artifact, validation result, deployment output, or runtime action.

**Violation Response**
The required action taken when a policy evaluation identifies non-compliance.

**Waiver**
A time-bound governance exception that permits a known non-compliance condition to proceed under defined constraints.

**Override**
A privileged governance action that permits progression despite a failed automated check or blocked condition.

**Escalation**
A runtime or governance event that suspends automated progression and requests action by a designated authority.

**Risk Tier**
A governance classification that determines required controls, approvals, validation depth, and evidence retention.

**Separation of Duties**
A governance rule preventing the same individual or identity from fulfilling incompatible approval roles for the same specification or construction lifecycle.

---

## 5.0 Governance Design Principles

This section defines the principles that govern all governance behavior in ISL. These principles ensure that governance is enforceable, auditable, and integrated into runtime behavior.

### 5.1 Governance Is Continuous

Governance MUST operate throughout the lifecycle, not only at readiness transitions or deployment approval. Governance checks may occur during authoring, validation, planning, execution, repair, consolidation, deployment preparation, and post-change reauthorization.

A platform MUST NOT treat governance as a single final review event.

### 5.2 Governance Is Enforceable

Governance rules MUST produce runtime behavior. A policy violation, missing approval, expired waiver, or separation-of-duties violation MUST cause a defined response such as block, escalate, warn, audit-only, pause, halt, or require approval.

Governance requirements that do not produce enforceable platform behavior MUST be treated as incomplete.

### 5.3 Governance Decisions Are Traceable

Every approval, rejection, waiver, override, escalation, policy evaluation, and deployment authorization MUST be traceable to the affected specification version, task, artifact, validation result, policy, or execution event.

Governance decisions MUST NOT exist as isolated records without affected targets.

### 5.4 Governance Records Are Immutable

Governance records that affect readiness, execution, waiver, override, or deployment authorization MUST be immutable after creation. Corrections MUST be appended as new records referencing the original.

No platform component MAY silently modify or delete a governance audit entry.

### 5.5 Risk Determines Control Strength

Higher-risk systems MUST require stronger governance controls. Risk tier MUST influence approval requirements, evidence retention, validation depth, tool restrictions, repair permissions, waiver authority, and deployment authorization.

### 5.6 Human Authority Remains Explicit

Autonomous construction MAY perform many engineering tasks, but authorization authority MUST remain explicit. Where human approval is required, the platform MUST pause until an authorized governance role acts.

---

## 6.0 Governance Roles

This section defines the standard governance roles used by ISL. Roles identify who may review, approve, authorize, override, administer, or audit governance-controlled activity.

Roles may be mapped to human users, groups, service identities, enterprise identity roles, or governance systems. However, the semantic authority of each role MUST remain consistent.

### 6.1 Standard Governance Roles

| Role                     | Responsibility                                                                                | Approval Authority                                         |
| ------------------------ | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Reviewer                 | Reviews specification completeness and coherence                                              | Reviewable → Machine-Valid                                 |
| Security Reviewer        | Reviews security policy coverage, data protection, and security risk                          | Machine-Valid security sign-off                            |
| Architecture Reviewer    | Reviews architecture, service boundaries, interfaces, data model, and operational feasibility | Machine-Valid architecture sign-off                        |
| Authorizing Official     | Authorizes autonomous construction and deployment progression                                 | Machine-Valid → Autonomous-Ready, deployment authorization |
| Governance Administrator | Maintains governance configuration, role mapping, policy profiles, risk tier rules            | Governance configuration                                   |
| Override Authority       | Approves manual overrides of failed checks or blocked criteria                                | Override approval                                          |
| Auditor                  | Reviews governance records, traceability, evidence, and compliance history                    | Audit access, no construction authority                    |
| Operations Approver      | Approves operational readiness or environment-specific deployment where required              | Deployment or operational approval                         |
| Data Steward             | Reviews data classification, retention, privacy, and data handling rules                      | Data governance approval where required                    |

### 6.2 Role Assignment Record Schema

Every governance role assignment MUST be recorded.

| Field            | Type     | Required    | Description                                          |
| ---------------- | -------- | ----------- | ---------------------------------------------------- |
| assignment-id    | string   | REQUIRED    | Unique role assignment identifier                    |
| role             | enum     | REQUIRED    | Governance role assigned                             |
| subject-id       | string   | REQUIRED    | User, group, service, or authority assigned          |
| scope            | enum     | REQUIRED    | global, project, specification, environment, policy  |
| specification-id | string   | CONDITIONAL | Required when assignment is specification-scoped     |
| effective-from   | ISO 8601 | REQUIRED    | Assignment start time                                |
| effective-until  | ISO 8601 | CONDITIONAL | Assignment expiration time                           |
| assigned-by      | string   | REQUIRED    | Governance Administrator or authority assigning role |
| recorded-at      | ISO 8601 | REQUIRED    | Time assignment was recorded                         |
| status           | enum     | REQUIRED    | active, expired, revoked                             |

### 6.3 Role Assignment Rules

A role MUST NOT be exercised unless an active role assignment exists.

A role assignment MUST specify scope.

Expired or revoked role assignments MUST NOT authorize governance actions.

Role assignments that affect readiness, authorization, override, or deployment MUST be auditable.

---

## 7.0 Separation of Duties

This section defines separation-of-duties requirements. Separation of duties reduces the risk that one individual can define, review, approve, override, and authorize the same autonomous construction lifecycle without independent control.

Separation-of-duties rules are mandatory for enterprise-grade governance.

### 7.1 Mandatory Separation Rules

For the same specification version:

* The Authorizing Official MUST be distinct from the Reviewer.
* The Authorizing Official MUST be distinct from the Security Reviewer.
* The Authorizing Official SHOULD be distinct from the Architecture Reviewer.
* The Override Authority MUST be distinct from the requester of the override.
* The Governance Administrator SHOULD NOT approve their own governance configuration change.
* The Auditor MUST NOT approve readiness transitions or deployment authorization.

### 7.2 Separation Validation

Before a readiness transition to Autonomous-Ready, the Governance Engine MUST validate separation-of-duties rules.

Before an override is applied, the Governance Engine MUST validate that the Override Authority is eligible.

Before deployment authorization, the Governance Engine MUST validate required role separation according to the risk tier.

### 7.3 Separation Violation Record Schema

| Field                  | Type     | Required | Description                 |
| ---------------------- | -------- | -------- | --------------------------- |
| violation-id           | string   | REQUIRED | Unique violation identifier |
| specification-id       | string   | REQUIRED | System identifier           |
| specification-version  | semver   | REQUIRED | Specification version       |
| conflicting-subject-id | string   | REQUIRED | Identity causing conflict   |
| conflicting-roles      | array    | REQUIRED | Roles in conflict           |
| governance-action      | string   | REQUIRED | Action blocked or affected  |
| severity               | enum     | REQUIRED | blocking, high, medium, low |
| detected-at            | ISO 8601 | REQUIRED | Time violation was detected |
| required-action        | string   | REQUIRED | Required remediation        |

### 7.4 Separation Violation Handling

A blocking separation-of-duties violation MUST prevent the associated governance action.

A separation violation MUST NOT be bypassed except by an approved governance policy that explicitly permits the exception for the applicable risk tier.

---

## 8.0 Policy Governance

This section defines how ISL Policy entities and governance policies are evaluated and enforced. Policy governance is the mechanism through which security, compliance, operational, technology, risk, and data-handling rules constrain platform behavior.

Policies must be verifiable and actionable. A policy that cannot be evaluated or enforced cannot serve as a governance control.

### 8.1 Policy Sources

Governance policies MAY originate from:

* ISL Policy entities
* organizational policy libraries
* security control frameworks
* architecture standards
* platform configuration
* environment-specific deployment policies
* regulatory control mappings
* risk-tier profiles

### 8.2 Policy Evaluation Target Types

Policies MAY apply to:

* specification entities
* canonical relationships
* construction tasks
* generated artifacts
* interfaces
* data entities
* infrastructure definitions
* tool invocations
* model invocations
* repair cycles
* deployment artifacts
* readiness transitions
* runtime actions

### 8.3 Policy Violation Responses

Policy violation responses MUST use one of the following values.

| Response   | Meaning                                                         |
| ---------- | --------------------------------------------------------------- |
| block      | Prevent progression until violation is resolved or waived       |
| escalate   | Suspend automated progression and request governance action     |
| warn       | Record finding and allow progression unless risk tier escalates |
| audit-only | Record finding without affecting progression                    |

### 8.4 Policy Evaluation Record Schema

Every policy evaluation MUST produce a record.

| Field              | Type     | Required    | Description                                                                                                                     |
| ------------------ | -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------- |
| evaluation-id      | string   | REQUIRED    | Unique policy evaluation identifier                                                                                             |
| policy-id          | string   | REQUIRED    | Policy evaluated                                                                                                                |
| policy-source      | enum     | REQUIRED    | isl-policy, organization-policy, platform-policy, external-control                                                              |
| target-id          | string   | REQUIRED    | Entity, task, artifact, invocation, or event evaluated                                                                          |
| target-type        | enum     | REQUIRED    | specification-entity, task, artifact, validation-result, tool-invocation, model-invocation, deployment-artifact, runtime-action |
| outcome            | enum     | REQUIRED    | compliant, non-compliant, not-applicable, waived                                                                                |
| violation-response | enum     | REQUIRED    | block, escalate, warn, audit-only                                                                                               |
| findings           | array    | CONDITIONAL | Required when outcome is non-compliant                                                                                          |
| evaluated-by       | string   | REQUIRED    | Tool, engine, role, or component performing evaluation                                                                          |
| evaluated-at       | ISO 8601 | REQUIRED    | Evaluation timestamp                                                                                                            |
| evidence           | array    | CONDITIONAL | Evidence supporting evaluation                                                                                                  |
| waiver-id          | string   | CONDITIONAL | Required when outcome is waived                                                                                                 |

### 8.5 Policy Evaluation Rules

A policy with violation-response `block` MUST prevent progression when non-compliant unless a valid waiver exists.

A policy with violation-response `escalate` MUST pause affected runtime activity and produce an escalation event.

A policy with violation-response `warn` MUST produce an audit record and MAY permit progression unless the applicable risk tier promotes warnings to escalation.

A policy with violation-response `audit-only` MUST produce an audit record.

A policy evaluation MUST reference the target evaluated.

---

## 9.0 Approval Gates

This section defines approval gates used by ISL. Approval gates are lifecycle checkpoints that require authorized governance approval before progression.

Approval gates make governance executable. The runtime, readiness evaluator, or planning engine MUST pause or block when an approval gate is required and not satisfied.

### 9.1 Standard Approval Gate Types

| Gate Type                      | Required Approver                                          |
| ------------------------------ | ---------------------------------------------------------- |
| reviewable-transition          | Reviewer                                                   |
| machine-valid-security         | Security Reviewer                                          |
| machine-valid-architecture     | Architecture Reviewer                                      |
| autonomous-ready               | Authorizing Official                                       |
| deployment-authorization       | Authorizing Official                                       |
| override-approval              | Override Authority                                         |
| waiver-approval                | Waiver Authority or Authorizing Official                   |
| post-change-reauthorization    | Authorizing Official                                       |
| tool-use-approval              | Governance Administrator or designated Tool Authority      |
| model-use-approval             | Governance Administrator or designated Model Authority     |
| high-risk-repair-approval      | Authorizing Official or Security Reviewer                  |
| production-deployment-approval | Authorizing Official and Operations Approver when required |

### 9.2 Approval Gate Record Schema

| Field                 | Type     | Required    | Description                                                     |
| --------------------- | -------- | ----------- | --------------------------------------------------------------- |
| gate-id               | string   | REQUIRED    | Unique approval gate identifier                                 |
| gate-type             | enum     | REQUIRED    | Approval gate type                                              |
| specification-id      | string   | REQUIRED    | System identifier                                               |
| specification-version | semver   | REQUIRED    | Specification version                                           |
| target-id             | string   | CONDITIONAL | Task, artifact, policy, deployment, or runtime action affected  |
| required-role         | string   | REQUIRED    | Role required to approve                                        |
| requested-by          | string   | REQUIRED    | User, component, or governance process requesting approval      |
| requested-at          | ISO 8601 | REQUIRED    | Time gate was requested                                         |
| status                | enum     | REQUIRED    | pending, approved, rejected, expired, cancelled                 |
| decision-by           | string   | CONDITIONAL | Required when approved or rejected                              |
| decision-at           | ISO 8601 | CONDITIONAL | Required when approved or rejected                              |
| decision-rationale    | string   | CONDITIONAL | Required for rejection, override, waiver, or high-risk approval |
| expiry                | ISO 8601 | CONDITIONAL | Expiration time for pending approval                            |

### 9.3 Approval Gate Rules

A required approval gate MUST block progression until approved.

A rejected approval gate MUST prevent the associated transition or action.

An expired approval gate MUST NOT authorize progression.

Approval MUST reference the exact specification version and target action.

Approval for one specification version MUST NOT authorize another version unless governance explicitly permits reuse.

---

## 10.0 Runtime Approval Gate Enforcement

This section defines how approval gates integrate with runtime behavior. This section is critical because governance must not merely record approvals; it must actively control execution.

When the runtime reaches a governance-controlled point, it MUST create or evaluate the required approval gate and transition the affected task, phase, or execution graph into a waiting, blocked, escalated, or halted state.

### 10.1 Runtime Gate States

| State        | Meaning                                       |
| ------------ | --------------------------------------------- |
| not-required | No approval gate applies                      |
| pending      | Approval requested and awaiting decision      |
| approved     | Approval granted and action may proceed       |
| rejected     | Approval denied and action MUST NOT proceed   |
| expired      | Approval request expired before decision      |
| cancelled    | Approval request no longer applies            |
| superseded   | Approval replaced by a later approval request |

### 10.2 Runtime Gate Enforcement Points

The runtime MUST enforce approval gates at the following points when applicable.

| Enforcement Point                                 | Gate Type                                                                 |
| ------------------------------------------------- | ------------------------------------------------------------------------- |
| Readiness transition to Machine-Valid             | reviewable-transition, machine-valid-security, machine-valid-architecture |
| Readiness transition to Autonomous-Ready          | autonomous-ready                                                          |
| Execution start                                   | autonomous-ready, risk-tier controls                                      |
| High-risk task execution                          | approval gate required by policy                                          |
| High-risk repair continuation                     | high-risk-repair-approval                                                 |
| Override application                              | override-approval                                                         |
| Waiver application                                | waiver-approval                                                           |
| Tool invocation with restricted trust profile     | tool-use-approval                                                         |
| Model invocation with restricted provider profile | model-use-approval                                                        |
| Deployment preparation completion                 | deployment-authorization                                                  |
| Post-change continuation                          | post-change-reauthorization                                               |

### 10.3 Runtime Pause Behavior

When an approval gate is pending, the runtime MUST:

* pause the affected task, phase, or execution action
* record the pause in the Execution Graph
* create or update the Approval Gate Record
* emit an audit event
* notify or expose the gate to the required approver role
* prevent dependent tasks from proceeding when dependency rules require the blocked action

### 10.4 Runtime Resumption Behavior

When an approval gate is approved, the runtime MUST:

* verify the approver has the required active role
* verify separation-of-duties rules
* verify the approval applies to the current specification version and target
* record the approval decision
* update the Execution Graph
* resume only the affected task, phase, or execution action permitted by the approval

### 10.5 Runtime Rejection Behavior

When an approval gate is rejected, the runtime MUST:

* mark the affected task, phase, or action as blocked, failed, halted, or escalated according to policy
* record the rejection rationale
* prevent dependent tasks from proceeding when required
* emit an audit event
* require replanning, repair, waiver, override, or termination before continuation

### 10.6 Approval Timeout Behavior

If a pending approval gate reaches expiry, the runtime MUST:

* mark the gate as expired
* suspend or halt the affected action according to policy
* record an audit event
* notify or expose the expired state to governance roles
* require a new approval request before continuation

---

## 11.0 Risk Tier Model

This section defines risk tiers used to calibrate governance controls. Risk tier determines required reviewers, validation depth, evidence retention, waiver strictness, runtime approval points, and deployment authorization requirements.

Risk tier MUST be assigned before Autonomous-Ready readiness.

### 11.1 Standard Risk Tiers

| Tier     | Criteria                                                                                                             |
| -------- | -------------------------------------------------------------------------------------------------------------------- |
| Standard | Internal use, no regulated data, no safety-critical behavior, limited operational impact                             |
| Elevated | Handles personal data, integrates with regulated systems, or affects important business workflows                    |
| High     | Operates in regulated domains such as finance, healthcare, public-sector systems, or critical infrastructure support |
| Critical | Safety-critical, legally mandated, public-impacting, high-availability, or mission-critical systems                  |

### 11.2 Risk Tier Assignment Record Schema

| Field                 | Type     | Required    | Description                           |
| --------------------- | -------- | ----------- | ------------------------------------- |
| risk-assignment-id    | string   | REQUIRED    | Unique risk assignment identifier     |
| specification-id      | string   | REQUIRED    | System identifier                     |
| specification-version | semver   | REQUIRED    | Specification version                 |
| risk-tier             | enum     | REQUIRED    | standard, elevated, high, critical    |
| assignment-rationale  | string   | REQUIRED    | Explanation for tier assignment       |
| assigned-by           | string   | REQUIRED    | Role or authority assigning risk tier |
| assigned-at           | ISO 8601 | REQUIRED    | Assignment timestamp                  |
| evidence              | array    | CONDITIONAL | Supporting evidence                   |
| review-required-by    | ISO 8601 | CONDITIONAL | Required reassessment date            |

### 11.3 Risk Tier Control Requirements

| Risk Tier | Required Governance Controls                                                                                                                  |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Standard  | Default readiness, validation, traceability, and approval requirements                                                                        |
| Elevated  | Security Reviewer approval, policy coverage evidence, enhanced evidence retention                                                             |
| High      | Security Reviewer and Architecture Reviewer approval, stricter tool/model restrictions, waiver review, deployment authorization               |
| Critical  | Independent authorization, strictest evidence retention, restricted tool/model profiles, mandatory deployment approval, enhanced audit review |

### 11.4 Risk Tier Change Rules

A risk tier increase MUST trigger readiness reevaluation.

A risk tier reduction MUST require governance approval.

A risk tier change MUST be recorded as a governance event.

If risk tier changes during execution, the runtime MUST evaluate whether active tasks must pause, halt, or re-enter approval gates.

---

## 12.0 Waivers

This section defines waivers. A waiver permits a known policy violation, validation finding, or governance exception to proceed under explicit constraints. Waivers are not permanent permission to ignore controls.

Waivers are appropriate when risk is understood, compensating controls exist, and governance accepts the exception for a limited time or scope.

### 12.1 Waiver Conditions

A waiver MAY be requested when:

* a policy violation is known and documented
* a validation finding cannot be immediately resolved
* compensating controls are defined
* risk is accepted by an authorized role
* the waiver has a defined expiry
* affected targets are explicitly identified

### 12.2 Waiver Prohibitions

A waiver MUST NOT be used to bypass:

* missing system identity
* missing construction authorization
* separation-of-duties violation unless a specific governance policy permits it
* unbounded repair cycles
* absent traceability for stable artifacts
* unknown artifact provenance
* prohibited tool or model use unless an Override Authority approves an explicit override
* critical safety or legal constraints where waiver is forbidden by policy

### 12.3 Waiver Record Schema

| Field                 | Type     | Required    | Description                                                              |
| --------------------- | -------- | ----------- | ------------------------------------------------------------------------ |
| waiver-id             | string   | REQUIRED    | Unique waiver identifier                                                 |
| waiver-type           | enum     | REQUIRED    | policy, validation, security, operational, deployment, traceability      |
| specification-id      | string   | REQUIRED    | System identifier                                                        |
| specification-version | semver   | REQUIRED    | Specification version                                                    |
| target-id             | string   | REQUIRED    | Policy, validation, task, artifact, finding, or deployment target waived |
| justification         | string   | REQUIRED    | Reason waiver is requested                                               |
| compensating-controls | string   | REQUIRED    | Controls mitigating the waived condition                                 |
| risk-tier             | enum     | REQUIRED    | Risk tier at time of waiver                                              |
| requested-by          | string   | REQUIRED    | Requesting user, role, or component                                      |
| approved-by           | string   | REQUIRED    | Authorized approver                                                      |
| approved-at           | ISO 8601 | REQUIRED    | Approval timestamp                                                       |
| expiry                | ISO 8601 | REQUIRED    | Waiver expiration                                                        |
| status                | enum     | REQUIRED    | active, expired, revoked, closed                                         |
| review-date           | ISO 8601 | CONDITIONAL | Required reassessment date                                               |
| evidence              | array    | CONDITIONAL | Supporting evidence                                                      |

### 12.4 Waiver Rules

A waiver MUST have an expiry date.

A waiver MUST identify the target it applies to.

A waiver MUST include compensating controls.

An expired waiver MUST NOT authorize progression.

A waiver MUST be traceable to the affected policy, validation result, artifact, task, or deployment action.

A waiver affecting High or Critical risk systems MUST require Authorizing Official approval unless governance defines a stricter role.

---

## 13.0 Overrides

This section defines overrides. Overrides permit progression despite failed automated checks, blocked states, or platform-level controls. Overrides are stronger and riskier than waivers and require stricter governance.

An override should be rare. It exists to handle exceptional circumstances where the platform’s normal control path cannot resolve a blocked condition but governance accepts responsibility for continuation.

### 13.1 Override Conditions

An override MAY be requested when:

* an automated check is known to be incorrect
* a governance authority accepts documented risk
* an external business or operational condition requires continuation
* compensating controls are defined
* the override is time-bound and scoped
* the Override Authority approves it

### 13.2 Override Prohibitions

An override MUST NOT be used to:

* authorize construction without a specification
* authorize construction without a canonical model
* authorize construction without traceability initialization
* bypass missing Autonomous-Ready authorization
* bypass audit logging
* delete or alter historical governance records
* suppress known critical security findings without explicit risk acceptance
* bypass legal or safety constraints where override is prohibited

### 13.3 Override Record Schema

| Field                 | Type     | Required    | Description                                                     |
| --------------------- | -------- | ----------- | --------------------------------------------------------------- |
| override-id           | string   | REQUIRED    | Unique override identifier                                      |
| override-type         | enum     | REQUIRED    | readiness, validation, policy, runtime, tool, model, deployment |
| specification-id      | string   | REQUIRED    | System identifier                                               |
| specification-version | semver   | REQUIRED    | Specification version                                           |
| target-id             | string   | REQUIRED    | Target being overridden                                         |
| failed-control        | string   | REQUIRED    | Control or check being overridden                               |
| justification         | string   | REQUIRED    | Reason override is required                                     |
| compensating-controls | string   | REQUIRED    | Controls mitigating override risk                               |
| requested-by          | string   | REQUIRED    | Requester                                                       |
| approved-by           | string   | REQUIRED    | Override Authority                                              |
| approved-at           | ISO 8601 | REQUIRED    | Approval timestamp                                              |
| expiry                | ISO 8601 | REQUIRED    | Expiration timestamp                                            |
| status                | enum     | REQUIRED    | active, expired, revoked, closed                                |
| evidence              | array    | CONDITIONAL | Supporting evidence                                             |

### 13.4 Override Rules

An override MUST have an expiry.

An override MUST be approved by an active Override Authority.

An override MUST be recorded before the affected action resumes.

An override MUST be traceable to the affected control and target.

The runtime MUST verify that an override is active before using it.

An override MUST NOT be reused across specification versions unless explicitly authorized.

---

## 14.0 Escalation Model

This section defines escalation behavior. Escalation occurs when the platform cannot safely proceed autonomously and requires governance or human authority.

Escalation is not failure by itself. It is a controlled pause that routes a blocked condition to the appropriate role.

### 14.1 Escalation Triggers

The runtime or governance engine MUST escalate when:

* repair limits are reached
* policy response is escalate
* approval gate is required and not satisfied
* waiver is required
* override is required
* tool conflict cannot be resolved automatically
* restricted tool or model use requires approval
* traceability integrity failure blocks completion
* security finding requires human review
* deployment authorization is missing
* execution state integrity is inconsistent
* risk tier changes during active execution

### 14.2 Escalation Record Schema

| Field                 | Type     | Required    | Description                                                                                        |
| --------------------- | -------- | ----------- | -------------------------------------------------------------------------------------------------- |
| escalation-id         | string   | REQUIRED    | Unique escalation identifier                                                                       |
| escalation-type       | enum     | REQUIRED    | repair, policy, approval, waiver, override, tool, model, traceability, deployment, state-integrity |
| specification-id      | string   | REQUIRED    | System identifier                                                                                  |
| specification-version | semver   | REQUIRED    | Specification version                                                                              |
| target-id             | string   | REQUIRED    | Task, artifact, validation, policy, gate, or runtime action affected                               |
| triggering-event-id   | string   | CONDITIONAL | Event causing escalation                                                                           |
| required-role         | string   | REQUIRED    | Role required to resolve escalation                                                                |
| severity              | enum     | REQUIRED    | blocking, high, medium, low                                                                        |
| status                | enum     | REQUIRED    | open, resolved, rejected, expired, cancelled                                                       |
| opened-at             | ISO 8601 | REQUIRED    | Escalation creation time                                                                           |
| resolved-by           | string   | CONDITIONAL | Authority resolving escalation                                                                     |
| resolved-at           | ISO 8601 | CONDITIONAL | Resolution time                                                                                    |
| resolution            | string   | CONDITIONAL | Resolution description                                                                             |
| next-action           | enum     | CONDITIONAL | resume, repair, replan, waive, override, halt, fail                                                |

### 14.3 Escalation Runtime Behavior

When escalation is opened, the runtime MUST:

* mark the affected task, artifact, phase, or action as escalated
* pause dependent actions when required
* emit an audit event
* preserve current execution state
* expose or notify the required role
* prevent unauthorized continuation

### 14.4 Escalation Resolution

An escalation may resolve by:

* approval
* rejection
* waiver
* override
* repair authorization
* replanning
* execution halt
* task failure
* deployment rejection

The runtime MUST apply only the next-action authorized by the escalation resolution.

---

## 15.0 Governance State

This section defines governance state. Governance state records the current and historical governance posture of a specification, construction plan, execution, artifact, deployment preparation, or platform configuration.

Governance state is part of platform memory and must be preserved for audit and runtime control.

### 15.1 Governance State Categories

| Category                       | Description                                              |
| ------------------------------ | -------------------------------------------------------- |
| Role State                     | Active role assignments and role history                 |
| Policy State                   | Active policies, policy versions, and evaluation results |
| Approval State                 | Approval gates, decisions, and pending approvals         |
| Waiver State                   | Active, expired, revoked, and closed waivers             |
| Override State                 | Active, expired, revoked, and closed overrides           |
| Escalation State               | Open and resolved escalations                            |
| Risk State                     | Risk tier assignments and history                        |
| Audit State                    | Immutable governance audit events                        |
| Deployment Authorization State | Deployment approvals and deployment restrictions         |

### 15.2 Governance State Rules

Governance state MUST be versioned.

Governance state MUST be queryable by authorized governance and audit roles.

Governance state MUST be updated when governance events occur.

Governance state MUST be consulted by readiness evaluators, planning engines, execution runtimes, tool selectors, and deployment preparation components where applicable.

Governance state MUST NOT be silently modified or deleted.

---

## 16.0 Governance Runtime Control Contract

This section defines the contract by which runtime components interact with the Governance Engine. This contract closes the gap between governance as documentation and governance as runtime control.

The runtime MUST consult the Governance Engine at defined control points and MUST obey the returned decision.

### 16.1 Governance Check Request Schema

| Field                 | Type     | Required    | Description                                                                                                                      |
| --------------------- | -------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| check-id              | string   | REQUIRED    | Unique governance check identifier                                                                                               |
| check-type            | enum     | REQUIRED    | readiness, planning, execution, policy, tool, model, repair, deployment, waiver, override                                        |
| specification-id      | string   | REQUIRED    | System identifier                                                                                                                |
| specification-version | semver   | REQUIRED    | Specification version                                                                                                            |
| target-id             | string   | REQUIRED    | Task, artifact, policy, validation, invocation, or deployment target                                                             |
| target-type           | enum     | REQUIRED    | specification, task, artifact, validation-result, policy, tool-invocation, model-invocation, deployment-artifact, runtime-action |
| requested-action      | string   | REQUIRED    | Action the runtime wants to perform                                                                                              |
| risk-tier             | enum     | CONDITIONAL | Current risk tier                                                                                                                |
| context               | object   | CONDITIONAL | Additional structured context                                                                                                    |
| requested-at          | ISO 8601 | REQUIRED    | Time check was requested                                                                                                         |
| requested-by          | string   | REQUIRED    | Runtime component requesting check                                                                                               |

### 16.2 Governance Check Response Schema

| Field               | Type     | Required    | Description                                                                         |
| ------------------- | -------- | ----------- | ----------------------------------------------------------------------------------- |
| check-id            | string   | REQUIRED    | Matching governance check identifier                                                |
| decision            | enum     | REQUIRED    | allow, block, warn, escalate, approval-required, waiver-required, override-required |
| applicable-policies | array    | CONDITIONAL | Policies evaluated                                                                  |
| required-gate-id    | string   | CONDITIONAL | Approval gate required                                                              |
| required-role       | string   | CONDITIONAL | Role required for approval or escalation                                            |
| findings            | array    | CONDITIONAL | Policy or governance findings                                                       |
| rationale           | string   | REQUIRED    | Explanation of decision                                                             |
| expires-at          | ISO 8601 | CONDITIONAL | Expiration of decision, if applicable                                               |
| decided-at          | ISO 8601 | REQUIRED    | Time decision was produced                                                          |
| decided-by          | string   | REQUIRED    | Governance Engine or authority producing decision                                   |

### 16.3 Governance Decision Handling

The runtime MUST handle governance decisions as follows.

| Decision          | Runtime Behavior                                       |
| ----------------- | ------------------------------------------------------ |
| allow             | Proceed with requested action                          |
| block             | Do not proceed; mark affected action blocked or failed |
| warn              | Proceed and record warning                             |
| escalate          | Pause affected action and open escalation              |
| approval-required | Create or wait for approval gate                       |
| waiver-required   | Pause until valid waiver exists                        |
| override-required | Pause until valid override exists                      |

### 16.4 Runtime Contract Rules

A runtime MUST NOT proceed with an action when governance returns block, escalate, approval-required, waiver-required, or override-required.

Governance check responses MUST be recorded.

Governance decisions MUST be traceable to affected runtime actions.

Expired governance decisions MUST NOT authorize later actions.

---

## 17.0 Deployment Authorization

This section defines deployment authorization. Deployment preparation may be automated, but deployment authorization remains a governance-controlled action.

A deployment-ready package is not the same as an authorized deployment.

### 17.1 Deployment Authorization Conditions

Deployment authorization MUST verify:

* construction execution completed or required scope is complete
* required validation passed or was waived
* blocking policy violations are resolved or waived
* traceability integrity checks passed
* deployment artifacts are produced and validated
* risk-tier-specific approvals are satisfied
* operational readiness evidence exists
* environment-specific constraints are satisfied

### 17.2 Deployment Authorization Record Schema

| Field                       | Type     | Required    | Description                                    |
| --------------------------- | -------- | ----------- | ---------------------------------------------- |
| deployment-authorization-id | string   | REQUIRED    | Unique authorization identifier                |
| specification-id            | string   | REQUIRED    | System identifier                              |
| specification-version       | semver   | REQUIRED    | Specification version                          |
| deployment-target           | string   | REQUIRED    | Environment or target authorized               |
| deployment-artifacts        | array    | REQUIRED    | Deployment artifacts covered                   |
| risk-tier                   | enum     | REQUIRED    | Risk tier at authorization                     |
| validation-evidence         | array    | REQUIRED    | Validation records supporting authorization    |
| policy-evidence             | array    | REQUIRED    | Policy evaluations supporting authorization    |
| traceability-snapshot-id    | string   | REQUIRED    | Traceability snapshot supporting authorization |
| authorized-by               | string   | REQUIRED    | Authorizing Official or required role          |
| authorized-at               | ISO 8601 | REQUIRED    | Authorization timestamp                        |
| expiry                      | ISO 8601 | CONDITIONAL | Expiration time if authorization is time-bound |
| status                      | enum     | REQUIRED    | authorized, rejected, expired, revoked         |

### 17.3 Deployment Authorization Rules

Production deployment MUST NOT occur without deployment authorization.

Deployment authorization MUST apply to a specific specification version and deployment target.

Deployment authorization MUST NOT be reused after material specification, artifact, infrastructure, or policy change unless governance revalidates applicability.

A rejected deployment authorization MUST block deployment.

---

## 18.0 Post-Change Governance

This section defines governance behavior after specification or artifact changes. Changes may invalidate readiness, approvals, waivers, deployment authorization, or execution state.

Post-change governance ensures that prior approvals are not improperly applied to changed content.

### 18.1 Change Events Requiring Governance Review

Governance review MUST occur when:

* a must-have requirement is added, removed, or materially changed
* a security policy changes
* a data classification changes
* an interface contract changes
* infrastructure deployment model changes
* risk tier changes
* validation coverage changes
* deployment artifacts change after authorization
* traceability impact analysis identifies affected approved artifacts

### 18.2 Post-Change Reauthorization

Post-change reauthorization MUST verify:

* readiness regression outcome
* impact analysis result
* affected approvals
* affected waivers and overrides
* affected deployment authorizations
* affected construction plan tasks
* required renewed sign-offs

### 18.3 Post-Change Reauthorization Record Schema

| Field                       | Type     | Required    | Description                               |
| --------------------------- | -------- | ----------- | ----------------------------------------- |
| reauthorization-id          | string   | REQUIRED    | Unique reauthorization identifier         |
| specification-id            | string   | REQUIRED    | System identifier                         |
| prior-specification-version | semver   | REQUIRED    | Prior version                             |
| new-specification-version   | semver   | REQUIRED    | New version                               |
| impact-analysis-id          | string   | REQUIRED    | Impact analysis used                      |
| affected-approvals          | array    | CONDITIONAL | Prior approvals affected                  |
| affected-waivers            | array    | CONDITIONAL | Waivers affected                          |
| affected-deployments        | array    | CONDITIONAL | Deployment authorizations affected        |
| required-actions            | array    | REQUIRED    | Actions required before continuation      |
| authorized-by               | string   | CONDITIONAL | Required when reauthorization approved    |
| status                      | enum     | REQUIRED    | pending, approved, rejected, not-required |
| recorded-at                 | ISO 8601 | REQUIRED    | Time recorded                             |

---

## 19.0 Audit Logging

This section defines governance audit logging. Audit logs provide the immutable history needed to demonstrate accountability, compliance, and system integrity.

Audit logging is mandatory for governance-relevant events.

### 19.1 Required Audit Event Types

The platform MUST support the following audit event types:

* role-assignment
* role-revocation
* approval-requested
* approval-recorded
* approval-rejected
* policy-evaluated
* violation-detected
* waiver-requested
* waiver-granted
* waiver-expired
* waiver-revoked
* override-requested
* override-applied
* override-expired
* escalation-received
* escalation-resolved
* risk-tier-assigned
* risk-tier-changed
* deployment-authorized
* deployment-rejected
* governance-check-performed
* governance-configuration-changed

### 19.2 Audit Log Entry Schema

| Field                 | Type     | Required    | Description                                                                      |
| --------------------- | -------- | ----------- | -------------------------------------------------------------------------------- |
| audit-event-id        | string   | REQUIRED    | Unique audit event identifier                                                    |
| event-type            | enum     | REQUIRED    | Audit event type                                                                 |
| specification-id      | string   | CONDITIONAL | Required when event is specification-scoped                                      |
| specification-version | semver   | CONDITIONAL | Required when event is specification-version scoped                              |
| target-id             | string   | CONDITIONAL | Entity, task, artifact, policy, gate, waiver, override, or deployment target     |
| actor-id              | string   | REQUIRED    | User, role, component, or system causing event                                   |
| actor-role            | string   | CONDITIONAL | Governance role used                                                             |
| event-time            | ISO 8601 | REQUIRED    | Time event occurred                                                              |
| outcome               | enum     | CONDITIONAL | Event outcome                                                                    |
| rationale             | string   | CONDITIONAL | Required for approvals, rejections, waivers, overrides, and deployment decisions |
| evidence              | array    | CONDITIONAL | Evidence references                                                              |
| prior-state           | string   | CONDITIONAL | State before event                                                               |
| new-state             | string   | CONDITIONAL | State after event                                                                |
| correlation-id        | string   | CONDITIONAL | Related workflow, execution, or governance check identifier                      |

### 19.3 Audit Immutability Rules

Audit log entries MUST be immutable.

No platform component MAY modify or delete an audit log entry after it is written.

Corrections MUST be recorded as new audit events referencing the original event.

Audit records MUST be retained according to governance retention requirements.

---

## 20.0 Governance Evidence

This section defines evidence requirements. Governance decisions must be supported by evidence that can be inspected later.

Evidence connects approvals, waivers, overrides, and authorizations to validation reports, traceability snapshots, policy evaluations, tool results, and review findings.

### 20.1 Evidence Types

Governance evidence MAY include:

* readiness reports
* canonical validation reports
* construction plans
* planning validation reports
* traceability snapshots
* validation results
* test results
* security scan results
* policy evaluations
* risk tier assignments
* review findings
* approval records
* waiver records
* override records
* deployment preparation records
* audit log references

### 20.2 Evidence Rules

A governance decision that authorizes progression MUST include sufficient evidence.

Evidence MUST reference the applicable specification version.

Evidence MUST be preserved for audit.

Evidence used for High or Critical risk systems SHOULD be retained under stricter integrity controls.

A governance decision lacking required evidence MUST be treated as incomplete and MUST NOT authorize progression.

---

## 21.0 Governance Error Classes

This section defines governance-specific error classes. These errors allow the platform to report governance failures consistently across readiness, planning, execution, deployment, and audit workflows.

### 21.1 Governance Error Record Schema

| Field                 | Type     | Required    | Description                                               |
| --------------------- | -------- | ----------- | --------------------------------------------------------- |
| error-id              | string   | REQUIRED    | Unique error identifier                                   |
| error-class           | enum     | REQUIRED    | Governance error class                                    |
| severity              | enum     | REQUIRED    | blocking, high, medium, low                               |
| specification-id      | string   | CONDITIONAL | Related system identifier                                 |
| specification-version | semver   | CONDITIONAL | Related specification version                             |
| target-id             | string   | CONDITIONAL | Related target                                            |
| governance-record-id  | string   | CONDITIONAL | Related gate, waiver, override, approval, or audit record |
| message               | string   | REQUIRED    | Human-readable explanation                                |
| required-action       | string   | REQUIRED    | Remediation required                                      |
| detected-at           | ISO 8601 | REQUIRED    | Detection timestamp                                       |

### 21.2 Error Classes

| Error Class                                 | Description                                        | Default Severity |
| ------------------------------------------- | -------------------------------------------------- | ---------------- |
| governance-role-missing                     | Required governance role assignment does not exist | blocking         |
| governance-role-expired                     | Role assignment is expired                         | blocking         |
| governance-separation-violation             | Separation-of-duties rule violated                 | blocking         |
| governance-approval-missing                 | Required approval gate has no approval             | blocking         |
| governance-approval-expired                 | Approval expired before use                        | blocking         |
| governance-approval-rejected                | Required approval was rejected                     | blocking         |
| governance-policy-noncompliant              | Policy evaluation produced non-compliance          | blocking or high |
| governance-waiver-missing                   | Required waiver does not exist                     | blocking         |
| governance-waiver-expired                   | Waiver expired before use                          | blocking         |
| governance-override-missing                 | Required override does not exist                   | blocking         |
| governance-override-expired                 | Override expired before use                        | blocking         |
| governance-risk-tier-missing                | Required risk tier assignment missing              | blocking         |
| governance-audit-write-failed               | Audit event could not be written                   | blocking         |
| governance-evidence-incomplete              | Required evidence is missing                       | blocking         |
| governance-runtime-check-failed             | Governance check failed or could not complete      | blocking         |
| governance-deployment-authorization-missing | Deployment authorization missing                   | blocking         |

### 21.3 Error Handling Rules

Blocking governance errors MUST prevent the affected action.

A governance audit write failure MUST halt or escalate governance-controlled actions because the action cannot be made auditable.

Governance errors MUST be traceable to affected targets when applicable.

---

## 22.0 Governance Telemetry and Monitoring

This section defines minimum telemetry requirements for governance activity. Governance telemetry supports operational awareness, compliance monitoring, and detection of abnormal governance patterns.

Telemetry does not replace immutable audit logging. Audit records are authoritative. Telemetry supports monitoring and alerting.

### 22.1 Required Governance Telemetry Events

The platform SHOULD emit telemetry for:

* governance check requested
* governance check decision
* approval gate opened
* approval gate resolved
* escalation opened
* escalation resolved
* waiver granted
* waiver expired
* override applied
* policy violation detected
* deployment authorization granted
* separation-of-duties violation detected
* audit write failure

### 22.2 Governance Alert Conditions

The platform SHOULD alert governance or operations when:

| Condition                 | Example Trigger                                       |
| ------------------------- | ----------------------------------------------------- |
| Repeated waiver use       | Same policy waived repeatedly                         |
| Override frequency spike  | Overrides exceed configured threshold                 |
| Approval timeout          | Required gate remains pending beyond allowed duration |
| Critical policy violation | Critical or blocking policy violation detected        |
| Audit write failure       | Audit event cannot be recorded                        |
| Separation violation      | Incompatible roles detected                           |
| Deployment blocked        | Deployment authorization rejected or missing          |
| Risk tier mismatch        | Runtime action exceeds approved risk tier controls    |

---

## 23.0 Governance Configuration

This section defines governance configuration. Governance configuration determines roles, policies, approval rules, risk tiers, waiver rules, override rules, and runtime enforcement behavior.

Governance configuration itself is sensitive and must be controlled.

### 23.1 Governance Configuration Schema

| Field                 | Type     | Required    | Description                                  |
| --------------------- | -------- | ----------- | -------------------------------------------- |
| configuration-id      | string   | REQUIRED    | Unique governance configuration identifier   |
| configuration-version | semver   | REQUIRED    | Configuration version                        |
| effective-from        | ISO 8601 | REQUIRED    | Effective start time                         |
| effective-until       | ISO 8601 | CONDITIONAL | Expiration time                              |
| role-mappings         | array    | REQUIRED    | Governance role assignments or mapping rules |
| approval-rules        | array    | REQUIRED    | Approval gate rules                          |
| risk-tier-rules       | array    | REQUIRED    | Risk classification rules                    |
| waiver-rules          | array    | REQUIRED    | Waiver authorization and limits              |
| override-rules        | array    | REQUIRED    | Override authorization and limits            |
| policy-profiles       | array    | REQUIRED    | Policy sets and enforcement profiles         |
| deployment-rules      | array    | CONDITIONAL | Deployment authorization requirements        |
| configured-by         | string   | REQUIRED    | Governance Administrator                     |
| recorded-at           | ISO 8601 | REQUIRED    | Configuration record time                    |
| status                | enum     | REQUIRED    | draft, active, superseded, revoked           |

### 23.2 Configuration Rules

Governance configuration changes MUST be versioned.

Governance configuration changes MUST be audited.

An active governance configuration MUST be available before readiness progression to Machine-Valid or Autonomous-Ready.

A platform MUST NOT silently change governance configuration during active execution.

If governance configuration changes during active execution, the runtime MUST evaluate whether active tasks are affected.

---

## 24.0 Governance and Standards Alignment

This section defines how governance aligns with external standards and control frameworks. ISL does not require a single external control framework, but enterprise implementations SHOULD map governance policies to applicable control references.

### 24.1 Standards Alignment

| Standard or Framework | Governance Alignment                                                            |
| --------------------- | ------------------------------------------------------------------------------- |
| NIST SP 800-53        | Security and privacy controls may map to Policy control-reference               |
| ISO/IEC 25010         | Quality-related governance may apply to non-functional requirement review       |
| W3C PROV-DM           | Governance events align with provenance activity records                        |
| OpenTelemetry         | Governance telemetry SHOULD align with observability conventions                |
| TOGAF / ArchiMate     | Architecture governance MAY align with enterprise architecture review practices |

### 24.2 Control Reference Rules

A security or compliance Policy SHOULD include an external control reference where applicable.

A governance profile used in regulated environments MUST define the external control framework or internal control baseline it follows.

Where ISL governance diverges from an adopted external control framework, the divergence MUST be documented.

---

## 25.0 Governance Audit Queries

This section defines the minimum governance audit queries that a conforming platform MUST support. Audit queries allow authorized roles to inspect governance history, verify approvals, investigate violations, and support compliance review.

### 25.1 Required Audit Queries

| Query                            | Description                                                        |
| -------------------------------- | ------------------------------------------------------------------ |
| approval-history                 | List approvals for a specification version                         |
| readiness-governance-history     | List governance events affecting readiness transitions             |
| policy-violation-history         | List policy violations by specification, policy, artifact, or task |
| waiver-history                   | List waivers, targets, status, expiry, and approvers               |
| override-history                 | List overrides, targets, status, expiry, and approvers             |
| escalation-history               | List escalations and resolution outcomes                           |
| role-assignment-history          | List active and historical role assignments                        |
| separation-violations            | List detected separation-of-duties violations                      |
| deployment-authorization-history | List deployment authorization decisions                            |
| governance-check-history         | List governance runtime checks and decisions                       |
| risk-tier-history                | List risk tier assignments and changes                             |
| audit-integrity-check            | Verify audit records are present and immutable                     |

### 25.2 Audit Query Rules

Audit query results MUST include identifiers, timestamps, actors, roles, target references, outcomes, and evidence references where applicable.

Audit queries MUST respect authorization rules.

Audit query output SHOULD be exportable in machine-readable form.

---

## 26.0 Governance Record Retention

This section defines retention requirements for governance records. Retention requirements may vary by organization, risk tier, and regulatory domain, but minimum preservation expectations are required for ISL conformance.

### 26.1 Required Retained Records

The platform MUST retain:

* role assignment records
* approval gate records
* policy evaluation records
* waiver records
* override records
* escalation records
* risk tier records
* deployment authorization records
* governance check records
* audit log entries
* governance configuration records

### 26.2 Retention Rules

Governance records associated with a specification version that reached Autonomous-Ready MUST be retained for the life of the generated system unless governance requires longer retention.

Governance records associated with High or Critical risk systems SHOULD be retained under enhanced integrity controls.

Governance records MUST remain queryable for audit.

Expired waivers and overrides MUST remain retained as historical records.

---

## 27.0 Conformance Requirements

This section defines conformance for governance configurations, governance engines, platforms, and governance records.

### 27.1 Governance Configuration Conformance

A governance configuration conforms to ISL v1.7 if it:

* defines required governance roles
* defines role assignment scope rules
* defines approval gate rules
* defines risk tier rules
* defines policy profiles
* defines waiver and override rules
* defines deployment authorization rules when deployment is supported
* supports separation-of-duties validation
* is versioned and auditable

### 27.2 Governance Engine Conformance

A Governance Engine conforms to ISL v1.7 if it can:

* evaluate policies
* enforce violation responses
* manage approval gates
* validate role assignments
* enforce separation-of-duties rules
* assign or validate risk tiers
* manage waivers
* manage overrides
* open and resolve escalations
* produce governance check responses
* record immutable audit events
* expose governance audit queries
* integrate with readiness, planning, execution, tool selection, and deployment preparation

### 27.3 Runtime Governance Conformance

An execution runtime conforms to ISL v1.7 governance requirements if it can:

* request governance checks at defined enforcement points
* pause when approval is required
* block when governance returns block
* escalate when governance returns escalate
* resume only after valid approval, waiver, or override
* record governance effects in execution state
* prevent deployment without deployment authorization
* preserve governance traceability and audit events

### 27.4 Platform Conformance

A platform conforms to ISL v1.7 if it:

* treats governance as continuous control
* enforces readiness approval gates
* enforces runtime approval gates
* enforces policy violation responses
* enforces risk-tier controls
* enforces separation of duties
* preserves immutable governance records
* supports required governance audit queries
* prevents autonomous construction without authorization
* prevents deployment without authorization
* integrates governance with traceability, planning, execution, tools, and artifact lifecycle

---

## 28.0 Summary

ISL v1.7 defines the Governance and Control Model for autonomous software construction. It establishes the roles, approval gates, policies, violation responses, risk tiers, waivers, overrides, escalations, deployment authorization, audit logging, runtime governance contract, and conformance requirements necessary to make autonomous construction enterprise-controlled.

This revision strengthens the governance model by making approval gates executable at runtime. The runtime must pause, block, escalate, resume, or halt according to governance decisions. Governance records must be immutable, traceable, scoped to specification versions, and auditable.

A conforming IMHOTEP platform MUST treat governance as a continuous architectural control layer. Autonomous construction MUST NOT proceed merely because the platform is technically capable of continuing. It may proceed only when the specification, task, artifact, validation, policy, risk tier, and governance state permit it.
