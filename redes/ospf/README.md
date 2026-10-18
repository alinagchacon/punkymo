---
description: Material de Cisco Netacad
hidden: true
---

# 🚧 OSPF

**Funciones y características de OSPF**

Open Shortest Path First (OSPF) es un protocolo de enrutamiento de estado de enlace desarrollado como alternativa del protocolo RIP de enrutamiento por vector de distancias.

Este protocolo de enrutamiento dinámico presenta ventajas importantes en comparación con RIP, ya que ofrece una convergencia más rápida y escala a implementaciones de red mucho más grandes.

* OSPF es un protocolo de routing que utiliza el concepto de áreas para realizar la escalabilidad.
* Un enlace es una interfaz en un router.
* Un vínculo es también un segmento de red que conecta dos routers, o una red auxiliar, como una LAN Ethernet que está conectada a un único router.
* Toda la información del estado del vínculo incluye el prefijo de red, la longitud del prefijo y el costo.
* Todos los protocolos de enrutamiento utilizan mensajes de protocolo de enrutamiento para intercambiar información de ruta.
* Los mensajes contribuyen a armar estructuras de datos, que luego se procesan con un algoritmo de enrutamiento.
* Los routers que ejecutan mensajes de intercambio OSPF, utilizan cinco tipos de paquetes: paquete Hello, paquete de descripción de base de datos, paquete de solicitud de estado de enlace, paquete de actualización de estado de enlace y paquete de reconocimiento de estado de enlace.
* Los mensajes OSPF se utilizan para crear y mantener tres bases de datos OSPF:
  * la base de datos de adyacencia crea la tabla vecina,
  * la base de datos de estado de vínculo (LSDB) crea la tabla de topología y
  * la base de datos de reenvío crea la tabla de enrutamiento.
* El router arma la tabla de topología; para ello, utiliza los resultados de cálculos realizados a partir del algoritmo SPF (Primero la ruta más corta) de Dijkstra. El algoritmo SPF se basa en el costo acumulado para llegar a un destino.
* En OSPF, el costo se utiliza para determinar la mejor ruta al destino. A fin de mantener la información de enrutamiento, los routers OSPF realizan el siguiente proceso genérico de routing de estado de enlace para alcanzar un estado de convergencia:
  1. Establecimiento de adyacencias de vecinos
  2. Intercambio de anuncios de estado de enlace
  3. Crear la base de datos de estado de vínculo
  4. Ejecución del algoritmo SPF
  5. Elegit la mejor ruta

Con `OSPF de área única` se puede usar cualquier número para el área, la mejor práctica es usar el área 0, siendo útil en redes más pequeñas con pocos routers.

Con `OSPF multiárea`, se puede dividir un dominio de enrutamiento grande en áreas más pequeñas a fin de admitir el enrutamiento jerárquico. El enrutamiento todavía ocurre entre las áreas (enrutamiento entre áreas), mientras que muchas de las operaciones de enrutamiento que son intensivas para el procesador, como el recálculo de la base de datos, se mantienen dentro de un área.

OSPFv3 es el equivalente a OSPFv2 para intercambiar prefijos IPv6.

{% hint style="info" %}
Recuerda que, en IPv6, la dirección de red se denomina “prefijo” y la máscara de subred se denomina “longitud de prefijo”.
{% endhint %}

**Paquetes OSPF**

OSPF utiliza los siguientes paquetes de estado de enlace (LSP) para establecer y mantener adyacencias vecinas e intercambiar actualizaciones de enrutamiento:

1. Hello
2. DBD
3. LSR
4. LSU
5. LSAck

Los paquetes LSU también se usan para reenviar actualizaciones de routing OSPF, como cambios de enlace. Los paquetes de hello se utilizan para:

* Descubrir vecinos OSPF y establecer adyacencias de vecinos.
* Anunciar parámetros en los que dos routers deben acordar convertirse en vecinos.
* Elige el router designado (DR) y el router designado de respaldo (BDR) en redes multiacceso, como Ethernet. Los enlaces punto a punto no requieren DR o BDR.

Algunos campos importantes del paquete Hello son tipo, router ID, ID de área, máscara de red, intervalo de saludo, prioridad del router, intervalo dead, DR, BDR y lista de vecinos.

**Operaciones OSPF**

Cuando un router OSPF se conecta inicialmente a una red, intenta hacer lo siguiente:

