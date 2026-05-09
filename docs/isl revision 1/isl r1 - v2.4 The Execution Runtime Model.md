# IMHOTEP Specification Language (ISL) v2.4

# The Execution Runtime Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3
**Supersedes:** ISL r0 v2.4 The Execution Runtime Model
**Document Type:** Runtime Execution, Scheduling, and Error Handling Specification

---

## 1.0 Scope

This document defines the Execution Runtime Model for the IMHOTEP platform. It establishes how executable construction plans are accepted, scheduled, dispatched, executed, monitored, paused, repaired, resumed, failed, halted, and completed.

This document applies to runtime execution after a specification has reached the readiness state required for autonomous construction and after a valid Construction Task Graph has been produced. It defines runtime lifecycle behavior, task scheduling, worker coordination, queues, leases, locks, retries, timeouts, checkpoints, distributed execution, governance control, runtime isolation, error handling, recovery, and conformance requirements.

This document defines:

* runtime responsibilities
* runtime lifecycle
* execution admission
* task scheduling
* execution queues
* worker model
* task dispatch and leases
* task execution lifecycle
* agent invocation during runtime
* tool invocation during runtime
* artifact repository interaction
* validation and feedback loops
* bounded repair cycles
* parallel and distributed execution
* runtime governance enforcement
* runtime checkpoints and recovery
* runtime error taxonomy
* runtime configuration
* runtime telemetry
* conformance requirements

---

## 2.0 Purpose of the Execution Runtime Model

The purpose of the Execution Runtime Model is to define how the IMHOTEP platform operationally performs autonomous construction. Earlier ISL documents define the specification, semantic model, readiness gates, construction plan, traceability model, tool integration model, governance controls, state model, and artifact repository model. This document defines how those elements become running work.

The execution runtime is the platform’s construction engine. It turns the static Construction Task Graph into controlled runtime activity. It decides which tasks are eligible to run, which worker should execute them, which governance checks apply, which tools or agents must be invoked, which artifacts are produced, which validations must run, and how failures are handled.

The Execution Runtime Model exists to ensure that:

* execution begins only from authorized input
* task scheduling respects dependencies and governance constraints
* runtime work is performed by controlled workers
* artifacts are generated through repository-controlled flows
* deterministic validation gates artifact acceptance
* repair cycles are bounded and observable
* runtime state survives interruption
* distributed execution preserves consistency
* all runtime errors have defined handling paths
* completion is based on evidence rather than optimism

---

## 3.0 Normative References

This section identifies the ISL documents required to interpret and implement the Execution Runtime Model.

| Reference     | Purpose                                                                             |
| ------------- | ----------------------------------------------------------------------------------- |
| ISL v0.0      | Corpus-level principles, conformance, and normative language                        |
| ISL v1.1      | Canonical semantic model and entity identifiers                                     |
| ISL v1.2      | Execution phases, validation, repair, and completion rules                          |
| ISL v1.3      | Readiness levels and Autonomous-Ready gating                                        |
| ISL v1.4      | Traceability graph, traceability timing, and integrity checks                       |
| ISL v1.5      | Construction Task Graph, task types, dependencies, and planning handoff             |
| ISL v1.6      | Tool registration, invocation, result normalization, and tool errors                |
| ISL v1.7      | Governance checks, approval gates, waivers, overrides, escalation, and audit        |
| ISL v2.0      | Platform architecture and runtime subsystem boundaries                              |
| ISL v2.1      | Agent roles, invocation contracts, output contracts, and failure handling           |
| ISL v2.2      | State, memory, checkpoints, snapshots, transactions, and recovery                   |
| ISL v2.3      | Artifact repository, artifact lifecycle, branching, merge, promotion, and packaging |
| OpenTelemetry | Runtime telemetry alignment                                                         |
| RFC 2119      | Normative terminology                                                               |

---

## 4.0 Terms and Definitions

**Execution Runtime**
The platform subsystem responsible for executing an executable construction plan.

**Runtime Instance**
A running instance of the Execution Runtime. A deployment MAY contain one or more runtime instances.

**Runtime Worker**
A process, thread, container, service, or node that executes assigned runtime work.

**Scheduler**
The runtime component that determines which tasks are eligible for execution and dispatches them to workers.

**Execution Queue**
A controlled queue containing runnable or delayed work items.

**Work Item**
A runtime unit of executable work derived from a construction task, validation task, repair task, governance check, or platform lifecycle action.

**Task Lease**
A temporary claim by a worker on a work item to prevent duplicate execution.

**Runtime Lock**
A concurrency control mechanism that protects artifacts, branches, repositories, state records, or shared resources.

**Checkpoint**
A durable runtime recovery marker representing a safe state boundary.

**Retry**
A repeated attempt to execute a failed or interrupted work item.

**Runtime Error**
A structured error produced by the runtime or one of its controlled execution activities.

**Terminal Runtime State**
A runtime state from which execution does not proceed without a new execution request, recovery action, or governance decision.

---

## 5.0 Runtime Design Principles

### 5.1 Runtime Is Plan-Driven

The runtime MUST execute work from a valid Construction Task Graph. It MUST NOT execute arbitrary construction work that is not represented as a task, lifecycle obligation, governance action, validation action, or recovery action.

### 5.2 Runtime Is State-Driven

The runtime MUST make scheduling and execution decisions from durable platform state. It MUST NOT depend on hidden process memory as the source of truth for execution progress.

### 5.3 Runtime Is Governed

The runtime MUST consult the Governance Engine at required control points. A governance decision of block, escalate, approval-required, waiver-required, or override-required MUST prevent the governed action from continuing.

### 5.4 Runtime Is Traceable

The runtime MUST create traceability links at the time runtime actions occur. Runtime traceability MUST connect tasks, workers, agents, tools, artifacts, validations, repairs, governance events, and state transitions.

### 5.5 Runtime Is Validation-Centered

The runtime MUST NOT treat generated artifacts as valid until required validation has passed or a valid governance waiver exists.

### 5.6 Runtime Is Recoverable

The runtime MUST persist enough state, events, leases, checkpoints, and artifact metadata to recover safely after interruption.

### 5.7 Runtime Errors Are Structured

Every runtime error that affects execution progress, artifact status, governance, traceability, validation, or recovery MUST be recorded as a structured runtime error.

---

## 6.0 Runtime Responsibilities

This section defines the required responsibilities of the Execution Runtime. The runtime coordinates many subsystems but MUST NOT absorb their responsibilities.

### 6.1 Required Runtime Responsibilities

The Execution Runtime MUST:

* accept execution handoff records from the Planning Engine
* validate execution admission preconditions
* initialize execution state
* maintain execution graph state
* schedule eligible tasks
* manage execution queues
* dispatch work to runtime workers
* manage task leases and runtime locks
* invoke agents through the Agent Orchestrator
* invoke tools through the Tool Integration Layer
* interact with the Artifact Repository through controlled interfaces
* initiate validation tasks
* initiate repair tasks when permitted
* enforce retry, timeout, and repair limits
* consult the Governance Engine at required control points
* update traceability as runtime events occur
* update state and memory according to ISL v2.2
* emit runtime telemetry
* create checkpoints
* recover after interruption
* produce runtime completion records

