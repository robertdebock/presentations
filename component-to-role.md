---
title: Component to Ansible role

---

# Component -> Role

### How to:
- Deploy your component
- Manage your component

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/component-to-role/"/>

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

# Deployment 1/4

Your role will be included from a playbook that includes many roles. For example:

```
- name: my-offering
  hosts: all
  become: yes
```

- There can be multiple offerings.
- A `limit` can be used.
- This playbook uses `become`.

----

# Deployment 2/4

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

# Deployment 3/4

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

# Deployment 4/4

requirements.yml
```
- src: https://github.com/username/ansible-role-backup.git
  version: 1.3.0
  name: backup

- src: https://github.com/username/ansible-role-monitoring.git
  version: 2.0.1
  name: monitoring
```

- With a [pull](https://help.github.com/articles/creating-a-pull-request/) or [merge](https://docs.gitlab.com/ee/user/project/merge_requests/) request you can add your component.
