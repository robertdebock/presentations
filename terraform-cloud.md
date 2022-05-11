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
- Registry
- Environments/workspaces

----

# Why?

- Easy/easier CI.
- Policy-as-code. (Sentinel)
- Cost estimation.
- Host modules locally.
- Sharing remote state.

---

# How

- [VCS workflow](https://www.terraform.io/cloud-docs/run/ui).
- [CLI workflow](https://www.terraform.io/cloud-docs/run/cli).
- [API workflow](https://www.terraform.io/cloud-docs/run/api).

----

# VCS

Trigger a plan on commits/pushes.

- GitHub
- GitLab
- BitBucket
- Azure Devlops

---

# VCS Demo

1. Commit [here](https://github.com/robertdebock/git-terraform-demo).
2. Review [Terraform Cloud](https://app.terraform.io/app/robertdebock/workspaces/git-terraform-demo/runs/).
3. Plan is automatic, apply manual. (Can be automatic.)

---

# Alternatives

----

# Alternative State

- GitLab.
- (AWS S3|(Azure|GCP) Storage).
- Locally.

----

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
