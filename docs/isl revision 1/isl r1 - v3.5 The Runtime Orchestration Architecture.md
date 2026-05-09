# IMHOTEP Specification Language (ISL) v3.5

# The Runtime Orchestration Architecture

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1, ISL v3.4
**Supersedes:** ISL r0 v3.5 The Runtime Orchestration Architecture
**Document Type:** Runtime Orchestration Implementation Architecture Specification

---

## 1.0 Scope

This document defines the Runtime Orchestration Architecture for the IMHOTEP reference implementation. It specifies how runtime services, workers, agents, tools, queues, state stores, artifact repositories, governance controls, traceability records, and telemetry systems are coordinated during autonomous construction.

This document applies to the implementation architecture of runtime orchestration. ISL v2.4 defines the normative execution runtime model. This document defines the reference architecture that realizes that model in implementation form.

This document defines:

* runtime orchestration responsibilities
* orchestrator components
* orchestration service boundaries
* task graph execution
* scheduling and dispatch
* queue and work item orchestration
* worker orchestration
* agent orchestration
* tool orchestration
* artifact orchestration
* validation orchestration
* repair orchestration
* event-driven orchestration
* parallel execution orchestration
* governance orchestration
* recovery orchestration
* observability requirements
* conformance requirements

---

## 2.0 Purpose

The purpose of the Runtime Orchestration Architecture is to define how the IMHOTEP platform coordinates active construction work. The runtime orchestrator is the implementation control center that turns an executable construction task graph into ordered, governed, observable, recoverable work.

The runtime orchestrator does not replace the Planning Engine, Agent Orchestrator, Tool Gateway, Artifact Service, Traceability Service, Governance Service, State Manager, or Telemetry Service. Instead, it coordinates those components through controlled contracts.

The Runtime Orchestration Architecture exists to ensure that:

* task graph execution is ordered and deterministic where required
* task eligibility is evaluated consistently
* agents and tools are invoked only through authorized paths
* artifacts are admitted and promoted through repository controls
* validation failures trigger bounded repair behavior
* runtime state remains durable and recoverable
* parallel work does not corrupt artifacts, state, or traceability
* governance gates pause and resume execution through explicit signals
* telemetry shows runtime behavior without replacing state or audit records
* runtime orchestration can scale from local execution to distributed worker pools

---

## 3.0 Runtime Orchestration Principles

### 3.1 The Orchestrator Coordinates but Does Not Own All Work

The runtime orchestrator MUST coordinate execution but MUST NOT absorb the responsibilities of other platform services.

The orchestrator MUST call the Agent Orchestrator for agent work, the Tool Gateway for deterministic tool work, the Artifact Service for artifact lifecycle work, the Governance Service for governed decisions, the Traceability Service for traceability writes, and the State Manager for state transitions.

### 3.2 Task Graph Is the Source of Runtime Work

The orchestrator MUST execute work derived from a valid Construction Task Graph.

The orchestrator MUST NOT create arbitrary construction work that is not represented as a task, validation obligation, repair obligation, governance action, repository action, recovery action, or lifecycle action.

### 3.3 Runtime State Is Authoritative

The orchestrator MUST make scheduling and continuation decisions from durable runtime state.

In-memory scheduler state MAY be used for efficiency but MUST NOT be the sole source of truth.

### 3.4 Governance Is a Control Signal

Governance decisions MUST directly affect orchestration. A block, approval-required, waiver-required, override-required, or escalation decision MUST pause, stop, or route execution according to the decision.

### 3.5 Events Trigger Work, State Controls Work

The orchestration architecture MAY be event-driven. However, events MUST NOT bypass state validation. An event may trigger scheduling evaluation, but the current state MUST determine whether work is eligible.

### 3.6 Concurrency Requires Isolation

Parallel execution MUST be permitted only when dependencies, locks, leases, artifact paths, governance constraints, and resource limits allow it.

### 3.7 Recovery Is an Orchestration Responsibility

The orchestrator MUST detect interrupted, ambiguous, expired, or inconsistent runtime work and coordinate recovery before resuming dispatch.

---

## 4.0 Runtime Orchestrator Components

The runtime orchestration implementation SHOULD include the following components.

