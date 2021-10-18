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

N.B. Run this on a resolvable node...

----

# Login to Rancher

----

# Welcome

![Welcome screen for Rancher](images/rancher-welcome.png)

----

# Get the password

![Get the password](images/rancher-bootstrap-password.png)

----

# Set the password

![Set the password](images/rancher-set-password.png)

----

# Add cloud credentials

![Add cloud credentials](images/rancher-add-cloud-credentials-1.png)

----

![Add cloud credentials](images/rancher-add-cloud-credentials-2.png)

----

![Add cloud credentials](images/rancher-add-cloud-credentials-3.png)

----

![Add cloud credentials](images/rancher-add-cloud-credentials-4.png)

----

# Create node template(s)

![Add node templates](images/rancher-create-node-template-1.png)

----

![Add node templates](images/rancher-create-node-template-2.png)

----

![Add node templates](images/rancher-create-node-template-3.png)

----

![Add node templates](images/rancher-create-node-template-4.png)

----

# Create cluster

![Create cluster](images/rancher-create-cluster-1.png)

----

![Create cluster](images/rancher-create-cluster-2.png)

----

![Create cluster](images/rancher-create-cluster-3.png)

----

![Create cluster](images/rancher-create-cluster-4.png)

---

# Your turn!

Go to https://rancher.meinit.nl/

(Use "Adfinis!")

---

# Conclusion

1. It's easy to experiment with Rancher. (Did not follow the [best practices though](https://rancher.com/docs/rancher/v2.x/en/overview/architecture-recommendations/#environment-for-kubernetes-installations).
2. Managing Kubernetes cluster just became a click of a button.
3. You can learn Rancher [yourself](https://academy.rancher.com/) for free.
4. Use [this code](https://github.com/robertdebock/ansible-playbook-rancher) yourself.
