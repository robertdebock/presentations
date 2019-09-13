---
title: Perfect Ansible

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Perfect Ansible

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/perfect-ansible/"/>

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Princicpals

1. Simpler is better
2. Focus on the user

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Simpler is better

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Q: More readable?

```
- name: install screen
  package:
    name: screen
    state: present
```

```
- yum: name="{{ item }}" state=present update_cache=no
  with_items:
    - screen
```

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Reflection

1. Does the code fit in 1 screen?
2. Could you explain it to your partner?
3. Would your partner understand?
4. Do you feel an urge to refactor it?
5. Was `shell` or `command` used often?
6. Is there repetition of same `when`?
7. Is ansible-lint happy?
8. Do you know why you did not use defaults?

Note: 5. Are there modules, are you describing state?
----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Style

1. Name every task carefully.
2. Use YAML.
3. Omit default values.
4. Use `package` over [specific package manger](http://docs.ansible.com/ansible/latest/list_of_packaging_modules.html).
5. Use `service` over [specific service manger](http://docs.ansible.com/ansible/latest/list_of_system_modules.html).

Note: 1. Easier debugging, 3. `file: backup: no`.
----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Q: good enough?

```
- name: Install screen on Red Hat
  yum:
    name: screen
  when:
    - ansible_distribution == "Red Hat Enterprise Linux"

- name: Install screen on Debian
  apt:
    name: screen
  when: ansible_pkg_mgr == "apt"
```

Note: `name:` it's not always going to install, `yum` is not only for RHEL, `apt` is also for debian, when is inconsitent.

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# I love this

```
- name: install apache httpd
  package:
    name: "{{ apache_httpd_package }}"
    state: present
```

```
_apache_httpd_package:
  Debian: apache2
  RedHat: httpd

apache_httpd_package: "{{ _apache_httpd_package[ansible_os_family] }}"
```

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Complexity levels

- `defaults/main.yml` - Super simple
- `tasks/main.yml` - Simple
- `vars/main.yml` - Not so simple

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Good examples of bad

[Difficult interface](https://github.com/geerlingguy/ansible-role-nginx)
[Linting issues](https://galaxy.ansible.com/dev-sec/os-hardening)
[More linting issues](https://galaxy.ansible.com/juniper/junos)
[`state: latest`](https://github.com/DavidWittman/ansible-redis/blob/59f05097f828fff9c613f31072031e798c8b10f4/tasks/dependencies.yml#L29)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Focus on the user

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Variables

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 1

tasks/main.yml:
```
- name: ensure java is installed
  package:
    name: java-1.8.0-openjdk
```

What can we make variable?

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 2

defaults/main.yml
```
java_version: 1.8.0
```

tasks/main.yml:
```
- name: ensure java is installed
  package:
    name: java-{{ java_version }}-openjdk
```

What more can we make variable?

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 3

vars/main.yml:
```
java_jre_name: openjdk
java_jdk_name: openjdk-devel
```

(continued)

----

defaults/main.yml:
```
java_version: 1.8.0
java_type: jre
```

(continued)

----
tasks/main.yml:
```
- name: ensure java jre is installed
  package:
    name: java-{{ java_version }}-{{ java_jre_name }}
  when:
    - java_type == jre

- name: ensure java jdk is installed
  package:
    name: java-{{ java_version }}-{{ java_jdk_name }}
  when:
    - java_type == jdk
```

---

# Problems

### Solve problems at the right place.

```
- name: ensure my-software is installed
  package:
    name: my-software

- name: set correct permission on a file from my-software
  file:
    name: /opt/my-software/some-binary
    owner: root
    group: root
    mode: 0750
```

---

# Bit-rot

Wikipedia: (so scientifically true)

```txt
A deterioration of software performance over time or its
diminishing responsiveness because of the changing
environment in which it resides.
```

----

# Software

<iframe width="600" height="400" frameborder="0" scrolling="no" src="//plot.ly/~robertdebock/1.embed"></iframe>

Graph-inspiration: [Sam Doran](https://github.com/samdoran) ([Youtube](https://www.youtube.com/watch?v=7gEKmsuJr7s))

----

# Prevent bit-rot

- Make small "units" (Ansible roles)
- Build often (Molecule)
- Integrate (DigitalOcean)
- Periodic rebuilds (Travis)

---
  
<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Conclusion

- Simplicity wins
- Compatibility wins
- Easier variables win
