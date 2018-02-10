---
title: Ansible Roles

---

# Ansible Role

In 15 minutes

<img height="28%" width="28%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/content/images/dave.png"/>

---

# Goals

- You focus on hard stuff.
- Consumers simply pickup your role.

---

# Role

ansible-role-httpd/tasks/main.yml
```yaml
- name: install httpd software
  yum:
    name: httpd

- name: start and enable software
  service:
    name: httpd
    state: started
    enabled: yes
```

----

# Playbook

```yaml
---
- hosts: all
  become: yes

  roles:
    - httpd

  tasks:
    - name: Place content
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```

----

# Dependencies

```text
    +- Reversed proxy to Tomcat.
    |
    V
+-------+ +-------------+
| httpd | | tomcat      | <- Installs Tomcat.
+-------+ +-------------+
+-----------------------+
| java                  | <- Installs OpenJDK.
+-----------------------+
+-----------------------+
| bootstrap             | <- Ensures python, sudo is installed.
+-----------------------+
```

---

# An idea please

[Inspiration](https://galaxy.ansible.com/)/suggestions:
- ansible-role-httpd
- ansible-role-browsers
- ansible-role-devtools
- ansible-role-opstools

---

# Analyse

- packages
- services
- files
- distributions

* Only Linux OS's please: alpine, fedora, ubuntu, etc.

---

# Code!

```
molecule init role \
  --driver-name docker \
  --role-name ansible-role-MYROLE 

cd ansible-role-MYROLE

vi tasks/main.yml
vi meta/main.yml
```

----

# Molecule

- Environment: molecule/default/molecule.yml
- Playbooks: molecule/default/playbook.yml

----

# Ready? Test!

```
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

