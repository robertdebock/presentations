---
title: CI/CD

---

# CI/CD

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ci-cd/"/>

---

# Journey

## code -> service

How does code go from a developer to a service?

---

# Components

```yaml
- name: install haproxy
  package:
    name: haproxy
    state: present
```

----

# Components

Code in itself is not so interesting, it needs to be:

- Tested.
- Documented.
- Versioned.
- Packaged.
- Released.

----

# GitLab can:

- Do the [tests](https://gitlab.com/robertdebock/ansible-role-haproxy/-/pipelines).
- Provide a [documentation area](https://gitlab.com/robertdebock/ansible-role-haproxy#haproxy).
- Release [versions](https://gitlab.com/robertdebock/ansible-role-haproxy/-/releases).
- Provide [packages](https://gitlab.com/robertdebock/ansible-role-haproxy/-/packages).

----

# Conclusion

GitLab helps component maintainers simplify development. The only interface required is (git and) GitLab.

Components typically are not sufficient to build a full service. Typically _many_ components make up a service.

---

# Running a service

```yaml
- name: install my service
  vars_files:
    - my_configuration.yml

  roles:
    - x
    - y
    - haproxy
    - z
```

----

# Running a service

Product owners and stakeholders typically focus on the service, not the component.

Issues, roadmaps and milestones help structure work.

----

# Running a service

Developers (and operators) hate [context switching](https://en.wikipedia.org/wiki/Context_switch). One interface helps people get into a "flow".

The closer to the code the better.

---

# Competitors

----

# OpenShift

RedHat OpenShift is a feature-rich product that can deploy (Helm) code automatically.

----

# OpenShift

## Benefits

- Stay in the RedHat eco-system.
- One product to solve multiple challenges.

----

# OpenShift

## Drawbacks

- You would still need a tool for all other development work.
- It's not the same thing.

----

# GitHub

## Benefits

- Older (2008, GitLab 2011).
- Bigger (69m, GitLab 0.5m).

----

# Drawbacks

- More expensive.
- Less features.
- Slower CI.

----

Sources:

- [GitLab vs GitHub](https://usersnap.com/blog/gitlab-github/).
- [GitLab vs GitHub)[https://kinsta.com/blog/gitlab-vs-github/).
