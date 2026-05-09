# IMHOTEP Specification Language (ISL) v3.2

# The First Proof-of-Concept Construction Pipeline

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.0, ISL v1.1, ISL v1.2, ISL v1.3, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.1, ISL v2.2, ISL v2.3, ISL v2.4, ISL v2.5, ISL v2.6, ISL v2.7, ISL v3.0, ISL v3.1
**Supersedes:** ISL r0 v3.2 The First Proof-of-Concept Construction Pipeline
**Document Type:** Proof-of-Concept Pipeline Specification

---

## 1.0 Scope

This document defines the first proof-of-concept construction pipeline for the IMHOTEP platform. It specifies the minimum executable workflow required to demonstrate that a structured ISL specification can be transformed into a working software artifact through canonical normalization, construction planning, agent-assisted generation, deterministic validation, bounded repair, artifact finalization, traceability, and telemetry.

This document applies to the first operational milestone of the IMHOTEP reference implementation. It intentionally defines a narrow pipeline. It does not attempt to demonstrate the full enterprise platform, distributed runtime, multi-tenant deployment, advanced governance, advanced security scanning, complex orchestration, or multi-service application construction.

This document defines:

* proof-of-concept objectives
* supported input scope
* minimum platform components
* pipeline stages
* required records and evidence
* task graph requirements
* generated artifact expectations
* deterministic validation requirements
* repair cycle requirements
* artifact finalization rules
* telemetry requirements
* success criteria
* non-goals
* conformance requirements

---

## 2.0 Purpose of the Proof-of-Concept Pipeline

The purpose of the Proof-of-Concept Construction Pipeline is to provide the first executable proof that the IMHOTEP architecture can function in practice. The platform must show that it can accept a structured specification, interpret it through a canonical model, produce a construction plan, generate implementation artifacts, validate those artifacts using deterministic tools, repair failures, and produce stable outputs.

The proof of concept is not a product launch. It is an architectural viability test. Its value is not in the complexity of the generated system, but in proving that the construction loop works end to end without relying on informal manual steps, hidden assumptions, or unrecorded decisions.

The PoC exists to demonstrate that:

* a structured ISL input can be processed by the platform
* a canonical semantic model can be produced from the input
* a construction task graph can be generated from the model
* agent-generated artifacts can be admitted into an artifact repository
* deterministic tools can validate generated artifacts
* repair cycles can respond to validation failures
* final artifacts can be traced back to specification entities
* telemetry can show what happened during the pipeline
* success can be determined by evidence rather than assertion

---

## 3.0 Proof-of-Concept Design Principles

This section defines principles that constrain the first pipeline. These principles are necessary because the first implementation must be intentionally small while still proving the core IMHOTEP claims.

### 3.1 Narrow Scope, Real Execution

The PoC MUST be narrow in supported domain, but it MUST execute real platform behavior. It MUST NOT be a scripted demo that skips canonical normalization, planning, artifact generation, deterministic validation, state recording, or traceability.

The implementation MAY use simplified modules, but those modules MUST preserve the same control boundaries defined in ISL v3.0 and v3.1.

### 3.2 No Fake Success

The PoC MUST NOT report success unless generated artifacts compile or otherwise pass the configured deterministic validation tools.

A successful natural-language explanation from an agent MUST NOT be treated as successful validation.

### 3.3 Evidence Before Claim

Every success criterion MUST be supported by a record, artifact, validation result, traceability link, state transition, or telemetry event.

The pipeline MUST produce an evidence bundle sufficient to review the run after completion.

### 3.4 Repair Must Be Bounded

The PoC MUST support repair cycles, but repair MUST be bounded by the repair termination rules defined in ISL v1.2, v1.5, and v2.4.

An implementation MUST NOT allow an unbounded generate-validate-repair loop.

### 3.5 Minimal Governance, Not No Governance

The PoC MAY use a simplified governance profile. However, it MUST still represent readiness, execution authorization, validation waivers if any, and audit-relevant decisions.

The PoC MUST NOT bypass governance-controlled state transitions merely because it is a proof of concept.

---

## 4.0 Proof-of-Concept Objectives

This section defines the objectives of the first pipeline. These objectives are intentionally limited and measurable.

### 4.1 Primary Objectives

The PoC MUST demonstrate the ability to:

* ingest a structured ISL specification
* validate specification structure
* normalize the specification into canonical entities
* generate a construction task graph
* execute the task graph through the runtime
* invoke at least one implementation-generation agent
* generate source, test, and configuration artifacts
* admit generated artifacts into an artifact repository
* compile or otherwise build the generated system
* execute deterministic validation tests
* detect validation failures
* perform bounded repair cycles
* revalidate repaired artifacts
* finalize stable artifacts
* create traceability links from specification entities to artifacts
* emit telemetry for major pipeline stages
* produce a completion report

### 4.2 Secondary Objectives

The PoC SHOULD demonstrate:

* deterministic task ordering
* basic retry behavior
* repair history preservation
* artifact versioning after repair
* tool result normalization
* model invocation traceability
* repository integrity checking
* minimal dashboard or queryable telemetry output
* repeatable execution from the same input

### 4.3 Objective Rules

A primary objective MUST be satisfied for the PoC to be considered successful.

A secondary objective SHOULD be satisfied where feasible, but failure to satisfy a secondary objective MUST be recorded as a limitation.

