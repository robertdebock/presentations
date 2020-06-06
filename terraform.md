---
title: Terraform

---

# Terraform

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/terraform/"/>

---

# What

Infrastructure as code

---

# Why

- Much quicker.
- Repeatable.
- Versionable & history.
- Easy collaboration.
- Teams can own their infra.

---

# Examples

Mostly stolen from the [Terraform documentation](https://www.terraform.io/docs/providers/do/).

----

# Define

```hcl
resource "digitalocean_droplet" "web-1" {
  image  = "ubuntu-18-04-x64"
  name   = "web-1"
  region = "ams3"
  size   = "s-1vcpu-1gb"
}
```

----

# Passing data

```hcl
resource "digitalocean_loadbalancer" "public" {
  name   = "loadbalancer-1"
  region = "ams3"

  # (Non-relevant lines removed.)

  droplet_ids = [digitalocean_droplet.web-1.id]
}
```

---

# State

Terraform keeps state, which allows:

- Multiple repositories.
- Determine differences.

Nota bene; Don't loose the state...

---

# Dry-run

```bash
terraform plan
```

----

The most relevant lines:

```
  # digitalocean_droplet.web-1 will be created
  + resource "digitalocean_droplet" "web-1" {
      + ipv4_address         = (known after apply)
      + name                 = "web-1"
      + region               = "ams3"
      + size                 = "s-1vcpu-1gb"
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

---

# Apply!

```bash
terraform apply
```

----

![terraform apply output](images/terraform.gif)

---

# Save money

```bash
terraform destroy
```

---

# Terraform & Ansible

[Better together](https://www.hashicorp.com/resources/ansible-terraform-better-together/).

----

# Demo time

[https://github.com/robertdebock/terraform-demo](https://github.com/robertdebock/terraform-demo)

Three branches:
- [master](https://github.com/robertdebock/terraform-demo): Start with this one.
- [two](https://github.com/robertdebock/terraform-demo/two/three): A little more complex.
- [three](https://github.com/robertdebock/terraform-demo/tree/three): Provisioning with Ansible.

---

# Conclusion

- Infrastructure components are just "resources".
- Easy to read, learn and contribute.
- Ansible is great. ;-)
