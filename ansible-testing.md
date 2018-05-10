---
title: Ansible testing

---

# Ansible testing

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ansible-testing/"/>

---

# Immortalize thyself

- See an error, make a [pull request](https://github.com/robertdebock/presentations).
- I'm collecting stars on GitHub.

---

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/containers.jpg"/>

---

# Context

- About [me](https://tookapic.com/robertdb)
- About [work](https://github.com/robertdebock)

---

# Ansible role

tasks/main.yml
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

mywebserver.yml
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

# Galaxy criteria

https://galaxy.ansible.com/

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

The `artifact` (tested code) is published to Galaxy

---

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

I love [ASCII art](https://groups.google.com/forum/#!forum/alt.ascii-art)

---

# Lets Go!

```
$ molecule init role \
  --driver-name docker \
  --verifier-name goss \
  --role-name ansible-role-java
```
----

# Write tests

molecule/default/test_default.yml
```
file:
  /usr/bin/java:
    exists: true
```

See [GOSS documentation](https://github.com/aelsabbahy/goss/blob/master/docs/manual.md) for more information.

----

# Write the role

tasks/main.yml
```
- name: install java (openjdk)
  package:
    - name: java-1.8.0-openjdk
```

There are [many variations](https://github.com/robertdebock/ansible-role-java) of java.

----

# Test it

```
$ molecule test
```

----

# Test it deluxe

molecule/default/molecule.yml
```
dependency:
  name: galaxy
  options:
    role-file: requirements.yml
driver:
  name: docker
lint:
  name: yamllint
platforms:
  - name: java-centos-6
    image: centos:6
  - name: java-centos-7
    image: centos:7
provisioner:
  name: ansible
  lint:
    name: ansible-lint
scenario:
  name: centos
verifier:
  name: goss
  lint:
    name: 'flake8'
```

----

<img height="70%" width="70%" src="https://raw.githubusercontent.com/robertdebock/presentations/master/images/molecule.gif"/>

---

# Limitations

- This just tests the role.
- The developer determines the tests.

---

# Integration tests

```text
+--- GitHub --------+    +--- Travis ---+    +--- Dgtlcn  --+
| - .travis.yml     | -> | - terraform  | -> | droplets:    |
|   /ansible/       |    | - ansible    | -> |   - machine1 |
|     - fnctn1.yml  |    +--------------+    |   - machine2 |
|     - nvntr1      |          |             |   - machine3 |
|     - rqrmnts.yml |          V             |   - machine4 |
|   /terraform/     |    +--- GitHub ---+    |   - machine5 |
|     - fnctn1.tf   |    | Report       |    |   - machine5 |
+-------------------+    +--------------+    +--------------+
```

----

# Ansible 1/2

```
- name: configure items for application servers.
  hosts: application-server
  become: yes
  gather_facts: yes

  tasks:
    - name: tomcat
      include_role:
        name: robertdebock.tomcat
      vars:
        tomcat_layout:
          - name: sample
            directory: /opt/sample
            non_ssl_connector_port: 8080
            ssl_connector_port: 8443
            shutdown_port: 8005
            ajp_port: 8009
            wars:
              - url: https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/sample.war
```

----

# Ansible 2/2

```
- name: configure items for web servers.
  hosts: web-server
  become: yes
  gather_facts: yes

  tasks:
    - name: httpd
      include_role:
        name: robertdebock.httpd
      vars:
        httpd_applications:
        - name: sample
          location: /sample
          backend_url: http://localhost:8080/sample
```

---

# Setup

Quite complex, but broken down into steps it's easier to understand

----

# Preparation

.travis.yml
```
before_install:
  - pip install --upgrade pip
  - pip install ansible
  - pip install ara
  - wget https://releases.hashicorp.com/terraform/"${terraform_version}"/terraform_"${terraform_version}"_linux_amd64.zip
  - mkdir bin
  - unzip terraform_"${terraform_version}"_linux_amd64.zip -d bin
  - chmod 750 bin/terraform
```

----

# Create infra

.travis.yml
```
install:
  - cd terraform ; ssh-keygen -f id_rsa -N ""
  - ../bin/terraform init
  - ../bin/terraform apply -auto-approve -var="do_token=${do_token}" -var="cloudflare_email=${cloudflare_email}" -var="cloudflare_token=${cloudflare_token}"
  - cd ../ansible
  - ansible-galaxy install -r mail-requirements.yml
  - ansible-galaxy install -r infrastructure-requirements.yml
  - ansible-galaxy install -r webapp-requirements.yml
  - ansible-playbook wait.yml
```

----

# Integration

.travis.yml
```
  - ansible-playbook mail.yml
  - ansible-playbook infrastructure.yml
  - ansible-playbook webapp.yml
  - cd ../terraform
  - ../bin/terraform destroy -auto-approve -var="do_token=${do_token}" -var="cloudflare_email=${cloudflare_email}" -var="cloudflare_token=${cloudflare_token}"
  - cd ../ansible
  - ara generate html ../report
  - cd ../
```

----

# Save a report

.travis.yml
```
deploy:
  - provider: pages
    repo: robertdebock/ansible-integration
    target-branch: gh-pages
    local-dir: "report"
    skip-cleanup: true
    github-token: "${github_token}"
    keep-history: false
    on:
      branch: master
```

---

# ARA

[Ansible Runtime Analysis](https://github.com/openstack/ara) or [ARA Records Ansible](https://github.com/openstack/ara).

Used to save a report, useful to see [what happened](https://robertdebock.nl/ansible-integration/).

---

# Loose ends

----

# Goss

[Goss](https://github.com/aelsabbahy/goss) is great, but it's not easy to use variables.

molecule/default/tests/test_default.yml
```
package:
  httpd:
    installed: true
```

- Alpine/Debian/Ubuntu: apache2
- Archlinux: apache
- CentOS/Fedora/OpenSUSE: httpd

----

# Travis

Integration tests don't use the [Travis build matrix](https://docs.travis-ci.com/user/customizing-the-build#Build-Matrix)

I could not figure out how to collect all artifacts (reports) and deploy them to GitHub Pages.

----

# Complexity

It's difficult to draw a line with the integration tests.

For example: A mail integration test can includes:
- postfix
- dovecot
- spamassassin
- clamav
- roundcubemail

----

# When?

I'm not sure if the integration test should run after each unit test.

The unit tests take about 30 minutes, integration about 15 minutes and cost some $0.07.

---

# Conclusion

- It's easy to test roles.
- Testing roles on multiple platforms is simple.
- Integration tests help to run the roles against a more realistic environment.
