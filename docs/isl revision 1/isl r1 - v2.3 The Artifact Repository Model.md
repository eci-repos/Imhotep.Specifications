# IMHOTEP Specification Language (ISL) v2.3

# The Artifact Repository Model

**Status:** Normative
**Depends On:** ISL v0.0, ISL v1.1, ISL v1.2, ISL v1.4, ISL v1.5, ISL v1.6, ISL v1.7, ISL v2.0, ISL v2.2
**Supersedes:** ISL r0 v2.3 The Artifact Repository Model
**Document Type:** Artifact Repository, Versioning, and Lifecycle Specification

---

## 1.0 Scope

This document defines the Artifact Repository Model for the IMHOTEP platform. It establishes how generated artifacts are named, stored, versioned, validated, promoted, organized, branched, merged, packaged, retained, queried, governed, and traced throughout the autonomous construction lifecycle.

This document applies to all artifacts produced, modified, repaired, packaged, imported, or retained by the IMHOTEP platform. Artifacts include source code, configuration files, schemas, interface contracts, tests, infrastructure definitions, documentation, operational scripts, deployment manifests, packages, and evidence records.

This document defines:

* artifact repository principles
* artifact categories and types
* artifact identifiers and naming conventions
* artifact metadata schemas
* repository structure and modular organization
* artifact lifecycle states
* artifact admission and promotion rules
* versioning and supersession behavior
* branching strategy
* merge and conflict handling
* artifact validation evidence
* artifact packaging
* access control and subsystem access
* retention and archival requirements
* integrity checks
* repository error classes
* conformance requirements

---

## 2.0 Purpose of the Artifact Repository Model

The purpose of the Artifact Repository Model is to ensure that the tangible outputs of autonomous software construction are managed with the same rigor as the specifications, task graphs, traceability records, and governance decisions that produced them. The repository is the controlled workspace and historical archive where generated implementation artifacts are preserved.

Artifacts are not incidental files. They are lifecycle objects derived from canonical specification entities, construction tasks, agent outputs, deterministic tools, repair cycles, validation results, and governance decisions. The repository therefore must record not only artifact content, but also artifact identity, source, status, validation evidence, version history, traceability associations, and lifecycle state.

The Artifact Repository Model exists to ensure that:

* every generated artifact is identifiable
* every stable artifact is traceable to specification intent
* artifacts are organized according to architectural boundaries
* artifact versions are preserved across construction cycles
* partially generated or failed artifacts do not pollute stable repository state
* repair cycles preserve supersession history
* parallel construction does not corrupt repository state
* merge conflicts are detected and resolved explicitly
* deployment packages are derived only from valid and approved artifacts
* repository history supports audit, recovery, maintenance, and impact analysis

---

## 3.0 Normative References

This section identifies the documents required to interpret and implement the Artifact Repository Model. Artifact management depends on canonical identifiers, execution behavior, traceability relationships, construction planning, deterministic validation, governance, platform architecture, and platform state.

| Reference     | Purpose                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------- |
| ISL v0.0      | Corpus-level principles, conformance, and system integrity rules                                     |
| ISL v1.1      | Canonical entity identifiers and semantic model                                                      |
| ISL v1.2      | Execution phases, artifact generation, validation, repair, consolidation, and deployment preparation |
| ISL v1.4      | Artifact traceability, artifact association, impact analysis, and traceability integrity             |
| ISL v1.5      | Construction tasks, artifact production planning, verification tasks, and planning handoff           |
| ISL v1.6      | Deterministic tool validation, findings, evidence, and tool outcomes                                 |
| ISL v1.7      | Governance controls, waivers, approvals, deployment authorization, and audit records                 |
| ISL v2.0      | Platform architecture and Artifact Repository subsystem responsibility                               |
| ISL v2.2      | Artifact state, repository state, snapshots, recovery, and consistency rules                         |
| W3C PROV-DM   | Provenance alignment for artifact derivation and supersession                                        |
| OpenTelemetry | Artifact lifecycle telemetry alignment                                                               |

---

## 4.0 Terms and Definitions

This section defines artifact repository terminology. These terms are used by the Construction Engine, Artifact Repository, Traceability Engine, State Manager, Governance Engine, Tool Integration Layer, Agent Orchestrator, and Deployment Preparer.

**Artifact**
A tangible output produced, modified, imported, validated, packaged, or retained by the platform.

**Artifact Repository**
The controlled storage environment where artifacts, metadata, versions, validation evidence, package outputs, and repository state are maintained.

**Artifact Metadata**
Structured information describing artifact identity, type, source, version, status, validation state, traceability, repository location, and lifecycle state.

**Stable Artifact**
An artifact that has satisfied required validation, traceability, and governance conditions and is eligible for consolidation, packaging, or deployment preparation.

**Candidate Artifact**
An artifact proposed or generated by an agent, tool, repair cycle, or import process but not yet admitted as stable.

**Repository Workspace**
A working area where artifacts may be generated, modified, validated, repaired, and reviewed before promotion.

**Stable Repository State**
The repository state containing artifacts accepted as valid, traceable, and governed for a specific specification version.

**Artifact Promotion**
The controlled transition of an artifact from candidate, pending, or repaired state into stable repository state.

**Artifact Supersession**
The replacement of one artifact version by a later version while preserving historical traceability.

**Repository Branch**
A version-control or logical isolation line used to separate construction work, repair work, review work, release preparation, or deployment packaging.

**Merge Conflict**
A condition in which two or more changes affect the same artifact, path, metadata record, or package output in incompatible ways.

---

## 5.0 Artifact Repository Design Principles

This section defines the principles governing artifact repository behavior. These principles ensure that generated outputs remain trustworthy, maintainable, auditable, and recoverable.

### 5.1 Artifacts Are First-Class Lifecycle Objects

Artifacts MUST be represented as structured lifecycle objects, not merely files in a directory. The platform MUST maintain metadata, state, version, traceability, validation, and governance information for each artifact.

A file without artifact metadata MUST NOT be treated as a valid platform artifact.

### 5.2 Repository State Is Controlled

The repository MUST distinguish candidate, pending, valid, failed, repaired, escalated, superseded, deprecated, stable, packaged, and archived artifacts.

Generated files MUST NOT automatically enter stable repository state. Stable state requires validation, traceability, and applicable governance conditions.

### 5.3 Traceability Is Mandatory

Every artifact accepted by the repository MUST be traceable to at least one specification entity, construction task, approved inferred derivation, imported source record, or platform lifecycle rule.

An untraced artifact MUST NOT be promoted to stable repository state.

### 5.4 Validation Precedes Promotion

An artifact MUST NOT be promoted to stable state unless required deterministic validation has passed or a valid governance waiver exists.

Reasoning-agent output alone MUST NOT be sufficient for artifact promotion.

### 5.5 Version History Is Preserved

Artifact versions MUST be preserved when artifacts are modified, repaired, superseded, packaged, released, archived, or deprecated.

Repository history MUST support reconstruction of prior stable states.

### 5.6 Parallel Construction Requires Isolation

