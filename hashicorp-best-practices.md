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

- CLI
- Enterprise/Cloud

----

## Terraform CLI

- Write reusable modules.
- Use a (private) registry.
- Test the module using `examples/*`.
- Version pin everything.
- [...](https://robertdebock.nl/learn-terraform/ADVANCED/best-practices)

----

## Terraform Enterprise/Cloud

----

## Deployment

Use the [reference architecture](https://developer.hashicorp.com/terraform/enterprise/reference-architecture).

- Stand-alone, simple.
- Active-active, scalable, redundant.
- Mixed, stand-alone, external data.

> Chicken-egg problem.

----

## Ways of working

- State storage.
- Watching a Git repo.
- API driven.

Mix and match.

----

## Terraform Private registry

----

## Development

How to prevent duplicate code?

----

## Terraform workspaces

How to organize workspaces:

- Per team
- Per product/service

----

## Terraform Projects

A collection of workspaces, shared by users/groups.

----

## Operating Terraform

Monitoring, logging & backup

---

## Vault

A secrets management tool.

----

## Deploying Vault

----

## Deploying Vault method

- Everything as code: precise deployment.

----

## Sizing Vault

- CPU: 2, 4 or 8+ cores.
- Memory: 8, 16, 32 or 64 GB.
- disk: 100 GB or 200+ GB.
- Disk speed: 3k IOPS - 10k+ IOPS.

----

## Architecture

- Invest time, use the [reference architecture](https://developer.hashicorp.com/vault/tutorials/day-one-raft/raft-reference-architecture).
- Think of [multi-cluster](https://developer.hashicorp.com/vault/tutorials/day-one-raft/multi-cluster-architecture).
- Maintain a [hardened](https://developer.hashicorp.com/vault/tutorials/day-one-raft/production-hardening) deployment.

----

## Auto unseal

Unsealing Vault manually is a human-involved process. (Error prone) Auto unsealing simplifies this process.

----

## Configure Vault

- Replication
- Auto snapshot
- Autopilot
- Namespaces
- Selfservice

----

## Using Vault

----

## Initialize

- How many people have a part of the (recovery or unseal) key?
- How many parts are required to access data?

----

## Authentication

- Use an external system like LDAP.
- Revoke the root token.
- Leases consume memory.

----

## Secrets

- Dynamic secrets engines provide short-lived-secrets.
- PKI or EaaS are CPU intensive.

----

## Operating Vault

----

## Montoring

- Use [telemetry](https://developer.hashicorp.com/vault/tutorials/monitoring/monitor-telemetry-grafana-prometheus).
- Write functional tests.
- Measure response speeds.
- Lack of memory will kill Vault.

----

## Procedures

Create hostname/ip specific runbooks for scenarios:

- Quorum loss
- Region/DC loss
- Emergency sealing
- Upgrades
- ...

---

## Conclusion
