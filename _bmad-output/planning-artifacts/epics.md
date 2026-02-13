---
stepsCompleted: [step-01-validate-prerequisites, step-02-design-epics, step-03-create-stories, step-04-final-validation]
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/architecture.md
  - _bmad-output/planning-artifacts/prd-validation-report.md
---

# cicd-templates - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for cicd-templates, decomposing the requirements from the PRD, UX Design if it exists, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Platform engineers can publish reusable deployment workflows for organization use.
FR2: Platform engineers can version reusable workflow contracts.
FR3: Platform engineers can communicate compatibility status for workflow versions.
FR4: Platform engineers can define required and optional workflow inputs.
FR5: Platform engineers can define standardized workflow outputs for deployment and rollback outcomes.
FR6: Platform engineers can deprecate outdated workflow versions with migration guidance.
FR7: Service teams can consume reusable workflows from external repositories.
FR8: Service teams can configure caller repositories using an explicitly documented required integration input checklist.
FR9: Service teams can onboard using cookbook-style documentation and complete a first successful non-production run in <= 1 hour.
FR10: Service teams can select service-specific deployment paths during integration.
FR11: Service teams can validate that workflow contracts are correctly configured before production use.
FR12: Service teams can trigger governed deployments through reusable workflows.
FR13: Deployments can proceed only after configured environment approvals are satisfied.
FR14: Operations owners can apply emergency gate decisions for sensitive deployments within <= 15 minutes and with decision audit records.
FR15: Service teams can run >= 90% of standard deployments without manual execution steps, excluding approvals and emergency gates.
FR16: The platform can support deployment execution for Azure Databricks in Phase 1.
FR17: The platform can support deployment execution for Azure Data Factory in Phase 1.
FR18: Service teams can trigger code rollback through a standardized rollback capability.
FR19: Support teams can identify rollback eligibility and rollback status from workflow outcomes.
FR20: Operations owners can authorize rollback when governance controls require approval.
FR21: Support teams can execute incident recovery using documented rollback procedures and complete code rollback within <= 1 hour for >= 90% of rollback-eligible incidents.
FR22: The platform can enforce a defined CI/CD control baseline across eligible repositories.
FR23: The platform can record auditable evidence for approvals, gate outcomes, and run history.
FR24: Security reviewers can verify identity and permission controls on deployment workflows.
FR25: Platform teams can evaluate control compliance status per repository.
FR26: Platform teams can identify policy violations requiring remediation.
FR27: Support teams can access standardized diagnostics for failed deployments.
FR28: Support teams can distinguish failure categories (configuration, policy, service-path, rollback-related).
FR29: Service teams can access run-level metadata needed for troubleshooting and post-incident review.
FR30: Platform teams can review weekly cross-repo execution trends (failure rate, rerun rate, rollback outcomes) to improve reliability and adoption.
FR31: Platform teams can provide migration paths from local pipelines to reusable workflows with prerequisites, cutover, and rollback steps.
FR32: Service teams can adopt reusable workflows incrementally per repository by stage (build, deploy, rollback) with per-stage validation checks.
FR33: Platform teams can track reusable workflow adoption across eligible repositories.
FR34: Platform teams can evolve service coverage beyond Phase 1 without breaking existing consumers.

### NonFunctional Requirements