| Component                     | Responsibility                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------------- |
| Runtime Admission Controller  | Verifies execution may begin                                                          |
| Execution Graph Manager       | Maintains runtime execution graph                                                     |
| Eligibility Evaluator         | Determines which tasks may run                                                        |
| Scheduler                     | Selects work for dispatch                                                             |
| Queue Manager                 | Manages ready, delayed, retry, repair, validation, governance, and dead-letter queues |
| Dispatch Manager              | Assigns work items to workers or services                                             |
| Worker Coordinator            | Registers workers, monitors heartbeats, and manages worker capacity                   |
| Lease Manager                 | Claims and renews work item leases                                                    |
| Lock Manager                  | Coordinates locks over artifacts, paths, branches, packages, and state                |
| Agent Dispatch Adapter        | Invokes the Agent Orchestrator                                                        |
| Tool Dispatch Adapter         | Invokes the Tool Gateway                                                              |
| Artifact Dispatch Adapter     | Invokes the Artifact Service                                                          |
| Governance Dispatch Adapter   | Invokes the Governance Service                                                        |
| Traceability Dispatch Adapter | Invokes the Traceability Service                                                      |
| State Transition Coordinator  | Commits state transitions and events                                                  |
| Repair Coordinator            | Orchestrates repair cycles                                                            |
| Recovery Coordinator          | Orchestrates runtime recovery                                                         |
| Telemetry Coordinator         | Emits orchestration telemetry                                                         |

### 4.1 Component Rules

Each component MUST expose a defined interface or internal contract.

Components that mutate lifecycle state MUST do so through the State Transition Coordinator or equivalent State Manager contract.

Dispatch adapters MUST NOT bypass target service contracts.

The orchestrator MUST record structured errors from each component.

---

## 5.0 Orchestration Service Boundary

The Runtime Orchestration Service is the deployable service or in-process module that coordinates runtime execution.

### 5.1 Runtime Orchestration Service Responsibilities

The service MUST:

* accept execution start requests
* validate execution admission
* initialize execution graph state
* create work items
* evaluate task eligibility
* schedule work items
* dispatch work items
* monitor worker progress
* coordinate agent and tool invocations
* coordinate artifact repository operations
* coordinate validation and repair
* enforce governance decisions
* update traceability
* persist state transitions
* emit telemetry
* coordinate recovery
* produce completion outcomes

### 5.2 Service Boundary Rules

The Runtime Orchestration Service MUST NOT directly call model providers.

The Runtime Orchestration Service MUST NOT directly execute deterministic tools except through the Tool Gateway or registered tool worker path.

The Runtime Orchestration Service MUST NOT directly promote artifacts except through the Artifact Service.

The Runtime Orchestration Service MUST NOT write governance audit records except through Governance Service or approved governance module.

---

## 6.0 Execution Admission Orchestration

Execution admission determines whether runtime orchestration may begin.

### 6.1 Admission Inputs

| Input                        | Required |
| ---------------------------- | -------- |
| Execution Start Request      | YES      |
| Executable Construction Plan | YES      |
| Readiness State              | YES      |
| Governance State             | YES      |
| Runtime Configuration        | YES      |
| Tool Capability Catalog      | YES      |
| Agent Registry               | YES      |
| Artifact Repository Profile  | YES      |
| Traceability State           | YES      |
| State Store Health           | YES      |

### 6.2 Admission Output

| Output                     | Required    |
| -------------------------- | ----------- |
| Execution Admission Record | YES         |
| Initial Execution Graph    | CONDITIONAL |
| Initial Checkpoint         | CONDITIONAL |
| Admission Error Record     | CONDITIONAL |

### 6.3 Admission Rules

The orchestrator MUST reject execution if the specification is not Autonomous-Ready.

The orchestrator MUST reject execution if the construction plan is not executable.

The orchestrator MUST reject execution if required tool capabilities or agent roles are unavailable.

The orchestrator MUST pause or escalate execution if governance requires approval before start.

The orchestrator MUST NOT create work items until admission succeeds.

---

## 7.0 Execution Graph Orchestration

The Execution Graph Manager maintains the runtime graph representing active construction.

### 7.1 Execution Graph Node Types

| Node Type             | Purpose                                           |
| --------------------- | ------------------------------------------------- |
| task-node             | Construction task                                 |
| work-item-node        | Runtime work item derived from a task             |
| agent-invocation-node | Agent invocation                                  |
| model-invocation-node | Model invocation supporting agent work            |
| tool-invocation-node  | Tool invocation                                   |
| artifact-node         | Artifact produced or modified                     |
| validation-node       | Validation result                                 |
| repair-node           | Repair record                                     |
| governance-node       | Governance check, approval, waiver, or escalation |
| state-event-node      | Significant state event                           |
| telemetry-node        | Telemetry reference where needed                  |

