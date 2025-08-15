---
title: 'Talos Linux: Kubernetes setup the right way'
date: 2025-08-12T8:45:00+01:00
draft: false
type: 'blog'
tags: 
  - Linux
  - Kubernetes

images:
  featured_image: '/img/blog/talos-linux-banner.png'
---

![Talos Linux logo](/img/blog/talos-linux-banner.png)

In this article I am going to talk about the **advantages** and **disavantages** that [Talos Linux](https://www.talos.dev/) offers when setting up **Kubernetes clústers** and **my personal experience** with this operating system. I am aware that saying "the right way" is a bit of a provocative title, but I really believe that, if you are setting up a local or hybrid Kubernetes cluster, it is a very interesting option to consider. Let me explain...

### ¿What is Talos Linux?

As the company behind **Talos Linux**, [Sidero Labs](https://www.siderolabs.com/), descibes, Talos Linux is the result of designing a **Linux distribution** from the ground up and only **for Kubernetes**, obtaining as a result a **safe, inmutable and mínimal distribution**.

As a result of rethinking how should a Linux distribution for Kubernetes be, they obtained the next characteristics:
  - All system **maintenance and administration** is performed through an **API**. There is **no SSH, shell, or console access**.
  - The system has **only 12 executable binaries** in total.
  - **Read-only file system**.
  - **Immutable operating system**.

These characteristics ensure a **minimal distribution**, with **almost no attack surfaces**, **an efficient resource usage**, and **virtually ephemeral nodes**.

### Without SSH, how do we manage the nodes?

The **nodes are managed** in the same way we’re used to managing our Kubernetes clusters, through a **CLI tool** that uses the **API**. If in Kubernetes we’re used to using **_kubectl_**, here we use **_talosctl_**. When the cluster is created, a **public/private key** pair is generated, allowing us to connect to the cluster.

This **CLI** allows us to do various things, including viewing a **dashboard** with the **status of the cluster** and the **logs** of each node. It’s the same information we would see on the nodes' screens directly:

![talosctl screenshot](/img/blog/talosctl-bigger.png)

Other functions we can perform include, of course, **creating a new cluster**, **applying patches** (this is how we declaratively configure our cluster), **updating** the version of Talos Linux running on the nodes, checking the **cluster health status**, or r**ebooting or resetting** one or more nodes.

### Amazing but... What are the downsides?

Well, I think they’re quite obvious. On these nodes, **you can only run the vanilla Kubernetes distribution** (although you can always add and customize what you need) **and nothing else**, forget about having a monitoring agent running, a few simple Docker services, or any external binaries, the system is almost bare-bones and locked down.

### My experience using Talos Linux

I personally **discovered Talos Linux** while working on my **Bachelor's Thesis**, which focused on _High-performance system management for Deep Learning model execution_.

I decided to use **Kubernetes for the project development**. Since it was my first time deploying Kubernetes, and although I had read about Talos Linux, I initially chose to go with _Ubuntu Server_, a distribution I was very familiar with. During the setup, I had to add repositories, modify config files and system constants, and while doing so, I kept wondering... **Is this really the right way to deploy Kubernetes?** What sense does this setup have for a system that’s known for being declarative? So I went back to the drawing board and, after reading online, I came across **people recommending Talos Linux** again... **Once I tried it, I was convinced**. I **now use it in my private home cluster** where I host some services and keep learning about **Kubernetes and Docker**.

Obviously, if tyour Kubernetes cluster is in the cloud, the provider will offer a customized distribution that saves you this headache of deploying locally, but **if for some reason you have a local or hybrid cluster, I recommend checking it out**, you might just like it too. 