NFR1: Reusable workflow invocation setup guidance enables new repository onboarding completion in <= 1 hour for standard scenarios.
NFR2: Standard deployment pipeline execution paths maintain deterministic stage progression with >= 95% of standard deployments completing without unplanned manual pauses outside configured governance gates.
NFR3: Rollback initiation to completion for code rollback is achievable within <= 1 hour for >= 90% of rollback-eligible incidents.
NFR4: Deployment authentication and authorization use federated identity with least-privilege access boundaries.
NFR5: Workflow execution permissions are scoped to minimum required levels per job and environment.
NFR6: Governance controls for protected environments enforce approval and emergency gate policies before sensitive deployments proceed.
NFR7: Action and workflow sourcing controls prevent unauthorized execution dependencies in deployment paths.
NFR8: The platform supports >= 90% deployment runs without manual deployment execution intervention by month 6.
NFR9: Deployment and rollback workflows achieve >= 95% successful completion across supported Phase 1 services (Databricks, Data Factory) over a rolling 30-day window under normal operating conditions.
NFR10: Failure handling provides documented recovery paths and enables recovery initiation within <= 15 minutes and completion within <= 60 minutes for rollback-eligible incidents.
NFR11: The reusable workflow platform supports incremental onboarding across multiple repositories without requiring per-repo architectural redesign.
NFR12: Workflow contract and module architecture supports expansion from Phase 1 services to additional Azure services in later phases without breaking existing consumers.
NFR13: Operational reporting supports portfolio-level visibility across eligible repositories for adoption and compliance tracking.
NFR14: Caller repositories can integrate with reusable workflows through standardized cross-repo workflow contracts.
NFR15: Phase 1 integration supports Azure Databricks and Azure Data Factory deployment paths with service-specific module behavior.
NFR16: Integration diagnostics provide standardized outputs and metadata for troubleshooting across repositories.
NFR17: Compliance evidence from approvals, gate decisions, and run records is capturable in auditable form across integrated repos.

### Additional Requirements

- Starter template requirement: use an organization-owned template repository for reusable workflow platform setup; this must drive Epic 1 Story 1 initialization.
- Project initialization command requirement: `gh repo create <org>/cicd-reusable-workflows --private --template <org>/reusable-workflows-template --clone`.
- Infrastructure/deployment requirements: fail-fast CI/CD stages (lint/policy/security -> validate -> deploy -> verify -> rollback), Dev/Test/Prod environment model with increasing protections.
- Integration requirements: contract-first `workflow_call` APIs with typed inputs/outputs and standardized output schema across core and modules.
- Data/setup requirements: versioned contracts, compatibility matrix, deprecation windows, and schema validation for module configs.
- Monitoring/logging requirements: standardized job summaries, diagnostics artifacts, run metadata, and compliance evidence outputs.
- API versioning/compatibility requirements: immutable refs for production, semantic release channels, strict backward-compatibility handling.
- Security implementation requirements: OIDC-only Azure auth (`azure/login@v2`), explicit least-privilege `permissions`, full SHA pinning for third-party actions, environment-scoped secrets.
- Governance requirements: environment approvals, emergency gates, required checks, and policy enforcement embedded in reusable workflows (not duplicated in caller repos).
- Rollback requirements: standardized rollback entry points, rollback metadata outputs, and documented runbooks for incident recovery.
- Architecture starter decision impact: initialization and platform skeleton are implementation priority before service module expansion.
- Validation-driven quality constraints from `prd-validation-report.md`: strengthen measurability and acceptance criteria in stories where PRD items were previously flagged (especially FR1/3/8/9/14/15/21/30/31/32 and NFR specificity concerns).
- Traceability requirement: each story should map to explicit FR/NFR and user-journey value to avoid orphan implementation.

### FR Coverage Map