### 6.2 Runtime Prohibitions

The runtime MUST NOT:

* execute a non-executable construction plan
* begin autonomous construction for a non-Autonomous-Ready specification
* bypass governance checks
* bypass artifact repository admission
* mark artifacts valid without validation or waiver
* modify canonical models directly
* silently drop failed tasks
* retry indefinitely
* treat tool timeout or agent timeout as success
* complete execution while required traceability is incomplete

---

## 7.0 Runtime Inputs

This section defines the structured inputs required before runtime execution may begin.

### 7.1 Required Inputs

| Input                         | Required    | Description                                                |
| ----------------------------- | ----------- | ---------------------------------------------------------- |
| Execution Handoff Record      | YES         | Handoff from Planning Engine                               |
| Construction Task Graph       | YES         | Executable task graph                                      |
| Readiness State               | YES         | Autonomous-Ready readiness evidence                        |
| Canonical Model Reference     | YES         | Canonical model used for planning                          |
| Traceability State            | YES         | Initialized traceability graph                             |
| Governance State              | YES         | Policies, approvals, waivers, overrides, risk tier         |
| Artifact Repository Reference | YES         | Repository target for outputs                              |
| Runtime Configuration         | YES         | Scheduling, retry, timeout, worker, and isolation settings |
| Tool Registry                 | YES         | Registered deterministic tools                             |
| Agent Role Registry           | YES         | Supported agent roles and contracts                        |
| State Store Reference         | YES         | Runtime state and memory storage                           |
| Observability Configuration   | CONDITIONAL | Required when telemetry is enabled or mandated             |

### 7.2 Input Consistency Rules

All runtime inputs MUST reference the same specification-id and specification-version.

The Construction Task Graph MUST match the plan-id in the Execution Handoff Record.

The plan MUST have status executable.

The specification version MUST be Autonomous-Ready.

The runtime MUST reject execution admission if input versions conflict.

---

## 8.0 Runtime Outputs

This section defines required runtime outputs.

### 8.1 Required Outputs

| Output                   | Description                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------ |
| Execution Graph          | Runtime graph of phases, tasks, work items, workers, validations, repairs, and governance events |
| Runtime State Records    | Current and historical runtime state                                                             |
| Work Item Records        | Runtime records for dispatched work                                                              |
| Task Execution Records   | Task-level execution results                                                                     |
| Agent Invocation Records | Agent invocations and outputs                                                                    |
| Tool Invocation Records  | Tool invocations and normalized results                                                          |
| Artifact Records         | Generated or modified artifact metadata                                                          |
| Validation Records       | Validation results and findings                                                                  |
| Repair Records           | Repair attempts and outcomes                                                                     |
| Governance Check Records | Governance decisions affecting runtime                                                           |
| Runtime Error Records    | Structured errors and handling outcomes                                                          |
| Runtime Telemetry        | Operational runtime events                                                                       |
| Checkpoint Records       | Recovery markers                                                                                 |
| Completion Report        | Final execution result                                                                           |

### 8.2 Output Integrity Rules

Runtime outputs MUST be persisted before they are used as evidence for downstream lifecycle actions.

Runtime outputs that affect audit, governance, traceability, validation, artifacts, or recovery MUST NOT be stored only in transient memory.

---

## 9.0 Runtime Lifecycle

This section defines the lifecycle of runtime execution.

### 9.1 Runtime Lifecycle States

| State              | Meaning                                                              |
| ------------------ | -------------------------------------------------------------------- |
| initialized        | Runtime instance is available but no execution is active             |
| admission-checking | Runtime is validating execution admission                            |
| admitted           | Execution request accepted                                           |
| preparing          | Runtime is initializing queues, state, workers, and checkpoints      |
| running            | Runtime is actively executing work                                   |
| pausing            | Runtime is moving toward paused state                                |
| paused             | Runtime is suspended but resumable                                   |
| recovering         | Runtime is validating and reconstructing state after interruption    |
| draining           | Runtime is finishing in-flight work but not accepting new dispatches |
| completed          | Execution completed successfully                                     |
| failed             | Execution ended due to unrecoverable failure                         |
| halted             | Execution stopped by governance, runtime, or operator decision       |
| cancelled          | Execution was intentionally cancelled before completion              |
| escalated          | Execution awaits governance or human action                          |

### 9.2 Lifecycle Transition Rules

| From               | To                 | Condition                                                     |
| ------------------ | ------------------ | ------------------------------------------------------------- |
| initialized        | admission-checking | Execution request received                                    |
| admission-checking | admitted           | Admission checks pass                                         |
| admission-checking | failed             | Admission checks fail                                         |
| admitted           | preparing          | Runtime begins setup                                          |
| preparing          | running            | Queues, state, and checkpoints initialized                    |
| running            | pausing            | Pause requested or governance requires pause                  |
| pausing            | paused             | In-flight policy satisfied or suspended                       |
| paused             | running            | Resume authorized                                             |
| running            | draining           | Completion, halt, cancellation, or controlled shutdown begins |
| draining           | completed          | Completion criteria satisfied                                 |
| draining           | halted             | Halt confirmed                                                |
| draining           | cancelled          | Cancellation confirmed                                        |
| running            | escalated          | Blocking escalation opened                                    |
| escalated          | running            | Escalation resolved with resume                               |
| escalated          | halted             | Escalation resolved with halt                                 |
| running            | recovering         | Interruption or consistency failure detected                  |
| recovering         | running            | Recovery outcome is resume-safe                               |
| recovering         | halted             | Recovery cannot safely resume                                 |
| running            | failed             | Unrecoverable failure occurs                                  |

No other lifecycle transitions are permitted unless an approved extension defines them.

---

## 10.0 Execution Admission

Execution admission is the runtime gate that determines whether a construction plan may enter runtime execution.

### 10.1 Admission Checks

The runtime MUST verify:

* specification is Autonomous-Ready
* construction plan status is executable
* execution handoff record is valid
* canonical model reference exists
* traceability state is initialized and active
* governance state permits execution
* required approval gates are satisfied
* required tool capabilities are available
* required agent roles are available
* artifact repository is available
* state store is available
* runtime configuration is valid
* no blocking regression exists
* no expired waiver or override is being used
* no unresolved planning validation error remains

### 10.2 Execution Admission Record Schema

| Field | Type | Required | Description |
|---|---|---|
| admission-record-id | string | REQUIRED | Unique admission record identifier |
| execution-request-id | string | REQUIRED | Execution request evaluated |
| plan-id | string | REQUIRED | Construction plan |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| admission-outcome | enum | REQUIRED | admitted, rejected, escalated |
| checks-performed | array | REQUIRED | Admission checks performed |
| failed-checks | array | CONDITIONAL | Failed checks |
| governance-check-id | string | CONDITIONAL | Governance check used |
| required-action | string | CONDITIONAL | Required action when rejected or escalated |
| evaluated-at | ISO 8601 | REQUIRED | Evaluation time |
| evaluated-by | string | REQUIRED | Runtime component evaluating admission |

