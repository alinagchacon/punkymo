---
description: OSPF
---

# Métrica de costos

### Métrica de costos OSPF de Cisco

El costo de Cisco de una interfaz es inversamente proporcional al ancho de banda de la interfaz. Por lo tanto, <mark style="color:violet;">cuanto mayor es el ancho de banda, menor es el costo</mark>. La fórmula que se usa para calcular el costo de OSPF es la siguiente:

```
Cost = reference bandwidth / interface bandwidth
```

El ancho de banda de referencia predeterminado es 10<sup>8</sup> (100,000,000); por lo tanto, la fórmula es:

```
Cost = 100,000,000 bps / interface bandwidth in bps
```

Debido a que el valor del costo OSPF debe ser un número entero, las interfaces FastEthernet, Gigabit Ethernet y 10 GigE comparten el mismo costo. Para corregir esta situación, se puede ajustar el ancho de banda de referencia con el comando siguiente  en cada router OSPF.

```
auto-cost reference-bandwidth 
```

<figure><img src="../../.gitbook/assets/image (964).png" alt=""><figcaption><p>Cálculo de costos - CCNA Cisco</p></figcaption></figure>

Para corregir esta situación, se puede:

* Ajustar el ancho de banda de referencia con el comando: **auto-cost reference-bandwidth** en cada router OSPF.
* Establecer manualmente el valor de costo OSPF con el comando **ip ospf cost**  en las interfaces necesarias.

### Ajustar el ancho de banda de referencia

El valor del costo debe ser un número entero. Si se calcula un valor menor que un número entero, OSPF redondea al número entero más cercano. Por lo tanto, el costo OSPF asignado a una interfaz Gigabit Ethernet con el ancho de banda de referencia predeterminado de 100.000.000 bps equivale a 1, porque el entero más cercano para 0.1 es 1 en lugar de 0.

```
Cost = 100,000,000 bps / 1,000,000,000 = 1
```

Por esta razón, todas las interfaces más rápidas que Fast Ethernet tendrán el mismo valor de costo de 1 que una interfaz Fast Ethernet. Para ayudar a OSPF a determinar la ruta correcta, se debe cambiar el ancho de banda de referencia a un valor superior a fin de admitir redes con enlaces más rápidos que 100 Mbps.

El cambio del ancho de banda de referencia en realidad no afecta la capacidad de ancho de banda en el enlace, sino que simplemente afecta el cálculo utilizado para determinar la métrica.&#x20;

Para ajustar el ancho de banda de referencia, usamos el comando de configuración del router **auto-cost reference-bandwidth** auto-cost reference-bandwidth Mb/s.

<pre><code><strong>Router(config-router)# auto-cost reference-bandwidth Mbps
</strong></code></pre>

Se debe configurar este comando en cada router en el dominio OSPF. Observa que el valor se expresa en Mbps, por lo tanto:

* Para ajustar los costos de Gigabit Ethernet, usamos el comando **auto-cost reference-bandwidth 1000.**&#x20;
* Para 10 Gigabit Ethernet, usamos el comando **auto-cost reference-bandwidth 10000.**
* Para volver al ancho de banda de referencia predeterminado usamos el comando **auto-cost reference-bandwidth 100** .

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
Aunque utilizar el comando **ip ospf cost**  es el método recomendado para manipular los valores de costo OSPF, un administrador también podría hacerlo mediante el comando: &#x20;

interface configuration **bandwidth** _kbps_&#x20;

Sin embargo, eso solo funcionaría si todos los routers son routers Cisco.
{% endhint %}



#### Costos acumulados de OSPF

El costo de una ruta de OSPF es el valor acumulado desde un router hasta la red de destino. Suponiendo que el comando:&#x20;

**auto-cost reference-bandwidth 10000**&#x20;

se haya configurado en los tres routers, el costo de los enlaces entre cada router es ahora 10. Las interfaces loopback tienen un costo predeterminado de 1, como se muestra en la figura.

<figure><img src="../../.gitbook/assets/image (965).png" alt=""><figcaption></figcaption></figure>

Por lo tanto, podemos calcular el costo de cada router para llegar a cada red. Por ejemplo, el costo total de R1 para alcanzar la red 10.10.2.0/24 es 11. Esto se debe a que el vínculo al costo R2 = 10 y el costo predeterminado del loopback = 1. 10 + 1 = 11.

La tabla de enrutamiento de R1 en la figura confirma que la métrica para llegar a la LAN de R2 es un costo de 11.

<figure><img src="../../.gitbook/assets/image (966).png" alt=""><figcaption></figcaption></figure>

#### Establecimiento manual de los valores de costo en OSPF

Los valores de costo OSPF se pueden manipular para influir en la ruta elegida por OSPF. Por ejemplo, en la configuración actual, R1 equilibra la carga a la red 10.1.1.8/30. Enviará algo de tráfico a R2 y algo de tráfico a R3. Puede ver esto en la tabla de enrutamiento.

<figure><img src="../../.gitbook/assets/image (967).png" alt=""><figcaption></figcaption></figure>

Es posible que el administrador desee que el tráfico pase a través de R2 y use R3 como una ruta de copia de seguridad en caso de que el enlace entre R1 y R2 se desconecte.

Otra razón para cambiar el valor de costo es porque otros proveedores pueden calcular OSPF de una manera diferente. Al manipular el valor de costo, el administrador puede asegurarse de que los costos de ruta compartidos entre los routers de varios proveedores OSPF se reflejen con precisión en las tablas de enrutamiento.

Para cambiar el valor de costo notificado por el router OSPF local a otros routeres OSPF, utiliza el comando de configuración de interfaz **ip ospf cost** _value_.&#x20;

En la figura, tenemos que cambiar el costo de las interfaces de loopback a 10 para simular velocidades Gigabit Ethernet. Además, cambiaremos el costo del enlace entre R2 y R3 a 30 para que este enlace se use como enlace de copia de seguridad.

<figure><img src="../../.gitbook/assets/image (968).png" alt=""><figcaption></figcaption></figure>

El siguiente ejemplo es la configuración de R1.

<pre><code><strong>R1(config)# interface g0/0/1
</strong><strong>R1(config-if)# ip ospf cost 30
</strong><strong>R1(config-if)# interface lo0
</strong><strong>R1(config-if)# ip ospf cost 10
</strong><strong>R1(config-if)# endR1#
</strong></code></pre>

Suponiendo que los costos OSPF para R2 y R3 se hayan configurado para que coincidan con la topología de la figura anterior, las rutas OSPF para R1 tendrían los siguientes valores de costo. Observe que R1 ya no equilibra la carga en la red 10.1.1.8/30. De hecho, todas las rutas pasan por R2 como desee el administrador de red.

<figure><img src="../../.gitbook/assets/image (969).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Aunque utilizar el comando **ip ospf cost**  es el método recomendado para manipular los valores de costo OSPF, un administrador también podría hacerlo mediante el comando interface configuration **bandwidth** _kbps._ Sin embargo, eso solo funcionaría si todos los routers son routers Cisco.
{% endhint %}