Parallel construction tasks MUST NOT write directly into the same stable artifact location without isolation, locking, branch separation, or merge control.

The repository MUST prevent concurrent construction from corrupting artifact state.

### 5.7 Repository Operations Are Auditable

Artifact creation, modification, validation, promotion, repair, supersession, merge, packaging, archival, and deletion MUST produce auditable records.

---

## 6.0 Artifact Categories and Types

This section defines standard artifact categories. Artifact categories allow the platform to classify generated outputs, select validation tools, assign repository locations, apply governance rules, and prepare deployment packages.

### 6.1 Standard Artifact Types

| Artifact Type  | Description                                                                        |
| -------------- | ---------------------------------------------------------------------------------- |
| source         | Source code or implementation logic                                                |
| config         | Application, runtime, tool, or environment configuration                           |
| schema         | Data schema, validation schema, or persistence model                               |
| infrastructure | Infrastructure-as-code, environment definition, or resource declaration            |
| contract       | Interface, API, event, message, or integration contract                            |
| test           | Unit, integration, acceptance, contract, security, or operational test             |
| documentation  | Generated system, component, API, operational, or user documentation               |
| script         | Operational, build, migration, deployment, or maintenance script                   |
| package        | Compiled, bundled, containerized, or distributable output                          |
| deployment     | Deployment manifest, release descriptor, environment package, or deployment bundle |
| evidence       | Validation report, scan report, test report, coverage report, or audit evidence    |
| metadata       | Repository, artifact, package, manifest, or traceability metadata                  |

### 6.2 Artifact Type Rules

Every artifact MUST declare exactly one primary artifact type.

An artifact MAY declare secondary classifications in metadata.

Artifact type MUST determine default repository location, validation expectations, packaging eligibility, and lifecycle rules.

Artifact types MAY be extended by approved extensions, but extensions MUST NOT redefine standard artifact type meanings.

---

## 7.0 Artifact Identifier and Naming Model

This section defines artifact identifiers and naming conventions. Artifact names and identifiers are required for traceability, repository organization, audit, merge control, package assembly, and impact analysis.

Artifact identity MUST be stable even when repository paths change.

### 7.1 Artifact Identifier Format

Artifact identifiers MUST follow the traceability object identifier pattern:

`ART-{system-id}-{sequence}`

Where:

| Component | Description                                           |
| --------- | ----------------------------------------------------- |
| ART       | Artifact object prefix                                |
| system-id | Normalized system identifier                          |
| sequence  | Zero-padded sequence unique within artifact namespace |

Example:

`ART-acme-order-00042`

### 7.2 Artifact File Naming Convention

Repository file names SHOULD follow the following convention unless implementation language or ecosystem conventions require another form:

`{semantic-name}.{artifact-role}.{extension}`

Examples:

* `order-service.controller.cs`
* `order.schema.json`
* `order-api.contract.yaml`
* `order-service.tests.cs`
* `deployment.manifest.yaml`

### 7.3 Repository Path Naming Convention

Repository paths SHOULD follow this form:

`/{domain}/{module-or-service}/{artifact-category}/{artifact-name}`

Example:

`/services/order-service/source/order-service.controller.cs`

### 7.4 Naming Rules

Artifact identifiers MUST NOT depend on file path.

File names SHOULD be lowercase or ecosystem-standard.

Repository paths MUST be stable enough to support traceability and impact analysis.

A path rename MUST preserve artifact identifier and version history.

Two active artifacts MUST NOT share the same repository path in the same repository workspace unless the path is intentionally versioned or environment-scoped.

Generated artifact names MUST be deterministic for equivalent specification input, planning configuration, and artifact naming policy.

---

## 8.0 Artifact Metadata Model

This section defines required artifact metadata. Metadata is the primary mechanism through which the repository connects artifact content to specification intent, construction tasks, validation evidence, repository state, and governance decisions.

### 8.1 Artifact Metadata Schema

Every artifact MUST have an Artifact Metadata Record.

| Field | Type | Required | Description |
|---|---|---|
| artifact-id | string | REQUIRED | Unique artifact identifier |
| artifact-name | string | REQUIRED | Human-readable or file-oriented artifact name |
| artifact-type | enum | REQUIRED | Artifact type from §6.1 |
| artifact-version | semver | REQUIRED | Artifact version |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version that produced or governs artifact |
| canonical-model-version | semver | CONDITIONAL | Canonical model version used |
| task-id | string | REQUIRED | Construction task that produced or modified artifact |
| source-entity-ids | array | REQUIRED | Specification entities or approved source references |
| derivation-type | enum | REQUIRED | direct, derived, inferred, repair, imported, packaged |
| repository-location | string | REQUIRED | Logical or physical repository location |
| branch-id | string | CONDITIONAL | Branch or workspace containing artifact |
| content-hash | string | CONDITIONAL | Integrity hash when supported |
| created-at | ISO 8601 | REQUIRED | Artifact creation time |
| created-by | string | REQUIRED | Agent, tool, subsystem, or import process creating artifact |
| updated-at | ISO 8601 | REQUIRED | Last update time |
| current-state | enum | REQUIRED | Artifact lifecycle state from §10.1 |
| validation-status | enum | REQUIRED | not-evaluated, passed, failed, warning, waived |
| traceability-status | enum | REQUIRED | complete, incomplete, inconsistent |
| governance-status | enum | REQUIRED | not-required, pending, approved, waived, blocked |
| supersedes-artifact-id | string | CONDITIONAL | Prior artifact replaced |
| superseded-by-artifact-id | string | CONDITIONAL | Later artifact replacing this artifact |
| retention-class | enum | REQUIRED | transient, operational, audit, archival |

### 8.2 Metadata Rules

Artifact metadata MUST be created before or at the time artifact content is admitted to the repository.

Artifact metadata MUST be updated when artifact state changes.

Artifact metadata MUST NOT be silently overwritten.

A metadata change affecting validation, traceability, governance, repository location, version, or state MUST emit a state event.

---

## 9.0 Repository Structure and Modularity

This section defines repository organization. The repository structure should reflect the logical and architectural structure of the generated system while remaining predictable enough for automation, review, and maintenance.

The repository is both a machine-controlled workspace and a human-readable software engineering artifact.

### 9.1 Standard Repository Layout

A conforming implementation SHOULD organize repository content using the following top-level structure.

| Path              | Purpose                                                                     |
| ----------------- | --------------------------------------------------------------------------- |
| `/specification`  | Authored specification references, snapshots, or exported canonical context |
| `/canonical`      | Canonical model exports or references                                       |
| `/services`       | Service-specific implementation artifacts                                   |
| `/data`           | Data entities, schemas, migrations, seed data, or data contracts            |
| `/interfaces`     | API, event, message, file, or integration contracts                         |
| `/workflows`      | Workflow definitions or generated workflow support artifacts                |
| `/tests`          | Test artifacts and test configuration                                       |
| `/infrastructure` | Infrastructure and environment artifacts                                    |
| `/deployment`     | Deployment manifests, release bundles, and deployment descriptors           |
| `/docs`           | Generated documentation                                                     |
| `/scripts`        | Operational, build, migration, or utility scripts                           |
| `/evidence`       | Validation, test, scan, policy, and audit evidence                          |
| `/metadata`       | Artifact metadata, manifests, traceability exports, and repository indexes  |