### 7.2 Execution Graph Rules

The graph MUST preserve task dependency relationships.

The graph MUST link each work item to a task.

The graph MUST link generated artifacts to producing work items.

The graph MUST link validation results to evaluated artifacts.

The graph MUST link repair records to failed validation results.

The graph MUST link governance decisions to affected actions.

The graph MUST remain recoverable from durable state and events.

---

## 8.0 Task Eligibility Evaluation

Eligibility evaluation determines whether a task may be queued for execution.

### 8.1 Eligibility Criteria

A task is eligible only when:

* task state is pending or retry-authorized
* all required predecessor tasks are satisfied
* required source entities are available
* required artifacts are available
* required agent role or tool capability is available
* required locks can be acquired or reserved
* governance does not block execution
* retry and repair limits have not been exceeded
* runtime configuration allows execution
* project or tenant isolation constraints are satisfied

### 8.2 Eligibility Evaluation Record Schema

| Field                  | Type     | Required    | Description                                     |
| ---------------------- | -------- | ----------- | ----------------------------------------------- |
| eligibility-record-id  | string   | REQUIRED    | Unique eligibility evaluation identifier        |
| execution-graph-id     | string   | REQUIRED    | Execution graph                                 |
| task-id                | string   | REQUIRED    | Task evaluated                                  |
| prior-task-state       | enum     | REQUIRED    | Task state before evaluation                    |
| eligibility-outcome    | enum     | REQUIRED    | eligible, blocked, delayed, escalated, rejected |
| satisfied-dependencies | array    | CONDITIONAL | Dependencies satisfied                          |
| blocking-reasons       | array    | CONDITIONAL | Reasons task is not eligible                    |
| required-locks         | array    | CONDITIONAL | Locks required                                  |
| governance-check-id    | string   | CONDITIONAL | Governance check affecting eligibility          |
| evaluated-at           | ISO 8601 | REQUIRED    | Evaluation time                                 |
| evaluated-by           | string   | REQUIRED    | Evaluating component                            |

### 8.3 Eligibility Rules

A task with unsatisfied dependencies MUST NOT be queued as ready.

A governance-blocked task MUST be routed to governance handling.

A task requiring unavailable capability MUST be delayed, failed, or escalated according to policy.

Eligibility evaluation MUST be recorded for tasks that are blocked, delayed, escalated, or rejected.

---

## 9.0 Scheduling and Queue Orchestration

Scheduling converts eligible tasks into executable work items and places them into queues.

### 9.1 Queue Types

The orchestrator SHOULD support:

* ready queue
* delayed queue
* retry queue
* validation queue
* repair queue
* governance queue
* repository queue
* dead-letter queue

### 9.2 Scheduling Decision Record Schema

| Field                  | Type     | Required    | Description                                   |
| ---------------------- | -------- | ----------- | --------------------------------------------- |
| scheduling-decision-id | string   | REQUIRED    | Unique scheduling decision identifier         |
| execution-graph-id     | string   | REQUIRED    | Execution graph                               |
| task-id                | string   | REQUIRED    | Task scheduled                                |
| work-item-id           | string   | CONDITIONAL | Work item created                             |
| queue-id               | string   | CONDITIONAL | Queue selected                                |
| priority               | enum     | REQUIRED    | critical, high, medium, low                   |
| scheduling-outcome     | enum     | REQUIRED    | queued, delayed, blocked, escalated, rejected |
| scheduling-reason      | string   | REQUIRED    | Explanation                                   |
| scheduled-at           | ISO 8601 | REQUIRED    | Scheduling time                               |

### 9.3 Scheduling Rules

The scheduler MUST NOT queue duplicate active work items for the same task unless the task contract permits parallel sub-work.

The scheduler MUST preserve dependency order.

The scheduler SHOULD prioritize critical path tasks when all other constraints are equal.

The scheduler MUST move unrecoverable work items to dead-letter or escalation handling.

---

## 10.0 Dispatch Orchestration

Dispatch assigns work to an appropriate execution path.

### 10.1 Dispatch Targets

| Work Item Type | Dispatch Target                                                               |
| -------------- | ----------------------------------------------------------------------------- |
| agent          | Agent Orchestrator or Agent Worker                                            |
| model          | Model Gateway only through agent/model contract                               |
| tool           | Tool Gateway or Tool Worker                                                   |
| validation     | Tool Gateway, Validation Worker, or Review Agent depending on validation type |
| repair         | Repair Coordinator and Repair Analyst                                         |
| repository     | Artifact Service or Repository Worker                                         |
| governance     | Governance Service or Governance Worker                                       |
| traceability   | Traceability Service                                                          |
| platform       | Runtime internal component                                                    |