A PoC run MUST distinguish primary objective failure from secondary objective limitation.

---

## 5.0 Non-Goals

This section defines what the first PoC is not required to prove. Explicit non-goals prevent the first implementation from becoming too broad or ambiguous.

The first PoC is not required to demonstrate:

* multi-service distributed system generation
* production deployment
* multi-tenant isolation
* enterprise identity integration
* complex policy governance
* advanced security scanning
* full infrastructure provisioning
* database migration generation
* distributed runtime workers
* high availability
* disaster recovery
* multiple programming languages
* advanced UI generation
* domain-specific enterprise architecture modeling
* human approval workflow automation beyond minimal records

The PoC MAY include any of these capabilities only if they do not obscure or weaken the primary objectives.

---

## 6.0 Supported Input Scope

The first PoC MUST support a constrained subset of ISL sufficient to describe a minimal service-based application.

The input specification is the pipeline’s starting point. It must be structured enough to be parsed, normalized, planned, and traced. The earlier review identified that the proof-of-concept pipeline’s own input must not be absent; therefore, the PoC MUST define and include a concrete input specification fixture. 

### 6.1 Required Input Entities

The PoC input specification MUST include at least:

| Entity      | Minimum Count | Purpose                                      |
| ----------- | ------------: | -------------------------------------------- |
| Project     |             1 | Identifies the system being constructed      |
| Context     |             1 | Describes scope and assumptions              |
| Requirement |             1 | Defines expected behavior                    |
| Capability  |             1 | Groups behavior into platform capability     |
| Service     |             1 | Defines the generated service                |
| Interface   |             1 | Defines externally visible operation         |
| DataEntity  |             1 | Defines request or response structure        |
| Validation  |             1 | Defines acceptance or verification condition |

### 6.2 Optional Input Entities

The PoC input MAY include:

* Actor
* Workflow
* Policy
* Infrastructure
* additional Requirements
* additional Interfaces
* additional DataEntities
* additional Validations

### 6.3 Input Format Rules

The PoC MUST support at least one structured representation format.

The supported input format MUST be documented.

The PoC input MUST be stored as a versioned example under the reference repository.

The PoC input MUST have deterministic identifiers for canonical entities.

The PoC input MUST be sufficient to generate the minimal target system without hidden manual requirements.

---

## 7.0 Required PoC Input Fixture

This section defines the minimum required fixture that must exist in the reference implementation repository.

### 7.1 Fixture Location

The reference implementation SHOULD store the PoC input fixture at:

`/examples/specifications/poc/minimal-service.isl.md`

If another location is used, it MUST be documented in the PoC run configuration.

### 7.2 Fixture Metadata

The fixture MUST include:

| Field                 | Required | Description                           |
| --------------------- | -------- | ------------------------------------- |
| specification-id      | YES      | Unique system identifier              |
| specification-version | YES      | Specification version                 |
| isl-version           | YES      | ISL version targeted                  |
| name                  | YES      | Human-readable system name            |
| owner                 | YES      | Owning role, team, or project         |
| readiness-target      | YES      | Target readiness for PoC              |
| risk-tier             | YES      | Standard unless explicitly changed    |
| target-platform       | YES      | Runtime platform for generated system |
| target-language       | YES      | Programming language or framework     |
| validation-profile    | YES      | Validation checks required            |

### 7.3 Fixture Content Requirements

The fixture MUST describe:

* one minimal service
* one externally callable operation
* one request or response data structure
* at least one functional requirement
* at least one validation criterion
* expected success behavior
* expected generated artifact types

### 7.4 Fixture Rules

The fixture MUST be version controlled.

The fixture MUST be used by automated scenario tests.

The fixture MUST be sufficient to reproduce a PoC run.

Changes to the fixture MUST trigger scenario regression testing.

---

## 8.0 Minimal Target System

This section defines the system the PoC pipeline must generate. The target system must be intentionally simple but real enough to compile, test, and validate.

The original MACS document identifies the first reference system as a minimal .NET REST service with a basic project structure, endpoint, data model, and tests.  This document defines the pipeline-level expectations for constructing that system or an equivalent minimal service.

### 8.1 Default Target System

The default PoC target system SHOULD be a minimal .NET REST service.

The generated system SHOULD include:

* project file
* program entry point
* minimal service endpoint
* request or response data model
* simple business logic or handler
* unit or integration test project
* minimal configuration file
* repository metadata
* validation evidence

### 8.2 Equivalent Target System

An implementation MAY choose another platform or language if it provides equivalent proof.

An equivalent target system MUST include:

* executable or buildable project structure
* callable interface or operation
* generated source artifact
* generated test artifact
* deterministic build validation
* deterministic test validation
* artifact repository metadata
* traceability links

### 8.3 Target System Rules

The target system MUST be generated from the ISL fixture.

The target system MUST NOT be prewritten and copied as the final output without agent or generation activity.

Boilerplate templates MAY be used only if the generated artifact metadata records which content was templated and which content was derived from specification entities.

The generated system MUST be independently buildable using the declared validation profile.

---

## 9.0 Minimum Platform Components

This section defines the minimum platform components required for the PoC.

### 9.1 Required Components

The PoC MUST include functional implementations or constrained substitutes for:

