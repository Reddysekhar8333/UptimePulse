# UptimePulse Security Baseline

## 1. Account and identity

- Protect the AWS root user with MFA; do not use root for daily operations.
- Prefer IAM Identity Center or an appropriately scoped administrative role for human access.
- Use separate roles for infrastructure deployment and application workloads.
- Apply least privilege and review permissions at each milestone.
- Use GitHub Actions OIDC federation with a repository/branch/environment-restricted trust policy.
- Never store long-lived AWS access keys in GitHub repository secrets.

## 2. Secrets and configuration

- Commit only safe placeholders in `.env.example`.
- Ignore `.env`, `.env.*` except explicitly approved examples, private keys, certificates, kubeconfigs, Terraform state, plan files, and credential files.
- Do not output secret values from Terraform.
- For local development, use untracked local secrets or a local secret manager.
- For AWS, store application credentials in Secrets Manager and sync only required keys using External Secrets Operator.
- Restrict each workload's secret access to its own secret ARNs.
- Rotate credentials and document the rotation procedure.

## 3. Network and data

- Databases and caches must use private/isolated subnets.
- Database security groups accept traffic only from the application workload security group.
- Redis security groups accept traffic only from authorized workloads.
- Enforce TLS where supported.
- Encrypt data at rest and in transit.
- Define retention and deletion policies for monitor history, incidents, user data, and logs.
- Backups and restore tests are required before pilot onboarding.

## 4. Kubernetes workload baseline

- Run containers as non-root.
- Set `allowPrivilegeEscalation: false`.
- Drop Linux capabilities unless explicitly required.
- Use seccomp RuntimeDefault where supported.
- Set CPU/memory requests and limits.
- Use read-only root filesystems where compatible.
- Do not use privileged containers or host namespaces by default.
- Pin images by immutable digest for production promotion.
- Enforce approved registries and baseline policies with Kyverno.

## 5. Supply-chain security

- Scan images with Trivy in CI and use ECR scanning when available and cost-appropriate.
- Fail builds on agreed critical/high vulnerability thresholds, with documented exceptions.
- Pin GitHub Actions to reviewed immutable commit SHAs before production use.
- Generate and retain build metadata/SBOM where practical.
- Avoid floating image tags for production releases.

## 6. Application-specific risks

Endpoint monitoring is an SSRF-sensitive workload.

- Permit only approved URL schemes (normally HTTP and HTTPS).
- Reject loopback, private, link-local, multicast, reserved, and cloud metadata destinations.
- Resolve and validate DNS results before connecting; defend against DNS rebinding.
- Revalidate every redirect target.
- Apply connect/read timeouts, response-size limits, redirect limits, and concurrency limits.
- Do not forward internal credentials or arbitrary headers to monitored endpoints.
- Enforce per-account quotas and rate limits.
- Store only the minimum response metadata required for monitoring.

Authentication must use secure password hashing, secure session/token handling, account verification and abuse controls. Detailed choices will be recorded during application design.

## 7. Security acceptance checks

- Secret scanning finds no credentials in repository history.
- Terraform plan shows no public database/cache exposure.
- IAM policies are scoped and reviewed.
- Workloads run non-root and pass Kyverno admission policies.
- Trivy scan results are reviewed.
- SSRF test cases cover IPv4, IPv6, redirects, DNS rebinding and metadata IPs.
- Backup restore and credential rotation procedures are tested.