### 9.2 Service-Level Layout

Service artifacts SHOULD be organized as:

| Path                                 | Purpose                       |
| ------------------------------------ | ----------------------------- |
| `/services/{service-name}/source`    | Service implementation source |
| `/services/{service-name}/config`    | Service configuration         |
| `/services/{service-name}/tests`     | Service tests                 |
| `/services/{service-name}/contracts` | Service-specific contracts    |
| `/services/{service-name}/docs`      | Service documentation         |
| `/services/{service-name}/metadata`  | Service artifact metadata     |

### 9.3 Layout Rules

Repository layout MUST be deterministic based on artifact type, service/module ownership, and repository policy.

A repository MAY use ecosystem-native layout conventions when required, but it MUST preserve artifact metadata and traceability.

Artifacts belonging to distinct services or modules SHOULD be isolated to support parallel construction and impact-based regeneration.

Repository layout MUST support bidirectional navigation from artifact metadata to file location and from file location to artifact metadata.

---

## 10.0 Artifact Lifecycle State Model

This section defines artifact lifecycle states and transitions. Artifact lifecycle state determines whether an artifact may be validated, repaired, promoted, consolidated, packaged, deployed, archived, or deleted.

### 10.1 Artifact Lifecycle States

| State      | Meaning                                                                           |
| ---------- | --------------------------------------------------------------------------------- |
| candidate  | Artifact proposed by an agent or import but not yet admitted                      |
| pending    | Artifact admitted to working repository state but not fully validated             |
| valid      | Artifact passed required validation                                               |
| failed     | Artifact failed validation or repository integrity checks                         |
| repaired   | Artifact was modified through repair and requires or has completed revalidation   |
| escalated  | Artifact requires governance, human review, or manual intervention                |
| waived     | Artifact has unresolved finding accepted by valid governance waiver               |
| stable     | Artifact is valid or waived, traceable, and accepted into stable repository state |
| packaged   | Artifact is included in a package or deployment bundle                            |
| superseded | Artifact has been replaced by a later version                                     |
| deprecated | Artifact remains retained but SHOULD NOT be used for new construction             |
| archived   | Artifact is no longer active but retained according to retention policy           |
| deleted    | Artifact content removed according to approved retention and deletion policy      |

### 10.2 Permitted State Transitions

| From       | To         | Condition                                                 |
| ---------- | ---------- | --------------------------------------------------------- |
| candidate  | pending    | Repository admission accepted                             |
| candidate  | rejected   | Admission rejected                                        |
| pending    | valid      | Required validation passed                                |
| pending    | failed     | Required validation failed                                |
| pending    | escalated  | Governance or human review required                       |
| failed     | repaired   | Repair task modifies artifact                             |
| repaired   | valid      | Revalidation passed                                       |
| repaired   | failed     | Revalidation failed                                       |
| failed     | escalated  | Repair limit, policy, or unresolved failure               |
| valid      | stable     | Traceability complete and governance conditions satisfied |
| waived     | stable     | Waiver valid and traceability complete                    |
| stable     | packaged   | Artifact included in package                              |
| stable     | superseded | Later artifact replaces it                                |
| stable     | deprecated | Governance or specification evolution marks obsolete      |
| superseded | archived   | Retention policy archives artifact                        |
| deprecated | archived   | Retention policy archives artifact                        |
| archived   | deleted    | Deletion permitted by retention policy                    |
| escalated  | pending    | Escalation resolved and retry authorized                  |
| escalated  | failed     | Escalation closed as failure                              |
| escalated  | waived     | Waiver granted                                            |

A `rejected` state MAY be implemented as a terminal admission state. Rejected artifacts MUST NOT be promoted.

### 10.3 Lifecycle Rules

An artifact MUST NOT enter stable state unless validation-status is passed or waived.

An artifact MUST NOT enter stable state unless traceability-status is complete.

An artifact MUST NOT enter packaged state unless it is stable or explicitly approved by governance for non-deployable packaging.

An artifact in failed state MUST NOT be packaged for deployment.

An artifact in superseded, deprecated, archived, or deleted state MUST preserve metadata required for historical traceability.

---

## 11.0 Artifact Admission

This section defines artifact admission. Admission is the controlled process of accepting a candidate artifact into repository working state.

Admission does not mean the artifact is valid. It means the repository recognizes and controls the artifact.

### 11.1 Admission Sources

Artifacts MAY be admitted from:

* Implementation Generator output
* Test Generator output
* Deployment Preparer output
* Tool-generated output
* Repair task output
* Imported external artifact
* Human-authored artifact registered through governance
* Platform-generated metadata or evidence

### 11.2 Admission Record Schema

| Field | Type | Required | Description |
|---|---|---|
| admission-id | string | REQUIRED | Unique admission identifier |
| artifact-id | string | REQUIRED | Artifact admitted |
| admission-source | enum | REQUIRED | agent, tool, repair, import, human, platform |
| source-reference | string | REQUIRED | Output, invocation, task, import, or user reference |
| task-id | string | CONDITIONAL | Required when task-produced |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| repository-location | string | REQUIRED | Target repository location |
| admission-outcome | enum | REQUIRED | admitted, rejected, escalated |
| admission-findings | array | CONDITIONAL | Findings from admission checks |
| admitted-at | ISO 8601 | REQUIRED | Admission time |
| admitted-by | string | REQUIRED | Runtime, repository service, or authorized role |

### 11.3 Admission Checks

The repository MUST check:

* artifact identifier validity
* artifact type validity
* metadata completeness
* source task or source reference validity
* repository location availability
* path collision risk
* traceability source presence
* content availability
* governance constraints
* malware or unsafe content checks where required by policy

### 11.4 Admission Rules

A candidate artifact MUST NOT become pending unless admission checks pass.

An artifact with missing metadata MUST be rejected or escalated.

An artifact with no source reference MUST be rejected unless it is imported under governance-controlled registration.

An artifact with path collision MUST enter conflict handling.

---

## 12.0 Artifact Promotion

This section defines artifact promotion. Promotion moves artifacts from working repository state into stable state, package-ready state, or release-ready state.

Promotion is a governed lifecycle transition that depends on validation, traceability, artifact state, and policy conditions.

### 12.1 Promotion Preconditions

An artifact MAY be promoted to stable only when:

* artifact metadata is complete
* artifact state is valid or waived
* validation-status is passed or waived
* traceability-status is complete
* governance-status is approved, waived, or not-required
* repository integrity checks pass
* no unresolved merge conflict exists
* content hash or equivalent integrity marker is recorded where supported

### 12.2 Promotion Record Schema

| Field | Type | Required | Description |
|---|---|---|
| promotion-id | string | REQUIRED | Unique promotion identifier |
| artifact-id | string | REQUIRED | Artifact promoted |
| from-state | enum | REQUIRED | Prior artifact state |
| to-state | enum | REQUIRED | New artifact state |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| validation-results | array | REQUIRED | Validation evidence supporting promotion |
| traceability-snapshot-id | string | CONDITIONAL | Traceability evidence |
| governance-records | array | CONDITIONAL | Approvals, waivers, or policy checks |
| promoted-at | ISO 8601 | REQUIRED | Promotion time |
| promoted-by | string | REQUIRED | Runtime, repository service, or authority |

### 12.3 Promotion Rules

Promotion MUST emit an artifact-state-changed event.

Promotion MUST update artifact metadata.

Promotion MUST be traceable to validation and governance evidence.

Promotion to stable MUST be blocked when required evidence is missing.

---

## 13.0 Artifact Versioning and Supersession

This section defines artifact versioning. Artifact versioning preserves the evolution of implementation outputs across construction cycles, repairs, specification changes, and deployment packages.

### 13.1 Artifact Versioning Rules

Each artifact MUST have an artifact-version.

Artifact versions MUST use semantic versioning unless an ecosystem-specific versioning scheme is explicitly mapped.

A semantic change to artifact behavior SHOULD increment minor or major version depending on compatibility.

A non-behavioral correction MAY increment patch version.

A repair that changes artifact content MUST create a new artifact version or preserve prior content hash and repair history.

### 13.2 Supersession Rules

When one artifact replaces another, the new artifact MUST reference `supersedes-artifact-id`.

The replaced artifact MUST reference `superseded-by-artifact-id`.

Supersession MUST create a traceability edge.

Superseded artifacts MUST remain queryable.

A superseded artifact MUST NOT be used for new packaging unless a rollback or governance-approved reconstruction requires it.

### 13.3 Artifact Version Record Schema

| Field | Type | Required | Description |
|---|---|---|
| artifact-version-record-id | string | REQUIRED | Unique version record identifier |
| artifact-id | string | REQUIRED | Artifact identifier |
| artifact-version | semver | REQUIRED | Artifact version |
| prior-artifact-version | semver | CONDITIONAL | Prior version |
| version-change-type | enum | REQUIRED | initial, patch, minor, major, repair, supersession, rollback |
| content-hash | string | CONDITIONAL | Content hash for this version |
| repository-location | string | REQUIRED | Repository location |
| changed-by | string | REQUIRED | Agent, tool, runtime, or role causing change |
| change-reason | string | REQUIRED | Reason for version creation |
| created-at | ISO 8601 | REQUIRED | Version creation time |

---

## 14.0 Version Control Integration

This section defines integration with version control systems. Version control MAY be Git or another system that supports equivalent repository history, branching, merging, and commit identity.

The repository model does not depend exclusively on Git, but any version control integration MUST preserve the required artifact semantics.

### 14.1 Version Control Requirements

Version control integration SHOULD support:

* commit history
* branch isolation
* merge operations
* tags or release markers
* diff inspection
* rollback or checkout
* author or actor attribution
* content hashing or integrity checks

### 14.2 Commit Metadata Requirements

Each commit or equivalent version-control change SHOULD include:

| Field                    | Description                            |
| ------------------------ | -------------------------------------- |
| specification-id         | System identifier                      |
| specification-version    | Specification version                  |
| plan-id                  | Construction plan identifier           |
| task-id                  | Task causing change                    |
| artifact-ids             | Artifacts affected                     |
| agent-or-tool-id         | Agent, model, or tool causing change   |
| validation-status        | Validation status at commit time       |
| traceability-snapshot-id | Traceability snapshot where applicable |
| governance-record-id     | Governance record where applicable     |

### 14.3 Version Control Rules

Version control commits MUST NOT substitute for artifact metadata.

Artifact metadata MUST remain authoritative for artifact lifecycle state.

Version control history SHOULD be linked to artifact version records.

A commit that modifies artifacts without corresponding artifact metadata updates MUST produce a repository integrity finding.

---

## 15.0 Branching Strategy

This section defines required branching behavior for autonomous construction. Branching isolates generated changes, repairs, reviews, release preparation, and deployment packaging.

Branching strategy is required because autonomous construction may involve parallel agents, repair cycles, and concurrent task execution.

### 15.1 Standard Branch Types

| Branch Type  | Purpose                                                |
| ------------ | ------------------------------------------------------ |
| main         | Stable accepted repository state                       |
| construction | Active artifact generation for a specification version |
| task         | Isolated work for a construction task                  |
| repair       | Isolated repair attempt for a failed artifact          |
| review       | Human or agent review workspace                        |
| release      | Release or package preparation                         |
| hotfix       | Urgent correction to stable or released artifact       |
| experiment   | Non-production exploratory generation                  |

### 15.2 Branch Naming Convention

Branches SHOULD follow this format:

`{branch-type}/{specification-id}/{specification-version}/{task-or-purpose}`

Examples:

* `construction/acme-order/1.2.0/full-plan`
* `task/acme-order/1.2.0/TSK-acme-order-00018`
* `repair/acme-order/1.2.0/RPR-acme-order-00003`
* `release/acme-order/1.2.0/prod-candidate`

### 15.3 Branch Rules

Stable artifacts MUST reside in main or a governed stable-equivalent branch.

Construction work MUST occur outside main until promotion.

Repair work SHOULD occur in repair branches.

Task-level parallel work SHOULD occur in task branches or isolated workspaces.

A branch MUST reference specification-id and specification-version.

A branch merge into main MUST require promotion checks.

High and Critical risk systems SHOULD require governance approval before merging release branches into main.

---

## 16.0 Merge and Conflict Handling

This section defines merge and conflict handling. Merge conflicts must be treated as controlled lifecycle events, not developer inconvenience.

Autonomous construction can create concurrent changes, and those changes must not corrupt repository state.

### 16.1 Conflict Types

| Conflict Type         | Description                                              |
| --------------------- | -------------------------------------------------------- |
| path-conflict         | Two artifacts attempt to occupy the same repository path |
| content-conflict      | Concurrent changes modify overlapping content            |
| metadata-conflict     | Artifact metadata differs across branches                |
| version-conflict      | Artifact versions are incompatible or non-monotonic      |
| traceability-conflict | Artifact traceability differs or becomes inconsistent    |
| validation-conflict   | Conflicting validation status exists for same artifact   |
| governance-conflict   | Governance decision blocks or conflicts with merge       |
| package-conflict      | Package composition differs between branches             |
| deletion-conflict     | One branch deletes an artifact modified by another       |

### 16.2 Merge Conflict Record Schema

| Field | Type | Required | Description |
|---|---|---|
| conflict-id | string | REQUIRED | Unique merge conflict identifier |
| conflict-type | enum | REQUIRED | Conflict type from §16.1 |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| source-branch-id | string | REQUIRED | Branch being merged |
| target-branch-id | string | REQUIRED | Branch receiving merge |
| artifact-ids | array | REQUIRED | Artifacts affected |
| task-ids | array | CONDITIONAL | Tasks causing conflict |
| conflict-description | string | REQUIRED | Explanation of conflict |
| severity | enum | REQUIRED | blocking, high, medium, low |
| detected-at | ISO 8601 | REQUIRED | Detection time |
| resolution-status | enum | REQUIRED | open, resolved, escalated, rejected |
| resolution-record-id | string | CONDITIONAL | Resolution record when resolved |

