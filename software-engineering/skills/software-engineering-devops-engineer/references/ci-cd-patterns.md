---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[devops-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# CI/CD Pipeline Patterns (2025)

## Standard Pipeline Stages

```
Trigger → Build → Test → Security Scan → Deploy (Dev) →
Deploy (Staging) → Approval → Deploy (Prod) → Validate
```

## Deployment Strategies

### Blue-Green Deployment
Two identical production environments. Switch traffic after validation.

**Advantages**: Instant rollback, zero downtime
**Trade-offs**: Doubles infrastructure, requires database migration handling

### Canary Deployment
Route small percentage of traffic to new version. Monitor metrics. Gradually increase or rollback.

**Advantages**: Real traffic validation, quick rollback
**Trade-offs**: Complex traffic routing, requires monitoring

### Rolling Deployment
Gradually replace instances. Remove from load balancer, update, restore to rotation.

**Advantages**: Minimal infrastructure overhead
**Trade-offs**: Requires graceful shutdown handling

## Security Gates

Every pipeline must include:

1. **Static Analysis**: SAST (SonarQube, CodeQL)
2. **Dependency Scanning**: Check for known CVEs (Snyk, Dependabot)
3. **Container Scanning**: Scan images for vulnerabilities (Trivy, Grype)
4. **Compliance Checks**: Policy-as-code (OPA, Checkov)
5. **Secrets Detection**: Prevent secret commits (GitGuardian, detect-secrets)

```yaml
# Example: GitHub Actions security gate
- name: Scan for secrets
  uses: gitleaks/gitleaks-action@v2

- name: Container scan
  run: trivy image --severity HIGH,CRITICAL my-image:${{ github.sha }}

- name: Infrastructure compliance
  run: checkov -d terraform/
```

## Observability in CI/CD

Deploy hooks must validate:
- Service health checks pass
- Metrics within expected ranges
- Error rates below threshold
- Resource utilization normal

Automated rollback on validation failure.
