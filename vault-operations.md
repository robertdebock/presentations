---
title: Vault operations

---

# Vault operations

---

This presentation guides you through the Vault operations you may need to do to properly setup Vault.

---

## Topics

- Installing
- Updating
- Clustering (HA)
- Scaling
- Initializing
- Unsealing

----

## Topics 

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

Supported architectures: `arm`, `arm64`, `386` and `amd64` .

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

Some updates contain security fixes and may require a quick installation.

----

## Updating

Simply replace the binary or package. To prevent unnecessary outages, use this order:

1. Followers of replication secondary/secondaries.
2. Leader of the replication secondary/secondaries.
3. Followers of the replication primary/primaries.
4. Leader of the replication primary/primaries.

> Note: You can technically update in any order, but may experience minor outages when a new leader is elected.

----

## Updating

As updating is required somewhat frequently, is specific and critical; automating this using for example Ansible will prove valuable.

> Note: Please have a non-production environment prepared to exercise the update.

---

## Clustering (HA)

You can create a cluster of Vault instances, which can offer redundancy.

> Note: Vault write actions will always be handled by the leader, to performance is not typically improved by more than one machine.

----

## Clustering (HA)

You can manually create a cluster:

```shell
vault operator raft join http://127.0.0.2:8200
```

> Note: Automatic joining is preferred to reduce administrative burden.

----

## Clustering (HA)

Interesting to know:

1. When a Vault instance joins a cluster, a "bootstrap" sequence is started. This happens on port `:tcp/8200` (by default)
2. Cluster communication will happen on port `:tcp:8201` (by default).

----

## Clustering (HA)

3. Both ports should be available to all members of a cluster.
4. Port `:tcp/8200` uses `HTTPS`. The nodes should trust each others certificates. This is typically done by haveing a single certificate for all nodes, having either a wildcard (`*.examples.com`) or a specific Subject Alternate Name ("SAN"). For "SAN", node names need to be known.

----

## Clustering (HA)

Automatic joining is preferred over manual joining. It allows instance replacements to occur without human intervention.

```hcl
  storage "raft" {
  path    = "/vault/vault"
  node_id = "vault_x"
  retry_join {
    auto_join = "provider=aws addr_type=public_v4 tag_key=auto_join tag_value=my_raft_instances region=us-east-1"
    auto_join_scheme = "http"
  }
}
```

----

## Clustering (HA)

