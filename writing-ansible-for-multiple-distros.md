---
title: Writing Ansible roles for multiple distribution

---

# Multi distro Ansible

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/writing-ansible-for-multiple-distros/"/>

---

# Problem

The same functionality is implemented differently per distribution.

----

# RHEL 7

- package: npt
- config: /etc/ntp.conf
- service: ntpd

----

## RHEL 8

- package: chrony
- config: /etc/crony.conf
- service: chronyd

---

# Solution 1

A (slightly rewritten) version from [MathieuMD](https://gitlab.com/MathieuMD):

[`tasks/main.yml`](https://gitlab.com/MathieuMD/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: Install required packages for SELinux
  yum:
    name: libselinux-python
    state: present
  when:
    - ansible_os_family == 'RedHat'
```

----

# Benefits

- Understandable for anybody.

----

# Drawbacks

- May need very similar tasks for every distribution.

---

# Solution 2 (1/3)

A pattern that Ansible God [Jeff Geerling](https://www.jeffgeerling.com/) uses:

[`tasks/main.yml`](https://github.com/geerlingguy/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: Include OS-specific variables.
  include_vars: "{{ ansible_os_family }}.yml"
```

----

# Solution 2 (2/3)

[`vars/RedHat.yml`](https://github.com/geerlingguy/ansible-role-ntp/blob/master/vars/RedHat.yml)

```yaml
ntp_daemon: ntpd
ntp_tzdata_package: tzdata
ntp_driftfile: /var/lib/ntp/drift
```

----

# Solution 2 (3/3)

[`tasks/main.yml`](https://github.com/geerlingguy/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: Ensure NTP is running and enabled as configured.
  service:
    name: "{{ ntp_daemon }}"
    state: started
    enabled: true
  when: ntp_enabled | bool
```

----

# Benefits

- Easy to understand and maintain.
- Separated file per distribution.

----

# Drawbacks

- Multiple files to maintain.

---

# Solution 3 (1/2)

A pattern that [I](https://robertdebock.nl/) use a lot:

[`tasks/main.yml`](https://github.com/robertdebock/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: install {{ ntp_packages }}
  package:
    name: "{{ ntp_packages }}"
    state: present
```

----

# Solution 3 (2/2)

[`vars/main.yml`](https://github.com/robertdebock/ansible-role-ntp/blob/master/vars/main.yml)

```yaml
_ntp_packages:
  '8':
    - chrony
  '7':
    - nptd

ntp_packages: "{{ _ntp_packages[ansible_distribution_marjor_version] }}"
```

----

# Benefits

- One file (`vars/main.yml`) contains all.
- The `tasks/main.yml` is easy to read.

----

# Drawbacks

- The `vars/main.yml` is more difficult to understand.

---

# Testing

How do you test multiple distributions?

---

# Travis

### "The simplest way to test and deploy your projects."

#### Travis about Travis.

----

[.travis.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/.travis.yml): (pseudo code)

```yaml
env:
  global:
    namespace="robertdebock"
  matrix:
    - image="fedora" tag="31"
    - image="fedora" tag="latest"
    - image="fedora" tag="rawhide"
    - image="ubuntu" tag="latest"
    - image="ubuntu" tag="bionic"
    - image="ubuntu" tag="xenial"

script:
  - molecule test
```

----

[molecule/defaults/molecule.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/molecule/default/molecule.yml): (pseudo code)

```yaml
platforms:
  - name: "bootstrap-${image:-fedora}-${tag:-latest}"
    image: "${namespace:-robertdebock}/${image:-fedora}:${tag:-latest}"
    command: /sbin/init
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
    privileged: yes
    pre_build_image: yes
```

----

# Experience

- Travis is stable.
- 5 runners for [free](https://www.travis-ci.com/plans).
- Scheduling in web-interface.

---

# GitLab Actions

### "Automate your workflow from idea to production"

#### GitHub about Actions

----

```yaml
name: Ansible Molecule

on:
  push:
    tags_ignore:
      - '*'
  pull_request:
  schedule:
    - cron: '2 2 2 * *'
```

----

```
  test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        config:
          - image: "fedora"
            tag: "31"
          - image: "fedora"
            tag: "latest"
          - image: "fedora"
            tag: "rawhide"
          - image: "ubuntu"
            tag: "latest"
          - image: "ubuntu"
            tag: "bionic"
          - image: "ubuntu"
            tag: "xenial"
```

----

```
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: molecule
        uses: robertdebock/molecule-action@2.3.4
        with:
          image: ${{ matrix.config.image }}
          tag: ${{ matrix.config.tag }}
          options: "--parallel all"
        env:
          TOX_PARALLEL_NO_SPINNER: 1
```

----

# Experience

- 20 runners!
- A little faster than Travis.
- Runners are ["tainted"](https://github.com/actions/virtual-environments/tree/master/images/linux).
- [Actions on Marketplace are cool](https://github.com/marketplace).
