---
description: Switches
---

# Arranque

Los switches se utilizan  para conectar dispositivos en una misma red y son responsables de controlar el flujo de datos en la capa de acceso y de dirigir  el tráfico a los recursos conectados en red.

En el caso  de Cisco, los switches son de configuración automática y no necesitan de configuración adicional para funcionar, pero si se pueden configurar de modo manual. Algunas de las configuraciones que podemos realizar en los switches incluye: el ajuste de la velocidad, el ancho de banda y la seguridad de los puertos.

<figure><img src="../../.gitbook/assets/image (440).png" alt="" width="563"><figcaption><p>Tomado de <a href="https://redes.umh.es/cisco/CCNA/es/RSE/index.html#5.1.1.1">https://redes.umh.es/cisco/CCNA/</a></p></figcaption></figure>

Cuando se enciende un switch de Cisco, se lleva a cabo la siguiente secuencia de arranque:

1. Se carga el programa POST de autodiagnóstico que se encuentra almacenado en la ROM. Este programa verifica la CPU, la DRAM y la parte del dispositivo flash que integra el sistema de archivos flash.
2. A continuación, el switch carga el software de arranque, que no es más que un pequeño programa almacenado en la ROM que se ejecuta una vez que el POST finaliza.
3. El software de arranque inicializa de la CPU de bajo nivel y los registros de la CPU, que controlan dónde está asignada la memoria física, la cantidad de memoria y su velocidad.
4. Este software de arranque es quien inicia el sistema de archivos flash en la placa del sistema.
5. Finalmente, el arranque localiza y carga la imagen del sistema operativo en la memoria y delega el control del switch al sistema.

Es el sistema operativo quien inicia las interfaces utilizando los comandos del sistema que se encuentran en el archivo de configuración del arranque, que se almacena en la NVRAM.

