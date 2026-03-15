---
description: Apuntes ... breves
---

# 🚧 Imágenes - Vídeos

Este mundo de las imágenes y los vídeos es fascinante. Hace un par de años tuve un colega muy joven, Isaac, que había estudiado Telecos con especialidad en audiovisuales. Algunas cosas me explicó y me quedé con ganas de aprender más. Después tuve dos alumnos a los que le gustaba el tema de la música y por ellos aprendí sobre FFmpeg. Este curso tenemos otro alumno al que le gusta la música y espero que algo más me enseñe.

Comencemos por aclarar que es la codificación.

La codificación es el proceso que toma los datos a transmitir y los convierte en un formato aceptable para la transmisión. Codificar permite convertir los datos a un formato específico para su almacenamiento o transmisión. Este proceso es completamente reversible si conocemos el método utilizado para codificar, por tanto, no confundir con cifrar porque no se trata de un método de seguridad.

Por tanto, la decodificación permite invertir  el proceso de codificación facilitando la interpretación de  la información.&#x20;

Algunos ejemplos de codificación de caracteres, bien conocidos son:  [ASCII, Unicode, UTF-8, Base64](../../miscelaneas/datos-codificacion.md).<br>

Algunos **tipos de codificación** son:

* Binaria: 0/1
* Caracteres: ASCII, Unicode, ISO-8859
* Imágenes: JPEG, PNG, BMP
* Audio: MP3, WAV, AAC
* Video: AVI, MP4, MKV

Cada formato de imágenes, vídeo y audio utiliza diferentes algoritmos de compresión y descompresión.

## Compresión de imágenes

La compresión de imágenes es un proceso que reduce el tamaño de los archivos de imagen. Funciona eliminando bytes de información de la imagen o utilizando un algoritmo de compresión de imágenes reescribiendo el archivo de imagen a fin de que ocupe menos espacio de almacenamiento con mayor o menor pérdida de calidad de la misma.

En sitios web, comprimir una imagen nos asegura que se cargue rápidamente cuando el usuario visualiza el sitio web o aplicación, siendo una parte importante de la optimización de imagen.

### Compresión con pérdida

Este tipo de compresión de imágenes conserva la información más significativa de la misma sin mantener cada píxel. Existen diferentes tipos de algoritmos cuyo principio de funcionamiento consiste en eliminar información del archivo, reduciendo el tamaño del mismo manteniendo una calidad visual "razonable". Por ejemplo: jpeg, png, gif.

#### JPEG

Este  tipo de archivos puede comprimir archivos en una proporción de 10:1 con una reducción mínima de la calidad de imagen, por tanto, minimiza la pérdida de calidad percibida,  aunque se pierde calidad en cada compresión. Se trata de un formato de imagen adecuado para fotografías y gráficos complejos y no es ideal para imágenes con texto o gráficos con bordes nítidos. Permite obtener una elevada compresión y mantener a su vez una buena calidad en la imagen.

Dado que el ojo humano es más sensible a los detalles de brillo que al color, se permite reducir ciertos datos sin que la imagen pierda mucha calidad.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>Tomado de <a href="https://commons.wikimedia.org/wiki/File:JPEG_compression_Example.jpg">https://commons.wikimedia.org/wiki/File:JPEG_compression_Example.jpg</a></p></figcaption></figure>



### Compresión sin pérdida

Entre los formatos que hacen este tipo de compresión encontramos:&#x20;

* **PNG** - comprime la imagen pero la compresión es totalmente reversible al formato original.
* **GIF** - formato de intercambio de gráficos, a menudo utilizado también en la web. Crea una tabla de 256 colores a partir de una tabla de 16 millones, de tal modo que si la imagen contiene menos de 256 colores, puede almacenar la imagen sin pérdida. Si por el contrario, la imagen contiene muchos colores, entonces el software que crea el gif pudiera utilizar un algoritmo que genere los colores más cercanos a los reales dentro de la paleta de 256. En función de la calidad del algoritmo encontrará o no el conjunto de colores óptimos dentro de los 256. También puede ajustar el llamado **error de difusión** que permite reajustar los colores de los píxeles vecinos corrigiendo el error en cada píxel.
* **BMP** - Suelen ser demasiado grandes para uso práctico en la web. Es un formato de almacenamiento sin compresión de imágenes propiedad de Microsoft.
* **RAW** – Este tipo de imágenes no están comprimidas en lo absoluto. Por ejemplo: muchas de las cámaras digitales toman fotos en RAW. No se trata de un método estandarizado con lo cual, cada marca puede tener su propia versión del mismo y nos obliga a usar el software de la cámara para poder visualizar las imágenes.

