---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8]
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/prd-validation-report.md
  - _bmad-output/planning-artifacts/research/technical-github-reusable-workflows-azure-deploy-cicd-best-practices-research-2026-02-12.md
workflowType: 'architecture'
lastStep: 8
status: 'complete'
completedAt: '2026-02-13'
project_name: 'cicd-templates'
user_name: 'Dasper'
date: '2026-02-12T21:26:46-06:00'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**
The platform must provide a centralized reusable workflow system with stable, versioned contracts and clear compatibility signaling for caller repositories. Functional scope includes publisher-side governance (contract/version management and deprecation guidance), consumer-side onboarding (lightweight integration and validation), deployment execution (governed approvals and service-module selection), rollback operations, and portfolio-level lifecycle management. Architecturally, this implies a contract-first platform surface with strict interface discipline, caller/callee separation, and standardized outputs for deployment, rollback, and diagnostics. The FR set also requires explicit support for Databricks and Data Factory in Phase 1 while preserving extensibility for future Azure module expansion without consumer breakage.

**Non-Functional Requirements:**
Architecture must prioritize least-privilege security, federated identity, environment governance, and auditable controls. Reliability and operability requirements emphasize high automation rate, predictable deployment flow, and repeatable rollback outcomes under SLA constraints. Scalability requires multi-repo onboarding without per-repo redesign and support for future service-module growth. Integration NFRs require standardized cross-repo contracts and diagnostic consistency. Validation findings indicate several NFRs need stronger measurable acceptance definitions, so architecture decisions should include explicit verification hooks and operational evidence mechanisms.

**Scale & Complexity:**
This is a high-complexity internal platform initiative with strong cross-cutting governance and reliability requirements.

- Primary domain: CI/CD platform and Azure deployment orchestration
- Complexity level: High
- Estimated architectural components: 11

### Technical Constraints & Dependencies

Known constraints include phased delivery (Databricks + Data Factory first), strict governance retention (approvals/emergency gates), federated identity boundaries, and rollback SLA targets. Dependencies include cross-repo GitHub reusable workflow consumption, Azure deployment targets, and platform-level telemetry/evidence collection. Contract evolution must be managed to prevent caller-repo breakage during platform growth. Additional dependency is organizational adoption maturity: onboarding quality and migration playbooks are required for platform success.

### Cross-Cutting Concerns Identified

Security hardening, permission minimization, and identity trust boundaries affect all components. Governance and compliance evidence generation must be embedded throughout deployment paths. Contract versioning/deprecation policy is a platform-wide concern spanning producers and consumers. Observability and diagnostics standardization are required for supportability and incident response. Rollback consistency, migration safety, and adoption tracking are lifecycle-wide concerns that must be designed as first-class capabilities.

## Starter Template Evaluation

### Primary Technology Domain

CI/CD platform automation (cross-repo reusable GitHub workflows with Azure deployment governance) based on project requirements analysis.

### Starter Options Considered

1. **GitHub official starter workflows (`actions/starter-workflows`)**
   - Pros: broad coverage, standard conventions, strong visibility.
   - Cons: generic templates; not purpose-built for cross-repo reusable workflow platforms and Azure governance controls.
   - Maintenance signal: active repository with ongoing updates; contribution intake is restricted, so customization must happen in your own repos.

2. **Organization workflow templates (`.github/workflow-templates`)**
   - Pros: official org-wide distribution mechanism, metadata-driven discoverability, excellent for onboarding consistency.
   - Cons: optimized for workflow bootstrapping in consumer repos, not full platform lifecycle/versioning by itself.

3. **Template-repository starter for reusable workflow platform (recommended)**
   - Pros: directly matches your architecture (caller/callee model, versioned contracts, Azure OIDC defaults, CI/CD guardrails), easiest to evolve with semantic versioning.
   - Cons: requires initial platform-team setup effort and governance ownership.

### Selected Starter: Organization-Owned Template Repository for Reusable Workflow Platform

**Rationale for Selection:**
This option aligns best with your FR/NFR set: it supports strict contract-first reusable workflows, stable version evolution, and embedded governance controls while enabling fast replication and onboarding. It also cleanly supports your Phase 1 scope (Databricks + Data Factory) with a shared core and service modules.

