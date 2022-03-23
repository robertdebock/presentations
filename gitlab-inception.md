---
title: GitLab inception

---

# GitLab inception

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/gitlab-inception/"/>

---

# Welcome! (back?)

My first physical Meetup since 2019.

Right on time for new t-shirts.

---

# About me

- Father of [3 children](https://raw.githubusercontent.com/robertdebock/presentations/master/images/3.jpg).
- Husband on [1 wife](https://mylucie.com).
- Love to [tinker](https://robertdebock.nl/).
- Specialized in infrastructure.

---

# Tonight

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-inception.png"/>

----

# How?

- Ask questions, get swag.
- Code: https://gitlab.com/robertdebock/gitlab-demo/
- Pseudo-code in presentation.
----

# Terraform

Used to:

1. Spin up 1 GitLab instance.
2. Spin up 1+ GitLab runner instance.
3. Set A-records to the instance.
4. Drop inventory for Ansible to pickup.

----

# GitLab instance

```hcl
resource "aws_instance" "gitlab" {
  ami               = data.aws_ami.default.id
  instance_type     = "c5.2xlarge"
  root_block_device {
    volume_size = 8
    volume_type = "gp3"
  }
}
```

----

# GitLab Runner instance

```hcl
resource "aws_instance" "runner" {
  count           = 1
  ami             = data.aws_ami.default.id
  instance_type   = "t2.medium"
}
```

----

# A-records

```hcl
data "cloudflare_zones" "default" {
  filter {
    name = "meinit.nl"
  }
}

resource "cloudflare_record" "gitlab" {
  zone_id = data.cloudflare_zones.default.zones[0].id
  name    = "gitlab"
  value   = aws_instance.gitlab.public_ip
  type    = "A"
  ttl     = 300
}
```

----

# Inventory

```hcl
resource "local_file" "gitlab" {
  content              = templatefile("templates/gitlab.tpl", { hosts = [cloudflare_record.gitlab.hostname] })
  filename             = "${path.module}/../ansible/inventory/gitlab"
  directory_permission = "0755"
  file_permission      = "0644"
}
```

---

# Ansible

Used to:

1. Install GitLab.
2. Fetch "initial_root_password".
3. Install GitLab Runner.


----

# Install GitLab

```yaml
- name: provision gitlab
  hosts: gitlab.meinit.nl
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.users
    - role: robertdebock.cron
    - role: robertdebock.gitlab
```

----

# Fetch password

```yaml
post_tasks:
  - name: get initial password
    ansible.builtin.fetch:
      src: /etc/gitlab/initial_root_password
      dest: ../files/initial_root_password
```

----

# Install GitLab Runner

```yaml
- name: provision runner
  hosts: runner-*.meinit.nl
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.gitlab_runner
```

---

# GitLab

Used to:

1. Run Terraform
2. Run Ansible

----

# Run Terraform

```yaml
apply:
  stage: deploy
  script:
    - gitlab-terraform apply
  artifacts:
    name: ansible
    paths:
      - ansible/inventory/all
      - ansible/inventory/gitlab
      - ansible/inventory/runners
      - files/id_rsa
```

----

# Run Ansible

```yaml
play:
  stage: deploy
  script:
    - ansible-galaxy install -r roles/requirements.yml
    - ./playbook.yml
  artifacts:
    name: gitlab
    paths:
      - files/initial_root_password
```

---

# Relax

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-inception.png"/>

---

# Improvements

- Store sensitive information in [Vault](https://www.vaultproject.io).

---

# Bonus

Shared state in GitLab.

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-state.png"/>

---

# Conclusion

- 3 tools, 3 qualities.
- Everything is code.
- Physical meetups = drinks!
