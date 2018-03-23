---
title: Ansible testing

---

# Ansible testing

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ansible-testing/"/>

---

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/containers.jpg"/>

---

# Context

- About [me](https://tookapic.com/robertdb)
- About [work](https://github.com/robertdebock)

---

# Ansible

Another [new technology](https://trends.google.com/trends/explore?date=today%205-y&q=%2Fm%2F03d3cjz,%2Fm%2F05zxlz3,%2Fm%2F0k0vzjb,Saltstack)?

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

Warning: I love [ASCII art](https://groups.google.com/forum/#!forum/alt.ascii-art)

---

# Lets Go!

```
molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-java
```
----

# Write tests

```
file:
  /usr/bin/java:
    exists: true
    owner: root
    group: root
```

See [GOSS documentation](https://github.com/aelsabbahy/goss/blob/master/docs/manual.md) for more information.

----

# Write the role

```
- name: install java (openjdk)
  package:
    - name: java-1.8.0-openjdk
```

There are [many variations](https://github.com/robertdebock/ansible-role-java) of java.

----

# Test it

```
molecule test
```

----

# Test it deluxe

```
vi molecule/default/molecule.yml
```

----

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/molecule.gif"/>

---

# Limitations

- This just tests the role
- De developer determines the tests

---

# Conclusion

- It's easy to test roles.
- Testing roles on multiple platforms is simple.
- Integration tests not covered.