| Component               | Minimum Requirement                                   |
| ----------------------- | ----------------------------------------------------- |
| Specification Engine    | Parse and structurally validate PoC input             |
| Semantic Model Engine   | Produce canonical entities and relationships          |
| Readiness Evaluator     | Confirm PoC execution eligibility                     |
| Planning Engine         | Produce construction task graph                       |
| Execution Runtime       | Execute task graph and maintain task state            |
| Agent Orchestrator      | Invoke required agent roles                           |
| Model Integration Layer | Route model invocation or local generation equivalent |
| Artifact Repository     | Admit, store, version, and finalize artifacts         |
| Tool Integration Layer  | Run deterministic build and test tools                |
| Traceability Engine     | Link specification entities, tasks, and artifacts     |
| State Manager           | Persist state transitions and completion state        |
| Observability Emitter   | Emit structured telemetry                             |
| Governance Module       | Enforce minimal PoC governance profile                |

### 9.2 Permitted Simplifications

The PoC MAY simplify:

* identity integration
* distributed worker scheduling
* dashboard presentation
* complex model routing
* enterprise policy libraries
* artifact branch strategy
* deployment packaging
* external telemetry export

### 9.3 Component Rules

A simplified component MUST still preserve the contract of the subsystem it represents.

A mocked or stubbed component MUST be clearly identified in the PoC run report.

A mocked or stubbed component MUST NOT be used to satisfy deterministic validation, artifact creation, or completion evidence unless the test explicitly declares itself non-conforming.

---

## 10.0 Pipeline Overview

The PoC pipeline consists of ordered stages. Each stage has required inputs, outputs, evidence, and failure behavior.

### 10.1 Pipeline Stages

| Stage | Name                               | Primary Purpose                                                    |
| ----: | ---------------------------------- | ------------------------------------------------------------------ |
|     1 | Specification Intake               | Load and structurally validate PoC fixture                         |
|     2 | Semantic Normalization             | Produce canonical semantic model                                   |
|     3 | Readiness and Governance Admission | Confirm execution eligibility                                      |
|     4 | Construction Planning              | Generate executable task graph                                     |
|     5 | Runtime Initialization             | Initialize execution graph, state, telemetry, repository workspace |
|     6 | Artifact Generation                | Generate source, test, config, and metadata artifacts              |
|     7 | Repository Admission               | Admit artifacts into controlled repository state                   |
|     8 | Deterministic Validation           | Compile/build and test generated artifacts                         |
|     9 | Repair Cycle                       | Analyze failures, revise artifacts, revalidate                     |
|    10 | Artifact Finalization              | Promote valid artifacts to stable state                            |
|    11 | Traceability and Evidence Closure  | Produce links, snapshots, evidence bundle                          |
|    12 | Completion Reporting               | Produce final PoC completion report                                |

### 10.2 Stage Rules

Stages MUST execute in the listed order unless a later revision defines a valid parallelization model.

A stage MUST NOT be marked complete unless its required outputs exist.

A failed stage MUST produce a structured error record.

A failed stage MAY trigger retry, repair, escalation, or terminal failure according to this document and ISL v2.4.

---

## 11.0 Stage 1 — Specification Intake

Specification Intake loads the PoC input fixture and verifies that it is structurally valid for the supported subset of ISL.

### 11.1 Required Inputs

| Input                                    | Required |
| ---------------------------------------- | -------- |
| PoC input fixture                        | YES      |
| Supported ISL subset declaration         | YES      |
| Parser configuration                     | YES      |
| Specification schema or structural rules | YES      |

### 11.2 Required Outputs

| Output                       | Required |
| ---------------------------- | -------- |
| Parsed Specification Record  | YES      |
| Structural Validation Report | YES      |
| Source Location Map          | SHOULD   |
| Specification State Record   | YES      |

### 11.3 Intake Validation Rules

The Specification Engine MUST verify:

* required metadata exists
* required sections exist
* required entity identifiers exist
* required entity types are supported
* entity references are syntactically valid
* unsupported sections are reported
* blocking structural errors are recorded

### 11.4 Intake Failure Rules

If required metadata is missing, intake MUST fail.

If required entities are missing, intake MUST fail.

If unsupported content is present but not required, intake MAY warn.

If source cannot be parsed, pipeline execution MUST stop before semantic normalization.

---

## 12.0 Stage 2 — Semantic Normalization

Semantic Normalization converts the parsed specification into canonical entities and relationships.

### 12.1 Required Inputs

| Input                       | Required |
| --------------------------- | -------- |
| Parsed Specification Record | YES      |
| Source Location Map         | SHOULD   |
| Canonical entity schemas    | YES      |
| Identifier policy           | YES      |

### 12.2 Required Outputs

| Output                         | Required |
| ------------------------------ | -------- |
| Canonical Model Record         | YES      |
| Canonical Entity Records       | YES      |
| Canonical Relationship Records | YES      |
| Canonical Validation Report    | YES      |
| Canonical Model State Record   | YES      |

### 12.3 Required Canonical Entities

The canonical model MUST include canonical records for the required PoC entities defined in §6.1.

### 12.4 Normalization Rules

Each parsed entity MUST map to a canonical entity or produce a finding.

Every canonical entity MUST include required base fields.

Every canonical relationship MUST reference valid entity identifiers.

A canonical model with blocking validation errors MUST NOT proceed to planning.

---

