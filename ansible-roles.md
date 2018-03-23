---
title: Ansible Roles

---

# Ansible Roles

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ansible-roles/"/>

---

# Ansible?

- Server provisioning tool
- Ansible Roles are sharable

---

# Testing? Why?

- High quality roles are great
- Easier to write roles
- Easier to consume roles

---

# An idea please

[Inspiration](https://galaxy.ansible.com/)/suggestions:
- ansible-role-httpd
- ansible-role-browsers
- ansible-role-devtools
- ansible-role-opstools

---

# httpd

----

# Docker

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-httpd
```

- No reboot or systemctl, but fast.

----

# Vagrant

```
molecule init role \
  --driver-name vagrant \
  --verifier-name goss \
  --role-name ansible-role-httpd

vi ansible-role-httpd/molecule/default/molecule.yml
```

Note:
```
    box: centos/7
```

----

# Ansible tests

```
vi ansible-role-httpd/molecule/default/playbook.yml
```

Note:
```
- name: Converge
  hosts: all
  become: true

  roles:
    - role: ansible-role-httpd

  tasks:
    - name: test connection
      wait_for:
        port: 80
```

----

# Goss tests

```
vi ansible-role-httpd/molecule/default/tests/test_default.yml
```

Note:
```
package:
  httpd:
    installed: true
service:
  httpd:
    enabled: true
    running: true
process:
  httpd:
    running: true
port:
  tcp6:80
    listening: true
    ip:
      - '::'
```

----

# Write the role

```
vi ansible-role-httpd/tasks/main.yml
```

Note:
```
# tasks file for ansible-role-httpd
- name: install apache
  package:
    name: httpd

- name: start apache
  service:
    name: httpd
    state: started
    enabled: true
```

---

# browsers

----

# Docker

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-browsers
```

- No reboot or systemctl, but fast.

----

# Vagrant

```
molecule init role \
  --driver-name vagrant \
  --verifier-name goss \
  --role-name ansible-role-browsers

vi ansible-role-browsers/molecule/default/molecule.yml
```

Note:
```
    box: centos/7
```

----

# Ansible tests

```
vi ansible-role-browsers/molecule/default/playbook.yml
```

Note:
```
- name: Converge
  hosts: all
  become: true

  roles:
    - role: ansible-role-browsers

  tasks:
    - name: test browsers
      command: "{{ item.name }} {{ item.argument }}"
      changed_when: no
      with_items:
        - name: firefox
          argument: -version
        - name: chrome
          argument: --version
```

----

# Goss tests

```
vi ansible-role-browsers/molecule/default/tests/test_default.yml
```

Note:
```
package:
  firefox:
    installed: true
package:
  chrome:
    installed: true
```

----

# Write the role

```
vi ansible-role-browsers/tasks/main.yml
```

Note:
```
# tasks file for ansible-role-browsers
- name: install browser
  package:
    name: firefox
  with_items:
    - firefox
    - chrome
```

---

# devtools

----

# Docker

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-devtools
```

- No reboot or systemctl, but fast.

----

# Vagrant

```
molecule init role \
  --driver-name vagrant \
  --verifier-name goss \
  --role-name ansible-role-devtools

vi ansible-role-devtools/molecule/default/molecule.yml
```

Note:
```
    box: centos/7
```

----

# Ansible tests

```
vi ansible-role-devtools/molecule/default/playbook.yml
```

Note:
```
- name: Converge
  hosts: all
  become: true

  roles:
    - role: ansible-role-devtools

  tasks:
    - name: test software
    command: "{{ item.name }} {{ item.argument }}"
    changed_when: no
      with_items:
        - name: vim
          argument: --version
```

----

# Goss tests

```
vi ansible-role-devtools/molecule/default/tests/test_default.yml
```

Note:
```
package:
  vim:
    installed: true
```

----

# Write the role

```
vi ansible-role-devtools/tasks/main.yml
```

Note:
```
# tasks file for ansible-role-devtools
- name: install devtool
  package:
    name: "{{ item }}"
  with_items:
     - vim
```

---

# opstools

----

# Docker

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-opstools
```

- No reboot or systemctl, but fast.

----

# Vagrant

```
molecule init role \
  --driver-name vagrant \
  --verifier-name goss \
  --role-name ansible-role-opstools

vi ansible-role-opstools/molecule/default/molecule.yml
```

Note:
```
    box: centos/7
```

----

# Ansible tests

```
vi ansible-role-opstools/molecule/default/playbook.yml
```

Note:
```
- name: Converge
  hosts: all
  become: true

  roles:
    - role: ansible-role-opstools

  tasks:
    - name: test opstool
      command: "{{ item.name }} {{ item.argument }}"
      changed_when: no
      with_items:
        - name: tcpdump
          argument: --version
        - name: strace
          argument: -V
```

----

# Goss tests

```
vi ansible-role-opstools/molecule/default/tests/test_default.yml
```

Note:
```
package:
  tcpdump:
    installed: true
package:
  strace:
    installed: true
```

----

# Write the role

```
vi ansible-role-opstools/tasks/main.yml
```

Note:
```
# tasks file for ansible-role-opstools
- name: install opstool
  package:
    name: "{{ item }}"
  with_items: "{{ opstools_packages }}"
```

----

# Variables

```
vi ansible-role-opstools/vars/main.yml
```

```
opstools_packages:
  - tcpdump
  - strace
```

---

# Ready? Test!

```
cd ansible-role-MYROLE/
molecule test
```

---

# Publish

Go to [GitHub](https://github.com/), create a repository.
Go to [Travis CI](https://travis-ci.org/), enable builds.
Go to [Galaxy](https://galaxy.ansible.com/) add your role.

---

# World domination

<img height="40%" width="40%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/content/images/evil.png"/>