### 10.3 Admission Rules

A rejected execution request MUST NOT initialize execution queues.

An escalated execution request MUST wait for governance resolution.

An admitted execution request MUST create an Execution Graph and initial checkpoint.

---

## 11.0 Execution Graph Runtime Representation

The runtime MUST maintain an Execution Graph derived from the Construction Task Graph and enriched with runtime state.

### 11.1 Runtime Execution Graph Schema

| Field | Type | Required | Description |
|---|---|---|
| execution-graph-id | string | REQUIRED | Unique execution graph identifier |
| plan-id | string | REQUIRED | Construction plan being executed |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| runtime-instance-id | string | REQUIRED | Runtime instance that initialized graph |
| execution-state | enum | REQUIRED | Runtime lifecycle state |
| task-nodes | array | REQUIRED | Task nodes |
| work-item-nodes | array | CONDITIONAL | Runtime work item nodes |
| artifact-nodes | array | CONDITIONAL | Artifact nodes |
| validation-nodes | array | CONDITIONAL | Validation result nodes |
| repair-nodes | array | CONDITIONAL | Repair nodes |
| governance-nodes | array | CONDITIONAL | Governance events |
| edges | array | REQUIRED | Dependency, derivation, runtime, and state edges |
| created-at | ISO 8601 | REQUIRED | Creation time |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 11.2 Execution Graph Rules

The Execution Graph MUST remain consistent with task state, artifact state, validation state, repair state, governance state, and traceability state.

Runtime workers MUST update Execution Graph state only through authorized runtime state transitions.

The Execution Graph MUST be recoverable from durable state and events.

---

## 12.0 Task Scheduling

Task scheduling determines which tasks may run and when. Scheduling MUST respect dependencies, readiness, governance, resource limits, locks, priorities, worker availability, and runtime configuration.

### 12.1 Scheduling Eligibility

A task is eligible for scheduling only when:

* task state is pending
* all required dependencies are satisfied
* required source artifacts or inputs exist
* required governance gates are satisfied or not required
* required agent role or tool capability is available
* required runtime resources are available
* required locks can be acquired
* task has not exceeded retry limits
* execution state permits dispatch

### 12.2 Scheduling Policy Inputs

The scheduler SHOULD consider:

* task priority
* critical path membership
* dependency fan-out
* resource requirements
* worker availability
* agent/model availability
* tool availability
* governance constraints
* artifact lock conflicts
* retry count
* age in queue
* risk tier
* parallel group membership

### 12.3 Scheduling Record Schema

| Field | Type | Required | Description |
|---|---|---|
| scheduling-record-id | string | REQUIRED | Unique scheduling decision identifier |
| execution-graph-id | string | REQUIRED | Execution graph |
| task-id | string | REQUIRED | Task evaluated |
| eligibility-outcome | enum | REQUIRED | eligible, blocked, delayed, skipped |
| blocking-reasons | array | CONDITIONAL | Reasons task is not eligible |
| selected-queue-id | string | CONDITIONAL | Queue selected when eligible |
| priority-score | number | CONDITIONAL | Scheduler priority score |
| evaluated-at | ISO 8601 | REQUIRED | Evaluation time |
| evaluated-by | string | REQUIRED | Scheduler component |

### 12.4 Scheduling Rules

The scheduler MUST NOT dispatch a task with unsatisfied required dependencies.

The scheduler MUST NOT dispatch a task blocked by governance.

The scheduler MUST NOT dispatch a task requiring a lock that cannot be acquired.

The scheduler MAY delay eligible tasks to optimize resource usage or preserve critical path scheduling.

---

## 13.0 Execution Queues

Execution queues hold work items awaiting dispatch. Queues may be implemented in memory, durable message brokers, relational tables, distributed queues, or other mechanisms, provided required semantics are preserved.

### 13.1 Queue Types

| Queue Type  | Purpose                                                         |
| ----------- | --------------------------------------------------------------- |
| ready       | Work items eligible for execution                               |
| delayed     | Work items delayed by time, resource, or retry policy           |
| blocked     | Work items blocked by dependency, governance, lock, or resource |
| retry       | Work items awaiting retry                                       |
| repair      | Repair work items                                               |
| validation  | Validation and deterministic tool work                          |
| governance  | Governance check or approval work                               |
| dead-letter | Work items that cannot proceed automatically                    |

### 13.2 Work Item Schema

| Field | Type | Required | Description |
|---|---|---|
| work-item-id | string | REQUIRED | Unique work item identifier |
| task-id | string | REQUIRED | Source construction task |
| execution-graph-id | string | REQUIRED | Execution graph |
| work-item-type | enum | REQUIRED | agent, tool, validation, repair, governance, repository, platform |
| queue-id | string | REQUIRED | Current queue |
| priority | enum | REQUIRED | critical, high, medium, low |
| required-capabilities | array | CONDITIONAL | Agent roles or tool capabilities required |
| required-locks | array | CONDITIONAL | Runtime locks required |
| retry-count | integer | REQUIRED | Number of retries attempted |
| max-retries | integer | REQUIRED | Maximum retries permitted |
| lease-id | string | CONDITIONAL | Active lease when claimed |
| available-at | ISO 8601 | REQUIRED | Time work item may be executed |
| created-at | ISO 8601 | REQUIRED | Creation time |
| updated-at | ISO 8601 | REQUIRED | Last update time |

### 13.3 Queue Rules

Queues used for distributed execution MUST be durable or recoverable.

A work item MUST exist in exactly one queue at a time.

A work item MUST NOT be dispatched unless it is in ready, validation, repair, governance, or retry queue and available-at has passed.

A work item that exceeds retry limits MUST be moved to dead-letter or escalated according to policy.

---

## 14.0 Runtime Worker Model

Runtime workers execute work items assigned by the scheduler. Workers may be local threads, background jobs, containers, distributed nodes, or services.

### 14.1 Worker Types

| Worker Type       | Purpose                                                            |
| ----------------- | ------------------------------------------------------------------ |
| agent-worker      | Invokes agents through Agent Orchestrator                          |
| tool-worker       | Invokes deterministic tools through Tool Integration Layer         |
| validation-worker | Runs validation work and records results                           |
| repair-worker     | Coordinates repair workflows                                       |
| repository-worker | Performs repository operations                                     |
| governance-worker | Executes governance checks and waits for approvals                 |
| platform-worker   | Performs runtime lifecycle actions such as checkpoints and cleanup |

### 14.2 Worker Record Schema

| Field | Type | Required | Description |
|---|---|---|
| worker-id | string | REQUIRED | Unique worker identifier |
| worker-type | enum | REQUIRED | Worker type |
| runtime-instance-id | string | REQUIRED | Runtime instance owning worker |
| capabilities | array | REQUIRED | Agent roles, tool capabilities, or platform operations supported |
| status | enum | REQUIRED | idle, leased, running, draining, failed, disabled |
| current-work-item-id | string | CONDITIONAL | Work item currently assigned |
| resource-profile | object | CONDITIONAL | CPU, memory, network, sandbox, or model capacity |
| heartbeat-at | ISO 8601 | REQUIRED | Last worker heartbeat |
| started-at | ISO 8601 | REQUIRED | Worker start time |