## 13.0 Stage 3 — Readiness and Governance Admission

This stage confirms that the PoC specification and pipeline run are eligible for execution.

### 13.1 Required Inputs

| Input                       | Required |
| --------------------------- | -------- |
| Canonical Model Record      | YES      |
| Canonical Validation Report | YES      |
| Readiness rules             | YES      |
| Minimal governance profile  | YES      |
| PoC risk tier               | YES      |

### 13.2 Required Outputs

| Output                       | Required |
| ---------------------------- | -------- |
| Readiness Evaluation Record  | YES      |
| Governance Check Record      | YES      |
| Execution Admission Decision | YES      |
| Audit-Support Record         | SHOULD   |

### 13.3 Admission Rules

The PoC MUST NOT proceed to construction planning unless canonical validation passed or non-blocking warnings are explicitly accepted by the PoC governance profile.

The PoC MUST use Standard risk tier unless the input fixture declares a different tier.

The PoC MUST record execution authorization, even if implemented as a simplified local governance decision.

A blocked governance decision MUST stop the pipeline.

---

## 14.0 Stage 4 — Construction Planning

Construction Planning produces the task graph required to generate and validate the minimal target system.

### 14.1 Required Inputs

| Input                       | Required |
| --------------------------- | -------- |
| Canonical Model Record      | YES      |
| Readiness Evaluation Record | YES      |
| Governance Check Record     | YES      |
| Tool Capability Catalog     | YES      |
| Agent Role Registry         | YES      |
| Artifact Type Policy        | YES      |

### 14.2 Required Outputs

| Output                            | Required |
| --------------------------------- | -------- |
| Construction Plan Record          | YES      |
| Construction Task Graph           | YES      |
| Planning Validation Report        | YES      |
| Task-to-Entity Traceability Seeds | YES      |

### 14.3 Minimum Task Graph

The task graph MUST include tasks equivalent to:

| Task Type                    | Required    | Agent or Tool                               |
| ---------------------------- | ----------- | ------------------------------------------- |
| specification-interpretation | YES         | Specification Interpreter                   |
| architecture                 | SHOULD      | Architecture Planner or Planning Engine     |
| implementation               | YES         | Implementation Generator                    |
| schema                       | CONDITIONAL | Implementation Generator                    |
| interface                    | YES         | Implementation Generator                    |
| test                         | YES         | Test Generator or Implementation Generator  |
| verification-build           | YES         | Tool capability: compile or build           |
| verification-test            | YES         | Tool capability: unit-test or equivalent    |
| repair                       | YES         | Repair Analyst and Implementation Generator |
| artifact-finalization        | YES         | Artifact Repository                         |
| traceability-closure         | YES         | Traceability Engine                         |
| completion-report            | YES         | Runtime                                     |

### 14.4 Planning Rules

Every task MUST trace to at least one canonical entity or pipeline obligation.

Every artifact-producing task MUST declare expected artifact types.

Every validation task MUST declare required tool capability.

Every repair task MUST declare repair limits.

The plan MUST NOT be executable until planning validation passes.

---

## 15.0 Stage 5 — Runtime Initialization

Runtime Initialization prepares the execution environment for controlled execution.

### 15.1 Required Inputs

| Input                         | Required |
| ----------------------------- | -------- |
| Executable Construction Plan  | YES      |
| Runtime Configuration         | YES      |
| Artifact Repository Workspace | YES      |
| State Store                   | YES      |
| Telemetry Configuration       | YES      |

### 15.2 Required Outputs

| Output                       | Required |
| ---------------------------- | -------- |
| Execution Graph              | YES      |
| Initial Runtime State Record | YES      |
| Initial Checkpoint           | YES      |
| Work Item Records            | YES      |
| Repository Workspace Record  | YES      |

### 15.3 Runtime Initialization Rules

The runtime MUST initialize an execution graph before dispatching work.

The runtime MUST create an initial checkpoint.

The runtime MUST create work items for eligible tasks.

The runtime MUST emit execution-start telemetry.

The runtime MUST NOT dispatch work if state, repository, governance, or traceability components are unavailable.

---

## 16.0 Stage 6 — Artifact Generation

Artifact Generation creates candidate implementation artifacts from the construction task graph.

### 16.1 Required Inputs

| Input                  | Required |
| ---------------------- | -------- |
| Implementation tasks   | YES      |
| Canonical entities     | YES      |
| Agent context package  | YES      |
| Output contracts       | YES      |
| Artifact naming policy | YES      |

### 16.2 Required Outputs

| Output                                               | Required |
| ---------------------------------------------------- | -------- |
| Agent Invocation Records                             | YES      |
| Model Invocation Records or Local Generation Records | YES      |
| Agent Output Validation Records                      | YES      |
| Candidate Artifact Records                           | YES      |
| Artifact Content                                     | YES      |

### 16.3 Minimum Generated Artifacts

The PoC MUST generate at least:

| Artifact Type | Example                                  |
| ------------- | ---------------------------------------- |
| source        | service entry point or handler           |
| source        | data model or DTO                        |
| test          | unit or integration test                 |
| config        | minimal project or runtime configuration |
| metadata      | artifact metadata record                 |

For a .NET target, generated artifacts SHOULD include:

* project file
* program entry point
* endpoint or controller handler
* data model class or record
* test project file
* test file

### 16.4 Generation Rules