* Crear adyacencias con los vecinos
* Intercambiar información de enrutamiento
* Calcular las mejores rutas
* Lograr la convergencia

Los estados por los que OSPF pasan son:

* down
* Init
* Two-Way
* ExStart
* Exchange
* Loading
* Full

Cuando OSPF está habilitado en una interfaz, el router debe determinar:

* si hay otro vecino OSPF en el vínculo enviando un paquete Hello que contenga su router ID fuera de todas las interfaces habilitadas para OSPF.
* El paquete Hello se envía a todos los routers OSPF por la dirección de multicast reservada IPv4 224.0.0.5.
* Sólo los routers OSPFv2 procesarán estos paquetes.
* Cuando un router vecino con OSPF habilitado recibe un paquete Hello con un router ID que no figura en su lista de vecinos, el router receptor intenta establecer una adyacencia con el router que inició la comunicación.
* Después del estado Two-Way, los routers pasan a los estados de sincronización de bases de datos, el cual es un proceso de tres pasos:
  * Decidir el primer router
  * Intercambio DBD
  * Enviar un LSR

Las redes de `accesos múltiples` pueden suponer dos desafíos para OSPF en relación con la saturación con LSA: **la creación de varias adyacencias y la saturación intensa con LSA**.

Un aumento espectacular en el número de routers también aumenta drásticamente el número de LSA intercambiados entre los routers. Esta inundación de LSA repercute significativamente en el funcionamiento de OSPF. Si cada router en una red multiacceso tuviera que saturar y reconocer todas las LSA recibidas, de todos los demás routers en la misma red multiacceso, el tráfico de la red se volvería bastante caótico. Esta es la razón por la cual las elecciones de DR y BDR son necesarias. En las redes multiacceso, OSPF elige un DR como punto de recolección y distribución de las LSA enviadas y recibidas. También se elige un BDR en caso de que falle el DR.

### Configurar explícitamente un router ID

```
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# end
R1# show ip protocols | include Router ID
```

### Modificación del router ID

```
R1# show ip protocols | include Router ID
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# end
R1# clear ip ospf process
```

### Configure el OSPF utilizando el comando network

<pre><code><strong>R1(config)# router ospf 10
</strong>R1(config-router)# network 10.10.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.4 0.0.0.3 area 0
R1(config-router)# network 10.1.1.12 0.0.0.3 area 0
R1(config-router)#
</code></pre>

### Configure OSPF utilizando el comando ip ospf

```
R1(config-router)# interface GigabitEthernet 0/0/0
R1(config-if)# ip ospf 10 area 0
R1(config-if)# interface GigabitEthernet 0/0/1
R1(config-if)# ip ospf 10 area 0
R1(config-if)# interface Loopback 0
R1(config-if)# ip ospf 10 area 0
R1(config-if)#
```

### Configuración de interfaces pasivas

```
R1(config)# router ospf 10
R1(config-router)# passive-interface loopback 0
R1(config-router)# end
```

### Syntax Checker - Configure interfaces pasivas en R2 y R3

Todas las interfaces se pueden volver pasivas utilizando passive-interface default

```
R2(config-router)#passive-interface lo0
```

### Redes punto a punto OSPF

```
R1# show ip ospf interface GigabitEthernet 0/0/0
R2# show ip route | include 10.10.1
```

Para simular una LAN real, la interfaz Loopback 0 se configura como una red punto a punto para que R1 anuncie la red 10.10.1.0/24 completa a R2 y R3.

```
R1(config-if)# interface Loopback 0
R1(config-if)# ip ospf network point-to-point
```

## Tipos de redes OSPF

Las redes OSPF multiacceso son únicas, ya que un router controla la distribución de los LSA. El router elegido para este rol debe ser determinado por el administrador de red a través de la configuración adecuada. OSPF puede incluir procesos adicionales dependiendo del tipo de red.

### Router designado OSPF

Recordemos que, en redes multiacceso, OSPF elige una DR y BDR como solución para administrar el número de adyacencias y la inundación de anuncios de estado de enlace (LSA). El DR es responsable de recolectar y distribuir los LSA enviados y recibidos. El DR usa la dirección IPv4 multicast 224.0.0.5 que está destinada a todos los routers OSPF.

