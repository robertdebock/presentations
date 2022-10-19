---
title: Terraform new things
---

# Terraform new things

---

# What

New features in Terraform.

---

# Terraform 0.13 (2020)

----

# Validation

Verify if an input variable meets expectations.

Can be used in a `variable` object.

----

```hcl
variable "vault_name" {
  description = "The name of the vault cluster."
  type        = string
  default     = "unset"
  validation {
    condition     = length(var.vault_name) >= 3 && length(var.vault_name) <= 5 && var.vault_name != "default"
    error_message = "Please use a minimum of 3 and a maximum of 5 characters. \"default\" is reserved."
  }
}
```
---

# Terraform 1.2.0 (2022)

----

# Precondition

To check if a resource meets the requirements to be usable.

(Pretty close to validation)

Can be used in a `resource`, `data` and `output` object.

----

```hcl
...
  lifecycle {
    precondition {
      condition     = length(var.vault_name) >= 3 && length(var.vault_name) <= 5 && var.vault_name != "default"
      error_message = "Please use a minimum of 3 and a maximum of 5 characters. \"default\" is reserved."
    }
  }
...

```

----

# Postcondition

To check if an existing resource meets the requirements.

Can be used in a `resource`, `data` and `output` object.

----

```hcl
data "aws_internet_gateway" "default" {
  filter {
    name   = "attachment.vpc-id"
    values = [var.vpc_id]
  }
  lifecycle {
    postcondition {
      condition     = length(self) != 1
      error_message = "No Internet Gateway was found in the VPC."
    }
  }
}
```
---

# Terraform 1.3.0 (2022)

---
