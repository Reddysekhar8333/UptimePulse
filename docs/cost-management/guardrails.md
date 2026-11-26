# AWS Cost Guardrails

## 1. Account assumptions

The client has described the AWS account as a clean Free Tier account. Exact Free Tier eligibility, credit balance, account plan, expiry date, and service-specific allowances have not yet been verified.

Do not assume EKS, NAT Gateway, RDS, load balancers, public IPv4, CloudWatch ingestion, or data transfer are free. Free Tier benefits vary by account creation date, plan, region, and service.

Before provisioning, inspect Billing and Cost Management, Free Tier/credits, and the AWS Pricing Calculator for the selected region and configuration.

## 2. Development policy

- Use local Docker and kind as the default development platform.
- Use local PostgreSQL, Redis, and queue emulation where practical.
- Do not keep EKS running continuously during application development.
- Create short-lived AWS integration environments only when needed.
- Do not create NAT Gateways, load balancers, RDS, ElastiCache, or EKS without a written estimate and client approval.
- Use small test datasets and short log retention.
- Apply ECR lifecycle policies to remove unneeded images.
- Tag all billable resources with `Project=UptimePulse`, `Environment`, `Owner`, and `ManagedBy`.

## 3. Budget and alerts

Configure an AWS Budget and billing alerts as soon as billing access is available. Proposed alert thresholds: 50%, 75%, and 90% of the client-approved monthly budget, plus forecasted-spend alerts where supported.

Budget alerts are notifications, not a hard spending cap and not an automatic shutdown mechanism. Review actual and forecasted costs regularly.

The client must approve the monthly budget amount before it is configured. Do not invent a budget value.

## 4. Cost review checklist

For every AWS deployment, record:

- Resource name and region.
- Hourly/monthly estimate and usage assumptions.
- Data transfer and storage assumptions.
- Free Tier/credit applicability and expiry.
- Whether the resource continues charging when idle.
- Shutdown/destroy procedure.
- Data backup/export requirements.
- Re-creation time and recovery impact.

## 5. Teardown principles

- Destroy disposable compute and networking resources when the integration window ends.
- Preserve Terraform state bucket and required state versions.
- Preserve required database backups and user data before destructive operations.
- Delete orphaned load balancers, EBS volumes, snapshots, NAT Gateways, Elastic IPs, and log groups only after confirming retention requirements.
- Verify resource inventory and Cost Explorer after teardown; delayed billing data may take time to appear.
- Keep an explicit allowlist of persistent resources.

Never run `terraform destroy` against the state backend from the same state that manages the backend. Bootstrap state infrastructure separately and protect it from accidental deletion.
