
# infra-azure-terraform

Infrastructure-as-Code (IaC) for Azure — modular, environment-based Terraform setup

## Overview

This repository contains Terraform code to manage Azure infrastructure in a modular and environment-driven fashion.  
The code is organized so that common reusable modules live under `module/` and each environment’s configuration lives under `environments/`.  
CI/CD is configured via Azure DevOps pipelines (`azure-pipelines.yml` / `ado_pipeline_optimize.yaml`) to enable automated `terraform init`, `plan` and `apply`.  

---

## Repository Layout
/
├── module/ # Reusable Terraform modules (network, storage, etc.)
├── environments/ # Environment-specific configurations (e.g. dev, prod)
│ ├── <env-name>/ # Example: dev/, prod/
│ ├── *.tf # Terraform configs for this environment
│ ├── *.tfvars # Variable definitions
│ └── README.md # (Optional) docs for this env
├── .gitignore
├── azure-pipelines.yml # Azure DevOps pipeline definition (init → plan → apply)
├── ado_pipeline_optimize.yaml # (Optional) alternative/optimized pipeline config
└── README.md


---

## Getting Started (Local / Manual)

> Use this if you want to run Terraform manually (outside of CI/CD pipeline).

1. Install [Terraform](https://www.terraform.io/downloads) if not already installed.  
2. Copy or create an environment folder under `environments/`, e.g. `environments/dev/`.  
3. Populate `*.tfvars` (variables) with appropriate values (subscription, resource-names, region, etc.).  
4. From that environment directory, run:

```bash
terraform init
terraform plan -var-file="yourenv.tfvars"
terraform apply -var-file="yourenv.tfvars"



Why This Structure?

Modularity & Reusability — Terraform modules under module/ avoid duplication, and can be reused across different environments.

Environment Isolation — Each environment has its own directory and state, avoiding interference (dev / prod separation).

Automation & Consistency — CI/CD ensures that every infrastructure change runs the same steps: init → plan → apply.

Maintainability — Changes to shared infrastructure are easier to manage; environment-specific tweaks do not affect common modules.

This structure is similar to recommended patterns used widely in Azure + Terraform projects



How to Add a New Environment

Copy one of the existing environment folders under environments/, and rename it (e.g. dev → staging)

Update the variable file(s) (*.tfvars) with correct names, resource prefixes, region, backend key, etc.

(Optional) If needed, override or extend module configurations via variable overrides

Commit and push — the CI/CD pipeline will pick up the new environment automatically

