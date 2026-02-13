---
stepsCompleted: [step-01-init, step-02-discovery, step-03-success, step-04-journeys, step-05-domain, step-06-innovation, step-07-project-type, step-08-scoping, step-09-functional, step-10-nonfunctional, step-11-polish]
inputDocuments:
  - _bmad-output/planning-artifacts/research/technical-github-reusable-workflows-azure-deploy-cicd-best-practices-research-2026-02-12.md
documentCounts:
  briefCount: 0
  researchCount: 1
  brainstormingCount: 0
  projectDocsCount: 0
workflowType: 'prd'
classification:
  projectType: developer_tool
  domain: general
  complexity: high
  projectContext: greenfield
date: '2026-02-12T20:34:25-06:00'
---

# Product Requirements Document - cicd-templates

**Author:** Dasper
**Date:** 2026-02-12T20:34:25-06:00

## Executive Summary

`cicd-templates` is an internal developer platform product that standardizes CI/CD through reusable GitHub workflows consumed cross-repo with `workflow_call`. The product focuses on secure, governed Azure deployments with built-in rollback, low-friction onboarding, and measurable operational outcomes.

The Phase 1 MVP is intentionally narrow: Azure Databricks and Azure Data Factory deployment modules only. This boundary reduces delivery risk while proving core platform value: reusable contracts, governance controls, code rollback, and adoption-ready documentation.

Post-MVP expansion adds additional Azure service modules (Function Apps, Web Apps, Logic Apps), richer policy automation, and broader self-service platform capabilities. Success is measured through automation rate, compliance rate, onboarding efficiency, rollback reliability, and adoption growth.

## Success Criteria

### User Success

- 90% of deployments run with no manual deployment intervention by month 6.
- New repo onboarding via documentation is completed in <= 1 hour.
- Reusable workflow integration in caller repos is completed in <= 1 day (implementation = call workflow).
- Code rollback path is available and operational with <= 1 hour rollback time and >= 90% rollback success rate.

### Business Success

- Achieve >= 95% compliance against a defined CI/CD control catalog.
- Reduce change failure rate by >= 30% by month 6 versus the 3-month pre-rollout baseline.
- Reach >= 75% adoption across eligible repositories using reusable workflows by month 6.

### Technical Success

- Central reusable workflows with stable contracts support Databricks and Data Factory in Phase 1, with planned expansion to Function Apps, Web Apps, and Logic Apps in later phases.
- Governance controls preserved by design: environment approvals, emergency gates, and secure policy controls.
- Standard telemetry and KPI instrumentation exist for automation, compliance, adoption, and rollback performance.
- Workflow architecture supports common core + service-specific deployment modules.

### Measurable Outcomes

- Automation: 90% by month 6.
- Onboarding: <= 1 hour.
- Integration implementation: <= 1 day.
- Code rollback: <= 1 hour, >= 90% success.
- Compliance: >= 95% against explicit controls.
- Adoption target: >= 75% of eligible repositories by month 6.
- Change failure reduction target: >= 30% by month 6 versus 3-month pre-rollout baseline.

## Product Scope

### MVP - Minimum Viable Product

- Shared reusable workflow repository callable from other repos.
- Simple onboarding documentation and minimal caller integration.
- Azure deployment + code rollback support for initial target services.
- Baseline compliance controls and measurement.

### Growth Features (Post-MVP)

- Expanded service-specific modules and richer guardrails.
- Adoption and compliance dashboards.
- Advanced deployment strategies and reliability automation.

### Vision (Future)

- Organization-wide internal delivery platform with versioned contracts, self-service onboarding, and continuous optimization.

## User Journeys

### 1) Platform Engineer - Core Success Path

Opening scene: Sofia, a platform engineer, is tasked with standardizing deployments across dozens of repos. Teams currently use inconsistent YAML and ad-hoc scripts.

Rising action: She designs a reusable `workflow_call` contract, adds Phase 1 service modules (Databricks and Data Factory), defines required inputs, and publishes onboarding docs. She validates security defaults (OIDC, permissions, approvals) and versioning.

Climax: A new repo integrates by calling the reusable workflow in less than a day, and deployments run without manual execution steps.

Resolution: Platform consistency improves; teams move faster with less duplication.