### 16.3 Conflict Resolution Rules

A blocking conflict MUST prevent merge.

A metadata conflict MUST be resolved before artifact promotion.

A traceability conflict MUST be resolved before stable state.

A validation conflict MUST be resolved in favor of failed or warning unless governance approves waiver or evidence proves the passing result applies to the current artifact version.

A governance conflict MUST be routed to the Governance Engine.

A content conflict MAY be resolved by agent-assisted merge only if deterministic validation is required afterward.

### 16.4 Merge Resolution Record Schema

| Field | Type | Required | Description |
|---|---|---|
| resolution-record-id | string | REQUIRED | Unique resolution identifier |
| conflict-id | string | REQUIRED | Conflict resolved |
| resolution-method | enum | REQUIRED | automated, agent-assisted, human-review, governance-decision, reject-merge |
| resolved-artifact-ids | array | REQUIRED | Artifacts resolved |
| resolution-summary | string | REQUIRED | Explanation of resolution |
| validation-required | boolean | REQUIRED | Whether validation must run after merge |
| governance-record-id | string | CONDITIONAL | Governance decision if required |
| resolved-by | string | REQUIRED | Agent, runtime, tool, or authority resolving conflict |
| resolved-at | ISO 8601 | REQUIRED | Resolution time |

### 16.5 Post-Merge Validation

Artifacts affected by merge conflict resolution MUST be revalidated unless governance explicitly records why validation is not applicable.

A merge into stable repository state MUST NOT complete while affected artifacts are pending, failed, or traceability-incomplete.

---

## 17.0 Repository State and Consistency

This section defines repository state and consistency. Repository state must agree with artifact metadata, task state, validation state, traceability state, and governance state.

### 17.1 Repository State Schema

| Field | Type | Required | Description |
|---|---|---|
| repository-state-id | string | REQUIRED | Unique repository state identifier |
| repository-id | string | REQUIRED | Repository identifier |
| specification-id | string | CONDITIONAL | System identifier when scoped |
| specification-version | semver | CONDITIONAL | Specification version when scoped |
| active-branch-id | string | CONDITIONAL | Current branch or workspace |
| stable-branch-id | string | REQUIRED | Stable branch reference |
| repository-status | enum | REQUIRED | initialized, active, locked, inconsistent, recovering, archived |
| artifact-count | integer | REQUIRED | Number of tracked artifacts |
| pending-conflicts | array | CONDITIONAL | Open conflict identifiers |
| last-integrity-check-id | string | CONDITIONAL | Last repository integrity check |
| updated-at | ISO 8601 | REQUIRED | Last repository state update |

### 17.2 Repository Consistency Rules

Repository state MUST match artifact metadata records.

An artifact file present without metadata MUST produce an unmanaged-artifact finding.

Metadata for missing artifact content MUST produce a missing-artifact finding.

Stable branch MUST contain only stable, packaged, deprecated, superseded, or archived artifacts unless governance permits another state.

Pending, failed, or escalated artifacts MUST NOT appear as stable outputs.

### 17.3 Repository Locking

The repository MAY lock branches, paths, artifacts, or packages during high-risk operations.

Locking SHOULD occur during:

* promotion to stable
* merge into main
* release packaging
* deployment package creation
* repository recovery
* destructive cleanup
* archive transition

A lock MUST record owner, scope, start time, and expiry or release condition.

---

## 18.0 Artifact Validation Evidence

This section defines how validation evidence is associated with artifacts. Validation evidence proves that artifacts satisfy required checks and supports promotion, packaging, audit, and deployment authorization.

### 18.1 Evidence Types

Artifact evidence MAY include:

* compilation results
* unit test results
* integration test results
* acceptance test results
* contract validation reports
* schema validation reports
* static analysis reports
* security scan reports
* dependency audit reports
* infrastructure validation reports
* deployment dry-run reports
* governance review records

### 18.2 Artifact Evidence Record Schema

| Field | Type | Required | Description |
|---|---|---|
| evidence-record-id | string | REQUIRED | Unique evidence identifier |
| artifact-id | string | REQUIRED | Artifact supported by evidence |
| artifact-version | semver | REQUIRED | Artifact version evaluated |
| evidence-type | enum | REQUIRED | compile, test, scan, analysis, policy, infrastructure, deployment, review |
| validation-result-id | string | CONDITIONAL | Validation result associated with evidence |
| tool-invocation-id | string | CONDITIONAL | Tool invocation producing evidence |
| governance-record-id | string | CONDITIONAL | Governance evidence if applicable |
| evidence-location | string | REQUIRED | Repository or evidence store location |
| evidence-hash | string | CONDITIONAL | Integrity hash when supported |
| outcome | enum | REQUIRED | passed, failed, warning, waived, not-applicable |
| created-at | ISO 8601 | REQUIRED | Evidence creation time |

### 18.3 Evidence Rules

A stable artifact MUST have evidence supporting validation-status passed or waived.

Evidence records MUST reference the artifact version evaluated.

Evidence for one artifact version MUST NOT be reused for another version unless impact analysis proves applicability and governance permits reuse.

Security evidence for High or Critical systems MUST be retained according to governance retention rules.

---

## 19.0 Artifact Packaging

This section defines artifact packaging. Packaging creates distributable or deployable outputs from stable artifacts and deployment preparation records.

Packaging does not itself authorize deployment. Deployment authorization remains governed by ISL v1.7.

### 19.1 Package Types

| Package Type          | Description                                                        |
| --------------------- | ------------------------------------------------------------------ |
| build-package         | Compiled or bundled application output                             |
| container-image       | Containerized runtime image                                        |
| library-package       | Reusable library or package artifact                               |
| deployment-bundle     | Set of deployment manifests, packages, and environment descriptors |
| documentation-package | Documentation bundle                                               |
| evidence-package      | Validation, governance, and traceability evidence bundle           |
| release-candidate     | Candidate package for release or deployment authorization          |

### 19.2 Package Manifest Schema

| Field | Type | Required | Description |
|---|---|---|
| package-id | string | REQUIRED | Unique package identifier |
| package-type | enum | REQUIRED | Package type from §19.1 |
| package-version | semver | REQUIRED | Package version |
| specification-id | string | REQUIRED | System identifier |
| specification-version | semver | REQUIRED | Specification version |
| artifact-ids | array | REQUIRED | Artifacts included |
| artifact-versions | array | REQUIRED | Artifact versions included |
| build-task-id | string | CONDITIONAL | Task that produced package |
| validation-results | array | REQUIRED | Validation results supporting package |
| traceability-snapshot-id | string | REQUIRED | Traceability snapshot for package |
| governance-records | array | CONDITIONAL | Approvals or waivers affecting package |
| package-location | string | REQUIRED | Repository or registry location |
| package-hash | string | CONDITIONAL | Package integrity hash when supported |
| created-at | ISO 8601 | REQUIRED | Package creation time |
| package-status | enum | REQUIRED | candidate, validated, rejected, authorized, deployed, archived |