### 14.3 Worker Rules

A worker MUST declare capabilities before receiving work.

A worker MUST send heartbeats while executing leased work.

A worker that misses heartbeat thresholds MUST be treated as suspect.

A failed worker MUST release or expire active leases through recovery logic.

A worker MUST NOT execute work outside its declared capabilities.

---

## 15.0 Task Leases

Task leases prevent duplicate execution of the same work item in parallel or distributed environments.

### 15.1 Lease Schema

| Field | Type | Required | Description |
|---|---|---|
| lease-id | string | REQUIRED | Unique lease identifier |
| work-item-id | string | REQUIRED | Work item leased |
| task-id | string | REQUIRED | Source task |
| worker-id | string | REQUIRED | Worker holding lease |
| lease-state | enum | REQUIRED | active, renewed, released, expired, revoked |
| acquired-at | ISO 8601 | REQUIRED | Acquisition time |
| expires-at | ISO 8601 | REQUIRED | Expiration time |
| renewed-at | ISO 8601 | CONDITIONAL | Last renewal time |
| release-reason | string | CONDITIONAL | Reason lease was released, expired, or revoked |

### 15.2 Lease Rules

A work item MUST NOT have more than one active lease.

A worker MUST renew long-running leases before expiration.

An expired lease MAY be reclaimed by another worker only after recovery checks confirm that no unsafe side effects remain.

Lease expiration MUST NOT automatically mark a task failed; it MUST trigger recovery evaluation.

---

## 16.0 Runtime Locks

Runtime locks protect shared resources from unsafe concurrent modification.

### 16.1 Lock Types

| Lock Type       | Protected Resource                               |
| --------------- | ------------------------------------------------ |
| artifact-lock   | Artifact content or metadata                     |
| path-lock       | Repository path                                  |
| branch-lock     | Repository branch                                |
| package-lock    | Package manifest or bundle                       |
| state-lock      | State record or transaction                      |
| governance-lock | Approval, waiver, or override target             |
| tool-lock       | Exclusive tool runtime or environment            |
| model-lock      | Restricted model capacity or provider constraint |

### 16.2 Lock Record Schema

| Field | Type | Required | Description |
|---|---|---|
| lock-id | string | REQUIRED | Unique lock identifier |
| lock-type | enum | REQUIRED | Lock type |
| resource-id | string | REQUIRED | Resource protected |
| owner-id | string | REQUIRED | Worker, task, or runtime instance holding lock |
| scope | enum | REQUIRED | read, write, exclusive |
| acquired-at | ISO 8601 | REQUIRED | Acquisition time |
| expires-at | ISO 8601 | CONDITIONAL | Expiration time |
| status | enum | REQUIRED | active, released, expired, revoked |
| release-reason | string | CONDITIONAL | Reason lock was released |

### 16.3 Lock Rules

A task that modifies an artifact MUST acquire the required artifact or path lock.

A merge into stable repository state MUST acquire branch lock.

A package creation operation MUST acquire package lock.

Locks MUST be released after successful completion, failure handling, or recovery.

A stale lock MUST trigger recovery before reuse of the protected resource.

---

## 17.0 Task Execution Lifecycle

Each task follows a controlled lifecycle within the runtime.

### 17.1 Task Runtime Stages

| Stage                 | Purpose                                                            |
| --------------------- | ------------------------------------------------------------------ |
| initialize            | Load task definition and runtime context                           |
| governance-check      | Consult governance where required                                  |
| acquire-resources     | Acquire worker, locks, tools, models, and repository access        |
| assemble-context      | Assemble agent, tool, artifact, and validation context             |
| execute               | Perform agent, tool, repository, validation, or platform action    |
| capture-output        | Persist outputs, findings, artifacts, or records                   |
| validate-output       | Validate agent output, tool result, artifact, or repository change |
| update-state          | Commit state transitions and events                                |
| update-traceability   | Record runtime traceability                                        |
| determine-next-action | Proceed, repair, retry, escalate, halt, or complete                |
| release-resources     | Release leases, locks, and transient resources                     |

### 17.2 Task Execution Record Schema

| Field | Type | Required | Description |
|---|---|---|
| task-execution-record-id | string | REQUIRED | Unique execution record |
| task-id | string | REQUIRED | Task executed |
| work-item-id | string | REQUIRED | Runtime work item |
| execution-graph-id | string | REQUIRED | Execution graph |
| worker-id | string | REQUIRED | Worker executing task |
| lifecycle-stage-results | array | REQUIRED | Results of lifecycle stages |
| started-at | ISO 8601 | REQUIRED | Task execution start |
| completed-at | ISO 8601 | CONDITIONAL | Task execution end |
| outcome | enum | REQUIRED | completed, failed, retry, repaired, escalated, halted, cancelled |
| produced-artifacts | array | CONDITIONAL | Artifacts produced |
| validation-results | array | CONDITIONAL | Validation results |
| error-records | array | CONDITIONAL | Runtime errors |
| next-action | enum | REQUIRED | schedule-successors, repair, retry, escalate, halt, complete |

### 17.3 Lifecycle Rules

A task MUST NOT enter execute stage until required governance checks and resource acquisition complete.

A task that produces artifacts MUST route those artifacts through Artifact Repository admission.

A task that requires validation MUST NOT be marked completed until validation passes or is waived.

A task that fails validation MUST trigger repair, retry, escalation, or failure according to policy.

---

## 18.0 Agent Invocation During Runtime

The runtime invokes agents through the Agent Orchestrator according to ISL v2.1.

### 18.1 Agent Runtime Rules

The runtime MUST:

* provide task-scoped context package requirements
* request agent invocation through Agent Orchestrator
* preserve agent invocation records
* validate agent outputs before downstream use
* route artifact candidates to the Artifact Repository
* route review, security, or repair findings to appropriate tasks
* apply retry limits for agent invocation failures
* escalate role violations or repeated failures

### 18.2 Agent Invocation Runtime Outcomes

| Outcome                  | Runtime Action                                                         |
| ------------------------ | ---------------------------------------------------------------------- |
| completed-valid-output   | Continue to artifact admission, review, validation, or downstream task |
| completed-partial-output | Accept only if task contract permits partial output                    |
| output-invalid           | Retry, request revision, or escalate                                   |
| timeout                  | Retry according to policy, then escalate                               |
| role-violation           | Reject and escalate if material                                        |
| governance-blocked       | Pause or halt affected task                                            |
| model-failure            | Retry, fallback, or escalate according to model policy                 |

---

## 19.0 Tool Invocation During Runtime

The runtime invokes deterministic tools through the Tool Integration Layer according to ISL v1.6.

### 19.1 Tool Runtime Rules

The runtime MUST:

* select tools only through declared capabilities
* invoke only registered and permitted tools
* enforce tool trust profiles
* pass structured invocation requests
* record invocation results
* normalize tool outputs before runtime decision-making
* treat timeout and error as non-success outcomes
* map tool findings to artifacts, validations, policies, and tasks
* trigger repair or escalation when required

