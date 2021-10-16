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

# Me

- Creative background. (No `and`, `or`, `nor`...)
- Open Source fan.
- [Love coding](https://github.com/robertdebock).
- Husband of [Lucie](https://mylucie.com/), father of [3](https://raw.githubusercontent.com/robertdebock/presentations/master/images/3.jpg).
- Mission: "To create playgrounds for others"

---

# Why

- Prove that your code is working.
- Easier collaboration.
- Examples that _consumers_ can use.
- Prevent [scope creep](https://en.wikipedia.org/wiki/Scope_creep).

---

# Tools

1. [pre-commit](https://pre-commit.com/)
2. [yamllint](https://yamllint.readthedocs.io/en/stable/)
3. [ansible-lint](https://ansible-lint.readthedocs.io/en/latest/)
4. **NEW** [ansible-later](https://ansible-later.geekdocs.de/)
1. [Molecule](https://molecule.readthedocs.io/en/latest/)

----

# pre-commit

Prevent typos by trying a few things before commits.

Should be quick, likely static code tests.

Easy to [roll your own](https://github.com/robertdebock/pre-commit) and [add to your repository](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.pre-commit-config.yaml).

----

# yamllint

Check if the format is [YAML](https://yaml.org/).

Does not understand what _Ansible_ is, just YAML.

Easy to [set your preferences](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.yamllint).

----

# ansible-lint

Give advice on how to better use Ansible.

> "ansible-lint is **always** right."

_Always_ is [configurable](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.ansible-lint).

----

# ansible-later

NEW. Futher hints on how to improve your code.

Likely needs [help on what's right](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.later.yml).

----

# molecule

Dynamic code analysis; actually run your (Ansible) code and verify if it works.

---

# Lets have a look

1. [A random role](https://github.com/robertdebock/ansible-role-haproxy).
2. [The molecule directory](https://github.com/robertdebock/ansible-role-haproxy/tree/master/molecule/default).
3. [CI for GitHub](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.github/workflows/molecule.yml) ([Results](https://github.com/robertdebock/ansible-role-haproxy/actions)).
4. [CI for GitLab](https://github.com/robertdebock/ansible-role-haproxy/blob/master/.gitlab-ci.yml) ([Results](https://gitlab.com/robertdebock/ansible-role-haproxy/-/pipelines)).

----

# Molecule flow

1. [prepare.yml](https://github.com/robertdebock/ansible-role-haproxy/blob/master/molecule/default/prepare.yml)
2. [converge.yml](https://github.com/robertdebock/ansible-role-haproxy/blob/master/molecule/default/converge.yml)
3. [verify.yml](https://github.com/robertdebock/ansible-role-haproxy/blob/master/molecule/default/verify.yml)

---

# Conclusion

- Not in Git(Hu|La)b? It does not exist.
- No CI? It probably does not work.

It's (reasonably) easy to add CI, solves a lot of problems.

---

# THANKS!