FR1: Epic 1 - Publish reusable deployment workflows for organizational consumption.
FR2: Epic 1 - Version reusable workflow contracts for stable evolution.
FR3: Epic 1 - Communicate compatibility status across workflow versions.
FR4: Epic 1 - Define required/optional workflow inputs in contract.
FR5: Epic 1 - Define standardized deployment and rollback outputs.
FR6: Epic 1 - Deprecate outdated versions with migration guidance.
FR7: Epic 2 - Consume reusable workflows from external repositories.
FR8: Epic 2 - Configure caller repos with required integration checklist.
FR9: Epic 2 - Onboard via cookbook and achieve first non-prod run <= 1 hour.
FR10: Epic 2 - Select service-specific deployment paths during integration.
FR11: Epic 2 - Validate workflow contract configuration pre-production.
FR12: Epic 3 - Trigger governed deployments through reusable workflows.
FR13: Epic 3 - Enforce environment approvals before deployment proceeds.
FR14: Epic 3 - Apply emergency gate decisions with response/audit constraints.
FR15: Epic 3 - Achieve >= 90% standard deployments without manual execution (excluding governance gates).
FR16: Epic 3 - Support Azure Databricks deployment execution in Phase 1.
FR17: Epic 3 - Support Azure Data Factory deployment execution in Phase 1.
FR18: Epic 4 - Trigger standardized code rollback capability.
FR19: Epic 4 - Identify rollback eligibility and status from workflow outcomes.
FR20: Epic 4 - Authorize rollback under governance controls.
FR21: Epic 4 - Execute recovery with rollback SLA targets.
FR22: Epic 5 - Enforce CI/CD control baseline across eligible repos.
FR23: Epic 5 - Record auditable evidence for approvals/gates/runs.
FR24: Epic 5 - Verify identity and permission controls.
FR25: Epic 5 - Evaluate control compliance status per repository.
FR26: Epic 5 - Identify policy violations requiring remediation.
FR27: Epic 5 - Access standardized failed-deployment diagnostics.
FR28: Epic 5 - Distinguish failure categories for troubleshooting.
FR29: Epic 5 - Access run-level metadata for incident review.
FR30: Epic 5 - Review weekly cross-repo reliability/adoption trends.
FR31: Epic 6 - Provide migration paths from local pipelines.
FR32: Epic 6 - Enable staged incremental adoption per repository.
FR33: Epic 6 - Track reusable workflow adoption portfolio-wide.
FR34: Epic 6 - Evolve service coverage without breaking consumers.

## Epic List

### Epic 1: Reusable Workflow Product Foundation
Platform engineers can publish, version, and govern reusable workflow contracts that consumer repositories can adopt safely.
**FRs covered:** FR1, FR2, FR3, FR4, FR5, FR6

### Epic 2: Consumer Onboarding and Integration Experience
Service teams can quickly onboard and integrate caller repositories using cookbook documentation, validated setup, and clear service-path selection.
**FRs covered:** FR7, FR8, FR9, FR10, FR11

### Epic 3: Governed Deployments for Phase 1 Azure Services
Service teams can execute secure, governed deployments for Databricks and Data Factory through reusable workflows.
**FRs covered:** FR12, FR13, FR14, FR15, FR16, FR17

### Epic 4: Standardized Rollback and Incident Recovery
Operations and support teams can execute controlled rollback and recovery flows with explicit governance and SLA-backed outcomes.
**FRs covered:** FR18, FR19, FR20, FR21

### Epic 5: Compliance, Security Evidence, and Troubleshooting
Platform, security, and support teams can enforce controls, produce auditable evidence, and troubleshoot failures with standardized diagnostics.
**FRs covered:** FR22, FR23, FR24, FR25, FR26, FR27, FR28, FR29, FR30

### Epic 6: Adoption Management and Platform Evolution
Platform teams can migrate repositories, track adoption, and evolve service coverage while preserving compatibility for current consumers.
**FRs covered:** FR31, FR32, FR33, FR34

## Epic 1: Reusable Workflow Product Foundation

Platform engineers can publish, version, and govern reusable workflow contracts that consumer repositories can adopt safely.

### Story 1.1: Set up initial project from starter template

As a platform engineer,
I want to initialize the reusable workflow repository from the approved starter template,
So that we have a consistent baseline structure for all platform capabilities.

**Acceptance Criteria:**

**Given** the organization template repository is available
**When** I create the platform repository using the approved initialization pattern
**Then** the repository contains the baseline workflow, contracts, policy, docs, and test directories defined by architecture
**And** initialization instructions are documented for repeatable setup.

### Story 1.2: Define v1 Workflow Contract Schemas

As a platform engineer,
I want to define typed `workflow_call` input/output contracts for Phase 1 modules,
So that caller repositories integrate against stable, explicit interfaces.

**Acceptance Criteria:**

**Given** the baseline repository structure exists
**When** contract schema files are authored for Databricks deploy, Data Factory deploy, and rollback
**Then** required and optional inputs are explicitly defined per contract
**And** standardized outputs are defined, including deployment status and diagnostics references.

