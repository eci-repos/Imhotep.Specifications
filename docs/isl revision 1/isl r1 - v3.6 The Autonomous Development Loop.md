# IMHOTEP Specification Language (ISL) v3.6

# The Autonomous Development Loop

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1, ISL v3.2, ISL v3.3, ISL v3.4, ISL v3.5
**Supersedes:** ISL r0 v3.6 The Autonomous Development Loop
**Document Type:** Autonomous Development Loop Control Specification

---

## 1.0 Scope

This document defines the Autonomous Development Loop for the IMHOTEP platform. It specifies the repeatable control cycle through which the platform transforms formal specifications into validated, traceable, repository-controlled, package-ready, and deployment-prepared software artifacts.

This document applies to autonomous construction runs, repair cycles, specification-change cycles, revalidation cycles, artifact regeneration cycles, package preparation cycles, and deployment preparation cycles.

This document defines:

* loop purpose and principles
* loop lifecycle states
* loop inputs and outputs
* loop stages
* stage gates
* iteration and termination rules
* convergence criteria
* repair and revalidation behavior
* change-driven re-entry
* packaging and deployment preparation gates
* governance controls
* state, traceability, evidence, and telemetry requirements
* loop error classes
* conformance requirements

---

## 2.0 Purpose

The purpose of the Autonomous Development Loop is to define the operational rhythm of IMHOTEP autonomous construction. The loop is the platform-level control cycle that coordinates interpretation, planning, generation, validation, repair, packaging, deployment preparation, and continuous evolution.

The loop is not merely a sequence of phases. It is a governed feedback system. Each iteration produces artifacts, evidence, validation results, state transitions, traceability links, telemetry, and decisions that determine whether the platform should proceed, repair, replan, package, prepare deployment, escalate, halt, or fail.

The Autonomous Development Loop exists to ensure that:

* specification intent drives construction
* construction plans become executable work
* generated artifacts are validated before acceptance
* validation failures trigger bounded repair
* repair does not continue indefinitely
* convergence is evidence-based
* specification changes trigger targeted re-entry
* packaging occurs only after stable artifact convergence
* deployment preparation occurs only after packaging and governance conditions
* every loop decision is recorded, traceable, observable, and governable

---

## 3.0 Loop Design Principles

### 3.1 Specification-Driven Operation

The loop MUST begin from a versioned specification or a governed change to an existing specification. The loop MUST NOT begin from informal instructions, hidden context, or untracked implementation changes.

### 3.2 Evidence-Based Progression

A loop stage MUST NOT be considered complete unless required evidence exists. Evidence MAY include canonical model records, construction plans, task state, artifacts, validation results, repair records, traceability snapshots, governance decisions, or completion reports.

### 3.3 Validation Before Acceptance

Generated or modified artifacts MUST be validated before they are promoted to stable state. Agent or model assertion MUST NOT substitute for deterministic validation when deterministic validation is applicable.

### 3.4 Bounded Repair

Repair cycles MUST be bounded by configured repair limits. The loop MUST escalate or fail when repair limits are reached.

### 3.5 State-Controlled Continuation

The loop MUST use durable platform state to determine continuation. Telemetry, logs, or in-memory task status MUST NOT be the sole authority for loop progression.

### 3.6 Governance-Aware Autonomy

The loop MAY operate autonomously only within governance constraints. Governance decisions MUST be treated as loop control signals.

### 3.7 Targeted Re-Entry

When a specification changes, the loop SHOULD use traceability and impact analysis to identify affected tasks and artifacts. The platform SHOULD avoid full regeneration when targeted reconstruction is sufficient and governed.

---

## 4.0 Loop Lifecycle States

The Autonomous Development Loop MUST maintain explicit lifecycle state.

| State                | Meaning                                                                                |
| -------------------- | -------------------------------------------------------------------------------------- |
| not-started          | Loop has not begun                                                                     |
| admitted             | Loop has passed admission checks                                                       |
| interpreting         | Specification interpretation is active                                                 |
| planning             | Construction planning is active                                                        |
| executing            | Artifact generation and runtime execution are active                                   |
| validating           | Deterministic validation is active                                                     |
| repairing            | Repair cycle is active                                                                 |
| converging           | Platform is evaluating whether generated system satisfies completion criteria          |
| packaging            | Artifact packaging is active                                                           |
| preparing-deployment | Deployment preparation is active                                                       |
| completed            | Loop completed successfully for current scope                                          |
| re-entering          | Loop is restarting due to specification, artifact, policy, tool, or environment change |
| escalated            | Loop requires governance or human resolution                                           |
| halted               | Loop was stopped by policy, runtime, or operator decision                              |
| failed               | Loop ended because required criteria could not be satisfied                            |
| cancelled            | Loop was intentionally cancelled                                                       |

