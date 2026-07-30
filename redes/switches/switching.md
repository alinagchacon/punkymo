# Switching

Hablar de switching y reenvío de tramas es  obligatorio en tecnología de redes y telecomunicaciones.&#x20;

Los switches toman decisiones para hacer el reenvío de tráfico. Estas decisiones se toman teniendo en cuenta el flujo de ese tráfico. Para entender ésto, hay dos términos asociados a las tramas que entran y salen de una interfaz:

* **Entrada** - término que permite describir el puerto por donde una trama ingresa al dispositivo.
* **Salida** - término para describir el puerto que las tramas utilizarán al salir del dispositivo.

Un switch LAN mantiene una tabla a la que hace referencia al reenviar tráfico a través de sus puertos. La inteligencia de un switch LAN radica en su capacidad de utilizar su tabla para reenviar el tráfico. Para ello se basa en el puerto de entrada y la dirección MAC de destino de una trama Ethernet.&#x20;

En un switch LAN, hay una sola tabla de switching principal que describe una asociación estricta entre las direcciones MAC y los puertos. Esto hace que una trama Ethernet que tiene una dirección de destino determinada siempre sale por el mismo puerto de salida, independientemente del puerto de entrada por el que llegue.

### Tabla de direcciones MAC&#x20;

Los switches se componen de circuitos integrados y de un software que controla las rutas de datos a través del mismo. Las direcciones MAC de destino son usadas por el switch para dirigir las comunicaciones de red a través del switch hacia el destino.

Lo primero que tiene que hacer un switch para definir qué puerto utilizar para transmitir una trama, es precisamente conocer cuáles son los dispositivos que existen en cada puerto. A medida que el switch aprende los dispositivos que tiene conectado a cada puerto, va construyendo la llamada tabla de direcciones MAC - que se almacena en la Memoria de Contenido Direccionable (Content-Addressable Memory, CAM - que no es más que un tipo especial de memoria utilizada en aplicaciones de búsqueda de alta velocidad.&#x20;

Un switch LAN:&#x20;

* Determina cómo manejar las tramas de datos entrantes manteniendo la tabla de direcciones MAC.&#x20;
* Llena su tabla de direcciones MAC al registrar la dirección MAC de origen de cada dispositivo conectado a cada uno de sus puertos.&#x20;
* Hace referencia a la información en la tabla de direcciones MAC para enviar tramas destinadas a un dispositivo específico fuera del puerto que se ha asignado a ese dispositivo.



### Método - aprender y reenviar

Para cada trama de Ethernet que ingresa a un switch se realiza el siguiente proceso de dos pasos:

**(1)  Aprender - se examina la dirección origen MAC**

Cada trama que ingresa a un switch es revisada para obtener información nueva. Para ello, se examina la MAC de origen de la trama y el número de puerto por el que ingresó al switch.

* Si la  MAC de origen no existe en la tabla de direcciones MAC, la  MAC y el número de puerto entrante son agregados a la tabla.
* Si la  MAC de origen existe, el switch actualiza el temporizador para esa entrada. De manera predeterminada, la mayoría de los switches Ethernet guardan una entrada en la tabla durante cinco minutos. Si la dirección MAC de origen existe en la tabla, pero en un puerto diferente, el switch la trata como una entrada nueva. La entrada se reemplaza con la misma  MAC, pero con el número de puerto más actual.

**(2) Reenviar - se examina la dirección destino MAC**

En caso de que la MAC de destino es una dirección de unidifusión, el switch busca una coincidencia entre la dirección MAC de destino de la trama y una entrada de la tabla de direcciones MAC:

* Si la MAC de destino está en la tabla, rse eenviará la trama por el puerto especificado.
* Si la MAC de destino no está en la tabla, el switch reenviará la trama por todos los puertos, excepto por el de entrada.&#x20;
  * Esto se conoce como unidifusión desconocida.&#x20;
  * Si la  MAC de destino es de difusión o de multidifusión, la trama también se envía por todos los puertos, excepto por el de entrada.



### Métodos de reenvío del switch

Los switches toman decisiones de reenvío de capa 2 de manera muy rápida, gracias al software ASIC - circuitos integrados para aplicaciones específicas. ASIC reduce el tiempo de manejo de paquetes dentro del dispositivo permitiendo que el dispositivo pueda manejar mayor cantidad de puertos sin disminuir su rendimiento.

Un switch de capa 2 usa uno de estos métodos para cambiar tramas:

* **Almacenamiento y reenvío de switching** - toma una decisión de reenvío en una trama después de haber recibido la trama completa y revisada para la detección de errores mediante un mecanismo matemático de verificación de errores conocido como Verificación por Redundancia Cíclica (Cyclic Redundancy Check, CRC). El intercambio por almacenamiento y envío es el método principal de switching LAN de Cisco.
* **Método de corte** - inicia el proceso de reenvío una vez que se determinó la dirección MAC de destino de una trama entrante y se estableció el puerto de salida.

#### Almacenamiento y reenvío

El intercambio de almacenamiento y reenvío tiene las siguientes características:

* **Verificación de errores** - Después de recibir la trama completa en el puerto de entrada, el switch compara el valor de Secuencia de Verificación de Trama (Frame Check Sequence, FCS) en el último campo del datagrama con sus propios cálculos de FCS. FCS es un proceso de verificación de errores que contribuye a asegurar que la trama no contenga errores físicos ni de enlace de datos. Si la trama no posee errores, el switch la reenvía. De lo contrario, se descartan las tramas.
* **Almacenamiento en búfer automático** - El proceso de almacenamiento en buffer que usan los switches de almacenamiento y envío proporciona la flexibilidad para admitir cualquier combinación de velocidades de Ethernet. Por ejemplo, manejar una trama entrante que viaja a un puerto Ethernet de 100 Mbps que debe enviarse a una interfaz de 1 Gbps, requeriría utilizar el método de almacenamiento y reenvío. Ante cualquier incompatibilidad de las velocidades de los puertos de entrada y salida, el switch almacena la trama completa en un buffer, calcula la verificación de FCS, la reenvía al buffer del puerto de salida y después la envía.

#### Switching por método de corte

Si el método de switching de almacenamiento y reenvío elimina las tramas que no pasan la comprobación FCS, no reenviando las tramas no válidas - el método de corte si puede reenviar tramas no válidas, ya que no realizan la verificación de FCS. Sin embargo, el switching de corte tiene la capacidad de realizar un cambio de trama rápida. Esto significa que los switches que usan el método de corte pueden tomar una decisión de reenvío tan pronto como encuentren la dirección MAC de destino de la trama en la tabla de direcciones MAC.

El switch no tiene por qué esperar a que el resto de la trama ingrese al puerto de entrada antes de tomar la decisión de reenvío.

El **`switching libre de fragmentos`** es una vatiante del método de corte, en la que el switch solo comienza a reenviar la trama después de haber leído el campo **Tipo**. Este método proporciona una mejor verificación de errores que el método de corte, casi sin aumento de latencia.

Lógicamente, la velocidad de latencia más baja del **switching por corte** hace que resulte más adecuado para las aplicaciones mas demandantes de Tecnología Informática de Alto Rendimiento (High-Performance Computing, HPC).

El switching de corte puede reenviar tramas con errores. Si hay un índice de error alto (tramas no válidas) en la red, el switching por corte puede tener un impacto negativo en el ancho de banda, de esta forma, se obstruye el ancho de banda con las tramas dañadas y no válidas.<br>

Ejemplos:

<figure><img src="../../.gitbook/assets/image (933).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (934).png" alt=""><figcaption></figcaption></figure>

