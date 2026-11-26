# UptimePulse Network and CIDR Plan

**Status:** Proposed design only. No VPC or subnet has been created.

## 1. Proposed VPC

- Region: ap-south-1 (Mumbai), pending service-availability and pricing validation.
- VPC CIDR: `10.40.0.0/16`.
- DNS support and DNS hostnames: enabled.
- Availability zones: two AZs for the pilot design, subject to service support and cost review.

## 2. Proposed subnet allocation

| AZ | Subnet role | CIDR | Route intent |
|---|---|---|---|
| AZ-a | Public/edge | 10.40.0.0/20 | Internet Gateway route |
| AZ-b | Public/edge | 10.40.16.0/20 | Internet Gateway route |
| AZ-a | Private application | 10.40.32.0/20 | Controlled egress; NAT or endpoints only if justified |
| AZ-b | Private application | 10.40.48.0/20 | Controlled egress; NAT or endpoints only if justified |
| AZ-a | Isolated data | 10.40.64.0/20 | No internet route |
| AZ-b | Isolated data | 10.40.80.0/20 | No internet route |

The large subnet blocks are reserved for future flexibility; actual pilot workloads may need much smaller allocations. Revisit subnet sizing before provisioning, particularly EKS pod networking and available IP capacity.

## 3. Routing principles

- Public subnets: default route to an Internet Gateway only for resources that genuinely require public ingress/egress.
- Private application subnets: no direct route to an Internet Gateway. Evaluate NAT Gateway hourly/data-processing charges against VPC endpoints and alternative egress designs.
- Isolated data subnets: no default internet route.
- Do not deploy a NAT Gateway merely by habit. It is a recurring-cost decision requiring approval.
- Prefer VPC endpoints for supported AWS service access where their fixed and data-processing costs are justified.

## 4. Security group intent

| Security group | Inbound intent |
|---|---|
| Edge/load balancer | HTTPS 443 from intended internet clients |
| Kubernetes nodes/pods | Only required cluster and application traffic |
| Database | Database port only from authorized application identity/security group |
| Redis | Redis port only from authorized application identity/security group |
| Administrative access | Prefer SSM; no unrestricted public SSH |

Exact ports and source groups will be defined from the selected application and EKS networking model, not guessed in advance.

## 5. EKS networking considerations

- Confirm VPC CNI IP consumption and pod density before selecting subnet sizes.
- Keep control-plane endpoint access restricted; consider private endpoint access and tightly scoped administrative access.
- Use workload identity rather than node-wide AWS permissions.
- Evaluate security groups for pods only if supported and operationally justified.
- Karpenter node pools must constrain instance families, architectures, capacity types, zones, and maximum resources to avoid surprise spend.

## 6. Cost decisions requiring approval

1. NAT Gateway count and topology.
2. Public load balancer count and exposure.
3. Number of AZs for each managed service.
4. VPC endpoint set.
5. EKS cluster lifetime and node capacity.
6. RDS and Redis instance sizes and availability configuration.

No network resources are to be provisioned until the cost review is complete.