### 4.1 Lifecycle Transition Rules

| From | To | Condition |
|---|---|
| not-started | admitted | Admission checks pass |
| admitted | interpreting | Specification interpretation begins |
| interpreting | planning | Canonical model passes required validation |
| planning | executing | Construction plan is executable |
| executing | validating | Generated artifacts are ready for validation |
| validating | repairing | Required validation fails and repair is permitted |
| repairing | validating | Repaired artifacts are ready for revalidation |
| validating | converging | Required validation passes or valid waivers exist |
| converging | packaging | Convergence criteria satisfied and packaging required |
| converging | completed | Convergence criteria satisfied and packaging not required |
| packaging | preparing-deployment | Packaging succeeds and deployment preparation required |
| packaging | completed | Packaging succeeds and deployment preparation not required |
| preparing-deployment | completed | Deployment preparation criteria satisfied |
| completed | re-entering | Governed change requires new loop cycle |
| re-entering | interpreting | New or changed specification scope admitted |
| any active state | escalated | Governance, repair, validation, or ambiguity requires resolution |
| any active state | halted | Runtime, governance, security, or operator halt occurs |
| any active state | failed | Terminal failure occurs |
| any active state | cancelled | Cancellation is authorized |

No other transition is permitted unless defined by an approved extension.

---

## 5.0 Loop Inputs

Each loop run MUST declare its inputs.

| Input                    | Required    | Description                                |
| ------------------------ | ----------- | ------------------------------------------ |
| specification-id         | YES         | Specification being constructed or changed |
| specification-version    | YES         | Version being constructed or changed       |
| canonical-model-version  | CONDITIONAL | Required after interpretation              |
| readiness-state          | YES         | Readiness level and execution eligibility  |
| construction-plan-id     | CONDITIONAL | Required after planning                    |
| governance-profile-id    | YES         | Governance profile controlling loop        |
| runtime-configuration-id | YES         | Runtime configuration                      |
| model-routing-policy-id  | CONDITIONAL | Required when agents use models            |
| tool-profile-id          | YES         | Tool capabilities available                |
| artifact-repository-id   | YES         | Repository target                          |
| traceability-graph-id    | YES         | Traceability graph for loop                |
| telemetry-profile-id     | YES         | Telemetry profile                          |
| change-trigger-id        | CONDITIONAL | Required for re-entry cycles               |

### 5.1 Input Rules

All loop inputs MUST reference compatible specification and version context.

The loop MUST reject input sets with conflicting specification versions.

The loop MUST reject execution if required tools, agents, stores, or governance profiles are unavailable.

A re-entry loop MUST identify the trigger that caused re-entry.

---

## 6.0 Loop Outputs

Each loop run MUST produce outputs.

| Output                         | Required    | Description                                 |
| ------------------------------ | ----------- | ------------------------------------------- |
| autonomous-loop-run-record     | YES         | Primary loop run record                     |
| canonical-model-record         | CONDITIONAL | Required after interpretation               |
| construction-plan-record       | CONDITIONAL | Required after planning                     |
| execution-graph-record         | YES         | Runtime execution graph                     |
| artifact-records               | YES         | Generated or modified artifact metadata     |
| validation-records             | YES         | Validation results                          |
| repair-records                 | CONDITIONAL | Required when repair occurs                 |
| package-records                | CONDITIONAL | Required when packaging occurs              |
| deployment-preparation-records | CONDITIONAL | Required when deployment preparation occurs |
| governance-records             | CONDITIONAL | Required for governed actions               |
| traceability-snapshot          | YES         | Final traceability snapshot                 |
| telemetry-summary              | YES         | Loop telemetry summary                      |
| evidence-bundle                | YES         | Evidence bundle                             |
| loop-completion-report         | YES         | Final loop report                           |

### 6.1 Output Rules

A successful loop MUST produce a complete evidence bundle.

A failed loop SHOULD produce a partial evidence bundle when possible.

Loop outputs MUST be traceable to loop inputs.

---

## 7.0 Autonomous Loop Run Record

Each loop execution MUST produce an Autonomous Loop Run Record.

