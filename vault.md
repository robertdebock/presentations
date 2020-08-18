---
title: Vault

---

# Vault

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/vault/"/>

---

# What

A secrets store.

----

# What more

- CLI and API.
- Multiple "engines".
- Mutliple persistent storage backends.

---

# Why?

Shared secrets can be compromised.

- Un-intended usage of secrets.

---

# Engines

----

# Secret

A simple key-value store

```
vault kv put secret/hello foo=world
vault kv get secret/hello
vault kv delete secret/hello
```

----

# Dynamic secret 1/4

A on-the-spot-generated credential.

```
vault secrets enable -path=aws aws
vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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 \
    region=us-east-1
```

----

# Dynamic secret 2/4

```
$ vault write aws/roles/my-role \
        credential_type=iam_user \
        policy_document=-<<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1426528957000",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
EOF
```

----

# Dynamic secrets 3/4

```
vault read aws/creds/my-role
Key                Value
---                -----
lease_id           aws/creds/my-role/0bce0782-32aa-25ec-f61d-c026ff22106e
lease_duration     768h
lease_renewable    true
access_key         AKIAJELUDIANQGRXCTZQ
secret_key         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
security_token     <nil>
```

----

# Dynamic secrets 4/4

```
vault lease revoke aws/creds/my-role/0bce0782-32aa-25ec-f61d-c026ff22106
Success! Revoked lease: aws/creds/my-role/0bce0782-32aa-25ec-f61d-c026ff22106e
```
