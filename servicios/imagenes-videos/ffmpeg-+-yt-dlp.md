# FFMPEG + YT-DLP

## ¿Qué es FFMPEG?

Es una herramienta por línea de comandos que permite convertir audio o video de un formato a otro.\
Es capaz de capturar y codificar en tiempo real desde DirectShow, una tarjeta de televisión u otro dispositivo compatible.

Se trata de una colección de software libre que puede grabar, convertir o transcodificar y hacer streaming de audio y vídeo. Puede codificar, decodificar, transcodificar, multiplexar, demultiplexar, transmitir, filtrar y reproducir casi cualquier cosa en términos de audio y video. Es compatible con formatos antiguos  y modernos.

Está desarrollado en GNU/Linux, pero puede ser compilado en la mayoría de los sistemas operativos, incluyendo Windows. La mayoría de los desarrolladores de FFmpeg lo son también del proyecto MPlayer y está hospedado en el servidor del proyecto MPlayer. Incluye la biblioteca de códecs libavcodec.

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
Se queremos convertir  un video .mp4 en .mkv pero con las opciones siguentes:

```
ffmpeg -i video_original.mp4 -vcodec libx264 video_264.mkv
ffmpeg -i video_original.mp4 -vcodec libx265 video_265.mkv
```

donde:\
h264 - codec de video que usa la librería libx264\
h265 - codec de video que usa la librería libx265



Para recortar un fragmento de tiempo de un video más largo. Digamos, obtener el fragmento de video desde los 35seg hasta los 65seg (30seg de duración). Existe un parámetro con el que podemos realizar estas operaciones:

```
$ ffmpeg -i coldplay.mp4 -ss 35 -t 30 coldplay_frag.mp4
$ ffmpeg -i coldplay.mp4 -ss 00:35 -to 01:05 coldplay_frag.mp4
```



## Aportaciones de estudiantes

A continuación os dejo algunas aportaciones realizadas por los estudiantes del grupo de ASIX2º del curso 2024/2025.

<details>

<summary>Subtítulos y playlist entre otros - Asmae El Haouari y Marcos Blasco</summary>

Selección de calidad específica:

```
-f bestvideo+bestaudio)
```

​ Descarga de subtítulos:

```
--write-subs
```

Descarga de playlists completas:

```
--yes-playlist
```

Si FFmpeg nos baja demasiado la calidad o hace que el archivo pese mucho, podemos controlar esto\
con el bitrate:

```
ffmpeg -i coldplay.avi -b:v 2500k -b:a 192k coldplay.mp4
```

donde:

* -b:v 2500k → Fija el bitrate de video en 2500 kbps (mejor calidad, pero más peso)
* ​-b:a 192k → Fija el bitrate de audio en 192 kbps (buena calidad sin ocupar\
  demasiado).<br>

Nota: Si el bitrate es muy bajo, el video se verá pixelado. Si es muy alto, ocupará demasiado espacio.

Si queremos **extraer una imagen por segundo**, usamos:

```
ffmpeg -i video.mp4 -vf "fps=1" fotograma_%04d.png
```

Opciones clave:

Para extraer **una imagen por segundo** utilizamos:\
&#x20;<mark style="color:purple;">fps=1</mark>&#x20;

Si queremos **2 imágenes por segundo**, usamos\
<mark style="color:purple;">fps=2</mark>

Para **numerar** las imágenes en formato 0001, 0002, etc.\
&#x20;<mark style="color:purple;">%04d</mark>&#x20;

Si necesitamos **un fotograma en un tiempo específico**, lo hacemos así:

```
ffmpeg -i video.mp4 -ss 00:01:30 -vframes 1 fotograma.png
```

Explicación rápida:\
●​ -ss 00:01:30 → Saca el fotograma en el minuto 1:30.​\
●​ -vframes 1 → Solo extrae una imagen.​

Si queremos **una imagen cada 60 segundos**, usamos:

```
ffmpeg -i video.mp4 -vf "fps=1/60" fotograma_%04d.png
```

Nota: Esto es útil para obtener capturas periódicas sin saturar de imágenes.



Para **extraer imágenes entre el minuto 2 y el 5, sacando 1 imagen por segundo**, hacemos:

```
ffmpeg -i video.mp4 -ss 00:02:00 -to 00:05:00 -vf "fps=1" fotograma_%04d.png
```

\
Nota: Útil si solo nos interesa una parte del video.



Si en **lugar de PNG queremos JPG**, solo cambiamos la extensión:

```
ffmpeg -i video.mp4 -vf "fps=1" fotograma_%04d.jpg
```

Nota: También podemos usar formatos como BMP, TIFF, etc.



## Links

* RTMP
  * Adobe RTMP Specification: https://www.adobe.com/devnet/rtmp.html
  * Wowza RTMP vs HLS: https://www.wowza.com/blog/hls-vs-rtmp
* HLS\
  ○​ Apple HLS Docs: https://developer.apple.com/streaming/\
  ○​ Cloudflare HLS Guide: https://www.cloudflare.com/learning/video/what-is-http-live-streaming/3.​
* RTSP\
  ○​ RTSP RFC: https://tools.ietf.org/html/rfc2326
* &#x20;SRT\
  ○​ SRT Alliance: https://www.srtalliance.org/\
  FFmpeg
  * &#x20;FFmpeg Wiki: https://trac.ffmpeg.org/wiki
* yt-dlp
  * &#x20;GitHub Repository: https://github.com/yt-dlp/yt-dlp
  * Installation Guide: https://github.com/yt-dlp/yt-dlp/wiki/Installation
* Codecs Comparativos
  * &#x20;Xiph.org (VP9/Opus): https://xiph.org/
  * AOMedia (AV1): https://aomedia.org/

</details>

<details>

<summary>Los básicos imprescindibles - Marc Gea, Miquel Burguera, David Valverde</summary>

Conversión básica de formatos

```
ffmpeg -i entrada.mp4 salida.avi
```

Extraer audio de un video

```
ffmpeg -i video.mp4 -vn -c:a copy audio.mp3
```

Extraer video sin audio

```
ffmpeg -i video.mp4 -an video_sin_audio.mp4
```

Recortar un fragmento de video

```
ffmpeg -i video.mp4 -ss 00:00:10 -to 00:00:20 -c copy recorte.mp4
```

Cambiar resolución (ej: a 720p)

```
ffmpeg -i video.mp4 -vf "scale=1280:720" video_720p.mp4
```

Cambiar bitrate (calidad)

```
ffmpeg -i video.mp4 -b:v 1M -b:a 128k video_compressed.mp4
```

Unir múltiples videos (usando lista.txt)

```
ffmpeg -f concat -i lista.txt -c copy salida.mp4
```

Mezclar audio y video

```
ffmpeg -i video_sin_audio.mp4 -i musica.mp3 -c:v copy -c:a aac final.mp4
```

Además tenemos:

* Grabar pantalla en Linux (X11)\
  ffmpeg -f x11grab -i :0.0 -f alsa -i default grabacion.mp4
* Capturar webcam (v4l2)\
  ffmpeg -f v4l2 -i /dev/video0 webcam.mp4
* Convertir video a GIF\
  ffmpeg -i video.mp4 -vf "fps=10,scale=640:-1" animacion.gif
* Aplicar filtros (ej: rotar, espejo, desenfoque)\
  ffmpeg -i video.mp4 -vf "hflip,vflip,boxblur=5" video\_editado.mp4
* Añadir logo o imagen superpuesta\
  ffmpeg -i video.mp4 -i logo.png -filter\_complex "overlay=10:10"\
  video\_con\_logo.mp4
* Streaming a RTMP (Twitch/YouTube)\
  ffmpeg -i entrada.mp4 -c:v libx264 -preset fast -f flv\
  rtmp://live.twitch.tv/app/STREAM\_KEY
* Comando completo (recortar, escalar y comprimir)\
  ffmpeg -i entrada.mov -ss 00:01:30 -to 00:02:30 -vf "scale=1280:720" -c:v libx264\
  -crf 23 -c:a aac -b:a 128k salida.mp4

</details>

<details>

<summary>Seguridad y metadatos - Gerard Soteras, Xavier Conde, Timofey Kalugin </summary>

Sentencia enfocada a la ciberseguridad, con el atributo `–xff` podemos\
hacer creer que estamos descargando el video desde otro país.

