---
title: Terraform and Ansible

---

# Terraform and Ansible

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/terraform-and-ansible/"/>

---

# Terraform

Can spin up "resources" anywhere.

- Instances
- Loadbalancers
- DNS records
- Volumes

----

# Terraform

Can't really configure instances.

([cloud-init](https://cloudinit.readthedocs.io/en/latest/) can be used.)

----

# Ansible

Does not have state, but has [great roles](https://robertdebock.nl/)!

---

# Combining

Terraform and Ansible can be combined using a couple of patterns.

----

# Terraform inventory

Terraform can write an Ansible inventory.

```
# Now save the droplets into an Ansible inventory.
resource "local_file" "inventory" {
  filename = "../ansible/inventory/hosts"
  file_permission = "0644"
  content = <<-EOT
    %{ for ip in digitalocean_droplet.mail.*.ipv4_address ~}${ip}%{ endfor }
  EOT
}
```

----

# Ansible reads Terraform output

Ansible can run Terraform.

Snippets stolen from [ansible-playbook-rancher](https://github.com/robertdebock/ansible-playbook-rancher/blob/master/playbook.yml).

----

# Ansible runs Terraform

```yaml
- name: create machines
  hosts: localhost
  gather_facts: no

  tasks:
    - name: apply terraform code
      terraform:
        project_path: ./terraform
        state: present
      register: terraform
```

----

# Ansible adds hosts

```yaml
    - name: add terraform hosts to inventory
      add_host:
        name: "{{ item }}"
        groups:
          - rancher
      loop: "{{ terraform.outputs.name.value }}"
```