### 19.3 Packaging Rules

Only stable artifacts MAY be included in deployment-capable packages unless governance explicitly permits otherwise.

A package MUST include a manifest.

A package MUST reference exact artifact versions.

A package MUST be traceable to specification version and traceability snapshot.

A package MUST NOT be marked authorized unless deployment authorization exists where required.

---

## 20.0 Repository Access by Platform Components

This section defines repository access by platform components. Multiple subsystems need access to artifacts, but access must remain controlled, scoped, and traceable.

### 20.1 Standard Access Roles

| Component              | Access Need                                                           |
| ---------------------- | --------------------------------------------------------------------- |
| Construction Engine    | Create and update candidate and pending artifacts                     |
| Agent Orchestrator     | Retrieve task-relevant artifacts and submit candidate outputs         |
| Tool Integration Layer | Read artifacts for validation and write evidence or generated outputs |
| Traceability Engine    | Read metadata and create artifact traceability links                  |
| Governance Engine      | Read artifact metadata, evidence, and policy-relevant content         |
| Execution Monitor      | Read artifact lifecycle telemetry and status                          |
| Deployment Preparer    | Read stable artifacts and write deployment candidates                 |
| State Manager          | Read and update artifact state records                                |
| Human Reviewer         | Read artifacts and metadata within authorized scope                   |

### 20.2 Access Request Schema

| Field | Type | Required | Description |
|---|---|---|
| access-request-id | string | REQUIRED | Unique access request identifier |
| requester-id | string | REQUIRED | Component, agent, tool, or user |
| requester-type | enum | REQUIRED | subsystem, agent, tool, governance-role, user |
| requested-action | enum | REQUIRED | read, write, update-metadata, promote, merge, package, archive, delete |
| artifact-ids | array | CONDITIONAL | Target artifacts |
| repository-paths | array | CONDITIONAL | Target paths |
| branch-id | string | CONDITIONAL | Target branch |
| purpose | string | REQUIRED | Reason for access |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| requested-at | ISO 8601 | REQUIRED | Request time |

### 20.3 Access Rules

Repository access MUST enforce identity, role, task, branch, and governance constraints.

Agents MUST NOT write directly to stable repository state.

Tools MUST write outputs only to approved workspace or evidence locations.

Delete, archive, merge-to-main, package, and promote operations MUST be governed.

Access to restricted artifacts MUST honor sensitivity and governance policies.

---

## 21.0 External and Imported Artifacts

This section defines how externally created or human-authored artifacts are admitted. Enterprise systems may require existing code, schemas, contracts, documentation, or infrastructure assets to be incorporated into the repository.

Imported artifacts must not bypass traceability and governance.

### 21.1 Import Record Schema

| Field | Type | Required | Description |
|---|---|---|
| import-id | string | REQUIRED | Unique import identifier |
| artifact-id | string | REQUIRED | Artifact assigned to imported content |
| import-source | string | REQUIRED | Source repository, file, package, or system |
| import-reason | string | REQUIRED | Reason for import |
| imported-by | string | REQUIRED | User, role, or subsystem performing import |
| imported-at | ISO 8601 | REQUIRED | Import time |
| source-license | string | CONDITIONAL | License or usage condition when applicable |
| source-integrity-hash | string | CONDITIONAL | Integrity hash of imported content |
| source-entity-ids | array | CONDITIONAL | Specification entities connected to import |
| governance-record-id | string | CONDITIONAL | Required when governance approval applies |
| validation-required | boolean | REQUIRED | Whether imported artifact must be validated |

### 21.2 Import Rules

Imported artifacts MUST receive artifact identifiers.

Imported artifacts MUST receive artifact metadata.

Imported artifacts MUST be validated before stable promotion unless governance records a valid exception.

Imported artifacts with no specification association MUST include a justification and governance approval before stable promotion.

Imported third-party artifacts SHOULD include license or provenance metadata.

---

## 22.0 Repository Integrity Checks

This section defines repository integrity checks. Integrity checks verify that repository content, artifact metadata, traceability, validation evidence, and governance state agree.

### 22.1 Required Integrity Checks

The platform MUST check:

* artifact files have metadata
* metadata references existing artifact content
* artifact identifiers are valid
* repository paths are unique within branch scope
* artifact states are valid
* stable artifacts have complete traceability
* stable artifacts have validation evidence
* package manifests reference existing artifact versions
* branch merge records exist for stable merges
* supersession relationships are bidirectional
* archived artifacts remain discoverable by metadata
* governance-controlled operations have governance records

### 22.2 Integrity Check Record Schema

| Field | Type | Required | Description |
|---|---|---|
| repository-integrity-check-id | string | REQUIRED | Unique integrity check identifier |
| repository-id | string | REQUIRED | Repository checked |
| branch-id | string | CONDITIONAL | Branch checked |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| check-scope | enum | REQUIRED | artifact, branch, package, traceability, metadata, full-repository |
| outcome | enum | REQUIRED | passed, failed, warning |
| findings | array | CONDITIONAL | Integrity findings |
| checked-at | ISO 8601 | REQUIRED | Time checked |
| checked-by | string | REQUIRED | Component performing check |

### 22.3 Integrity Failure Rules

A failed integrity check MUST block promotion, packaging, deployment preparation, or execution completion when the failed check affects those actions.

An unmanaged artifact in stable branch MUST be treated as blocking.

A missing artifact referenced by metadata MUST be treated as blocking.

A traceability-incomplete stable artifact MUST be treated as blocking.

---

## 23.0 Retention, Archival, and Deletion

This section defines retention and archival behavior. Artifact repositories accumulate many versions, evidence records, packages, and intermediate outputs. Retention policy must preserve auditability while allowing efficient repository operation.

### 23.1 Retention Classes

| Retention Class   | Meaning                                                                         |
| ----------------- | ------------------------------------------------------------------------------- |
| transient         | Temporary working artifact, may be removed after task completion if not needed  |
| operational       | Required for active development, repair, validation, or deployment preparation  |
| audit             | Required for governance, compliance, traceability, or historical reconstruction |
| archival          | Retained long-term but not active                                               |
| deletion-approved | Approved for deletion under retention policy                                    |

### 23.2 Minimum Retention Rules

Artifacts that reached stable state MUST be retained while the generated system remains active.

Artifacts included in deployment packages MUST be retained with package manifest and evidence.

Artifacts associated with High or Critical risk systems SHOULD be retained under enhanced integrity controls.

Validation evidence supporting stable or deployed artifacts MUST be retained.

Superseded artifacts MUST retain metadata and traceability even if content is archived.

### 23.3 Archival Record Schema

| Field | Type | Required | Description |
|---|---|---|
| archival-record-id | string | REQUIRED | Unique archival record identifier |
| artifact-id | string | REQUIRED | Artifact archived |
| artifact-version | semver | REQUIRED | Version archived |
| archive-location | string | REQUIRED | Storage location |
| archive-reason | string | REQUIRED | Reason for archival |
| retention-class | enum | REQUIRED | Retention class |
| archived-at | ISO 8601 | REQUIRED | Archival time |
| archived-by | string | REQUIRED | Subsystem or role archiving |
| integrity-hash | string | CONDITIONAL | Archive integrity marker |

