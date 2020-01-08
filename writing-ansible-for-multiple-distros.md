---
title: Writing Ansible roles for multiple distribution

---

# Ansible for multiple distribution

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

A (slightly rewritten) version from Mathieu:

[`tasks/main.yml`](https://gitlab.com/MathieuMD/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: Install required packages for SELinux in Redhat derivatives
  yum:
    name: libselinux-python
    state: present
  when:
    - ansible_os_family == 'RedHat'
```

----

# Benefits

- Understandable for anybody.

# Drawbacks

- My need very similar tasks for every distribution.

---

# Solution 2

A pattern that Ansible God [Jeff Geerling](https://www.jeffgeerling.com/) uses:

[`tasks/main.yml`](https://github.com/geerlingguy/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
---
- name: Include OS-specific variables.
  include_vars: "{{ ansible_os_family }}.yml"
```

----

[`vars/RedHat.yml`](https://github.com/geerlingguy/ansible-role-ntp/blob/master/vars/RedHat.yml)

```yaml
ntp_daemon: ntpd
ntp_tzdata_package: tzdata
ntp_driftfile: /var/lib/ntp/drift
```

----

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

# Drawbacks

- Multiple files to maintain.


---

# Solution 3

A pattern that [I](https://robertdebock.nl/) use a lot:

[`tasks/main.yml`](https://github.com/robertdebock/ansible-role-ntp/blob/master/tasks/main.yml)

```yaml
- name: install {{ ntp_packages }}
  package:
    name: "{{ ntp_packages }}"
    state: present
```

----

[`vars/main.yml`](https://github.com/robertdebock/ansible-role-ntp/blob/master/vars/main.yml)

```yaml
_ntp_packages:
  default:
    - chrony
  RedHat-7:
    - chrony

ntp_packages: "{{ _ntp_packages[ansible_os_family ~ '-' ~ ansible_distribution_marjor_version] | default(_ntp_packages['default']) }}"
```

----

# Benefits

- One file (`vars/main.yml`) contains all differences.
- The `tasks/main.yml` is easy to read.

# Drawbacks

- The `vars/main.yml` is more difficult to understand.
