---
title: GitLab and Vault

---

# GitLab and Vault

Deploy to AWS without knowing AWS credentials

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/gitlab-vault/"/>

---

# About me

- Father of [3 children](https://raw.githubusercontent.com/robertdebock/presentations/master/images/3.jpg).
- Husband of [1 wife](https://mylucie.com).
- Love to [tinker](https://robertdebock.nl/).
- Specialized in infrastructure.

---

# About Adfinis

- .ch, .nl & .au
- Nerd culture, focus on open source solutions.
- Partners of HashiCorp, GitLab, Red Hat, Suse and others.

---

# What?

GitLab pipelines using Vault to authenticate to AWS.

<img height="50%" width="50%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-vault.png"/>

---

# Why?

- Developers don't need AWS credentials.
- Short lived AWS credentials.

---

# How?

1. Create a Vault instance.
2. Configure Vault.
3. Create a GitLab project.
4. Add Terraform files.
5. Have GitLab deploy.

----

# Notes

- Mostly pseudo code in presentation.

---

# Create a Vault instance

- A [Vault](https://registry.terraform.io/modules/robertdebock/vault/aws/latest) instance.

In this case I'll spin it up [here](https://github.com/robertdebock/terraform-gitlab-vault/tree/master/vault).

----

# Vault instance details.

- A highly available cluster (3-5)
- Automatically unsealed
- Automatically patched
- With CloudWatch

---

# Configure Vault

Vault has two engines that can be used here:

- [JWT](https://developer.hashicorp.com/vault/docs/auth/jwt) authentication engine.
- [AWS](https://developer.hashicorp.com/vault/docs/secrets/aws) secrets engine.

----

# Terraform to configure Vault

```hcl
resource "vault_aws_secret_backend" "default" {
  description               = "AWS secret engine."
  path                      = "aws"
  access_key                = var.aws_access_key
  secret_key                = var.aws_secret_key
  default_lease_ttl_seconds = 300
  max_lease_ttl_seconds     = 600
}
```

See [here](https://github.com/robertdebock/terraform-gitlab-vault/blob/master/main.tf).

---

## Create a GitLab project

```hcl
# Make a GitLab project.
resource "gitlab_project" "default" {
  name                   = "vault-aws-demo"
  description            = "A demo to use HashiCorp Vault in a GitLab pipeline."
  default_branch         = "main"
  shared_runners_enabled = false
  visibility_level       = "public"
  depends_on             = [vault_jwt_auth_backend.default]
}
```

---
# Add files

In this case using Terraform. Normally for developers.

```hcl
# Add a Terraform file to the GitLab project.
resource "gitlab_repository_file" "maintf" {
  project        = gitlab_project.default.id
  file_path      = "main.tf"
  branch         = "main"
  content        = base64encode(file("${path.module}/files/main.tf"))
  author_email   = "robert@meinit.nl"
  author_name    = "Robert de Bock"
  commit_message = "Add main.tf."
}
```

---
# GitLab deploys

GitLab pipeline is authenticated to deploy.

---

Set variables described in [README](https://github.com/robertdebock/terraform-gitlab-vault/blob/master/README.md#setup).

`terraform apply` to:

- Configure Vault
- Create a GitLab repository

---

# Conclusion

- A developer does not need AWS credentials.
- Short lived secrets can leak.
- Same for [DBs](https://developer.hashicorp.com/vault/tutorials/db-credentials/database-secrets) [Azure](https://developer.hashicorp.com/vault/docs/secrets/azure), [GCP](https://developer.hashicorp.com/vault/docs/secrets/gcp), and others.