### 19.2 Tool Invocation Runtime Outcomes

| Outcome        | Runtime Action                                                          |
| -------------- | ----------------------------------------------------------------------- |
| passed         | Mark validation satisfied and proceed                                   |
| failed         | Trigger repair, retry, fail, or escalate                                |
| warning        | Proceed only if warning is acceptable under policy                      |
| timeout        | Retry once unless configuration is stricter                             |
| error          | Retry, substitute tool, escalate, or fail                               |
| not-applicable | Record justification and confirm validation coverage remains sufficient |

---

## 20.0 Artifact Runtime Handling

The runtime interacts with artifacts only through the Artifact Repository model.

### 20.1 Artifact Runtime Rules

The runtime MUST:

* route generated artifact candidates through repository admission
* assign or obtain artifact identifiers
* record artifact metadata
* preserve source task and source entity links
* update artifact state after validation
* prevent failed artifacts from stable promotion
* acquire locks before artifact modification
* preserve supersession history during repair
* trigger repository integrity checks at promotion and consolidation

### 20.2 Artifact Runtime State Actions

| Runtime Event           | Artifact Action                            |
| ----------------------- | ------------------------------------------ |
| artifact generated      | admit as candidate or pending              |
| validation passed       | update validation-status to passed         |
| validation failed       | update state to failed                     |
| repair started          | update or create repaired artifact version |
| repair validated        | update state to valid or failed            |
| promotion allowed       | update state to stable                     |
| supersession created    | link prior and replacement artifact        |
| merge conflict detected | block promotion and create conflict record |

---

## 21.0 Validation Feedback Loop

Validation feedback loops ensure that artifacts converge toward correctness under deterministic validation.

### 21.1 Feedback Loop Steps

The runtime MUST perform the following when validation is required:

1. Identify validation task or criterion.
2. Select appropriate tool or validation method.
3. Invoke validation.
4. Normalize result.
5. Record validation result.
6. Update artifact and task state.
7. Determine whether to proceed, repair, retry, escalate, or halt.
8. Record traceability and telemetry.

### 21.2 Feedback Loop Rules

Validation failures MUST NOT be ignored.

Validation results MUST reference the artifact version evaluated.

A failed validation MUST prevent artifact stable promotion unless waived.

A validation result that triggers repair MUST link to the repair record.

---

## 22.0 Repair Runtime Management

Repair cycles are controlled runtime subflows triggered by validation, test, policy, or repository failure.

### 22.1 Repair Admission Conditions

The runtime MAY initiate repair only when:

* failure is repairable
* repair is permitted by governance
* repair limits have not been reached
* failed artifact and validation result are available
* repair task can be generated or already exists
* required agent role is available
* required locks can be acquired

### 22.2 Runtime Repair Limits

The runtime MUST enforce:

| Parameter                                             | Default                                 |
| ----------------------------------------------------- | --------------------------------------- |
| Maximum repair iterations per artifact                | 5                                       |
| Maximum total repair iterations per construction plan | 50                                      |
| Escalation threshold for same artifact                | 3 consecutive failures                  |
| Retry after tool timeout                              | 1 retry unless stricter configuration   |
| Retry after agent timeout                             | 2 retries unless stricter configuration |

### 22.3 Repair Runtime Rules

A repair MUST produce or reference a Repair Record.

A repaired artifact MUST be revalidated.

A repair MUST NOT mark an artifact valid without validation or waiver.

A repair that reaches iteration limit MUST escalate.

A repair that introduces a governance violation MUST pause and route to Governance Engine.

---

## 23.0 Retry and Timeout Model

Retries and timeouts prevent transient failures from becoming immediate terminal failures while also preventing indefinite execution.

### 23.1 Timeout Categories

| Timeout Type       | Applies To                      |
| ------------------ | ------------------------------- |
| task-timeout       | Full task execution             |
| agent-timeout      | Agent invocation                |
| model-timeout      | Model provider invocation       |
| tool-timeout       | Tool invocation                 |
| validation-timeout | Validation work item            |
| repository-timeout | Repository operation            |
| governance-timeout | Approval or governance response |
| lease-timeout      | Work item lease                 |
| lock-timeout       | Lock acquisition or hold period |

### 23.2 Retry Policy Schema

| Field | Type | Required | Description |
|---|---|---|
| retry-policy-id | string | REQUIRED | Unique retry policy identifier |
| applies-to | enum | REQUIRED | task, agent, model, tool, validation, repository, governance |
| max-attempts | integer | REQUIRED | Maximum attempts |
| backoff-strategy | enum | REQUIRED | none, fixed, exponential, jittered |
| initial-delay-ms | integer | REQUIRED | Initial retry delay |
| max-delay-ms | integer | CONDITIONAL | Maximum retry delay |
| retryable-error-classes | array | REQUIRED | Error classes eligible for retry |
| non-retryable-error-classes | array | REQUIRED | Error classes not eligible for retry |
| escalation-after-exhaustion | boolean | REQUIRED | Whether escalation occurs after retries fail |

### 23.3 Retry Rules

Retries MUST be bounded.

A retry MUST create or update retry count.

A retry MUST NOT bypass governance or validation.

Non-retryable errors MUST NOT be retried.

Retry exhaustion MUST result in escalation, failure, dead-letter, or halt according to policy.

---

## 24.0 Parallel Execution

The runtime MAY execute tasks in parallel when dependency, lock, governance, resource, and traceability requirements permit.

### 24.1 Parallel Execution Preconditions

Tasks MAY run in parallel only when:

* no dependency path exists between them
* they do not require conflicting locks
* they do not modify the same artifact or path without isolation
* they do not require exclusive governance review
* required workers and resources are available
* traceability can be recorded independently
* runtime configuration permits concurrency

### 24.2 Parallel Execution Rules

The runtime MUST respect parallel groups produced by the Planning Engine.

The runtime MAY reduce concurrency below the planned parallel group.

The runtime MUST NOT increase concurrency in a way that violates dependencies or governance constraints.

Parallel task failure MUST pause dependent tasks but SHOULD NOT halt unrelated tasks unless policy requires halt.

---

## 25.0 Distributed Execution

Distributed execution allows runtime work to execute across multiple nodes or workers.

### 25.1 Distributed Execution Requirements

A distributed runtime MUST provide:

* durable shared state or replicated state consistency
* durable queues or recoverable queues
* lease-based task claiming
* heartbeat monitoring
* lock coordination
* artifact repository consistency
* traceability event ordering
* governance decision consistency
* recovery after worker failure

### 25.2 Distributed Execution Rules

Distributed workers MUST NOT rely on local-only state for lifecycle-critical decisions.

Distributed workers MUST renew leases and locks.

State updates affecting task completion, artifact status, validation, repair, or governance MUST be committed through the State and Memory Model.

Split-brain execution MUST be detected and halted or resolved before continuing.

---

## 26.0 Runtime Governance Enforcement