**Initialization Command:**

```bash
gh repo create <org>/cicd-reusable-workflows --private --template <org>/reusable-workflows-template --clone
```

**Architectural Decisions Provided by Starter:**

**Language & Runtime:**
YAML-first workflow architecture, shell/CLI execution for deployment steps, optional TypeScript or composite actions for reusable logic.

**Styling Solution:**
Not applicable (infrastructure automation domain).

**Build Tooling:**
GitHub Actions-native validation and execution, with action pinning and reusable workflow references as contract boundaries.

**Testing Framework:**
Workflow lint/validation, contract checks for inputs/outputs, and policy/security gates (permissions, OIDC, environment protections).

**Code Organization:**
Clear separation of `core` reusable workflows, service-specific deployment modules (Databricks/ADF), and consumer-facing examples/docs.

**Development Experience:**
Fast setup via template cloning, standardized repo structure, and ready-to-consume caller examples for cross-repo adoption.

**Note:** Project initialization using this command should be the first implementation story.

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**
- Reusable workflow contract model: typed `workflow_call` interfaces with strict required/optional inputs and standardized outputs.
- Identity model: OIDC-only Azure authentication using `azure/login@v2`; no long-lived cloud credentials in deployment flows.
- Deployment boundary model: central platform repo owns deploy logic; caller repos remain thin orchestration layers.
- Governance model: GitHub Environments with approval gates for protected targets and explicit permissions per job.
- Versioning model: immutable ref consumption for production (SHA pinning), with semantic release channels for workflow contracts.

**Important Decisions (Shape Architecture):**
- Module topology: shared core pipeline + service-specific deploy modules (Databricks, Data Factory).
- Evidence model: standardized run outputs/artifacts for compliance and incident diagnostics.
- Policy posture: full SHA pinning for third-party actions, least-privilege `permissions`, branch protection required checks.
- Rollback model: standardized rollback entry points and metadata outputs from reusable workflows.

**Deferred Decisions (Post-MVP):**
- Extended Azure module catalog (Function Apps, Web Apps, Logic Apps).
- Advanced progressive rollout strategies (canary/blue-green where service-appropriate).
- Enterprise analytics dashboards beyond baseline KPI extraction.

### Data Architecture

- **Decision:** Platform metadata-as-code in repository (YAML contracts, docs, schema files), with run-time evidence stored as workflow artifacts and logs.
- **Validation strategy:** Contract validation for `workflow_call` inputs/outputs and schema checks for module-specific config payloads.
- **Migration strategy:** Versioned contract evolution with compatibility matrix and deprecation windows.
- **Caching strategy:** Native Actions caching/artifact reuse where deterministic; no shared mutable state between caller repos.

### Authentication & Security

- **Authentication:** GitHub OIDC tokens to Azure federation (`id-token: write`) via `azure/login@v2`.
- **Authorization:** Least-privilege Azure role assignments per environment and module; GitHub job permissions explicitly scoped.
- **Supply chain controls:** Pin third-party actions by full commit SHA in production tracks.
- **Secrets posture:** Prefer environment-scoped secrets; avoid broad repo/org secret exposure for protected deployments.
- **API/workflow security:** Restrict who can call deployment workflows (org/repo policy), enforce branch and environment protections.

### API & Communication Patterns

- **Pattern:** Workflow contract as API surface (`workflow_call` with typed inputs, declared outputs).
- **Error standard:** Structured output fields for status category, failure reason class, and remediation hint.
- **Inter-workflow communication:** Output-driven handoff and artifacts, not implicit side effects.
- **Rate control:** Concurrency groups and environment-level serialization for high-risk targets.
- **Documentation:** Contract reference per workflow version plus caller examples per module.

### Frontend Architecture

Not applicable for MVP (platform CI/CD architecture focus).

### Infrastructure & Deployment

- **Hosting strategy:** GitHub-hosted runners by default; self-hosted only for explicit network/control requirements.
- **CI/CD approach:** Fail-fast stages: lint/policy/security -> validate -> deploy -> verify -> rollback path.
- **Environment model:** Dev/Test/Prod with progressively stricter approvals and protection rules.
- **Monitoring/logging:** Standardized job summaries, artifacts, and run metadata for compliance and support.
- **Scaling strategy:** Reuse via centralized workflows + caller thin wrappers; module expansion without contract breakage.