| Field                    | Type     | Required    | Description                                                                                            |
| ------------------------ | -------- | ----------- | ------------------------------------------------------------------------------------------------------ |
| loop-run-id              | string   | REQUIRED    | Unique loop run identifier                                                                             |
| loop-run-type            | enum     | REQUIRED    | initial-construction, repair-only, revalidation, change-driven, packaging-only, deployment-preparation |
| specification-id         | string   | REQUIRED    | Specification identifier                                                                               |
| specification-version    | semver   | REQUIRED    | Specification version                                                                                  |
| change-trigger-id        | string   | CONDITIONAL | Change trigger for re-entry                                                                            |
| prior-loop-run-id        | string   | CONDITIONAL | Prior loop run when continuing or re-entering                                                          |
| current-state            | enum     | REQUIRED    | Lifecycle state from §4.0                                                                              |
| governance-profile-id    | string   | REQUIRED    | Governance profile                                                                                     |
| runtime-configuration-id | string   | REQUIRED    | Runtime configuration                                                                                  |
| artifact-repository-id   | string   | REQUIRED    | Artifact repository                                                                                    |
| traceability-graph-id    | string   | REQUIRED    | Traceability graph                                                                                     |
| started-at               | ISO 8601 | REQUIRED    | Start time                                                                                             |
| completed-at             | ISO 8601 | CONDITIONAL | Completion time                                                                                        |
| final-outcome            | enum     | CONDITIONAL | succeeded, failed, escalated, halted, cancelled                                                        |
| evidence-bundle-id       | string   | CONDITIONAL | Evidence bundle produced                                                                               |

---

## 8.0 Stage 1 — Specification Interpretation

Specification Interpretation converts the authored specification into canonical form.

This stage gives the loop a machine-interpretable source of truth. It ensures that later planning and generation operate from canonical entities and relationships rather than unstructured prose.

### 8.1 Required Inputs

* authored specification
* ISL syntax or representation schema
* canonical entity schemas
* identifier policy
* readiness target

### 8.2 Required Outputs

* parsed specification record
* canonical model record
* canonical entity records
* canonical relationship records
* canonical validation report
* specification-to-canonical traceability links

### 8.3 Completion Criteria

Specification Interpretation is complete only when:

* required specification metadata exists
* required sections and entities are parsed
* canonical model generation succeeds
* canonical validation has no blocking findings
* canonical model state is persisted
* traceability links from source specification to canonical entities exist

### 8.4 Failure Rules

A blocking parse error MUST fail or escalate the loop.

A blocking canonical validation error MUST prevent planning.

Unsupported specification content MUST be recorded as warning, rejection, or escalation according to the active governance profile.

---

## 9.0 Stage 2 — Construction Planning

Construction Planning converts the canonical model into an executable construction plan.

This stage determines what must be built, which tasks are required, how tasks depend on one another, which agents or tools are needed, and what artifacts are expected.

### 9.1 Required Inputs

* canonical model record
* readiness state
* governance constraints
* tool capability catalog
* agent role registry
* artifact policy
* traceability graph

### 9.2 Required Outputs

* construction plan record
* construction task graph
* task dependency records
* expected artifact declarations
* validation task declarations
* repair task declarations
* planning validation report

### 9.3 Completion Criteria

Construction Planning is complete only when:

* every required canonical entity is addressed by a task or justified exclusion
* task dependencies are valid
* required agent roles are available
* required tool capabilities are available
* expected artifacts are declared
* validation tasks are declared
* repair policy is attached
* planning validation passes

### 9.4 Failure Rules

A construction plan with unsatisfied dependencies MUST NOT become executable.

A construction plan missing required validation tasks MUST NOT become executable.

A plan requiring unavailable required tools or agents MUST be rejected, delayed, or escalated.

---

## 10.0 Stage 3 — Runtime Execution and Artifact Generation

Runtime Execution performs the construction tasks that generate, modify, or prepare artifacts.

This stage is the main construction activity. Agents, models, tools, and repository services work under runtime orchestration to create candidate implementation artifacts.

### 10.1 Required Inputs

* executable construction plan
* execution graph
* runtime configuration
* agent registry
* model routing policy
* artifact repository workspace
* governance state
* traceability state

### 10.2 Required Outputs

* task execution records
* agent invocation records
* model invocation records when models are used
* candidate artifact records
* artifact content references
* artifact admission records
* artifact state records
* runtime telemetry

### 10.3 Completion Criteria

Runtime Execution and Artifact Generation is complete only when:

* required artifact-producing tasks have completed
* generated artifacts are admitted into repository working state
* artifacts include metadata
* artifacts reference producing tasks and source entities
* task state is persisted
* generation-related traceability links exist

### 10.4 Failure Rules

A failed artifact-producing task MUST trigger retry, repair, escalation, or failure according to runtime policy.

An artifact candidate missing required metadata MUST be rejected.