Con `–embed-metadata` podemos descargarnos todos los metadatos del video:

```
sudo yt-dlp --xff US --embed-metadata
```

Un comando bastante interesante que permite crear una versión del video únicamente con el\
contenido visual, sin sonido. Esto puede ser útil durante el montaje si se desea usar el\
video como fondo, mientras el contenido principal es una narración, como en los videos\
de historias de vida, un formato que fue bastante popular en YouTube hace algunos años.

```
ffmpeg -i video.mp4 -an mute_video.mp4

```

</details>

<details>

<summary>Marca de agua y subtítulos - Adrià Trillo, Beatriz Suárez</summary>

Vamos a descargar un vídeo sobre el que vamos a trabajar toda la primera parte. Para ello, hemos listado los formatos disponibles para el vídeo de la canción de River Flows in you:

<mark style="color:purple;">sudo yt-dlp -F https://www.youtube.com/watch?v=7maJOI3QMu0\&ab\_channel=YirumaVEVO</mark>

Ahora, vamos a descargar el vídeo en el formato que queramos. En nuestro caso, como es una canción, voy a descargar sólo el audio en webm. ya que es una opción con 155k bitrate, opus codec, que, entre todas las opciones disponibles parece ser la mejor opción en términos de calidad de audio, ya que tiene el bitrate más alto y un codec eficiente como opus, que es conocido por su buena calidad a tasas de bits moderadas.

<figure><img src="../../.gitbook/assets/image (403).png" alt=""><figcaption></figcaption></figure>

Ahora, vamos a descargar el vídeo en el formato que queramos. En nuestro caso, como es una canción, voy a descargar sólo el audio en webm. ya que es una opción con 155k bitrate, opus codec, que, entre todas las opciones disponibles parece ser la mejor opción en términos de calidad de audio, ya que tiene el bitrate más alto y un codec eficiente como opus, que es conocido por su buena calidad a tasas de bits moderadas.

<mark style="color:purple;">yt-dlp -f 251</mark>\ <mark style="color:purple;">https://www.youtube.com/watch?v=7maJOI3QMu0\&pp=ygURcml2ZXIgZmxvdyB</mark>\ <mark style="color:purple;">pbiB5b3U%3D</mark>

<figure><img src="../../.gitbook/assets/image (404).png" alt=""><figcaption><p>Descargando el video</p></figcaption></figure>

Para poder trabajar con el resto de los comandos, vamos a descargar el vídeo completo, tomaremos la opción 231.\
Esta opción tiene una resolución de 640x480, que es bastante buena para ver detalles, y la tasa de bits es relativamente alta, lo que implica buena calidad. El tamaño es más grande, pero si la calidad es lo que más te importa, esta opción es la mejor.<br>

<figure><img src="../../.gitbook/assets/image (405).png" alt=""><figcaption></figcaption></figure>

Por otra parte, yt-dlp nos ofrece la opción de descargarnos el mismo vídeo, pero con una mejor resolución, esto se puede hacer mediante el comando:

<mark style="color:purple;">yt-dlp -f bv\*+ba</mark>\ <mark style="color:purple;">https://www.youtube.com/watch?v=7maJOI3QMu0\&pp=ygURcml2ZXIgZmxvdyB</mark>\ <mark style="color:purple;">pbiB5b3U%3D</mark>

<figure><img src="../../.gitbook/assets/image (406).png" alt=""><figcaption></figcaption></figure>

También, tenemos la opción de convertir formatos de vídeo en otro que queramos. Como ejemplo. convertiremos el vídeo que nos hemos descargado antes que está en el formato .mp4 al .mkv .

<figure><img src="../../.gitbook/assets/image (407).png" alt=""><figcaption></figcaption></figure>

### Descargar subtítulos

Para descargar los subtítulos, primero tenemos que mirar si estos están disponibles. Para ello, los listamos de la siguiente forma:

<mark style="color:purple;">yt-dlp --list-subs https://www.youtube.com/watch?v=\_KztNIg4cvE</mark>

<figure><img src="../../.gitbook/assets/image (408).png" alt=""><figcaption></figcaption></figure>

Una vez sabemos todos los subtítulos que nos podemos descargar, descargamos el vídeo junto a los subtítulos:<br>