### Tipos de algoritmos

**Con** **pérdida**:&#x20;

* Codificación por transformación<br>

**Sin pérdida**:

* Run-length encoding (RLE)
* Codificación aritmética
* Codificación Huffman



## Algo sobre vídeos

Un vídeo es una **secuencia rápida de imágenes** llamadas _frames_ o **fotogramas**, con lo cual **no es movimiento continuo.**

**Los fotogramas por segundo o frames per second es lo mismo que FPS e** indican **cuántas imágenes individuales se muestran cada segundo en un vídeo o animación**.

Si un vídeo tiene:

* 24 FPS → se muestran 24 imágenes cada segundo
* 30 FPS → se muestran 30 imágenes cada segundo
* 60 FPS → se muestran 60 imágenes cada segundo

Cuantos más FPS, más fluido se ve el movimiento. Por ejemplo:

* 15 FPS - Movimiento entrecortado
* 24 FPS - Estándar del cine
* 30 FPS - Vídeo TV / streaming
* 60 FPS - Muy fluido (gaming, deportes)
* 120+ FPS - Ultra fluido

{% hint style="info" %}
Debemos tener claro que **bitrade** y los **FPS** no son lo mismo porque:

* Los FPT son el número de imágenes por segundo
* Bitrade es la cantidad de datos por segundo
* La resolución es el tamaño de la imagen. Por ejemplo: 1920x1080
{% endhint %}

### Ejemplo&#x20;

<table><thead><tr><th width="186.5">Resolución</th><th width="185.5">FPS</th><th>Resultado</th></tr></thead><tbody><tr><td>4K</td><td>24 FPS</td><td>se ve muy nítido pero menos fluido</td></tr><tr><td>1080p</td><td>60 FPS</td><td>tiene menos resolución pero es más fluido</td></tr></tbody></table>

### En redes y streaming&#x20;

En streaming: RTMP, HLS, cámaras IP:

* Si los FPS son altos consumen más ancho de banda
* Si los FPS sib bajos es menor el tráfico

El uso típico en estos casos:

* Con 15 FPS  se usa en videovigilancia
* Con 25 - 30 FPS es streaming normal
* Con 60 FPS hablamos de gaming



Cuando ejecutas la herramienta ffprobe:

```bash
ffprobe video.mp4
```

puede aparecer algo como:

```
r_frame_rate=30/1
```

Eso significa:

```
30 FPS
```

### Codificación de vídeo

Los formatos de codificación de vídeo son métodos para optimizar los archivos de vídeo digital para diferentes tipos de plataformas, programas y dispositivos. Cada formato de codificación de vídeo se compone de dos partes principales:\
\- un códec\
\- un contenedor<br>

El <mark style="color:purple;">códec</mark> y el <mark style="color:green;">contenedor</mark> especifican la forma en que se almacena, transmite y visualiza la entrada de vídeo sin comprimir. En la transmisión, es importante que el formato de codificación sea compatible con el mayor número posible de dispositivos, para que el flujo esté disponible para todos los usuarios.

Por tanto, los <mark style="color:purple;">códecs</mark> se refieren a la forma en que se codifican el audio, el video u otros datos, mientras que los <mark style="color:green;">contenedores</mark> se refieren al archivo que contiene audio y video codificados.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Tomado de: <a href="https://www.profesionalreview.com/2023/08/12/codec-multimedia/">https://www.profesionalreview.com/2023/08/12/codec-multimedia/</a></p></figcaption></figure>

## Formatos de video

Para seleccionar el formato de vídeo adecuado necesitamos tener en cuenta tres factores principales:

* Disponibilidad de almacenamiento.
* Calidad de salida de vídeo.
* Compatibilidad con diferentes reproductores o programas de vídeo.

Por ejemplo:

En caso de necesitar subir un vídeo a un sitio web, debemos considerar las limitaciones de ancho de banda y la calidad del vídeo. En este caso, pudiéramos utilizar el formato WebM que es un formato de archivo libre y compatible con Android y con la mayoría de los navegadores web y sitios de transmisión de vídeo HTML5 como YouTube. Además de que permite comprimir archivos de vídeo sin perder apenas calidad de vídeo.

En el caso de querer almacenar vídeos caseros  que no ocupen mucho espacio en disco podemos pensar en formato estilo MP4 porque conserva la calidad del vídeo tras la compresión y es compatible con la mayoría de programas y dispositivos.

### Codificación de vídeos

Se trata del proceso de convertir la entrada de vídeo sin comprimir en una forma que pueda ser almacenada y reproducida por diversos dispositivos. Dicho proceso implica dos subprocesos:

* **Compresión** - implica eliminar datos superfluos, lo que disminuye considerablemente el tamaño de un vídeo para que sea más manejable.
* **Transcodificación** - se trata del proceso de conversión de audio y vídeo de un formato a otro, lo que garantiza que un archivo de vídeo sea compatible con el reproductor de vídeo o la plataforma que se utiliza.

### Compresión de vídeos

Para comprimir videos es necesario eliminar aquellas imágenes y sonidos repetitivos determinados por el algoritmo del codec que comprime. Esta pérdida de imágenes y sonidos pasa prácticamente inadvertida al ojo humano. Está claro que los problemas surgen si quieres reducir un archivo a un tamaño `pequeño` conservando la misma resolución. En este caso es que vemos las imágenes pixeladas, granuladas.

La compresión de vídeo opera típicamente entre grupos de píxeles vecinos, conocidos o macro-bloques. Estos bloques de píxeles son comparados con el fotograma siguiente y el codec de compresión de vídeo envía solo las diferencias de esos bloques. Una vez la pérdida en la compresión del vídeo compromete la calidad de imagen, es imposible recuperar la imagen a su calidad original.

### Transmisión de vídeos

La transmisión de vídeos puede hacerse en directo y en vivo:

* Transmisión en **directo**: La transmisión de vídeo se segmenta, se comprime y se codifica en tiempo real.
* Transmisión en **vivo**: Los dispositivos de los usuarios que reciben la transmisión, descodifican y descomprimen el vídeo segmentado antes de reproducirlo.

### Códecs - codificador / decodificador

Son métodos para comprimir y descomprimir datos para que puedan ser transportados y recibidos fácilmente por diferentes aplicaciones. Se utilizan diferentes códecs para comprimir archivos de audio y vídeo, pero suelen funcionar de la misma manera. Los códecs codifican los archivos usando:

* compresión con pérdidas
* compresión sin pérdidas

#### Compresión con pérdida

Al igual que en el caso de las imágenes, este tipo de compresión simplifica el archivo de vídeo conservando solo las partes esenciales. El vídeo puede parecer pixelado o "borroso".

#### Compresión sin pérdida

Este tipo de compresión preserva la alta calidad del archivo de vídeo original al copiar con exactitud cada dato, haciendo que el tamaño del archivo resultante sea igualmente grande. Se fundamenta en conceptos de la teoría de la información, como la redundancia y entropía de los datos.

#### Contenedores

Un contenedor combina la transmisión de audio y vídeo en un único archivo de vídeo. Esto es:

* Audio codificado (códec de audio)
* Vídeo codificado (códec de vídeo)
* Metadatos<br>

Los <mark style="color:purple;">metadatos</mark> indican al reproductor de vídeo cómo coordinar los códecs de audio y vídeo, así como brindar subtítulos.  Algunos contenedores solo funcionan con un único tipo de códec y reproductor de vídeo, limitando las opciones de reproducción. Otros contenedores son compatibles con muchos tipos de códecs y reproductores de vídeo.

