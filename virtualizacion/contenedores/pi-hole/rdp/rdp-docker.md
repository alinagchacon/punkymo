---
description: En Docker
---

# RDP

El protocolo RDP, de escritorio remoto sirve para conectarse a un PC de escritorio. El protocolo de escritorio remoto o RDP lo lanzó inicialmente Microsoft y se encuentra disponible para la mayoría de los sistemas operativos Windows, así como en los sistemas operativos Mac.

Este protocolo RDP tiene la capacidad de permitir la conexión y utilización de un PC de manera remota desde otro. Un usuario de escritorio remoto pueden acceder al escritorio, así como abrir y editar ficheros, utilizar aplicaciones como si estuvieran realmente frente al equipo. Un modo de verlo es los empleados que suelen utilizar el escritorio remoto para acceder al PC del trabajo cuando están de viaje o trabajan desde casa.

No confundamos el acceso al escritorio remoto con el acceso a la nube porque cierto que ambos permiten que se trabaje a distancia. Sin embargo, en la nube, los usuarios acceden a archivos y aplicaciones almacenados  en servidores en la nube, pero si utilizamos un software de escritorio remoto (RDP), lo que hacemos es acceder de verdad a un PC de escritorio, y solo podemos utilizar los archivos y aplicaciones almacenados en local en ese equipo.



### Funcionamiento del protocolo RDP



### Ventajas y desventajas



### Seguridad





### Instalando XRDP en Docker

Para hacer la instalación nos vamos a [https://hub.docker.com/r/linuxserver/rdesktop](https://hub.docker.com/r/linuxserver/rdesktop) com me descargué la imagen de RDP con: docker pull linuxserver/rdesktop

* Se genera la imagen, bastante grande por cierto porque ocupa 1.5GB y se llama: linuxserver/rdesktop
* Creamos el contenedor pero no entramos, tengo que: docker run --name rdesktop -d -it linuxserver/rdesktop /bin/sh
* Con el comando anterior se crea pero no OK
* Me copié de linuxserver

docker run -d \ --name=rdesktop \ -e PUID=1000 \ -e PGID=1000 \ -e TZ=Etc/UTC \ -p 3389:3389 \ --restart unless-stopped \ [lscr.io/linuxserver/rdesktop:latest](http://lscr.io/linuxserver/rdesktop:latest)

* The Default USERNAME and **PASSWORD is: abc/abc**

### Links

* C
* Si quisiera instalar XRDP en modo standalone: [https://hayhost.am/knowledgebase/349/-How-to-Install-XRDP-Server-Remote-Desktop-on-Debian-11or12.html](https://hayhost.am/knowledgebase/349/-How-to-Install-XRDP-Server-Remote-Desktop-on-Debian-11or12.html)
