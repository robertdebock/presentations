---
title: Ansible speedy code

---

# Ansible speedy code

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ansible-speed/"/>

---

# About me

- Love to [tinker](https://robertdebock.nl/).
- Specialized in infrastructure as code.

---

# About this

A presentation to explain different coding scenarios and the impact to speed in Ansible. Mostly coding practices.

---

# Defaults

- "Ansible is slow!"
- "Ansible does not scale!"

---

# ansible.cfg

```cfg
[defaults]
forks = 50
```

---

# Strategy

## 24% to be gained.

| Strategy | Time(s) | Improvement(%) |
| -------- | ------- | -------------- |
| linear   | 0.854   | 0 (baseline)   |
| free     | 0.643   | 24             |

----

```yaml
- name: Do something
  hosts: all
  strategy: free
```

- [code free](https://github.com/robertdebock/ansible-speedy-code/blob/master/strategy-free.yml)
- [code linear](https://github.com/robertdebock/ansible-speedy-code/blob/master/strategy-linear.yml)

----

# Strategy limitations

- `free` can't be used when depending tasks.

---

# Serial

## -15% to be gained.

| Serial | Time(s) | Improvement(%) |
| ------ | ------- | -------------- |
| 100%   | 0.658   | 0 (baseline)   |
| 50%    | 0.759   | -15            |

----

```yaml
- name: Do something
  hosts: all
  serial: 50%
```

[code 100%](https://github.com/robertdebock/ansible-speedy-code/blob/master/serial-1.yml)
[code 50%](https://github.com/robertdebock/ansible-speedy-code/blob/master/serial-2.yml)

---

# Conditions

```yaml
- name: Do something
  ansible.builtin.debug:
    msg: "Hello RedHat"
  when:
    - ansible_os_family == "RedHat"

- name: Do some other thing
  ansible.builtin.debug:
    msg: "Hello Debian!"
  when:
    - ansible_os_family == "Debian"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/conditions-1.yml)

----

# Faster, with limitations

```
- name: Do something
  ansible.builtin.debug:
    msg: "Hello {{ ansible_os_family }}!"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/conditions-2.yml)

----

# Faster without limitations (1/2)

vars/main.yml:

```
_message:
  Debian: "Linux is a way of living."
  RedHat: "Linux is my profession."
message: "{{ _message[ansible_os_family] }}"
```

----

# Faster without limitations (2/2)

tasks/main.yml:

```yaml
- name: Display message
  ansible.builtin.message:
   debug:
     - msg: "{{ message }}"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/conditions-3.yml)

---

# Faster without limitations benefits

- It's faster, 1 task instead of 2.
- The "data"/"config" (`vars/main.yml`) is in 1 file.
- The "logic" (`tasks/main.yml`) is easier to read.

---

# Block and include

Which is faster?

----

Tasks in a `block` with  shared condition:

```yaml
- name: Do something
  when:
    - ansible_os_family == "RedHat"
  block:
    - name: Show first
      ansible.builtin.debug:
        msg: "One"

    - name: Show second
      ansible.builtin.debug:
        msg: "Two"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/block.yml)

----

Tasks conditionally included:

```yaml
- name: Include RedHat tasks
  ansible.builtin.include_tasks: RedHat.yml
  when:
    - ansible_os_family == "RedHat"

- name: Include Debian tasks
  ansible.builtin.include_tasks: Debian.yml
  when:
    - ansible_os_family == "Debian"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/include-1.yml)

----

# Better:

```yaml
- name: Include OS specific tasks
  ansible.builtin.include_tasks: "{{ ansible_os_family }}.yml"
```

[code](https://github.com/robertdebock/ansible-speedy-code/blob/master/include-2.yml)

---

# Gathering facts

Facts are quite expensive to have.
Yes or no
Subsets

---

# Conclusion
