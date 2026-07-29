---
description: capa física
---

# Switches - puertos

### Modo duplex y semiduplex

* La comunicación en dúplex completo aumenta el ancho de banda eficaz al permitir que ambos extremos de una conexión transmitan y reciban datos simultáneamente.
  * Se conoce como comunicación bidireccional y requiere microsegmentación.
  * Las LAN microsegmentadas se crean cuando un puerto de switch tiene solo un dispositivo conectado y funciona en modo dúplex completo.
  * Cuando un puerto de switch opera en modo dúplex completo, no hay dominio de colisión conectado al puerto.
* La comunicación en semidúplex es unidireccional.
  * Genera problemas de rendimiento debido a que los datos fluyen en una sola dirección por vez, lo que a menudo provoca colisiones.
  * Las conexiones semidúplex suelen verse en los dispositivos de hardware más antiguos, como los hubs. L
  * a comunicación en dúplex completo reemplazó a la semidúplex en la mayoría del hardware.
* Las configuraciones de dúplex y de la velocidad para los puertos de switches Cisco como los Catalyst 2960 y 3560 es automática.
  * Los puertos 10/100/1000 funcionan en modo semidúplex cuando están configurados en 10 o 100 Mbps y operan solo en modo dúplex completo cuando está configurado en 1000 Mbps (1 Gbps).
  * La negociación automática es útil cuando la configuración de velocidad y dúplex del dispositivo que se conecta al puerto es desconocida o puede cambiar.
  * Cuando se conecta a dispositivos conocidos como servidores, estaciones de trabajo dedicadas o dispositivos de red, la mejor práctica es establecer manualmente la configuración de velocidad y dúplex.

Cuando hablamos de los puertos de fibra óptica, como los puertos 1000BASE-SX, éstos solo funcionan a una velocidad predefinida y siempre en modo dúplex completo.

**Nota:** Cuando encontramos incompatibilidades a la hora de configurar el modo dúplex y la velocidad de puertos del switch, se pueden producir problemas de conectividad. Un ejemplo sería una falla de autonegociación provoca incompatibilidades en la configuración.

### auto-MDIX

Hasta hace poco, se requerían determinados tipos de cable (cruzado o directo) para conectar dispositivos.&#x20;

En conexiones como: switch a switch o switch a router  se requería el uso de diferentes cables Ethernet. Actualmente esto se soluciona con el uso de la característica automática de conexión cruzada de interfaz dependiente del medio (auto-MDIX) en una interfaz.

* Una vez que se habilita esta característica auto-MDIX, la interfaz detecta automáticamente el tipo de conexión de cable requerido (directo o cruzado) y configura la conexión conforme a esa información.&#x20;
* Al conectarse a los switches sin la función auto-MDIX, los cables directos deben utilizarse para conectar a dispositivos como servidores, estaciones de trabajo o routers.&#x20;
* Los cables cruzados se deben utilizar para conectarse a otros switches o repetidores.
* Al usar auto-MDIX en una interfaz, la velocidad de la interfaz y el dúplex deben configurarse para que la función **auto** funcione correctamente.
* El comando para habilitar Auto-MDIX se emite en el modo de configuración de interfaz en el switch como se muestra: _S1(config-if)# **mdix auto**_

Si queremos examinar la configuración de auto-MDIX para una interfaz específica, podemos usar el comando:&#x20;

**show controllers ethernet-controller**  con la palabra clave **phy**.&#x20;

Para limitar la salida a líneas que hagan referencia a auto-MDIX, use el filtro **include Auto-MDIX** Como se muestra el resultado indica On (Habilitada) u Off (Deshabilitada) para la característica.

<pre><code><strong>S1# show controllers ethernet-controller fa0/1 phy | include MDIXAuto-MDIX
</strong><strong>:  On   [AdminState=1   Flags=0x00052248]
</strong></code></pre>

### Problemas de la capa de acceso a la red

La salida del comando **show interfaces** nos muestra un resultado útil para detectar problemas comunes de medios.&#x20;

<pre><code><strong>S1# show interfaces fastEthernet 0/18
</strong><strong>FastEthernet0/18 is up, line protocol is up (connected)
</strong><strong>Hardware is Fast Ethernet, address is 0025.83e6.9092 (bia 0025.83e6.9092)MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
</strong></code></pre>

Una parte importante de esta salida es visualizar la línea y el estado del protocolo de enlace de datos:

* El primer parámetro (FastEthernet0 / 18 está activo) hace referencia  a la capa de hardware e indica si la interfaz está recibiendo una señal de detección de portadora.&#x20;
* El segundo parámetro (line protocol is up) se refiere a la capa de enlace de datos e indica si se reciben los keepalives del protocolo de capa de enlace de datos.

Los posibles problemas  que se presentan pueden solucionar de la siguiente manera:

* Si la interfaz está activa y el protocolo de línea está inactivo, hay un problema.&#x20;
  * Puede haber una incompatibilidad en el tipo de encapsulación, la interfaz en el otro extremo puede estar inhabilitada por errores o puede haber un problema de hardware.
* Si el protocolo de línea y la interfaz están inactivos, no hay un cable conectado o existe algún otro problema de interfaz.&#x20;
  * Por ejemplo, en una conexión directa, el otro extremo de la conexión puede estar administrativamente inactivo.
* Si la interfaz está administrativamente down, quiere decir que se ha desactivado manualmente (the \*\*\*\*shutdown) en la configuración activa.



### Tipos de errores

Hay errores que no son lo suficientemente graves como para hacer que el circuito falle, pero si pueden generar problemas de rendimiento de la red.

<figure><img src="../../.gitbook/assets/image (930).png" alt="" width="375"><figcaption><p>Tomado de CCNA</p></figcaption></figure>

* **input errors** - cantidad total de errores (en la imagen dice 3). Incluye runts, gigantes, CRC, desbordamiento y recuentos ignorados.
* **fragmentos de colisión** - son paquetes que se descartan porque son más pequeños que el mínimo tamaño del paquete para el medio. Por ejemplo, un paquete de Ethernet que sea menor que 64 bytes se considera un `runt`.
* **gigantes** - son paquetes que se descartan porque exceden el tamaño máximo de paquete para el medio. En este caso si un paquete de Ethernet es mayor que 1.518 bytes se considera un `gigante`.
* **CRC** - Este tipo de errores se genera cuando la suma de comprobación calculada no es la misma que la suma de comprobación recibida.
* **Errores de salida** - Es la suma de todos los errores que impiden la transmisión final de datagramas de la interfaz que se está examinando.
* **Colisiones** - Cantidad de mensajes retransmitidos debido a una colisión de Ethernet.
* **Colisiones tardías** - Se refiere a una colisión que ocurre después de que se han transmitido 512 bits de la trama. La longitud excesiva de los cables es la causa más frecuente de las colisiones tardías. Otra causa frecuente es la configuración incorrecta de dúplex. Por ejemplo, el extremo de una conexión puede estar configurado para dúplex completo y el otro para semidúplex.



### Resolución de problemas de la capa de acceso a la red

Muchos de los problemas que afectan a las redes conmutadas se produce durante la implementación inicial. En principio, cuando se instalan, las redes continúan funcionando sin problemas, pero los cables se dañan, la configuración puede ser modificada, se conectan al switch nuevos dispositivos que obligan a realizar cambios de configuración en el switch. Por tanto, se requiere de mantenimiento y de resolución de problemas de infraestructura de la red de manera permanente.

El comando `show interfaces` nos permite verificar el estado de una interfaz de red  y diagnosticar problemas físicos y de enlace:

¿Interfaz Inactiva (Down)? ──────► 1. Verificar cableado/conectores.\
&#x20;                                                               2\. Corregir desajuste de velocidad (Speed).

¿Interfaz Activa (Up) con errores? ─► 1. Revisar ruido/longitud (CRC, Runts, Giants). \
&#x20;                                                                2\. Corregir desajuste de Dúplex (Colisiones).



| **Estado de la interfaz** | **Síntoma / Contador**                  | **Causa probable**                                                                    | **Solución recomendada**                                                         |
| ------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Inactiva (Down)           | Sin enlace físico                       | Cable defectuoso, tipo de cable incorrecto o conector dañado.                         | Inspeccionar y sustituir el cableado. Verificar distancias.                      |
| Inactiva (Down)           | Fallo en negociación                    | Incompatibilidad en la configuración de velocidad (_speed mismatch_).                 | Configurar manualmente la misma velocidad (`speed`) en ambos extremos.           |
| Activa (Up) con fallos    | Incremento de _CRC_, _Runts_ o _Giants_ | Ruido excesivo, interferencias electromagnéticas o cable fuera de categoría/longitud. | Eliminar fuentes de ruido, verificar categoría del cable y no superar los 100 m. |
| Activa (Up) con fallos    | Colisiones / _Late Collisions_          | Desajuste en el modo de transmisión (_duplex mismatch_).                              | Configurar manualmente ambos extremos en modo Full-Duplex (`duplex full`).       |

#### Aspectos clave a considerar

1. **La autonegociación puede fallar**: Aunque la velocidad y el dúplex suelen negociarse de forma automática, si hay un fallo de hardware o mala configuración, se debe pasar a configuración manual en ambos extremos.
2. **"Up" no significa "Sin errores"**: Una interfaz puede estar encendida (_Up/Up_) pero transmitir con un rendimiento deficiente debido a errores de trama (CRC) o colisiones debidas a un dúplex desajustado (_Half_ vs _Full_).







<br>