### Story 1.3: Establish Contract Versioning and Compatibility Policy

As a platform engineer,
I want a documented versioning and compatibility policy for reusable workflows,
So that consumers can adopt new versions without unexpected breakage.

**Acceptance Criteria:**

**Given** v1 contracts are defined
**When** semantic versioning and compatibility rules are documented
**Then** the project includes a compatibility matrix and deprecation policy with migration guidance
**And** production consumption guidance requires immutable refs for approved release tracks.

### Story 1.4: Implement Core Reusable Workflow Skeleton

As a platform engineer,
I want a core reusable workflow skeleton with standardized input validation and output handling,
So that all service modules follow a consistent execution contract.

**Acceptance Criteria:**

**Given** contract definitions exist
**When** core reusable workflows are created for input validation and common orchestration
**Then** they expose standardized output keys defined in contracts
**And** module workflows can call the core workflows without requiring service-specific duplication.

### Story 1.5: Add Contract and Workflow Validation CI Checks

As a platform engineer,
I want CI checks that validate workflow syntax, contract schema conformance, and compatibility rules,
So that invalid or breaking contract changes are blocked before release.

**Acceptance Criteria:**

**Given** core workflows and contracts are committed
**When** CI runs on pull requests
**Then** workflow lint and contract validation checks execute as required checks
**And** changes that violate contract schema or compatibility policy fail the pipeline.

### Story 1.6: Publish Governance and Deprecation Communication Baseline

As a platform engineer,
I want a release communication baseline for workflow versions and deprecations,
So that consumer teams can understand adoption impact and migration timelines.

**Acceptance Criteria:**

**Given** versioning and compatibility rules are defined
**When** a workflow version is prepared for release
**Then** release notes include compatibility status and deprecation impact
**And** the compatibility log is updated with migration guidance for affected consumers.

## Epic 2: Consumer Onboarding and Integration Experience

Service teams can quickly onboard and integrate caller repositories using cookbook documentation, validated setup, and clear service-path selection.

### Story 2.1: Publish Caller Quickstart Cookbook

As an application developer,
I want a cookbook-style quickstart for integrating a caller repository,
So that I can complete first integration without platform-team intervention.

**Acceptance Criteria:**

**Given** reusable workflow contracts are published
**When** I follow the quickstart cookbook
**Then** I can configure a caller workflow reference and required inputs using documented steps
**And** the cookbook includes troubleshooting guidance for common setup errors.

### Story 2.2: Provide Required Integration Input Checklist

As an application developer,
I want an explicit integration checklist for required inputs and prerequisites,
So that I can validate caller readiness before running deployments.

**Acceptance Criteria:**

**Given** a caller repository is onboarding
**When** the team applies the integration checklist
**Then** all required inputs, secrets, environment mappings, and service selectors are verified
**And** missing required items are clearly identified before execution.

### Story 2.3: Add Caller Contract Validation Workflow

As an application developer,
I want pre-deployment validation of caller configuration against workflow contracts,
So that integration mistakes are detected before governed deployment starts.

**Acceptance Criteria:**

**Given** caller workflow configuration is present
**When** contract validation runs in CI
**Then** invalid or incomplete caller configuration fails with actionable feedback
**And** valid configurations proceed to deployment stages.

### Story 2.4: Create Service Path Selection Patterns for Databricks and Data Factory

As a service owner,
I want clear module-selection patterns for Databricks and Data Factory,
So that I can choose the correct reusable workflow path for my workload.

**Acceptance Criteria:**

**Given** a team needs to deploy a Phase 1 workload
**When** they select a service deployment path
**Then** Databricks and Data Factory module options are documented with required inputs
**And** each option includes examples for standard and governed deployment usage.

### Story 2.5: Validate Onboarding Time-to-First-Run Objective

As a platform engineer,
I want onboarding validation scenarios that measure first non-production run completion,
So that we can confirm the <= 1 hour onboarding objective is achievable.