Automatically joining uses [`go discovery`](https://github.com/hashicorp/go-discover), which has many "providers", including:

- AWS
- Azure
- vSphere

> Note: "auto join" and "auto unseal" are often confused. They are different concepts.

---

## Scaling

Vault can be scaled. This is done for availability, not for performance.

A Vault cluster needs an odd number of instances, either `3` and `5`. (`5` is recommended, but `3` can be used when no more than `3` "zones" or "lanes" are available.)

You can scale both up and down, from `3` to `5` and back from `5` to `3`.

> Note: Scaling down required auto-pilot to be configured to cleanup ["dead servers"](https://developer.hashicorp.com/vault/docs/concepts/integrated-storage/autopilot#dead-server-cleanup). If this configuration is skipped, quorum may be lost.

----

## Scaling

HashiCorp advices this [sizing](https://developer.hashicorp.com/vault/tutorials/day-one-raft/raft-reference-architecture#hardware-sizing-for-vault-servers) for individual Vault nodes:

| Size  | CPU | Memory (GB) | Disk (GB) Disk IO (IOPS) | Disk thoughput (MB/s) |
| ----- | --- | ----------- | ------------------------ | --------------------- |
| small | 2-4 | 8-16        | 100+                     | 75+                   |
| large | 4-8 | 32-64       | 200+                     | 250+                  |

What `small` and `large` are, is not very explicit.

> Note: Smaller instances can certainly work, but if a Vault instance runs out of memory, the Vault cluster may become unstable. This situation can be difficult to fix.

---

## Initializing

Any Vault node or cluster needs to be initialized. Initializing created an encrypted storage backend and returns the "unseal keys" or "recovery keys" and root-token. (for automatically unsealed instances) **ONCE**.

Unseal/recovery keys can be PGP can be used to prevent plain-text (shared) keys to be displayed.

It's adviced to carefully run this procedure. Although it's done once.

----

## Initializing

The "root-token" or "root-key" is the initial key that allows all actions on Vault.

This token should be revoked as early as possible, after personal users have "high privileged" access to Vault.

> Note: Most Vault installations have the root-token **NOT** revoked, which is a serurity issue.

---

## Unsealing

Every time Vault starts, it needs to be unseal. These are typical situations when unsealing is required:

- Reboot
- Restart
- Crash and start
- Stop, update, start

----

## Unsealing

Manual unsealing requires unsealing as many times as the amount of `key-threshold` set when initializing Vault. The default value for `key-threshold` is `3` out of the `5` `key-shares`.

This means at least 3 people need to enter an unseal key on **each Vault instance**. A typical installation of Vault has  10 to 20 nodes. That would mean 30 to 60 unseal commands.

As you understand, manaul unsealing is not preferred.

---

## Unsealing

You can have Vault unseal automatically in a few way.

- AWS KMS
- Azure Key Vault
- GCP Cloud KMS
- HSM
- Transit

----

## Unsealing

AWS KMS, Azure Key Vault and GCP Cloud KMS use a key on a cloud provider to unseal. Access to such a key becomes critical for availability and sensitive.

> Note You can unseal a non-cloud Vault instance using cloud keys.

----

## Unsealing

An HSM can be used to unseal Vault. This does introduce a dependency on an HSM, but greatly reduces the administrative work required for unsealing.

`Transit` can also be used to unseal Vault. This mechanism unseals Vault using another Vault. This means there is a dependency on that other Vault and the initial Vault needs to be unsealed as well, likely manual.

---

## Disaster Recovery

Vault clusters (or instances) can be related in a "disaster recovery" replication setup.

Disaster Recovery:

- Syncronizes all data.
- Has a primary and secondary
- Secondary can be promoted.
- Secondary does not provide service until promoted.
- Is an Enterprise feature.

----

## Disaster Recovery

The size of a "cluster" (`1`, `3` or `5`) has no impact on DR, you can mix any size.

Disaster Recovery can also be used to migrate from one cluster to another.

---

## Performance Replication

Vault clusters can syncronize (selected) data in a "performance replication" setup.

Performance Replication:

- Syncronizes selected data.
- Has a primary and secondary
- Secondary can be promoted.
- Secondary does provide service.
- Is an Enterprise feature.

---

## Architecture

Here are a few architecture examples.

----

## Architecture single node

```text
+--- Vault-1 ---+
|               |
+---------------+
```

----

## Architecture multiple nodes

```text
                +--- Load balancer ---+
                |                     |
                +---------------------+
                  /        |         \
                 /         |          \
+--- Vault-1 ---+  +--- Vault-2 ---+   +--- Vault-3 ---+
|               |  |               |   |               |
+---------------+  +---------------+   +---------------+
```

- Low latency connection.
- Different availability zones.

----

## Architecture multiple clusters DR

```text
                  +--- load balancer ---+
                  |                     |
                  +---------------------+
                    /                   \
                   /                     \
+---- load balancer ----+          +---- load balancer ----+
|                       |          |                       |
+-----------------------+          +-----------------------+
            |                                  |
            V                                  V
+--- Vault-cluster-1 ---+          +--- Vault-cluster-2 ---+
|                       | -> DR -> |                       |
+-----------------------+          +-----------------------+
```

- Connection quite forgiving.
- Different regions.

----

## Architecture multiple clusters PR

```text
                  +--- load balancer ---+
                  |                     |
                  +---------------------+
                    /                   \
                   /                     \
+---- load balancer ----+          +---- load balancer ----+
|                       |          |                       |
+-----------------------+          +-----------------------+
            |                                  |
            V                                  V

+--- Vault-cluster-1 ---+          +--- Vault-cluster-2 ---+
|                       | -> PR -> |                       |
+-----------------------+          +-----------------------+
```

- Connection quite forgiving.
- Different regions.

----

## Architecture multiple clusters DR and PR

```text
+---- load balancer ----+          +---- load balancer ----+
|                       |          |                       |<--------------+
+-----------------------+          +-----------------------+               |
            |                                  |                           |
            V                                  V                           |
+--- Vault-cluster-1 ---+          +--- Vault-cluster-2 ---+               |
|                       | -> PR -> |                       |               |
+-----------------------+          +-----------------------+               |
           |                                  |                            |
           V                                  V                 +--- load balancer ---+
          DR                                  DR                | Also on other side. |
           |                                  |                 +---------------------+
           V                                  V                            |
+--- Vault-cluster-3 ---+          +--- Vault-cluster-4 ---+               |
|                       |          |                       |               |
+-----------------------+          +-----------------------+               |
           ^                                  ^                            |
           |                                  |                            |
+---- load balancer ----+          +---- load balancer ----+               |
|                       |          |                       |<--------------+
+-----------------------+          +-----------------------+
```
```
