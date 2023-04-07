# Plex

Plex es un servicio de multimedia que permite organizar y transmitir contenido multimedia a través de diferentes dispositivos y plataformas. Facilita la organización de videos, música y fotos utilizando una biblioteca de medios personales que centraliza los contenidos y los reproduce desde cualquier dispositivo con una conexión a internet.

Adicionalmente:

* utiliza tecnología de trans-codificación que garantiza que el contenido se reproduzca sin correctamente en cualquier dispositivo, con independencia del formato o la resolución original.
* incluye características como la capacidad de agregar subtítulos, integración con servicios de transmisión de contenido en línea y la sincronización de contenido para su acceso sin conexión.

## Instalando Plex

Archivo de docker-compose:

```
---
version: "2.1"
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - VERSION=docker
      - PLEX_CLAIM= #optional
    volumes:
      - /path/to/library:/config
      - /path/to/tvseries:/tv
      - /path/to/movies:/movies
    restart: unless-stopped
```

**PLEX\_CLAM** - Se puede obtener un token de [https://plex.tv/claim](https://plex.tv/claim) e ingresarlo aquí. Hay que tener en cuenta que estos tokens caducan en 4 minutos.

Una vez que esté instalado puedes acceder a través de:

<pre><code><strong>http://IP:32400/web
</strong></code></pre>

Esto te permite acceder a una pantalla  de inicio de sesión de Plex.  Ingresa tus credenciales o regístrate.&#x20;

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Plex en Ubuntu local</p></figcaption></figure>

Una vez que te registras deberías obtener esta pantalla como ésta:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Plex en Ubuntu Server - docker</p></figcaption></figure>

Una vez que clicas en Entendido, verás una ventana emergente que te ofrece comprar Plex Pass con características exclusivas. Omite este paso dado que no es necesario para comenzar a usar Plex Media Server.&#x20;

* Ingreseamos un nombre para el servidor de medios.&#x20;
* Asegúrate de seleccionar `Permitirme acceder a mis medios fuera de mi casa`. dado que es  es necesario para poder acceder al contenido desde fuera del hogar.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Ahora podemos organizar la biblioteca de medios:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Una vez terminada la configuración, en mi caso, seleccioné Netflix que es lo que tengo...

## Links

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Plex - Netflix</p></figcaption></figure>





* [https://docs.linuxserver.io/images/docker-plex](https://docs.linuxserver.io/images/docker-plex)&#x20;
* [https://medium.com/@faisalmhashem/plex-media-server-on-docker-ubuntu-a3f3753c7990](https://medium.com/@faisalmhashem/plex-media-server-on-docker-ubuntu-a3f3753c7990)&#x20;



&#x20;
