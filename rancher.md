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

# Set the password

![Set the password for Rancher](images/rancher-set-password.png)

----

# Set the URL

![Set the url for Rancher](images/rancher-set-url.png)

N.B. "All the nodes in your clusters will need to be able to reach this."

----

# Add cloud credentials

![DESCRIPTION](images/rancher-add-cloud-credential.png)

----

# Add node template(s)

![DESCRIPTION](images/rancher-add-node-template.png)

----

# Add cluster

![DESCRIPTION](images/rancher-add-cluster.png)

----

# Wait for cluster

![DESCRIPTION](images/rancher-provisioning-cluster.png)

----

# Wait for the nodes

![DESCRIPTION](images/rancher-provisioning-nodes.png)

---

# Conclusion

1. It's easy to experiment with Rancher. (Did not follow the [best practices though](https://rancher.com/docs/rancher/v2.x/en/overview/architecture-recommendations/#environment-for-kubernetes-installations).
2. Managing Kubernetes cluster just became a click of a button.
3. You can learn Rancher [yourself](https://academy.rancher.com/) for free.
4. Use [this code](https://github.com/robertdebock/ansible-playbook-rancher) yourself.
