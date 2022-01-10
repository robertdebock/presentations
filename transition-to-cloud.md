---
title: Transition to Cloud

---

# Transition to Cloud

----

# Introduction

- Adfinis
- Robert de Bock

----

# Agenda

1. What
2. Why
3. How

----

# What?

| Traditional   | Cloud                  |
|---------------|------------------------|
| on-prem       | on-somebody-elses-prem |
| "in control"  | "exposed"              |
| Teams-service | Self-service           |
| ITSM, Prince2 | DevOps, Agile          |

----

# Why?

- Speed.
- Talent.
- Growth.
- Compliance.
- Repeatability.

----

# How?

A few key ingredients are required:

- A version control system.
- Tooling to support "IaC".
- Tooling for secret management.

---

# VCS

- Stores code for both Dev and Ops.
- Runs tests and audits (automated).
- Stores artefacts.

Generally; keep development efforts close to the code.

---

# GitLab

- On-prem, on-somebody-elses-prem or SaaS.
- OpenSource (Free, Premium and Ultimate).
- Supported (Premium and Ultimate).
- Very [feature rich](https://about.gitlab.com/features/).

---

# Demo

[gitlab.meinit.nl](https://gitlab.meinit.nl/)

- username: root
- password: Demo-123!

---

# Competitors

- GitHub
- BitBucket
- Gitea
- ... (many more)

----

> "Infrastructure as code (IaC) is the process of managing and provisioning computer data centers through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools"

- [Source](https://en.wikipedia.org/wiki/Infrastructure_as_code).

---

# Example tools

- Terraform.
- Ansible, Chef, Puppet, Salt, etc.

---

# Terraform

Describe (compute) resources in a fairly readable way:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-005e54dee72cc1d00"
  instance_type = "t3.micro"
}
```

---

# Benefits of Terraform

- Broadly adopted.
- Supports [many providers](https://registry.terraform.io/browse/providers).
- [Modules available](https://registry.terraform.io/browse/modules).
- Rather easy to learn.
- Not just "forward".

----

# Container orchestration platform

- Containers need to be orchestrated.
- Kubernetes (K8s) is widely used.
- It's not easy to manage K8s clusters. (manually)

---

# Rancher

Can provision and manage K8s clusters.

## Alternatives:

- Managed K8s: AKS, EKS, GKE, etc.

---

# Hands-on!

[rancher.meinit.nl](https://rancher.meinit.nl/)

----

# Secret management

- More instances = more secrets.
- Cloud = not trusted (zero-trust).

---

# HashiCorp Vault

Authentication using:

- Kerberos.
- Amazon AWS.
- MicroSoft Azure.
- Google Cloud.
- [many others](https://www.vaultproject.io/docs/auth).

---

# HashiCorp Vault

Secrets for:

- Active Directory.
- AWS, Azure, GCP.
- Static secrets.
- Certificates.
- [many others](https://www.vaultproject.io/docs/secrets).

---

# The beauty!

[Dynamic Secrets](https://learn.hashicorp.com/tutorials/vault/database-secrets#solution).

----

# Conclusion

IaC requires:
- Changes to the organization.
- Some tools.
- Some skills.
- Different way of thinking.

----

# Questions?

Thanks!