### 2) Application Developer - Edge Case + Recovery Path

Opening scene: Mateo needs to ship a hotfix for a Databricks deployment quickly.

Rising action: He integrates the reusable workflow from docs in under an hour, opens a PR, and the pipeline runs through checks and governed approvals. Deployment starts automatically.

Climax: A post-deploy issue appears. Instead of manual patching, he triggers standardized code rollback.

Resolution: Rollback completes within SLA, incident is contained, and postmortem data is captured from pipeline telemetry.

### 3) Release/Operations Owner - Governance Path

Opening scene: Priya oversees production releases and compliance posture.

Rising action: She reviews environment approvals and emergency gates for a critical deployment window. Policies enforce required checks and branch protections before deployment can continue.

Climax: A high-risk release is approved with confidence because controls are explicit and evidence is visible.

Resolution: Release completes under governance; compliance reporting stays above target.

### 4) Support/Troubleshooting Engineer - Incident Path

Opening scene: Alex receives an alert for failed deployment in a Databricks-related pipeline.

Rising action: He inspects standardized logs/outputs from the reusable workflow, quickly identifies whether failure is contract misuse, service-specific config, or environment policy block.

Climax: He provides a precise remediation path and, if needed, coordinates rollback.

Resolution: MTTR stays low because diagnostics are consistent across repositories.

### 5) Service Owner (Azure Workload-Specific) - Adoption Path

Opening scene: Lina owns Databricks and Data Factory workloads with service-specific deployment nuances.

Rising action: She selects the appropriate module variant from the reusable workflow platform and configures only required service inputs.

Climax: Service-specific deployment succeeds without custom pipeline reinvention.

Resolution: Team keeps service flexibility while staying on the standard platform path.

### Journey Requirements Summary

- Central reusable workflow contracts with versioning and compatibility guarantees.
- Fast onboarding and thin caller integration experience.
- Built-in code rollback flow with measurable SLA.
- Governance controls (approvals/emergency gates) preserved by design.
- Service-specific deployment modules under a shared core architecture.
- Standardized telemetry, diagnostics, and audit evidence across all repos.

## Domain-Specific Requirements

### Compliance & Regulatory

- Maintain >= 95% compliance against a defined CI/CD control catalog across eligible repositories.
- Ensure deployment governance is auditable: approvals, gate outcomes, and workflow run evidence.
- Define and enforce reusable workflow control baselines (identity, permissions, action sourcing, environment protections).

### Technical Constraints

- Achieve 90% deployments with no manual deployment execution intervention by month 6.
- Preserve manual governance controls where required (environment approvals and emergency gates).
- Provide code rollback with <= 1 hour rollback time and >= 90% success rate.
- Version reusable workflow contracts with compatibility management to avoid caller-repo breakage.

### Integration Requirements

- Support cross-repo `workflow_call` adoption with low-friction caller implementation.
- Cover Azure Databricks and Azure Data Factory in Phase 1, with Function Apps, Web Apps, and Logic Apps planned for post-MVP phases.
- Use shared deployment core + service-specific modules for workload-specific behavior.

### Risk Mitigations

- Mitigate contract drift through version/deprecation policy and compatibility checks.
- Mitigate security risk through strict federated identity boundaries and least privilege.
- Mitigate adoption risk with simple onboarding docs and validated quick-start paths.
- Mitigate incident impact with standardized diagnostics, telemetry, and rollback procedures.

## Developer Tool Specific Requirements

### Project-Type Overview

This product is a reusable CI/CD developer platform delivered as shared GitHub workflows callable from other repositories. The initial focus is practical adoption and deployment reliability for Azure workloads, starting with Databricks and Azure Data Factory. The product optimizes for low-friction integration, governance-by-default, and standardized deployment/rollback behavior.

### Technical Architecture Considerations

- Core implementation pattern: centralized reusable workflows (`workflow_call`) consumed by caller repositories.
- Contract design: stable typed inputs/outputs with explicit versioning and compatibility policy.
- Security model: federated identity and least-privilege permissions with governed environment controls.
- Modularity model: shared deployment core plus service modules, beginning with Databricks and Data Factory.
- Operational model: standard telemetry and diagnostics emitted across all workflow executions.