### 10.2 Dispatch Record Schema

| Field                | Type     | Required    | Description                                                 |
| -------------------- | -------- | ----------- | ----------------------------------------------------------- |
| dispatch-record-id   | string   | REQUIRED    | Unique dispatch record                                      |
| work-item-id         | string   | REQUIRED    | Work item dispatched                                        |
| task-id              | string   | REQUIRED    | Source task                                                 |
| dispatch-target-type | enum     | REQUIRED    | agent, tool, repository, governance, traceability, platform |
| dispatch-target-id   | string   | REQUIRED    | Target service, worker, or component                        |
| lease-id             | string   | CONDITIONAL | Lease acquired                                              |
| lock-ids             | array    | CONDITIONAL | Locks acquired                                              |
| dispatch-outcome     | enum     | REQUIRED    | dispatched, delayed, failed, escalated                      |
| dispatched-at        | ISO 8601 | REQUIRED    | Dispatch time                                               |

### 10.3 Dispatch Rules

A work item MUST NOT be dispatched without an active lease when leases are enabled.

A work item requiring exclusive resource access MUST NOT be dispatched without required locks.

Dispatch failures MUST produce structured runtime errors.

Dispatch MUST preserve correlation identifiers.

---

## 11.0 Worker Orchestration

Workers execute dispatched work items.

### 11.1 Worker Coordination Requirements

The Worker Coordinator MUST:

* register workers
* validate worker capabilities
* monitor worker heartbeats
* track worker assignments
* detect failed workers
* reclaim expired leases through recovery
* support worker draining
* support worker disablement

### 11.2 Worker Assignment Rules

A worker MUST receive only work matching its declared capabilities.

A worker MUST NOT receive work outside its project or tenant authorization scope.

A worker MUST send heartbeat events while executing work.

A worker failure MUST trigger recovery evaluation for active work.

---

## 12.0 Lease and Lock Orchestration

Leases and locks protect execution correctness.

### 12.1 Lease Rules

A work item MUST have no more than one active lease.

Expired leases MUST trigger recovery evaluation.

A lease MUST NOT be reassigned until the orchestrator determines whether prior execution produced side effects.

Long-running work MUST renew leases.

### 12.2 Lock Rules

Artifact-modifying work MUST acquire artifact or path locks.

Branch merge work MUST acquire branch locks.

Package creation work MUST acquire package locks.

State transition work requiring exclusive access MUST acquire state locks or use transactional state updates.

Stale locks MUST trigger recovery before reuse.

---

## 13.0 Agent Orchestration

Agent orchestration coordinates reasoning work through the Agent Orchestrator and Model Gateway.

### 13.1 Agent Orchestration Flow

1. Identify task requiring agent role.
2. Validate role availability.
3. Assemble context package.
4. Perform governance check if required.
5. Bind model capabilities through Model Gateway.
6. Invoke agent.
7. Validate agent output.
8. Route artifact candidates, findings, repair proposals, or review outputs.
9. Update state and traceability.
10. Emit telemetry.

### 13.2 Agent Orchestration Rules

Agents MUST be invoked through the Agent Orchestrator.

Model calls MUST be routed through the Model Gateway.

Agent context MUST be task-scoped.

Agent output MUST be validated before use.

Artifact-producing agent outputs MUST enter Artifact Service admission.

Agent failure MUST be classified and handled through runtime policy.

---

## 14.0 Tool Orchestration

Tool orchestration coordinates deterministic tools through the Tool Gateway.

### 14.1 Tool Orchestration Flow

1. Identify required tool capability.
2. Select registered tool or plugin.
3. Validate tool trust profile.
4. Prepare tool invocation request.
5. Execute tool through Tool Gateway or Tool Worker.
6. Normalize tool result.
7. Record evidence.
8. Map findings to artifacts, validations, tasks, or policies.
9. Determine proceed, repair, retry, escalate, or fail.
10. Emit telemetry.

### 14.2 Tool Orchestration Rules

Tools MUST be registered before use.

Tool invocation MUST produce structured result records.

Tool timeout MUST NOT be treated as success.

Tool failure MUST trigger repair, retry, escalation, or task failure.

Tool evidence required for validation MUST be retained.

---

## 15.0 Artifact Orchestration

