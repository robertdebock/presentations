---
title: Terraform Cloud
---

# Terraform Cloud

---

# What

A managed runtime for Terraform.

- Users/teams
- Variables
- State
- Sentinel
- **Registry**
- Environments/workspaces

----

# Registry

A Terraform modules catalogue.

---

# Requirements

- [Supported VCS provider](https://www.terraform.io/cloud-docs/vcs#supported-vcs-providers)
- Repository name: terraform-{{ provider }}-{{ name }}
- [Correct module structure](https://www.terraform.io/language/modules/develop/structure).
- Tagged ([SEM-versioning](https://semver.org).)

[Source](https://www.terraform.io/cloud-docs/registry/publish-modules#preparing-a-module-repository)

---

# Example 1/3

This [repository](https://github.com/robertdebock/terraform-azurerm-scale-set/) meets the requirements.

---

# Example 2/3

1. [Connect a VCS](https://app.terraform.io/app/robertdebock/settings/version-control).

---

# Example 3/3

1. Visit [Terraform Cloud](https://app.terraform.io/app/robertdebock/registry/private/providers)
2. Publish -> Module -> GitHub
3. Select the repository -> Publish module

---

# Modules

- Great to help teams up to speed.
- Good to abstract functionality.
- Not for policies.

----

# Terraform runtime

Terraform cloud instead of "your laptop".

---

# How

- [VCS workflow](https://www.terraform.io/cloud-docs/run/ui).
- [CLI workflow](https://www.terraform.io/cloud-docs/run/cli).
- [API workflow](https://www.terraform.io/cloud-docs/run/api).

---

# VCS

Trigger a plan on commits/pushes.

- GitHub
- GitLab
- BitBucket
- Azure DevOps

---

# VCS Demo

1. Commit [here](https://github.com/robertdebock/git-terraform-demo).
2. Review [Terraform Cloud](https://app.terraform.io/app/robertdebock/workspaces/git-terraform-demo/runs/).
3. Plan is automatic, apply manual. (Can be automatic.)

---

# Product offerings

1. Terraform - no cloud/enterprise/business
2. Terraform - Cloud
3. Terraform - Cloud for business
4. Terraform - Enterprise

[Source](https://cloud.hashicorp.com/products/terraform/pricing).

----

# Alternatives

Terraform Cloud is complete, easy to use and supported. There are alternatives.

---

# Alternative State

- GitLab.
- (AWS S3|(Azure|GCP) Storage).
- Locally.

---

# Alternative Registry

- GitLab. (!)
- Terraform Enterprise.
- A manual index of modules.

---

# Alternative Sentinel

- [Open Policy Agent](https://www.openpolicyagent.org/docs/latest/terraform/).
- Hyper scaler policies.

---

# Alternative cost-estimation

- [Infracost](https://www.infracost.io).
- [Terraform cost estimation](https://github.com/antonbabenko/terraform-cost-estimation). (!)

----

# Conclusion

Terraform Cloud can offer value, it's an easy-to-use SaaS offering. Terraform Enterprise is an alternative for an on-premise solution.
