---
description: Nginx-rtmp  YT-DLP  FFmpeg
---

# Servidor de video

En esta práctica vamos a configurar un servidor de video streaming utilizando para ello  `NGINX-RTMP`. Utilizaremos también una herramienta denominada `YT-DLP` para descargar videos de YouTube. Transmitiremos ese contenido multimedia a través de nuestro servidor RTMP utilizando otra herramienta ya conocida: `FFmpeg`.

Tendremos que tener en cuenta los siguientes aspectos:

1. **NGINX-RTMP**: hará de servidor de streaming de video.
2. **YT-DLP**: Permite la descarga de videos de YouTube.
3. **Transmisión en Tiempo Real:** veremos cómo `FFmpeg` permite tomar los videos descargados y transmitirlos en tiempo real a través del servidor `NGINX-RTMP`.

Podemos trabajar desde una misma VM con Ubuntu Desktop (por ejemplo) aunque lo interesante sería configurar una estructura de red entre dos VM conectadas en red Nat. En la VM que hace de servidor instalamos Nginx-rtmp, yt-dlp y FFmpeg y desde el equipo cliente visualizamos la transmisión del video.

Comenzamos por asegurar el nombre del host. Lo ideal sería disponer de un servidor con nombre de dominio. Para ello hacemos:

```
hostname
hostname set-hostname haven.local
```



## Instalar Nginx-rtmp

```
apt install libnginx-mod-rtmp
```



Configurar nginx

```
sudo nano /etc/nginx/nginx.conf
rtmp{
    server {
        listen 1935;
        chunk_size 4096;
        allow publish 127.0.0.1;
        deny publish all;
        
        application ive {
           live on;
           record off;
        }
    }
}
```



\
