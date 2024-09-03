---
description: En Docker
---

# RDP

### Protocolo de escritorio remoto

### Funcionamiento

### Ventajas y Desventajas

### Seguridad





### Instalando xrdp en docker

Me copié de linuxserver:

```
docker run -d \ 
--name=rdesktop \ 
-e PUID=1000 \ 
-e PGID=1000 \ 
-e TZ=Etc/UTC \ 
-p 3389:3389 \ 
--restart unless-stopped \ 
lscr.io/linuxserver/rdesktop:latest
```

The Default USERNAME and **PASSWORD is: abc/abc**

### Links

* [https://docs.linuxserver.io/images/docker-rdesktop/](https://docs.linuxserver.io/images/docker-rdesktop/)&#x20;
* Si quisiera instalar XRDP en modo standalone: [https://hayhost.am/knowledgebase/349/-How-to-Install-XRDP-Server-Remote-Desktop-on-Debian-11or12.html](https://hayhost.am/knowledgebase/349/-How-to-Install-XRDP-Server-Remote-Desktop-on-Debian-11or12.html)
* [https://www.cloudflare.com/es-es/learning/access-management/what-is-the-remote-desktop-protocol/](https://www.cloudflare.com/es-es/learning/access-management/what-is-the-remote-desktop-protocol/)

