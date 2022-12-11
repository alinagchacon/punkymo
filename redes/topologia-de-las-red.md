# Topología de las red

Nos referimos como topología de una red a la disposición (relación) de los dispositivos de red y las conexiones que se establecen entre los mismos. Hay dos tipos de topologías utilizadas al describir redes LAN y WAN: física y lógica. En estos caso podemos referirnos como mapa físico y lógico de la red.

**Topología física o mapa físico**

* Identifica las conexiones físicas y la manera de interconectar los dispositivos finales (PC, impresoras, etc.) y los dispositivos intermedios (routers, switch y puntos de acceso inalámbrico).&#x20;
* Incluyen la ubicación física de los dispositivos: habitación u oficina, ubicación en el rack, etc.. Las topologías físicas suelen ser punto a punto o estrella.

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption><p><strong>Tomado de</strong> <a href="https://ccnadesdecero.es"><strong>https://ccnadesdecero.es</strong></a><strong></strong></p></figcaption></figure>

**Topología lógica o mapa lógico**

* Se refiere a la forma en que una red transfiere los datos de un nodo a otro.&#x20;
* Identifica conexiones virtuales mediante interfaces de dispositivo y esquemas de direccionamiento IP de capa 3 (capa de red del modelo OSI).
* La capa de enlace de datos (capa 2 del modelo OSI) accede a la topología lógica de una red al controlar el acceso de datos a los medios. Es la topología lógica que influye en el tipo de trama de red y control de acceso a medios utilizado.

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>Tomado de</strong> <a href="https://ccnadesdecero.es"><strong>https://ccnadesdecero.es</strong></a><strong></strong></p></figcaption></figure>

### Topología en redes WAN

Veremos tres topologías físicas: punto a punto, estrella y malla.

**Punto a punto**&#x20;

* la más simple y común, pues se trata de un enlace permanente entre dos puntos finales.&#x20;
* utiliza el protocolo PPP,&#x20;
* los protocolos de enlace de datos lógicos pueden ser muy simples, ya que todas las tramas en los medios solo pueden viajar hacia o desde los dos nodos.&#x20;

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption><p>Enlace punto a punto</p></figcaption></figure>

![](<../.gitbook/assets/image (145).png>)

**Estrella**

Un sitio central interconecta a las sucursales mediante  enlaces punto a punto. Las sucursales no pueden intercambiar información con otras sucursales sin pasar por el nodo central.

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption><p>Enlace en estrella</p></figcaption></figure>

**Malla**

Proporciona alta disponibilidad, pero requiere que cada sistema final esté interconectado con cualquier otro sistema. Por lo que, los costes  económicos pueden ser elevados. Cada enlace es esencialmente un enlace punto a punto al otro nodo.



<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption><p>Enlace en malla, aunque en este caso sería malla parcial</p></figcaption></figure>



### Topologías en redes LAN

Las llamadas topologías bus y anillo son de las primeras tecnologías LAN Ethernet y Token Ring heredadas:

**Bus**

* todos los nodos están encadenados entre sí y con un "tope" en cada extremo.&#x20;
* No se requiere de switch para interconectar los dispositivos finales.&#x20;
* A menudo usaban cable coaxial por lo económico y fácil de configurar.

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption><p>Bus</p></figcaption></figure>

**Anillo**

* los nodos se conectan a sus respectivos vecinos formando un anillo.&#x20;
* El anillo no necesita ser terminado, a diferencia de la topología del bus.&#x20;
* La interfaz de datos distribuidos de fibra heredada (FDDI) y las redes Token Ring usaban topologías de anillo.

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p>Anillo</p></figcaption></figure>

#### Estrella

* Los nodos se conectan a un punto central, un dispositivo intermedio central, switch o conmutador Ethernet.&#x20;
* Una estrella extendida extiende esta topología al interconectar múltiples conmutadores Ethernet.&#x20;
* Las topologías en estrella y extendidas son fáciles de instalar, muy escalables  y fáciles de solucionar.

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption><p>Estrella</p></figcaption></figure>

