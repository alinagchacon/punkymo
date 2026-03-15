---
description: FFmpeg
---

# FFprobe

Esta es una herramienta que viene con el proyecto `FFmpeg` y sirve para analizar ficheros multimedia como vídeos, audios. FFprobe **no convierte ni procesa**, solo **muestra información técnica**.

`ffprobe` recopila información de flujos multimedia y la muestra en formato legible para las personas y para  las máquinas. Por ejemplo, nos permite comprobar el formato del contenedor de un flujo multimedia, así como el formato y el tipo de cada flujo multimedia que contiene.

También se puede usar como una aplicación independiente o en combinación con un filtro de texto, que puede realizar un procesamiento más sofisticado, como el procesamiento estadístico o la creación de gráficos.&#x20;

`ffprobe` sirve para:

* inspeccionar archivos multimedia.
* comprobar streams.
* obtener datos técnicos para scripts.
* diagnosticar problemas de codificación.

### Uso básico de ffprobe

El uso más simple  que podemos darle a esta herramienta es:

```bash
ffprobe archivo.mp4
```

A pesar de ser un formato simple, nos muestra mucha información técnica del tipo:

* Duración
* Bitrate
* Codec de vídeo
* Codec de audio
* Resolución

Un ejemplo de salida sería:

```
Duration: 00:03:15.20
Stream #0:0: Video: h264, 1920x1080, 30 fps
Stream #0:1: Audio: aac, 48000 Hz, stereo
```

### Mostrando información básica

Para obtener un resumen limpio podemos utilizar ffprobe con las opciones siguientes que permiten ocultar mensajes poco útiles, información general, etc.

```bash
ffprobe -v error -show_format -show_streams archivo.mp4
```

donde:

* `-v error` → oculta mensajes innecesarios.
* `-show_format` → información general
* `-show_streams` → información de cada stream

### ¿Y si queremos saber la duración exacta de un vídeo?

Si queremos conocer la duración exacta de un vídeo, algo especialmente útil si trabajamos como  scripts podemos hacer:

```bash
ffprobe -v error -show_entries format=duration \
-of default=noprint_wrappers=1:nokey=1 archivo.mp4
```

Y nos muestra una salida como la siguiente, en segundos:

```
195.204000 
```

### ¿Y si queremos  ver resolución del vídeo?

Para ver la resolución del vídeo podemos hacer lo siguiente:

```bash
ffprobe -v error -select_streams v:0 \
-show_entries stream=width,height \
-of csv=s=x:p=0 archivo.mp4
```

Y una salida que nos pudiera mostrar sería:

```
1920x1080
```

### Para ver el tipo de codec utilizado

Si queremos ver el tipo de codex que se ha utilizado podemos hacer lo siguiente:

```bash
ffprobe -v error -select_streams v:0 \
-show_entries stream=codec_name \
-of default=noprint_wrappers=1:nokey=1 archivo.mp4
```

Monstrándonos una salida como la siguiente:

```
h264
```

## Cantidad de fotogramas por segundo - FPS

Si quisiéramos saber la cantidad de **frames per second o** **fotogramas por segundo** que indican cuántas imágenes individuales se muestran cada segundo en un vídeo o animación tendríamos que hacer lo siguiente:

```bash
ffprobe -v error -select_streams v:0 \
-show_entries stream=r_frame_rate \
-of default=noprint_wrappers=1:nokey=1 archivo.mp4
```

{% hint style="info" %}
Un vídeo realmente no es movimiento continuo, sino una secuencia rápida de imágenes llamadas _frames_ o fotogramas. Si un vídeo tiene:

* **24 FPS** → se muestran **24 imágenes cada segundo**
* **30 FPS** → se muestran **30 imágenes cada segundo**
* **60 FPS** → se muestran **60 imágenes cada segundo**



Por tanto, cuantos **más FPS**, **más fluido se ve el movimiento**.
{% endhint %}

El formato de salida sería algo como:

```
30/1
```

eso sería:

```
30 FPS
```

## Mostrando la información en formato JSON

Obtener la información en formato JSON resulta muy útil para scripts o automatización:

```bash
ffprobe -v quiet -print_format json \
-show_format -show_streams archivo.mp4
```

Ejemplo de salida:

```json
{
  "streams": [...],
  "format": {
     "duration": "195.204000",
     "bit_rate": "4200000"
  }
}
```



### Links

* [https://ffmpeg.org/ffprobe.html](https://ffmpeg.org/ffprobe.html)&#x20;

