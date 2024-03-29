---
title: Vault version

---

## Vault versions

---

## Versions

There are [multiple version or types](https://www.hashicorp.com/products/vault/pricing) of HashiCorp Vault.

- [Open Source](https://github.com/hashicorp/vault) (`oss`).
- [Cloud](https://cloud.hashicorp.com/products/vault/pricing) (`hcp`).
- [Enterprise](https://www.vaultproject.io/docs/enterprise) (`ent`).

---

All types support:

- High availability. (3-5 node cluster.)
- All [secret engines](https://www.vaultproject.io/docs/secrets), except [KMIP](https://learn.hashicorp.com/tutorials/vault/kmip-engine?in=vault/enterprise).
- All [authentication engines](https://www.vaultproject.io/docs/auth).

---

## HCP variations

- Development.
- Starter.
- Standard.
- Plus.

[Source](https://www.hashicorp.com/blog/multi-region-replication-now-available-with-hcp-vault).

----

## HCP Development

- Single node.
- Up to 25 clients.

----

## HCP Starter

- Three (3) node deployment.
- Up to 25 clients.

----

## HCP Standard

- Three (3) larger node deployment.
- Any number of clients.

----

## HCP Plus

- Three (3) larger node deployment.
- Any number of clients.
- Performance Replication.

---

## Enterprise variation

- Standard.
- Plus.
- Premium.

----

## Vault Enterprise Standard

- [Disaster Recovery](https://www.vaultproject.io/docs/enterprise/replication).
- [Namespaces](https://www.vaultproject.io/docs/enterprise/namespaces).
- [Monitoring and Telemetry](https://www.vaultproject.io/docs/internals/telemetry).

----

## Vault Enterprise Plus

"Governance & Policy"

- All "Standard" features.
- [HSM Auto-unseal](https://www.vaultproject.io/docs/enterprise/hsm).
- [Multi-factor Authentication](https://www.vaultproject.io/docs/enterprise/mfa).
- [Sentinel Integration](https://www.vaultproject.io/docs/enterprise/sentinel).
- [FIPS 140-2 & Seal Wrap](https://www.vaultproject.io/docs/enterprise/fips).
- [Entropy Augmentation](https://www.vaultproject.io/docs/enterprise/entropy-augmentation).
- [Lease Count Quotas](https://www.vaultproject.io/docs/enterprise/lease-count-quotas).
- [Control Groups](https://www.vaultproject.io/docs/enterprise/control-groups).

----

## Premium

"Multi-datacenter & Scale"

- All "Plus" features.
- [Performance Replication](https://www.vaultproject.io/docs/enterprise/replication).
- Replication Filters.
- Read Replicas.
- [Path Filters](https://www.vaultproject.io/docs/enterprise/replication#paths-filter).

---

## Feature comparison

|type|H.A.|D.R.|N.S.|S.S.|
|----|----|----|----|----|
|oss |Yes |    |    |    |
|hcp |Yes |    |Yes |Yes |
|ent |Yes |Yes |Yes |Yes |

H.A. = High availabilty. D.R. = Disaster Recovery, N.S. = Namespacing, S.S. = SnapShot

[Source](https://cloud.hashicorp.com/docs/vault#feature-parity).

---

## Summary

- oss - Do it yourself.
- ent
  - standard - Disaster Recovery.
  - plus - HSM
  - premium - Performance Repication.
- hcp
  - developer - Try it.
  - starter - Small deployment.
  - standard - Larger deployment.
  - plus - Performance Replication.
