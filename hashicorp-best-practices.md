---
title: HashiCorp best practices

---

# HashiCorp best practices

---

## Adfinis

----

## Me

---

## Terraform

- Enterprise/Cloud
- CLI

----
## Terraform Enterprise/Cloud

----

## Deployment

Use the [reference architecture](https://developer.hashicorp.com/terraform/enterprise/reference-architecture) to deploy an [operational mode](https://developer.hashicorp.com/terraform/enterprise/operational-modes#operational-modes):

- Stand-alone, simple.
- Active-active, scalable, redundant.
- Mixed, stand-alone, external data.

> Chicken-egg problem.

----

## Ways of working

- [State](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) storage.
- [Watching](https://developer.hashicorp.com/terraform/cloud-docs/vcs) a Git repo.
- [API](https://developer.hashicorp.com/terraform/enterprise/api-docs) driven.

Mix and match.

----

## Development

How to prevent duplicate code?

- Write [reusable modules](https://registry.terraform.io/modules/robertdebock/vault/aws/latest).

----

## Terraform CLI

- Use a (private) [registry](https://registry.terraform.io).
- Test the module using [`examples/*`](https://github.com/robertdebock/terraform-aws-vault/tree/master/examples).
- Version [pin everything](https://github.com/robertdebock/terraform-aws-vault/blob/master/versions.tf).
- [...](https://robertdebock.nl/learn-terraform/ADVANCED/best-practices)

----

## Terraform workspaces

How to [organize workspaces](https://robertdebock.nl/learn-terraform/ADVANCED/terraform-cloud-workspace-design.html):

- Per team
- Per product/service

----

## Terraform Projects

[NEW](https://www.hashicorp.com/blog/terraform-cloud-adds-projects-to-organize-workspaces-at-scale): A collection of workspaces, shared by users/groups.

----

## Operating Terraform

- [Monitoring](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/monitoring).
- [Logging](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/logging).
- [Backup](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/backup-restore).

---

## Vault

A secrets management tool.

----

## Deploying Vault

----

## Deploying Vault method

- Everything as code: precise deployment.

----

## Architecture

- Invest time, use the [reference architecture](https://developer.hashicorp.com/vault/tutorials/day-one-raft/raft-reference-architecture).
- Think of [multi-cluster](https://developer.hashicorp.com/vault/tutorials/day-one-raft/multi-cluster-architecture).
- Maintain a [hardened](https://developer.hashicorp.com/vault/tutorials/day-one-raft/production-hardening) deployment.

----

## Sizing Vault

- CPU: 2, 4 or 8+ cores.
- Memory: 8, 16, 32 or 64 GB.
- disk: 100 GB or 200+ GB.
- Disk speed: 3k IOPS - 10k+ IOPS.

----

## Auto unseal

Unsealing Vault manually is a [human-involved process](https://developer.hashicorp.com/vault/tutorials/recommended-patterns/pattern-unseal#operator-overhead). (Error prone) Auto unsealing simplifies this process.

> Auto unseal is a feature of any Vault type.

----

## Configure Vault

- **[Replication](https://developer.hashicorp.com/vault/docs/enterprise/replication#architecture)**.
- **[Auto snapshot](https://developer.hashicorp.com/vault/docs/enterprise/automated-integrated-storage-snapshots)**.
- Autopilot.
- **Namespaces**.

> Most organization strive for a self-service solution.

----

## Using Vault

----

## Initialize

- How many people have a part of the (recovery or [unseal](https://developer.hashicorp.com/vault/docs/concepts/seal)) key?
- [How many parts](https://developer.hashicorp.com/vault/docs/concepts/seal#shamir-seals) are required to access data?

----

## Authentication

- Use an external system like [LDAP](https://developer.hashicorp.com/vault/docs/auth/ldap).
- Revoke the [root token](https://developer.hashicorp.com/vault/docs/concepts/tokens#root-tokens).
- Leases and tokens consume memory.

----

## Secrets

- [Dynamic secrets engines](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dynamic-secrets) provide short-lived-secrets.
- PKI or EaaS are CPU intensive.

----

## Operating Vault

----

## Montoring

- Use [telemetry](https://developer.hashicorp.com/vault/tutorials/monitoring/monitor-telemetry-grafana-prometheus).
- Write functional tests.
- Measure response speeds for reference.
- Lack of memory will kill Vault.

----

## Procedures

Create hostname/ip specific [runbooks](https://developer.hashicorp.com/vault/tutorials/standard-procedures) for scenarios:

- Quorum loss
- Region/DC loss
- Emergency sealing
- Upgrades
- ...

---

## Conclusion

HashiCorp products have grown up, they are enterprise ready and require knowledge and effort to operate.
