---
title: Ansible testing

---

# Ansible testing

Follow along: http://robertdebock.nl/presentations/
Nerdier: https://github.com/robertdebock/presentations/

---

# Context

- About me
- About work

---

# Ansible

Another new technology?

Note: kickstart, scripts, clusterssh, cfengine, puppet.

---

# Playbooks vs Roles

- Roles are sharable
- Playbooks refer to roles

----

# Ansible role

```
- name: install software
  package:
    name: software

- name: start software
  service
    name: software
    state: started
    enabled: "{{ software_enabled }}"
```

----

# Ansible playbook

```
- hosts: webservers
  become: yes

  roles:
    - name: ansible-role-software
      software_enabled: yes

  tasks:
    - name: do something specific
      copy:
        src: /some/file.txt
        dest: /opt/software/file.txt
```

---

# Roles, where?

https://galaxy.ansible.com/

----

# Galaxy criteria

- Downloads
- Stars
- Watches
- Build status*
- Last commit
- OS compatibility

---

# Build status

Developer determines quality

Note: tests are optional, but really help.

----

# Not really a build

The `artifact` is published to Galaxy

----

# Unit tests

```text
+---- GitHub ----+    +-- Travis --+
| - .travis.yml  |    | - playbook |
| - playbook.yml | -> | - tests    |
| - molecule.yml |    +------------+
+----------------+          |
                            V
                      +-- Galaxy --+
                      | - roles    |
                      +------------+
```

---

# Integration tests

Write a play that incorporates roles