También se elige un BDR en caso de que falle el DR. El BDR escucha pasivamente y mantiene una relación con todos los routers. Si el DR deja de producir paquetes Hello, el BDR se asciende a sí mismo y asume la función de DR.

Todos los demás routers se convierten en DROTHER (un router que no es DR ni BDR). Los DROTHER utilizan la dirección de acceso múltiple 224.0.0.6 (todos los routers designados) para enviar paquetes OSPF al DR y al BDR. Sólo DR y BDR escuchan 224.0.0.6.

En la figura, R1, R5 y R4 son DROTHERs. Haga clic en reproducir para ver la animación de R2 actuando como DR. Observe que sólo el DR y el BDR procesan el LSA enviado por R1 utilizando la dirección multicast IPv4 224.0.0.6. A continuación, el DR envía el LSA a todos los routers OSPF utilizando la dirección multicast IPv4 224.0.0.5.

Debido a que los routers están conectados por medio de una red multiacceso con difusión común, OSPF seleccionó automáticamente un DR y un BDR. En este ejemplo, se eligió al R3 como el DR porque la ID del router es 3.3.3.3, que es la más alta en la red. El R2 es el BDR porque tiene la segunda ID del router más alta en la red.

FULL/DROTHER - Este es un router DR o BDR que está completamente adyacente con un router que no sea DR o BDR. Estos dos vecinos pueden intercambiar paquetes Hello, actualizaciones, consultas, respuestas y acuses de recibo.\
FULL/DR - El router está completamente adyacente con el vecino DR indicado. Estos dos vecinos pueden intercambiar paquetes Hello, actualizaciones, consultas, respuestas y acuses de recibo.\
FULL/BDR - El router es completamente adyacente con el vecino BDR indicado. Estos dos vecinos pueden intercambiar paquetes Hello, actualizaciones, consultas, respuestas y acuses de recibo.\
2-WAY/DROTHER - El router que no es DR o BDR tiene una relación vecina con otro router no DR o BDR. Estos dos vecinos intercambian paquetes Hello.\
El estado normal de un router OSPF suele ser COMPLETO. Si un router está atascado en otro estado, es un indicio de que existen problemas en la formación de adyacencias. La única excepción a esto es el estado 2-WAY, que es normal en una red broadcast multiacceso. Por ejemplo, los DROTHERs formarán una adyacencia vecina de 2-Way con cualquier DROTHER que se una a la red. Cuando esto sucede, el estado vecino se muestra como 2-WAY/DROTHER.

### Proceso de elección del DR/BDR predeterminado

¿Cómo se eligen el DR y el BDR? La decisión de elección del DR y el BDR OSPF se hace según los siguientes criterios, en orden secuencial:

Los routers en la red seleccionan como DR al router con la prioridad de interfaz más alta. El router con la segunda prioridad de interfaz más alta se elige como BDR. La prioridad puede configurarse para que sea cualquier número entre 0 y 255. Si el valor de prioridad de la interfaz se establece en 0, esa interfaz no se puede elegir como DR ni BDR. La prioridad predeterminada de las interfaces broadcast de acceso múltiple es 1. Por lo tanto, a menos que se configuren de otra manera, todos los routers tienen un mismo valor de prioridad y deben depender de otro método de diferenciación durante la elección del DR/BDR.\
Si las prioridades de interfaz son iguales, se elige al router con la ID más alta como DR. El router con la segunda ID más alta es el BDR.\
Recuerde que el router ID se determina de una de las siguientes tres maneras:

El router ID se puede configurar manualmente.\
Si no hay un router ID configurado, la dirección IPv4 de loopback más alta determina el router ID.\
Si no hay interfaces de loopback configuradas, el router ID lo determina la dirección IPv4 activa más alta.

¿Cómo se eligen el DR y el BDR? La decisión de elección del DR y el BDR OSPF se hace según los siguientes criterios, en orden secuencial:

Los routers en la red seleccionan como DR al router con la prioridad de interfaz más alta. El router con la segunda prioridad de interfaz más alta se elige como BDR. La prioridad puede configurarse para que sea cualquier número entre 0 y 255. Si el valor de prioridad de la interfaz se establece en 0, esa interfaz no se puede elegir como DR ni BDR. La prioridad predeterminada de las interfaces broadcast de acceso múltiple es 1. Por lo tanto, a menos que se configuren de otra manera, todos los routers tienen un mismo valor de prioridad y deben depender de otro método de diferenciación durante la elección del DR/BDR.

