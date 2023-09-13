# Plex

**Plex** es un servicio de multimedia que permite organizar y transmitir contenido multimedia a través de diferentes dispositivos y plataformas. Facilita la organización de videos, música y fotos utilizando una biblioteca de medios personales que centraliza los contenidos y los reproduce desde cualquier dispositivo con una conexión a internet.

Inicialmente fue un proyecto sin ánimo de lucro pero desde 2010 evolucionó hasta convertirse en un software comercial  que  es desarrollado y es propiedad de una solo startup: **Plex, Inc**.

En el 2013, se lanzó [Plex Home Theater ](https://www.plex.tv/es/blog/plex-home-theater-1-0-released/)o [Plex Media Center](https://www.plex.tv/es/blog/plex-home-theater-1-0-released/) que es de código cerrado. Pero el código fuente de Plex se inició en mayo de 2008 como una bifurcación de [Kodi](https://kodi.tv) o [XBMC ](https://github.com/xbmc/xbmc)o [Xbox Media Center](https://github.com/xbmc/xbmc), como era antes conocido.&#x20;

**Plex Media Server**:

* Es el motor del servidor de media.
* Es compatible con un amplio rango de formatos multimedia
* Permite listas de reproducción, visualizaciones de audio y plugin de terceros.&#x20;
* Puede reproducir la mayor parte de los ficheros de audio y video, al igual que visualizar imágenes de diferentes fuentes como disco duro, CD/DVD-ROM, USB flash, Internet y redes LAN ([SMB/SAMBA/CIFS](https://es.wikipedia.org/wiki/Server\_Message\_Block) shares (Windows File-Sharing) o realizar streaming sobre [UPnP (Universal Plug and Play)](https://es.wikipedia.org/wiki/Universal\_Plug\_and\_Play) y [DLNA](https://es.wikipedia.org/wiki/Digital\_Living\_Network\_Alliance) y media servers.&#x20;
* Tiene listas de reproducción de música y video, así como una función de [karaoke](https://es.wikipedia.org/wiki/Karaoke) y gran variedad de audio visualizers y [screensavers](https://es.wikipedia.org/wiki/Screensaver).
* Incluye características como la capacidad de agregar subtítulos, integración con servicios de transmisión de contenido en línea y la sincronización de contenido para su acceso sin conexión.
* Alberga los contenidos y los plugins que son enviados por streaming al media player:  Plex Home Theater y las apps móviles de Plex tanto si se encuentran en el mismo equipo, en la misma red local como en Internet.
* **Plex Online** da acceso a un listado de plugin que permite disfrutar contenido en línea como [Netflix ](https://www.netflix.com/es/login?nextpage=https%3A%2F%2Fwww.netflix.com%2Fbrowse)y [Hulu](https://www.hulu.com/welcome?orig\_referrer=https%3A%2F%2Fwww.google.com%2F).
* Puede ser configurado para clasificar contenido en cualquier ruta del equipo en que se ejecuta, además de poder obtener el contenido de **iTunes**, **iPhoto**, y **Aperture** de forma automática.&#x20;
* Antes de que el servidor envíe el contenido vía [streaming](https://es.wikipedia.org/wiki/Streaming), éste puede ser [transcodificado](https://es.wikipedia.org/wiki/Transcodificar) (conversión directa del tipo de señal de digital a digital entre [codecs](https://es.wikipedia.org/wiki/C%C3%B3dec)) por el servidor para reducir banda ancha, o por brindar compatibilidad con el dispositivo al que se envía el contenido.  En resumen, garantiza que el contenido se reproduzca correctamente en cualquier dispositivo, con independencia del formato o la resolución original.
* Utiliza los metadatos de librerías de código abierto  de manera automática para encontrar información de los elementos de la librería.
* Puede decodificar video de alta definición Full HD, FHD o [1080p](https://es.wikipedia.org/wiki/1080p), a
* Con el hardware apropiado, es capaz de soportar decodificación por hardware de video a H.264 o MPEG-4 (norma que define un [códec de vídeo](https://es.wikipedia.org/wiki/C%C3%B3dec\_de\_v%C3%ADdeo) de alta compresión​)
* Está diseñado para tomar ventaja de una conexión de internet si  está disponible, y usa por defecto:&#x20;
  * [TheMovieDB](https://www.themoviedb.org) para obtener los [thumbnails](https://es.wikipedia.org/wiki/Thumbnail) y las sinopsis de películas.
  * [TheTVDB ](https://thetvdb.com)para las previsualizaciones de [shows de tv](https://es.wikipedia.org/wiki/Programa\_\(difusi%C3%B3n\))&#x20;
  * [Metadata](https://es.wikipedia.org/wiki/Metadata), [CDDB](https://es.wikipedia.org/wiki/CDDB) (vía [FreeDB](https://es.wikipedia.org/wiki/FreeDB)) para información de CD de audio.
  * [AMG](https://es.wikipedia.org/wiki/Allmusic) para imágenes de las portadas de álbumes.&#x20;

## Instalando Plex

La guía realmente útil la podemos encontrar en [linuxserver.io](https://docs.linuxserver.io/images/docker-plex-meta-manager) donde encontraremos un montón de aplicaciones a instalar. Podemos crear un contenedor a partir de docker-compose o docker cli.

Para hacer esta instalación he utilizado:&#x20;

* VM de Ubuntu Server jammy 22.04.2 LTS&#x20;
* Modo adaptador puente
* Docker
* Portainer 2.9.3

Siguiendo las recomendaciones de [linuxserver.io](https://docs.linuxserver.io/images/docker-plex-meta-manager) hago la instalación a través del archivo de docker-compose que presento a continuación:

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

<figure><img src="../.gitbook/assets/image (5) (2).png" alt=""><figcaption><p>Plex en Ubuntu local</p></figcaption></figure>

Una vez que te registras deberías obtener esta pantalla como ésta:

<figure><img src="../.gitbook/assets/image (2) (6).png" alt=""><figcaption><p>Plex en Ubuntu Server - docker</p></figcaption></figure>

Una vez que clicas en <mark style="color:blue;">`Entendido`</mark>, verás una ventana emergente que te ofrece comprar `Plex Pass` con características exclusivas. Omite este paso dado que no es necesario para comenzar a usar `Plex Media Server`.  A continuación:&#x20;

* Asignamos un nombre para el servidor de medios.&#x20;
* Nos aseguramos de seleccionar `Permitirme acceder a mis medios fuera de mi casa`. dado que es  es necesario para poder acceder al contenido desde fuera del hogar.

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

Una vez hecho esto, podemos organizar la biblioteca de medios:

<figure><img src="../.gitbook/assets/image (9) (6).png" alt=""><figcaption></figcaption></figure>

Una vez terminada esa parte de la configuración, en mi caso, seleccioné Netflix que es lo que tengo...

## Links

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Plex - Netflix</p></figcaption></figure>



## Algunos extras

### ¿Qué se entiende por compresión de videos?

Para comprimir videos es necesario eliminar aquellas imágenes y sonidos repetitivos determinados por el algoritmo del códec que comprime. Esta pérdida de imágenes y sonidos pasa prácticamente inadvertida al ojo humano.&#x20;

Está claro que los problemas surgen si quieres reducir un archivo a un tamaño pequeño conservando la misma resolución. En este caso es que vemos las cosas pixeladas, granuladas.

### Comprensión de videos

La mayoría de los tipos de compresiones de vídeo son con pérdida, esto es, se realizan partiendo de la base de que muchos de los datos no son necesarios para percibir una buena calidad de vídeo. La compresión no deja de ser una compensación entre almacenamiento, la calidad del vídeo y el costo del hardware requerido para descomprimir el vídeo en un tiempo razonable.&#x20;

Por ejemplo, un DVD utiliza un tipo de codificación estándar MPEG-2 capaz de comprimir entre 15 a 30 veces los datos de un vídeo manteniendo una calidad de imagen alta calidad.&#x20;

### ¿Cómo se hace?

La compresión de vídeo opera típicamente entre grupos de píxeles vecinos, conocidos o macro-bloques. Estos bloques de píxeles son comparados con el fotograma siguiente y el códec de compresión de vídeo envía solo las diferencias de esos bloques. Una vez la pérdida en la compresión del vídeo compromete la calidad de imagen, es imposible recuperar la imagen a su calidad original.

### ¿Qué es un códec?

Son los encargados de comprimir los videos, reduciendo el tamaño del archivo para el almacenamiento y luego descomprimirlos nuevamente para verlo.&#x20;

Por lo general, los códecs funcionan automáticamente dentro del programa y sin éstos, el archivo tendría un tamaño entre 3-5 veces mayor. Por ejemplo, [Handbrake](https://handbrake.es) es un software de transcodificación que permite convertir el códec de un video.

**Tipos de códec:**

* **MPEG**: uno de los más usados y que generan una reproducción de video de alta calidad, incluso después de la compresión.&#x20;
* **WMV**: el software de edición de video en Windows.
* **ProRes**: un códec que se usa en softwares como iMovie de Apple, proporcionando una excelente resolución de color.

**Tipos de video -** afectan el tamaño y la calidad, siendo éstos:

* **MP4** - es un tipo común de formato de archivo de vídeo. Puede reproducirse en la mayoría de los dispositivos. Utiliza el algoritmo de codificación MPEG-4 para almacenar archivos de vídeo, audio y texto, pero ofrece una definición inferior a la de los demás. Funciona bien para vídeos de YouTube, Facebook, Twitter e Instagram.
*   **AVI o Audio Video Interleave** - funciona bien con casi todos los navegadores web en Windows, Mac y Linux. Fue desarrollado por Microsoft y ofrece alta calidad, pero con grandes tamaños de archivo. Es compatible con YouTube y funciona bien para ser visualizado en TV.

    &#x20;
* **FLV, F4V y SWF (Shockwave Flash)** -  fueron diseñados para Flash Player, pero se usan para transmitir vídeos en YouTube. Flash no es compatible con dispositivos iOS.
* **MOV o QuickTime Movie** - fue desarrollado para el reproductor QuickTime por Apple y usan codificación MPEG-4 para reproducir en QuickTime para Windows. Almacenan vídeo, audio y efectos de alta calidad, pero los archivos suelen ser bastante grandes. Este formato es compatible con Facebook y YouTube, y también funcionan bien para su visualización en TV.
* **WMV o Windows Media Viewer** - ofrecen buena calidad de vídeo pero grandes tamaños de archivo al igual que MOV. Microsoft lo desarrolló para su reproductor de medios. Es un formato compatible con YouTube y para que un usuario de Apple pueda ver este tipo de vídeos debe descargar el reproductor de medios de Windows para Apple.
* **AVCHD** o **Advanced Video Coding High Definition -** es un formato destinado al vídeo de alta definición. Fue creado para videocámaras Panasonic y Sony y los archivos se comprimen para conseguir un fácil almacenamiento sin perder definición.
* **MKV** o **Matroska Multimedia Container** - fue desarrollado en Rusia y es gratuito, de código abierto y compatible con casi cualquier códec, pero no así con muchos programas. Este  formato es una opción razonable si quieres ver el vídeo en un TV u PC que utilice un reproductor de medios de código abierto como VLC o Miro.
* **WEBM o HTML5** - Buenos formatos para vídeos incrustados en un sitio web. Son archivos pequeños, por lo que se cargan rápidamente y se transmiten con facilidad.

**FPS** - La velocidad de cuadro o cuadros por segundo afecta la calidad del video. Un video de alta calidad utiliza un mínimo de 24 FPS. Cuanto más cuadros tenga, mayor cantidad de detalles y por supuesto, mayor será el archivo.&#x20;

**Resolución de video -** Un video de mayor resolución tiene un tamaño de pantalla más grande y, por lo general, un tamaño de archivo más grande. Ejemplo, si tenemos un video de 480p, quiere decir que está diseñado para ser una pantalla general más pequeña de unos 852x480 píxeles. Un video de 720p entre en la alta calidad de unos 1280x720.&#x20;



## Links

* [https://docs.linuxserver.io/images/docker-plex](https://docs.linuxserver.io/images/docker-plex)&#x20;
* [https://medium.com/@faisalmhashem/plex-media-server-on-docker-ubuntu-a3f3753c7990](https://medium.com/@faisalmhashem/plex-media-server-on-docker-ubuntu-a3f3753c7990)&#x20;
* [https://handbrake.es](https://handbrake.es)



&#x20;
