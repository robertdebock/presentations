---
title: Ansible Roles

---

# Ansible Role

In 15 minutes

<img height="28%" width="28%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/content/images/dave.png"/>

---

# Goals

- You focus on "hard" stuff.
- Consumers simply pickup your role.

---

# An idea please

[Inspiration](https://galaxy.ansible.com/)/suggestions:
- ansible-role-httpd
- ansible-role-browsers
- ansible-role-devtools
- ansible-role-opstools

---

# ansible-role-httpd

----

# Docker *

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-MYROLE 
```

- * = No reboot or systemctl, but fast.

----

# Vagrant

```
molecule init role \
  --driver-name vagrant \
  --verifier-name goss \
  --role-name ansible-role-MYROLE 
```

```
vi ansible-role-MYROLE/molecule/default/molecule.yml
```

Note:
```
    box: centos/7
```

----

# Write ansible tests

```
vi ansible-role-MYROLE/molecule/default/playbook.yml
```

Note:
```
---
- name: Converge
  hosts: all
  become: true

  roles:
    - role: ansible-role-MYROLE

  tasks:
    - name: test connection
      wait_for:
        port: 80
```

----

# Write goss tests

```
vi ansible-role-MYROLE/molecule/default/tests/test_default.yml
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
vi ansible-role-MYROLE/tasks/main.yml
```

Note:
```
---
# tasks file for ansible-role-MYROLE
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