An artifact produced outside repository admission MUST be quarantined or rejected.

---

## 11.0 Stage 4 — Deterministic Validation

Deterministic Validation verifies artifacts using registered tools and defined validation criteria.

This stage is the inspection function of the loop. It determines whether generated artifacts satisfy structural, functional, security, policy, packaging, or deployment-readiness expectations.

### 11.1 Required Inputs

* pending or repaired artifacts
* validation tasks
* tool capability catalog
* validation profile
* artifact metadata
* governance policy

### 11.2 Required Outputs

* tool invocation records
* normalized tool results
* validation result records
* validation findings
* validation evidence
* artifact validation state updates
* validation-to-artifact traceability links

### 11.3 Required Validation Categories

The loop SHOULD support:

* compile or build validation
* unit test validation
* integration test validation where applicable
* static analysis where configured
* security scan where configured
* dependency audit where configured
* configuration validation where configured
* repository integrity validation
* traceability integrity validation

### 11.4 Completion Criteria

Validation is complete only when:

* required validation tasks have run
* validation outcomes are recorded
* validation evidence is retained
* artifact state reflects validation results
* failed validations are classified
* next action is determined for each failed validation

### 11.5 Failure Rules

A failed required validation MUST prevent artifact promotion unless a valid waiver exists.

A tool timeout MUST NOT be treated as success.

A missing validation result MUST block convergence.

A validation result without artifact version reference MUST be invalid.

---

## 12.0 Stage 5 — Automated Repair

Automated Repair responds to validation failures and repository findings.

This stage uses repair analysis and controlled generation to correct artifacts, re-enter validation, and determine whether the loop is converging or diverging.

### 12.1 Required Inputs

* failed validation result
* failed artifact metadata
* failed artifact content reference
* tool findings
* source canonical entities
* repair policy
* prior repair records
* governance constraints

### 12.2 Required Outputs

* repair analysis record
* repair proposal
* repair task record
* revised artifact version or correction record
* supersession metadata when applicable
* revalidation request
* repair outcome record

### 12.3 Repair Limits

| Parameter                                             |                Default | Configurable |
| ----------------------------------------------------- | ---------------------: | ------------ |
| Maximum repair iterations per artifact                |                      5 | YES          |
| Maximum total repair iterations per construction plan |                     50 | YES          |
| Escalation threshold for same artifact                | 3 consecutive failures | NO           |

### 12.4 Completion Criteria

Repair is complete only when:

* failed validation is linked to repair record
* repair proposal is produced or escalation is recorded
* affected artifacts are identified
* revised artifacts are admitted to repository state
* revised artifacts are revalidated
* repair outcome is recorded

### 12.5 Failure Rules

Repair MUST NOT continue beyond configured repair limits.

Three consecutive failures on the same artifact MUST escalate.

A repair proposal that cannot identify affected artifacts MUST escalate.

A repaired artifact MUST NOT be promoted without revalidation or valid waiver.

---

## 13.0 Stage 6 — Convergence Evaluation

Convergence Evaluation determines whether the generated system satisfies the current loop scope.

This stage prevents the platform from treating partial success as completion. It checks tasks, artifacts, validations, traceability, governance, and repository integrity.

### 13.1 Required Inputs

* execution graph
* task states
* artifact states
* validation records
* repair records
* traceability graph
* governance state
* repository integrity report

### 13.2 Required Outputs

* convergence evaluation record
* convergence outcome
* unresolved issue list
* next action decision

### 13.3 Convergence Outcomes

| Outcome             | Meaning                                                   |
| ------------------- | --------------------------------------------------------- |
| converged           | All required criteria satisfied                           |
| repair-required     | Validation or repository failure can be repaired          |
| replan-required     | Plan is insufficient or invalid                           |
| waiver-required     | Validation or policy exception requires governance waiver |
| escalation-required | Human or governance decision required                     |
| failed              | Required criteria cannot be satisfied                     |
| halted              | Runtime or governance halt applies                        |

### 13.4 Convergence Criteria

The loop converges only when:

* all required tasks are completed, skipped, or waived
* all required artifacts are valid or waived
* no required validation is missing
* no blocking repair remains unresolved
* repository integrity passes
* traceability integrity passes
* governance gates are closed or validly waived
* evidence bundle can be created

### 13.5 Convergence Rules

The loop MUST NOT converge when required validation failed.

The loop MUST NOT converge when stable artifacts are untraced.

The loop MUST NOT converge when governance blocks continuation.

The loop MUST NOT converge solely because no runnable tasks remain.

---

## 14.0 Stage 7 — Artifact Packaging

