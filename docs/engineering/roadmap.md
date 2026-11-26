# UptimePulse Engineering Roadmap

## Phase 1 — Foundation, state, security and CI/CD

### M1.1 Repository and architecture foundation
- [x] Confirm repository and client-approved local-first approach.
- [x] Document architecture, ownership boundaries, and environment model.
- [x] Document proposed VPC CIDRs and network security intent.
- [x] Document security baseline and cost guardrails.
- [ ] Review and approve architecture assumptions with client.

### M1.2 Local engineering workstation
- [ ] Validate Windows/WSL2 and Docker prerequisites.
- [ ] Install and validate Git, Docker, kind, kubectl, Helm, Terraform, and Ansible.
- [ ] Create a single-node local kind cluster.
- [ ] Record tool versions and setup instructions.

### M1.3 Terraform state and AWS account guardrails
- [ ] Verify account plan, Free Tier eligibility, credits, and billing visibility.
- [ ] Confirm client-approved monthly budget.
- [ ] Enable root MFA and establish least-privilege human access.
- [ ] Bootstrap versioned, encrypted S3 state bucket.
- [ ] Enable native S3 state locking with Terraform `use_lockfile = true`.
- [ ] Test state locking and recovery.
- [ ] Configure GitHub Actions OIDC trust policy.

### M1.4 Infrastructure and automation
- [ ] Implement reviewed Terraform modules and environment composition.
- [ ] Implement Ansible roles only for hosts that require configuration management.
- [ ] Add Terraform fmt/validate/plan and security checks to CI.
- [ ] Add Ansible lint/syntax validation.

**Phase 1 exit:** repeatable local tooling, reviewed architecture, protected state, account cost controls, and validated CI.

## Phase 2 — Application, workers, GitOps and autoscaling

- [ ] Implement authentication and user management.
- [ ] Implement monitor CRUD and safe endpoint validation.
- [ ] Implement check scheduler, worker, incident lifecycle, and digest workflow.
- [ ] Implement status page and history APIs.
- [ ] Containerize frontend, API, workers, and scheduled jobs.
- [ ] Add application tests and Trivy scanning.
- [ ] Package workloads with Helm.
- [ ] Deploy to kind and reconcile with Argo CD.
- [ ] Validate HPA and KEDA locally.
- [ ] Add Kyverno admission policies.
- [ ] Validate queue retries, idempotency, and DLQ handling.
- [ ] Prepare AWS-specific integration tests.

**Phase 2 exit:** end-to-end application flow works in local Kubernetes and deployments are GitOps-managed.

## Phase 3 — Observability, pilot onboarding and cost teardown

- [ ] Deploy Prometheus, Grafana, and Loki.
- [ ] Integrate CloudWatch for AWS infrastructure telemetry.
- [ ] Configure service-level dashboards and actionable alerts.
- [ ] Complete security, load, failure, backup, and restore tests.
- [ ] Review and approve AWS pilot cost estimate.
- [ ] Provision AWS resources incrementally.
- [ ] Validate EKS workload identity, ESO, Karpenter, and AWS service integrations.
- [ ] Deploy frontend with CloudFront and private S3 origin.
- [ ] Onboard a controlled initial pilot group.
- [ ] Test incident emails and operational runbooks.
- [ ] Validate teardown and recreation procedures.

**Phase 3 exit:** pilot users complete the monitoring journey, operational alerts work, and the platform can be recovered and safely shut down according to its data-retention requirements.
