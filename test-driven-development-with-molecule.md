---
title: Test driven development with Molecule (TDD)

---

# Test driven development with Molecule (TDD)

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/test-driven-development-with-molecule/"/>

---

# Who am I?

- Robert de Bock, working for [Adfinis](https://adfinis.com/en/).
- Father of 3 children, husband of [Lucie](https://mylucie.com/).
- Working with infrastructure engineering since +- 2000.
- Working with Ansible since +- 2017

---

# TDD, what?

[Wikipedia](https://en.wikipedia.org/wiki/Test-driven_development): Test-driven development (TDD) is a software development process relying on software requirements being converted to test cases before software is fully developed, and tracking all software development by repeatedly testing the software against all test cases. This is as opposed to software being developed first and test cases created later.

Fits nicely into a peer programming or [mob programming](https://robertdebock.nl/presentations/mob-programming/#/).

---

# How to test Ansible code?

----

# Static tests

- [yamllint](http://www.yamllint.com/).
- [ansible-lint](https://ansible-lint.readthedocs.io/).

---

# Dynamic tests

- [molecule](https://molecule.readthedocs.io/).

---

# Molecule

Molecule is a tool to test Ansible code. Typically roles. There are a couple of (important) stages:

- `prepare` - Prepare a system.
- `converge` - Run the code in the repository.
- `verify` - Run steps to test the result.

----

# Example stages

- `prepare` - Install `python`, `sudo` and `ca_certificates`.
- `converge` - Install Zabbix repositories.
- `verify` - Ensure that a Zabbix package can be installed.

----

# Basically, you create a chain:

1. Install requirements
2. Run the current role
3. Ensure that either a) follow-up tasks will work or b) the system functions as expected.

----

# The trains


bootstrap

```text
+--- prepare ---+    +--- converge ---+    +--- verify ---+
| (empty)       | -> | python         | -> | zabbix-repo  |
+---------------+    | sudo           |    +--------------+
                     +----------------+
```

zabbix-repo

```text
+--- prepare ---+    +--- converge ---+    +--- verify ---+
| python        | -> | zabbix-repo    | -> | zabbix-agent |
| sudo          |    +----------------+    +--------------+
+---------------+
```

zabbix-agent

```text
+--- prepare ---+    +--- converge ---+    +--- verify -----+
| python        | -> | zabbix-agent   | -> | port 10050/tcp |
| sudo          |    +----------------+    +----------------+
| zabbix-repo   |
+---------------+
```

----

# Molecule prepare

```yaml
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
```

----

# Molecule converge

```yaml
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: ansible-role-zabbix_repository
```

What actually happens here is run the Ansible role using `tasks/main.yml`.

----

# Molecule verify

```yaml
- name: Verify
  hosts: all
  become: yes
  gather_facts: no

  tasks:
    - name: install a zabbix package
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop:
        - zabbix-get
        - zabbix-server-mysql
        - zabbix-frontend-php
        - zabbix-apache-conf
        - zabbix-agent
        - zabbix-proxy
```

---

# CI/CD

Wouldn't it be great to automatically run this code an a `push`?

In test driven development, this step is curcial and comes quite early.

----

# GitHub

```yaml
name: Ansible Molecule

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-20.04
    strategy:
      fail-fast: false
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: molecule
        uses: robertdebock/molecule-action@2.6.16
```

----

# GitLab

```yaml
image: robertdebock/github-action-molecule:3.0.6

services:
  - docker:dind

variables:
  DOCKER_HOST: "tcp://docker:2375"
  PY_COLORS: 1

molecule:
  script:
    - molecule test
  rules:
    - if: $CI_COMMIT_REF_NAME == "master"
```

---

Sources:

- [zabbix-repository role](https://github.com/robertdebock/ansible-role-zabbix_repository).