Artifact orchestration coordinates generated and modified artifacts through the Artifact Service.

### 15.1 Artifact Orchestration Flow

1. Receive artifact candidate.
2. Request repository admission.
3. Create or update artifact metadata.
4. Set artifact state to pending.
5. Trigger validation where required.
6. Update artifact state based on validation.
7. Route failed artifacts to repair when allowed.
8. Promote valid and traced artifacts to stable state.
9. Preserve evidence and supersession history.
10. Emit repository and artifact telemetry.

### 15.2 Artifact Orchestration Rules

Generated artifacts MUST NOT bypass repository admission.

Artifacts MUST NOT become stable without validation and traceability.

Repair MUST create a new version or supersession metadata when artifact content changes.

Repository integrity failure MUST block promotion and completion.

---

## 16.0 Validation Orchestration

Validation orchestration coordinates deterministic checks and review checks.

### 16.1 Validation Flow

1. Identify validation criteria from task, artifact, policy, or specification.
2. Select validation tool or review path.
3. Dispatch validation work item.
4. Record validation result.
5. Update artifact, task, and execution state.
6. Trigger repair or escalation if failed.
7. Link validation result to traceability graph.
8. Emit telemetry.

### 16.2 Validation Rules

Validation results MUST reference evaluated artifact versions.

A failed required validation MUST block task completion unless waived.

A waived validation MUST reference a valid governance waiver.

Validation evidence MUST be retained for stable artifacts.

---

## 17.0 Repair Orchestration

Repair orchestration manages bounded convergence loops.

### 17.1 Repair Flow

1. Receive failed validation result.
2. Confirm repair is permitted.
3. Check repair limits.
4. Create repair work item.
5. Invoke Repair Analyst.
6. Produce repair proposal.
7. Invoke Implementation Generator or appropriate artifact-producing path.
8. Admit revised artifact.
9. Revalidate revised artifact.
10. Record repair outcome.
11. Continue, escalate, or fail.

### 17.2 Repair Limits

| Limit                                                 |                Default |
| ----------------------------------------------------- | ---------------------: |
| Maximum repair iterations per artifact                |                      5 |
| Maximum total repair iterations per construction plan |                     50 |
| Escalation threshold for same artifact                | 3 consecutive failures |

### 17.3 Repair Rules

Repair MUST be triggered by a recorded validation failure or repository finding.

Repair MUST preserve prior artifact history.

Repair MUST not exceed configured limits.

Repair success MUST be based on revalidation, not agent assertion.

Repair exhaustion MUST create escalation.

---

## 18.0 Event-Driven Orchestration

The runtime orchestration architecture MAY use events to trigger work.

### 18.1 Runtime Event Types

| Event                        | Triggered By                    |
| ---------------------------- | ------------------------------- |
| execution-admitted           | Admission success               |
| task-state-changed           | Task state transition           |
| work-item-queued             | Scheduler                       |
| work-item-dispatched         | Dispatch Manager                |
| agent-output-validated       | Agent output validation         |
| artifact-admitted            | Artifact Service                |
| validation-failed            | Tool or validation result       |
| repair-required              | Failed validation or policy     |
| repair-completed             | Repair Coordinator              |
| governance-decision-received | Governance Service              |
| lock-expired                 | Lock Manager                    |
| lease-expired                | Lease Manager                   |
| checkpoint-created           | Checkpoint Manager              |
| recovery-required            | Runtime error or inconsistency  |
| execution-completed          | Completion conditions satisfied |

### 18.2 Event Rules

Events MUST include correlation identifiers.

Events MUST NOT be considered authoritative without corresponding state records where lifecycle state is affected.

Event handling MUST be idempotent where duplicate delivery is possible.

Event-triggered scheduling MUST re-check current state before dispatch.

---

## 19.0 Parallel Execution Orchestration

Parallel orchestration allows independent work to run concurrently.

### 19.1 Parallel Eligibility

Tasks MAY run in parallel only when:

* no dependency relationship requires ordering
* no shared exclusive lock is required
* no artifact path conflict exists
* no governance gate requires sequencing
* worker capacity is available
* project or tenant isolation is preserved
* traceability can be recorded independently

### 19.2 Parallel Execution Rules

The orchestrator MUST prevent duplicate execution of the same work item.

The orchestrator MUST prevent conflicting artifact writes.

The orchestrator MUST pause dependent tasks when prerequisite tasks fail.

The orchestrator MAY continue unrelated tasks after a local failure when governance and runtime policy permit it.