### Decision Impact Analysis

**Implementation Sequence:**
1. Define contract schemas and versioning policy.
2. Implement core reusable workflow skeleton and security defaults.
3. Add Databricks and Data Factory modules.
4. Add caller reference workflows and onboarding docs.
5. Add evidence/diagnostic outputs and rollback flows.
6. Enforce org/repo governance controls and required checks.

**Cross-Component Dependencies:**
- Contract schema drives caller integration and validation strategy.
- OIDC trust model drives environment design and role assignments.
- Governance settings (permissions, approvals, protection rules) constrain deployment topology and rollback operations.
- Standard outputs influence observability, compliance reporting, and support runbooks.

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

**Critical Conflict Points Identified:**
14 areas where AI agents could make different choices

### Naming Patterns

**Database Naming Conventions:**
Not applicable for MVP architecture scope (workflow-platform first). If persistence is added later, default to snake_case tables/columns and plural table names.

**API Naming Conventions:**
- Reusable workflow input/output keys use `snake_case`.
- Workflow file names use kebab-case (e.g., `deploy-databricks.yml`).
- Contract outputs must be explicit and stable:
  - `deployment_status`
  - `deployment_id`
  - `rollback_available`
  - `rollback_reference`
  - `diagnostics_artifact`

**Code Naming Conventions:**
- Composite actions: kebab-case directories under `.github/actions/`.
- Internal reusable job IDs: snake_case.
- Environment names: lowercase (`dev`, `test`, `prod`).

### Structure Patterns

**Project Organization:**
- `.github/workflows/core/` for shared orchestration workflows.
- `.github/workflows/modules/` for service-specific deploy workflows.
- `.github/workflows/caller-examples/` for integration examples.
- `docs/contracts/` for versioned contract docs.
- `docs/runbooks/` for rollback and incident procedures.

**File Structure Patterns:**
- One reusable workflow per concern (validate, deploy, rollback, evidence).
- Module workflows call core workflows; callers call modules.
- Governance/policy checks live in reusable workflows, not duplicated in callers.

### Format Patterns

**API Response Formats:**
All reusable workflows emit standardized outputs via `jobs.<job_id>.outputs` and surface final status in a consistent schema:
- `deployment_status`: `success|failed|partial`
- `failure_class`: `config|policy|platform|service|unknown`
- `failure_message`: concise human-readable summary

**Data Exchange Formats:**
- JSON artifacts use snake_case keys.
- Date/time values are ISO-8601 UTC strings.
- Boolean values are native booleans (`true|false`), never numeric stand-ins.
- Nullability is explicit in contract docs.

### Communication Patterns

**Event System Patterns:**
- Workflow/job summary events use normalized tags:
  - `deploy.started`
  - `deploy.succeeded`
  - `deploy.failed`
  - `rollback.started`
  - `rollback.succeeded`
  - `rollback.failed`
- Artifact names follow: `<module>-<environment>-<run_id>-<purpose>`.

**State Management Patterns:**
- State is passed only through declared outputs/artifacts.
- No hidden state assumptions between caller and called workflows.
- Concurrency groups used for environment-serialized deploys.

### Process Patterns

**Error Handling Patterns:**
- Every failure path must classify error type and emit remediation hint.
- Retry policy is explicit and bounded per operation.
- User-facing failure summary and detailed diagnostics artifact are both required.

**Loading State Patterns:**
- Not UI-oriented; operational equivalent is stage-state reporting.
- Each major stage emits start/end markers and duration in job summary.

### Enforcement Guidelines

**All AI Agents MUST:**
- Preserve contract key names and output schema exactly once published.
- Use explicit `permissions` blocks per workflow/job.
- Keep Azure authentication OIDC-based (`id-token: write`, `azure/login@v2` path).

**Pattern Enforcement:**
- Enforce with workflow lint + schema checks + PR required checks.
- Record deviations in `docs/contracts/compatibility-log.md`.
- Update patterns only via versioned ADR/architecture change entry.

