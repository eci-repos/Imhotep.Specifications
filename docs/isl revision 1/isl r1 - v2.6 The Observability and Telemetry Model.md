# IMHOTEP Specification Language (ISL) v2.6

# The Observability and Telemetry Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.2, ISL v1.4, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5
**Supersedes:** ISL r0 v2.6 The Observability and Telemetry Model
**Document Type:** Observability, Telemetry, Monitoring, and Operational Transparency Specification

---

## 1.0 Scope

This document defines the Observability and Telemetry Model for the IMHOTEP platform. It establishes how platform activity is measured, emitted, correlated, stored, queried, visualized, alerted, retained, protected, and used for operational understanding.

This document applies to all observable activity across specification processing, canonical normalization, readiness evaluation, construction planning, runtime execution, agent collaboration, model invocation, deterministic tool invocation, artifact lifecycle management, validation, repair, governance, traceability, repository operations, deployment preparation, recovery, and platform health.

This document defines:

* observability principles
* telemetry categories
* telemetry event schema
* trace, metric, and log requirements
* correlation identifiers
* execution telemetry
* agent telemetry
* model telemetry
* tool telemetry
* artifact telemetry
* governance telemetry
* traceability telemetry
* state and recovery telemetry
* performance and resource telemetry
* dashboards and alerting
* telemetry storage and retention
* privacy and security controls
* OpenTelemetry alignment
* conformance requirements

---

## 2.0 Purpose of the Observability and Telemetry Model

The purpose of the Observability and Telemetry Model is to ensure that autonomous construction is visible, measurable, explainable, and operationally manageable. Autonomous software construction involves many interacting subsystems: specifications, semantic models, planning engines, runtime workers, reasoning agents, deterministic tools, governance policies, artifact repositories, model providers, and traceability graphs. Without observability, the platform would be difficult to operate safely.

Observability provides the operational evidence required to understand what happened, what is happening, and what may require intervention. It allows operators to detect blocked tasks, failing tools, repeated repair cycles, model failures, governance bottlenecks, repository conflicts, performance degradation, and abnormal behavior.

The Observability and Telemetry Model exists to ensure that:

* platform activity can be monitored in real time
* execution history can be reconstructed
* failures can be diagnosed
* performance and resource usage can be measured
* governance effects can be observed
* agent and model behavior can be evaluated
* deterministic validation outcomes can be analyzed
* operational dashboards can support human oversight
* telemetry can support continuous improvement without replacing audit or traceability records

---

## 3.0 Normative References

| Reference                   | Purpose                                                                    |
| --------------------------- | -------------------------------------------------------------------------- |
| ISL v0.0                    | Corpus-level principles and OpenTelemetry standards alignment              |
| ISL v1.2                    | Execution phases, runtime events, validation, repair, and completion       |
| ISL v1.4                    | Traceability events, graph snapshots, and audit queries                    |
| ISL v1.6                    | Tool invocation telemetry and tool outcome records                         |
| ISL v1.7                    | Governance events, approvals, waivers, overrides, escalation, and audit    |
| ISL v2.0                    | Execution Monitor and platform observability subsystem                     |
| ISL v2.1                    | Agent invocation, collaboration, failure, and telemetry events             |
| ISL v2.2                    | State events, snapshots, recovery, and consistency telemetry               |
| ISL v2.3                    | Artifact repository lifecycle and repository telemetry                     |
| ISL v2.4                    | Runtime telemetry, scheduling, workers, queues, locks, leases, and errors  |
| ISL v2.5                    | Model routing, invocation, fallback, and model observability               |
| OpenTelemetry Specification | Trace, metric, log, context propagation, and semantic convention alignment |
| RFC 2119                    | Normative terminology                                                      |

---

## 4.0 Terms and Definitions

**Observability**
The platform capability that enables operators, governance roles, engineers, and authorized systems to understand platform behavior through telemetry, state, traceability, audit, metrics, logs, traces, and alerts.

**Telemetry**
Structured data emitted by platform components describing events, measurements, state transitions, errors, resource usage, or operational behavior.

**Telemetry Event**
A discrete structured record describing something that occurred.

**Metric**
A numerical measurement sampled, aggregated, or calculated over time.

**Trace**
A correlated sequence of spans representing an end-to-end operation.

**Span**
A timed unit of work within a trace.

**Log**
A timestamped diagnostic or operational record.

**Signal**
A telemetry type, including trace, metric, log, event, profile, or alert.

**Correlation Identifier**
An identifier used to connect telemetry records across subsystems and lifecycle objects.

**Operational Dashboard**
A visual interface showing current or historical platform state, health, progress, or alerts.

**Alert**
A notification generated when telemetry satisfies a defined condition requiring attention.

---

## 5.0 Observability Design Principles

### 5.1 Observability Is Core Platform Capability

Observability MUST be integrated into platform subsystems. It MUST NOT be treated as optional debugging instrumentation.

