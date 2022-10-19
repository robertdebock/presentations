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
variable "cluster_name" {
  description = "The name of the cluster."
  type        = string
  default     = "unset"
  validation {
    condition     = var.vault_name != "default"
    error_message = "The name \"default\" is reserved, please use another name."
  }
}
```
---

# Terraform 1.2.0 (2022)

----

# Precondition

To check if a resource meets the requirements to be usable.

Can be used in a `resource`, `data` and `output` object.

----

First, read the availability zones that can be deployed to.

```hcl
# Read the availability zones.
data "aws_availability_zones" "default" {
  state = "available"
}
```

---

Next, make subnets if there are sufficient availability zones.

```hcl
resource "aws_subnet" "private" {
  count             = length(data.aws_availability_zones.default.names)
  availability_zone = data.aws_availability_zones.default.names[count.index]
  ...
  lifecycle {
    precondition {
      condition     = length(data.aws_availability_zones.default.names) < 3
      error_message = "There are less than 3 availability zones, need 3 or more."
    }
  }
}
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

# Optional attributes

In variables that are objects, the attributes can now be marked as optional.

```hcl
variable "some_variable" {
  type = object({
    name = string
    type = optional(string)
    size = optional(number, 23)
  })
  default {
    name = "Robert"
  }
}
```

----

```hcl
output "here" {
  value = var.some_variable
}
```

Renders to:
```
some_variable = {
  name = "Robert"
  type = null
  size = 23
}
```
