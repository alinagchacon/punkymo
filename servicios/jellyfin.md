# Jellyfin

Es un proyecto que se fundó en el 2018, de software libre, bajo licencia _GPL_ de _GNU como una bifurcación de <mark style="color:blue;">`Emby`</mark>_. En realidad, desciende de la versión 3.5.2 de <mark style="color:blue;">Emby</mark> que se trasladó a  .NET Core para habilitar el soporte multiplataforma.&#x20;

Está soportado por **Debian, Ubuntu**, **Fedora, CentOS**, **Windows** en versiones portátiles y de instalación, y en **Docker**.



​Jellyfin es un sistema de medios que facilita el control, administración y transmisión de medios, como una alternativa a Emby y Plex que son sistemas propietarios. Proporciona medios desde un servidor dedicado a dispositivos de usuarios finales a través de múltiples aplicaciones.&#x20;



## Instalando Jellyfin

La guía realmente útil la podemos encontrar en [linuxserver.io](https://docs.linuxserver.io/images/docker-jellyfin) donde encontraremos un montón de aplicaciones a instalar. Podemos crear un contenedor a partir de docker-compose o docker cli.

Para hacer esta instalación he utilizado:&#x20;

* VM de Ubuntu Server jammy 22.04.2 LTS&#x20;
* Modo adaptador puente
* Docker
* Portainer 2.9.3

Desde el navegador, nos conectamos a nuestra VM. En mi caso sería: [https://192.168.1.123:9443](https://192.168.1.123:9443/#!/2/docker/stacks)

Ya dentro de Portainer, vamos a Stacks para crear uno nuevo:&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Siguiendo las recomendaciones de [linuxserver.io](https://docs.linuxserver.io/images/docker-jellyfin), crearé el contenedor a partir del `docker-compose` que nos proporciona:

```
---
version: "2.1"
services:
  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - JELLYFIN_PublishedServerUrl=192.168.0.5 #optional
    volumes:
      - /path/to/library:/config
      - /path/to/tvseries:/data/tvshows
      - /path/to/movies:/data/movies
    ports:
      - 8096:8096
      - 8920:8920 #optional
      - 7359:7359/udp #optional
      - 1900:1900/udp #optional
    restart: unless-stopped
```

Copiamos el archivo de docker-compose en el espacio habilitado para ello en Web editor, le damos un nombre a nuestro stack y le damos al botón de "deploy stack" para su implementación o despligue.

<figure><img src="../.gitbook/assets/image (11) (4) (1).png" alt=""><figcaption><p>Copia y pega el docker-compose en este apartado para hacer el despliegue</p></figcaption></figure>

Una vez realizado este paso, podemos acceder al apartado de Container y lo vemos ahí:

<figure><img src="../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption><p>El contenedor de Jellyfin </p></figcaption></figure>

Para acceder bastaría ir al navegador y escribir la IP de nuestra VM. En mi caso sería:  <mark style="color:blue;">https://192.168.1.123:8096</mark> y ya estaríamos en jellyfin, donde tendríamos que seleccionar el idioma y proporcionar el nombre y contraseña del administrador del servicio:&#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption><p>BIenvenidos a Jellyfin</p></figcaption></figure>

Llegados a este punto podemos configurar nuestra biblioteca de medios, el tipo de medio que queremos y la carpeta donde se almacenará.&#x20;

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>Configuración básica de Jellyfin</p></figcaption></figure>



## Plugins

Lo ideal, al menos para el usuario novel,  es usar el repositorio oficial de plugins de Jellyfin. Esto permite  instalar fácilmente el plugin Jellyfin para Kodi, así como mantenerlo actualizado con la última versión.&#x20;

**Kodi Sync Queue** - Es un plugin que mantendrá la biblioteca de medios al día sin esperar una re-sincronización periódica de Kodi.&#x20;

<figure><img src="../.gitbook/assets/image (7) (4).png" alt=""><figcaption><p>Plugin Kodi Sync Queue</p></figcaption></figure>

## Links

* [https://www.jellyfin.eu/instalacion-de-jellyfin/](https://www.jellyfin.eu/instalacion-de-jellyfin/)&#x20;
* [https://docs.linuxserver.io/images/docker-jellyfin](https://docs.linuxserver.io/images/docker-jellyfin)
*