Parallel execution MUST not weaken validation or traceability requirements.

---

## 20.0 Governance Orchestration

Governance orchestration integrates approvals, waivers, overrides, policy checks, and escalations into runtime execution.

### 20.1 Governance Orchestration Points

The orchestrator MUST consult governance before:

* execution start
* governed task dispatch
* restricted model use
* restricted tool use
* high-risk artifact generation
* artifact promotion when governed
* repair of high-risk artifacts
* waiver application
* override application
* deployment preparation completion

### 20.2 Governance Work Item Schema

| Field                       | Type     | Required    | Description                                                    |
| --------------------------- | -------- | ----------- | -------------------------------------------------------------- |
| governance-work-item-id     | string   | REQUIRED    | Unique governance work item                                    |
| governance-action           | enum     | REQUIRED    | check, approval, waiver, override, escalation                  |
| affected-task-id            | string   | CONDITIONAL | Task affected                                                  |
| affected-artifact-id        | string   | CONDITIONAL | Artifact affected                                              |
| affected-execution-graph-id | string   | REQUIRED    | Execution graph affected                                       |
| gate-type                   | enum     | CONDITIONAL | Approval gate type                                             |
| requested-decision          | enum     | REQUIRED    | allow, block, warn, escalate, approve, reject, waive, override |
| status                      | enum     | REQUIRED    | pending, approved, rejected, blocked, escalated, expired       |
| requested-at                | ISO 8601 | REQUIRED    | Request timestamp                                              |
| resolved-at                 | ISO 8601 | CONDITIONAL | Resolution timestamp                                           |

### 20.3 Governance Rules

A governance work item in pending state MUST pause affected execution.

A rejected approval MUST block the affected action.

An expired waiver MUST NOT authorize continuation.

A governance decision MUST re-enter the execution graph as a recorded event and state transition.

---

## 21.0 Recovery Orchestration

Recovery orchestration restores safe execution after interruption, failure, or inconsistency.

### 21.1 Recovery Triggers

Recovery MUST be triggered by:

* runtime restart during active execution
* worker heartbeat expiration
* lease expiration
* stale lock detection
* state transaction failure
* repository inconsistency
* traceability write failure
* governance audit write failure
* distributed split-brain detection
* checkpoint validation failure

### 21.2 Recovery Flow

1. Pause dispatch.
2. Mark affected execution recovering.
3. Load latest checkpoint.
4. Inspect active leases and locks.
5. Validate worker state.
6. Validate state events after checkpoint.
7. Validate artifact repository state.
8. Validate traceability state.
9. Validate governance state.
10. Requeue safe work.
11. Quarantine ambiguous artifacts.
12. Escalate unsafe work.
13. Record recovery event.
14. Resume, halt, or escalate.

### 21.3 Recovery Rules

The orchestrator MUST NOT resume dispatch until recovery validation completes.

Ambiguous side effects MUST be quarantined or escalated.

Recovery MUST not erase prior state events.

Recovery failure MUST halt affected execution.

---

## 22.0 Completion Orchestration

Completion orchestration determines whether execution can finish.

### 22.1 Completion Checks

Execution may complete only when:

* all required tasks are completed, skipped, or waived
* no ready, delayed, retry, validation, repair, or governance work remains unresolved
* all required artifacts are stable or governed as non-deployable
* required validation results passed or were waived
* repair records are resolved
* traceability integrity passes
* repository integrity passes
* governance gates are closed
* completion report is produced
* final checkpoint is created

### 22.2 Completion Rules

The orchestrator MUST NOT complete execution when blocking work remains.

The orchestrator MUST NOT complete execution when traceability is incomplete.

The orchestrator MUST NOT complete execution when repository integrity fails.

Completion MUST produce a completion record and telemetry event.

---

## 23.0 Orchestration State Records

The runtime orchestrator MUST maintain state records sufficient for recovery and audit.

### 23.1 Required Records

| Record                          | Purpose                          |
| ------------------------------- | -------------------------------- |
| execution-orchestration-record  | Overall orchestration state      |
| eligibility-record              | Task eligibility result          |
| scheduling-decision-record      | Scheduling decision              |
| dispatch-record                 | Work dispatch                    |
| worker-assignment-record        | Worker assignment                |
| lease-record                    | Work item claim                  |
| lock-record                     | Resource protection              |
| governance-work-item            | Runtime governance decision flow |
| repair-orchestration-record     | Repair loop state                |
| recovery-orchestration-record   | Recovery behavior                |
| completion-orchestration-record | Completion evidence              |