\
Si las prioridades de interfaz son iguales, se elige al router con la ID más alta como DR. El router con la segunda ID más alta es el BDR.

Recuerde que el router ID se determina de una de las siguientes tres maneras:

El router ID se puede configurar manualmente.\
Si no hay un router ID configurado, la dirección IPv4 de loopback más alta determina el router ID.\
Si no hay interfaces de loopback configuradas, el router ID lo determina la dirección IPv4 activa más alta.

```
R1(config)# interface GigabitEthernet 0/0/0
R1(config-if)# ip ospf priority 255
R1(config-if)# end
R1#
R1# clear ip ospf process
R1# show ip ospf interface GigabitEthernet 0/0/0
```

{% hint style="info" %}
Es posible configurar el router ID para manipular la elección de DR/BDR. Sin embargo, el proceso solo funciona si hay un plan riguroso para establecer el router ID de todos los routers. Configurar el router ID puede ayudar a controlar esto. Sin embargo, en redes grandes esto puede ser engorroso.

En vez de depender del router ID, es mejor controlar la elección mediante el establecimiento de prioridades de interfaz. Esto también permite que un router sea el DR en una red y un DROTHER en otra.&#x20;

Para establecer la prioridad de una interfaz, utilizamos el comando:&#x20;

**ip ospf priority** _value_, donde value es 0 a 255.&#x20;

Un valor de 0 no se convierte en DR o BDR. Un valor de 1 a 255 en la interfaz hace más probable que el router se convierta en DR o BDR.
{% endhint %}

#### Configurar la prioridad OSPF

En la topología, el comando **ip ospf priority** se utiliza para cambiar el DR y el BDR de la siguiente manera:

* El R1 debe ser el DR y se configura con una prioridad de 255.
* El R2 debe ser el BDR y se le deja la prioridad predeterminada de 1.
* El R3 nunca debe ser un DR ni BDR y se configura con una prioridad de 0.

Cambiamos la prioridad de la interfaz R1 G0/0/0 de 1 a 255.

<pre><code><strong>R1(config)# interface GigabitEthernet 0/0/0R1(config-if)# ip ospf priority 255R1(config-if)# endR1#
</strong></code></pre>

Cambiamos la prioridad de la interfaz R3 G0/0/0 de 1 a 0.

<pre><code><strong>R3(config)# interface GigabitEthernet 0/0/0R3(config-if)# ip ospf priority 0R3(config-if)# endR3#
</strong></code></pre>

El siguiente ejemplo muestra cómo borrar el proceso OSPF en R1. El comando **clear ip ospf process**  también debe introducirse en R2 y R3. Observa la información de estado OSPF que se genera.

<pre><code><strong>R1# clear ip ospf processReset ALL OSPF processes? [no]: yR1#*Jun  5 03:47:41.563: %OSPF-5-ADJCHG: Process 10, Nbr 2.2.2.2 on GigabitEthernet0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached*Jun  5 03:47:41.563: %OSPF-5-ADJCHG: Process 10, Nbr 3.3.3.3 on GigabitEthernet0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached*Jun  5 03:47:41.569: %OSPF-5-ADJCHG: Process 10, Nbr 2.2.2.2 on GigabitEthernet0/0/0 from LOADING to FULL, Loading Done*Jun  5 03:47:41.569: %OSPF-5-ADJCHG: Process 10, Nbr 3.3.3.3 on GigabitEthernet0/0/0 from LOADING to FULL, Loading Done
</strong></code></pre>

El resultado del comando **show in ospf interface g0/0/0** en R1 confirma que R1 es ahora el DR con una prioridad de 255 e identifica las nuevas adyacencias vecinas de R1.

<pre><code><strong>R1# show ip ospf interface GigabitEthernet 0/0/0GigabitEthernet0/0/0 is up, line protocol is up  Internet Address 192.168.1.1/24, Area 0, Attached via Interface Enable  Process ID 10, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1  Topology-MTID    Cost    Disabled    Shutdown      Topology Name        0           1         no          no            Base  Enabled by interface config, including secondary ip addresses  Transmit Delay is 1 sec, State DR, Priority 255  Designated Router (ID) 1.1.1.1, Interface address 192.168.1.1  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.1.2  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5    oob-resync timeout 40    Hello due in 00:00:00  Supports Link-local Signaling (LLS)  Cisco NSF helper support enabled  IETF NSF helper support enabled  Index 1/1/1, flood queue length 0  Next 0x0(0)/0x0(0)/0x0(0)  Last flood scan length is 1, maximum is 2  Last flood scan time is 0 msec, maximum is 1 msec  Neighbor Count is 2, Adjacent neighbor count is 2    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)    Adjacent with neighbor 3.3.3.3  Suppress hello for 0 neighbor(s)R1#
</strong></code></pre>