Generated artifacts MUST reference source canonical entities.

Generated artifacts MUST be produced as candidate artifacts before repository admission.

Agent outputs MUST pass output validation before artifact admission.

Generated behavior MUST NOT exceed the supported specification scope unless explicitly recorded as implementation scaffolding.

---

## 17.0 Stage 7 — Repository Admission

Repository Admission accepts generated candidate artifacts into controlled repository working state.

### 17.1 Required Inputs

| Input                       | Required |
| --------------------------- | -------- |
| Candidate Artifact Records  | YES      |
| Artifact Content            | YES      |
| Artifact Metadata           | YES      |
| Repository Workspace Record | YES      |

### 17.2 Required Outputs

| Output                     | Required |
| -------------------------- | -------- |
| Artifact Admission Records | YES      |
| Artifact Metadata Records  | YES      |
| Artifact State Records     | YES      |
| Repository State Record    | YES      |

### 17.3 Admission Rules

Every candidate artifact MUST receive an artifact identifier.

Every artifact MUST have metadata.

Every artifact MUST be associated with a producing task.

Every artifact MUST be associated with one or more source entities or pipeline obligations.

Artifacts with missing content MUST be rejected.

Artifacts with missing metadata MUST be rejected.

Artifacts admitted before validation MUST have pending state.

---

## 18.0 Stage 8 — Deterministic Validation

Deterministic Validation verifies generated artifacts through build and test tools.

### 18.1 Required Inputs

| Input                   | Required |
| ----------------------- | -------- |
| Pending artifacts       | YES      |
| Tool Capability Catalog | YES      |
| Build or compile tool   | YES      |
| Test tool               | YES      |
| Validation criteria     | YES      |

### 18.2 Required Outputs

| Output                    | Required |
| ------------------------- | -------- |
| Tool Invocation Records   | YES      |
| Tool Invocation Results   | YES      |
| Validation Result Records | YES      |
| Tool Evidence Records     | YES      |
| Artifact State Updates    | YES      |

### 18.3 Required Validation Checks

The PoC MUST perform:

| Validation Check                | Required                |
| ------------------------------- | ----------------------- |
| source build or compile         | YES                     |
| automated test execution        | YES                     |
| project/configuration validity  | SHOULD                  |
| repository integrity check      | SHOULD                  |
| traceability completeness check | YES before finalization |

### 18.4 Validation Rules

A build failure MUST produce a failed validation result.

A test failure MUST produce a failed validation result.

A tool timeout MUST NOT be treated as success.

A validation result MUST reference the artifact version evaluated.

A failed validation MUST prevent artifact finalization unless repaired or waived.

---

## 19.0 Stage 9 — Repair Cycle

Repair Cycle responds to failed validation by analyzing findings, generating corrected artifacts, and revalidating.

### 19.1 Required Inputs

| Input                    | Required    |
| ------------------------ | ----------- |
| Failed Validation Result | YES         |
| Failed Artifact Records  | YES         |
| Tool Findings            | YES         |
| Repair Limits            | YES         |
| Prior Repair Records     | CONDITIONAL |

### 19.2 Required Outputs

| Output                   | Required                           |
| ------------------------ | ---------------------------------- |
| Repair Analysis Record   | YES                                |
| Repair Proposal          | YES                                |
| Repair Task Record       | YES                                |
| Revised Artifact Version | CONDITIONAL                        |
| Revalidation Result      | YES when repair modifies artifacts |
| Repair Outcome Record    | YES                                |

### 19.3 Repair Limits

The PoC MUST enforce:

| Parameter                                |       Required Default |
| ---------------------------------------- | ---------------------: |
| Maximum repair iterations per artifact   |                      5 |
| Maximum total repair iterations per plan |                     50 |
| Escalation threshold for same artifact   | 3 consecutive failures |

### 19.4 Repair Rules

A repair cycle MUST reference the validation failure that triggered it.

A repaired artifact MUST create a new version or preserve sufficient supersession metadata.

A repaired artifact MUST be revalidated.

A repair MUST NOT be marked resolved without successful revalidation or valid waiver.

Repair exhaustion MUST escalate or fail the PoC run.

---

## 20.0 Stage 10 — Artifact Finalization

Artifact Finalization promotes valid artifacts into stable repository state.

### 20.1 Required Inputs

| Input                     | Required |
| ------------------------- | -------- |
| Valid artifacts           | YES      |
| Passed validation results | YES      |
| Artifact metadata         | YES      |
| Traceability links        | YES      |
| Governance state          | YES      |

### 20.2 Required Outputs

| Output                        | Required |
| ----------------------------- | -------- |
| Artifact Promotion Records    | YES      |
| Stable Artifact Records       | YES      |
| Repository Integrity Check    | YES      |
| Final Repository State Record | YES      |

### 20.3 Finalization Rules

An artifact MUST NOT be promoted to stable unless validation passed or a valid waiver exists.

An artifact MUST NOT be promoted to stable unless traceability is complete.

An artifact MUST NOT be promoted to stable if repository integrity checks fail.

The PoC MUST produce a stable artifact repository or fail.

---

## 21.0 Stage 11 — Traceability and Evidence Closure

Traceability and Evidence Closure ensures that outputs can be reviewed after the run.

### 21.1 Required Traceability Links

The PoC MUST record links between:

