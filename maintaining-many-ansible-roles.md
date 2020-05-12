---
title: Maintaining many Ansible roles

---

# Maintaining many Ansible roles

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/maintaining-many-ansible-roles/"/>

---

# Immortalize thyself

- See an error, make a [pull request](https://github.com/robertdebock/presentations).
- I'm collecting stars on GitHub.

----

# Many roles?

Above 10 can already be a pain to maintain.

I have well over 100 roles now.

---

# Scaling issues

1. What is the [status of my roles](https://robertdebock.nl/)?
2. Are there [contributions that need attention](https://robertdebock.nl/contributions.html)?
3. Many files can be [generated](https://github.com/robertdebock/ansible-generator).
4. You can `[probed](https://github.com/robertdebock/ansible-probe)` to generate CI-files

---

# Roles become data

Take a step back after writing a few roles.

What files are static, what unique and what can be generated?

----

# Static

Files that every repository has, and does not need any changes.

- `[FUNDING.yml](https://github.com/robertdebock/ansible-generator/blob/master/files/FUNDING.yml)`
- `[bug_report.md](https://github.com/robertdebock/ansible-generator/blob/master/files/bug_report.md)`
- `[feature_request.md](https://github.com/robertdebock/ansible-generator/blob/master/files/feature_request.md)`
- `[gitignore](https://github.com/robertdebock/ansible-generator/blob/master/files/gitignore)`
- `[yamllint](https://github.com/robertdebock/ansible-generator/blob/master/files/yamllint)`

# Unique per role

- `[meta/main.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/meta/main.yml)`
- `[tasks/*](https://github.com/robertdebock/ansible-role-bootstrap/tree/master/tasks)`
- `[defaults/main.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/defaults/main.yml)`
- `[vars/main.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/vars/main.yml)`
- `files/*` and `templates/*`

----

# Generated

Using [ansible-probe](https://github.com/robertdebock/ansible-probe) all static and generated files can be updated.

----

# Generated documents

- `[CONTRIBUTING.md.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/CONTRIBUTING.md.j2)`
- `[LICENSE-2.0.txt.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/LICENSE-2.0.txt.j2)`
- `[README.md.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/README.md.j2)`
- `[SECURITY.md.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/SECURITY.md.j2)`

----

# Generated tests

- `[ansible-lint.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/ansible-lint.j2)`
- `[molecule.yml.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/molecule.yml.j2)`
- `[settings.yml.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/settings.yml.j2)`
- `[tox.ini.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/tox.ini.j2)`

----

# Generated CI

- `[galaxy.yml.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/galaxy.yml.j2)`
- `[molecule-action.yml.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/molecule-action.yml.j2)`
- `[travis.yml.j2](https://github.com/robertdebock/ansible-generator/blob/master/templates/travis.yml.j2)`

---

# Ideas

Save them [somewhere](https://trello.com/b/K9EUrZL2/refactoring), treat them like user stories.

---

# Dependencies

As soon as you see code repeated, step back, consider making a role.

----

```
digraph PhiloDilemma {
  label = "Robert de Bock" ;
  overlap=false
  {
    bootstrap [fillcolor=green style=filled]
    "core_dependencies" [fillcolor=yellow style=filled]
    dns [fillcolor=orange style=filled penwidth=3]
  }
  dns -> "core_dependencies"
  "core_dependencies" -> bootstrap
}
```

----

![dependency image of ansible role dns](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/dns.png)

For example:

