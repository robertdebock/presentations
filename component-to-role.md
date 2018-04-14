---
title: Component to Ansible role

---

# Components

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/component-to-role/"/>

---

# How to:
- Deploy your component
- Manage your component

---

# History

Maybe you have these types of code:
1. Puppet
2. BigFix
3. Perl, Bash, Bourne shell, etc

----

# Future

## Ansible
- Roles for deployment
- Playbooks for 2nd day operations

---

# How your role will integrate

----

# Integrate 1/4

Your role will be included from a playbook that includes many roles. For example:

```
- name: stack
  hosts: all
  become: yes
```

- There can be multiple stack.
- A `limit` can be used.
- This playbook uses `become`.

----

# Integrate 2/4

```
  pre_tasks:
  - name: install other ansible requirements
    package:
      name: "{{ item }}"
      - libselinux-python
      - libsemanage-python
      - python-simplejson
      - yum-plugin-ovl
      - lvm2
      - tar
      - unzip
      - gzip
```

- Your role just does your component

----

# Integrate 3/4

```
  tasks:
    - name: run backup role
      include_role:
        name: backup
      vars:
        backup_server: backupserver1.example.com

    - name: run monitoring role
      include_role:
        name: monitoring
      vars:
        monitoring_server: monitoringserver1.example.com
```

----

# Integrate 4/4

requirements.yml
```
- src: https://github.com/username/backup.git
  version: 1.3.0
  name: backup

- src: https://github.com/username/monitoring.git
  version: 2.0.1
  name: monitoring
```

- With a [pull](https://help.github.com/articles/creating-a-pull-request/) (GitHub) or [merge](https://docs.gitlab.com/ee/user/project/merge_requests/) (GitLab) request you can add your component.

----

# Graphical

----

Before

```
+=== Repository "stack" ===================================+
|                        +--- Requirements --------------+ |
|                        | - src: https://../backup.git  | |
| +--- Stack -------+    |   version: 1.3.0              | |
| | - role: backup  | <- |   name: backup                | |
| | - role: monitor |    | - src: https://../monitor.git | |
| +-----------------+    |   version: 2.0.1              | |
|                        |   name: monitoring            | |
|                        +-------------------------------+ |
+==========================================================+

+=== Repository "backup" ==+   +=== Repository "monitor" ==+
| - defaults/main.yml      |   | - defaults/main.yml       |
| - files/                 |   | - files/                  |
| - handlers/main.yml      |   | - handlers/main.yml       |
| - tasks/main.yml         |   | - tasks/main.yml          |
| - templates/             |   | - templates/              |
| - vars/main.yml          |   | - vars/main.yml           |
+==========================+   +===========================+
```

----

After

```
+=== Repository "stack" ===================================+
|                        +--- Requirements --------------+ |
|                        | - src: https://../backup.git  | |
| +--- Stack -------+    |   version: 1.3.0              | |
| | - role: backup  | <- |   name: backup                | |
| | - role: monitor |    | - src: https://../monitor.git | |
| | - role: yours   |    |   version: 2.0.1              | |
| +-----------------+    |   name: monitoring            | |
|                        | - src: https://../yours.git   | |
|                        |   version: 1.0.0              | |
|                        |   name: yours                 | |
|                        +-------------------------------+ |
+==========================================================+

+=== Repository "backup" ==+   +=== Repository "monitor" ==+   +=== Repository "yours" ===+
| - defaults/main.yml      |   | - defaults/main.yml       |   | - defaults/main.yml      |
| - files/                 |   | - files/                  |   | - files/                 |
| - handlers/main.yml      |   | - handlers/main.yml       |   | - handlers/main.yml      |
| - tasks/main.yml         |   | - tasks/main.yml          |   | - tasks/main.yml         |
| - templates/             |   | - templates/              |   | - templates/             |
| - vars/main.yml          |   | - vars/main.yml           |   | - vars/main.yml          |
+==========================+   +===========================+   +==========================+
```

----

So;
1. Make your role.
2. Add the referen to requirements.yml.
3. Add your role to the stack.

---

# Your component

----

Write your role to be version specific:

```
- name: install my-software
  package:
    name: my-software-1.2.3
```

(Now it can be used for deployment and 2nd-day operations (update)).

----

Once (unit) tested, `release` (GitHub) or `tag` (GitLab) your repository.

----

Create a [pull](https://help.github.com/articles/creating-a-pull-request/) (GitHub) or [merge](https://docs.gitlab.com/ee/user/project/merge_requests/) (GitLab) request for the playbook that defines the release.
- stack.yml
- requirements.yml
