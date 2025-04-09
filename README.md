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
