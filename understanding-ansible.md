---
title: Understaning Ansible

---

# Understanding Ansible

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/understanding-ansible/"/>

---

# Simple?

Although ansible is [simple](https://www.ansible.com/blog/simplicity-the-art-of-automation) to learn, it can be [difficult to master](https://en.wikipedia.org/wiki/Bushnell%27s_Law).

---

# Bread

[INSERT IMAGE]

----

# modules - tools

- `package` - kneading machine
- `service` - oven
- `template` - baking mold

----

# roles - baking methods

- `httpd` - in an oven
- `nginx` - in a [pan](https://www.petromax.de/en)
- `lighttpd` - on the [kamado](https://www.kamadojoe.com/grills/)

----

# variables - ingredients

- `httpd_port: 80` - white flour
- `httpd_ssl_port: 443` - spelt flour

----

# playbooks - recipes

- `webserver.yml` - [no knead bread](https://pinchofyum.com/no-knead-bread)

---

# Baking bread

1. Have a few tools - unit test
2. Select a baking method - functional test
3. Buy all ingredients
4. Use some recipe - interation test
