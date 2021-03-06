---
title: Cloud Native

---

# Cloud Native

> Cloud native computing is an approach in software development that utilizes cloud computing to "build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds"

[source](https://en.wikipedia.org/wiki/Cloud_native_computing)

---

# Automation is key

*Everything* needs to be automated:

- Spinning up machines.
- Configuring machines.
- Authenticating to resources.

---

# Required tools

1. Some version control system.
2. An API to a hyper visor or cloud provider.
3. Knowledge and time.

---

# Terraform

Spinning up resources with [HashiCorp Terraform](https://www.terraform.io/).

----

# Define machines

```hcl
resource "vsphere_virtual_machine" "vm" {
  name             = "terraform-test"
  resource_pool_id = "${data.vsphere_compute_cluster.cluster.resource_pool_id}"
  datastore_id     = "${data.vsphere_datastore.datastore.id}"

  num_cpus = 2
  memory   = 1024
  guest_id = "other3xLinux64Guest"

  network_interface {
    network_id = "${data.vsphere_network.network.id}"
  }

  disk {
    label = "disk0"
    size  = 20
  }
}
```

Stolen from the [documentation](https://registry.terraform.io/providers/hashicorp/vsphere/latest/docs/resources/virtual_machine).

----

# Create the environment

```shell
terraform plan
terraform apply
terraform destroy
```

---

# Ansible

Configuring machines with [Red Hat Ansible](https://www.ansible.com/).

Ansible is easy to read/write and there are many [modules](https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html) and [roles](https://galaxy.ansible.com/search?keywords=&order_by=-relevance&deprecated=false&type=role&page=1) that can be used.  
----

# Configuring

```yaml
- name: install nginx
  package:
    name: nginx
    state: present

- name: start and enable nginx
  service:
    name: nginx
    state: started
    enabled: yes
```
Stolen from [GitHub](https://github.com/robertdebock/ansible-role-nginx/blob/master/tasks/main.yml).

----

[Ansible Tower](https://www.ansible.com/products/tower) or [AWX](https://github.com/ansible/awx) offers an API and can execute playbooks.
---

# Vault

Authenticating to resources with [HashiCorp Vault](https://www.vaultproject.io/).

Vault allows users or applications to use short-lived credentials, generated by Vault, to authenticate to other resources, such as [databases](https://www.vaultproject.io/docs/secrets/databases), cloud-providers, [active directory](https://www.vaultproject.io/docs/secrets/ad) and [others](https://www.vaultproject.io/docs/secrets). [Certificate management](https://www.vaultproject.io/docs/secrets/pki) can be automated using Vault too.

----

# Secret

A simple key-value store

```shell
vault kv put secret/hello foo=world
vault kv get secret/hello
vault kv delete secret/hello
```

----

# Dynamic secret 1/4

An on-the-spot-generated credential.

```shell
vault secrets enable -path=aws aws
vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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 \
    region=us-east-1
```

----

# Dynamic secret 2/4

```shell
vault write aws/roles/my-role \
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

```shell
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

---

# Conclusion

1. To work "cloud native", automation is required.
2. All tools exist and are mature.
3. Only buying a tool is not sufficient.
4. Ansible, Terraform & Vault combine well.
