---
title: Ansible use cases

---

# Ansible use cases

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/ansible-use-cases/"/>

---

# About me

- Love to [tinker](https://robertdebock.nl/).
- Specialized in infrastructure.
- Fan of [Jeff Geerling](https://www.jeffgeerling.com) / [geerlingguy](https://github.com/geerlingguy)

---

# Use case "Traditional"

----

Configure one or more systems.

- [bootstrap](https://github.com/robertdebock/ansible-role-bootstrap)
- [java](https://github.com/robertdebock/ansible-role-java)
- [tomcat](https://github.com/robertdebock/ansible-role-tomcat)

----

Benefit over shell-scripts:

- Easier to test. ([molecule](https://molecule.readthedocs.io/en/latest/))
- More structured.
- Idempotent.

---

# Use case "Security"

----

- [Patching](https://github.com/robertdebock/ansible-role-update)
- CVE testing [2018_19788](https://github.com/robertdebock/ansible-role-cve_2018_19788) and [2021_44228](https://github.com/robertdebock/ansible-role-cve_2021_44228)

---

# Use case "Scripts"

----

Tools that I use.

- [ansible-generator](https://github.com/robertdebock/ansible-generator/blob/master/generate.yml)
- [gitlab-settings](https://github.com/robertdebock/ansible-generator/blob/master/generate.yml)

---

# Use case "Terraform"

----

- Terraform for IaC
- Ansible instance configuration
- Ansible for orchestration

----

- [Ansible drops an inventory](https://gitlab.com/robertdebock/gitlab-demo/-/blob/master/terraform/main.tf#L155)
- [Ansible calls Terraform](https://github.com/robertdebock/ansible-playbook-cloudtop/blob/master/ansible/playbook.yml#L14)

---

# Use case "Others"

----

- [Ansible CMDB](https://github.com/fboender/ansible-cmdb)
- [Bios updates](https://github.com/robertdebock/ansible-role-bios_update)
---

# Conclusion

- Ansible roles are the tools in my toolbox.
- Ansible shines with structured data.
- Ansible is "low-code".
- Ansible structures my world.
