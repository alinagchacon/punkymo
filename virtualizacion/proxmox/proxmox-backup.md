---
description: Backup en Proxmox
---

# 🚧 Proxmox Backup

No vamos a hablar sobre la importancia de tener un backup porque eso lo doy por sentado. De hecho, trabajando en los proyectos de síntesis de Asix2º hemos padecido la pérdida del trabajo por no tener una copia de seguridad.&#x20;

Los equipos donde están trabajando son AIO Dell \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ con 4GB de RAM, lo que constituye todo un reto al que nos toca enfrentarnos para poder crear una infraestructura virtual.

Por tanto, en este artículo nos vamos a concentrar en la configuración de copias de seguridad en Proxmox.

Tenemos **dos modos** de hacer un backup para Proxmox:

1. Utilizar un NAS (Truenas) o un USB, HDD externo
2. Proxmox Backup Server&#x20;

La idea radica en crear un backup de Proxmox y por una parte: enviar a un servidor NAS o dispositivo externo la copia de seguridad que creamos y por la otra aprender a utilizar la herramienta: Proxmox Backup Server.

En el Proxmox podemos configurar el backup en **Datacenter > backup**:

<figure><img src="../../.gitbook/assets/image (401).png" alt="" width="563"><figcaption><p>Backup en Proxmox</p></figcaption></figure>

Una vez creado el backup lo puedo ejecutar para testear:

<figure><img src="../../.gitbook/assets/image (400).png" alt=""><figcaption><p>Ejecutando el backup en Proxmox</p></figcaption></figure>



Incluso, por línea de comandos podemos acceder a la información no solo de los backups:

* En /var/lib/vz/dump - se encuentran los backus creados y los logs.&#x20;
* En /var/lib/lxc - información de las VM y contenedores creados en Proxmox
* En /var/lib/vz/template/iso - las iso que podemos tener almacenadas en Proxmox



## (1) Backup en almacenamiento externo

En este caso he querido almacenar la copia de seguridad creada en Proxmox en un servidor externo conectado a la misma red de Proxmox. Recuerdo que:&#x20;

* estoy en VirtualBox
* tengo Proxmox conectado a adaptador puente

Por tanto, tengo acceso al mismo tanto desde mi propio equipo anfitrión como desde una VM Debian conectada a la misma red.

Nota: he configurado samba en un servidor Debian en la red de Proxmox, pero es inaccesible desde afuera. No tengo firewall.

```
nc -zv IP 445
```

<mark style="color:red;">Continuará ...</mark>

## (2) Proxmox Backup Server

* Descargar la ISO e Instalar: [https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)&#x20;

<figure><img src="../../.gitbook/assets/image (402).png" alt=""><figcaption><p>Descarga de Proxmox Backup Server</p></figcaption></figure>

<mark style="color:red;">Continuará ...</mark>







## Links&#x20;

* [https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)
* [https://nosololinux.es/configurar-backups-de-proxmox-en-un-nas-asustor-via-smb-cifs/](https://nosololinux.es/configurar-backups-de-proxmox-en-un-nas-asustor-via-smb-cifs/)
* [https://www.youtube.com/watch?v=TqUuoZ3IKZY](https://www.youtube.com/watch?v=TqUuoZ3IKZY)&#x20;
* [https://www.youtube.com/watch?v=\_-8N\_MQmN80](https://www.youtube.com/watch?v=_-8N_MQmN80)
