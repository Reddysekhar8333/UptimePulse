# Repository and Git Standards

## Branching

- `main`: stable, reviewed baseline.
- `feat/<short-description>`: application or platform feature.
- `fix/<short-description>`: bug fix.
- `infra/<short-description>`: infrastructure change.
- `docs/<short-description>`: documentation-only change.
- `chore/<short-description>`: maintenance and project setup.

Use pull requests for meaningful changes. Do not commit directly to `main` once CI and branch protection are configured.

## Commit messages

Use conventional commit prefixes:

- `feat:` new capability
- `fix:` defect correction
- `infra:` infrastructure change
- `ci:` pipeline change
- `docs:` documentation
- `test:` tests
- `chore:` maintenance

Example: `docs: add initial UptimePulse architecture baseline`.

## File and configuration rules

- Keep environment-specific values out of reusable modules.
- Commit `.env.example` only with safe, non-secret placeholders.
- Never commit populated `.env` files, credentials, kubeconfigs, private keys, certificates, Terraform state, Terraform plan files, or local database dumps.
- Pin tool and dependency versions where practical.
- Keep Helm values separated by environment.
- Keep Kubernetes secrets out of plaintext Git; use a secure secret workflow.
- Document any intentional exception.

## Review requirements

Infrastructure changes must include:
- Purpose and affected resources.
- Terraform plan summary.
- Cost impact.
- Security impact.
- Rollback or teardown procedure.

Application changes must include:
- Tests or a reason tests are not applicable.
- Container image impact.
- Configuration/secrets impact.
- Observability impact.