Artifact Packaging prepares stable artifacts for distribution, release, or deployment preparation.

This stage occurs only after convergence or after a governed packaging-only loop entry.

### 14.1 Required Inputs

* stable artifact records
* artifact content references
* validation evidence
* traceability snapshot
* package profile
* governance state

### 14.2 Required Outputs

* package manifest
* package artifact records
* package validation records
* package evidence records
* package traceability links

### 14.3 Packaging Completion Criteria

Packaging is complete only when:

* package manifest references exact artifact versions
* package contents are stable or explicitly governed exceptions
* required package validation passes
* package hash or equivalent integrity marker exists where supported
* package evidence is retained
* package traceability links exist

### 14.4 Packaging Rules

Packaging MUST NOT include failed artifacts.

Packaging MUST NOT include untraced artifacts.

Packaging MUST NOT include waived artifacts without recording waiver references in the package manifest.

Packaging failures MUST trigger repair, repackaging, escalation, or failure.

---

## 15.0 Stage 8 — Deployment Preparation

Deployment Preparation prepares packaged artifacts for operational deployment.

This stage bridges construction and operation. It does not necessarily deploy the system; it prepares the deployment evidence, manifests, infrastructure definitions, and authorization records required for deployment.

### 15.1 Required Inputs

* package manifest
* stable package artifacts
* target environment profile
* deployment preparation profile
* infrastructure or deployment requirements
* governance state
* operational readiness criteria

### 15.2 Required Outputs

* deployment preparation record
* deployment manifest or deployment descriptor
* environment compatibility report
* deployment validation results
* operational readiness findings
* deployment authorization request when required

### 15.3 Completion Criteria

Deployment Preparation is complete only when:

* deployment manifests or descriptors are produced where required
* target environment compatibility is verified or exceptions are recorded
* required deployment validation passes
* operational readiness findings are resolved, waived, or escalated
* deployment authorization request is created when required

### 15.4 Deployment Preparation Rules

Deployment preparation MUST NOT authorize production deployment by itself.

Deployment authorization MUST follow governance rules.

Deployment preparation MUST preserve traceability to package artifacts and specification version.

---

## 16.0 Change-Driven Loop Re-Entry

The loop may re-enter when specifications, artifacts, tools, policies, models, configuration, or environments change.

### 16.1 Re-Entry Triggers

The loop MUST support re-entry for:

* specification version change
* canonical model change
* requirement change
* policy change
* tool version change affecting validation confidence
* model routing or capability change affecting generated outputs
* artifact repository conflict
* failed deployment preparation
* operational feedback requiring specification update
* governance reauthorization requirement

### 16.2 Change Trigger Record Schema

| Field                          | Type     | Required    | Description                                                                                                                 |
| ------------------------------ | -------- | ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| change-trigger-id              | string   | REQUIRED    | Unique trigger identifier                                                                                                   |
| trigger-type                   | enum     | REQUIRED    | specification, canonical-model, artifact, validation, policy, tool, model, configuration, environment, operational-feedback |
| source-object-id               | string   | REQUIRED    | Object that changed                                                                                                         |
| prior-version                  | string   | CONDITIONAL | Prior version                                                                                                               |
| new-version                    | string   | CONDITIONAL | New version                                                                                                                 |
| affected-specification-id      | string   | REQUIRED    | Specification affected                                                                                                      |
| affected-specification-version | semver   | REQUIRED    | Specification version affected                                                                                              |
| detected-at                    | ISO 8601 | REQUIRED    | Detection time                                                                                                              |
| detected-by                    | string   | REQUIRED    | Component detecting change                                                                                                  |
| impact-analysis-required       | boolean  | REQUIRED    | Whether impact analysis is required                                                                                         |

### 16.3 Re-Entry Rules

A re-entry loop MUST perform impact analysis unless governance requires full reconstruction.

Affected artifacts MUST be revalidated, repaired, regenerated, repackaged, or retired.

Unaffected artifacts SHOULD remain stable if impact analysis confidence is complete.

Partial or uncertain impact analysis MUST escalate or trigger broader reconstruction.

---

## 17.0 Impact Analysis Within the Loop

Impact analysis determines the scope of re-entry.

### 17.1 Impact Analysis Inputs

* change trigger record
* traceability graph
* artifact metadata
* construction plan
* validation records
* package manifests
* governance policy

### 17.2 Impact Analysis Outputs

* affected canonical entities
* affected tasks
* affected artifacts
* affected validations
* affected packages
* affected deployment preparation records
* unaffected artifacts with rationale
* required actions

### 17.3 Required Actions

