---
title: Secrets management

---

# Secrets management

A general discussion on how to manage secrets.

---

## What are secrets?

Secrets are sensitive data:

- Credentials (passwords, tokens, keys)

---

## How it was

Secrets are (statically) stored in a configuration file.

```text
+--- application ---+    +--- backend ---+
| - application.cnf | -> | - user + pass |
+-------------------+    +---------------+
```

----

## Issues with how it was

1. A person would have to know the credentials.
2. Changing the credentials would require careful coordination.
3. The credentials can be stored in a repository/image.
4. Who has access to the credentials is vague.
5. A leaked secret can cause a lot of damage.

---

## How it is

Secrets are obtained from specific providers.

- AWS Secrets Manager.
- Azure Key Vault.
- Google Cloud KMS.
- Kubernetes Secrets.
- Gitlab Secrets.
- GitHub Secrets.

----

```text
+--- AWS ----+   +--- Azure ---+   +--- GCP ----+
| KMS        |   | Key Vault   |   | KMS        |
| (policies) |   | (policies)  |   | (policies) |
+------------+   +-------------+   +------------+

+--- K8s ---+   +--- Gitlab ---+  +--- GitHub ---+
| Secrets   |   | Secrets      |  | Secrets      |
| (policies)|   | (policies)   |  | (policies)   |
+-----------+   +--------------+  +--------------+
```

----

## Issues with how it is

1. Secrets management is specific to a provider.
2. Policies are specific to a provider.
3. Secrets can be exfiltrated.
4. A leaked secret can cause a lot of damage.

---

## How is can be

Secrets are obtained from a generic provider.

- Hashicorp Vault.
- CyberArk Conjur.
- AKeyless.

----

## Benefits of how it can be

1. Secrets management is generic.
2. Policies are generic.
4. Leaking secrets is less relevant. (*)

- * = Only when using just-in-time-credentials.
