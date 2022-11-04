---
title: GitLab - Vault

---

# About me

---
# GitLab - Vault

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/gitlab-vault/"/>

---

## What?

GitLab pipelines uses Vault to authenticate to AWS

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-state.png"/>

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

- A [Vault](https://registry.terraform.io/modules/robertdebock/vault/aws/latest) instance.

In this case I'll spin it up [here](https://github.com/robertdebock/terraform-gitlab-vault/tree/master/vault).

---

## Demo

Set variables described in [README](https://github.com/robertdebock/terraform-gitlab-vault/blob/master/README.md#setup).

`terraform apply` to:

- Configure Vault
- Create a GitLab repository