Runtime governance enforcement integrates ISL v1.7 into active execution.

### 26.1 Governance Enforcement Points

The runtime MUST consult governance before:

* execution start
* governed task dispatch
* restricted model use
* restricted tool use
* high-risk artifact generation
* repair of high-risk artifacts
* waiver application
* override application
* artifact promotion when governed
* merge into stable branch when governed
* package creation when governed
* deployment preparation completion

### 26.2 Governance Runtime Actions

| Governance Decision | Runtime Action                            |
| ------------------- | ----------------------------------------- |
| allow               | Continue                                  |
| warn                | Continue and record warning               |
| block               | Stop affected action                      |
| escalate            | Pause affected action and open escalation |
| approval-required   | Open or wait for approval gate            |
| waiver-required     | Pause until valid waiver exists           |
| override-required   | Pause until valid override exists         |

### 26.3 Governance Runtime Rules

The runtime MUST obey governance decisions.

The runtime MUST record governance check responses.

The runtime MUST NOT continue a governed action using an expired approval, waiver, or override.

Governance audit write failure MUST halt governed action.

---

## 27.0 Runtime Checkpoints

Runtime checkpoints provide recovery-safe markers.

### 27.1 Required Checkpoint Triggers

The runtime MUST create checkpoints:

* after execution admission
* before execution start
* before artifact consolidation
* after artifact consolidation
* before deployment preparation
* before high-risk repair
* after high-risk repair
* before governance override application
* after plan adaptation
* before controlled shutdown
* at execution completion

### 27.2 Runtime Checkpoint Schema

| Field | Type | Required | Description |
|---|---|---|
| checkpoint-id | string | REQUIRED | Unique checkpoint identifier |
| execution-graph-id | string | REQUIRED | Execution graph |
| plan-id | string | REQUIRED | Construction plan |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| checkpoint-type | enum | REQUIRED | admission, start, phase, repair, consolidation, deployment, governance, shutdown, completion |
| included-state-categories | array | REQUIRED | State categories captured |
| queue-state-reference | string | REQUIRED | Queue state reference |
| lease-state-reference | string | CONDITIONAL | Lease state reference |
| lock-state-reference | string | CONDITIONAL | Lock state reference |
| artifact-state-reference | string | REQUIRED | Artifact state reference |
| traceability-snapshot-id | string | CONDITIONAL | Traceability snapshot |
| created-at | ISO 8601 | REQUIRED | Creation time |
| recovery-eligible | boolean | REQUIRED | Whether checkpoint may be used for recovery |

### 27.3 Checkpoint Rules

A checkpoint MUST be durable.

A recovery-eligible checkpoint MUST include enough state to resume or safely halt.

A checkpoint MUST NOT claim recovery eligibility if required state categories are missing.

---

## 28.0 Runtime Recovery

Runtime recovery restores execution after interruption, crash, worker failure, lease expiration, lock expiration, or state inconsistency.

### 28.1 Recovery Triggers

The runtime MUST initiate recovery when:

* runtime process restarts during active execution
* worker heartbeat expires while lease is active
* lease expires during in-progress work
* lock expires during protected operation
* state transaction fails
* repository state differs from artifact state
* traceability recording fails
* execution graph integrity check fails
* distributed split-brain is detected
* checkpoint validation fails

### 28.2 Recovery Process

Runtime recovery MUST:

1. Enter recovering state.
2. Stop new dispatches.
3. Identify active leases and locks.
4. Validate worker heartbeats.
5. Load latest recovery-eligible checkpoint.
6. Inspect state events after checkpoint.
7. Validate task, artifact, validation, repair, traceability, governance, and repository state.
8. Requeue safe work items.
9. Escalate unsafe or ambiguous work.
10. Release stale locks when safe.
11. Record recovery event.
12. Resume, halt, or escalate.

### 28.3 Recovery Rules

The runtime MUST NOT resume dispatch until recovery validation completes.

In-flight work with uncertain side effects MUST be escalated or isolated before retry.

Artifacts produced by interrupted tasks MUST remain pending, failed, or quarantined until validated.

Recovery MUST preserve traceability and audit history.

---

## 29.0 Runtime Completion

Runtime completion occurs only when all required execution obligations are satisfied.

### 29.1 Completion Conditions

Execution MAY be marked completed only when:

* all required tasks are completed, skipped, or waived
* no required work item remains ready, delayed, retry, repair, validation, or blocked
* all required artifacts are stable, waived, or governed as non-deployable
* required validations passed or were waived
* repair cycles are resolved
* no blocking governance gate is pending
* traceability integrity checks passed
* repository integrity checks passed
* deployment preparation completed where required
* completion report is generated
* final checkpoint exists

### 29.2 Completion Record Schema

| Field | Type | Required | Description |
|---|---|---|
| runtime-completion-id | string | REQUIRED | Unique runtime completion identifier |
| execution-graph-id | string | REQUIRED | Execution graph completed |
| plan-id | string | REQUIRED | Construction plan |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| final-state | enum | REQUIRED | completed, failed, halted, cancelled, escalated |
| tasks-completed | array | REQUIRED | Completed task IDs |
| tasks-failed | array | CONDITIONAL | Failed task IDs |
| artifacts-stable | array | CONDITIONAL | Stable artifact IDs |
| validations-passed | array | CONDITIONAL | Passed validation IDs |
| waivers-used | array | CONDITIONAL | Waiver IDs used |
| errors-recorded | array | CONDITIONAL | Runtime error IDs |
| final-checkpoint-id | string | REQUIRED | Final checkpoint |
| completed-at | ISO 8601 | REQUIRED | Completion time |

### 29.3 Completion Rules

The runtime MUST NOT mark execution completed while any blocking error remains unresolved.

The runtime MUST NOT mark execution completed if required traceability or repository integrity fails.

A failed, halted, cancelled, or escalated final state MUST include required-action or resolution guidance.

---

## 30.0 Runtime Configuration

Runtime configuration controls scheduling, workers, retries, timeouts, isolation, governance checks, and telemetry.

### 30.1 Runtime Configuration Schema

| Field | Type | Required | Description |
|---|---|---|
| runtime-configuration-id | string | REQUIRED | Unique configuration identifier |
| configuration-version | semver | REQUIRED | Configuration version |
| scheduling-policy | string | REQUIRED | Scheduling policy identifier |
| max-parallel-tasks | integer | REQUIRED | Maximum concurrent tasks |
| max-workers | integer | REQUIRED | Maximum workers |
| default-task-timeout-seconds | integer | REQUIRED | Default task timeout |
| default-agent-timeout-seconds | integer | REQUIRED | Default agent timeout |
| default-tool-timeout-seconds | integer | REQUIRED | Default tool timeout |
| retry-policies | array | REQUIRED | Retry policies |
| repair-limits | object | REQUIRED | Repair limits |
| lease-duration-seconds | integer | REQUIRED | Default lease duration |
| heartbeat-interval-seconds | integer | REQUIRED | Worker heartbeat interval |
| lock-timeout-seconds | integer | REQUIRED | Default lock timeout |
| isolation-profile | string | REQUIRED | Runtime isolation profile |
| governance-enforcement-level | enum | REQUIRED | strict, standard, advisory |
| telemetry-profile | string | REQUIRED | Telemetry profile |
| configured-at | ISO 8601 | REQUIRED | Configuration time |
| configured-by | string | REQUIRED | Authority or subsystem setting configuration |