Every major lifecycle action SHOULD emit telemetry sufficient to monitor execution and diagnose failures.

### 5.2 Telemetry Is Not Audit

Telemetry supports monitoring and diagnostics. It MUST NOT replace immutable governance audit logs, traceability records, artifact metadata, validation evidence, or state records.

Where an action is audit-relevant, both telemetry and audit/state records MAY exist, but the audit/state record remains authoritative.

### 5.3 Correlation Is Mandatory

Telemetry emitted by platform subsystems MUST include correlation identifiers sufficient to connect events to specifications, plans, executions, tasks, artifacts, agents, tools, models, governance checks, and traceability records where applicable.

Uncorrelated telemetry SHOULD be treated as degraded observability.

### 5.4 Structured Telemetry Is Required

Telemetry used for monitoring, alerting, dashboards, or automated analysis MUST be structured.

Free-form logs MAY exist, but they MUST NOT be the only telemetry source for lifecycle-critical operations.

### 5.5 Observability Must Preserve Security

Telemetry MUST NOT leak secrets, restricted context, sensitive artifacts, raw model prompts, confidential policy data, credentials, or regulated data unless explicitly authorized by governance.

Sensitive telemetry MUST be classified, protected, retained, and accessed according to policy.

### 5.6 Telemetry Must Be Actionable

Telemetry SHOULD support operational action. Events, metrics, logs, and alerts SHOULD identify affected targets, severity, likely cause, and remediation path when determinable.

### 5.7 OpenTelemetry Alignment

A conforming platform SHOULD align telemetry concepts with OpenTelemetry traces, metrics, logs, attributes, resources, context propagation, and semantic conventions.

Where ISL defines domain-specific attributes, implementations SHOULD map them into OpenTelemetry-compatible attributes where possible.

---

## 6.0 Observability Architecture

This section defines the logical observability architecture.

### 6.1 Logical Components

| Component           | Responsibility                                                |
| ------------------- | ------------------------------------------------------------- |
| Telemetry Emitter   | Emits structured telemetry from platform subsystems           |
| Telemetry Collector | Receives telemetry from emitters                              |
| Telemetry Processor | Normalizes, enriches, filters, samples, and redacts telemetry |
| Telemetry Store     | Persists telemetry events, logs, metrics, and traces          |
| Metrics Engine      | Aggregates and calculates metrics                             |
| Trace Engine        | Correlates spans across subsystems                            |
| Log Store           | Stores structured and diagnostic logs                         |
| Alert Engine        | Evaluates alert rules                                         |
| Dashboard Service   | Provides visual operational views                             |
| Exporter            | Sends telemetry to external observability systems             |
| Policy Filter       | Applies redaction, access, retention, and export controls     |

### 6.2 Architecture Rules

Every platform subsystem SHOULD emit telemetry through the Telemetry Emitter or equivalent interface.

Telemetry processors MUST apply classification and redaction rules before export.

Telemetry storage MUST preserve correlation identifiers.

Alert evaluation MUST use structured telemetry or metrics.

Telemetry export MUST obey governance and sensitivity rules.

---

## 7.0 Telemetry Signal Types

This section defines standard telemetry signal types.

| Signal Type | Purpose                                                  |
| ----------- | -------------------------------------------------------- |
| event       | Records a discrete lifecycle or operational occurrence   |
| trace       | Records an end-to-end operation composed of spans        |
| span        | Records a timed unit of work within a trace              |
| metric      | Records numerical measurement or aggregate               |
| log         | Records diagnostic or operational text/structured detail |
| alert       | Records notification generated by alert rule             |
| profile     | Records performance profile or resource breakdown        |
| heartbeat   | Records liveness of workers, services, or integrations   |

### 7.1 Signal Rules

A lifecycle transition SHOULD emit an event.

A long-running operation SHOULD emit a trace and spans.

Repeated measurements SHOULD be represented as metrics.

Human-readable diagnostic details MAY be logs, but lifecycle state MUST be represented structurally.

Worker liveness SHOULD be represented through heartbeat telemetry.

---

## 8.0 Common Telemetry Event Schema

This section defines the base schema for telemetry events. Domain-specific telemetry extends this schema.

