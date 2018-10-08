# Galaxy

---

## Vulnerability scanning

----

# molecule.yml

```
platforms:
  - name: somename
    image: fedora:28
```

----

# Plan

1. Scan before applying playbooks.
2. Scan after applying playbook.
3. Report extra vulnerabilities.

----

Galaxy would need to:
- Spin up the `platform`
- Scan & store
- Run playbook
- Scan & store
- Diff before and after
- Show as a quality measure.

----

Arg specs

- default, in here of defaults/main.yml
- Choices, how?
