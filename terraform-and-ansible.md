---
title: Terraform and Ansible

---

# Terraform and Ansible

Follow along: http://robertdebock.nl/

![QR code to this presentation](https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/terraform-and-ansible/ "Don't worry, you're seeing the presentation already.")

---

# Who am I

- [Robert de Bock](https://robertdebock.nl/).
- Working for [Adfinis](https://adfinis.com/en/).
- [Art-school](https://www.hku.nl/) drop out.
- Loves Linux and open source.

---

# Contribute

Feel free to [correct mistakes](https://github.com/robertdebock/presentations/blob/master/terraform-and-ansible.md).

![Mistakes have been made](https://i.imgur.com/8zerI4V.png "I can not be wrong.")

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

([cloud-init](https://cloudinit.readthedocs.io/en/latest/), or [remote-exec](https://www.terraform.io/docs/provisioners/remote-exec.html) can be used, not configuration management though.)

----

# Ansible

Is simple, used often and has [great roles](https://robertdebock.nl/), but no state.

---

# Combining

Terraform and Ansible can be combined using a couple of patterns.

----

# Terraform local-exec

```
  provisioner "local-exec" {
    command = "ansible-playbook -i '${self.public_ip},' playbook.yml" 
  }
```

Drawback: runs on a single host, no "neighbours", no clusters.

----

# Terraform "local_file"

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

Drawback: Does not run the playbook automatically.

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
        state: "{{ terraform_state | default('present') }}"
      register: terraform
```

----

# Ansible adds hosts

```yaml
    - name: add terraform hosts to inventory
      add_host:
        name: "{{ item }}"
      loop: "{{ terraform.outputs.name.value }}"
```

----

# Terraform provider Ansible

There is a [provider for terraform](https://github.com/nbering/terraform-provider-ansible) that acts as a dynamic inventory.

---

# Anti-patterns

There are a few methods that will cause issues.

1. No combination manual inventory management.
2. Dynamic inventory, too many hosts.

---

# Conclusion

There are ways to combine Terraform and Ansible, each approach has it's *unique characteristics*.

![Multiple forks, one is broken: Just because you are unique, does not mean you are useful.](https://3.bp.blogspot.com/-XX12ahOZHqY/WUg226Hd0UI/AAAAAAAAkjA/wzFWmJv-EL0DknBcCOi6EPPgQBnHe3AoQCLcBGAs/s1600/unique.jpg "Unique is not always good.")
