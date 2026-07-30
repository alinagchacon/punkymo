---
description: Tomado de CCNA
---

# Dominios de colisión

Cuando se utilizaban hubs en segmentos Ethernet, los dispositivos de red tenían que competir por el medio compartido. Los segmentos de red que comparten el mismo ancho de banda entre diferentes dispositivos se conocen como dominios de colisión.&#x20;

Cuandos dos o más dispositivos del mismo dominio de colisión tratan de comunicarse simultáneamente, se produce una colisión.

Si un puerto Ethernet de un switch funciona en modo semi duplex, cada segmento está en su propio dominio de colisión. En puertos del switch funcionando en modo duplex completo no se generan dominios de colisión.

No obstante, de modo predeterminado los puertos Ethernet de un switch van a negociar el full duplex cuando ven que el dispositivo adyacente también puede funcionar de esta manera. &#x20;



### Dominios de difusión

Cuando hay varios switches interconectados se forma un dominio de difusión simple. Únicamente los dispositivos de capa de red como los routers, pueden dividir un dominio de difusión de capa 2. Un router puede segmentar los dominios de difusión y también un dominio de colisión.

Si tenemos un dispositivo que desea enviar una difusión de la capa de enlace de datos (capa 2), se establece la MAC de destino de la trama solo en números uno binario. Cuando hablamos del dominio de difusión de capa 2 hablamos de dominio de difusión MAC que consta de todos los dispositivos de la LAN que reciben tramas de difusión de un host.

Si un switch recibe una trama de difusión, la reenvía por cada uno de sus puertos excepto por el puerto de entrada en el que se recibió la trama. Esto hace que cada dispositivo conectado al switch recibirá una copia de la trama y la procesa.

A veces son necesarias las difusiones porque se utilizan para localizar otros dispositivos y servicios de red (como el DHCP), pero también reducen la eficacia de la red porque el ancho de banda es lo que se utiliza para propagar el tráfico de difusión. Con lo cual, si hay demasiadas difusiones y una carga de tráfico intensa en una red, se producirá una congestión y se reducirá el rendimiento de la red.

Igualmente, si tenemos dos switches conectados entre sí, se aumenta el dominio de difusión.



### Alivio de la congestión en la red

Los switches LAN tienen características que los hace eficaces para aliviar la congestión. De modo predeterminado, los puertos interconectados tratan de establecer un enlace full duplex y basta esto para eliminar los dominios de colisión.

Como los switches interconectan segmentos de redes LAN, utilizan la tabla de direcciones MAC para determinar los puertos de salida y así pueden reducir o eliminar las colisiones.&#x20;

Algunas características de los switches que sirven para aliviar la congestión de la red son:

* **Velocidad de puertos rápida** - las velocidades de los puertos varían según el modelo y propósito.&#x20;
  * &#x20;La mayoría de los switches de capa de acceso admiten velocidades de puertos de 100 Mbps y 1 Gbps.&#x20;
  * En el caso de los switches de distribución, éstos admiten velocidades de puerto de 100 Mbps, 1 Gbps y 10 Gbps.
  * Los switches de nivel central y de centros de datos admiten mayores velocidades de puertos: 10Gbps, 40 Gbps y 100 Gbps.
* **Cambio interno rápido** - los switches usan un bus interno rápido o memoria compartida para proporcionar un alto rendimiento.
* **Buffer de trama grande**: los switches utilizan búferes de memoria grande para almacenar temporalmente más tramas recibidas antes de tener que descartarlas, lo que permite que el tráfico de entrada desde un puerto más rápido, por ejemplo, 1 Gbps se reenvíe a un puerto de salida más lento, 100 Mbps, sin perder tramas.
* **Alta densidad de puertos** - un switch de alta densidad de puertos reduce los costes generales porque reduce el número de switches que se puedan necesitar. Por ejemplo: si se necesitan 96 puertos de acceso, es menos costoso comprar dos switches de 48 puertos que 4 switches de 24 puertos.