| Action      | Meaning                                              |
| ----------- | ---------------------------------------------------- |
| no-action   | Object unaffected                                    |
| revalidate  | Validation must rerun                                |
| repair      | Artifact can be corrected                            |
| regenerate  | Artifact must be regenerated                         |
| replan      | Construction plan must change                        |
| repackage   | Package must be rebuilt                              |
| reauthorize | Governance authorization must be renewed             |
| retire      | Artifact or package must be deprecated or superseded |

### 17.4 Impact Rules

Impact analysis MUST use traceability links.

Affected stable artifacts MUST NOT remain stable without required action.

Uncertain impact analysis MUST NOT be treated as no-action.

---

## 18.0 Governance Control Within the Loop

Governance controls loop entry, progression, exception handling, escalation, packaging, deployment preparation, and re-entry.

### 18.1 Governance Control Points

The loop MUST consult governance before:

* initial loop admission
* readiness transition use
* execution start
* restricted model use
* restricted tool use
* waiver application
* override application
* artifact promotion when governed
* packaging when governed
* deployment preparation completion when governed
* change-driven re-entry when policy requires reauthorization

### 18.2 Governance Decision Effects

| Decision          | Loop Effect                 |
| ----------------- | --------------------------- |
| allow             | Continue                    |
| warn              | Continue and record warning |
| block             | Stop affected stage         |
| escalate          | Enter escalated state       |
| approval-required | Pause until approval exists |
| waiver-required   | Pause until waiver exists   |
| override-required | Pause until override exists |

### 18.3 Governance Rules

A governance block MUST prevent affected loop progression.

A pending approval MUST pause affected loop progression.

A waiver MUST be explicit and persisted.

An expired waiver or approval MUST NOT authorize continuation.

Governance audit write failure MUST halt governed loop actions.

---

## 19.0 Loop State and Memory Requirements

The loop MUST maintain durable state and memory.

### 19.1 Required State Categories

The loop MUST update:

* specification state
* canonical model state
* readiness state
* planning state
* execution state
* task state
* artifact state
* validation state
* repair state
* package state
* deployment preparation state
* governance state
* traceability state
* telemetry summary state

### 19.2 State Rules

Loop stage transitions MUST be state events.

Loop continuation decisions MUST be persisted.

A loop restart MUST recover from state and checkpoints.

A loop MUST NOT resume from ambiguous state.

---

## 20.0 Loop Evidence Model

The loop MUST produce evidence demonstrating what occurred.

### 20.1 Evidence Bundle Contents

A complete loop evidence bundle MUST include:

* input specification reference
* canonical model reference
* readiness and governance admission records
* construction plan
* execution graph
* task execution records
* agent invocation records
* model invocation records where applicable
* tool invocation records
* artifact metadata records
* validation records
* repair records where applicable
* package records where applicable
* deployment preparation records where applicable
* traceability snapshot
* telemetry summary
* completion report

### 20.2 Evidence Rules

A successful loop MUST have complete evidence.

A failed loop SHOULD preserve partial evidence.

Evidence records MUST be retained according to governance policy.

Evidence MUST be sufficient to explain why the loop continued, repaired, escalated, halted, failed, or completed.

---

## 21.0 Loop Telemetry

The loop MUST emit telemetry sufficient for operational visibility.

### 21.1 Required Telemetry Events

The platform SHOULD emit:

* autonomous-loop-started
* autonomous-loop-stage-started
* autonomous-loop-stage-completed
* autonomous-loop-stage-failed
* loop-state-transitioned
* specification-interpreted
* construction-plan-created
* artifact-generation-started
* artifact-generation-completed
* validation-started
* validation-failed
* validation-passed
* repair-started
* repair-completed
* convergence-evaluated
* packaging-started
* packaging-completed
* deployment-preparation-started
* deployment-preparation-completed
* loop-reentry-triggered
* autonomous-loop-completed
* autonomous-loop-failed
* autonomous-loop-escalated

### 21.2 Telemetry Rules

Loop telemetry MUST include loop-run-id.

Loop telemetry SHOULD include specification-id, execution-graph-id, task-id, artifact-id, validation-result-id, and repair-record-id where applicable.

Telemetry MUST NOT replace durable loop state or evidence.

---

## 22.0 Loop Termination

The loop MUST terminate explicitly.

### 22.1 Terminal Outcomes

| Outcome   | Meaning                                                       |
| --------- | ------------------------------------------------------------- |
| succeeded | Loop satisfied all required criteria                          |
| failed    | Loop could not satisfy required criteria                      |
| escalated | Loop requires unresolved governance or human action           |
| halted    | Loop stopped by runtime, policy, security, or operator action |
| cancelled | Loop intentionally cancelled                                  |

