---
description: Material de Cisco Netacad
---

# 🚧 OSPF

**Funciones y características de OSPF**

Open Shortest Path First (OSPF) es un protocolo de enrutamiento de estado de enlace desarrollado como alternativa del protocolo RIP de enrutamiento por vector de distancias.&#x20;

Este protocolo de enrutamiento dinámico presenta ventajas importantes en comparación con RIP, ya que ofrece una convergencia más rápida y escala a implementaciones de red mucho más grandes.&#x20;

* OSPF es un protocolo de routing que utiliza el concepto de áreas para realizar la escalabilidad.&#x20;
* Un enlace es una interfaz en un router.&#x20;
* Un vínculo es también un segmento de red que conecta dos routers, o una red auxiliar, como una LAN Ethernet que está conectada a un único router.&#x20;
* Toda la información del estado del vínculo incluye el prefijo de red, la longitud del prefijo y el costo.&#x20;
* Todos los protocolos de enrutamiento utilizan mensajes de protocolo de enrutamiento para intercambiar información de ruta.
* Los mensajes contribuyen a armar estructuras de datos, que luego se procesan con un algoritmo de enrutamiento.&#x20;
* Los routers que ejecutan mensajes de intercambio OSPF, utilizan cinco tipos de paquetes: paquete Hello, paquete de descripción de base de datos, paquete de solicitud de estado de enlace, paquete de actualización de estado de enlace y paquete de reconocimiento de estado de enlace.&#x20;
* Los mensajes OSPF se utilizan para crear y mantener tres bases de datos OSPF:&#x20;
  * la base de datos de adyacencia crea la tabla vecina,&#x20;
  * la base de datos de estado de vínculo (LSDB) crea la tabla de topología y&#x20;
  * la base de datos de reenvío crea la tabla de enrutamiento.&#x20;
* El router arma la tabla de topología; para ello, utiliza los resultados de cálculos realizados a partir del algoritmo SPF (Primero la ruta más corta) de Dijkstra. El algoritmo SPF se basa en el costo acumulado para llegar a un destino.&#x20;
* En OSPF, el costo se utiliza para determinar la mejor ruta al destino. A fin de mantener la información de enrutamiento, los routers OSPF realizan el siguiente proceso genérico de routing de estado de enlace para alcanzar un estado de convergencia:
  1. Establecimiento de adyacencias de vecinos
  2. Intercambio de anuncios de estado de enlace
  3. Crear la base de datos de estado de vínculo
  4. Ejecución del algoritmo SPF
  5. Elija la mejor ruta

Con `OSPF de área única` se puede usar cualquier número para el área, la mejor práctica es usar el área 0, siendo útil en redes más pequeñas con pocos routers.&#x20;

Con `OSPF multiárea`, se puede dividir un dominio de enrutamiento grande en áreas más pequeñas a fin de admitir el enrutamiento jerárquico. El enrutamiento todavía ocurre entre las áreas (enrutamiento entre áreas), mientras que muchas de las operaciones de enrutamiento que son intensivas para el procesador, como el recálculo de la base de datos, se mantienen dentro de un área.&#x20;

OSPFv3 es el equivalente a OSPFv2 para intercambiar prefijos IPv6.&#x20;

{% hint style="info" %}
Recuerde que, en IPv6, la dirección de red se denomina “prefijo” y la máscara de subred se denomina “longitud de prefijo”.
{% endhint %}

**Paquetes OSPF**

OSPF utiliza los siguientes paquetes de estado de enlace (LSP) para establecer y mantener adyacencias vecinas e intercambiar actualizaciones de enrutamiento:&#x20;

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

Los estados por los que OSPF pasan son:&#x20;

* down
* Init
* Two-Way
* ExStart
* Exchange
* Loading
* Full

Cuando OSPF está habilitado en una interfaz, el router debe determinar:&#x20;

* si hay otro vecino OSPF en el vínculo enviando un paquete Hello que contenga su router ID fuera de todas las interfaces habilitadas para OSPF.&#x20;
* El paquete Hello se envía a todos los routers OSPF por la dirección de multicast reservada IPv4 224.0.0.5.&#x20;
* Sólo los routers OSPFv2 procesarán estos paquetes.&#x20;
* Cuando un router vecino con OSPF habilitado recibe un paquete Hello con un router ID que no figura en su lista de vecinos, el router receptor intenta establecer una adyacencia con el router que inició la comunicación.&#x20;
* Después del estado Two-Way, los routers pasan a los estados de sincronización de bases de datos, el cual es un proceso de tres pasos:
  * Decidir el primer router
  * Intercambio DBD
  * Enviar un LSR

Las redes de `accesos múltiples` pueden suponer dos desafíos para OSPF en relación con la saturación con LSA: **la creación de varias adyacencias y la saturación intensa con LSA**.&#x20;

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

Los routers en la red seleccionan como DR al router con la prioridad de interfaz más alta. El router con la segunda prioridad de interfaz más alta se elige como BDR. La prioridad puede configurarse para que sea cualquier número entre 0 y 255. Si el valor de prioridad de la interfaz se establece en 0, esa interfaz no se puede elegir como DR ni BDR. La prioridad predeterminada de las interfaces broadcast de acceso múltiple es 1. Por lo tanto, a menos que se configuren de otra manera, todos los routers tienen un mismo valor de prioridad y deben depender de otro método de diferenciación durante la elección del DR/BDR.\
Si las prioridades de interfaz son iguales, se elige al router con la ID más alta como DR. El router con la segunda ID más alta es el BDR.\
Recuerde que el router ID se determina de una de las siguientes tres maneras:

El router ID se puede configurar manualmente.\
Si no hay un router ID configurado, la dirección IPv4 de loopback más alta determina el router ID.\
Si no hay interfaces de loopback configuradas, el router ID lo determina la dirección IPv4 activa más alta.

R1(config)# interface GigabitEthernet 0/0/0\
R1(config-if)# ip ospf priority 255\
R1(config-if)# end\
R1#

R1# clear ip ospf process

R1# show ip ospf interface GigabitEthernet 0/0/0

### Métrica de costos OSPF de Cisco

El costo de Cisco de una interfaz es inversamente proporcional al ancho de banda de la interfaz. Por lo tanto, cuanto mayor es el ancho de banda, menor es el costo. La fórmula que se usa para calcular el costo de OSPF es la siguiente:

Cost = reference bandwidth / interface bandwidth

El ancho de banda de referencia predeterminado es 108 (100,000,000); por lo tanto, la fórmula es:

Cost = 100,000,000 bps / interface bandwidth in bps

Debido a que el valor del costo OSPF debe ser un número entero, las interfaces FastEthernet, Gigabit Ethernet y 10 GigE comparten el mismo costo. Para corregir esta situación, puede:

Ajuste el ancho de banda de referencia con el auto-cost reference-bandwidth comando en cada router OSPF.\


### Ajuste el ancho de banda de referencia

El valor del costo debe ser un número entero. Si se calcula un valor menor que un número entero, OSPF redondea al número entero más cercano. Por lo tanto, el costo OSPF asignado a una interfaz Gigabit Ethernet con el ancho de banda de referencia predeterminado de 100.000.000 bps equivaldría a 1, porque el entero más cercano para 0.1 es 0 en lugar de 1.

**Cost = 100,000,000 bps / 1,000,000,000 = 1**

Por esta razón, todas las interfaces más rápidas que Fast Ethernet tendrán el mismo valor de costo de 1 que una interfaz Fast Ethernet. Para ayudar a OSPF a determinar la ruta correcta, se debe cambiar el ancho de banda de referencia a un valor superior a fin de admitir redes con enlaces más rápidos que 100 Mbps.

El cambio del ancho de banda de referencia en realidad no afecta la capacidad de ancho de banda en el enlace, sino que simplemente afecta el cálculo utilizado para determinar la métrica. Para ajustar el ancho de banda de referencia, use el comando de configuración del router **auto-cost reference-bandwidth** auto-cost reference-bandwidth Mb/s.

<pre><code><strong>Router(config-router)# auto-cost reference-bandwidth Mbps
</strong></code></pre>

Se debe configurar este comando en cada router en el dominio OSPF. Observe que el valor se expresa en Mbps; por lo tanto, para ajustar los costos de Gigabit Ethernet, use el comando **auto-cost reference-bandwidth 1000.** For 10 Gigabit Ethernet, use el comando **auto-cost reference-bandwidth 10000.**

Para volver al ancho de banda de referencia predeterminado **auto-cost reference-bandwidth 100** use el comando.

```
R1(config)# interface g0/0/1
R1(config-if)# ip ospf cost 30
R1(config-if)# interface lo0
R1(config-if)# ip ospf cost 10
R1(config-if)# end
R1#

R1# show ip route ospf | begin 10
```

{% hint style="info" %}
**Nota:** Aunque utilizar el **ip ospf cost** comando es el método recomendado para manipular los valores de costo OSPF, un administrador también podría hacerlo mediante el comando interface configuration **bandwidth** _kbps_ Sin embargo, eso solo funcionaría si todos los routers son routers Cisco.\

{% endhint %}



### Intervalos de los paquetes Hello

Como se muestra en la figura, los paquetes hello OSPFv2 se transmiten a la dirección de multicast 224.0.0.5 (todos los routers OSPF) cada 10 segundos. Este es el valor predeterminado del temporizador en redes multiacceso y punto a punto.

{% hint style="info" %}
**Nota:** Los paquetes de saludono se envían en las interfaces LAN simuladas porque esas interfaces se configuraron como pasivas mediante el comando **passive-interface** de configuración del router.
{% endhint %}

El intervalo Dead es el período que el router espera para recibir un paquete Hello antes de declarar al vecino como inactivo. Si el intervalo Dead caduca antes de que los routers reciban un paquete Hello, OSPF elimina ese vecino de su base de datos (LSDB). El router satura la LSDB con información acerca del vecino inactivo por todas las interfaces con OSPF habilitado. Cisco usa un valor predeterminado de 4 veces el intervalo Hello. Esto es 40 segundos en redes de acceso múltiple y punto a punto.

{% hint style="info" %}
**Nota:** En redes de acceso múltiple non-broadcast (NBMA), el intervalo Hello predeterminado es 30 segundos y el intervalo indefinido predeterminado es 120 segundos. Las redes NBMA están fuera del alcance de este módulo.
{% endhint %}