| Field | Type | Required | Description |
|---|---|---|
| telemetry-event-id | string | REQUIRED | Unique telemetry event identifier |
| event-name | string | REQUIRED | Event name |
| event-category | enum | REQUIRED | execution, agent, model, tool, artifact, governance, traceability, state, repository, performance, security, telemetry, platform |
| signal-type | enum | REQUIRED | event, trace, span, metric, log, alert, profile, heartbeat |
| timestamp | ISO 8601 | REQUIRED | Event time |
| severity | enum | REQUIRED | critical, high, medium, low, informational |
| specification-id | string | CONDITIONAL | System identifier when scoped |
| specification-version | semver | CONDITIONAL | Specification version when scoped |
| execution-graph-id | string | CONDITIONAL | Execution graph identifier |
| plan-id | string | CONDITIONAL | Construction plan identifier |
| task-id | string | CONDITIONAL | Construction task identifier |
| artifact-id | string | CONDITIONAL | Artifact identifier |
| agent-invocation-id | string | CONDITIONAL | Agent invocation identifier |
| model-invocation-id | string | CONDITIONAL | Model invocation identifier |
| tool-invocation-id | string | CONDITIONAL | Tool invocation identifier |
| governance-record-id | string | CONDITIONAL | Governance record identifier |
| traceability-node-id | string | CONDITIONAL | Traceability node identifier |
| correlation-id | string | REQUIRED | Correlation identifier |
| parent-correlation-id | string | CONDITIONAL | Parent operation correlation identifier |
| source-subsystem | string | REQUIRED | Subsystem emitting event |
| outcome | enum | CONDITIONAL | started, completed, failed, blocked, escalated, warning, skipped |
| attributes | object | OPTIONAL | Additional structured attributes |
| redaction-status | enum | REQUIRED | none, redacted, summarized, restricted |
| retention-class | enum | REQUIRED | transient, operational, audit-support, archival |

### 8.1 Event Schema Rules

Telemetry events MUST include timestamp, source-subsystem, event-name, event-category, severity, and correlation-id.

Events scoped to a specification MUST include specification-id and specification-version.

Events related to execution MUST include execution-graph-id when available.

Events related to artifacts MUST include artifact-id when available.

Restricted events MUST indicate redaction-status.

---

## 9.0 Correlation and Context Propagation

Correlation allows telemetry from separate subsystems to be understood as one operation.

### 9.1 Required Correlation Identifiers

The platform SHOULD propagate:

* correlation-id
* trace-id
* span-id
* parent-span-id
* specification-id
* specification-version
* plan-id
* execution-graph-id
* task-id
* work-item-id
* artifact-id
* agent-invocation-id
* model-invocation-id
* tool-invocation-id
* governance-check-id
* repository-operation-id

### 9.2 Correlation Rules

A construction task execution SHOULD create or continue a trace.

Agent, model, tool, artifact, validation, repair, and governance events caused by a task SHOULD share the task correlation context.

A repair cycle SHOULD preserve correlation with the failed validation result and failed artifact.

A model fallback SHOULD preserve correlation with the original model invocation.

A telemetry event without required correlation identifiers SHOULD produce an observability warning.

---

## 10.0 Trace Model

This section defines traces and spans.

### 10.1 Standard Trace Types

| Trace Type                     | Description                                               |
| ------------------------------ | --------------------------------------------------------- |
| specification-processing-trace | Specification intake through canonical validation         |
| readiness-evaluation-trace     | Readiness evaluation and transition                       |
| planning-trace                 | Construction plan generation and validation               |
| execution-trace                | End-to-end construction execution                         |
| task-execution-trace           | Runtime execution of one task                             |
| agent-collaboration-trace      | Multi-agent collaboration session                         |
| model-invocation-trace         | Model routing, context control, invocation, normalization |
| tool-invocation-trace          | Tool selection, invocation, result normalization          |
| validation-trace               | Validation execution and result handling                  |
| repair-trace                   | Repair cycle from failure to revalidation                 |
| artifact-lifecycle-trace       | Artifact admission through promotion or archival          |
| governance-trace               | Governance check, approval, waiver, override, escalation  |
| deployment-preparation-trace   | Deployment preparation and authorization support          |

### 10.2 Span Schema

| Field | Type | Required | Description |
|---|---|---|
| trace-id | string | REQUIRED | Trace identifier |
| span-id | string | REQUIRED | Span identifier |
| parent-span-id | string | CONDITIONAL | Parent span |
| span-name | string | REQUIRED | Span name |
| span-kind | enum | REQUIRED | internal, client, server, producer, consumer |
| start-time | ISO 8601 | REQUIRED | Span start |
| end-time | ISO 8601 | CONDITIONAL | Span end |
| status | enum | REQUIRED | unset, ok, error |
| source-subsystem | string | REQUIRED | Subsystem emitting span |
| attributes | object | OPTIONAL | Span attributes |
| events | array | OPTIONAL | Span events |

### 10.3 Trace Rules

End-to-end execution SHOULD produce an execution-trace.

Every task SHOULD produce a task-execution-trace or span.

Model and tool invocations SHOULD be represented as child spans of the agent or validation task that caused them.

Trace status error MUST be set when the traced operation fails.

---

## 11.0 Metric Model

This section defines metrics used for platform monitoring.

### 11.1 Metric Types

| Metric Type | Purpose                          |
| ----------- | -------------------------------- |
| counter     | Monotonic count of events        |
| gauge       | Current value                    |
| histogram   | Distribution of observed values  |
| summary     | Aggregated statistical summary   |
| rate        | Derived rate over time           |
| ratio       | Derived proportion or percentage |