### 22.2 Success Termination Criteria

The loop may terminate as succeeded only when:

* convergence criteria are satisfied
* required packaging is complete or not required
* required deployment preparation is complete or not required
* required governance gates are closed
* evidence bundle is complete
* completion report is produced

### 22.3 Failure Termination Criteria

The loop MUST terminate as failed when:

* required specification interpretation fails
* required planning fails
* required artifact generation fails without recovery
* required validation fails after repair exhaustion
* repair cannot proceed and no waiver is available
* traceability closure fails
* evidence bundle cannot be produced
* recovery fails and execution cannot safely resume

### 22.4 Termination Rules

A terminal outcome MUST be recorded in the loop run record.

A terminal outcome MUST produce a completion report when possible.

A terminal outcome MUST emit telemetry.

A terminal outcome MUST preserve evidence generated before termination.

---

## 23.0 Autonomous Loop Completion Report

Each loop MUST produce a completion report.

| Field                          | Type     | Required    | Description                                     |
| ------------------------------ | -------- | ----------- | ----------------------------------------------- |
| loop-completion-report-id      | string   | REQUIRED    | Unique completion report identifier             |
| loop-run-id                    | string   | REQUIRED    | Loop run identifier                             |
| loop-run-type                  | enum     | REQUIRED    | Type of loop run                                |
| final-outcome                  | enum     | REQUIRED    | succeeded, failed, escalated, halted, cancelled |
| specification-id               | string   | REQUIRED    | Specification identifier                        |
| specification-version          | semver   | REQUIRED    | Specification version                           |
| stages-completed               | array    | REQUIRED    | Completed stages                                |
| stages-failed                  | array    | CONDITIONAL | Failed stages                                   |
| artifacts-produced             | array    | CONDITIONAL | Artifact identifiers                            |
| stable-artifacts               | array    | CONDITIONAL | Stable artifact identifiers                     |
| packages-produced              | array    | CONDITIONAL | Package identifiers                             |
| deployment-preparation-records | array    | CONDITIONAL | Deployment preparation records                  |
| validation-results             | array    | REQUIRED    | Validation result identifiers                   |
| repair-count                   | integer  | REQUIRED    | Repair attempts                                 |
| waivers-used                   | array    | CONDITIONAL | Waivers used                                    |
| escalations-opened             | array    | CONDITIONAL | Escalations opened                              |
| traceability-snapshot-id       | string   | REQUIRED    | Final traceability snapshot                     |
| evidence-bundle-id             | string   | REQUIRED    | Evidence bundle                                 |
| completed-at                   | ISO 8601 | REQUIRED    | Completion time                                 |

---

## 24.0 Loop Error Classes

This section defines loop-specific error classes.

| Error Class                        | Description                                 | Default Handling                      |
| ---------------------------------- | ------------------------------------------- | ------------------------------------- |
| loop-admission-failed              | Loop admission checks failed                | reject                                |
| loop-input-version-conflict        | Loop inputs reference incompatible versions | reject                                |
| loop-interpretation-failed         | Specification interpretation failed         | fail or escalate                      |
| loop-canonical-validation-failed   | Canonical validation failed                 | fail or escalate                      |
| loop-planning-failed               | Planning failed                             | fail or escalate                      |
| loop-plan-not-executable           | Plan cannot enter execution                 | reject                                |
| loop-artifact-generation-failed    | Artifact generation failed                  | retry or escalate                     |
| loop-artifact-admission-failed     | Artifact could not be admitted              | fail task                             |
| loop-validation-failed             | Required validation failed                  | repair                                |
| loop-validation-missing            | Required validation result missing          | block convergence                     |
| loop-repair-limit-reached          | Repair limit reached                        | escalate                              |
| loop-convergence-blocked           | Convergence criteria not satisfied          | continue, repair, replan, or escalate |
| loop-packaging-failed              | Packaging failed                            | repair, retry, or escalate            |
| loop-deployment-preparation-failed | Deployment preparation failed               | repair, retry, or escalate            |
| loop-impact-analysis-uncertain     | Impact analysis confidence insufficient     | escalate or broaden scope             |
| loop-governance-blocked            | Governance blocks loop progression          | block                                 |
| loop-traceability-incomplete       | Required traceability missing               | block convergence                     |
| loop-evidence-incomplete           | Required evidence missing                   | fail or escalate                      |
| loop-state-inconsistent            | Loop state inconsistent                     | recover                               |
| loop-recovery-failed               | Loop recovery failed                        | halt                                  |
| loop-cancellation-invalid          | Cancellation not authorized or unsafe       | reject cancellation                   |