Las extensiones de los archivos de vídeo reciben el nombre de los contenedores que utilizan, en lugar de los códecs de audio y vídeo que contienen. Ejemplo de ello: un archivo de vídeo MP4 es en realidad un contenedor MP4.

#### Tipos más comunes de formatos de contenedores de vídeo

* **MP4** – amplia compatibilidad con dispositivos como PC y teléfonos inteligentes, servicios de transmisión y programas de edición de vídeo. Tiene una calidad de vídeo relativamente alta, aunque la compresión de un archivo puede provocar pérdida de la calidad, el efecto es mínimo.
* **MOV** – creado por Apple, no ofrece tanta compresión como otros formatos. Compatible solo con QuickTime.
* **AVI** – creado por Microsoft, archivos más grandes porque prioriza la calidad del vídeo.
* **FLV** – creado por Adobe Flash. Durante mucho tiempo, estos archivos fueron utilizados por todos los videos insertados en Internet, incluidos los presentes en YouTube y otros sitios de transmisión. Aunque HTML5 es ahora preferido por los sitios.
* **WebM** – desarrollado por Google, subconjunto del formato estándar abierto Matroska Video Container (MKV) y es una alternativa a MP4. De código abierto y fácil de usar.
* **MKV** o Matroska Multimedia Container - fue desarrollado en Rusia y es gratuito, de código abierto y compatible con casi cualquier códec, pero no así con muchos programas. Este formato es una opción razonable si quieres ver el vídeo en un TV u PC que utilice un reproductor de medios de código abierto como VLC o Miro.

Veamos estos formatos de contenedores de video un poco más en detalle.

### MP4 - MPEG-4 Part 14

MP4 es un formato de almacenamiento de archivos multimedia ampliamente utilizado para almacenar vídeo y para transmisiones.

* **Año y origen:** Derivado del formato QuickTime de Apple, estandarizado por ISO/IEC en 2001 como parte del estándar MPEG-4.&#x20;
* El nombre completo del estándar MP4 es MPEG-4 Parte 14, pero los términos "MP4" y "MPEG-4" no significan lo mismo. MP4 es el archivo de contenedor digital y MPEG-4 es el estándar para codificar el contenido de vídeo dentro de los archivos MP4. El contenido de vídeo dentro de un archivo MP4 se codifica utilizando el estándar MPEG-4.
* **Funcionamiento:** Es un contenedor que puede alojar prácticamente cualquier combinación de códecs. Lo más habitual es vídeo H.264 o H.265 con audio AAC. El audio y el vídeo se comprimen de forma independiente, lo que permite optimizar cada stream por separado sin que uno afecte al otro.
* **Códecs más comunes dentro:**
  * Vídeo: H.264, H.265/HEVC, AV1, MPEG-4 Visual
  * Audio: AAC, MP3, AC-3, Opus
* **Puntos fuertes:** Compatibilidad universal, dado que es capaz de funcionar  en Android, iOS, Windows, macOS, Smart TVs, consolas, navegadores web, YouTube, Netflix y prácticamente cualquier plataforma. Es el <mark style="color:purple;">formato de entrega estándar de la industria</mark>.
* **Limitación real:** Al ser un contenedor muy abierto, dos archivos .mp4 pueden ser completamente incompatibles entre sí si usan códecs distintos. No es el formato ideal para edición profesional porque la mayoría de sus códecs son interframe (dependen de frames anteriores), lo que complica los cortes.

{% hint style="info" %}
La calidad  del video depende del códec y el bitrate, no del contenedor. MP4 con H.264 CRF 28 puede dar peor calidad que MKV con H.264 CRF 18.
{% endhint %}

### MOV - QuickTime File Format

MOV es otro tipo de archivo contenedor para vídeos. Apple desarrolló MOV para su uso con Apple QuickTime Player y al igual que los archivos MP4, los archivos MOV están codificados con el códec MPEG-4.

