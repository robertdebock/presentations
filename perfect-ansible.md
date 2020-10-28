---
title: Perfect Ansible

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Perfect Ansible

- Follow along: http://robertdebock.nl/
- https://github.com/robertdebock/presentations

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/perfect-ansible/"/>

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->

# Robert

Introduce yourself!

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# The norm

[Ansible Docker role by Jeff Geerling](https://galaxy.ansible.com/geerlingguy/docker)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Principles

1. Simpler is better
2. Focus on the user

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Simpler is better

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Q: More readable?

```yaml
- name: install screen
  package:
    name: screen
    state: present
```

```yaml
- yum: name="{{ item }}" state=present update_cache=no
  with_items:
    - screen
```

----


<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Q: Ease of use?

```yaml
httpd_configuration:
  port: 80
  ssl_enabled: yes
```

```yaml
httpd_port: 80
httpd_ssl_enabled: yes
```

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Reflection

1. Does the code fit on a screen?
2. Could you explain it to your partner?
3. Do you feel an urge to [refactor it](https://www.google.com/search?q=unfinished+painting)?
4. Was `shell` or `command` [used often](https://robertdebock.nl/2019/09/26/how-many-modules-do-you-need.html)?
5. Is there repetition of [same `when`](https://github.com/dj-wasabi/ansible-zabbix-agent/blob/master/tasks/main.yml#L92)? ([Use blocks](https://github.com/robertdebock/ansible-role-postgres/blob/master/tasks/main.yml#L48).)
6. Do you know why you did not use [defaults](https://docs.ansible.com/ansible/latest/modules/service_module.html)?
7. Did you need `ignore_errors` or `failed_when: false`?

Note: 5. Are there modules, are you describing state?
----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Ansible lint

## [is always right](https://docs.ansible.com/ansible-lint/rules/default_rules.html)

[some](https://github.com/ansible/ansible-lint/blob/master/lib/ansiblelint/rules/CommandHasChangesCheckRule.py) are [difficult](https://github.com/ansible/ansible-lint/blob/master/lib/ansiblelint/rules/LineTooLongRule.py)

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Style

1. Name every task carefully.
2. Use YAML, and yamllint.
3. Omit default values.
4. Use `package` over [specific package manager](http://docs.ansible.com/ansible/latest/list_of_packaging_modules.html).
5. Use `service` over [specific service manager](http://docs.ansible.com/ansible/latest/list_of_system_modules.html).

Note: 1. Easier debugging, 3. `file: backup: no`.
----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Q: Good enough?

```yaml
- name: Install httpd on Red Hat
  yum:
    name: httpd
  when:
    - ansible_distribution == "Red Hat Enterprise Linux"

- name: Install apache2 on Debian
  apt:
    name: apache2
  when: ansible_pkg_mgr == "apt"
```

Note: `name:` it's not always going to install, `yum` is not only for RHEL, `apt` is also for debian, when is inconsitent.

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# I love this

```yaml
- name: install apache httpd
  package:
    name: "{{ apache_httpd_package }}"
    state: present
```

```yaml
_apache_httpd_package:
  Debian: apache2
  RedHat: httpd

apache_httpd_package: "{{ _apache_httpd_package[ansible_os_family] }}"
```

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Complexity levels

- `defaults/main.yml` - [Super simple](https://www.volkskrant.nl/cultuur-media/reconstructie-het-bizarre-verhaal-van-who-s-afraid-of-red-yellow-and-blue-iii~b2bb8df2/)
- `tasks/main.yml` - [Simple](https://en.wikipedia.org/wiki/De_Stijl)
- `vars/main.yml` - [Not so simple](https://www.jackson-pollock.org/)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Good bad examples

- [Incorrect role name](https://galaxy.ansible.com/robertdebock/dns)
- [Difficult interface](https://github.com/geerlingguy/ansible-role-nginx)
- [Linting issues](https://galaxy.ansible.com/ansiblebit/oracle-java)
- [More linting issues](https://galaxy.ansible.com/juniper/junos)
- [`state: latest`](https://github.com/DavidWittman/ansible-redis/blob/59f05097f828fff9c613f31072031e798c8b10f4/tasks/dependencies.yml#L29)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 1

tasks/main.yml:
```yaml
- name: ensure java is installed
  package:
    name: java-1.8.0-openjdk
```

What can we make variable?

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 2

defaults/main.yml
```yaml
java_version: 1.8.0
```

tasks/main.yml:
```yaml
- name: ensure java is installed
  package:
    name: java-{{ java_version }}-openjdk
```

What more can we make variable?

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 3

vars/main.yml:
```yaml
java_jre_name: openjdk
java_jdk_name: openjdk-devel
```

(continued)

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->

defaults/main.yml:
```yaml
java_version: 1.8.0
java_type: jre
```

(continued)

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->

tasks/main.yml:
```yaml
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

----


<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Attempt 3

```yaml
_java_package:
  jre: java-{{ java_version }}-openjdk
  jdk: java-{{ java_version }}-openjdk-devel

java_package: "{{ _java_package[java_type] }}
```

```yaml
- name: install java
  package:
    name: "{{ java_package }}"
    state: present
```

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Duplicate data

```yaml
_my_role_requirements:
  RedHat:
    - some_package
  Amazon:
    - some_package
  Suse:
    - some_package
  Debian:
    - some_other_package
```

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Less duplicate

```yaml
_my_role_requirements:
  RedHat: &rhel
    - some_package
  Amazon: *rhel7
  Suse: *rhel7
  Debian:
    - some_other_package
```

These are called [YAML anchors and aliases](https://yaml.org/spec/1.2/spec.html#id2765878).

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Assert

```yaml
- name: test some variable
  assert:
    that:
      - variable is defined
      - variable_list is iterable
      - variable_string in ["one", "two"
      - variable_boolean | type_debug == "bool"
      - variable_integer | int
```

Should [become standard soon](https://github.com/ansible/proposals/issues/39)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Verify

```
- name: see if the sample index.html returns 200
  uri:
    url: "https://127.0.0.1:8443/"
    validate_certs: no
```

It's just a [part of Molecule](https://molecule.readthedocs.io/en/latest/configuration.html#verifier).

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Problems

### Solve problems at the right place.

```yaml
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

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Bit-rot

Wikipedia: (so scientifically true)

```txt
A deterioration of software performance over time or its
diminishing responsiveness because of the changing
environment in which it resides.
```

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Software

<iframe width="600" height="400" frameborder="0" scrolling="no" src="//plot.ly/~robertdebock/1.embed"></iframe>

Graph-inspiration: [Sam Doran](https://github.com/samdoran)

----

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Prevent bit-rot

- Make small "units" (Ansible roles)
- Build often (Molecule)
- Integrate (DigitalOcean)
- Periodic retests (Travis)

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Conclusion

- Simplicity wins
- Compatibility wins
- Easier variables win

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Thanks!

GitHub: [robertdebock](https://github.com/robertdebock)
