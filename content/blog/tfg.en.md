---
title: 'Final Degree Project: Kubeflow system for Deep Learning'
date: 2026-01-22T17:14:00+01:00
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

During a class on Computer and Network Maintenance Engineering, my tutor put a challenge on the table: the department had just acquired a machine with two GeForce RTX 4090s, an i9-13900KF, and 64 GB of RAM, and needed a way for multiple researchers to train AI models on it collaboratively, without stepping on each other's work and without wasting the hardware. He thought it would make a good final degree project. He was right.

## Starting at home

Before touching anything at the university, I used my own homelab to prototype. I already had Proxmox running on an HP EliteDesk 800 G4 (i5-8500T, 32 GB RAM), so I spun up six Ubuntu 24.04 Server VMs, installed K3s across them, and started experimenting with both MLflow and Kubeflow.

MLflow is elegant and lightweight — great for tracking experiments alongside an existing setup. But the department needed a complete environment for training, pipelines, notebooks, and user management. Kubeflow is much heavier to deploy, but it's the full package. That decided it.

Ubuntu, though, turned out to be the wrong OS for the cluster nodes. Its mutability made reproducing the environment across VMs a pain — every node had subtle differences after a few apt runs. I switched to Talos Linux, which is immutable, has no shell access by design, and is built specifically to run Kubernetes. The cluster became boring to manage, which is exactly what you want.

## Moving to the lab

Taking the same setup to the university machine meant reinstalling from scratch: Proxmox on bare metal, new VMs, same process. What I didn't expect was the network. The lab's configuration blocked the nodes from pulling Talos Linux images from the internet. The fix was to set up the Debian admin VM as a local Docker registry and route all image pulls through it — an extra moving part, but it worked.

Once that was sorted, the cluster came up, Kubeflow v1.10 was deployed via `kubectl apply` on the official manifests, and the system was up and running. Then time ran out. The machine was returned to its previous state.

## What the thesis actually delivers

The deployment on the university machine is partial. What I delivered instead is a complete, step-by-step guide covering every stage of the process: BIOS configuration and enabling IOMMU for GPU passthrough, installing and configuring Proxmox, creating the Talos Linux nodes, bootstrapping the Kubernetes cluster, setting up TrueNAS SCALE for persistent shared storage (the department works with large datasets), and standing up Kubeflow from scratch.

The department can hand this guide to whoever picks up the project next and have a working system without having to reverse-engineer anything.

## The stack

- **Proxmox VE** — type-1 hypervisor, open source, chosen over ESXi (Broadcom killed the free tier) and Hyper-V (Microsoft dropped free support in 2024)
- **Talos Linux** — immutable, Kubernetes-native OS for the cluster nodes
- **TrueNAS SCALE** — persistent storage via NFS/iSCSI for large model datasets
- **Kubeflow v1.10** — full MLOps platform: pipelines, notebooks, model registry, multi-user isolation

[**Download the full thesis (PDF, in Spanish)**](/memoria-tfg.pdf)
