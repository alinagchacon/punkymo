---
description: www.freenove.com
---

# Capítulo 0 Blink

Utilizaremos la placa `ESP32-S3 WROOM` para controlar el parpadeo de un LED común. Como véis solo vamos a conectar la placa al PC y ver como parpadea el led azul.

| Componente                | Imagen                                                                                                                                                        |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><br>ESP32-S3 WROOM</p> | <p><br><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></p> |
| <p><br>USB cable</p>      | ![](<../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)                                        |

El ESP32-S3 WROOM necesita una corriente de 5v, aunque en este tutorial lo vamos a conectar directamente al PC vía el cable USB. Open Arduino IDE 2.0.0 y&#x20;

Click Tools->Upload Mode y selecciona USB-OTG CDC(TinyUSB)\


<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

El puerto del PC puede ser diferente para cada usuario, así que tendrás que ver cuál es en tu caso. \


<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Clica el botón Upload y subirá y compilará el script correspondiente al ESP32-S3-WROOM.

Llegados a este punto pudiera parecer que todo estaba perfecto pero no es así puesto que al compilar tuve varios errores que tuve que ir solucionando:

* Crear un alias para python3 de modo que python llamara a python3. Buscando en Google  encontré el siguiente sitio [https://askubuntu.com/questions/320996/how-to-make-python-program-command-execute-python-3](https://askubuntu.com/questions/320996/how-to-make-python-program-command-execute-python-3) y seguí su recomendación. Esto es:

```
sudo apt install python-is-python3
```

*   Siguiente error:

    _File "/home/kirby/.arduino15/packages/esp32/tools/esptool\_py/4.2.1/esptool/loader.py", line 21, in \<module>_

    _import serial_

    _ModuleNotFoundError: No module named 'serial'_

    \


    Para solucionarlo he instalado pyserial que nos permite establecer comunicación con el puerto serie.

```
sudo apt install python3-pip
pip install pyserial
```

*   Ahora debería compilar pero tampoco, pues me sale el siguiente error:



    _Serial port /dev/ttyUSB0_

    _Connecting......._

    _A fatal error occurred: This chip is ESP32 not ESP32-S3. Wrong --chip argument?_

    _Failed uploading: uploading error: exit status 2_

\
Así que toca revisar desde el inicio la instalación de la ESP32-S3, pero no encuentro nada diferente a lo que dice el material de referencia así que en lugar de seleccionar el chip `ESP32S3`- selecciono este otro `ESP32 Wrover Kit (all versions)`:

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Y voilà esta vez todo funcionó bien: compila bien y veo que parpadea el led azul y esto es todo por ahora!

Está claro que necesitamos comprender algunos detalles, comenzando por la placa ES32. Así que dejo aquí un link que nos puede ayudar: [https://www.espressif.com/en/products/socs/esp32](https://www.espressif.com/en/products/socs/esp32)