### 30.2 Configuration Rules

Runtime configuration MUST be versioned.

Runtime configuration active at execution start MUST be recorded.

Configuration changes during active execution MUST trigger impact evaluation.

Governance-enforcement-level advisory MUST NOT be used for Autonomous-Ready execution unless governance explicitly permits it for the risk tier.

---

## 31.0 Runtime Isolation and Resource Control

Runtime isolation protects platform state, artifacts, tools, model calls, and worker execution.

### 31.1 Isolation Requirements

The runtime MUST support:

* task workspace isolation
* artifact write controls
* tool sandboxing through Tool Integration Layer
* model invocation controls through model abstraction
* repository branch or path isolation
* secret access restrictions
* network access restrictions
* resource limits for worker execution
* cleanup of transient workspaces

### 31.2 Resource Control Rules

A worker MUST NOT exceed configured resource limits without authorization.

A task requiring generated-code execution SHOULD run in isolated environment.

A tool requiring network access MUST operate under approved network policy.

Secrets MUST NOT be injected into task context unless explicitly required and governed.

---

## 32.0 Runtime Error Taxonomy

This section addresses ISL-007 by defining a normative runtime error taxonomy. Runtime errors classify failure conditions and define default handling paths.

### 32.1 Runtime Error Record Schema

| Field | Type | Required | Description |
|---|---|---|
| runtime-error-id | string | REQUIRED | Unique runtime error identifier |
| error-class | enum | REQUIRED | Runtime error class |
| error-category | enum | REQUIRED | admission, scheduling, worker, task, agent, model, tool, validation, repair, artifact, repository, governance, traceability, state, concurrency, distributed, security, telemetry, recovery |
| severity | enum | REQUIRED | blocking, high, medium, low |
| execution-graph-id | string | CONDITIONAL | Related execution graph |
| task-id | string | CONDITIONAL | Related task |
| work-item-id | string | CONDITIONAL | Related work item |
| artifact-id | string | CONDITIONAL | Related artifact |
| worker-id | string | CONDITIONAL | Related worker |
| invocation-id | string | CONDITIONAL | Related agent, model, or tool invocation |
| governance-record-id | string | CONDITIONAL | Related governance record |
| message | string | REQUIRED | Human-readable explanation |
| detected-at | ISO 8601 | REQUIRED | Detection time |
| detected-by | string | REQUIRED | Runtime component detecting error |
| default-handling | enum | REQUIRED | reject, retry, repair, requeue, escalate, halt, fail-task, fail-execution, quarantine, recover, warn |
| handling-status | enum | REQUIRED | pending, applied, failed, escalated, waived |
| required-action | string | REQUIRED | Remediation required |

### 32.2 Error Categories

| Category     | Meaning                                                        |
| ------------ | -------------------------------------------------------------- |
| admission    | Execution admission failure                                    |
| scheduling   | Scheduler or eligibility failure                               |
| worker       | Worker failure or heartbeat failure                            |
| task         | Task lifecycle failure                                         |
| agent        | Agent invocation or output failure                             |
| model        | Model provider or model invocation failure                     |
| tool         | Tool invocation or tool result failure                         |
| validation   | Validation failure or invalid validation state                 |
| repair       | Repair failure or convergence failure                          |
| artifact     | Artifact production or state failure                           |
| repository   | Repository operation, branch, merge, or package failure        |
| governance   | Governance check, approval, waiver, override, or audit failure |
| traceability | Traceability recording or integrity failure                    |
| state        | State transition, transaction, or consistency failure          |
| concurrency  | Lease, lock, race, or conflict failure                         |
| distributed  | Distributed runtime consistency or node failure                |
| security     | Runtime isolation, secret, sandbox, or access violation        |
| telemetry    | Observability or telemetry failure                             |
| recovery     | Recovery failure or unsafe recovery state                      |

### 32.3 Runtime Error Classes

| Error Class                               | Category     | Default Handling |
| ----------------------------------------- | ------------ | ---------------- |
| runtime-admission-readiness-failed        | admission    | reject           |
| runtime-admission-plan-not-executable     | admission    | reject           |
| runtime-admission-governance-blocked      | admission    | escalate         |
| runtime-input-version-mismatch            | admission    | reject           |
| runtime-scheduler-dependency-unsatisfied  | scheduling   | requeue          |
| runtime-scheduler-governance-blocked      | scheduling   | escalate         |
| runtime-scheduler-resource-unavailable    | scheduling   | requeue          |
| runtime-scheduler-lock-unavailable        | scheduling   | requeue          |
| runtime-work-item-invalid                 | scheduling   | fail-task        |
| runtime-worker-heartbeat-expired          | worker       | recover          |
| runtime-worker-capability-missing         | worker       | requeue          |
| runtime-worker-failed                     | worker       | recover          |
| runtime-lease-expired                     | concurrency  | recover          |
| runtime-lease-conflict                    | concurrency  | halt             |
| runtime-lock-conflict                     | concurrency  | requeue          |
| runtime-lock-stale                        | concurrency  | recover          |
| runtime-task-timeout                      | task         | retry            |
| runtime-task-output-missing               | task         | retry            |
| runtime-task-invalid-transition           | task         | halt             |
| runtime-agent-timeout                     | agent        | retry            |
| runtime-agent-output-invalid              | agent        | retry            |
| runtime-agent-role-violation              | agent        | escalate         |
| runtime-model-provider-unavailable        | model        | retry            |
| runtime-model-output-normalization-failed | model        | retry            |
| runtime-tool-timeout                      | tool         | retry            |
| runtime-tool-error                        | tool         | retry            |
| runtime-tool-result-conflict              | tool         | escalate         |
| runtime-validation-failed                 | validation   | repair           |
| runtime-validation-timeout                | validation   | retry            |
| runtime-validation-evidence-missing       | validation   | escalate         |
| runtime-repair-limit-reached              | repair       | escalate         |
| runtime-repair-nonconvergent              | repair       | escalate         |
| runtime-artifact-admission-failed         | artifact     | fail-task        |
| runtime-artifact-validation-failed        | artifact     | repair           |
| runtime-artifact-untraced                 | artifact     | escalate         |
| runtime-repository-unavailable            | repository   | retry            |
| runtime-repository-merge-conflict         | repository   | escalate         |
| runtime-repository-integrity-failed       | repository   | recover          |
| runtime-governance-check-failed           | governance   | halt             |
| runtime-governance-approval-missing       | governance   | escalate         |
| runtime-governance-audit-write-failed     | governance   | halt             |
| runtime-traceability-write-failed         | traceability | halt             |
| runtime-traceability-integrity-failed     | traceability | escalate         |
| runtime-state-transaction-failed          | state        | recover          |
| runtime-state-consistency-failed          | state        | recover          |
| runtime-distributed-split-brain           | distributed  | halt             |
| runtime-security-boundary-violation       | security     | halt             |
| runtime-secret-exposure-detected          | security     | halt             |
| runtime-telemetry-write-failed            | telemetry    | warn             |
| runtime-recovery-unsafe                   | recovery     | halt             |
| runtime-recovery-failed                   | recovery     | halt             |

