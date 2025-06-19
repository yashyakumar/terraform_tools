# 🚀 DevOps Tooling Summary (Cookiecutter, Tenv, Makefile, Pre-commit)

This is what I have learned today

This document summarizes the key DevOps tools you’ve learned and how to use them effectively in real-world Terraform workflows. You can copy this directly into your GitHub Wiki or internal documentation.

---

## 🧱 Cookiecutter

**Purpose:** Generate reusable project templates with standard structure and customization support.

**Why Use It:**

- Create repeatable file structures for Terraform, Python, Helm, etc.
- Allows dynamic values using `{{cookiecutter.variable}}`

**Key Commands:**

```bash
cookiecutter https://github.com/your-org/your-template-repo
```

**Example Files:**

```hcl
# provider.tf
terraform {
  required_providers {
    {{cookiecutter.provider_name}} = {
      source = "hashicorp/{{cookiecutter.provider_name}}"
      version = "4.33.0"
    }
  }
}
```

---

## 🧰 Tenv (Terraform Version Manager)

**Purpose:** Manage and switch between multiple versions of Terraform on the same machine.

**Why Use It:**

- Different projects may require different Terraform versions.
- Automatically switch using `.terraform-version`

**Key Commands:**

```bash
tenv tf install 1.6.0
tenv tf list
tenv tf use 1.6.0
echo "1.6.0" > .terraform-version
```

---

## 🛠️ Makefile

**Purpose:** Automate repetitive Terraform tasks using `make` commands.

**Why Use It:**

- Clean CLI interface for running common Terraform commands
- Supports multiple environments

**Sample Makefile:**

```makefile
ENV ?= dev
TF_DIR := $(ENV)

init:
	terraform -chdir=$(TF_DIR) init

plan:
	terraform -chdir=$(TF_DIR) plan -var-file=$(ENV).tfvars

apply:
	terraform -chdir=$(TF_DIR) apply -auto-approve -var-file=$(ENV).tfvars

destroy:
	terraform -chdir=$(TF_DIR) destroy -auto-approve -var-file=$(ENV).tfvars

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS=":.*?## "}; {printf "  \033[36m%-10s\033[0m %s\n", $$1, $$2}'
```

---

## 🧪 Pre-Commit Hooks

**Purpose:** Run automatic code checks (formatting, validation, linting) before committing.

**Why Use It:**

- Prevent broken or poorly formatted Terraform code from being committed
- Maintains high code quality automatically

**Installation:**

```bash
pip install pre-commit
pre-commit install
```

**.pre-commit-config.yaml:**

```yaml
repos:
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.79.0
    hooks:
      - id: terraform_fmt
      - id: terraform_validate
      - id: terraform_tflint
```

**Run Hooks Manually:**

```bash
pre-commit run --all-files
```

**Automatically Used On:**

```bash
git commit -m "your message"
```

---

## ✅ Summary

| Tool         | Use Case                              | Why It Matters                            |
| ------------ | ------------------------------------- | ----------------------------------------- |
| Cookiecutter | Scaffold new projects quickly         | Consistency, reusability                  |
| Tenv         | Manage Terraform versions per project | Avoid conflicts, auto-switching           |
| Makefile     | Simplify and document CLI workflows   | Developer efficiency, environment support |
| Pre-commit   | Auto-check code before commit         | Quality, safety, automation               |

---


This is now your clean, reusable foundation for Terraform-based DevOps projects 🚀