* specification and canonical model
* canonical entities and construction tasks
* construction tasks and agent invocations
* agent invocations and generated artifacts
* generated artifacts and validation results
* failed validation results and repair records
* repaired artifacts and superseded artifacts
* stable artifacts and source entities
* pipeline run and completion report

### 21.2 Evidence Bundle

The PoC MUST produce an evidence bundle containing:

| Evidence                              | Required    |
| ------------------------------------- | ----------- |
| input specification fixture           | YES         |
| parsed specification record           | YES         |
| canonical model record                | YES         |
| readiness/governance admission record | YES         |
| construction plan                     | YES         |
| execution graph                       | YES         |
| agent invocation records              | YES         |
| generated artifact metadata           | YES         |
| validation results                    | YES         |
| repair records, if any                | CONDITIONAL |
| traceability snapshot                 | YES         |
| telemetry summary                     | YES         |
| completion report                     | YES         |

### 21.3 Closure Rules

Traceability closure MUST run before completion reporting.

Missing required traceability MUST fail the PoC.

Missing required evidence MUST fail the PoC.

---

## 22.0 Stage 12 — Completion Reporting

Completion Reporting determines and records the final outcome of the PoC run.

### 22.1 Completion Report Schema

| Field | Type | Required | Description |
|---|---|---|
| poc-completion-report-id | string | REQUIRED | Unique completion report identifier |
| pipeline-run-id | string | REQUIRED | PoC run identifier |
| specification-id | string | REQUIRED | Input system identifier |
| specification-version | semver | REQUIRED | Input specification version |
| target-system | string | REQUIRED | Target system generated |
| final-outcome | enum | REQUIRED | succeeded, failed, escalated, cancelled |
| stages-completed | array | REQUIRED | Completed stages |
| stages-failed | array | CONDITIONAL | Failed stages |
| artifacts-produced | array | REQUIRED | Artifact identifiers |
| stable-artifacts | array | CONDITIONAL | Stable artifact identifiers |
| validation-results | array | REQUIRED | Validation result identifiers |
| repair-count | integer | REQUIRED | Total repair attempts |
| waivers-used | array | CONDITIONAL | Waivers used |
| traceability-snapshot-id | string | REQUIRED | Final traceability snapshot |
| evidence-bundle-id | string | REQUIRED | Evidence bundle |
| telemetry-summary-id | string | REQUIRED | Telemetry summary |
| completed-at | ISO 8601 | REQUIRED | Completion time |

### 22.2 Completion Rules

The PoC final-outcome MUST be succeeded only when all primary success criteria pass.

The PoC final-outcome MUST be failed when a required stage fails without repair, waiver, or valid escalation resolution.

The PoC final-outcome MUST be escalated when human or governance intervention is required and not resolved during the run.

The completion report MUST be persisted.

---

## 23.0 PoC Pipeline Run Record

Each PoC execution MUST produce a Pipeline Run Record.

| Field | Type | Required | Description |
|---|---|---|
| pipeline-run-id | string | REQUIRED | Unique PoC run identifier |
| pipeline-version | semver | REQUIRED | PoC pipeline version |
| input-specification-id | string | REQUIRED | Input specification identifier |
| input-specification-version | semver | REQUIRED | Input specification version |
| target-platform | string | REQUIRED | Target platform |
| target-language | string | REQUIRED | Target language |
| runtime-configuration-id | string | REQUIRED | Runtime configuration used |
| governance-profile-id | string | REQUIRED | Governance profile used |
| model-routing-policy-id | string | CONDITIONAL | Model routing policy used |
| tool-profile-id | string | REQUIRED | Tool profile used |
| artifact-workspace-id | string | REQUIRED | Artifact workspace |
| status | enum | REQUIRED | pending, running, succeeded, failed, escalated, cancelled |
| started-at | ISO 8601 | REQUIRED | Start time |
| completed-at | ISO 8601 | CONDITIONAL | Completion time |

---

## 24.0 Minimum Tool Profile

The PoC MUST define a minimum deterministic tool profile.

### 24.1 Required Tool Capabilities

| Capability                 | Required | Purpose                           |
| -------------------------- | -------- | --------------------------------- |
| compile or build           | YES      | Verify generated source can build |
| unit-test or equivalent    | YES      | Verify generated behavior         |
| format-check               | OPTIONAL | Validate formatting               |
| dependency-resolve         | SHOULD   | Verify dependencies restore       |
| repository-integrity-check | SHOULD   | Verify repository consistency     |

### 24.2 Tool Profile Rules

Tools MUST be registered before use.

Tool versions MUST be recorded.

Tool invocation outcomes MUST be normalized.

Tool evidence MUST be retained.

A missing required tool capability MUST fail execution admission.

---

## 25.0 Minimum Agent Profile

The PoC MUST define the minimum agent roles required.

### 25.1 Required Agent Roles

| Agent Role                | Required | Purpose                                   |
| ------------------------- | -------- | ----------------------------------------- |
| Specification Interpreter | YES      | Interpret input for execution             |
| Construction Planner      | SHOULD   | Assist plan generation or review          |
| Implementation Generator  | YES      | Generate artifacts                        |
| Test Generator            | SHOULD   | Generate tests                            |
| Repair Analyst            | YES      | Analyze validation failures               |
| Review Agent              | SHOULD   | Review generated outputs                  |
| Security Validator        | OPTIONAL | Only required if security policy included |
| Deployment Preparer       | NO       | Not required for first PoC                |

