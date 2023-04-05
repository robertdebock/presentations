---
title: Production grade Ansible

---

## Production grade Ansible

- Follow along: [robertdebock.nl](https://robertdebock.nl/)

![QR Code](https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/production-grade-ansible/" "QR Code")

---

## About me

- Robert de Bock
- [robertdebock.nl](https://robertdebock.nl/)
- Github: [robertdebock](https://github.com/robertdebock)

---

## Production grade?

What makes great Ansible code?

----

## Great Ansible code

- Easy to read. (`default`, `tasks` & `handlers`.)
- Documented. (`README.md`)
- Input checked. (`meta/argument_specs.yml` or `tasks/assert.yml`)
- Tested. (`molecule`)
- Verified. (`molecule/*/verify.yml`)
- Cleaned-up. (No empty directories or files)

Let's go over each topic in more detail.

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

- Spread code vertically.

```yaml
- name: Horizontal
  ansible.builtin.debug: msg="Hello world!"

- name: Vertical
  ansible.builtin.debug:
    msg: "Hello world!"
```

---

- Keep `defaults`, `tasks` & `handlers` easy to read.
- Hide complexity in `vars`

```yaml
# A variable, normally in `defaults/main.yml`.
country_code: nl

# A map, normally in `vars/main.yml`.
_country_code_to_country_map:
  nl: Netherlands
  ch: Switzerland

# A lookup, normally in `vars/main.yml`
country: "{{ _country_code_to_country_map[country_code] }}
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

## Documented

----

- Have a [generated](https://github.com/robertdebock/ansible-generator) README.md.
- Use [properly](https://ansible-lint.readthedocs.io/rules/name/) named tasks.

---

## Input checked

----

If your code uses variables, you may want to test the input.

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
  assert:
    that:
      - tcp_port is defined
      - tcp_port is number
      - tcp_port > 0
      - tcp_port < 65536
```

----

## meta/argument_specs.yml

```yaml
---

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

## Tested

Prove that your code is working, using [molecule](https://molecule.readthedocs.io/en/latest/).

----

[Describe the infrastructure](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/converge.yml).

----

[Describe what should be done to the target(s)](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/prepare.yml).

----

[Describe how to run the role](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/converge.yml).

---

## Verified

[Describe how to test the target(s)](https://github.com/robertdebock/ansible-role-ntp/blob/master/molecule/default/verify.yml).

---

Let's review the [most popular role on Galaxy](https://github.com/geerlingguy/ansible-role-java).

---

---

## Conculsion

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

# Testing

Let's take a role and review how it's setup.

----

# Reasons

- Prove that your [code works](https://github.com/robertdebock/ansible-role-vault_configuration/actions).
- Have [working examples](https://github.com/robertdebock/ansible-role-vault_configuration/blob/master/molecule/default/converge.yml).
- Helps limit [scope creep](https://github.com/robertdebock/ansible-role-vault_configuration/blob/master/molecule/default/verify.yml)

Quite personal.

---

## Best practices

1. Scope roles. Smaller is almost always better.
2. Never use `ignore_errors`.
3. Use `command`, `shell` and `raw` only when necessary.
4. Use `ansible.builtin` instead of short module names.
5. `ansible-lint` is always right.
6. Use `molecule` to test your code.
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

Great tool for writing code faster. Can procduce non-working code.

---

Idea; use a background image displaying famous art. Gamify by asking the audience to guess the artist.
