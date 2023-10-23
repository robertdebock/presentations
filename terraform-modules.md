---
title: Terraform modules

---

# Terraform modules

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=300x300&data=https://robertdebock.nl/presentations/terraform-modules/"/>

---

## About me

- Love to [tinker](https://robertdebock.nl/).
- Specialized in IaC, Terraform and Ansible.

---

## What

A Terraform module is a collection of Terraform code that can be used to create resources.

---

Terraform coding options:

- "Think yourself".
- "Consume something".

---

## Benefits "Think"

- You'll learn a lot.
- You'll understand what you're doing.
- You'll be able to fix things.
- You'll be able to extend things.

----

## Drawbacks "Think"

- Can be difficult/time consuming.
- Many edge-cases.

---

## Benefits "Consume"

- Just use something.
- No need to understand everything.

----

## Drawbacks "Consume"

- You many want something a little different.
- Maybe your general approach differs.

---

## Let's see some code!

----

## Think

```hcl
# Upload an SSH key.
resource "hcloud_ssh_key" "default" {
  count      = var.ssh_key_name == "" ? 1 : 0
  name       = var.name
  public_key = file("~/.ssh/id_rsa.pub")
}
```

----

## Consume

```hcl
module "hcloud_ssh_key" {
  source  = "dhoppeIT/ssh_key/hcloud"
  version = "~> 0.2"
  name       = "my_key"
  public_key = "~/.ssh/id_rsa.pub"
}```

---

## The Registry

A [place](https://registry.terraform.io) to find (and publish) modules.

----

## How to select

- Version.
- Downloads.
- Publication date.
- Managed by.
- Examples.
- CI/tests.

----

## What modules do

- Resouce specific. ([AWS VPC](https://github.com/terraform-aws-modules/terraform-aws-vpc))
- Solution specific. ([HashiCorp Vault on AWS](https://registry.terraform.io/modules/robertdebock/vault/aws/latest))

----

## Interesting

- The name of a module must include the cloud provider.
- There are modules that do not create resources. ([Example](https://registry.terraform.io/modules/cloudposse/label/null/latest)).

----

## Inner sourcing

[Terraform Enterprise](https://www.hashicorp.com/products/terraform/pricing) offers a place to host modules on-premise. It can also apply your Terraform code.

---

## CAF

Microsoft has a [Cloud Adoption Framework](https://registry.terraform.io/modules/aztfmod/caf/azurerm/latest), a "super-module" that can deploy a lot of things.

---

## Conclusion

---

## Questions?

---

## THANKS!
