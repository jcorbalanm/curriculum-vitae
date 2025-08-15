---
title: 'Talos Linux: Levantando Kubernetes de la forma correcta'
date: 2025-08-12T8:45:00+01:00
draft: false
type: 'blog'
tags: 
  - Linux
  - Kubernetes

images:
  featured_image: '/img/blog/talos-linux-banner.png'
---

![Logo de Talos Linux](/img/blog/talos-linux-banner.png)

En este artículo voy a hablar un poco de las **ventajas** y **desventajas** que ofrece [Talos Linux](https://www.talos.dev/) a la hora de configurar **clústers de Kubernetes** y **mi experiencia** con este sistema operativo. Se que decir que es "la forma correcta" es un título provocador, pero realmente creo que, si estás levantando un cluster de Kubernetes local o híbrido es una opción muy interesante a considerar. Déjame que te lo explique...

### ¿Qué es Talos Linux?

Como define la propia compañía detras de **Talos Linux**, [Sidero Labs](https://www.siderolabs.com/), Talos Linux es el resultado de diseñar una **distribución de Linux** desde cero y únicamente **para Kubernetes**, obteniendo como resultado una **distribución segura, inmutable y mínima**.

Como resultado de replantear cómo debería ser una distribución de Linux para Kubernetes, se obtienen las siguientes cualidades: 
  - Todo el **mantenimiento y administración** del sistema se realiza mediante una **API**. **No dispone de SSH, shell o consola**.
  - Sólo hay un total de **12 binarios ejecutables** en el sistema.
  - **Sistema de** ficheros de **sólo lectura**.
  - **Sistema operativo inmutable**.

Estas cualidades garantizan una **distribución mínima**, sin **apenas superficies de ataque**, una **eficiente utilización de los recursos**, y **nodos prácticamente efímeros**.

### Sin SSH, ¿Cómo administramos los nodos?

Los **nodos se administran** de la misma forma que acostumbramos a administrar nuestros clústers de Kubernetes, mediante una **herramienta CLI** que hace uso de la **API**. Si en Kubernetes estamos acostumbrados a **_kubectl_**, aquí debemos usar **_talosctl_**. Cuando creamos el clúster se generan un par de **claves pública y privada**, que nos permitirán conectarnos al clúster. 

Esta **CLI** nos permite hacer varias cosas, entre ellas ver un **_dashboard_** con el estado de nuestro **clúster**, y **logs** de cada **nodo**. Sería lo mismo que veremos en las pantallas de los nodos directamente:

![Captura de pantalla de talosctl](/img/blog/talosctl-bigger.png)

Otras de las funciones que podemos realizar son, evidentemente la **creación de un nuevo clúster**, la **aplicación de _patches_** (de esta forma podemos configurar nuestro clúster declarativamente), la **actualización** de la versión de Talos Linux que corre en los nodos, comprobar el **estado de salud** del clúster, o **reiniciar o restablecer** uno o varios nodos.

### Muy bonito pero... ¿Cuales son las desventajas?

Pues, creo que son evidentes. En estos nodos **sólo podrás correr la distribución _vanilla_ de Kubernetes** (siempre puedes añadir y personalizar lo que necesites) **y nada más**, olvídate de tener un agente para monitorizar el sistema, algún servicio sencillo en Docker o binarios externos ejecutándose, puesto que el sistema es prácticamente _bare-bones_.

### Mi experiencia utilizando Talos Linux

Yo personalmente **descubrí Talos Linux** durante la realización de mi **Trabajo de Fin de Grado**, sobre _Gestión de sistemas de alto rendimiento para la ejecución de modelos de Deep Learning_.

Decidí utilizar **Kubernetes para el desarrollo** del proyecto. Al ser mi primera experiencia desplegando Kubernetes, aunque había leído acerca de talos Linux, decidí utilizar _Ubuntu Server_, una distribución con la que tenía experiencia de sobra. Durante la instalación, tuve que añadir repositorios, modificar ficheros de configuración y constantes del sistema, y mientras lo hacía me preguntaba... **¿Realmente es esta la manera correcta de desplegar Kubernetes?** ¿Qué sentido tiene esto para un sistema que es conocido por su declarabilidad? Por lo que volví a la pizarra de ideas y leyendo en internet, volví a encontrarme con **gente recomendando Talos Linux**... Y **una vez lo probé, me convenció completamente**. Ahora **lo utilizo en mi clúster privado** en casa con el que hosteo algunos servicios y sigo aprendiendo sobre **Kubernetes y Docker**.

Evidentemente, si tu clúster de Kubernetes está en la nube, ya dispones de una distribución personalizada por el proveedor, que nos quitará todo este dolor de cabeza de desplegar en local, pero **si por lo que sea tienes un clúster en local o híbrido, os recomiendo que le echéis un vistazo**, igual te convence como a mí.