### 11.2 Metric Record Schema

| Field | Type | Required | Description |
|---|---|---|
| metric-name | string | REQUIRED | Metric name |
| metric-type | enum | REQUIRED | counter, gauge, histogram, summary, rate, ratio |
| value | number | REQUIRED | Metric value |
| unit | string | CONDITIONAL | Unit of measurement |
| timestamp | ISO 8601 | REQUIRED | Measurement time |
| attributes | object | OPTIONAL | Metric dimensions |
| source-subsystem | string | REQUIRED | Emitting subsystem |
| specification-id | string | CONDITIONAL | System identifier when scoped |
| execution-graph-id | string | CONDITIONAL | Execution graph when scoped |
| retention-class | enum | REQUIRED | Retention class |

### 11.3 Required Metric Categories

The platform SHOULD collect metrics for:

* execution progress
* task throughput
* task failure rate
* repair iteration count
* repair convergence rate
* agent invocation duration
* agent output validation failure rate
* model invocation latency
* model fallback rate
* tool invocation duration
* tool failure rate
* validation pass/fail rate
* artifact promotion rate
* governance approval wait time
* queue depth
* worker utilization
* lock wait time
* repository conflict rate
* telemetry ingestion failure rate

---

## 12.0 Log Model

Logs provide diagnostic detail.

### 12.1 Log Record Schema

| Field | Type | Required | Description |
|---|---|---|
| log-id | string | REQUIRED | Unique log identifier |
| timestamp | ISO 8601 | REQUIRED | Log time |
| severity | enum | REQUIRED | trace, debug, info, warn, error, fatal |
| source-subsystem | string | REQUIRED | Subsystem emitting log |
| message | string | REQUIRED | Human-readable message |
| correlation-id | string | REQUIRED | Correlation identifier |
| specification-id | string | CONDITIONAL | Specification context |
| execution-graph-id | string | CONDITIONAL | Execution context |
| task-id | string | CONDITIONAL | Task context |
| attributes | object | OPTIONAL | Structured attributes |
| redaction-status | enum | REQUIRED | none, redacted, summarized, restricted |

### 12.2 Log Rules

Logs SHOULD be structured.

Logs MUST NOT contain secrets.

Logs containing restricted information MUST be redacted or access-controlled.

Fatal logs SHOULD produce alert evaluation.

---

## 13.0 Execution Telemetry

Execution telemetry records progress and behavior of runtime execution.

### 13.1 Required Execution Events

The runtime SHOULD emit telemetry for:

* execution admission started
* execution admission completed
* execution lifecycle state changed
* execution started
* execution paused
* execution resumed
* execution escalated
* execution halted
* execution completed
* execution recovery started
* execution recovery completed
* phase started
* phase completed
* completion report generated

### 13.2 Execution Metrics

The platform SHOULD collect:

* active executions
* completed executions
* failed executions
* average execution duration
* execution recovery count
* execution halt count
* phase duration
* completion success rate

### 13.3 Execution Telemetry Rules

Execution telemetry MUST include execution-graph-id when execution has been initialized.

Execution completion telemetry MUST include final execution state.

Recovery telemetry MUST include recovery outcome.

---

## 14.0 Task and Scheduling Telemetry

Task and scheduling telemetry describes queueing, dispatch, worker execution, retries, locks, and leases.

### 14.1 Required Task Events

The runtime SHOULD emit telemetry for:

* task eligibility evaluated
* task queued
* task dispatched
* task started
* task completed
* task failed
* task skipped
* task escalated
* task retried
* task dead-lettered
* work item leased
* lease renewed
* lease expired
* lock acquired
* lock released
* lock conflict detected

### 14.2 Task Metrics

The platform SHOULD collect:

* task queue depth by queue type
* task throughput
* task duration by task type
* task failure rate by task type
* retry count by task type
* dead-letter count
* worker utilization
* lease expiration rate
* lock wait time
* lock conflict rate

---

## 15.0 Agent Telemetry

Agent telemetry records behavior of reasoning agents.

### 15.1 Required Agent Events

The platform SHOULD emit telemetry for:

* agent invocation requested
* context package assembled
* agent governance check completed
* agent output received
* agent output normalized
* agent output validated
* agent handoff created
* agent collaboration session started
* agent conflict detected
* agent failure detected
* agent retry attempted
* agent escalation opened
* agent invocation completed

### 15.2 Agent Metrics

The platform SHOULD collect:

* agent invocation count by role
* agent invocation duration by role
* agent output validation failure rate
* agent retry rate
* agent escalation rate
* collaboration conflict rate
* context assembly failure rate
* average context size by role

### 15.3 Agent Telemetry Rules

Agent telemetry MUST include agent-role and agent-invocation-id when applicable.

