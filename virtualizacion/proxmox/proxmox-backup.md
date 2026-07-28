---
description: Backup en Proxmox
---

# 🚧 Proxmox Backup

No vamos a hablar sobre la importancia de tener un backup porque eso lo doy por sentado. De hecho, trabajando en los proyectos de síntesis de Asix2º hemos padecido la pérdida del trabajo por no tener una copia de seguridad.

Los equipos donde están trabajando son AIO Dell \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ con 4GB de RAM, lo que constituye todo un reto al que nos toca enfrentarnos para poder crear una infraestructura virtual.

Por tanto, en este artículo nos vamos a concentrar en la configuración de copias de seguridad en Proxmox.

Tenemos **dos modos** de hacer un backup para Proxmox:

1. Utilizar un NAS (Truenas) o un USB, HDD externo
2. Proxmox Backup Server

La idea radica en crear un backup de Proxmox y por una parte: enviar a un servidor NAS o dispositivo externo la copia de seguridad que creamos y por la otra aprender a utilizar la herramienta: Proxmox Backup Server.

En el Proxmox podemos configurar el backup en **Datacenter > backup**:

<figure><img src="../../.gitbook/assets/image (864).png" alt="" width="563"><figcaption><p>Backup en Proxmox</p></figcaption></figure>

Una vez creado el backup lo puedo ejecutar para testear:

<figure><img src="../../.gitbook/assets/image (863).png" alt=""><figcaption><p>Ejecutando el backup en Proxmox</p></figcaption></figure>

Incluso, por línea de comandos podemos acceder a la información no solo de los backups:

* En /var/lib/vz/dump - se encuentran los backus creados y los logs.
* En /var/lib/lxc - información de las VM y contenedores creados en Proxmox
* En /var/lib/vz/template/iso - las iso que podemos tener almacenadas en Proxmox

## (1) Backup en almacenamiento externo

En este caso he querido almacenar la copia de seguridad creada en Proxmox en un servidor externo conectado a la misma red de Proxmox. Recuerdo que:

* estoy en VirtualBox
* tengo Proxmox conectado a adaptador puente

Por tanto, tengo acceso al mismo tanto desde mi propio equipo anfitrión como desde una VM Debian conectada a la misma red.

Nota: he configurado samba en un servidor Debian en la red de Proxmox, pero es inaccesible desde afuera. No tengo firewall.

```
nc -zv IP 445
```

<mark style="color:red;">Continuará ...</mark>

## (2) Proxmox Backup Server

* Descargar la ISO e Instalar: [https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)

<figure><img src="../../.gitbook/assets/image (865).png" alt=""><figcaption><p>Descarga de Proxmox Backup Server</p></figcaption></figure>

<mark style="color:red;">Continuará ...</mark>

## Samba

Algunos apuntes al respecto:

```
smbtree
testparm -s
systemctl status nmbd.service
smbclient -U user //IP/name_server
```

## Links

* [https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)
* [https://nosololinux.es/configurar-backups-de-proxmox-en-un-nas-asustor-via-smb-cifs/](https://nosololinux.es/configurar-backups-de-proxmox-en-un-nas-asustor-via-smb-cifs/)
* [https://www.youtube.com/watch?v=TqUuoZ3IKZY](https://www.youtube.com/watch?v=TqUuoZ3IKZY)
* [https://www.youtube.com/watch?v=\_-8N\_MQmN80](https://www.youtube.com/watch?v=_-8N_MQmN80)
* [https://www.redhat.com/en/blog/beginners-guide-firewalld](https://www.redhat.com/en/blog/beginners-guide-firewalld)
