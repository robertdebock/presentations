---
title: Perfect Ansible

---

<!-- .slide: data-background="https://raw.githubusercontent.com/robertdebock/presentations/master/images/creation.jpg" -->
# Perfect Ansible

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/perfect-ansible/"/>

---

# Princicpals

1. Simpler is better
2. Focus on the user

---

# Simpler is better

----

1. Does the code fit in 1 screen?
2. Could you explain it to your parents?
3. Would your parents understand?
4. Do you feel an urge to refactor it?
5. Was `shell` or `command` used often?
6. Is there repetition of same `when`?
7. Is ansible-lint happy?

----

# Question: More readable?

```
- name: ensure package screen is installed
  package:
    name: screen
```

```
- yum: name=screen state=present
```

----

# Style

1. Name every task.
2. Think about the name of the task.
3. Use YAML.
4. Omit default values.
5. Use `package` over [specific package manger](http://docs.ansible.com/ansible/latest/list_of_packaging_modules.html).
6. Use `service` over [specific service manger](http://docs.ansible.com/ansible/latest/list_of_system_modules.html).

----

# Question: good enough?

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

Note: It's not always going to install, yum is not only for RHEL, apt is also for debian, when is inconsitent.

---

# Focus on the user

---

