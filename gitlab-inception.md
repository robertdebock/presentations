---
title: GitLab inception

---

# GitLab inception

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/gitlab-inception/"/>

---

# About me

- Father of [3 children](https://raw.githubusercontent.com/robertdebock/presentations/master/images/3.jpg).
- Husband of [1 wife](https://mylucie.com).
- Love to [tinker](https://robertdebock.nl/).
- Specialized in infrastructure.

---

# Tonight

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-inception.png"/>

----

# How?

- Ask questions, get swag.
- https://gitlab.com/robertdebock/gitlab-demo/
- Pseudo-code in presentation.
----

# Terraform

Used to:

1. Spin up 1 GitLab instance.
2. Spin up 1+ GitLab runner instance.
3. Sets A-record to the instances.
4. Drop inventory for Ansible to pickup.

----

# Demo Terraform

https://gitlab.com/robertdebock/gitlab-demo/-/pipelines

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
  count         = 1
  ami           = data.aws_ami.default.id
  instance_type = "t2.medium"
}
```

----

# A-records

```hcl
data "aws_route53_zone" "default" {
  name         = "aws.adfinis.cloud"
  private_zone = false
}

resource "aws_route53_record" "gitlab" {
  zone_id = data.aws_route53_zone.default.id
  name    = "gitlab.aws.adfinis.cloud"
  type    = "A"
  ttl     = 300
  records = [aws_instance.gitlab.public_ip]
}
```

----

# Inventory

```hcl
resource "local_file" "gitlab" {
  content              = templatefile("templates/gitlab.tpl", { hosts = [aws_route53_record.gitlab.fqdn] })
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

# Demo Ansible

https://gitlab.com/robertdebock/gitlab-demo/-/pipelines

----

# Install GitLab

```yaml
- name: provision gitlab
  hosts: gitlab
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.users
    - role: robertdebock.cron
    - role: robertdebock.gitlab
```

----

# Ansible role details

- [Users](https://github.com/robertdebock/ansible-role-users)
- [Cron](https://github.com/robertdebock/ansible-role-cron)
- [GitLab](https://github.com/robertdebock/ansible-role-gitlab)

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
  hosts: runners
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.gitlab_runner
```

----

# Ansible role details

- [GitLab runner](https://github.com/robertdebock/ansible-role-gitlab_runner)

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
- Do not use Ansible at all, just Terraform.
- Store the runner token securly.
- CI should cache inventory files.

---

# Bonus

Shared state in GitLab.

[<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/gitlab-state.png"/>](https://gitlab.com/robertdebock/gitlab-demo/-/terraform).

---

# Conclusion

- 3 tools, 3 qualities.
- [Everything](https://github.com/robertdebock/presentations) is code.
- Physical meetups = drinks!