### 25.2 Agent Rules

Required agent roles MUST have role contracts.

Agent invocations MUST produce structured outputs.

Agent outputs MUST be validated before artifact admission.

Agent failures MUST be recorded and handled according to ISL v2.1 and v2.4.

---

## 26.0 Minimum Repository Profile

The PoC artifact repository MAY be local, but it MUST preserve artifact control.

### 26.1 Required Repository Capabilities

The repository MUST support:

* artifact admission
* artifact metadata
* artifact states
* artifact versions
* content storage
* validation evidence references
* traceability references
* stable promotion
* repository integrity check

### 26.2 Repository Rules

Generated files MUST NOT be treated as stable artifacts until promotion.

Artifacts MUST have identifiers.

Artifacts MUST have metadata.

Stable artifacts MUST have validation evidence.

Stable artifacts MUST have traceability links.

---

## 27.0 Minimum Governance Profile

The PoC governance profile MAY be simplified but MUST be explicit.

### 27.1 Required Governance Controls

The PoC MUST support:

* risk tier assignment
* execution authorization
* policy violation response values
* waiver record if validation is waived
* escalation record when repair limits are reached
* audit-support record for execution admission and completion

### 27.2 Governance Rules

The PoC SHOULD use Standard risk tier.

A waiver MUST NOT be silently assumed.

An escalation MUST stop the affected pipeline stage until resolved or recorded as unresolved.

Governance records MAY be local records but MUST be persisted.

---

## 28.0 Minimum Telemetry Profile

The PoC MUST emit telemetry sufficient to understand the run.

### 28.1 Required Telemetry Events

The PoC SHOULD emit telemetry for:

* pipeline run started
* stage started
* stage completed
* stage failed
* specification parsed
* canonical model generated
* construction plan generated
* runtime initialized
* task started
* task completed
* agent invoked
* model invoked
* artifact admitted
* tool invoked
* validation passed
* validation failed
* repair started
* repair completed
* artifact promoted
* traceability snapshot created
* pipeline run completed

### 28.2 Telemetry Rules

Telemetry events MUST include pipeline-run-id.

Telemetry events SHOULD include specification-id and task-id where applicable.

Telemetry MUST NOT replace evidence records.

Telemetry summary MUST be included in the evidence bundle.

---

## 29.0 Success Criteria

This section defines measurable PoC success criteria.

### 29.1 Required Success Criteria

The PoC is successful only if:

| Criterion                                                                  | Required Evidence                     |
| -------------------------------------------------------------------------- | ------------------------------------- |
| Input fixture parsed successfully                                          | Structural Validation Report          |
| Canonical model generated successfully                                     | Canonical Model Record                |
| Construction plan generated and validated                                  | Planning Validation Report            |
| Runtime executed task graph                                                | Execution Graph                       |
| Artifacts generated                                                        | Artifact Metadata Records             |
| Artifacts admitted to repository                                           | Artifact Admission Records            |
| Build or compile validation passed                                         | Tool Invocation Result                |
| Test validation passed                                                     | Test Result or Tool Invocation Result |
| Repair cycle works when triggered or is verified by failure-injection test | Repair Record or Scenario Test        |
| Stable artifacts produced                                                  | Artifact Promotion Records            |
| Traceability closure passed                                                | Traceability Snapshot                 |
| Completion report produced                                                 | PoC Completion Report                 |

### 29.2 Success Rules

A PoC run MUST NOT be considered successful if build validation fails.

A PoC run MUST NOT be considered successful if test validation fails.

A PoC run MUST NOT be considered successful if required traceability is missing.

A PoC run MUST NOT be considered successful if artifacts are generated but not admitted to repository state.

A PoC run MUST NOT be considered successful if the completion report lacks required evidence references.

---

## 30.0 Failure and Escalation Criteria

The PoC must fail clearly when required behavior does not work.

### 30.1 Failure Criteria

The PoC MUST fail when:

* input specification cannot be parsed
* canonical model cannot be generated
* required canonical entities are missing
* construction plan cannot be generated
* planning validation fails
* required agent role is unavailable
* required tool capability is unavailable
* runtime cannot initialize
* generated artifacts are missing
* repository admission fails
* build validation fails after repair exhaustion
* test validation fails after repair exhaustion
* traceability closure fails
* required evidence bundle cannot be produced

### 30.2 Escalation Criteria

The PoC MUST escalate when:

* repair threshold is reached
* governance blocks execution but resolution is possible
* validation failure cannot be automatically classified
* artifact conflict cannot be resolved
* model output repeatedly fails required output contract
* tool output cannot be normalized
* state consistency cannot be confirmed

### 30.3 Failure Rules

Every failure MUST produce a structured error record.

A terminal failure MUST produce a partial evidence bundle when possible.

A failed PoC run SHOULD be reproducible from its pipeline run record and evidence.

---

## 31.0 Failure-Injection Requirements

A PoC that never encounters failure does not prove the repair path. Therefore, the reference implementation SHOULD include at least one failure-injection scenario.

### 31.1 Failure-Injection Scenarios

The PoC SHOULD support one or more of:

* generate artifact with intentional compile error
* generate test that initially fails
* simulate tool timeout
* simulate malformed agent output
* simulate missing artifact metadata
* simulate traceability link omission