### Intervalos de los paquetes Hello

Como se muestra en la figura, los paquetes hello OSPFv2 se transmiten a la dirección de multicast 224.0.0.5 (todos los routers OSPF) cada 10 segundos. Este es el valor predeterminado del temporizador en redes multiacceso y punto a punto.

{% hint style="info" %}
**Nota:** Los paquetes de saludo no se envían en las interfaces LAN simuladas porque esas interfaces se configuraron como pasivas mediante el comando **passive-interface** de configuración del router.
{% endhint %}

El intervalo Dead es el período que el router espera para recibir un paquete Hello antes de declarar al vecino como inactivo. Si el intervalo Dead caduca antes de que los routers reciban un paquete Hello, OSPF elimina ese vecino de su base de datos (LSDB). El router satura la LSDB con información acerca del vecino inactivo por todas las interfaces con OSPF habilitado. Cisco usa un valor predeterminado de 4 veces el intervalo Hello. Esto es 40 segundos en redes de acceso múltiple y punto a punto.

{% hint style="info" %}
**Nota:** En redes de acceso múltiple non-broadcast (NBMA), el intervalo Hello predeterminado es 30 segundos y el intervalo indefinido predeterminado es 120 segundos. Las redes NBMA están fuera del alcance de este módulo.
{% endhint %}

#### Redes punto a punto OSPF

Los routers Cisco, de modo predeterminado, eligen DR y BDR en las interfaces Ethernet, incluso si solo hay otro dispositivo en el enlace.&#x20;

Podemos verificar esto con el  comando siguiente:&#x20;

```
show ip ospf interface 
```

<figure><img src="../../.gitbook/assets/image (955).png" alt=""><figcaption><p>Tomado de CCNA Cisco</p></figcaption></figure>

R1 es el BDR y R2 es el DR. El proceso de elección de DR/ BDR es innecesario ya que solo puede haber dos routers en la red punto a punto entre R1 y R2. Observe que en el resultado del router ha designado el tipo de red como Broadcast. Para cambiar esto a una red punto a punto, utilice el comando de configuración de interface, **ip ospf network point-to-point** en todas las interfaces en las que desee deshabilitar el proceso de elección DR/BDR. El siguiente ejemplo muestra esta configuración para R1. El estado de adyacencia del vecino OSPF disminuirá durante unos milisegundos.



#### Loopbacks y redes punto a punto

Se utilizan las interfaces de loopback para proporcionar interfaces adicionales para una variedad de propósitos. En este caso, estamos utilizando loopbacks para simular más redes de las que el equipo puede soportar. De forma predeterminada, las interfaces loopback se anuncian como rutas de host/32.&#x20;

Por ejemplo, R1 anunciaría la red 10.10.1.0/24 como 10.10.1.1/32 a R2 y R3.

<pre><code><strong>R2# show ip route | include 10.10.1O        10.10.1.1/32 [110/2] via 10.1.1.5, 00:03:05, GigabitEthernet0/0/0
</strong></code></pre>

Para simular una LAN real, la interfaz Loopback 0 se configura como una red punto a punto para que R1 anuncie la red 10.10.1.0/24 completa a R2 y R3.

<pre><code><strong>R1(config-if)# interface Loopback 0R1(config-if)# ip ospf network point-to-point
</strong></code></pre>

Ahora R2 recibe la dirección de red LAN más precisa y simulada de 10.10.1.0/24.

<pre><code><strong>R2# show ip route | include 10.10.1O        10.10.1.0/24 [110/2] via 10.1.1.5, 00:00:30, GigabitEthernet0/0/0
</strong></code></pre>



#### &#x20;Tipos de redes OSPF

Otro tipo de red que utiliza OSPF es la red OSPF multiacceso. Las redes OSPF multiacceso son únicas, ya que un router controla la distribución de los LSA. El router elegido para este rol debe ser determinado por el administrador de red a través de la configuración adecuada.