* **Año y origen:** Creado por Apple en 1991 junto con el software QuickTime. Es uno de los formatos de contenedor más antiguos aún en uso activo.
* **Funcionamiento:** Es muy similar a MP4, internamente, dado que MP4 se derivó de MOV. Usa una estructura de "átomos" para organizar los datos. Soporta múltiples pistas de vídeo, audio, subtítulos y metadatos simultáneamente.
* **Códecs más comunes dentro:**
  * Vídeo: ProRes, H.264, H.265, MJPEG, DNxHD
  * Audio: AAC, PCM sin comprimir, AC-3
* **Puntos fuertes:** Es el contenedor nativo del ecosistema Apple. [Final Cut Pro](https://www.apple.com/es/final-cut-pro/), [DaVinci Resolve](https://www.blackmagicdesign.com/es/products/davinciresolve) y [Adobe Premiere](https://www.adobe.com/es/products/premiere.html)  usan habitualmente MOV para proyectos de postproducción, especialmente con ProRes, que ofrece altísima calidad con edición fluida.
* **Limitación real:** Históricamente la compatibilidad fuera de macOS era limitada, pero hoy Windows 10+ lo reproduce de forma nativa y es ampliamente soportado.
* **Uso real hoy:** Cámaras de cine digital (RED, ARRI, Sony Cinema) y drones profesionales graban en MOV con ProRes. Es el formato de trabajo en flujos de postproducción de alto nivel, no para distribución final.

### AVI - Audio Video Interleave

Este tipo de formato permite almacenar un flujo de datos de video y varios flujos de audio de manera simultánea.&#x20;

El formato concreto de estos flujos no es objeto del formato AVI y es interpretado por los llamados códec que no son más que programas externos. Es decir, el audio y el video contenidos en el AVI pueden estar en cualquier formato (AC3/DivX, o MP3/Xvid, entre otros). Por eso se le considera un formato contenedor.

* **Año y origen:** Creado por Microsoft en 1992 como parte de Video for Windows. Fue el formato estándar en Windows durante los años 90 y principios de los 2000.
* **Funcionamiento:** Su estructura "intercalada" o interleave alterna bloques de datos de audio y vídeo a lo largo del archivo, lo que facilita la reproducción lineal pero complica el seeking, búsqueda o salto (hace referencia a la capacidad de un reproductor multimedia para saltar rápidamente a un punto específico de tiempo (minuto/segundo) dentro del vídeo, en lugar de verlo secuencialmente desde el principio). \
  Tiene limitaciones: no soporta subtítulos de forma nativa, no soporta capítulos, no maneja bien el VBR en audio, y tiene un límite teórico de 2 GB en su versión original que se puede superar con la extensión OpenDML.
* **Códecs más comunes dentro:**
  * Vídeo: DivX, XviD, MJPEG, H.264 (poco común)
  * Audio: MP3, AC-3, PCM
* Los códecs que se usaban habitualmente con AVI (DivX, XviD) son menos eficientes que los modernos H.264/H.265. Además, muchos AVI históricos usan audio PCM sin comprimir, que pesa mucho.
* Es considerado un formato legacy y nadie debería crear AVI  hoy en día. Se sigue encontrando en colecciones antiguas de vídeo de los años 90-2000. La mayoría de reproductores modernos lo soportan por compatibilidad hacia atrás.<br>

{% hint style="info" %}
Las señales PCM-Modulación por Código de Pulsos, transmiten una secuencia de muestras de 16 o 24 bits, donde cada bit representa un nivel de amplitud en un punto específico de una muestra de audio. Al reproducir rápidamente estas muestras, la señal PCM crea la ilusión de un flujo de audio continuo y fluido. Ver [aquí](https://areahifi.com/blogs/blog-areahifi/pcm-o-bitstream-tomando-la-decision-correcta-para-tu-configuracion-de-audio?srsltid=AfmBOoqGJR3eRZmVabWcnDomLzQLgsu5-rfDMgQUrXaLhoo2x2u375Wi).
{% endhint %}

### FLV - Flash Video

El formato Flash Video de Adobe Systems se utiliza para incluir vídeos en línea en sitios web como es el caso de YouTube, Vevo y otros.&#x20;

Casi cualquier S.O salvo  iOS, puede leer y visualizar archivos FLV mediante el uso de Adobe Flash Player que viene en complementos para navegadores. Los Android y iPhone pueden reproducir archivos FLV mediante software de código abierto y navegadores específicos. La versión "Jelly Bean" de Android permite a los usuarios utilizar archivos FLV.&#x20;

El audio de los vídeos FLV suele codificarse en formato MP3, pero puede grabarse sobre el vídeo utilizando un micrófono con el códec [Nellymoser Asao](https://en.wikipedia.org/wiki/Asao_\(codec\)).&#x20;



* **Año y origen:** Desarrollado por Macromedia (adquirida por Adobe en 2005). Fue el formato dominante en internet entre 2005 y 2015, cuando YouTube, Vimeo y la mayoría de plataformas de vídeo online lo usaban.
* **Funcionamiento:** Contenedor diseñado específicamente para streaming web dentro del plugin Adobe Flash Player. Estructura relativamente simple, optimizada para transmisión progresiva. Más tarde Adobe introdujo F4V, una evolución basada en MP4 para soportar H.264.
* **Códecs más comunes dentro:**
  * Vídeo: Sorenson Spark (H.263 modificado), On2 VP6, H.264 (en versiones posteriores)
  * Audio: MP3, AAC, ADPCM
* Está obsoleto. Adobe retiró Flash Player en diciembre de 2020. Los navegadores lo bloquearon progresivamente desde 2017. YouTube abandonó FLV en 2015 a favor de HTML5 con MP4/WebM.&#x20;

### WebM

Es un formato contenedor multimedia abierto y libre desarrollado por Google y orientado para usarse con HTML5. Se trata de un proyecto de software libre, bajo una licencia permisiva similar a la licencia BSD. Fue inicialmente pensado para ser utilizado con códec de vídeo VP8 y desarrollado originalmente por On2 Technologies y el códec de audio Vorbis dentro de un contenedor multimedia Matroska.

* **Año y origen:** Lanzado por Google en 2010 como formato abierto para vídeo en navegadores web, basado en una versión restringida del contenedor MKV.
* **Funcionamiento:** Es técnicamente un subconjunto simplificado de Matroska, diseñado con un perfil muy estricto para garantizar compatibilidad entre navegadores. Solo permite códecs específicos aprobados por el proyecto.
* **Códecs permitidos:**
  * Vídeo: VP8, VP9, AV1
  * Audio: Vorbis, Opus
* **Por qué importa:** VP9 y AV1 son royalty-free y muy eficientes. YouTube usa internamente AV1+Opus en WebM para servir vídeo cuando el navegador lo soporta, lo que reduce significativamente sus costes de ancho de banda a escala global.
* **Diferencia con MKV:** WebM restringe los códecs permitidos (solo los royalty-free de Google/AOMedia) y elimina algunas características de MKV para simplificar la implementación en navegadores. Un archivo WebM válido es técnicamente un MKV válido, pero no al revés.

Es bien soportado en Chrome, Firefox y Edge. Safari añadió soporte parcial. Es el formato estándar para vídeo HTML5 en la web junto a MP4.

### MKV (Matroska Multimedia Container)

**Año y origen:** Proyecto iniciado en 2002 por Lasse Kärkkäinen y Steve Lhomme. El nombre "Matroska" viene de las muñecas rusas Matryoshka, en referencia a su capacidad de contener múltiples streams anidados. El desarrollo fue inicialmente liderado desde Rusia aunque el proyecto es internacional.

**Cómo funciona:** Diseñado desde cero con filosofía de extensibilidad y apertura total. Su especificación es pública y libre. Usa el formato de datos EBML (Extensible Binary Meta Language), que permite añadir nuevas funcionalidades sin romper la compatibilidad.

**Lo que puede contener:**

* Cualquier códec de vídeo: H.264, H.265, AV1, VP9, MPEG-2, VC-1...
* Cualquier códec de audio: AAC, AC-3, DTS, FLAC, TrueHD, PCM...
* Múltiples pistas de subtítulos: SRT, ASS/SSA, PGS (Blu-ray), VobSub...
* Capítulos y menús
* Adjuntos (portadas, fuentes tipográficas)
* Múltiples ángulos de cámara

**Por qué los archivos son "grandes":** En realidad MKV no añade peso al archivo. El tamaño depende completamente del códec y bitrate del vídeo interior. Un MKV con H.265 CRF 23 será más pequeño que un MP4 con H.264 CRF 18.

**Limitación real:** No es el formato más compatible para distribución. Algunos Smart TVs, reproductores de Blu-ray, consolas antiguas y aplicaciones de streaming no lo soportan de forma nativa. Para uso doméstico con VLC, Kodi, Jellyfin o Plex es perfectamente válido.

**Uso real hoy:** El formato preferido de la escena de rips de Blu-ray y distribución de contenido de alta calidad, precisamente por su capacidad de incluir múltiples pistas de audio, subtítulos y capítulos en un solo archivo sin restricciones de códec.

***

**Resumen comparativo rápido:**

| Formato | Año  | Estado            | Mejor uso                                 |
| ------- | ---- | ----------------- | ----------------------------------------- |
| MP4     | 2001 | Vigente, estándar | Distribución universal, web, móvil        |
| MOV     | 1991 | Vigente           | Postproducción, ecosistema Apple          |
| AVI     | 1992 | Legacy            | Solo compatibilidad con archivos antiguos |
| FLV     | 2002 | Obsoleto          | Sin uso activo desde 2020                 |
| WebM    | 2010 | Vigente           | Vídeo web HTML5, streaming open source    |
| MKV     | 2002 | Vigente           | Almacenamiento local, home theater, rips  |

#### Codificación de vídeo avanzada AVC / H264

Se trata del estándar de compresión de vídeo que más se usa actualmente. El AVC/H.264 puede codificar vídeo de alta calidad con una velocidad de bits más baja que los estándares de compresión más antiguos. La "velocidad de bits" es el número de unidades de información que hay que procesar por cada segundo de vídeo. El Blu-ray y una gran variedad de servicios de transmisión, incluyendo la televisión a la carta y en directo, utilizan H.264.\
A pesar de que en ocasiones su uso requiere el pago de derechos a las organizaciones que poseen las patentes del mismo, más del 90% del sector del vídeo utiliza H.264. Un estándar de compresión de video que puede ser utilizado en: MP4, AVI, MKV, MOV, FLV, TS.

#### Protocolos de transmisión que utilizan H264

Casi todos los protocolos de transmisión en la actualidad son compatibles con H.264:

* el Protocolo de transmisión en tiempo real (RTSP)
* Transmisión en directo HTTP (HLS)
* Transmisión dinámica HTTP (HDS)
* Transmisión adaptativa dinámica sobre HTTP (MPEG-DASH).

Algunas aplicaciones de video son:

* FFmpeg
* Plex
* Jellyfin
* Kode

Nosotros veremos FFmpeg y YT-DLP.

## Links

* [https://digitalcommunications.wp.st-andrews.ac.uk/2019/04/08/what-is-a-jpeg-file/](https://digitalcommunications.wp.st-andrews.ac.uk/2019/04/08/what-is-a-jpeg-file/)&#x20;
* [https://grupo.us.es/gtocoma/pid/pid6/pid61.htm](https://grupo.us.es/gtocoma/pid/pid6/pid61.htm)
* [https://www.cloudflare.com/es-es/learning/video/what-is-mp4/](https://www.cloudflare.com/es-es/learning/video/what-is-mp4/)
* [https://areahifi.com/blogs/blog-areahifi/pcm-o-bitstream-tomando-la-decision-correcta-para-tu-configuracion-de-audio?srsltid=AfmBOoqGJR3eRZmVabWcnDomLzQLgsu5-rfDMgQUrXaLhoo2x2u375Wi](https://areahifi.com/blogs/blog-areahifi/pcm-o-bitstream-tomando-la-decision-correcta-para-tu-configuracion-de-audio?srsltid=AfmBOoqGJR3eRZmVabWcnDomLzQLgsu5-rfDMgQUrXaLhoo2x2u375Wi)
* [https://uniconverter.wondershare.es/file-formats/flv-file.html](https://uniconverter.wondershare.es/file-formats/flv-file.html)
*
