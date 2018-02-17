---
title: Ansible testing

---

# Ansible testing

Follow along: http://robertdebock.nl/

<img height="30%" width="30%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/content/images/robertdebock-nl.jpg"/>

---

# Context

- About me
- About work

---

# Ansible

Another new technology?

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/content/images/popularity.png"/>

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

Warning: I love [ASCII art](https://groups.google.com/forum/#!forum/alt.ascii-art)

```text
+---- GitHub ----+    +-- Travis --+    +----- Playbook ----+
| - .travis.yml  |    | - playbook |    |  - hosts: all     |
| - playbook.yml | -> | - tests    |    |    become: yes    |
| - molecule.yml |    +------------+    |                   |
+----------------+          |           |    roles:         |
                            V           |      - httpd      |
                      +-- Galaxy --+    |                   |
                      | - roles    | <- |    tasks:         |
                      +------------+    |      - name: test |
                                        |        ping:      |
                                        +-------------------+
```

---

# Lets Go!

```
molecule init role \
  --driver-name docker \
  --verifier-anme goss \
  ansible-role-MYROLE
```
----

# Write tests

```
```

----

# Write the role

```
```

----

# Test it

```
```

---

# Limitations

- This just tests the role
- De developer determines the tests