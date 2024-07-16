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

Un modo de corregirlo fue accediendo a las opciones de la VM y deshabilitamos la aceleración por hardware de KVM. Arrancamos la VM y nos debe permitir la instalación del sistema.

<figure><img src="../../.gitbook/assets/image (1) (6) (2).png" alt=""><figcaption><p>Dentro de Proxmox, las opciones para la VM</p></figcaption></figure>

No obstante, esto no es suficiente. La instalación de la VM se hace muy lenta y termina dando problemas, con lo cual seguí buscando soluciones. Dado que tengo Linux instalado en mi PC tuve que buscar como hacerlo y claro está, desde el terminal.

La virtualización anidada en el sistema Linux está deshabilitada. A partir de VirtualBox 6.1, podemos habilitarla para Intel y AMD.

Lo mejor siempre es ir al sitio oficial y en este caso sería: [https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html](https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html)

Hacía tiempo que quería aprender a controlar mis VM desde el terminal, así que esta ha sido una buena oportunidad.&#x20;

El comando que permite habilitar la opción de <mark style="color:blue;">`VT-x/AMD-V anidado`</mark> para determinada VM es el que aparece a continuación:

```
VBoxManage modifyvm PROXMOX --nested-hw-virt on
```

En la línea anterior PROXMOX es el nombre de la VM a la que quiero habilitar la virtualización anidada. Como se puede ver en la imagen ya está habilitado.

<figure><img src="../../.gitbook/assets/image (243).png" alt=""><figcaption><p>Habilitar VT-x/AMD-V anidado</p></figcaption></figure>

Como siempre, dejo algunos links que me han sido útiles.

### Links

* [https://www.youtube.com/watch?v=JMT2qimIL9Q](https://www.youtube.com/watch?v=JMT2qimIL9Q)&#x20;
* [https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html](https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html)
* [https://www.virtualbox.org/manual/ch08.html](https://www.virtualbox.org/manual/ch08.html)
* [https://redessy.com/como-habilitar-la-virtualizacion-anidada-en-virtualbox-en-linux/?expand\_article=1](https://redessy.com/como-habilitar-la-virtualizacion-anidada-en-virtualbox-en-linux/?expand\_article=1)