<mark style="color:purple;">yt-dlp --write-sub --sub-lang es-EkcP5AbUQBc --convert-subs srt -f</mark>\ <mark style="color:purple;">bestvideo+bestaudio https://www.youtube.com/watch?v=\_KztNIg4cvE</mark>

<figure><img src="../../.gitbook/assets/image (409).png" alt=""><figcaption></figcaption></figure>

donde:

•--write-subs: Indica que se descarguen los subtítulos.\
•--sub-lang es: Especifica que se descarguen subtítulos en español.\
•--convert-subs srt: Convierte los subtítulos a formato .srt.\
•-f bestvideo+bestaudio: Descarga la mejor calidad de video y audio.\
Si listamos los archivos, veremos que tenemos el vídeo en -webm y otro archivo con la extensión .srt . En este archivo se guardan los subtítulos.

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

Ahora, al reproducirlo tendremos que agregar los subtitulos descargados al vídeo y ya lo tendremos.

<figure><img src="../../.gitbook/assets/image (411).png" alt=""><figcaption></figcaption></figure>



### Añadir marca de agua con FFMPEG

Para añadir una marca de agua con ffmpeg, tenemos que seguir la siguiente sintaxis:

\
<mark style="color:purple;">ffmpeg -i video.mp4 -i marca\_de\_agua.png -filter\_complex "overlay=W-w-10:H-h-</mark>\ <mark style="color:purple;">10" -codec:a copy video\_con\_marca.mp4</mark><br>

Necesitamos una imagen en .png para poner de fondo y tendremos que indicarle la posición.\
\
<mark style="color:purple;">-filter\_complex "overlay=W-w-10:H-h-10"</mark> → Aplica un filtro para superponer la imagen\
sobre el video:

<mark style="color:purple;">overlay=W-w-10:H-h-10</mark> → Ubica la marca de agua en la esquina inferior derecha con\
un margen de 10 píxeles.\
\
<mark style="color:purple;">oW-w-10</mark> → La posición X: coloca la imagen 10 píxeles antes del borde derecho.

\
<mark style="color:purple;">oH-h-10</mark> → La posición Y: coloca la imagen 10 píxeles antes del borde inferior.

\
<mark style="color:purple;">-codec:a copy</mark> → Copia el audio sin re-codificarlo (mantiene la calidad original).

\
**Comandos para posicionar bien la marca de agua**:\
•Superior izquierda: overlay=10:10\
•Superior derecha: overlay=W-w-10:10\
•Inferior izquierda: overlay=10:H-h-10\
•Inferior derecha: overlay=W-w-10:H-h-10

Y si queremos redimensionar marca de agua:

\
<mark style="color:purple;">ffmpeg -i video.mp4 -i marca\_de\_agua.png -filter\_complex "\[1]\[0]scale=iw\*0.1:-</mark>\ <mark style="color:purple;">1\[wm];\[0]\[wm]overlay=W-w-10:H-h-10" -codec:a copy</mark>\ <mark style="color:purple;">video\_con\_marca\_redimensionada.mp4</mark>

<figure><img src="../../.gitbook/assets/image (412).png" alt=""><figcaption><p>La marca de agua de Amapola</p></figcaption></figure>



</details>

## Otras aportaciones&#x20;

Un documento sobre el protocolo RTMP, Docker y OBS:

{% file src="../../.gitbook/assets/M08UF4A2-RTMP_Leonardo+Duarte,+Joel+Diaz,+Marc+Mountoto,+Beatriz+Suarez,+Adrià+Trillo,+Nicolas+Guerra.pdf" %}

## Links

* [https://ffmpeg.org](https://ffmpeg.org)
* https://www.rapidseedbox.com/es/blog/yt-dlp-complete-guide
* [https://multimedia.easeus.com/es/video-download/como-utilizar-yt-dlp.html](https://multimedia.easeus.com/es/video-download/como-utilizar-yt-dlp.html)
* https://github.com/yt-dlp/yt-dlp-wiki/blob/master/Installation.md
* [https://terminaldelinux.com/terminal/multimedia/ffmpeg/](https://terminaldelinux.com/terminal/multimedia/ffmpeg/) \*\*\*



<br>
