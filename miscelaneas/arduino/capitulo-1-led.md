---
description: www.freenove.com
---

# Capítulo 1 Led

Ahora es que realmente vamos a comenzar a construir y explorar algunos proyectos basados en el chip ESP32-S3 WROOM. En este caso vamos a utilizar nuestro chip para controlar el parpadeo de un LED común.

<table><thead><tr><th width="288">Componente</th><th>Imagen</th></tr></thead><tbody><tr><td><p><br></p><p>SP32-S3 WROOM</p><p><br></p></td><td><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></td></tr><tr><td><p>GPIO Extension Board</p><p><br></p></td><td><p><br></p><p><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></p></td></tr><tr><td><p>Breadboard</p><p><br></p></td><td><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></td></tr><tr><td><p>LED</p><p><br></p></td><td><br><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></td></tr><tr><td><p>Resistencia</p><p><br></p></td><td><br><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""></td></tr><tr><td><p>Jumper</p><p><br></p></td><td><p></p><p><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><br></p></td></tr></tbody></table>

#### LED

Es un tipo de diodo. Los diodos tienen dos polos y funcionan si la corriente fluye en la dirección correcta. Un LED se enciende si el pin más largo (+) está conectado a la salida positiva de una fuente de alimentación. El pin más corto está conectado al negativo (-), que también se conoce como Tierra (GND).

Los diodos funcionan sólo si el voltaje de su electrodo (+) es mayor que el de su electrodo (-). Por otra parte, hay un rango estrecho de voltaje de funcionamiento para la mayoría de los diodos comunes de 1.9 y 3.4V, por lo que si utilizas mucho más de 3.3V, el LED se va a dañar y se quemará.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="89"><figcaption><p>Diodo</p></figcaption></figure>

El símbolo correspondiente a un diodo es:&#x20;

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="87"><figcaption><p>Simbolo del diodo</p></figcaption></figure>

La correspondencia de voltaje y amperaje para cada LED o diodo, sería:

| LED   | Voltaje     | MAX mA | Recomendado mA |
| ----- | ----------- | ------ | -------------- |
| ROJO  | 1.9V - 2.2V | 20mA   | 10mA           |
| VERDE | 2.9V - 3.4V | 10mA   | 5mA            |
| AZUL  | 2.9V - 3.4V | 10mA   | 5mA            |



#### Resistencia

Los LED no se pueden conectar directamente a una fuente de alimentación, porque puede acabar dañado. Para evitar que un componente se dañe necesitamos controlar el voltaje que le llega y para ello necesitamos una resistencia que no es más que un componente eléctrico pasivo que limita o regula el flujo de corriente en un circuito electrónico.

La resistencia utiliza ohmios (Ω) como unidad de medida de su resistencia ®, donde:

1MΩ = 1000kΩ

1kΩ = 1000Ω

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1).png" alt="" width="45"><figcaption><p>Resistencia </p></figcaption></figure>



El símbolo utilizado para representar la presencia de una resistencia en un diagrama o esquema de circuito.

Las bandas de colores de la resistencia indican el valor que posee. La relación que existe entre la corriente eléctrica, el voltaje y la resistencia se puede expresar mediante mediante la conocida Ley de Ohm:

I = V / R

donde:

I = Corriente

V = Voltaje

R = Resistencia

\
\
Si conocemos dos de estos valores podemos, por supuesto, calcular el tercero. Por ejemplo:

Si en el circuito de la imagen, conocemos que la resistencia tiene un valor de `10kΩ` y el voltaje es de `5V` entonces la corriente eléctrica que circula a través de la resistencia `R` es:

```
I=V/R => 5V/10kΩ = 0.0005A = 0.5mA
```

_Nota: No conectes los dos polos de una fuente de alimentación a algo de bajo valor de resistencia (un objeto de un metal o cable desnudo) porque puede provocar un cortocircuito: produce una corriente alta que puede dañar la fuente de alimentación y los componentes electrónicos._

\
Ya sabemos que el chip `ESP32-S3 WROOM` necesita una fuente de alimentación de `5V`. Necesitamos conectar el `ESP32-S3 WROOM` al PC usando un cable USB para alimentarlo.

#### Circuito

Para comenzar a construir el circuito, desconecta la placa `ESP32-S3 WROOM` de la corriente (o del PC).

Vamos a construir nuestro circuito según se muestra en el diagrama. Solo después de construirlo es que podemos conectarlo al PC para verificar que es correcto.

\


<figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Circuito</p></figcaption></figure>

O de otra manera:

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption><p>Circuito</p></figcaption></figure>

Según el circuito, si el nivel de salida `GPIO2` del `ESP32-S3 WROOM` es alto, el `LED` se enciende y cuando el nivel de salida es bajo, el `LED` se apaga. Por lo tanto, podemos dejar que el `GPIO2` emita circularmente niveles altos y bajos para hacer que el `LED` parpadee.

Busquemos el código del Blink:

```
Freenove_Ultimate_Starter_Kit_for_ESP32_S3\Sketches\Sketch_01.1_Blink
```

Observa en el menú los items siguientes: Board, Port y Upload Speed\


<figure><img src="../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

Recuerda que el chip que tenemos que utilizar es el llamado: `ESP32 Wrover Module`

En el siguiente enlace podrás ver el circuito en funcionamiento: [https://youtube.com/shorts/oylvwFoAS68?feature=share](https://youtube.com/shorts/oylvwFoAS68?feature=share)

## Links

1. [ubunlog.com/arduino-ide-en-ubuntu/](https://ubunlog.com/arduino-ide-en-ubuntu/)
2. [https://ubunlog.com/que-son-las-appimage-y-como-instalarlas-en-ubuntu/](https://ubunlog.com/que-son-las-appimage-y-como-instalarlas-en-ubuntu/)
3. [https://www.fantasystudios.es/arduino/pages/instalacion/instalacion\_2.html](https://www.fantasystudios.es/arduino/pages/instalacion/instalacion\_2.html)

\
\
