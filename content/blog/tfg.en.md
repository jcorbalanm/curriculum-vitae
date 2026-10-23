---
title: 'Final Degree Project: Kubeflow system for Deep Learning'
date: 2025-12-01T00:00:00+02:00
draft: false
type: 'blog'
tags:
  - Kubernetes
  - Kubeflow
  - Deep Learning
  - Talos Linux
  - Proxmox
  - MLOps
---

During a Computer and Network Maintenance Engineering class, my tutor proposed a challenge: design and deploy a system that would allow the Department of Information Technology and Computing at the University of Alicante to use a freshly acquired machine — an Intel Core i9-13900KF, 64 GB RAM, and **two GeForce RTX 4090s** — for collaborative AI model training. That became my Final Degree Project.

## The problem

The department needed a way to run machine learning workloads — model training, pipeline execution, data analysis — efficiently, concurrently, and with multi-user isolation on their hardware. No cloud, just bare-metal. The constraint: it had to be robust, scalable, and documented well enough for future maintainers who weren't there during development.

## The solution

I designed and deployed a full [Kubeflow](https://www.kubeflow.org/) environment running on [Talos Linux](https://www.talos.dev/) nodes, virtualised on top of [Proxmox VE](https://www.proxmox.com/). The system allows researchers and engineers to run and share ML workloads in a scalable, isolated, and reproducible way, with support for LDAP/SAML2/OAuth2 user management integration.

The stack:
- **Proxmox VE** — hypervisor layer with GPU passthrough via IOMMU
- **Talos Linux** — immutable, Kubernetes-native OS for the cluster nodes
- **TrueNAS SCALE** — persistent cluster storage via NFS/iSCSI
- **Kubeflow** — MLOps platform for training pipelines, notebooks, and model management

## Prototyping on the homelab first

I spent a significant part of the project prototyping on my personal homelab before touching the university's hardware. This meant working through container runtime issues, image pull failures in environments without external internet access, and storage class configuration for stateful workloads — problems much easier to debug at home than on a shared lab machine.

## What was delivered

Due to time constraints, the deployment on the university machine is partial. What I did deliver is a **complete, step-by-step installation and deployment guide** for the full environment — from BIOS configuration and Proxmox setup to bootstrapping the Talos cluster and standing up Kubeflow. The department can follow it to finish the deployment and maintain the system going forward.

## Read the full thesis

The full document (in Spanish) is available as a PDF:

[**Download — Sistema Kubeflow para ejecución de modelos de Deep Learning (PDF)**](/memoria-tfg.pdf)
