---
title: HashiCorp best practices

---

## HashiCorp best practices

---

## Adfinis

----

## Me

---

## Terraform

- Enterprise/Cloud
- CLI

----

## Terraform Enterprise

----

```text
                +--- TF ENT ---+    +--- Providers ---+
 \0/   +---+    | - registry   | -> | - AWS, GCP, ... |
  | -> | C | -> | - runtime    | <- | - VSphere       |
 / \   +---+    | - state      |    | - ...           |
                +--------------+    +-----------------+
```

----

## Deployment

Use the [reference architecture](https://developer.hashicorp.com/terraform/enterprise/reference-architecture).

- VMs: "Replicated depoyment".
- Containers: "Flexible deployment".

----

## Scoping

[Operational mode](https://developer.hashicorp.com/terraform/enterprise/operational-modes#operational-modes):

1. Mounted on disk, simple.
2. Mixed, stand-alone, external data. (db + objects)
3. Active-active, scalable, redundant, complex!

> External services need to be available.

----

## Sizing Terraform Enterprise

- CPU: 4+ cores.
- Memory: 8+ GB.
- disk: 10 GB (OS) + 40 GB /var/lib/docker

----

## Requirements

Listed [here]:

- Debian 10/11
- Ubuntu 18.04/20.04
- RHEL 7/8
- CentOS 7/8
- Amazon Linux 2.0
- Oracle Linux 7/8

----

## Ways of working

- [State](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) storage.
- [Watching](https://developer.hashicorp.com/terraform/cloud-docs/vcs) a Git repo.
- [API](https://developer.hashicorp.com/terraform/enterprise/api-docs) driven.

Mix and match.

> Set per workspace.

----

## Terraform workspaces

How to [organize workspaces](https://robertdebock.nl/learn-terraform/ADVANCED/terraform-cloud-workspace-design.html):

- Per team
- Per product/service

----

## Terraform Projects

[NEW](https://www.hashicorp.com/blog/terraform-cloud-adds-projects-to-organize-workspaces-at-scale): A collection of workspaces, shared by users/groups.

----

## Terraform Organizations

An organization contains projects and workspaces.

Users and teams are members of an organization.

----

## Operating Terraform

- [Monitoring](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/monitoring).
- [Logging](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/logging).
- [Backup](https://developer.hashicorp.com/terraform/enterprise/admin/infrastructure/backup-restore).

----

## Development

How to prevent duplicate code?

- Write [reusable modules](https://registry.terraform.io/modules/robertdebock/vault/aws/latest).
- Publish. (public or private)

----

## Terraform CLI

- Use a (private) [registry](https://registry.terraform.io).
- Test the module using [`examples/*`](https://github.com/robertdebock/terraform-aws-vault/tree/master/examples).
- Version [pin everything](https://github.com/robertdebock/terraform-aws-vault/blob/master/versions.tf).
- [...](https://robertdebock.nl/learn-terraform/ADVANCED/best-practices)

---

## Vault

A secrets management tool.

----

```text
\0/    +--- Authentication ----+    +--- Policies ---+    +---- Secrets ----+
 |  -> | - userpass, ldap, ... | -> |                | -> | - key value     |
/ \    +-----------------------+    +----------------+    | - ldap, db, ... |
                ^                                         +-----------------+
                |                                                  |
 |===|          |                                         +--- External ----+
 |===| ---------+                                         |                 |
 |===|                                                    +-----------------+
```

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
- Disk size: 100 GB or 200+ GB.
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

1. Machine based (approle, jwt, token, etc.)
2. Human based (userpass, ldap, github, etc.)

----

## Auth hints

- Use an external system like [LDAP](https://developer.hashicorp.com/vault/docs/auth/ldap).
- Revoke the [root token](https://developer.hashicorp.com/vault/docs/concepts/tokens#root-tokens).

----

## Leases

Leases consume memory, the default lease time is 30 days.

- Humans: 16 hours or so.
- Machines: 1 hour or so.

----

## Secrets

- [Dynamic secrets engines](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dynamic-secrets) provide short-lived-secrets.
- KeyValue is most popular.
- PKI or EaaS are CPU intensive.

----

## Operating Vault

----

## Montoring 1/2

- Use [telemetry](https://developer.hashicorp.com/vault/tutorials/monitoring/monitor-telemetry-grafana-prometheus).
- Write functional tests.
- Measure response speeds for reference.
- Check usage (trending) and plan for growth.

----

## Monitoring 2/2

- Send audit logs to a SIEM.
- Send service logs somewhere.
- Lack of memory will kill Vault.

----

## Procedures

Create hostname/ip specific [runbooks](https://developer.hashicorp.com/vault/tutorials/standard-procedures) for scenarios:

- Quorum loss
- Region/DC loss
- Emergency sealing
- Upgrades
- ...

----

## Namepaces

A namespace is a "virtual Vault" within a Vault cluster. Namespaces are isolated from each other and can be used by different teams.

- A namespace per product/service.
- Use [policies](https://developer.hashicorp.com/vault/tutorials/identity/policies) to control access.
- Monitor client count.
- Keep is flat.

---

## Conclusion

HashiCorp products are mature, they are enterprise ready and require knowledge and effort to operate.
