---
description: Apuntes de www.freenove.com
---

# Arduino

Estos apuntes están tomados de: [www.freenove.com](https://www.freenove.com), más específicamente del libro: `C_Tutorial.pdf`. También te recomiendo  tener en cuenta el material de Processing.pdf. Recuerda que no estoy plagiando nada, estoy aprendiendo y estos son mis notas de mi propio recorrido de aprendizaje. Mi intención es hacer un recorrido por Arduino utilizando la placa ESP32. \
Siempre dejo enlaces a los sitios web que me son útiles.&#x20;

Veamos algunos aspectos previos.

## Tinkercad <a href="#__refheading___toc418_2914418030" id="__refheading___toc418_2914418030"></a>

Es una colección online de herramientas de software de Autodesk que:&#x20;

* Permite crear modelos 3D.&#x20;
* Se basa en una geometría sólida constructiva (CSG).&#x20;
* Permite crear modelos complejos mediante la combinación de objetos más simples.&#x20;
* Fácil de usar y gratis.&#x20;
* Se puede utilizar para impresión 3D.

Lo puedes encontrar en su página oficial: [https://www.tinkercad.com](https://www.tinkercad.com) y más adelante haremos un buen uso de ella.

## Arduino IDE

Se trata de una suite de programación creado por los responsables de Arduino, que permite introducir el software en las placas Arduino. No solo es un editor de código sino que tiene un depurador y un compilador que nos facilita la creación del programa final y nos permite enviarlo a la memoria de la placa Arduino. Como es de esperar, existen muchos IDE gratuitos en Ubuntu, pero ninguno de ellos ofrece conexión con los modelos oficiales de placas Arduino.&#x20;

Las últimas versiones de Arduino IDE han hecho que este programa sea más compatibilidad con los nuevos modelos de Arduino sino que también han mejorado sus funciones de IDE, dado que:

* Permite tener una **interfaz cloud que facilita la creación de un programa para Arduino en cualquier parte del mundo**.
* Arduino IDE admite conexión con todo tipo de programas, editores de código incluidos que nos facilitará el trabajo con el hardware de Arduino.
* Es un software libre.

Ahora si vamos a instalar Arduino.

## Instalar Arduino IDE en Ubuntu 22.04

Arduino IDE no se encuentra en los repositorios oficiales de Ubuntu por lo que **tenemos que utilizar la web oficial de Arduino para descargar el IDE**.

Nos vamos a descargar la última versión de Arduino IDE porque de este modo podemos cambiar en cualquier momento de placa y la versión lo soportará.

<figure><img src="../../.gitbook/assets/image (228).png" alt=""><figcaption><p>Sitio de descarga de Arduino IDE en <a href="https://www.arduino.cc/en/software">https://www.arduino.cc/en/software</a></p></figcaption></figure>

### Opción 1

Vamos a Software > Download > Linux AppImage 64 bits (X86-64) y  descargamos nuestro fichero: arduino-ide\_nightly-20230908\_Linux\_64bit.AppImage.

Si te das cuenta está en formato AppImage y ¿qué es? Pues este formato tiene una gran **ventaja sobre los otros formatos de paquetes tradicionales, dado que es universal.** Es como si fuera una aplicación portable, donde el software se ejecuta con el archivo AppImage sin tener que hacer instalaciones, ni descomprimir archivos.

\
Otros beneficios de AppImage:

* Se puede ejecutar en la mayoría de las distribuciones de Linux actuales
* Es portable
* Las aplicaciones están en modo de solo lectura.
* Se puede ejecutar en versiones Live
* No hay que instalar y compilar software
* No es necesario el permiso de root dado que no se tocan los archivos del sistema

Para instalar el Arduino IDE desde el archivo .AppImage hacemos lo siguiente. Le damos permisos de ejecución al archivo de instalación y ejecutamos el comando:

```
chmod u+x arduino-ide_nightly-20230908_Linux_64bit.AppImage
```

Una vez que tiene los permisos de ejecución, ejecutamos desdela terminal ejecutar el comando:

```
./arduino-ide_nightly-20230908_Linux_64bit.AppImage
```

De inmediato nos preguntará si estamos de acuerdo con los términos y condiciones. Marcamos “Agree” y listo.

### Opción 2

Vamos a descargar el ZIP: `Software > Download > Linux >` [`ZIP file 64 bits (X86-64)`](https://downloads.arduino.cc/arduino-ide/arduino-ide\_2.2.1\_Linux\_64bit.zip?\_gl=1\*1djazbm\*\_ga\*NjAxNjU2OTMyLjE2OTQwODM1OTI.\*\_ga\_NEXN8H46L5\*MTY5NDM1NDAxMS41LjEuMTY5NDM1NDAzNy4wLjAuMA..) y lo descomprimimos en una carpeta (le podemos poner el nombre Arduino) y ejecutamos el archivo <mark style="color:blue;">`arduino-ide.`</mark>

### Opción 3

Actualizamos el sistema operativo:

```
sudo apt update
sudo apt upgrade
```

```
sudo apt install arduino
```

Y se nos instala la versión 1.8.19.

<figure><img src="../../.gitbook/assets/image (230).png" alt=""><figcaption><p>Arduino 1.8.19 instalado en Ubuntu 22.04</p></figcaption></figure>



## ESP32-S3 WROOM GPIO&#x20;

La extension board GPIO de la placa ESP32-S3 WROOM nos permite utilizar el ESP32-S3 de un modo más sencillo. Las interfaces de hardware de ESP32-S3 WROOM se distribuyen de la siguiente manera:

<figure><img src="../../.gitbook/assets/image (231).png" alt=""><figcaption><p>ESP32</p></figcaption></figure>

Donde:&#x20;

* Verde - power supplied by the extension board&#x20;
* Rojo - GPIO pin&#x20;
* Azul más oscuro - LED indicator&#x20;
* Azul claro - GPIO interface of development board&#x20;
* Fucsia - External power supply&#x20;

En la ESP32-S3, GPIO se trata de una interfaz para controlar el circuito periférico. En los proyectos que vamos a realizar aquí, solo utilizamos un cable USB para alimentar ESP32-S3 WROOM de forma predeterminada.

## CH343

En el manual de referencia  nos hablan del CH343 que es utilizado por el chip ESP32-S3 WROOM para descargar códigos. En dicho manual especifican qué hacer para asegurarnos que nos funciona todo bien en Windows y Mac pero no dice nada de Linux. De no hacerle caso a este, después te puedes encontrar con qué no podemos seleccionar el puerto de acceso al chip ESP32-S3. Al menos fue lo que me ocurrió.

Buscando en Google me encontré con la siguiente página que me dió la solución:

[https://fgcoca.github.io/Mis-notas-sobre-Linux-Ubuntu/ch340/](https://fgcoca.github.io/Mis-notas-sobre-Linux-Ubuntu/ch340/)



En dicho sitio nos dicen que los dispositivos CH340 USB no funcionan en la versión Ubuntu 22.04. En la versión 22.04 el soporte para dispositivos CH340 USB a adaptador serie no genera un /dev/ttyUSB0, cuando partimos de instalación limpia. En versiones anteriores, como es el caso de la 20.04, el soporte era nativo.

¿Qué ocurre? Que si conectamos una placa con el driver CH340 al USB no nos lista un puerto de escucha. Si vamos a la terminal y hacemos:

`ls /dev` no nos lista una entrada ttyUSB0.

<figure><img src="../../.gitbook/assets/image (232).png" alt=""><figcaption><p>Menú Tools de Arduino IDE</p></figcaption></figure>



Si hacemos `lsusb` nos devuelve algo como:

`Bus 003 Device 002: ID 1a86:7523 QinHeng Electronics CH340 serial converter`

También podemos comprobar que el módulo `ch34x` está cargado utilizando el comando:

```
lsmod
```

Parece ser (esto lo tengo que investigar) que existe un conflicto entre la identificación del producto con un chip basado en CH340 y el lector de pantalla Braille. En la página web que he consultado menciona el tema de la “pantalla Braille” y la necesidad de editar el fichero de reglas de brttty y hacer una modificación para asegurar que se soportan dispositivos CH340.Entonces editamos con nano y con permisos de root el archivo:

```
/usr/lib/udev/rules.d/85-brltty.rules
```

Buscamos la siguiente línea y la comentamos:

```
ENV{PRODUCT}=="1a86/7523/*", ENV{BRLTTY_BRAILLE_DRIVER}="bm", GOTO="brltty_usb_run"
```

`Ya solo nos queda reiniciar` el sistema y con esto ya tengo soporte para los dispositivos CH340.

## Configuración del entorno&#x20;

Lo primero es configurar la placa ESP32-S3 para poder trabajar con ella. Lo más importante es agregar el siguiente enlace en el apartado `Additional boards manager URLs`:&#x20;



```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

<figure><img src="../../.gitbook/assets/image (233).png" alt=""><figcaption><p>Preferencias en el menú de Arduino IDE</p></figcaption></figure>

Ahora clicamos `Boards Manager` y escribimos `esp32` y seleccionamos la versión `2.0.5`，y he instalamos.

<figure><img src="../../.gitbook/assets/image (234).png" alt=""><figcaption><p>nstalando el software de la ESP32</p></figcaption></figure>

Una vez instalado el chip de la ESP32 hacemos click en “Tools” en el menú seleccionamos “Board: "Arduino Uno" y ya podremos ver la información de la ESP32.

<figure><img src="../../.gitbook/assets/image (236).png" alt=""><figcaption><p>Ya tenemos instalado el software de la ESP32</p></figcaption></figure>

En el menú que se despliega selecciona `ESP32-S3 Dev Module` y ya se debe tener acceso a la información de la placa.

## Links

* [http://esp32.net/](http://esp32.net/)
* [https://www.tinkercad.com/](https://www.tinkercad.com/)