### Language Matrix

- Primary supported language/runtime in v1: Python.
- Workflow examples and templates prioritize Python project pipelines and deployment patterns.

### Installation Methods

- Primary ecosystem in v1: npm (for dependency/package automation paths where applicable).
- Caller repo setup remains lightweight: include reusable workflow call and required inputs/secrets configuration.

### API Surface

- Reusable workflow interface is the primary API surface:
  - Required deployment inputs (service/module selector, environment, artifact/config references).
  - Optional inputs for rollback behavior and controlled deployment strategies.
  - Standardized outputs for deployment result, rollback metadata, and diagnostics references.

### Code Examples

- v1 examples will include:
  - Databricks deployment workflow usage from caller repo.
  - Azure Data Factory deployment workflow usage from caller repo.
- Examples demonstrate minimal integration path and governed deployment flow.

### Migration Guide

- Migration path from repo-local bespoke pipelines to centralized reusable workflows:
  1. Map existing pipeline steps to reusable workflow contract.
  2. Replace local deployment jobs with workflow call.
  3. Validate environment approvals, rollback path, and telemetry outputs.
  4. Roll out per-repo with compatibility checks and documented fallback.

### Implementation Considerations

- Keep v1 IDE integration minimal (no dedicated extension investment initially).
- Adopt cookbook-style documentation with task-oriented recipes and copy-ready examples.
- Prioritize predictable onboarding and reliable service-specific deployment behavior over feature breadth.

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Platform MVP focused on proving reusable workflow consumption, governed deployments, and rollback reliability with a narrow service scope.
**Resource Requirements:** Small cross-functional platform pod (platform engineer + cloud/devops engineer + security/governance reviewer + part-time consumer team champions).

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:**

- Platform Engineer standardizes and publishes reusable deployment workflows.
- Application/Service teams integrate by calling workflows with minimal setup.
- Operations/release governance remains controlled via approvals/emergency gates.
- Support can troubleshoot and execute rollback through standardized paths.

**Must-Have Capabilities:**

- Central reusable workflow repository callable cross-repo.
- Stable contract/versioning model for `workflow_call`.
- Service modules limited to:
  - Azure Databricks
  - Azure Data Factory
- Cookbook-style onboarding docs (<= 1 hour onboarding target).
- Code rollback path (<= 1 hour, >= 90% success target).
- Compliance control baseline and auditable execution evidence.

### Post-MVP Features

**Phase 2 (Post-MVP):**

- Add Function Apps, Web Apps, and Logic Apps service modules.
- Expand governance dashboards and adoption telemetry.
- Improve deployment strategies and policy automation depth.

**Phase 3 (Expansion):**

- Self-service onboarding and broader platform productization.
- Advanced reliability and optimization capabilities.
- Organization-wide standardized delivery model.

### Risk Mitigation Strategy

**Technical Risks:** Limit MVP to Databricks + Data Factory to reduce abstraction complexity; enforce contract/version discipline.
**Market Risks:** Validate value via fast onboarding, adoption rate, and change failure reduction in pilot repos.
**Resource Risks:** Keep MVP scope narrow, prioritize must-have flows, defer broad service coverage to Phase 2.

## Functional Requirements

### Platform Workflow Governance

- FR1: Platform engineers can publish reusable deployment workflows for organization use.
- FR2: Platform engineers can version reusable workflow contracts.
- FR3: Platform engineers can communicate compatibility status for workflow versions.
- FR4: Platform engineers can define required and optional workflow inputs.
- FR5: Platform engineers can define standardized workflow outputs for deployment and rollback outcomes.
- FR6: Platform engineers can deprecate outdated workflow versions with migration guidance.

### Repository Onboarding & Consumption

- FR7: Service teams can consume reusable workflows from external repositories.
- FR8: Service teams can configure caller repositories using an explicitly documented required integration input checklist.
- FR9: Service teams can onboard using cookbook-style documentation and complete a first successful non-production run in <= 1 hour.
- FR10: Service teams can select service-specific deployment paths during integration.
- FR11: Service teams can validate that workflow contracts are correctly configured before production use.

### Deployment Operations

