# 🚧 FFMPEG + YT-DLP

## ¿Qué es FFMPEG?

Es una colección de software libre que puede grabar, convertir (transcodificar) y hacer streaming de audio y vídeo. Es capaz de codificar, decodificar, transcodificar, multiplexar, demultiplexar, transmitir, filtrar y reproducir casi cualquier cosa en términos de audio y video. Es compatible con formatos antiguos  y modernos.

Está desarrollado en GNU/Linux, pero puede ser compilado en la mayoría de los sistemas operativos, incluyendo Windows. La mayoría de los desarrolladores de FFmpeg lo son también del proyecto MPlayer y está hospedado en el servidor del proyecto MPlayer. Incluye la biblioteca de códecs libavcodec.

Es una herramienta de línea de comandos que permite convertir audio o video de un formato a otro.\
Puede capturar y codificar en tiempo real desde DirectShow, una tarjeta de televisión u otro dispositivo compatible.\
Es utilizado en proyectos libres y propietarios, como ffmpeg2theora, VLC, MPlayer, HandBrake, Blender, Google Chrome, MystiQ, Videomorph, etc.

## Instalar

Para instalar la herramienta:

```
sudo apt update && sudo apt upgrade

sudo apt-get install ffmpeg

```

## Algunos comandos básicos

Para pedir ayuda y ver las opciones que nos brinda.

```
ffmpeg -h
```

Para ver información de un video

```
ffmpeg -i colplay.mp4
```

## YT-DLP

YT-DLP es un fork de youtube-dl, que permite la descarga de contenido multimedia desde diversas fuentes en línea de un modo sencillo y eficiente.

\
Para instalar esta herramienta que funciona también por línea de comandos, debemos descargar la aplicación desde github y lo ubicaremos en `/usr/local/bin/` haciéndolo accesible desde cualquier directorio del sistema.

```
sudo wget https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -O /usr/local/bin/yt-dlp
```

Para hacer que yt-dlp sea ejecutable, tenemos que darle permisos de ejecución:

```
sudo chmod a+rx /usr/local/bin/yt-dlp
```

Si queremos ayuda:

```
sudo yt-dlp -h
```

Si queremos ver los formatos disponibles para el vídeo podemos usar la opción «-F»

```
sudo yt-dlp -F 
https://www.youtube.com/watch?v=zrnCBt2q-dY
```

Para descargar uno de los formatos posibles:

```
sudo yt-dlp -f ID 
https://www.youtube.com/watch?v=zrnCBt2q-dY
```

Si quieres descargar el video con la mejor resolución:

```
yt-dlp -f bv*+ba https://www.youtube.com/watch?v=bH3NFlkui4Y
```

### Otros comandos:

* <mark style="color:purple;">yt-dlp URL</mark> # Descarga el vídeo de la URL
* <mark style="color:purple;">yt-dlp -F URL</mark> # Muestra todos los formatos disponibles para la URL especificada.
* <mark style="color:purple;">yt-dlp -f “bestvideo+bestaudio”</mark> # Descarga el audio y el video de mejor calidad y los fusiona.
* <mark style="color:purple;">yt-dlp -o “%(titulo)s.%(ext)s”</mark> # Para descargar el vídeo con un nombre y extensión específicos.
* <mark style="color:purple;">yt-dlp -F ‘BV\*\[height=1080]+BA’ URL</mark> # Para descargar archivo con una calidad específica.

\
\






\
\
\
\
\




## Links

* [https://ffmpeg.org](https://ffmpeg.org)

\