**Acceptance Criteria:**

**Given** pilot consumer repositories are available
**When** onboarding walkthroughs are executed using the cookbook
**Then** time-to-first-non-production-run is measured and recorded per pilot
**And** at least one validated path demonstrates completion within <= 1 hour.

## Epic 3: Governed Deployments for Phase 1 Azure Services

Service teams can execute secure, governed deployments for Databricks and Data Factory through reusable workflows.

### Story 3.1: Implement Databricks Module Contract and Pre-Deployment Validation

As a service owner,
I want a Databricks deployment module with explicit contract validation,
So that deployment requests fail early when required inputs or environment context are invalid.

**Acceptance Criteria:**

**Given** core reusable workflow foundations are available
**When** the Databricks module receives a deployment request
**Then** required contract inputs are validated before deployment execution starts
**And** validation failures return actionable errors without attempting deployment.

### Story 3.2: Implement Databricks Deployment Execution and Evidence Outputs

As a service owner,
I want Databricks deployments to run through the reusable module and emit standardized evidence,
So that deployment outcomes are consistent and auditable.

**Acceptance Criteria:**

**Given** Databricks request validation passes
**When** the deployment module executes
**Then** deployment runs via `workflow_call` and returns standardized status outputs
**And** diagnostics and evidence artifacts are generated for troubleshooting and audit use.

### Story 3.3: Implement Data Factory Module Contract and Pre-Deployment Validation

As a service owner,
I want a Data Factory deployment module with explicit contract validation,
So that deployment requests fail early when required inputs or environment context are invalid.

**Acceptance Criteria:**

**Given** core reusable workflow foundations are available
**When** the Data Factory module receives a deployment request
**Then** required contract inputs are validated before deployment execution starts
**And** validation failures return actionable errors without attempting deployment.

### Story 3.4: Implement Data Factory Deployment Execution and Evidence Outputs

As a service owner,
I want Data Factory deployments to run through the reusable module and emit standardized evidence,
So that deployment outcomes are consistent and auditable.

**Acceptance Criteria:**

**Given** Data Factory request validation passes
**When** the deployment module executes
**Then** deployment runs via `workflow_call` and returns standardized status outputs
**And** diagnostics and evidence artifacts are generated for troubleshooting and audit use.

### Story 3.5: Enforce Environment Approval Gates in Deployment Flow

As a release/operations owner,
I want environment approvals enforced before protected deployments,
So that production changes follow governance requirements.

**Acceptance Criteria:**

**Given** a deployment targets a protected environment
**When** the workflow reaches the approval stage
**Then** deployment cannot proceed until required approvals are granted
**And** approval decisions are auditable in run history.

### Story 3.6: Implement Emergency Gate Decision Control

As a release/operations owner,
I want emergency gate decisions available for sensitive deployments,
So that high-risk changes can be escalated and controlled quickly.

**Acceptance Criteria:**

**Given** a deployment is flagged as emergency-gated
**When** an authorized operations owner takes a gate decision
**Then** decision outcome is applied within <= 15 minutes
**And** decision records include actor, timestamp, and rationale for auditability.

### Story 3.7: Enforce Automated Standard Deployment Execution

As a platform engineer,
I want standard deployment execution to remain automated by default,
So that manual execution is minimized while governance controls are preserved.

**Acceptance Criteria:**

**Given** deployment requests follow standard paths
**When** workflows run across Phase 1 modules
**Then** at least >= 90% of standard deployments execute without manual intervention steps
**And** approvals and emergency gates remain excluded from this automation metric.

### Story 3.8: Add Governed Caller Deployment Examples

As an application developer,
I want caller workflow examples for governed Databricks and Data Factory deployments,
So that I can adopt secure and compliant patterns without custom pipeline design.

**Acceptance Criteria:**

**Given** deployment modules are available
**When** consumer teams use caller examples
**Then** examples demonstrate module invocation, environment targeting, and governance behavior
**And** examples are aligned with contract versions and required security posture.