Agent telemetry MUST NOT include raw restricted context unless governance explicitly authorizes it.

---

## 16.0 Model Telemetry

Model telemetry records routing, context control, invocation, fallback, normalization, and failure behavior.

### 16.1 Required Model Events

The platform SHOULD emit telemetry for:

* model routing requested
* model routing decision made
* model context control passed
* model context control failed
* model invocation started
* model invocation completed
* model invocation failed
* model timeout
* model output normalization passed
* model output normalization failed
* model fallback selected
* model fallback exhausted
* model governance blocked
* model capability validation failed

### 16.2 Model Metrics

The platform SHOULD collect:

* model invocation count by model and provider
* model latency by model and provider
* model timeout rate
* model fallback rate
* model output contract failure rate
* model normalization failure rate
* model governance block rate
* model cost estimate where available
* token usage where available

### 16.3 Model Telemetry Rules

Model telemetry MUST include model-profile-id and provider-id when available.

Model telemetry MUST NOT expose raw prompts or restricted context to unauthorized telemetry consumers.

---

## 17.0 Tool Telemetry

Tool telemetry records deterministic tool selection, invocation, outcome, findings, and failure patterns.

### 17.1 Required Tool Events

The platform SHOULD emit telemetry for:

* tool selected
* tool invocation started
* tool invocation completed
* tool invocation failed
* tool timeout
* tool result normalized
* tool version drift detected
* tool result conflict detected
* tool sandbox violation
* tool evidence retained
* tool evidence retention failed

### 17.2 Tool Metrics

The platform SHOULD collect:

* tool invocation count by capability
* tool duration by tool and capability
* tool outcome count
* tool timeout rate
* tool error rate
* tool finding count by severity
* tool version drift count
* tool conflict count
* security tool finding rate

### 17.3 Tool Telemetry Rules

Tool telemetry MUST include tool-id and capability when applicable.

Security findings SHOULD generate high or critical severity telemetry depending on finding severity.

---

## 18.0 Artifact and Repository Telemetry

Artifact telemetry records artifact creation, admission, validation, promotion, packaging, merge, and archival behavior.

### 18.1 Required Artifact Events

The platform SHOULD emit telemetry for:

* artifact candidate received
* artifact admitted
* artifact rejected
* artifact state changed
* artifact validation linked
* artifact repaired
* artifact promoted
* artifact superseded
* artifact packaged
* artifact archived
* artifact deleted
* repository branch created
* repository merge attempted
* repository merge conflict detected
* repository integrity check completed
* repository recovery started
* repository recovery completed

### 18.2 Artifact Metrics

The platform SHOULD collect:

* artifact count by state
* artifact promotion rate
* artifact validation failure rate
* artifact repair rate
* artifact supersession count
* repository conflict rate
* package creation count
* repository integrity failure count

### 18.3 Artifact Telemetry Rules

Artifact telemetry MUST include artifact-id when an artifact is involved.

Repository telemetry MUST include repository-id or branch-id when applicable.

---

## 19.0 Governance Telemetry

Governance telemetry records policy evaluations, approval gates, waivers, overrides, escalations, risk changes, and deployment authorization behavior.

### 19.1 Required Governance Events

The platform SHOULD emit telemetry for:

* governance check requested
* governance check decided
* policy evaluated
* policy violation detected
* approval gate opened
* approval gate approved
* approval gate rejected
* approval gate expired
* waiver requested
* waiver granted
* waiver expired
* override requested
* override applied
* escalation opened
* escalation resolved
* risk tier assigned
* risk tier changed
* deployment authorization granted
* deployment authorization rejected
* governance audit write failed

### 19.2 Governance Metrics

The platform SHOULD collect:

* governance check count by decision
* policy violation count by severity
* approval wait time
* approval rejection rate
* waiver count by policy
* override count by type
* escalation duration
* deployment authorization wait time
* governance audit write failure count

### 19.3 Governance Telemetry Rules

Governance telemetry MUST NOT replace immutable governance audit records.

Governance telemetry SHOULD link to governance-record-id.

Governance audit write failure telemetry MUST be critical severity.

---

## 20.0 Traceability Telemetry

Traceability telemetry records traceability graph updates, snapshots, impact analysis, and integrity checks.

### 20.1 Required Traceability Events

The platform SHOULD emit telemetry for:

* traceability graph initialized
* traceability node created
* traceability edge created
* artifact association recorded
* traceability snapshot created
* traceability integrity check completed
* traceability integrity failed
* impact analysis started
* impact analysis completed
* untraced artifact detected

### 20.2 Traceability Metrics

The platform SHOULD collect:

* traceability node count
* traceability edge count
* untraced artifact count
* traceability integrity failure count
* impact analysis duration
* affected artifact count per impact analysis

### 20.3 Traceability Telemetry Rules

Traceability integrity failure telemetry SHOULD be high or critical severity when it blocks execution or artifact promotion.

