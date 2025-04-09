# Offline Kubernetes ELK Stack Deployment - Lab Project

> **Disclaimer:** This is a fully offline lab project designed for educational and experimental purposes.
> In production environments, it's highly recommended to install and maintain ELK stack with an online setup for easier upgrades, scalability, and support.

## 📌 Overview

This repository documents the **offline installation and configuration of an ELK Stack (Elasticsearch, Logstash, Kibana)** on a **Kubernetes cluster** (1 master, 5 worker nodes). All Docker images and required packages were manually downloaded and transferred to the offline environment.

> ELK stack is integrated with other critical components including:

- ✅ Ingress-NGINX
- ✅ Longhorn for storage management
- ✅ MetalLB as load balancer

---

## 🎯 Project Goals

- Demonstrate a production-like ELK deployment in an air-gapped (offline) Kubernetes lab.
- Provide reusable manifests, patch commands, and helper scripts.
- Test port forwarding, Ingress exposure, and advanced configuration options using ConfigMaps, services, and affinity rules.
- Showcase full lifecycle: setup, logs collection, service exposure, and scaling.

---

## 🏗️ Architecture Diagram

```plaintext
                                             +---------------------+
                                             |   Windows Hosts     |
                                             | Metricbeat/Filebeat |
                                             +---------+-----------+
                                                       |
                                                       |
+------------------+      +----------------+    +------+-------+
| Longhorn Storage | <--> | Kubernetes PVC |    | Ingress-NGINX |
+------------------+      +----------------+    +------+-------+
                                                       |
                 +-------------------------------------+---------------------+
                 |                  Kubernetes Cluster                         |
                 | +----------------+   +----------------+   +----------------+ |
                 | | Elasticsearch  |   |    Logstash    |   |    Kibana      | |
                 | |   StatefulSet  |   |   Deployment   |   |   Deployment   | |
                 | +----------------+   +----------------+   +----------------+ |
                 +-------------------------------------------------------------+
```
## ⚙️ Installation Guide (All Nodes)

Follow the steps below to prepare all nodes (both **master** and **worker**) for the Kubernetes cluster. These steps are tailored for **offline installation**, using pre-packaged tools and images.

### 1. Set Hostname

Set a unique hostname for each node:

```bash
hostnamectl set-hostname master-1   # Or worker-1, worker-2, etc.
hostnamectl status
```
---

### 2. Add Hostname-to-IP Mapping

Update the `/etc/hosts` file to include all nodes:

```bash
sudo nano /etc/hosts
```

Add entries like:

```
192.168.1.10  master-1
192.168.1.11  worker-1
```

---

### 3. Disable Swap

Kubernetes requires swap to be disabled:

```bash
sudo swapoff -a
sudo nano /etc/fstab  # Remove or comment swap line
free -h               # Confirm swap is 0
```

---

### 4. Install Required Storage Tools

Install iSCSI and NFS support using the offline scripts:

```bash
sudo chmod +x ./tools/iscsi-initiator-utils/install.sh
sudo ./tools/iscsi-initiator-utils/install.sh

sudo chmod +x ./tools/nfs-utils/install.sh
sudo ./tools/nfs-utils/install.sh
```

---

### 5. Install Container Runtime (Containerd)

```bash
sudo chmod +x ./tools/containerd/install.sh
sudo ./tools/containerd/install.sh
```

---

### 6. Import Prebuilt Images

These images are required for the cluster components and apps.

```bash
sudo ctr -n k8s.io image import ./elk.tar
sudo ctr -n k8s.io image import ./ingress.tar
sudo ctr -n k8s.io image import ./k8s-images.tar
sudo ctr -n k8s.io image import ./longhornio.tar
```

---

### 7. Install Kubernetes Binaries

Use the provided offline script to install `kubeadm`, `kubelet`, and `kubectl`:

```bash
sudo chmod +x ./tools/k8s/install.sh
sudo ./tools/k8s/install.sh
```

✅ Your nodes are now ready for Kubernetes initialization.
