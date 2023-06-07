---
title: HashiCorp best practices

---

# HashiCorp best practices

---

## Adfinis

---

## Me

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
- Disk speed: 3k IOPS - 10k+ IOPS

----

## Architecture

- Invest time, use the [reference architecture](https://developer.hashicorp.com/vault/tutorials/day-one-raft/raft-reference-architecture).
- Think of [multi-cluster](https://developer.hashicorp.com/vault/tutorials/day-one-raft/multi-cluster-architecture).
- Maintain a [hardened](https://developer.hashicorp.com/vault/tutorials/day-one-raft/production-hardening) deployment.

---

## Auto unseal

Unsealing Vault manually is a human-involved process. (Error prone) Auto unsealing simplifies this process.

----

## Configure Vault

- Replication
- Auto snapshot
- Namespaces
- Selfservice

----

## Using Vault

----

## Operating Vault

----

## Montoring

----

## Procedures

Create hostname/ip specific runbooks for scernarios:

- Quorum loss
- Region/DC loss
- Emergency sealing
- Upgrades
- ...

---

## Terraform

- CLI
- Cloud
- Enterprise

----

## Terraform CLI

----

## Terraform Cloud

----

## Terraform Enterprise

----

## Terraform workspaces

----

## Operating Terraform

---

## Conclusion