---

## 21.0 State, Memory, and Recovery Telemetry

State telemetry records state transitions, checkpoints, snapshots, consistency checks, memory access, and recovery.

### 21.1 Required State Events

The platform SHOULD emit telemetry for:

* state record created
* state transition attempted
* state transition committed
* state transition rejected
* state transaction failed
* consistency check passed
* consistency check failed
* snapshot created
* checkpoint created
* recovery started
* recovery completed
* memory access requested
* memory access denied
* archival completed
* retention violation detected

### 21.2 State Metrics

The platform SHOULD collect:

* state transition count
* state transition rejection count
* state consistency failure count
* recovery count
* recovery duration
* snapshot count
* memory access denial count
* retention violation count

---

## 22.0 Performance and Resource Telemetry

Performance telemetry measures platform efficiency and capacity.

### 22.1 Required Performance Metrics

The platform SHOULD collect:

* CPU utilization by subsystem
* memory utilization by subsystem
* disk utilization
* network utilization
* queue processing latency
* worker utilization
* model invocation latency
* tool invocation latency
* repository operation duration
* governance approval wait duration
* telemetry ingestion latency
* telemetry processing latency
* telemetry export latency

### 22.2 Resource Saturation Alerts

The platform SHOULD define alerts for:

* sustained high CPU usage
* sustained high memory usage
* queue backlog growth
* worker starvation
* model provider saturation
* tool execution saturation
* repository storage exhaustion
* telemetry ingestion backlog
* telemetry storage exhaustion

---

## 23.0 Alerting Model

Alerting converts telemetry into operational notifications.

### 23.1 Alert Severity

| Severity      | Meaning                                                                               |
| ------------- | ------------------------------------------------------------------------------------- |
| critical      | Immediate action required; execution, security, governance, or data integrity at risk |
| high          | Significant issue requiring timely intervention                                       |
| medium        | Degraded condition requiring monitoring or planned intervention                       |
| low           | Minor issue or warning                                                                |
| informational | Useful operational signal                                                             |

### 23.2 Alert Rule Schema

| Field | Type | Required | Description |
|---|---|---|
| alert-rule-id | string | REQUIRED | Unique alert rule identifier |
| alert-name | string | REQUIRED | Human-readable alert name |
| signal-source | enum | REQUIRED | event, metric, log, trace |
| condition | string | REQUIRED | Structured condition expression |
| severity | enum | REQUIRED | Alert severity |
| scope | enum | REQUIRED | global, project, specification, execution, subsystem |
| evaluation-window | string | REQUIRED | Time window |
| threshold | string | CONDITIONAL | Threshold value |
| notification-targets | array | CONDITIONAL | Roles, systems, channels, or dashboards notified |
| suppression-policy | string | CONDITIONAL | Deduplication or suppression behavior |
| runbook-reference | string | CONDITIONAL | Remediation guide |
| status | enum | REQUIRED | active, disabled, deprecated |

### 23.3 Required Alert Conditions

The platform SHOULD define alert rules for:

* repeated task failures
* repair iteration threshold reached
* execution halted
* execution recovery failed
* governance approval timeout
* governance audit write failure
* critical security finding
* traceability integrity failure
* repository integrity failure
* model fallback exhausted
* tool timeout spike
* worker heartbeat failure
* queue backlog threshold exceeded
* telemetry ingestion failure
* restricted context exposure attempt

### 23.4 Alert Rules

Critical alerts SHOULD notify operations and governance roles where applicable.

Alerts MUST include affected identifiers when determinable.

Alerts SHOULD include runbook or required-action references.

Alert suppression MUST NOT hide critical unresolved conditions.

---

## 24.0 Operational Dashboards

Dashboards provide human-facing views of platform behavior.

### 24.1 Required Dashboard Categories

A conforming enterprise platform SHOULD provide dashboards for:

* platform health
* execution progress
* task queues and workers
* agent activity
* model usage and failures
* tool validation outcomes
* artifact lifecycle state
* governance status
* traceability integrity
* repair cycles
* repository integrity
* resource utilization
* alert summary

### 24.2 Dashboard Rules

Dashboards MUST respect access control.

Dashboards MUST NOT display restricted telemetry content to unauthorized viewers.

Dashboards SHOULD provide drill-down from summary metrics to correlated events, traces, logs, and state records.

Governance dashboards SHOULD distinguish telemetry from authoritative audit records.

---

## 25.0 Telemetry Storage and Retention

Telemetry must be stored so it can support operational diagnostics, trend analysis, and audit support.

### 25.1 Telemetry Store Requirements

Telemetry storage SHOULD support:

* event storage
* metric storage
* trace storage
* log storage
* alert history
* query by correlation-id
* query by specification-id
* query by execution-graph-id
* query by task-id
* query by artifact-id
* query by time range
* access control
* retention policy enforcement
* export to external observability systems

