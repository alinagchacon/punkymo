---
description: Instalación
---

# Proxmox en VirtualBox

## Instalación

La instalación se puede realizar de manera habitual y para hacer la prueba, utilizaremos los tres archivos a continuación:

* ubuntu-22.04.2-live-server-amd64.iso
* proxmox-ve\_7.3-1.iso

El inconveniente lo tuve a la hora de acceder a la VM de Ubuntu Server, puesto que me da el siguiente error:

TASK ERROR: KVM virtualisation configured, but not available. Either disable in VM configuration or enable in BIOS.

Para corregirlo accedemos a las opciones de la VM y deshabilitamos la aceleración por hardware de KVM. Arrancamos la VM y nos debe permitir la instalación del sistema.

<figure><img src="../../../.gitbook/assets/image (1) (6).png" alt=""><figcaption></figcaption></figure>