### 23.2 State Rules

Orchestration records MUST include execution-graph-id and correlation-id.

Records affecting lifecycle state MUST be durable.

Records MUST be linked to traceability where applicable.

Records MUST be queryable after execution completion.

---

## 24.0 Orchestration Telemetry

Runtime orchestration MUST emit operational telemetry.

### 24.1 Required Telemetry Events

The orchestrator SHOULD emit:

* orchestration-started
* execution-admission-evaluated
* task-eligibility-evaluated
* task-scheduled
* work-item-dispatched
* worker-assigned
* lease-acquired
* lock-acquired
* agent-dispatched
* tool-dispatched
* artifact-operation-dispatched
* validation-dispatched
* repair-dispatched
* governance-work-item-created
* recovery-started
* recovery-completed
* execution-completion-evaluated
* orchestration-completed
* orchestration-failed

### 24.2 Telemetry Rules

Telemetry MUST include correlation identifiers.

Telemetry MUST include execution-graph-id when available.

Telemetry MUST not expose restricted context or secrets.

Telemetry failure MUST not hide lifecycle errors.

---

## 25.0 Runtime Orchestration Security

Runtime orchestration must protect platform control boundaries.

### 25.1 Security Rules

The orchestrator MUST authenticate service-to-service calls in enterprise deployments.

The orchestrator MUST enforce project and tenant isolation.

The orchestrator MUST not pass secrets into agent context unless explicitly governed.

The orchestrator MUST enforce model, tool, artifact, and repository access controls through their target services.

The orchestrator MUST treat untrusted agent, model, tool, artifact, and external input as untrusted until validated.

A security boundary violation MUST halt or isolate affected execution.

---

## 26.0 Orchestration Error Classes

Runtime orchestration errors specialize the runtime error taxonomy.

| Error Class                             | Description                                 | Default Handling              |
| --------------------------------------- | ------------------------------------------- | ----------------------------- |
| orchestration-admission-failed          | Execution admission failed                  | reject                        |
| orchestration-eligibility-failed        | Eligibility evaluation failed               | escalate                      |
| orchestration-scheduling-failed         | Scheduling could not create valid work item | retry or escalate             |
| orchestration-dispatch-failed           | Work item could not be dispatched           | retry or escalate             |
| orchestration-worker-unavailable        | Required worker unavailable                 | delay or escalate             |
| orchestration-lease-conflict            | Duplicate or conflicting lease detected     | halt and recover              |
| orchestration-lock-conflict             | Required lock unavailable or conflicting    | delay or recover              |
| orchestration-agent-dispatch-failed     | Agent dispatch failed                       | retry or escalate             |
| orchestration-tool-dispatch-failed      | Tool dispatch failed                        | retry or escalate             |
| orchestration-artifact-operation-failed | Artifact service operation failed           | retry, repair, or escalate    |
| orchestration-validation-failed         | Validation returned failure                 | repair or escalate            |
| orchestration-repair-limit-reached      | Repair limit reached                        | escalate                      |
| orchestration-governance-pending        | Governance action pending                   | pause                         |
| orchestration-governance-blocked        | Governance blocked action                   | block                         |
| orchestration-traceability-write-failed | Traceability write failed                   | halt or recover               |
| orchestration-state-commit-failed       | State transition failed                     | recover                       |
| orchestration-recovery-failed           | Recovery could not restore safe state       | halt                          |
| orchestration-completion-blocked        | Completion criteria not satisfied           | continue, repair, or escalate |

### 26.1 Error Rules

Every orchestration error MUST produce a structured error record.

Blocking orchestration errors MUST prevent affected action.

Retryable orchestration errors MUST respect retry policy.

Recovery-related orchestration errors MUST pause dispatch.

Governance-blocked orchestration errors MUST NOT be bypassed.

---

## 27.0 Local and Distributed Orchestration Modes

The runtime orchestrator MUST support local-first operation and MAY support distributed execution.

### 27.1 Local Mode

In local mode, orchestration MAY run in-process with local queues and local workers.

Local mode MUST still preserve:

* task state
* artifact repository admission
* deterministic validation
* governance checks
* traceability writes
* telemetry emission
* repair limits

### 27.2 Distributed Mode

In distributed mode, orchestration MUST provide:

* durable queues
* distributed leases
* distributed locks
* worker heartbeats
* shared state store
* artifact repository consistency
* traceability consistency
* governance consistency
* recovery after worker failure

### 27.3 Mode Rules

