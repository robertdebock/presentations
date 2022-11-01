---
title: GitLab - Vault

---

# GitLab - Vault

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/gitlab-vault/"/>

---

## What?

GitLab pipelines can use Vault to get secrets, like an AWS account.

---

## Why?

As a developer you don't need AWS credentials.

---

## How?

Vault has two engines that can be used here:

- [JWT](https://developer.hashicorp.com/vault/docs/auth/jwt) authentication engine.
- [AWS](https://developer.hashicorp.com/vault/docs/secrets/aws) secrets engine.

---

## Demo prerequisites 

- A [Vault] instance.
