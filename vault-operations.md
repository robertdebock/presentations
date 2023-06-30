---
title: Vault operations

---

# Vault operations

---

This presentation guides you through the Vault operations you may need to do to properly setup Vault.

---

## Topics:

- Installing
- Updating
- Clustering (HA)
- Scaling
- Initializing
- Unsealing
- Disaster Recovery
- Performance Replication
- Architecture
- Monitoring
- Audit & operational logs
- Backing up and restore

---

## Installing

Vault is a single binary in a couple of version.

- Open Source
- Enterprise
- Enterprise + HSM
- Enterprise FIPS
- Enterprise FIPS + HSM

----

## Installing

There are binaries available for many platforms:

- Darwin
- FreeBSD
- Linux
- NetBSD
- OpenBSD
- Solaris
- Windows

The supported architectures are arm, arm64, 386 and amd64.

----

## Installing

When using a binary, there are a few thinks that need to be done to a system:

- Create directories
- Create a configuration
- Create a group
- Create a user
- Create a startup method (SystemV or systemd)

----

## Installing

Rather than a binary, a package can also be installed:

- Debian-like: https://apt.releases.hashicorp.com
- RedHat-like: https://rpm.releases.hashicorp.com

More [detail](https://www.hashicorp.com/blog/announcing-the-hashicorp-linux-repository).

---

## Updating

Every 2 weeks a new version is [released](https://releases.hashicorp.com/vault/).

Some update contain security fixes and may require a quick installation.

----

## Updating

Simply replace the binary or package. To prevent unnecessary outages, keep this order:

1. Followers of replication secondary/secondaries.
2. Leader of the replication secondary/secondaries.
3. Followers of the replication primary/primaries.
4. Leader of the replication primary/primaries.

> Note: You can technically update in any order, but may experience minor outages when a new leader is elected.

----

## Updating

As updating is required somewhat frequently and is specific; automating this using for example Ansible will prove valuable.

> Note: Please have a non-production environment prepared to exercise the update.

