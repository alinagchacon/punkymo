---
description: Instalación
---

# NextCloud

## Nextcloud

* ¿Qué es? ¿Cuál sería la utilidad? ¿Qué incluye?
* ¿Qué diferencias hay entre Nextcloud All-in-One y Nextcloud VM?
* Comparativa de los modos de instalación posibles

### &#x20;Instalar versión AIO

Se necesita una VM de Ubuntu Server. Puedes no tener instalado ni Apache ni Nginx, pero necesitas tener instalado Docker.

Necesitamos tener un dominio público y poder abrir puertos para que la guía funcione correctamente. Una vez que tengamos la VM de Ubuntu (yo lo tengo en VMWare - Proxmox), desde un terminal de Linux inicia AIO con este comando:

```
# For x64 CPUs and without a web server or reverse proxy (like Apache, Nginx and else) already in place:
sudo docker run \
--sig-proxy=false \
--name nextcloud-aio-mastercontainer \
--restart always \
--publish 80:80 \
--publish 8080:8080 \
--publish 8443:8443 \
--volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config \
--volume /var/run/docker.sock:/var/run/docker.sock:ro \
nextcloud/all-in-one:latest
```

El proceso de instalación sería algo así como:

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Para abrir la interfaz escribe [https://localhost:8080](https://localhost:8080)  o [https://the.servers.ip.address:8080](https://the.servers.ip.address:8080) en el navegador.&#x20;

<figure><img src="../.gitbook/assets/image (4) (2).png" alt=""><figcaption><p>Nextcloud</p></figcaption></figure>

&#x20;Tienes que utilizar la contraseña que te proporciona, algo así como:

apache bonfire chitchat linked pessimist saloon settling gumdrop

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>



### Instalando a partir de una OVA descargada

Desde [https://nextcloud.com/es/install/](https://nextcloud.com/es/install/) me descargué la imagen de la VM:

\# Cómo tendríamos que hacerlo?

1. Importar la OVA en Virtualbox
2. Switch the network type to Network Bridge in the VM's network settings
3. Log in using 'ncadmin' as username and 'nextcloud' as password.
4. Further info will be shown to you then.

Una vez dentro de la VM me pide la contraseña y upgradear el master&#x20;

## &#x20;Links

1\.      [https://nextcloud.com/es/install/](https://nextcloud.com/es/install/) \*

2\.      [https://nextcloud.com/es/install/#instructions-server](https://nextcloud.com/es/install/#instructions-server) \*\*

2.1.    Docker   - [https://github.com/nextcloud/all-in-one#how-to-use-this](https://github.com/nextcloud/all-in-one#how-to-use-this)

2.2.    Docker   - [https://nextcloud.com/es/blog/como-instalar-nextcloud-all-in-one-en-linux/](https://nextcloud.com/es/blog/como-instalar-nextcloud-all-in-one-en-linux/)

2.3.    VM OVA - [https://download.nextcloud.com/aio-vm/](https://download.nextcloud.com/aio-vm/)

2.4.    Datadir - [https://github.com/nextcloud/all-in-one#how-to-change-the-default-location-of-nextclouds-datadir](https://github.com/nextcloud/all-in-one#how-to-change-the-default-location-of-nextclouds-datadir)

2.5.    Turnkey - [https://www.turnkeylinux.org/nextcloud](https://www.turnkeylinux.org/nextcloud)

&#x20;

##