### Pattern Examples

**Good Examples:**
- `deploy-datafactory.yml` exports `deployment_status` and `diagnostics_artifact`.
- Caller workflow pins reusable workflow by immutable ref in production.
- Failure writes `failure_class=policy` with actionable remediation text.

**Anti-Patterns:**
- Mixing `camelCase` and `snake_case` contract keys across modules.
- Module workflows inventing custom output names not in contract docs.
- Using long-lived Azure secrets when OIDC trust is available.
- Duplicating governance logic in each caller repository.

## Project Structure & Boundaries

### Complete Project Directory Structure

```text
cicd-reusable-workflows/
├── README.md
├── LICENSE
├── .gitignore
├── .editorconfig
├── .github/
│   ├── CODEOWNERS
│   ├── dependabot.yml
│   ├── workflows/
│   │   ├── ci/
│   │   │   ├── lint-workflows.yml
│   │   │   ├── validate-contracts.yml
│   │   │   ├── security-gates.yml
│   │   │   └── release-workflows.yml
│   │   ├── core/
│   │   │   ├── validate-inputs.yml
│   │   │   ├── deploy-core.yml
│   │   │   ├── collect-evidence.yml
│   │   │   └── rollback-core.yml
│   │   ├── modules/
│   │   │   ├── deploy-databricks.yml
│   │   │   ├── rollback-databricks.yml
│   │   │   ├── deploy-datafactory.yml
│   │   │   └── rollback-datafactory.yml
│   │   ├── caller-examples/
│   │   │   ├── caller-deploy-databricks.yml
│   │   │   ├── caller-deploy-datafactory.yml
│   │   │   └── caller-rollback.yml
│   │   └── ops/
│   │       ├── manual-rollback-dispatch.yml
│   │       └── compliance-evidence-export.yml
│   └── actions/
│       ├── common-setup/
│       │   └── action.yml
│       ├── emit-diagnostics/
│       │   └── action.yml
│       └── enforce-contract/
│           └── action.yml
├── contracts/
│   ├── workflow-contract.schema.json
│   ├── deploy-databricks-contract.v1.yaml
│   ├── deploy-datafactory-contract.v1.yaml
│   ├── rollback-contract.v1.yaml
│   └── compatibility-matrix.yaml
├── policy/
│   ├── permissions-baseline.yaml
│   ├── environment-protection-baseline.yaml
│   ├── action-pinning-policy.yaml
│   └── deprecation-policy.yaml
├── scripts/
│   ├── validate-contracts.py
│   ├── check-action-pins.py
│   ├── generate-evidence-index.py
│   └── release-contract-version.py
├── docs/
│   ├── architecture/
│   │   ├── adr/
│   │   └── architecture-overview.md
│   ├── contracts/
│   │   ├── deploy-databricks.md
│   │   ├── deploy-datafactory.md
│   │   ├── rollback.md
│   │   └── compatibility-log.md
│   ├── onboarding/
│   │   ├── quickstart-caller-repo.md
│   │   ├── migration-guide.md
│   │   └── faq.md
│   ├── runbooks/
│   │   ├── rollback-procedure.md
│   │   ├── failed-deployment-troubleshooting.md
│   │   └── emergency-change-procedure.md
│   └── compliance/
│       ├── controls-mapping.md
│       ├── evidence-retention.md
│       └── audit-prep-checklist.md
├── test/
│   ├── contract/
│   │   ├── deploy-databricks-contract.test.yaml
│   │   ├── deploy-datafactory-contract.test.yaml
│   │   └── rollback-contract.test.yaml
│   ├── integration/
│   │   ├── reusable-workflow-callers.test.yaml
│   │   └── evidence-generation.test.yaml
│   └── fixtures/
│       ├── databricks/
│       └── datafactory/
└── examples/
    ├── caller-repo-minimal/
    │   └── .github/workflows/deploy.yml
    └── caller-repo-enterprise/
        └── .github/workflows/deploy-with-approvals.yml
```

### Architectural Boundaries