### 24.1 Error Record Rules

Loop errors MUST be recorded as structured error records.

Loop errors MUST reference loop-run-id.

Blocking loop errors MUST prevent progression until resolved.

Retryable loop errors MUST respect retry policy.

Repairable loop errors MUST respect repair policy.

---

## 25.0 Loop Testing Requirements

The Autonomous Development Loop MUST be testable.

### 25.1 Required Test Categories

| Test Category          | Purpose                                                       |
| ---------------------- | ------------------------------------------------------------- |
| admission              | Verify loop admission checks                                  |
| interpretation         | Verify specification-to-canonical behavior                    |
| planning               | Verify construction task graph creation                       |
| generation             | Verify artifact generation path                               |
| validation             | Verify tool validation path                                   |
| repair                 | Verify bounded repair and revalidation                        |
| convergence            | Verify convergence criteria                                   |
| packaging              | Verify package creation and failure handling                  |
| deployment-preparation | Verify deployment preparation criteria                        |
| re-entry               | Verify change-driven re-entry and impact analysis             |
| governance             | Verify governance pause, block, waiver, and approval behavior |
| state-recovery         | Verify loop restart and recovery                              |
| telemetry              | Verify loop telemetry correlation                             |
| evidence               | Verify evidence bundle completeness                           |

### 25.2 Testing Rules

Repair tests MUST verify termination limits.

Convergence tests MUST verify that missing validation or traceability blocks success.

Re-entry tests MUST verify that affected artifacts are identified through traceability.

Governance tests MUST verify that blocked actions do not continue.

---

## 26.0 Conformance Requirements

### 26.1 Loop Run Conformance

A loop run conforms to ISL v3.6 if it:

* declares loop inputs
* creates a loop run record
* maintains explicit lifecycle state
* executes required stages for its loop-run-type
* records stage outputs
* enforces validation before artifact promotion
* enforces bounded repair
* evaluates convergence using required criteria
* applies governance controls
* records traceability
* emits telemetry
* produces evidence bundle and completion report

### 26.2 Initial Construction Loop Conformance

An initial construction loop conforms to ISL v3.6 if it:

* begins from a versioned specification
* performs interpretation, planning, generation, validation, repair where needed, convergence, and closure
* produces stable artifacts only after validation and traceability
* produces completion evidence

### 26.3 Change-Driven Loop Conformance

A change-driven loop conforms to ISL v3.6 if it:

* records a change trigger
* performs impact analysis
* identifies affected and unaffected artifacts
* applies required actions
* revalidates or regenerates affected artifacts
* preserves unaffected stable artifacts only when justified
* records re-entry evidence

### 26.4 Packaging Loop Conformance

A packaging loop conforms to ISL v3.6 if it:

* uses stable artifacts
* creates package manifests
* references exact artifact versions
* performs package validation
* records package evidence
* preserves package traceability

### 26.5 Deployment Preparation Loop Conformance

A deployment preparation loop conforms to ISL v3.6 if it:

* begins from package records or stable artifacts
* produces deployment preparation records
* verifies environment compatibility
* records operational readiness findings
* routes deployment authorization through governance where required

### 26.6 Platform Conformance

A platform conforms to ISL v3.6 if it:

* implements the Autonomous Development Loop as a stateful, governed control cycle
* supports initial construction and change-driven re-entry
* integrates planning, runtime, agents, models, tools, artifacts, validation, repair, packaging, deployment preparation, state, governance, traceability, and telemetry
* prevents unbounded repair
* prevents evidence-free completion
* prevents governance bypass
* prevents unvalidated artifacts from becoming stable
* supports loop testing and recovery

---

## 27.0 Summary

ISL v3.6 defines the Autonomous Development Loop for the IMHOTEP platform. It specifies the governed feedback cycle through which formal specifications become validated, traceable, repository-controlled, package-ready, and deployment-prepared software artifacts.

This revision strengthens the original loop model by converting a conceptual sequence into a normative control system. It defines lifecycle states, transition rules, loop inputs, loop outputs, loop run records, stage gates, validation requirements, repair limits, convergence criteria, packaging gates, deployment preparation gates, change-driven re-entry, impact analysis, governance control points, state requirements, evidence requirements, telemetry, terminal outcomes, error classes, testing requirements, and conformance requirements.

A conforming Autonomous Development Loop MUST not merely generate and inspect code. It MUST control autonomous construction as a bounded, evidence-based, governed, traceable, observable, recoverable, and repeatable development cycle.
 