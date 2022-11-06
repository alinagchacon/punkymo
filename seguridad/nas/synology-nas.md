---
description: 'Tomado de: https://www.youtube.com/watch?v=Z5X2mdJkD44'
---

# Synology NAS

Investigando....

1. [https://xpenology.club/downloads/](https://xpenology.club/downloads/)
2. Descargamos XPEnology: un emulador del hardware de Synology

<figure><img src="../../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

3\. Descomprimimos el .ZIP y vemos el fichero: <mark style="color:blue;">`synoboot.img`</mark>&#x20;

4\. En: [https://www.starwindsoftware.com](https://www.starwindsoftware.com) vamos a StarWind V2V Converter. La idea es transformar el <mark style="color:blue;">`synoboot.img`</mark> en <mark style="color:blue;">`synoboot.vmdk`</mark>.

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

Para crear una MV necesitamos convertir el fichero de imagen en un fichero vmdk y para ello usamos el V2V Converter de StarWind.

Clicas en el

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

Instalamos el software: starwindconverter.exe

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

Ahora ejecutamos la aplicación para convertir la imagen a vmdk:

&#x20;

<figure><img src="../../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

Sigue los pasos por defecto. Al final, tendrás el fichero con extensión .vdmk

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>



### Instalación

Crea una MV en VMWare

<figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

Seleccionamos Linux como sistema operativo.

<figure><img src="../../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

Seleccionar:

* CPU: 1
* RAM: 4096
* Adaptador: Bridge

<figure><img src="../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

Agregamos un nuevo disco duro de modo que nos quede así:

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

El primer disco duro "Hard Disk" de la lista es quien tiene asignado el simulador de hardware de Synology.

<figure><img src="../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

Si ntentamos inicializar la MV nos aparece .....

Vamos a un navegador y buscamos: [https://finds.synology.com](https://finds.synology.com) y nos descargamos el asistente de búsqueda de Synology:

<figure><img src="../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

Instala el asistente en cuestión.&#x20;

<figure><img src="../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

En la pasarela le asigné la: 192.168.56.1 y puede que salga error inicialmente pero si vuelves a pedir que busque saldrá correctamente:

![](<../../.gitbook/assets/image (190).png>)

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

Una vez descargado, volvemos a la pantalla siguiente y buscamos el archivo descargado con examinar.

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

He instalas







