---
title: Production grade Ansible

---

## Production grade Ansible

- Follow along: [robertdebock.nl](https://robertdebock.nl/)

![QR Code](https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/production-grade-ansible/ "QR Code")

---

## About me

- Robert de Bock
- [robertdebock.nl](https://robertdebock.nl/)
- Github: [robertdebock](https://github.com/robertdebock)

---

# Game on!

Guess the title or artist of the image.

---

<img src="https://live.staticflickr.com/2861/33925744335_2327cf2a37_k.jpg" width="40%" height="40%">

---

## Production grade?

What makes great Ansible code?

----

## Great Ansible code

- Easy to read. (`default`, `tasks` & `handlers`.)
- Documented. (`README.md`)
- Input checked. (`meta/argument_specs.yml` or `tasks/assert.yml`)
- Tested (in CI). (`molecule`)
- Verified. (`molecule/*/verify.yml`)
- Cleaned-up. (No empty directories or files)

Let's go over each topic in more detail.

---

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Mona_Lisa%2C_by_Leonardo_da_Vinci%2C_from_C2RMF_retouched.jpg/1280px-Mona_Lisa%2C_by_Leonardo_da_Vinci%2C_from_C2RMF_retouched.jpg" alt="Mona Lisa - Leonardo Davinci" height="40%" width="40%">

---

## Easy to read

----

[![CodeAethetic](https://img.youtube.com/vi/-J3wNP6u5YU/0.jpg)](https://www.youtube.com/watch?v=-J3wNP6u5YU)

----

- Use understandable (variable) names.

```yaml
# Difficult to relate to:
i_n: abcde

# Easier to relate to:
instance_name: abcde
```

----

Think about "string", "list" or "dict":

```yaml
default_shell: /bin/bash

users:
  - name: robert
  - name: kees
    group: kees
    groups:
      - docker

password_policy:
  minimum_length: 8
  maximum_length: 16
```

----

- Spread code vertically.

```yaml
- name: Horizontal
  ansible.builtin.debug: msg="Hello world!"

- name: Vertical
  ansible.builtin.debug:
    msg: "Hello world!"
```

---

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Johannes_Vermeer_-_Het_melkmeisje_-_Google_Art_Project.jpg/1920px-Johannes_Vermeer_-_Het_melkmeisje_-_Google_Art_Project.jpg" alt="Melk meisje - Johannes Vermeer" height="40%" width="40%">

---

- Keep `defaults`, `tasks` & `handlers` easy to read.
- Hide complexity in `vars`

```yaml
# A variable, normally in `defaults/main.yml`.
country_code: nl

# A map, normally in `vars/main.yml`.
_country_code_to_country_map:
  nl: Netherlands
  no: Norway # Interesting, a boolean...
  ch: Switzerland

# A lookup, normally in `vars/main.yml`
country: "{{ _country_code_to_country_map[country_code] }}"
```

----

```text
             +---------------------------------+
             |                                 |
             | Shitty code with a good facade  |
             |        is better than           |
             | good code with a shitty facade. |
             |                                 |
             +---------------------------------+
```

---

<img src="https://upload.wikimedia.org/wikipedia/en/d/de/Piss_Christ_by_Serrano_Andres_%281987%29.jpg" alt="Piss Christ - Andres Serrano" height="40%" width="40%">

---

## Documented

----

- Have a [generated](https://github.com/robertdebock/ansible-generator) README.md.
- Use [properly](https://ansible-lint.readthedocs.io/rules/name/) named tasks.

---

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b2/Caravaggio_Judith_Beheading_Holofernes.jpg" alt="Caravaggio - Judith beheading Holofernes" height="40%" width="40%">

---

## Input checked

----

If your code uses variables, test the input.

----

## tasks/main.yml

```yaml
- name: Configure TCP port
  ansible.builtin.lineinfile:
    path: /some/config.txt
    line: "port {{ tcp_port }}
```

----

## tasks/assert.yml

```yaml
- name: Check if tcp_port variable is set correctly
  ansible.builtin.assert:
    that:
      - tcp_port is defined
      - tcp_port is number
      - tcp_port > 0
      - tcp_port < 65536
```

----

## meta/argument_specs.yml

```yaml
argument_specs:
  main:
    short_description: A short description.
    description: A long description.
    author: Robert de Bock
    options:
      tcp_port:
        type: "int"
        default: 443
        description: The port to bind on.
```

----

## Argument specs

- An [example](https://github.com/robertdebock/ansible-role-ntp/blob/master/meta/argument_specs.yml)
- More [documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#role-argument-validation).

Fast, though not very complete.

---

<img src="https://fabian-claude-walter.com/fcw/wp-content/uploads/Sally-Man_-Listening-to-Madonna-by-the-Tadpole-Jar.jpg" alt="Sally Mann - Listening to Madonna" height="40%" width="40%">

---

## Tested

Prove that your code is working, using [molecule](https://molecule.readthedocs.io/).

----

[Describe the infrastructure](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/molecule.yml).

----

[Describe what should be done to the target(s)](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/prepare.yml).

----

[Describe how to run the role](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/converge.yml).

---

<img src="https://upload.wikimedia.org/wikipedia/commons/0/00/Rietveld_chair_1.JPG" alt="Gerrit Rietveld - Stoel" height="40%" width="40%">

---

## Verified

[Describe how to test the target(s)](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/verify.yml).

---

<img src="https://www.jackson-pollock.org/images/paintings/convergence.jpg" alt="Jackson Pollock - Convergence" height="40%" width="40%">

---

Let's review the [most popular role on Galaxy](https://github.com/geerlingguy/ansible-role-java).

---

<img src="https://upload.wikimedia.org/wikipedia/en/b/b9/MagrittePipe.jpg" alt="Rene Margeritte" height="40%" width="40%">

---

## Different ways

There are quite some methods to accomodate different needs. Let's take distribution differences for the Apache HTTPD package.

| distribution | package |
| ------------ | ------- |
| Debian       | apache2 |
| RedHat       | httpd   |

----

## Solution "when"

```yaml
- name: Install Apache HTTPD Debian
  ansible.builtin.package:
    name: apache2
  when:
    - ansible_distribution == "Debian"

- name: Install Apache HTTPD RedHat
  ansible.builtin.package:
    name: httpd
  when:
    - ansible_distribution == "RedHat"
```

----

## Solution "include_tasks"

```yaml
- name: Install Apache HTTPD
  ansible.builtin.include_tasks:
    file: install_{{ ansible_distribution | lower }}.yml
```

```yaml
# install_debian.yml
- name: Install Apache HTTPD
  ansible.builtin.package:
    name: apache2
```

```yaml
# install_redhat.yml
- name: Install Apache HTTPD
  ansible.builtin.package:
    name: httpd
```

----

## Solution "include_vars"

```yaml
- name: Install Apache HTTPD
  ansible.builtin.include_vars:
    file: "{{ ansible_distribution | lower }}.yml"
```

```yaml
# debian.yml
apache_httpd_package: apache2
```

```yaml
# redhat.yml
apache_httpd_package: httpd
```

----

## Solution "lookup"

```yaml
- name: Install Apache HTTPD
  ansible.builtin.package:
    name: "{{ apache_package }}"
```

```yaml
# vars/main.yml
_apache_packages:
  Debian: apache2
  RedHat: httpd

apache_package: "{{ _apache_packages[ansible_distribution] }}"
```

---

<img src="https://upload.wikimedia.org/wikipedia/commons/d/d0/Panorama_mesdag.PNG" alt="Panorama Mesdag - Hendrik Willem Mesdag" height="40%" width="40%">

---

## Conclusion

```text
+--- default ---+      +--- assert ---+
|               | ---> |              |
+---------------+      +--------------+
                              |
       +----------------------+
       |
       V
+--- tasks ---+      +--- verify ---+
|             | ---> |              |
+-------------+      +--------------+
```

----

# Reasons

- Prove that your [code works](https://github.com/robertdebock/ansible-role-vault_configuration/actions).
- Have [working examples](https://github.com/robertdebock/ansible-role-vault_configuration/blob/master/molecule/default/converge.yml).
- Helps limit [scope creep](https://github.com/robertdebock/ansible-role-vault_configuration/blob/master/molecule/default/verify.yml)

Quite personal.

---

<img src="https://upload.wikimedia.org/wikipedia/commons/3/3a/La_ronda_de_noche%2C_por_Rembrandt_van_Rijn.jpg" alt="Rembrandt van Rijn - Nachtwacht" height="40%" width="40%">

---

## Best practices (1/2)

1. Scope roles. Smaller is almost always better.
2. Never use `ignore_errors`.
3. Use `command`, `shell` and `raw` only when necessary.
4. Use `ansible.builtin` instead of short module names.
5. `ansible-lint` is always right.
6. Use `molecule` to test your code.

----

## Best practices (2/2)

7. Set sane default values for variables.
8. Use `meta/argument_specs.yml` to validate input.
9. Use `meta/main.yml` to describe your role.
10. Use `README.md` to describe your role.
11. Use `tasks/assert.yml` to validate input.

---

## Role scoping

How to scope a role?

- Smaller is better. (*)
- Describe what the role does first. (`README.md`)
- Repetetive code calls for splitting a role.

----

## Artifical Intelligence

Great tool for writing code faster. Can produce convincing, but non-working code.

---

## That's all folks!