### 25.2 Retention Classes

| Retention Class | Meaning                                                       |
| --------------- | ------------------------------------------------------------- |
| transient       | Short-lived operational telemetry                             |
| operational     | Retained for operational troubleshooting                      |
| audit-support   | Retained to support audit or governance review                |
| archival        | Long-term telemetry retained for trend or compliance analysis |

### 25.3 Retention Rules

Telemetry supporting High or Critical risk execution SHOULD be retained longer than telemetry for Standard systems.

Telemetry linked to governance audit write failure, security finding, deployment authorization, or execution failure SHOULD be retained as audit-support or archival.

Telemetry retention MUST NOT conflict with privacy, security, or data minimization requirements.

---

## 26.0 Telemetry Access Control and Privacy

Telemetry may contain sensitive operational details.

### 26.1 Access Control Rules

Telemetry access MUST be controlled by role, scope, sensitivity, and purpose.

Restricted telemetry MUST require explicit authorization.

Telemetry exports MUST enforce redaction and filtering policies.

Raw model prompts, raw context packages, secrets, and restricted artifact contents MUST NOT be exposed through telemetry dashboards unless explicitly authorized.

### 26.2 Redaction Rules

Telemetry processors MUST redact:

* secrets
* credentials
* tokens
* private keys
* restricted personal data
* raw restricted prompts or context
* sensitive infrastructure details when policy requires

Redaction MUST preserve enough metadata for diagnosis when possible.

### 26.3 Privacy Rules

Telemetry collection SHOULD follow data minimization.

Telemetry containing personal data MUST be classified and governed.

Telemetry used for continuous improvement MUST be aggregated or anonymized where required by policy.

---

## 27.0 Telemetry Export and Integration

The platform may integrate with external observability systems.

### 27.1 Export Targets

Telemetry MAY be exported to:

* OpenTelemetry collectors
* log aggregation systems
* metrics platforms
* tracing systems
* SIEM systems
* governance dashboards
* enterprise monitoring platforms
* data warehouses for analytics

### 27.2 Export Rules

Telemetry export MUST obey governance policy.

Restricted telemetry MUST be redacted, summarized, or blocked before export unless explicitly authorized.

Export failures SHOULD produce telemetry and alerts.

External export MUST preserve correlation identifiers.

---

## 28.0 OpenTelemetry Alignment

This section defines alignment with OpenTelemetry.

### 28.1 Mapping

| ISL Concept              | OpenTelemetry Concept             |
| ------------------------ | --------------------------------- |
| Telemetry Event          | Log record or span event          |
| Execution Trace          | Trace                             |
| Task Execution           | Span                              |
| Agent Invocation         | Span                              |
| Model Invocation         | Span                              |
| Tool Invocation          | Span                              |
| Artifact Lifecycle Event | Log record or span event          |
| Governance Check         | Span or log record                |
| Metric Record            | Metric                            |
| Correlation Identifier   | Trace context and attributes      |
| Source Subsystem         | Resource or instrumentation scope |
| Severity                 | Severity number/text              |

### 28.2 Attribute Naming

ISL implementations SHOULD use consistent attribute names, including:

* `isl.specification.id`
* `isl.specification.version`
* `isl.plan.id`
* `isl.execution.graph.id`
* `isl.task.id`
* `isl.artifact.id`
* `isl.agent.invocation.id`
* `isl.model.invocation.id`
* `isl.tool.invocation.id`
* `isl.governance.record.id`
* `isl.traceability.node.id`
* `isl.subsystem`
* `isl.risk.tier`
* `isl.outcome`

### 28.3 Alignment Rules

A platform claiming OpenTelemetry alignment SHOULD export traces, metrics, and logs in OpenTelemetry-compatible form.

Where OpenTelemetry lacks a semantic convention for ISL-specific concepts, ISL attributes MAY be used.

OpenTelemetry export MUST NOT weaken ISL security, retention, or governance rules.

---

## 29.0 Telemetry Quality and Reliability

Telemetry itself must be reliable enough to support operations.

### 29.1 Telemetry Quality Dimensions

The platform SHOULD monitor:

* completeness
* timeliness
* correlation coverage
* schema validity
* ingestion success rate
* processing failure rate
* export success rate
* redaction success rate
* storage availability
* query latency

### 29.2 Telemetry Health Metrics

The platform SHOULD collect:

* telemetry events emitted
* telemetry events ingested
* telemetry events dropped
* telemetry processing latency
* telemetry export failures
* telemetry schema validation failures
* uncorrelated telemetry count
* redaction failures
* dashboard query latency

### 29.3 Telemetry Reliability Rules

Telemetry failure MUST NOT silently hide platform failure.

Telemetry ingestion failure SHOULD produce local fallback logs or events when possible.

Telemetry pipeline failure affecting critical alerts MUST itself produce a critical alert when detection is possible.

---

## 30.0 Continuous Improvement Use

