---
description: Routing & Switching
---

# Enrutamiento estático

El enrutamiento es fundamental en las redes, porque es el modo de enviar  la información a través de las redes, de una red origen a una red destino, por tanto, los routers son los dispositivos encargados de transferir los paquetes de una red a otra.

Los routers tienen la posibilidad de descubrir las redes lejanas de manera dinámica, utilizando para ello:&#x20;

* protocolos de enrutamiento.
* rutas estáticas.&#x20;

Puede darse el caso que los routers utilicen combinaciones de protocolos de enrutamiento dinámico y estático.  Estas últimas son bastante comunes y no necesitan tanto procesamiento como los protocolos de enrutamiento dinámico.

Un router puede descubrir redes remotas de modo manual o dinámica:

* **Manual**: A través de las rutas estáticas se introducen las redes remotas  en la tabla de enrutamiento.
* **Dinámica**: de forma automática se descubren las rutas remotas utilizando protocolos de enrutamiento dinámico.



<figure><img src="../../.gitbook/assets/image (433).png" alt="" width="563"><figcaption><p>Enrutamiento estático</p></figcaption></figure>

Algunas ventajas del enrutamiento estático serían:

* No se anuncian a través de la red, brindando mayor seguridad.
* Consumen menos ancho de banda que los protocolos de enrutamiento dinámico dado que no se utiliza la CPU para el cálculo de las rutas.

Algunas de las desventajas serían:

* Configuración tediosa, propensa a errores y mantenimiento prolongados.
* Es el sysadmin quien mantiene la información de las rutas.
* No se adapta nada bien a las redes en crecimiento con lo cual el mantenimiento se  hace complicado.
* Requiere tener conocimiento de toda la red para su correcta implementación.

¿Cuándo son útiles las rutas estáticas?

* En redes pequeñas en las que no está previsto un crecimiento significativo y con solo una ruta hacia una red externa.&#x20;
* En  redes grandes,  para determinado tipo de tráfico o enlaces a otras redes que necesitan más seguridad.&#x20;
* Crear una ruta de respaldo
* Conectar un router de rutas internas
* Resumir entradas de la tabla de routing

El enrutamiento estático y el dinámico no son mutuamente excluyentes, por lo que podemos ver una combinación de protocolos de enrutamiento dinámico y rutas estáticas en la mayoría de las redes.  No obstante tenemos que tener presente que el valor de la distancia administrativa (AD) es una medida de la preferencia de los orígenes de ruta. Esto es, una ruta con valor  administrativo bajo tendrá mayor preferencia  sobre otras rutas con valores altos. En el caso del enrutamiento estático, la AD es 1, por lo tanto, tendrá prioridad sobre todas las rutas aprendidas dinámicamente, que tendrán valores mayores.

Existen diferentes tipos de rutas estáticas:&#x20;

1. Estándar
2. Predeterminada
3. Resumida
4. Flotante

### Ruta estática estándar

Las rutas estáticas son útiles para conectarse a una red remota específica. El router R3 se puede configurar con una ruta estática para alcanzar la red de rutas internas 192.168.10.0/24. En este caso no es necesario implementar un protocolo de enrutamiento dinámico con el router R4-iFP para llegar a 192.168.10.0/24. Basta configurar una ruta estática para alcanzar la red de rutas internas.

<br>

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt="" width="154"><figcaption></figcaption></figure>

### Ruta estática predeterminada

Se trata de una ruta que coincide con todos los paquetes y es utilizada por el router cuando un paquete no coincide con ninguna otra ruta más específica en la tabla de enrutamiento. Este tipo de ruta puede  ser aprendida de manera **dinámica** o haber sido configurada de manera **estática**.&#x20;

Por tanto, se trata de una ruta estática con 0.0.0.0/0 como dirección IPv4 de destino, en cuyo caso, se crea un gateway de último recurso.

¿Cuándo utilizamos las rutas estáticas predeterminadas?

* Cuando ninguna otra ruta de la tabla de routing coincide con la dirección IP destino del paquete. Esto es, si no existe ninguna coincidencia.&#x20;
* Comúnmente utilizada cuando conectamos un router periférico a la red de un ISP.
* Cuando un router conecta únicamente a otro router: router de rutas internas.

### Ruta estática resumida

Permite reducir el número de entradas en la tabla de enrutamiento. Para resumir varias rutas estáticas en una única ruta se deben cumplir las siguientes condiciones:

* Ser contiguas las redes de destino: se tienen que poder resumir en una única dirección de red.
* Todas las rutas estáticas usan la misma interfaz de salida o dirección IP del siguiente salto.

Si un router requiere cuatro rutas estáticas para alcanzar las redes:&#x20;

* 172.20.0.0/16
* 172.21.0.0/16
* 172.22.0.0/16
* 172.23.0.0/16

Entonces podemos configurar una ruta estática resumida que proporcione conectividad a las mismas. Por ejemplo: 172.20.0.0/14.





### Ruta estática flotante

Las rutas estáticas flotantes son aquellas rutas estáticas que tienen una distancia administrativa (AD) mayor que la de otra ruta estática o la de rutas dinámicas. Este tipo de rutas son útiles cuando se necesita proporcionar un respaldo a un enlace principal y se utiliza solo cuando dicha ruta principal no está disponible.

Para que dicha ruta sirva de respaldo, se configura con una distancia administrativa mayor que la ruta principal. Si existen varias rutas al destino, el router elegirá la que tenga una menor distancia administrativa.

Por tanto, podemos definir una ruta estática que “flota” y no se usa mientras está activa la ruta con una mejor AD. En caso de que se pierda la ruta de preferencia, la ruta estática flotante pudiera tomar el control y enviar el tráfico a través de la misma.

{% hint style="success" %}
Recordemos que de manera predeterminada, las rutas estáticas tienen una distancia administrativa de 1, lo que las hace preferibles a las rutas descubiertas mediante protocolos de routing dinámico. Por ejemplo, las distancias administrativas de algunos protocolos de routing dinámico comunes son las siguientes:

* EIGRP = 90
* IGRP = 100
* OSPF = 110
* IS-IS = 115
* RIP = 120

La distancia administrativa de una ruta estática se puede aumentar para hacer que la ruta sea menos deseable que otra ruta estática o que una ruta descubierta vía protocolo de enrutamiento dinámico.&#x20;
{% endhint %}

En resumen:

* Una ruta estática flotante nos sirve de respaldo de una ruta ya descubierta por un protocolo de enrutamiento dinámico o de una definida de manera estática. También estaría configurada con una distancia administrativa mayor que el protocolo de enrutamiento dinámico original.
* La ruta estática predeterminada hace coincidir todos los paquetes y los envía a un gateway predeterminado específico. También suele utilizarse en routers perimetrales para conectarse a la red del proveedor de servicios o ISP.
* Las rutas estándar son útiles para conectarse a una red de rutas internas.