Distributed mode MUST NOT be enabled without durable state and recoverable queues.

Local mode MUST NOT claim distributed conformance.

Mode changes during active execution MUST trigger impact and safety evaluation.

---

## 28.0 Runtime Orchestration Testing

The orchestration architecture MUST be tested.

### 28.1 Required Test Categories

| Test Category      | Purpose                                               |
| ------------------ | ----------------------------------------------------- |
| admission          | Validate execution admission checks                   |
| eligibility        | Validate task eligibility decisions                   |
| scheduling         | Validate queue placement and priority                 |
| dispatch           | Validate service and worker dispatch                  |
| lease-lock         | Validate leases, locks, expiration, and conflicts     |
| agent-dispatch     | Validate agent orchestration flow                     |
| tool-dispatch      | Validate tool orchestration flow                      |
| artifact-flow      | Validate artifact admission, validation, promotion    |
| repair-loop        | Validate bounded repair behavior                      |
| governance-flow    | Validate approval, waiver, block, escalation handling |
| recovery           | Validate recovery after interruption                  |
| parallel-execution | Validate dependency-safe concurrency                  |
| completion         | Validate completion criteria                          |
| telemetry          | Validate correlation and telemetry emission           |

### 28.2 Testing Rules

The orchestrator MUST have tests for success and failure paths.

Repair loop tests MUST verify termination.

Governance flow tests MUST verify pause and resume behavior.

Recovery tests SHOULD use failure injection.

Parallel execution tests MUST verify no conflicting artifact writes occur.

---

## 29.0 Conformance Requirements

### 29.1 Orchestrator Component Conformance

An orchestrator component conforms to ISL v3.5 if it:

* has a defined responsibility
* exposes structured contracts
* records lifecycle-relevant state
* emits telemetry where applicable
* handles errors using defined error classes
* preserves governance, traceability, artifact, and state boundaries

### 29.2 Runtime Orchestration Service Conformance

A Runtime Orchestration Service conforms to ISL v3.5 if it:

* validates execution admission
* initializes execution graph state
* evaluates task eligibility
* schedules work items
* dispatches work through authorized services or workers
* manages queues, leases, locks, and worker coordination
* coordinates agent, tool, artifact, validation, repair, governance, traceability, and state operations
* supports completion evaluation
* supports recovery
* emits telemetry
* produces structured error records

### 29.3 Local Orchestration Conformance

A local orchestration implementation conforms to ISL v3.5 if it:

* executes task graph work in local mode
* preserves logical orchestration boundaries
* records task, artifact, validation, repair, traceability, governance, and state records
* enforces deterministic validation
* enforces repair limits
* produces completion evidence

### 29.4 Distributed Orchestration Conformance

A distributed orchestration implementation conforms to ISL v3.5 if it:

* supports durable queues
* supports worker registration and heartbeat
* supports distributed leases and locks
* prevents duplicate execution
* preserves project and tenant isolation
* supports recovery after worker failure
* detects split-brain or inconsistent state
* preserves traceability and governance consistency

### 29.5 Platform Conformance

A platform conforms to ISL v3.5 if it:

* routes all runtime construction activity through runtime orchestration
* prevents direct bypass of agents, tools, artifacts, governance, traceability, and state services
* controls task graph execution through eligibility, scheduling, dispatch, leases, and locks
* supports bounded repair and recovery
* supports event-driven orchestration without bypassing state controls
* supports observability for orchestration behavior
* completes execution only when evidence-based completion criteria are satisfied

---

## 30.0 Summary

ISL v3.5 defines the Runtime Orchestration Architecture for the IMHOTEP reference implementation. It explains how the runtime acts as the construction coordinator that transforms an executable construction plan into governed, traceable, validated, recoverable work.

This revision strengthens the original runtime orchestration concept by making the orchestration architecture implementable. It defines orchestrator components, service boundaries, admission, execution graph management, eligibility evaluation, scheduling, queueing, dispatch, worker coordination, leases, locks, agent orchestration, tool orchestration, artifact orchestration, validation, bounded repair, event-driven behavior, parallel execution, governance pause/resume behavior, recovery, completion, state records, telemetry, security, errors, operating modes, testing, and conformance requirements.

A conforming Runtime Orchestration Architecture MUST coordinate autonomous construction without becoming an uncontrolled monolith. It MUST be the construction foreman: scheduling the work, assigning the workforce, enforcing safety, recording progress, responding to failure, calling inspections, and stopping the worksite when evidence or governance says the system is not ready.