- FR12: Service teams can trigger governed deployments through reusable workflows.
- FR13: Deployments can proceed only after configured environment approvals are satisfied.
- FR14: Operations owners can apply emergency gate decisions for sensitive deployments within <= 15 minutes and with decision audit records.
- FR15: Service teams can run >= 90% of standard deployments without manual execution steps, excluding approvals and emergency gates.
- FR16: The platform can support deployment execution for Azure Databricks in Phase 1.
- FR17: The platform can support deployment execution for Azure Data Factory in Phase 1.

### Rollback & Recovery

- FR18: Service teams can trigger code rollback through a standardized rollback capability.
- FR19: Support teams can identify rollback eligibility and rollback status from workflow outcomes.
- FR20: Operations owners can authorize rollback when governance controls require approval.
- FR21: Support teams can execute incident recovery using documented rollback procedures and complete code rollback within <= 1 hour for >= 90% of rollback-eligible incidents.

### Compliance, Auditability & Risk Control

- FR22: The platform can enforce a defined CI/CD control baseline across eligible repositories.
- FR23: The platform can record auditable evidence for approvals, gate outcomes, and run history.
- FR24: Security reviewers can verify identity and permission controls on deployment workflows.
- FR25: Platform teams can evaluate control compliance status per repository.
- FR26: Platform teams can identify policy violations requiring remediation.

### Observability & Troubleshooting

- FR27: Support teams can access standardized diagnostics for failed deployments.
- FR28: Support teams can distinguish failure categories (configuration, policy, service-path, rollback-related).
- FR29: Service teams can access run-level metadata needed for troubleshooting and post-incident review.
- FR30: Platform teams can review weekly cross-repo execution trends (failure rate, rerun rate, rollback outcomes) to improve reliability and adoption.

### Adoption & Lifecycle Management

- FR31: Platform teams can provide migration paths from local pipelines to reusable workflows with prerequisites, cutover, and rollback steps.
- FR32: Service teams can adopt reusable workflows incrementally per repository by stage (build, deploy, rollback) with per-stage validation checks.
- FR33: Platform teams can track reusable workflow adoption across eligible repositories.
- FR34: Platform teams can evolve service coverage beyond Phase 1 without breaking existing consumers.

## Non-Functional Requirements

### Performance

- NFR1: Reusable workflow invocation setup guidance enables new repository onboarding completion in <= 1 hour for standard scenarios.
- NFR2: Standard deployment pipeline execution paths maintain deterministic stage progression with >= 95% of standard deployments completing without unplanned manual pauses outside configured governance gates.
- NFR3: Rollback initiation to completion for code rollback is achievable within <= 1 hour for >= 90% of rollback-eligible incidents.

### Security

- NFR4: Deployment authentication and authorization use federated identity with least-privilege access boundaries.
- NFR5: Workflow execution permissions are scoped to minimum required levels per job and environment.
- NFR6: Governance controls for protected environments enforce approval and emergency gate policies before sensitive deployments proceed.
- NFR7: Action and workflow sourcing controls prevent unauthorized execution dependencies in deployment paths.

### Reliability

- NFR8: The platform supports >= 90% deployment runs without manual deployment execution intervention by month 6.
- NFR9: Deployment and rollback workflows achieve >= 95% successful completion across supported Phase 1 services (Databricks, Data Factory) over a rolling 30-day window under normal operating conditions.
- NFR10: Failure handling provides documented recovery paths and enables recovery initiation within <= 15 minutes and completion within <= 60 minutes for rollback-eligible incidents.

### Scalability

- NFR11: The reusable workflow platform supports incremental onboarding across multiple repositories without requiring per-repo architectural redesign.
- NFR12: Workflow contract and module architecture supports expansion from Phase 1 services to additional Azure services in later phases without breaking existing consumers.
- NFR13: Operational reporting supports portfolio-level visibility across eligible repositories for adoption and compliance tracking.

### Integration

- NFR14: Caller repositories can integrate with reusable workflows through standardized cross-repo workflow contracts.
- NFR15: Phase 1 integration supports Azure Databricks and Azure Data Factory deployment paths with service-specific module behavior.
- NFR16: Integration diagnostics provide standardized outputs and metadata for troubleshooting across repositories.
- NFR17: Compliance evidence from approvals, gate decisions, and run records is capturable in auditable form across integrated repos.