Telemetry may support platform improvement.

### 30.1 Improvement Inputs

Telemetry MAY be analyzed to improve:

* planning algorithms
* task scheduling
* repair strategies
* agent role prompts or contracts
* model routing policies
* tool selection policies
* governance process efficiency
* artifact repository workflows
* runtime scaling policies
* documentation and runbooks

### 30.2 Improvement Rules

Telemetry used for continuous improvement MUST respect privacy, retention, sensitivity, and governance constraints.

Model or agent improvement based on telemetry MUST NOT expose restricted context unless governance permits it.

Historical telemetry analysis MUST distinguish correlation from causation when used to update platform behavior.

Changes derived from telemetry analysis MUST follow configuration and governance rules before affecting active execution.

---

## 31.0 Observability Error Classes

This section defines observability-specific errors.

### 31.1 Error Classes

| Error Class                             | Description                                      | Default Handling       |
| --------------------------------------- | ------------------------------------------------ | ---------------------- |
| observability-event-schema-invalid      | Telemetry event failed schema validation         | warn or reject event   |
| observability-correlation-missing       | Required correlation identifier missing          | warn                   |
| observability-ingestion-failed          | Telemetry collector failed to ingest event       | retry or fallback      |
| observability-processing-failed         | Telemetry processor failed                       | retry or alert         |
| observability-redaction-failed          | Redaction failed for sensitive telemetry         | block export and alert |
| observability-export-failed             | Export to external system failed                 | retry and alert        |
| observability-storage-unavailable       | Telemetry store unavailable                      | fallback or alert      |
| observability-query-failed              | Telemetry query failed                           | warn or alert          |
| observability-alert-evaluation-failed   | Alert rule evaluation failed                     | alert operations       |
| observability-dashboard-unavailable     | Dashboard unavailable                            | warn or alert          |
| observability-retention-violation       | Telemetry retained too long or deleted too early | escalate               |
| observability-unauthorized-access       | Unauthorized telemetry access attempted          | block and escalate     |
| observability-critical-alert-suppressed | Critical alert suppressed improperly             | escalate               |

### 31.2 Error Record Schema

| Field | Type | Required | Description |
|---|---|---|
| observability-error-id | string | REQUIRED | Unique observability error identifier |
| error-class | enum | REQUIRED | Error class |
| severity | enum | REQUIRED | critical, high, medium, low |
| affected-signal-type | enum | CONDITIONAL | event, trace, span, metric, log, alert |
| affected-record-id | string | CONDITIONAL | Telemetry record affected |
| source-subsystem | string | REQUIRED | Subsystem detecting error |
| message | string | REQUIRED | Human-readable explanation |
| required-action | string | REQUIRED | Remediation required |
| detected-at | ISO 8601 | REQUIRED | Detection time |

---

## 32.0 Conformance Requirements

### 32.1 Telemetry Event Conformance

A telemetry event conforms to ISL v2.6 if it:

* includes required common schema fields
* uses a defined event category
* includes correlation-id
* includes specification and execution identifiers where applicable
* declares severity
* declares redaction status
* declares retention class
* avoids unauthorized sensitive content

### 32.2 Observability Subsystem Conformance

An observability subsystem conforms to ISL v2.6 if it can:

* collect structured events
* collect metrics
* collect traces and spans
* collect structured logs
* preserve correlation identifiers
* apply redaction and access controls
* store telemetry according to retention rules
* evaluate alert rules
* expose dashboards or query interfaces
* export telemetry where configured
* monitor telemetry pipeline health
* produce observability error records

### 32.3 Platform Conformance

A platform conforms to ISL v2.6 if it:

* emits telemetry for major lifecycle activities
* correlates telemetry across subsystems
* distinguishes telemetry from audit and traceability records
* provides execution, agent, model, tool, artifact, governance, traceability, state, and performance telemetry
* supports monitoring and alerting
* supports operational dashboards
* protects sensitive telemetry
* supports telemetry retention and export governance
* aligns with OpenTelemetry where claimed
* uses telemetry to support operational transparency and continuous improvement

---

## 33.0 Summary

ISL v2.6 defines the Observability and Telemetry Model for the IMHOTEP platform. It establishes how telemetry is emitted, structured, correlated, stored, queried, visualized, alerted, retained, protected, exported, and aligned with OpenTelemetry.

This revision strengthens the original observability model by turning general visibility concepts into explicit operational contracts. It defines telemetry schemas, signal types, traces, metrics, logs, correlation identifiers, domain-specific telemetry categories, alert rules, dashboards, retention, redaction, access control, export, telemetry quality, and conformance requirements.

A conforming IMHOTEP platform MUST make autonomous construction observable without confusing telemetry with audit or traceability. Telemetry shows platform behavior; audit proves governance history; traceability proves derivation. Together, they make autonomous construction transparent, governable, diagnosable, and continuously improvable.
