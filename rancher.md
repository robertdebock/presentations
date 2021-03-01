---
title: Rancher

---

# Rancher

Follow along: http://robertdebock.nl/

<img src="https://api.qrserver.com/v1/create-qr-code/?size=350x350&data=http://robertdebock.nl/presentations/rancher/"/>

---

# Introduction

----

# Adfinis

----

# Robert de Bock

---

# Rancher @adfinis

----

# Adfinis managed

Adfinis uses Rancher to manage Kubernetes clusters.

```text
+--- Adfinis ---+    +--- Azure|DigitalOcean|AWS|GCP ---+
| Rancher       | -> | - Kubernetes cluster x  <---+    |
+---------------+    | - Kubernetes cluster y      |    |
                     | - Kubernetes cluster z      |    |
                     +-----------------------------+----+
                                                   |
                                                   |
                                     +--- Customer x ---+
                                     |                  |
                                     +------------------+
```

----

# Self-managed clusters

Rancher is used by Adfinis' customers.

```text
+--- Azure|DigitalOcean|AWS|GCP ---+    +--- Customer x ---+
| - Kubernetes cluster x  <- - - - | <- | Rancher          |
+----------------------------------+    +------------------+
```

---

# Cloud readiness

Make sure you're [ready](https://cloudy-with-containers.ch/) for the journey.

---

# Demo

----

# Start Rancher container

As ~~stolen~~ [described](https://rancher.com/quick-start/) by Rancher:

```
docker run \
  --privileged \
  --detach \
  --restart=unless-stopped \
  --publish 80:80 \
  --publish 443:443 \
  rancher/rancher
```

----

# Login to Rancher

https://localhost/

----

# Set the password

![Set the password for Rancher](images/rancher-set-password.png)

----

# Set the URL

----

# Add cloud credentials

----

# Add node template(s)

---

# Add cluster

---

# 7 years later