### 23.4 Deletion Rules

Artifact deletion MUST be governed by retention policy.

Deletion MUST NOT remove records required for audit, traceability, legal, regulatory, deployment, or reconstruction requirements.

Deletion MUST produce a deletion record.

Deleted artifact metadata MUST remain discoverable when required for audit.

---

## 24.0 Repository Snapshots and Recovery

This section defines repository snapshots and recovery. Repository state must be recoverable after failed construction, merge conflict, interrupted promotion, repository corruption, or execution crash.

### 24.1 Snapshot Triggers

Repository snapshots SHOULD be created:

* before merge into stable branch
* before artifact promotion
* before package creation
* before deployment preparation
* before destructive cleanup
* after artifact consolidation
* at execution completion
* before and after major repair waves
* when governance requires audit preservation

### 24.2 Repository Snapshot Schema

| Field | Type | Required | Description |
|---|---|---|
| repository-snapshot-id | string | REQUIRED | Unique repository snapshot identifier |
| repository-id | string | REQUIRED | Repository captured |
| branch-id | string | CONDITIONAL | Branch captured |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| artifact-ids | array | REQUIRED | Artifacts included |
| metadata-version | string | REQUIRED | Metadata schema or version |
| snapshot-location | string | REQUIRED | Snapshot storage location |
| snapshot-purpose | enum | REQUIRED | promotion, merge, package, deployment, recovery, audit |
| integrity-hash | string | CONDITIONAL | Snapshot integrity marker |
| created-at | ISO 8601 | REQUIRED | Snapshot time |

### 24.3 Recovery Rules

Repository recovery MUST compare repository content, artifact metadata, state records, and traceability graph.

Recovery MUST NOT mark artifacts stable unless required validation and traceability evidence remains intact.

If repository content and metadata disagree, the platform MUST enter recovering or inconsistent state.

Recovery actions MUST be recorded.

---

## 25.0 Artifact Impact Analysis

This section defines artifact-level impact analysis. Impact analysis determines which artifacts must be regenerated, revalidated, repaired, repackaged, or retired when specification entities, task plans, dependencies, tools, policies, or infrastructure expectations change.

### 25.1 Impact Triggers

Artifact impact analysis MUST occur when:

* a source specification entity changes
* an interface contract changes
* a data schema changes
* a policy rule changes
* an infrastructure expectation changes
* a tool version or validation profile changes materially
* a dependency changes
* a package composition changes
* a branch merge modifies artifact content
* a repair supersedes an artifact

### 25.2 Artifact Impact Result Schema

| Field | Type | Required | Description |
|---|---|---|
| artifact-impact-id | string | REQUIRED | Unique impact analysis identifier |
| trigger-id | string | REQUIRED | Change, entity, task, tool, policy, or merge triggering analysis |
| specification-id | string | REQUIRED | System identifier |
| prior-specification-version | semver | CONDITIONAL | Prior version |
| new-specification-version | semver | CONDITIONAL | New version |
| affected-artifacts | array | REQUIRED | Artifacts requiring action |
| unaffected-artifacts | array | REQUIRED | Artifacts confirmed unaffected |
| required-actions | array | REQUIRED | regenerate, revalidate, repair, repackage, retire, no-action |
| confidence | enum | REQUIRED | complete, partial, uncertain |
| traceability-snapshot-id | string | CONDITIONAL | Traceability snapshot used |
| analyzed-at | ISO 8601 | REQUIRED | Analysis time |

### 25.3 Impact Rules

Artifacts identified as affected MUST NOT remain stable without required action.

Artifacts identified as unaffected MUST NOT be regenerated unless governance or runtime configuration requires full reconstruction.

Partial or uncertain impact analysis MUST trigger review or escalation before selective reconstruction.

---

## 26.0 Artifact Security and Sensitivity

This section defines security and sensitivity controls for artifacts. Artifacts may contain source code, configuration, secrets, infrastructure definitions, data schemas, operational details, or regulated information.

### 26.1 Sensitivity Levels

| Sensitivity  | Meaning                                                                                 |
| ------------ | --------------------------------------------------------------------------------------- |
| public       | Artifact may be shared without restriction                                              |
| internal     | Artifact is limited to authorized internal use                                          |
| confidential | Artifact contains sensitive business, security, or operational information              |
| restricted   | Artifact contains regulated, secret, security-critical, or highly sensitive information |

### 26.2 Security Rules

Artifacts MUST NOT contain secrets unless explicitly permitted by specification and governance policy.

Configuration artifacts MUST be scanned for secrets before stable promotion when secret detection tools are available.

Restricted artifacts MUST require controlled access.

Security findings affecting artifacts MUST link to artifact metadata and validation evidence.

Artifact access MUST obey governance and security policies.

### 26.3 Artifact Security Metadata

Artifact metadata SHOULD include:

| Field               | Description                                   |
| ------------------- | --------------------------------------------- |
| sensitivity         | Artifact sensitivity classification           |
| secret-scan-status  | not-required, pending, passed, failed, waived |
| policy-ids          | Policies governing artifact                   |
| access-profile      | Access control profile                        |
| encryption-required | Whether artifact requires encryption at rest  |
| export-restricted   | Whether artifact may be exported              |

---

## 27.0 Repository Query Requirements

This section defines required repository queries. Repository queries support platform operation, audit, governance, debugging, repair, deployment preparation, and maintenance.

### 27.1 Required Queries

| Query                          | Description                                                   |
| ------------------------------ | ------------------------------------------------------------- |
| artifact-by-id                 | Return artifact metadata and location for artifact identifier |
| artifacts-by-specification     | Return artifacts produced for a specification version         |
| artifacts-by-source-entity     | Return artifacts derived from a canonical entity              |
| artifacts-by-task              | Return artifacts produced by a construction task              |
| artifacts-by-state             | Return artifacts by lifecycle state                           |
| artifacts-by-validation-status | Return artifacts by validation state                          |
| artifacts-by-branch            | Return artifacts in a branch or workspace                     |
| artifact-version-history       | Return all versions of an artifact                            |
| artifact-supersession-history  | Return superseded and superseding artifacts                   |
| untraced-artifacts             | Return artifacts without complete traceability                |
| unstable-artifacts             | Return artifacts not eligible for stable promotion            |
| package-contents               | Return artifacts included in a package                        |
| artifact-evidence              | Return validation evidence for an artifact version            |
| repository-integrity-findings  | Return repository integrity findings                          |

### 27.2 Query Rules

Repository queries MUST include artifact identifiers, versions, states, and specification version context.

Governance-sensitive queries MUST enforce access control.

Audit queries SHOULD support machine-readable export.

---

## 28.0 Repository Telemetry

This section defines artifact repository telemetry. Telemetry provides operational visibility into artifact lifecycle activity, repository health, merge behavior, validation bottlenecks, and packaging progress.

### 28.1 Required Telemetry Events

