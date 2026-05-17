---
applyTo: '**/deploy/**,**/k8s/**,**/kubernetes/**,**/terraform/**,**/infra/**,**/.github/workflows/**,**/helm/**'
---

# Deployment Conventions

## Deployment Strategies

Choose the right strategy based on risk:

| Strategy          | When to Use                                                          |
| ----------------- | -------------------------------------------------------------------- |
| **Rolling**       | Standard services — gradually replace old instances                  |
| **Blue-Green**    | Zero-downtime releases with instant rollback capability              |
| **Canary**        | High-traffic or high-risk changes — test on a small percentage first |
| **Feature Flags** | Decouple deployment from release — ship code dark, enable per-user   |

## Kubernetes

- Always set `resources.requests` and `resources.limits` on containers
- Configure `readinessProbe` and `livenessProbe` on every container
- Use `PodDisruptionBudget` to ensure minimum availability during cluster maintenance
- Prefer `Deployment` over `StatefulSet` for stateless services
- Use `ConfigMap` for non-secret config, `Secret` for sensitive values
- Never bake secrets into container images

## CI/CD

- Fail the pipeline on test failure before deploying — never skip tests
- Gate production deployments on a passing staging deployment
- Include a rollback step or procedure in every pipeline
- Artifacts are immutable — the same image tag deployed to staging goes to production

## Terraform / Infrastructure as Code

- Plan before apply: always review `terraform plan` output
- State is stored remotely (S3, GCS, Terraform Cloud) — never local state in a shared team
- Use modules for repeated resource patterns
- Lock provider versions — do not use `>= x.y` without an upper bound

## Secrets

- Never store secrets in source control or CI environment variable plain text
- Use a secrets manager: AWS Secrets Manager, GCP Secret Manager, HashiCorp Vault, or Doppler
- Rotate secrets after any suspected exposure
