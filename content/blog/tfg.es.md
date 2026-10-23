---
title: 'Trabajo Fin de Grado: Sistema Kubeflow para Deep Learning'
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

En una clase de Ingeniería de Mantenimiento de Computadores y Redes, mi tutor puso un reto sobre la mesa: el departamento acababa de adquirir una máquina con dos GeForce RTX 4090, un i9-13900KF y 64 GB de RAM, y necesitaban una forma de que varios investigadores pudieran entrenar modelos de IA en ella de forma colaborativa, sin pisarse el trabajo unos a otros y sin desperdiciar el hardware. Le parecía un buen Trabajo de Fin de Grado. Tenía razón.

## Empezando en casa

Antes de tocar nada en la universidad, usé mi propio homelab para prototipar. Ya tenía Proxmox funcionando en un HP EliteDesk 800 G4 (i5-8500T, 32 GB de RAM), así que levanté seis VMs con Ubuntu 24.04 Server, instalé K3s y empecé a experimentar con MLflow y Kubeflow.

MLflow es elegante y ligero — ideal para registrar experimentos sobre un entorno ya existente. Pero el departamento necesitaba un entorno completo: entrenamiento, pipelines, notebooks, gestión de usuarios. Kubeflow es mucho más pesado de desplegar, pero es el paquete completo. Eso lo decidió.

Ubuntu, en cambio, resultó ser el SO equivocado para los nodos del clúster. Su mutabilidad hacía que reproducir el entorno entre VMs fuera un problema — cada nodo acababa con diferencias sutiles tras unos pocos `apt`. Cambié a Talos Linux, que es inmutable, no tiene acceso por shell, y está diseñado específicamente para ejecutar Kubernetes. El clúster se volvió aburrido de gestionar, que es exactamente lo que buscas.

## Al laboratorio

Llevar el mismo sistema a la máquina de la universidad implicó reinstalar desde cero: Proxmox sobre bare-metal, nuevas VMs, mismo proceso. Lo que no esperaba era la red. La configuración del laboratorio bloqueaba que los nodos descargaran las imágenes de Talos Linux desde internet. La solución fue configurar la VM Debian de administración como registro local de Docker y redirigir todas las descargas de imágenes a través de ella — un componente extra, pero funcionó.

Una vez resuelto, el clúster arrancó, Kubeflow v1.10 se desplegó con `kubectl apply` sobre los manifiestos oficiales, y el sistema quedó en pie. Entonces se acabó el tiempo. La máquina volvió a su configuración anterior.

## Lo que entrega la memoria

El despliegue en la máquina de la universidad es parcial. Lo que sí entregué es una guía completa, paso a paso, que cubre todo el proceso: configuración de la BIOS y activación de IOMMU para el passthrough de GPU, instalación y configuración de Proxmox, creación de los nodos Talos Linux, bootstrap del clúster Kubernetes, configuración de TrueNAS SCALE para almacenamiento persistente en red (el departamento trabaja con datasets de gran tamaño), y arranque de Kubeflow desde cero.

El departamento puede poner esta guía en manos de quien retome el proyecto y tener el sistema funcionando sin tener que descifrar nada.

## El stack

- **Proxmox VE** — hipervisor tipo 1, código abierto, elegido sobre ESXi (Broadcom eliminó el plan gratuito) y Hyper-V (Microsoft retiró el soporte gratuito en 2024)
- **Talos Linux** — SO inmutable y nativo de Kubernetes para los nodos del clúster
- **TrueNAS SCALE** — almacenamiento persistente vía NFS/iSCSI para datasets de modelos de gran tamaño
- **Kubeflow v1.10** — plataforma MLOps completa: pipelines, notebooks, registro de modelos, aislamiento multiusuario

[**Descargar la memoria completa (PDF)**](/memoria-tfg.pdf)
