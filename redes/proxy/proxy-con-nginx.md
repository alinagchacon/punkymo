---
description: Configuración de un proxy en Nginx
---

# Proxy con Nginx

En este tutorial vamos a configurar un servidor Ubuntu con Nginx como un proxy para un servidor que también tiene instalado un servidor web con Nginx en Ubuntu Linux.

En mi caso voy a utilizar dos VM:

| Equipo              | IP                   |
| ------------------- | -------------------- |
| Ubuntu Server 23.10 | 192.168.2.4          |
| Ubuntu Desktop      | 192.168.2.103 (DHCP) |
| Red NAT             |                      |

En el Ubuntu Server tengo instalado un servidor de DNS, el servicio DHCP así como dos sitios web: www.kirby.com y www.punky.com.  Para el sitio web kirby.com he habilitado el módulo de PHP y he configurado un certificado openSSL. El sitio web punky.com es solo una página web estática.&#x20;

En la VM con Ubuntu Desktop tenemos un sitio web: www.punkymo.com también configurado con Nginx.

To be continued ...