## Epic 4: Standardized Rollback and Incident Recovery

Operations and support teams can execute controlled rollback and recovery flows with explicit governance and SLA-backed outcomes.

### Story 4.1: Implement Standardized Rollback Entry Workflows

As a support engineer,
I want standardized rollback entry workflows for Phase 1 modules,
So that rollback can be triggered consistently during incidents.

**Acceptance Criteria:**

**Given** a deployment run produced rollback-eligible metadata
**When** rollback is invoked for Databricks or Data Factory
**Then** rollback executes through standardized workflow entry points
**And** rollback outputs include status, reference, and diagnostics artifacts.

### Story 4.2: Add Rollback Eligibility and Status Signaling

As a support engineer,
I want explicit rollback eligibility and status signals in workflow outputs,
So that incident responders can quickly decide and track recovery actions.

**Acceptance Criteria:**

**Given** deployment and rollback workflows complete
**When** outputs are emitted
**Then** rollback availability and status fields are consistently populated across modules
**And** failure classifications support troubleshooting decisions.

### Story 4.3: Enforce Rollback Governance Authorization Path

As an operations owner,
I want rollback authorization controls for governed environments,
So that recovery actions remain compliant with change governance policies.

**Acceptance Criteria:**

**Given** rollback targets a governed environment
**When** rollback authorization is required
**Then** workflow execution pauses until authorized approval is granted
**And** authorization events are captured in auditable evidence.

### Story 4.4: Define and Validate Rollback Runbook Procedures

As a support engineer,
I want clear rollback runbooks for common incident scenarios,
So that recovery execution is consistent and low risk.

**Acceptance Criteria:**

**Given** rollback flows are implemented
**When** support teams follow the runbook
**Then** procedures cover pre-checks, execution, verification, and escalation paths
**And** runbook steps align with workflow outputs and diagnostics artifacts.

### Story 4.5: Verify Rollback SLA Performance Targets

As a platform engineer,
I want rollback SLA validation for Phase 1 services,
So that we can demonstrate <= 1 hour rollback completion for >= 90% of rollback-eligible incidents.

**Acceptance Criteria:**

**Given** rollback test scenarios for Databricks and Data Factory
**When** SLA validation runs are executed
**Then** rollback completion time and success rate metrics are recorded
**And** evidence shows whether >= 90% of rollback-eligible incidents complete within <= 1 hour.

## Epic 5: Compliance, Security Evidence, and Troubleshooting

Platform, security, and support teams can enforce controls, produce auditable evidence, and troubleshoot failures with standardized diagnostics.

### Story 5.1: Implement CI/CD Control Baseline Enforcement

As a security reviewer,
I want reusable workflows to enforce the agreed CI/CD control baseline,
So that deployments consistently comply with required governance and security controls.

**Acceptance Criteria:**

**Given** control baseline definitions are available
**When** deployment workflows execute
**Then** control checks are applied consistently across eligible repositories
**And** non-compliant runs are blocked or flagged according to policy.

### Story 5.2: Capture Auditable Approval and Gate Evidence

As a compliance stakeholder,
I want approvals and emergency gate outcomes captured as evidence,
So that audit and control verification can be performed without manual reconstruction.

**Acceptance Criteria:**

**Given** governed deployment or rollback runs occur
**When** approvals and gate decisions are executed
**Then** evidence records include actor, timestamps, outcomes, and linked run identifiers
**And** evidence can be retrieved from standardized artifacts or logs.

### Story 5.3: Enforce Federated Identity and Least-Privilege Permissions

As a security reviewer,
I want OIDC-based authentication and least-privilege permission scopes enforced,
So that deployment workflows avoid overprivileged access and long-lived credential risk.

**Acceptance Criteria:**

**Given** deployment workflows are defined
**When** authentication and permission checks are validated
**Then** Azure access uses federated identity patterns
**And** workflow permissions are explicitly scoped to minimum required levels.

### Story 5.4: Implement Policy Violation Detection and Reporting

As a platform engineer,
I want policy violations detected and surfaced clearly,
So that teams can remediate non-compliant workflow usage quickly.