### 31.2 Failure-Injection Rules

Failure injection MUST be explicitly configured.

Failure injection MUST NOT contaminate normal successful runs.

A failure-injection run MUST prove that the expected error, repair, retry, escalation, or failure path occurs.

---

## 32.0 Repeatability and Reproducibility

The PoC should be repeatable from the same input.

### 32.1 Repeatability Requirements

The PoC SHOULD record:

* input specification version
* pipeline version
* runtime configuration
* model provider and model version
* tool versions
* repository workspace
* environment information
* random seed where applicable
* generation template versions where applicable

### 32.2 Repeatability Rules

A repeated run SHOULD produce equivalent artifact structure and validation outcomes.

A repeated run MAY produce textually different generated code if the system still passes validation and traceability.

Differences between runs SHOULD be visible through artifact metadata and run comparison records.

---

## 33.0 PoC Run Configuration

Each run SHOULD use an explicit run configuration.

| Field | Type | Required | Description |
|---|---|---|
| poc-run-configuration-id | string | REQUIRED | Unique run configuration identifier |
| input-fixture-location | string | REQUIRED | Input specification fixture |
| target-platform | string | REQUIRED | Target platform |
| target-language | string | REQUIRED | Target language |
| generation-mode | enum | REQUIRED | model-generated, template-assisted, hybrid |
| repair-enabled | boolean | REQUIRED | Whether repair cycles are enabled |
| max-repair-iterations-per-artifact | integer | REQUIRED | Repair limit |
| max-total-repair-iterations | integer | REQUIRED | Total repair limit |
| validation-profile-id | string | REQUIRED | Validation profile |
| governance-profile-id | string | REQUIRED | Governance profile |
| telemetry-profile-id | string | REQUIRED | Telemetry profile |
| failure-injection-profile-id | string | CONDITIONAL | Failure injection profile, if used |

---

## 34.0 PoC Evidence Bundle

The evidence bundle is the reviewable proof of the run.

### 34.1 Evidence Bundle Schema

| Field | Type | Required | Description |
|---|---|---|
| evidence-bundle-id | string | REQUIRED | Unique evidence bundle identifier |
| pipeline-run-id | string | REQUIRED | Associated run |
| specification-record-id | string | REQUIRED | Input specification record |
| canonical-model-id | string | REQUIRED | Canonical model |
| construction-plan-id | string | REQUIRED | Construction plan |
| execution-graph-id | string | REQUIRED | Execution graph |
| artifact-records | array | REQUIRED | Artifact records |
| validation-records | array | REQUIRED | Validation records |
| repair-records | array | CONDITIONAL | Repair records |
| traceability-snapshot-id | string | REQUIRED | Traceability snapshot |
| telemetry-summary-id | string | REQUIRED | Telemetry summary |
| completion-report-id | string | REQUIRED | Completion report |
| created-at | ISO 8601 | REQUIRED | Bundle creation time |

### 34.2 Evidence Rules

The evidence bundle MUST be created for every run, including failed runs where possible.

A successful run MUST have a complete evidence bundle.

The evidence bundle MUST be stored in the artifact repository or evidence store.

---

## 35.0 Pipeline Conformance Requirements

### 35.1 PoC Pipeline Conformance

A PoC pipeline conforms to ISL v3.2 if it:

* accepts a versioned PoC input fixture
* validates specification structure
* produces a canonical model
* evaluates readiness and execution admission
* generates a construction task graph
* initializes runtime execution
* generates source, test, config, and metadata artifacts
* admits artifacts into a repository
* performs deterministic build and test validation
* supports bounded repair cycles
* promotes valid artifacts to stable state
* records traceability links
* emits telemetry
* produces an evidence bundle
* produces a completion report

### 35.2 PoC Implementation Conformance

A reference implementation supports ISL v3.2 if it:

* includes the required PoC fixture
* includes a run configuration
* includes the minimum required components
* includes required tool capabilities
* includes required agent roles
* preserves state across the run
* records structured errors
* supports failure-injection testing or equivalent repair proof
* supports repeatable execution
* can be executed from the repository using documented commands

### 35.3 PoC Success Conformance

A PoC run conforms as successful only if:

* all primary success criteria pass
* no blocking failure remains unresolved
* stable artifacts exist
* validation evidence exists
* traceability closure passes
* completion report references evidence
* the run can be reviewed after completion

---

## 36.0 Summary

ISL v3.2 defines the first Proof-of-Concept Construction Pipeline for the IMHOTEP platform. It turns the original conceptual pipeline into a measurable execution specification for proving the core autonomous construction loop.

This revision strengthens v3.2 by defining the supported input scope, required fixture, minimal target system, minimum platform components, pipeline stages, stage inputs and outputs, task graph requirements, artifact generation expectations, repository admission rules, deterministic validation requirements, bounded repair behavior, finalization rules, traceability closure, telemetry, evidence bundles, success criteria, failure criteria, failure-injection expectations, repeatability, and conformance requirements.

A conforming PoC MUST prove the real loop: specification to canonical model, canonical model to plan, plan to generated artifacts, artifacts to deterministic validation, validation failure to bounded repair, repaired artifacts to revalidation, and stable artifacts to traceable evidence. Anything less is a demonstration, not a proof of concept.