**API Boundaries:**
- Public API is the reusable workflow contract (`workflow_call` inputs/outputs) under `.github/workflows/modules/*`.
- Internal API is core workflow composition under `.github/workflows/core/*`.
- Caller repositories consume module workflows only; they must not call private core workflows directly.

**Component Boundaries:**
- `core/` owns common orchestration (validation, shared deploy steps, evidence, rollback primitives).
- `modules/` own service-specific Azure deployment behavior.
- `actions/` contains reusable helper logic only (no module-specific business logic).

**Service Boundaries:**
- Databricks and Data Factory modules are isolated and versioned independently under shared contract governance.
- Policy and contract validation are centralized (`policy/`, `contracts/`, `scripts/`) and enforced in CI.

**Data Boundaries:**
- Persistent architecture state lives in repo contracts/docs; runtime execution evidence lives in workflow artifacts/logs.
- No hidden cross-run mutable state is allowed between caller repos.

### Requirements to Structure Mapping

**Feature/Epic Mapping:**
- Governance/versioning -> `contracts/`, `policy/`, `.github/workflows/ci/release-workflows.yml`, `docs/contracts/compatibility-log.md`
- Onboarding/consumption -> `.github/workflows/caller-examples/`, `examples/`, `docs/onboarding/`
- Deployment execution -> `.github/workflows/core/deploy-core.yml`, `.github/workflows/modules/deploy-*.yml`
- Rollback/recovery -> `.github/workflows/core/rollback-core.yml`, `.github/workflows/modules/rollback-*.yml`, `docs/runbooks/rollback-procedure.md`
- Compliance/auditability -> `.github/workflows/core/collect-evidence.yml`, `docs/compliance/`, `.github/workflows/ops/compliance-evidence-export.yml`
- Observability/troubleshooting -> `emit-diagnostics` action, `docs/runbooks/failed-deployment-troubleshooting.md`, evidence artifacts

**Cross-Cutting Concerns:**
- Security baselines -> `policy/permissions-baseline.yaml`, `policy/action-pinning-policy.yaml`, security CI workflows
- Contract consistency -> `contracts/*.yaml`, `contracts/workflow-contract.schema.json`, `scripts/validate-contracts.py`
- Reliability/rollback SLAs -> rollback workflows + runbooks + integration tests

### Integration Points

**Internal Communication:**
- Core and module workflows exchange data only through declared workflow inputs/outputs and artifacts.
- CI workflows validate contracts/policies before release and block incompatible changes.

**External Integrations:**
- Azure authentication via OIDC in module/core deploy flows.
- GitHub Environments for approvals/protection.
- Caller repositories invoking reusable workflows by versioned refs.

**Data Flow:**
- Caller repo passes deployment intent -> module workflow validates -> core deploy executes -> module emits standardized outputs -> evidence collected/exported -> rollback reference exposed.

### File Organization Patterns

**Configuration Files:**
- Governance and security configuration in `policy/`; workflow contracts in `contracts/`; CI config in `.github/workflows/ci/`.

**Source Organization:**
- Workflow logic split by responsibility: core/shared, module-specific, caller examples, ops tasks.

**Test Organization:**
- Contract tests in `test/contract/`; end-to-end workflow integration in `test/integration/`; reusable fixtures in `test/fixtures/`.

**Asset Organization:**
- Evidence and diagnostics are generated per run as artifacts; long-lived reference assets/docs remain under `docs/`.

### Development Workflow Integration

**Development Server Structure:**
- Not app-server oriented; local development centers on contract validation scripts and workflow lint/test tooling.

**Build Process Structure:**
- CI executes lint -> policy checks -> contract validation -> integration tests -> release workflow versions.

**Deployment Structure:**
- Deployment is initiated by caller repos; reusable module workflows perform deploy/rollback and return standardized outputs + evidence.

## Architecture Validation Results

### Coherence Validation ✅

**Decision Compatibility:**
The selected architecture is coherent end-to-end: reusable workflow contracts, OIDC-based Azure authentication, policy enforcement, and evidence generation align with one another. No conflicting decisions were identified between security posture, deployment boundaries, or versioning strategy.

