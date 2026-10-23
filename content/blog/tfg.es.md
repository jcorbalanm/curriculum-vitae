---
title: 'Trabajo Fin de Grado: Sistema Kubeflow para Deep Learning'
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

Durante una clase de Ingeniería de Mantenimiento de Computadores y Redes, mi tutor me propuso un reto: diseñar y desplegar un sistema que permitiera al Departamento de Tecnología Informática y Computación de la Universidad de Alicante aprovechar una máquina de reciente adquisición — un Intel Core i9-13900KF, 64 GB de RAM y **dos GeForce RTX 4090** — para el entrenamiento colaborativo de modelos de IA. Ese fue mi Trabajo de Fin de Grado.

## El problema

El departamento necesitaba una forma de ejecutar cargas de trabajo de machine learning — entrenamiento de modelos, pipelines, análisis de datos — de manera eficiente, concurrente y con aislamiento multiusuario sobre su propio hardware. Sin nube, sobre bare-metal. La condición: tenía que ser robusto, escalable y suficientemente documentado para que futuros administradores que no estuvieran durante el desarrollo pudieran mantenerlo.

## La solución

Diseñé y desplegué un entorno completo de [Kubeflow](https://www.kubeflow.org/) sobre nodos con [Talos Linux](https://www.talos.dev/), virtualizados sobre [Proxmox VE](https://www.proxmox.com/). El sistema permite a investigadores e ingenieros ejecutar y compartir cargas de trabajo de ML de forma escalable, aislada y reproducible, con soporte de integración de usuarios vía LDAP/SAML2/OAuth2.

El stack:
- **Proxmox VE** — capa de hipervisor con passthrough de GPU vía IOMMU
- **Talos Linux** — SO inmutable y nativo de Kubernetes para los nodos del clúster
- **TrueNAS SCALE** — almacenamiento persistente del clúster vía NFS/iSCSI
- **Kubeflow** — plataforma MLOps para pipelines de entrenamiento, notebooks y gestión de modelos

## Prototipado en el homelab primero

Dediqué una parte importante del proyecto a prototipado en mi homelab personal antes de tocar el hardware de la universidad. Esto implicó resolver problemas con el runtime de contenedores, fallos de descarga de imágenes en entornos sin acceso a internet externo, y configuración de storage classes para cargas con estado — problemas mucho más fáciles de depurar en casa que en una máquina compartida del laboratorio.

## Lo que se entregó

Por limitaciones de tiempo, el despliegue en la máquina de la universidad es parcial. Lo que sí entregué es una **guía completa de instalación y despliegue**, paso a paso, para todo el entorno — desde la configuración de la BIOS y la instalación de Proxmox hasta el bootstrap del clúster Talos y el arranque de Kubeflow. El departamento puede seguirla para completar el despliegue y mantener el sistema en el futuro.

## Leer la memoria completa

El documento completo está disponible en PDF:

[**Descargar — Sistema Kubeflow para ejecución de modelos de Deep Learning (PDF)**](/memoria-tfg.pdf)