### 32.4 Error Handling Rules

A blocking error MUST prevent the affected lifecycle action.

A security boundary violation MUST halt or isolate affected execution.

A governance audit write failure MUST halt governed actions.

A traceability write failure MUST halt artifact acceptance and execution completion.

A retryable error MUST respect retry policy.

A repairable error MUST respect repair limits.

A nonconvergent repair error MUST escalate.

A recovery-unsafe error MUST halt affected execution.

---

## 33.0 Dead-Letter and Quarantine Handling

Some runtime work or artifacts cannot proceed automatically.

### 33.1 Dead-Letter Conditions

A work item MUST be moved to dead-letter or escalated when:

* retry limit is exceeded
* required capability remains unavailable
* governance blocks continuation
* malformed work item cannot be corrected
* state consistency cannot be established
* repeated worker failure occurs

### 33.2 Quarantine Conditions

Artifacts or outputs MUST be quarantined when:

* provenance is unclear
* traceability is missing
* security finding indicates unsafe content
* repository integrity is uncertain
* interrupted task produced uncertain side effects
* artifact was created outside approved repository flow

### 33.3 Quarantine Rules

Quarantined artifacts MUST NOT be promoted.

Quarantined artifacts MUST be isolated from stable repository state.

Quarantine MUST produce a runtime error and repository finding.

Governance or recovery action is required before release from quarantine.

---

## 34.0 Runtime Telemetry

Runtime telemetry supports monitoring, debugging, audit support, and operational improvement.

### 34.1 Required Runtime Telemetry Events

The runtime SHOULD emit telemetry for:

* execution admission started
* execution admission completed
* runtime lifecycle state changed
* task scheduled
* work item queued
* work item leased
* worker heartbeat
* task started
* task completed
* task failed
* agent invocation started
* agent invocation completed
* tool invocation started
* tool invocation completed
* validation failed
* repair started
* repair completed
* governance check requested
* governance check decided
* lock acquired
* lock released
* lease expired
* checkpoint created
* recovery started
* recovery completed
* runtime error recorded
* execution completed

### 34.2 Telemetry Rules

Telemetry MUST include correlation identifiers where applicable.

Telemetry MUST NOT expose restricted artifact or context content to unauthorized observers.

Telemetry failure MUST NOT hide runtime errors.

Critical telemetry failure SHOULD produce operational alerting.

---

## 35.0 Runtime Security Requirements

This section defines minimum runtime security requirements.

### 35.1 Runtime Security Rules

The runtime MUST:

* enforce worker identity
* enforce repository access controls
* enforce governance access controls
* isolate task workspaces
* control secret access
* prevent direct agent writes to stable artifacts
* prevent unregistered tool execution
* prevent model invocation outside approved abstraction layer
* detect sandbox violations
* record security-relevant runtime errors
* halt or isolate unsafe execution paths

### 35.2 Runtime Secret Handling

Secrets MUST NOT be included in agent context packages unless explicitly required and governed.

Secrets used by tools MUST be scoped to the task and environment.

Secret exposure findings MUST be treated as security errors.

---

## 36.0 Runtime Audit Requirements

Runtime audit records provide evidence for execution decisions and outcomes.

### 36.1 Required Audit-Relevant Runtime Records

The runtime MUST preserve:

* execution admission records
* execution graph records
* scheduling records
* task execution records
* worker and lease records
* lock records for governed operations
* agent invocation records
* tool invocation records
* validation records
* repair records
* artifact state transitions
* governance check records
* runtime error records
* recovery records
* completion records

### 36.2 Audit Rules

Audit-relevant records MUST include specification-id and specification-version where applicable.

Audit-relevant records MUST be retained according to governance and state retention rules.

Runtime records used for governance decisions MUST be immutable or correction-only.

---

## 37.0 Runtime Conformance Requirements

### 37.1 Runtime Instance Conformance

A runtime instance conforms to ISL v2.4 if it can:

* accept and validate execution handoff records
* enforce execution admission checks
* initialize execution state
* maintain execution graph state
* schedule tasks based on dependencies and governance
* manage queues, leases, locks, workers, and retries
* invoke agents through the Agent Orchestrator
* invoke tools through the Tool Integration Layer
* route artifacts through the Artifact Repository
* enforce validation before artifact acceptance
* enforce bounded repair cycles
* create checkpoints
* recover after interruption
* emit runtime telemetry
* produce runtime error records
* produce completion records

### 37.2 Scheduler Conformance

A scheduler conforms to ISL v2.4 if it can:

* evaluate task eligibility
* respect task dependencies
* respect governance constraints
* respect resource constraints
* respect locks and leases
* place work items into appropriate queues
* prevent duplicate dispatch
* support retry and delayed scheduling
* support parallel execution safely
* record scheduling decisions

### 37.3 Worker Conformance

A worker conforms to ISL v2.4 if it can:

* declare capabilities
* claim work through leases
* send heartbeats
* execute only permitted work
* update task execution records
* release leases and locks
* report failures
* preserve output and error records
* operate under runtime isolation constraints

### 37.4 Distributed Runtime Conformance

A distributed runtime conforms to ISL v2.4 if it can:

* coordinate workers across nodes
* maintain durable queues or equivalent recoverable queues
* manage leases and locks across nodes
* detect worker failure
* detect split-brain conditions
* preserve artifact repository consistency
* preserve governance decision consistency
* recover safely after node failure

### 37.5 Platform Runtime Conformance

A platform conforms to ISL v2.4 if it:

* prevents execution before Autonomous-Ready readiness
* prevents execution of invalid plans
* integrates runtime with state, traceability, governance, agents, tools, and artifacts
* enforces runtime error handling paths
* preserves runtime auditability
* supports runtime recovery
* prevents unvalidated or untraced artifacts from becoming stable
* supports completion only when evidence conditions are satisfied

---

## 38.0 Summary

ISL v2.4 defines the Execution Runtime Model for the IMHOTEP platform. It specifies how construction plans become controlled runtime activity through admission checks, scheduling, queues, workers, leases, locks, task execution, agent invocation, tool invocation, artifact handling, validation feedback, repair cycles, governance enforcement, checkpoints, recovery, telemetry, and completion.

This revision strengthens the original runtime model by making runtime behavior operationally explicit. It defines task scheduling, worker execution, queue semantics, concurrency controls, distributed execution, runtime recovery, retry and timeout rules, runtime configuration, security requirements, and a normative error taxonomy addressing ISL-007.

A conforming IMHOTEP runtime MUST behave as a governed, traceable, recoverable, validation-centered execution engine. It MUST not merely coordinate autonomous activity; it MUST control it, record it, validate it, recover it, and stop it when the evidence, state, or governance conditions no longer permit continuation.
