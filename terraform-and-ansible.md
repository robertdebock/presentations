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

Can't really configurate of instances.

----

# Terraform

Can write an inventory file that Ansible can use.

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

---

# Ansible

Great fo managing the configuration of instances.
