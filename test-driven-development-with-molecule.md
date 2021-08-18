---
title: Test driven development with Molecule (TDD)

---

# Test driven development with Molecule (TDD)

Follow along: https://robertdebock.nl/

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

----

# TDD, why?

- Starting with the tests make the scope clear.
- You'll save time (and discussions).
- You can refine a story by defining the tests first.
- It's easier to contribute.

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

- `prepare` - Install `python` & `sudo`.
- `converge` - Install Zabbix repositories.
- `verify` - Install a Zabbix package.

----

# Basically, you create a chain:

1. Install requirements
2. Run the current role
3. Test the purpose of the role.

----

# The trains

A sequence of dependencies.

----

[bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap)

```text
+--- prepare ---+    +--- converge ---+    +--- verify ---+
| (empty)       | -> | python         | -> | zabbix-repo  |
+---------------+    | sudo           |    +--------------+
                     +----------------+
```

----

[zabbix-repo](https://galaxy.ansible.com/robertdebock/zabbix_repository)

```text
+--- prepare ---+    +--- converge ---+    +--- verify ---+
| python        | -> | zabbix-repo    | -> | zabbix-agent |
| sudo          |    +----------------+    +--------------+
+---------------+
```

----

[zabbix-agent](https://galaxy.ansible.com/robertdebock/zabbix_agent)

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

On [GitHub](https://github.com/robertdebock/ansible-role-zabbix_repository/blob/master/molecule/default/prepare.yml).

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

On [GitHub](https://github.com/robertdebock/ansible-role-zabbix_repository/blob/master/molecule/default/converge.yml).

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
        - zabbix-apache-conf
        - zabbix-agent
```

On [GitHub](https://github.com/robertdebock/ansible-role-zabbix_repository/blob/master/molecule/default/verify.yml).

---

# Input validation

You can also test "user input" (variables).

There are two ways.

----

# The OLD way

Using [`assert`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/assert_module.html).

----

# The OLD way

```yaml
- name: test input variable my_var
  ansible.builtin.assert:
    that:
      - my_var is defined
      - my_var is number
      - my_var > 0
      - my_var <= 1024
    quiet: yes
```

----

# The OLD way

Benefits:

- Works on any version of Ansible.
- Very fine-grained control, like ranges, combinations, etc.

----

# The OLD way

Drawbacks:

- A lot of coding.
- Takes a while to run.
- Lots of output, not pretty.
# Input validation NEW

----

# The NEW way

Using [`meta/argument_specs.yml`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#role-argument-validation).

Only on [Ansible 2.11 and up].

----

# The NEW way

Benefits:

- Native to Ansible. (2.11+)
- Quick and clean to run.
- Prepared for automatic documentation.

----

# The NEW way

Drawbacks:

- Testing input is not possible. (just `choices`.)

----

# The NEW way

```yaml
argument_specs:
  main:
    short_description: "My role."
    description: "My super role."
    author: Robert de Bock
    options:
      my_var:
        type: "int"
        default: 123
        description: "My variable, a number between 0 and 1024."
```

---

# CI/CD

Test code on push/merge.

Focus on CI/CD quite early.

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

On [GitHub](https://github.com/robertdebock/ansible-role-zabbix_repository/blob/master/.github/workflows/molecule.yml).

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

On [GitLab](https://gitlab.com/robertdebock/ansible-role-zabbix_repository/-/blob/master/.gitlab-ci.yml).

---

# Conclusion

- TDD is just another rythm to develop code.
- Molecule (`verify.yml` + CI) make TDD simple.

Sources:

- [zabbix-repository role](https://github.com/robertdebock/ansible-role-zabbix_repository).
- Any [other role](https://robertdebock.nl/) I wrote.