**Pattern Consistency:**
Implementation patterns directly support architectural decisions. Naming conventions, contract output formats, and workflow boundaries are consistent across core and module workflows. Conflict-prone areas (outputs, error taxonomy, artifact naming, permissions scope) are explicitly standardized.

**Structure Alignment:**
The project structure supports all selected decisions. Core/shared workflows, service modules, contract definitions, policy files, and runbooks are physically separated in a way that matches logical boundaries and supports maintainable evolution.

### Requirements Coverage Validation ✅

**Epic/Feature Coverage:**
FR categories are fully mapped to architecture components and directories, including governance/versioning, onboarding, deployment execution, rollback, compliance/evidence, and observability/troubleshooting.

**Functional Requirements Coverage:**
All major FR clusters are represented by concrete architecture elements:
- Cross-repo reuse -> module/core reusable workflows
- Azure deployment -> OIDC-enabled module deployment flows
- Rollback -> dedicated rollback workflows and runbooks
- Adoption/onboarding -> caller examples and migration docs

**Non-Functional Requirements Coverage:**
Security, compliance, reliability, and scalability are covered by:
- OIDC + least-privilege permissions
- environment protections + approval gates
- standardized diagnostics/evidence outputs
- versioned contracts + compatibility matrix + deprecation policy

### Implementation Readiness Validation ✅

**Decision Completeness:**
Critical decisions are explicit and actionable, including versions/majors where relevant (`azure/login@v2`, `azure/cli@v2`, OIDC trust model). Decision rationale and impact are documented.

**Structure Completeness:**
Project structure is specific, non-generic, and implementation-ready for platform engineering work. Integration points between caller repos, module workflows, and core workflows are clear.

**Pattern Completeness:**
Patterns cover naming, structure, data formats, communication tags, error handling, and enforcement. They are sufficient to minimize multi-agent divergence.

### Gap Analysis Results

**Critical Gaps:** None identified.

**Important Gaps (Non-blocking):**
- Add explicit contract test fixtures for edge-case rollback scenarios.
- Add policy-as-code checks for environment protection drift.
- Add optional attestation/SBOM extension path for future supply-chain maturity.

**Nice-to-Have Gaps:**
- Expanded module catalog templates beyond Databricks/ADF.
- Additional enterprise adoption dashboards and KPI automation exports.

### Validation Issues Addressed

- Resolved potential ambiguity around internal vs external workflow API boundaries by explicitly restricting callers to module workflows.
- Resolved risk of output-schema drift by standardizing required contract outputs and documenting enforcement location.
- Confirmed no conflicting naming or structure rules remain.

### Architecture Completeness Checklist

**✅ Requirements Analysis**
- [x] Project context thoroughly analyzed
- [x] Scale and complexity assessed
- [x] Technical constraints identified
- [x] Cross-cutting concerns mapped

**✅ Architectural Decisions**
- [x] Critical decisions documented with versions
- [x] Technology stack fully specified
- [x] Integration patterns defined
- [x] Performance considerations addressed

**✅ Implementation Patterns**
- [x] Naming conventions established
- [x] Structure patterns defined
- [x] Communication patterns specified
- [x] Process patterns documented

**✅ Project Structure**
- [x] Complete directory structure defined
- [x] Component boundaries established
- [x] Integration points mapped
- [x] Requirements to structure mapping complete

### Architecture Readiness Assessment

**Overall Status:** READY FOR IMPLEMENTATION

**Confidence Level:** High

**Key Strengths:**
- Contract-first platform design with clear caller/callee boundaries
- Security-forward model (OIDC, least privilege, approvals, policy controls)
- Strong multi-agent consistency rules reducing implementation conflicts
- Direct traceability from requirements to architecture and structure

**Areas for Future Enhancement:**
- Broader Azure module support post-MVP
- Additional policy automation and attestation depth
- Expanded analytics/reporting for adoption and compliance trends

### Implementation Handoff

**AI Agent Guidelines:**
- Follow all architectural decisions exactly as documented
- Use implementation patterns consistently across all components
- Respect project structure and boundaries
- Refer to this document for all architectural questions

**First Implementation Priority:**
Initialize the reusable workflow platform repository and implement contract validation + core workflow skeleton before adding Databricks/Data Factory modules.