**Acceptance Criteria:**

**Given** control and policy checks run in CI/CD
**When** a violation is detected
**Then** violation reports identify repository, violated rule, and remediation guidance
**And** platform teams can review violation trends over time.

### Story 5.5: Standardize Diagnostics and Failure Classification

As a support engineer,
I want standardized diagnostics outputs and failure categories,
So that troubleshooting is consistent across Databricks and Data Factory workflows.

**Acceptance Criteria:**

**Given** deployment or rollback failures occur
**When** workflows emit diagnostics
**Then** outputs include categorized failure classes and concise failure summaries
**And** diagnostics artifacts contain enough context for first-line triage.

### Story 5.6: Expose Run Metadata for Incident and Postmortem Analysis

As a service team,
I want run-level metadata and evidence references available after execution,
So that incidents and postmortems can be analyzed without manual data gathering.

**Acceptance Criteria:**

**Given** a workflow run completes
**When** run metadata is published
**Then** teams can access run identifiers, environment context, status, and evidence links
**And** metadata format remains consistent across Phase 1 modules.

### Story 5.7: Implement Weekly Reliability and Adoption Trend Reporting

As a platform engineer,
I want weekly cross-repo trend reporting for reliability and adoption,
So that we can prioritize improvements based on failure, rerun, rollback, and usage patterns.

**Acceptance Criteria:**

**Given** run and evidence data from eligible repositories
**When** weekly reporting is generated
**Then** reports include failure rate, rerun rate, rollback outcomes, and adoption indicators
**And** reporting outputs are accessible to platform and governance stakeholders.

## Epic 6: Adoption Management and Platform Evolution

Platform teams can migrate repositories, track adoption, and evolve service coverage while preserving compatibility for current consumers.

### Story 6.1: Publish Migration Playbook from Local Pipelines

As a platform engineer,
I want a migration playbook for moving from repo-local pipelines to reusable workflows,
So that teams can transition safely with clear prerequisites and rollback options.

**Acceptance Criteria:**

**Given** a repository currently uses local deployment pipelines
**When** the migration playbook is applied
**Then** it provides step-by-step prerequisites, cutover actions, validation checks, and fallback procedures
**And** migration outcomes can be verified before production promotion.

### Story 6.2: Enable Staged Incremental Adoption per Repository

As a service team,
I want to adopt reusable workflows incrementally by stage,
So that migration risk is reduced while preserving delivery continuity.

**Acceptance Criteria:**

**Given** a repository is onboarding to the platform
**When** staged adoption is configured
**Then** teams can adopt build, deploy, and rollback stages progressively
**And** each stage has explicit validation checks before advancing.

### Story 6.3: Implement Portfolio Adoption Tracking

As a platform team,
I want portfolio-level adoption tracking for eligible repositories,
So that I can measure rollout progress and identify support needs.

**Acceptance Criteria:**

**Given** repositories integrate reusable workflows
**When** adoption telemetry is collected
**Then** platform teams can view adoption status by repository and phase
**And** onboarding progress can be monitored against adoption goals.

### Story 6.4: Add Compatibility Guardrails for Platform Evolution

As a platform engineer,
I want compatibility guardrails on workflow evolution,
So that new service coverage and contract changes do not break existing consumers.

**Acceptance Criteria:**

**Given** a workflow contract or module update is proposed
**When** compatibility validation runs
**Then** incompatible changes are detected before release
**And** versioning/deprecation pathways are enforced for consumer-safe evolution.

### Story 6.5: Prepare Post-MVP Service Expansion Path

As a product/platform owner,
I want a structured expansion plan for additional Azure services,
So that post-MVP growth can be executed without destabilizing Phase 1 consumers.

**Acceptance Criteria:**

**Given** Phase 1 modules are operational
**When** post-MVP planning is documented
**Then** expansion scope includes Function Apps, Web Apps, and Logic Apps with dependency and readiness criteria
**And** expansion planning preserves current compatibility and governance baselines.