The platform SHOULD emit telemetry for:

* artifact candidate received
* artifact admitted
* artifact rejected
* artifact state changed
* artifact validation linked
* artifact promoted
* artifact superseded
* artifact archived
* artifact deleted
* branch created
* merge attempted
* merge conflict detected
* merge conflict resolved
* package created
* repository integrity check completed
* repository recovery started
* repository recovery completed

### 28.2 Telemetry Fields

Repository telemetry SHOULD include:

| Field                 | Description                         |
| --------------------- | ----------------------------------- |
| event-id              | Unique event identifier             |
| event-type            | Event type                          |
| timestamp             | Event time                          |
| repository-id         | Repository identifier               |
| branch-id             | Branch identifier when applicable   |
| artifact-id           | Artifact identifier when applicable |
| artifact-version      | Artifact version when applicable    |
| task-id               | Related task when applicable        |
| specification-id      | System identifier                   |
| specification-version | Specification version               |
| outcome               | Event outcome                       |
| severity              | Event severity when applicable      |

---

## 29.0 Repository Governance

This section defines governance behavior for repository operations. Repository operations can affect deployability, auditability, traceability, and production safety. Therefore, governance must be able to control high-impact repository actions.

### 29.1 Governed Repository Actions

Governance MAY control:

* artifact import
* promotion to stable
* merge into main
* package creation
* deployment package authorization
* deletion
* archival
* rollback
* use of waived artifacts
* use of restricted artifacts
* release candidate creation
* external artifact export

### 29.2 Governance Rules

A governance-blocked repository operation MUST NOT proceed.

A repository operation requiring approval MUST pause until approval exists.

Use of waived artifacts in a deployment-capable package MUST be visible in package manifest and deployment authorization evidence.

Deletion and archival MUST follow retention policy.

High and Critical risk systems SHOULD require stronger governance controls for merge, package, release, and deletion operations.

---

## 30.0 Repository Error Classes

This section defines repository-specific error classes. These errors support consistent handling by the repository service, runtime, governance engine, observability layer, and recovery mechanisms.

### 30.1 Repository Error Record Schema

| Field | Type | Required | Description |
|---|---|---|
| error-id | string | REQUIRED | Unique repository error identifier |
| error-class | enum | REQUIRED | Error class |
| severity | enum | REQUIRED | blocking, high, medium, low |
| repository-id | string | CONDITIONAL | Repository affected |
| branch-id | string | CONDITIONAL | Branch affected |
| artifact-id | string | CONDITIONAL | Artifact affected |
| task-id | string | CONDITIONAL | Task affected |
| specification-id | string | CONDITIONAL | System identifier |
| specification-version | semver | CONDITIONAL | Specification version |
| message | string | REQUIRED | Human-readable explanation |
| required-action | string | REQUIRED | Remediation required |
| detected-at | ISO 8601 | REQUIRED | Detection time |

### 30.2 Error Classes

| Error Class                            | Description                                 | Default Handling              |
| -------------------------------------- | ------------------------------------------- | ----------------------------- |
| repository-artifact-metadata-missing   | Artifact content lacks metadata             | block promotion               |
| repository-artifact-content-missing    | Metadata references missing content         | block promotion or recover    |
| repository-artifact-untraced           | Artifact lacks required traceability        | block stable state            |
| repository-artifact-validation-missing | Artifact lacks required validation evidence | block promotion               |
| repository-artifact-validation-failed  | Artifact validation failed                  | repair or escalate            |
| repository-path-conflict               | Multiple artifacts conflict on path         | conflict handling             |
| repository-merge-conflict              | Branch merge conflict detected              | block merge                   |
| repository-metadata-conflict           | Metadata differs incompatibly               | block merge or promote        |
| repository-version-conflict            | Artifact version conflict detected          | block promotion               |
| repository-branch-policy-violation     | Branch rule violated                        | block operation               |
| repository-stable-state-violation      | Invalid artifact present in stable state    | halt or recover               |
| repository-package-invalid             | Package manifest invalid or incomplete      | block package                 |
| repository-evidence-missing            | Required evidence absent                    | block promotion or deployment |
| repository-retention-violation         | Required artifact or evidence removed early | escalate                      |
| repository-access-denied               | Access request denied                       | block request                 |
| repository-integrity-failed            | Integrity check failed                      | halt affected action          |
| repository-recovery-failed             | Recovery could not restore consistent state | halt and escalate             |

---

## 31.0 Conformance Requirements

This section defines conformance requirements for artifact records, repositories, repository managers, and platforms.

### 31.1 Artifact Record Conformance

An artifact record conforms to ISL v2.3 if it:

* has a valid artifact identifier
* declares artifact type
* declares artifact version
* declares specification version context
* links to source entity identifiers or approved source records
* links to producing task or import record
* declares lifecycle state
* declares validation status
* declares traceability status
* declares repository location
* preserves metadata history

### 31.2 Artifact Repository Conformance

An Artifact Repository conforms to ISL v2.3 if it can:

* store artifacts and metadata
* enforce artifact admission checks
* enforce lifecycle state transitions
* preserve artifact versions
* support repository structure conventions or equivalent mapping
* support branch isolation
* detect merge conflicts
* record merge conflict resolution
* preserve validation evidence
* preserve traceability associations
* support artifact packaging
* enforce retention and archival rules
* perform repository integrity checks
* support required repository queries

### 31.3 Repository Manager Conformance

A repository manager conforms to ISL v2.3 if it can:

* admit candidate artifacts
* reject invalid artifacts
* promote artifacts to stable state only when eligible
* manage artifact versions and supersession
* manage branch creation and merge operations
* block invalid merges
* associate artifacts with validation evidence
* associate artifacts with governance records
* create repository snapshots
* recover repository state
* emit repository telemetry
* produce repository error records

### 31.4 Platform Conformance

A platform conforms to ISL v2.3 if it:

* routes all generated artifacts through the Artifact Repository
* prevents agents from directly writing stable artifacts
* prevents untraced artifacts from entering stable state
* prevents unvalidated artifacts from entering stable state unless waived
* integrates repository state with State and Memory Model
* integrates repository activity with Traceability Engine
* integrates repository operations with Governance Engine
* integrates validation evidence from Tool Integration Layer
* supports branch and conflict controls for parallel construction
* supports packaging and deployment preparation evidence
* preserves repository auditability and lifecycle history

---

## 32.0 Summary

ISL v2.3 defines the Artifact Repository Model for the IMHOTEP platform. It establishes how generated artifacts are stored, named, versioned, validated, promoted, branched, merged, packaged, retained, governed, queried, and traced.

This revision strengthens the original repository concept by making artifact identity, metadata, lifecycle state, repository layout, branching strategy, merge conflict handling, validation evidence, packaging, access control, integrity checks, retention, recovery, and conformance explicit.

A conforming IMHOTEP platform MUST treat the artifact repository as a governed construction archive and controlled engineering workspace. An artifact may be generated by autonomous reasoning, tools, repair cycles, imports, or humans, but it may become stable only when it is identified, traced, validated or waived, governed, and preserved according to repository lifecycle rules.
