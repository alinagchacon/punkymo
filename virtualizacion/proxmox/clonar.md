---
description: Clonación en Proxmox
---

# Clonar

Una característica interesante de Proxmox es la facilidad con la que se puede clonar una VM. Para ello, por supuesto que necesitamos tener instalada una VM y apagada.

Bastaría clicar sobre la VM y seleccionar la opción de **template**. Por tanto, podemos distinguir:

* **Template o plantilla**:  se trata de VM o contenedores  preconfigurados. Se implementan con un par de clics.&#x20;
* **Clon linked o vinculado**: una VM clonada vinculada requiere menos espacio en disco, pero no puede ejecutarse sin acceso a la plantilla de VM base.&#x20;
* **Clon completo o full**: se trata de una copia completa y totalmente independiente de la VM o la plantilla de VM original, pero requiere el mismo espacio en disco que el original.

Tener en cuenta que al clonar una VM la máquina clonada estará con la misma configuración de red que la original, por lo tanto, he modificado la IP estática en el archivo de configuración de `/etc/netplan/00-installer-config.yaml`.



### Links

* [https://pve.proxmox.com/wiki/VM\_Templates\_and\_Clones](https://pve.proxmox.com/wiki/VM\_Templates\_and\_Clones)