OSPF puede incluir procesos adicionales dependiendo del tipo de red.

En la sección anterior, la topología utilizaba vínculos punto a punto entre los routers. Sin embargo, los routers se pueden conectar al mismo switch para formar una red multiacceso, como se muestra en la figura.&#x20;

Las LAN Ethernet son el ejemplo más común de redes broadcast con multiacceso. En las redes broadcast, todos los dispositivos en la red pueden ver todas las tramas broadcast y multicast.

<figure><img src="../../.gitbook/assets/image (957).png" alt=""><figcaption><p>Tomado de CCNA Cisco</p></figcaption></figure>

#### Router designado OSPF

Como hemos visto en secciones anteriores, recordemos que en redes multiacceso, OSPF elige una DR y BDR como solución para administrar el número de adyacencias y la inundación de anuncios de estado de enlace (LSA). El DR es responsable de recolectar y distribuir los LSA enviados y recibidos. El DR usa la dirección IPv4 multicast 224.0.0.5 que está destinada a todos los routers OSPF.

También se elige un BDR en caso de que falle el DR. El BDR escucha pasivamente y mantiene una relación con todos los routers. Si el DR deja de producir paquetes Hello, el BDR se asciende a sí mismo y asume la función de DR.

Todos los demás routers se convierten en DROTHER (un router que no es DR ni BDR). Los DROTHER utilizan la dirección de acceso múltiple 224.0.0.6 (todos los routers designados) para enviar paquetes OSPF al DR y al BDR. Sólo DR y BDR escuchan 224.0.0.6.

#### Topología de referencia de acceso múltiple OSPF

En la topología de acceso múltiple que se muestra en la figura, hay tres routers interconectados a través de una red de acceso múltiple Ethernet común, 192.168.1.0/24. Cada router está configurado con la dirección IPv4 indicada en la interfaz Gigabit Ethernet 0/0/0.

Debido a que los routers están conectados por medio de una red multiacceso con difusión común, OSPF seleccionó automáticamente un DR y un BDR. En este ejemplo, se eligió al R3 como el DR porque la ID del router es 3.3.3.3, que es la más alta en la red. El R2 es el BDR porque tiene la segunda ID del router más alta en la red.

<figure><img src="../../.gitbook/assets/image (958).png" alt=""><figcaption></figcaption></figure>

**R1 DROTHER**

El resultado generado por R1 confirma que lo siguiente:

1. El R1 no es el DR ni el BDR, sino un DROTHER con una prioridad predeterminada de 1. (Línea 7)
2. El DR es el R3 con el router ID 3.3.3.3 en la dirección IPv4 192.168.1.3; el BDR es el R2 con el router ID 2.2.2.2 en la dirección IPv4 192.168.1.2.&#x20;
3. El R1 tiene dos adyacencias: una con el BDR y otra con el DR.&#x20;

<figure><img src="../../.gitbook/assets/image (959).png" alt=""><figcaption></figcaption></figure>

**R2 BDR**

El resultado generado por R2 confirma lo siguiente:

1. El R2 es el BDR, con una prioridad predeterminada de 1. (Line 7)
2. El DR es el R3 con el router ID 3.3.3.3 en la dirección IPv4 192.168.1.3; el BDR es el R2 con el router ID 2.2.2.2 en la dirección IPv4 192.168.1.2. (Líneas 8 y 9)
3. El R2 tiene dos adyacencias, una con un vecino que tiene el router ID 1.1.1.1 (R1) y la otra con el DR. (Lines 20-22)

<figure><img src="../../.gitbook/assets/image (960).png" alt=""><figcaption></figcaption></figure>

**R3 DR**

El resultado generado por R3 confirma lo siguiente:

1. El R3 es el DR, con una prioridad predeterminada de 1. (Line 7)
2. El DR es el R3 con el router ID 3.3.3.3 en la dirección IPv4 192.168.1.3; el BDR es el R2 con el router ID 2.2.2.2 en la dirección IPv4 192.168.1.2. (Lines 8 and 9)
3. El R3 tiene dos adyacencias, una con un vecino que tiene la ID de router 1.1.1.1 (R1) y la otra con el BDR. (Lines 20-22)

<figure><img src="../../.gitbook/assets/image (961).png" alt=""><figcaption></figcaption></figure>

