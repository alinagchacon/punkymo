---
description: NTech Admin Group
---

# Proxmox

## Introducción

Proxmox es un entorno de virtualización empresarial de código abierto. Un sistema operativo basado en un Debian, con un kernel modificado de RHEL (Red Hat). El sistema operativo tiene una interfaz web por donde se administra todo su contenido, las herramientas y comandos necesarios.

Durante años VMware ha sido el estándar de facto en temas de virtualización tanto en entornos empresariales como personales. No obstante, han surgido diferentes alternativas muy bien acogidas no solo por los usuarios (estudiantes) sino también por las empresas, como es el caso de Proxmox.&#x20;

Proxmox es open source y una de las opciones más completas que hay que permite construir infraestructuras de virtualización modernas sin depender de licencias.  Tiene dos tipos de virtualización compatibles:

* **Contenedores** basados en Linux containers (LXC): que permite ejecutar múltiples sistemas operativos en un solo host comparitendo el mismo núcleo del sistema operativo anfitrión
* La virtualización con **KVM** (Kernel **Virtual Machine**): de este modo permite virtualizar hardware así como ejecutar múltiples sistemas operativos en una sola máquina física.

Por tanto, Proxmox es una solución potente de código abierto para entornos de virtualización empresarial donde domina VMware ESXi y Hyper-V que son entornos con un alto coste económico.

#### Dos características importantes de Proxmox

Proxmox utiliza KVM, Kernel-based Virtual Machine, integrado en el kernel de Linux:  es un hipervisor de tipo 1, y por tanto, puedes ejecutar máquinas virtuales con un rendimiento cercano al hardware físico. Adicionalmente, asegura eficiencia, aislamiento y soporte para diferentes sistemas operativos.

Además y muy importante, Proxmox nos permite ejecutar contenedores de Linux, LXC. Este tipo de contenedores, al igual que los contenedores Docker,  son igualmente ligeros y utilizan mecanismos de aislamiento del kernel que permiten garantizar que los procesos en ejecución dentro de un contenedor no afecten a otros contenedores ni al sistema host. SIn embargo, funcionan a nivel de sistema operativo en vez de a nivel de proceso. ¿Qué quiere decir esto? pues que en LXC lo único que necesitas es el sistema operativo, mientras que en Docker necesitas de una aplicación.&#x20;

No todo son ventajas y entre los inconvenientes que tiene es que los contenedores no son tan portables  entre sistemas y solo se adaptan a arquitecturas Linux.

{% hint style="info" %}
Las máquinas virtuales tradicionales emulan todo un sistema operativo, mientras que los contenedores Linux comparten el mismo kernel del sistema operativo anfitrión, por lo son más eficientes y rápidos.&#x20;
{% endhint %}

#### ¿Cómo funciona LXC?

Los contenedores LXC  se basan en las funcionalidades de virtualización que brinda el kernel de Linux, como los `namespaces` y los `grupos de control`  o `cgroups:`&#x20;

* **namespaces -** permiten aislar diferentes aspectos del sistema, como el sistema de archivos, el entorno de red y los identificadores de proceso.&#x20;
* **cgroups o grupos de control -** gestionan los recursos asignados a los contenedores, como la memoria y la CPU.

Cuando creamos un contenedor LXC, se crea un nuevo **namespace** y se asignan recursos aislados para dicho contenedor, lo que permite que el contenedor tenga su propia vista del sistema, con su propio sistema de archivos, interfaces de red y procesos.&#x20;

{% hint style="info" %}
Todos los contenedores comparten el mismo kernel del sistema operativo host, lo que les permite ejecutarse de manera eficiente y sin la sobrecarga de un sistema operativo completo.
{% endhint %}



