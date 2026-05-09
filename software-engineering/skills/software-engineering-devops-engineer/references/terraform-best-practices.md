---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[devops-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Terraform Best Practices (2025)

## Module Organization

### Structure
```
terraform/
├── modules/
│   ├── networking/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── README.md
│   ├── compute/
│   ├── database/
│   └── monitoring/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   ├── staging/
│   └── prod/
└── shared/
    └── variables.tf
```

### Key Principles
- **Modules**: Build reusable, composable building blocks
- **Environments**: Separate state per environment with distinct backends
- **Variables**: Use tfvars for environment-specific overrides
- **Outputs**: Expose only necessary information

## State Management

### Remote State
Always use remote backends with locking:

```hcl
terraform {
  backend "s3" {
    bucket         = "terraform-state-prod"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

Benefits:
- Team collaboration
- State locking prevents concurrent modifications
- Encryption at rest

## Security in Terraform

### Secrets Management
Never commit secrets. Use vault providers:

```hcl
data "vault_generic_secret" "db_password" {
  path = "secret/data/prod/db"
}

resource "aws_db_instance" "main" {
  password = data.vault_generic_secret.db_password.data["password"]
}
```

### Validation & Compliance
Integrate scanning in CI/CD:

```bash
# Lint
terraform fmt -recursive

# Static analysis
terraform validate

# Security scanning
checkov -d .
tfsec .
```

## 2025 Best Practices

- **Auto-scaling**: Define scaling policies in code (not manual)
- **Tagging Strategy**: Enforce tags for cost attribution and lifecycle
- **Module Versioning**: Pin module versions in source declarations
- **Testing**: Use `terraform test` (new in 1.6+) or Terratest
- **Documentation**: Maintain README for each module with examples
