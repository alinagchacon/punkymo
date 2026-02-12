---
description: Switches
---

# Recuperación

### ¿Qué sucede si se daña el archivo del sistema o no se puede usar el sistema operativo del switch?&#x20;

Es el propio sistema de arranque el que brinda acceso al switch en esta situación. El sistema de arranque tiene una línea de comandos que proporciona acceso a los archivos ubicados en la memoria flash. Para ello, hay que utilizar la consola. Veamos que deberíamos hacer:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt="" width="337"><figcaption><p>Conexión de un PC por el puerto de consola de un Switch</p></figcaption></figure>

1. Con un cable de consola, conectamos un PC al puerto de consola del switch. Para ello debemos utilizar y configurar un software de emulación de terminal como pueden ser MobaXTerm, Tera Term o PuTTY para Windows.
2. Desconecta y vuelve a conectar el cable de alimentación del switch, esperando unos 15 segundos y, manteniendo presionado el botón Mode mientras el LED del sistema parpadea con luz verde.
3. Una vez que el LED del sistema se vea de color ámbar y luego verde, podemos soltar el botón Mode.
4. Veremos la petición de entrada en el switch, o sea, el software de arranque.

En la línea de comandos del **boot loader** podemos utilizar comandos para:&#x20;

* formatear el sistema de archivos flash.
* volver a instalar el software del sistema operativo.
* recuperar una contraseña perdida.
* visualizar la lista de archivos dentro de un directorio específico, con el comando **dir**.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1).png" alt="" width="563"><figcaption><p>Copia y visualización del running-config en la flash del Switch</p></figcaption></figure>

