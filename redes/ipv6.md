---
description: Direcciones IPv6
---

# IPv6

## Introducción

Ya hemos visto que las direcciones IPv4 tienen un largo de 32 bits, donde podemos identificar el prefijo de la red a partir de la máscara de subred, por ejemplo:  `255.255.255.0`  ó `/24` **Pero y  qué son las IPv6?**

### Algo de historia

Las direcciones IPv6 surgen ante la necesidad de ampliar el rango de direcciones IPv4 disponibles en la red.

El límite en el número de IP válidas ha comenzado a restringir el crecimiento de Internet, especialmente en países como China, India debido a que están densamente poblados.

El nuevo estándar mejorará el servicio globalmente; por ejemplo, proporcionará a futuras celdas telefónicas y dispositivos móviles sus direcciones propias y permanentes.

A principios de 2010, quedaban menos del 10% de IP sin asignar. En febrero de 2011​ la Agencia Internacional de Asignación de Números de Internet, (IANA) por sus siglas en inglés, entregó el último bloque de direcciones disponibles, 33 millones a la organización encargada de asignar IP en Asia, un mercado que está en auge y no tardará en consumirlas todas.

En 2008, el gobierno de EUA ordenó el despliegue de IPv6 por todas sus agencias federales.

### Comprendiendo las IPv6

Las direcciones IPv6:

* Se basan en el sistema hexadecimal
* Tienen una longitud de 128 bits
* Están representados en 8 octetos de 16 bits cada uno

#### &#x20;Ejemplo:

`2001:0DB8:AAAA:1111:0000:0000:0000:0100/64`

| 1       | 2       | 3       | 4       | 5       | 6       | 7       | 8       |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |
| 2001    | 0DB8    | AAAA    | 1111    | 0000    | 0000    | 0000    | 0100    |
| 16 bits | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits |

&#x20;

#### Algunas características a tener en cuenta

* IPv6 es una extensión conservadora de IPv4, en muchos aspectos
* La mayoría de los protocolos de transporte y red no necesitan cambios o casi, para operar con las IPv6

&#x20;

## Coexistencia de IPv4 e IPv6

1\)  **DUAL-STACK**: permite que IPv4 e IPv6 coexistan en la misma red.

2\) **Tunneling**: método para transportar paquetes IPv6 a través de redes IPv4. El paquete IPv6 se encapsula dentro de un paquete IPV4.

3\) **Traducción**: la traducción de direcciones de red 64 (NAT64) permite que los dispositivos con IPv6 habilitado se comuniquen con dispositivos con IPv4 habilitado mediante una técnica de traducción similar a la NAT para IPv4. Un paquete IPv6 se traduce en un paquete IPV4, y viceversa

## Sistema hexadecimal

#### &#x20;

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Sistema hexadecimal</p></figcaption></figure>

## Cambios relevantes en IPv6

Existen tres tipos de direcciones IPv6:

1\)  Unicast

2\)  Multicast

3\)  Anycast

&#x20;

### UNICAST

Identifican de forma exclusiva una interfaz en un dispositivo con IPv6 habilitado.

Un paquete que se envía a una dirección `UNICAST` es recibido por la interfaz que tiene asignada esa dirección

&#x20;

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>Tomado de ...</p></figcaption></figure>

#### Direcciones IPv6 unicast

#### Unicast global

* Similares a direcciones IPv4 públicas.
* Únicas a nivel global.
* Direcciones enrutables de Internet.
* Pueden configurarse estáticamente o asignarse de forma dinámica.
* Tienen 001 en sus tres bits de mayor peso, por lo que el prefijo es **2000::/3**.

#### Link-local

* Utilizada para comunicarse con los otros dispositivos en el mismo enlace local.
* Limitada a un único enlace: no se puede enrutar más allá del enlace
* Una dirección IPv6 link-local permite que un dispositivo se comunique con otros dispositivos con IPv6 habilitado en el mismo enlace y solo en ese enlace (subred).
* Los paquetes con una dirección link-local de origen o de destino no se pueden enrutar más allá del enlace en el cual se originó el paquete.
* La dirección unicast global no es un requisito, pero toda interfaz de red con IPv6 habilitado si debe tener una dirección link-local.
* Si no se configura una dirección link-local de forma manual, se crea automáticamente su propia dirección sin comunicarse con un servidor de DHCP.
* Esto permite que los dispositivos con IPv6 habilitado se comuniquen con otros dispositivos con IPv6 en la misma subred.
* Es una dirección IP creada únicamente para las comunicaciones dentro de una subred local.
* Los routers no enrutan paquetes con direcciones de enlace local.
* Incluye la comunicación con el gateway predeterminado (router).
  * Las direcciones IPv6 link-local están en el rango de FE80::/10, donde:&#x20;
  * /10 indica que los primeros 10 bits son 1111 1110 10xx xxxx.
  * El primer hexteto está en el rango de 1111 1110 1000 0000 (FE80) a 1111 1110 1011 1111 (FEBF).

**Nota**: por lo general, la dirección que se utiliza como gateway predeterminado para los otros dispositivos en el enlace es la dirección `link-local` del router, y no la dirección `unicast global`.

&#x20;

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>Tomado de www.sapalomera.cat</p></figcaption></figure>

### Prefijo de enrutado global (n bits).

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

1. Limita el rango de direcciones asignado al sitio.
2. Para redes de tamaño pequeño y medio suele constar de 48 bits y es gestionado por los servicios de registro internacionales y los ISPs.
3. La IANA (Internet Assigned Numbers Authority) es el organismo responsable de la coordinación, a nivel mundial, del direccionamiento.
4. El reparto de direcciones se realiza de forma jerárquica y hay una entidad responsable para cada región del mundo.

&#x20;

<figure><img src="../.gitbook/assets/image (7) (7).png" alt=""><figcaption></figcaption></figure>

### ANYCAST

1. Están dentro del espacio de las direcciones unicast, por lo que no se puede distinguir una dirección anycast de una unicast.
2. El "prefijo de subred" en una dirección anycast es el prefijo que identifica un enlace específico.
3. Esta dirección anycast es sintácticamente igual que cualquier otra dirección unicast de una interfaz del enlace, con el identificador de interfaz a cero.

&#x20;

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### MULTICAST

1. Direcciones que empiezan por 0xFF, es decir, por 1111 1111, en binario.
2. Tienen el rango FF00::/8
3. Los nodos que se configuran con una dirección multicast determinada forman un grupo de multidifusión.
4. Un nodo puede pertenecer a varios grupos de multidifusión
5. Cuando un paquete es enviado a una dirección de multidifusión, todos los miembros del grupo procesan el paquete.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Los primeros 8 bits (puestos a 1) identifican la dirección como multicast.

Existen dos tipos:

1. Assigned multicast
2. Solicited node multicast

### Assigned multicast

Son direcciones predefinidas por la norma para dirigirse a determinados grupos de dispositivos.

| Dirección IP | Dispositivos           |
| ------------ | ---------------------- |
|  FF02::1     | Todos los nodos        |
|  FF02::2     | Todos los routers      |
|  FF02::5     | Routers OSPF           |
|  FF02::6     | Routers asignados OSPF |
|  FF02::9     | Routers RIP            |
|  FF02::1:2   | Agente DHCP            |

&#x20;

La dirección multicast que se dirige a todos los nodos (la FF02::1) es la equivalente a la dirección de broadcast en IPv4.

### &#x20;Solicited node multicast

1. Se crean automáticamente cuando una dirección global unicast o una link-local es asignada.
2. Tienen la máscara FF02:0:0:0:0:1:FF00::/104, es decir FF02:0:0:0:0:1:FFXX:XXXX, donde las XX:XXXX son los últimos 24 bits de la dirección unicast o link-local
3. Sirve para resolver las direcciones físicas MAC de una manera más eficiente a como se hacía en IPv4.
   1. En IPv4 se utiliza el protocolo ARP para pedir a un host destino qué dirección MAC tiene.
   2. La dirección MAC es necesaria para que los switch sepan por qué puerto deben redireccionar un frame entrante.
   3. Con el protocolo ARP se utilizan direcciones de broadcast para notificar al host destino que informara sobre su dirección MAC, por lo que toda la red se inunda de paquetes que solo van dirigidos a un único host.
   4.  Otra de las utilidades es el de la detección de direcciones duplicadas (Duplicate Address Detection DAD)



Por tanto, una dirección multicast de nodo solicitado viene a resolver este problema, ya que los últimos 24 bits son los mismos que los de la dirección de link-local o unicast, de manera que un router puede dirigir mejor a qué hosts enviarlo.

Cierto que puede ser que una solicitud de dirección MAC llegue a más de un host dado que las direcciones link-local se pueden repetir en diferentes redes, pero el número de hosts que deben procesar y descartar el paquete es considerablemente menor.

Este mecanismo de solución de direcciones se conoce como Neighbor Discovery Protocol (NDP).

### &#x20;Resumido

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

## Algunos detalles&#x20;

Desde el CMD&#x20;

```
ipconfig
```

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

En la implantación de IPv6 se ha tenido en cuenta lo que se llama la «Precedencia de protocolos» lo cual puede hacer que nuestro equipo pase a funcionar en modo IPv6 en cualquier momento, sin avisarnos y por tanto escapando a nuestro control.

Veamos:

```
netsh interface ipv6 show prefixpolicies
```

<figure><img src="../.gitbook/assets/image (1) (2) (3).png" alt=""><figcaption></figcaption></figure>

El sistema operativo da precedencia a IPv6 sobre IPv4 si es posible utilizar este protocolo, lo cual ya supone una pérdida de tiempo, recursos, ancho de banda en intentar comprobar si puede establecer la comunicación mediante IPv6 antes de pasar a IPv4

Al teclear una URL en el navegador, primero intenta resolver dicha URL a una IP de IPv6 mediante consulta a servidores DNS de IPv6 que utilizan registros AAAA; si no es posible, la intenta resolver consultando a los servidores DNS de IPv4.

#### ¿Cuántos equipos con IPv6 hay habilitado en mi red?

Comprobemos nuestros vecinos con el siguiente comando:

```
netsh interface ipv6 show neighbors
```

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

## Detalles DE IPv6

1\)  IPv6 permite que una interfaz tenga más de una dirección IP

2\)  Algunas direcciones como las de enlace local FE80:: tienen un ámbito de red local,

3\)  Otras direcciones tienen ámbito global como 2001::/64.

a.  De este último tipo, un interfaz puede tener todas las que quiera.

_Nota: Las IP de ámbito global son las que nos permiten comunicarnos a través de internet a nivel mundial y son públicas._

&#x20;

4\)  Capacidad extendida de redireccionamiento: cambiando el prefijo anunciado por unos pocos routers es posible en principio reasignar la numeración de toda la red, ya que los identificadores de nodos (los 64 bits menos significativos de la dirección) pueden ser auto configurados independientemente por un nodo.

5\)  Autoconfiguración de direcciones libres de estado (SLAAC): Pueden configurarse a sí mismos automáticamente cuando son conectados a una red ruteada en IPv6.

6\)  Multicast: la habilidad de enviar un paquete único a destinos múltiples es parte de la especificación base de IPv6.

7\)  Seguridad de Nivel de red obligatoria: El protocolo para cifrado y autenticación IP (IPSec) forma parte integral del protocolo base en IPv6.

8\)  Procesamiento simplificado en los routers: Se hicieron varias simplificaciones en la cabecera de los paquetes, así como en el proceso de reenvío de paquetes para hacer el procesamiento de los paquetes más simple y por ello más eficiente.

&#x20;

### Comprimiendo las IPv6

Ya hemos dicho que las IPv6 tienen un largo de 128bits, repartidos en 8 octetos de 16 bits cada uno.

Existen dos reglas básicas para comprimir las direcciones IPv6 y hacerlas más “manejables” en lo posible. Veamos:

1\. Si un octeto de 4 dígitos (16 bits) es nulo, éste puede dejar de representarse.

2\. Los ceros a la izquierda en cada octeto, pueden obviarse



<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Omitiendo los segmentos que son ceros (0)

1\)  Los dos puntos dobles (::) pueden reemplazar cualquier cadena única y contigua de uno o más segmentos de 16 bits (hextetos) que estén compuestas solo por ceros.

2\)  Los dos puntos dobles (::) se pueden utilizar solamente una vez en una dirección; de lo contrario, la dirección será ambigua.

3\)  Ejemplo de dirección incorrecta:

a.  2001:0DB8::ABCD::1234

&#x20;

#### Ejemplo 1:

2001:0db8:85a3:0000:1319:8a2e:0370:7344

&#x20;

| <pre><code>2001:0db8:85a3::1319:8a2e:0370:7344
</code></pre> |                                |                                |                                |                                |                                |                                |                                |
| ------------------------------------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ |
| <pre><code>2001:
</code></pre>                               | <pre><code>0db8:
</code></pre> | <pre><code>85a3:
</code></pre> | <pre><code>0000:
</code></pre> | <pre><code>:1319
</code></pre> | <pre><code>:8a2e
</code></pre> | <pre><code>:0370
</code></pre> | <pre><code>:7344
</code></pre> |
| <pre><code>2001:
</code></pre>                               | <pre><code>0db8:
</code></pre> | <pre><code>85a3:
</code></pre> | <pre><code> 
</code></pre>     | <pre><code>:1319
</code></pre> | <pre><code>:8a2e
</code></pre> | <pre><code>:0370
</code></pre> | <pre><code>:7344
</code></pre> |

&#x20;

Siguiendo esta regla, podemos decir que si dos o más grupos “consecutivos” son nulos, los podemos comprimir igualmente como “::”.

&#x20;

#### Ejemplo 2:

2001:0DB8:0000:0000:0000:0000:1428:57ab

&#x20;

| <pre><code>2001: 
</code></pre> | <pre><code>0DB8: 
</code></pre> | <pre><code>0000: 
</code></pre> | <pre><code>0000: 
</code></pre> | <pre><code>0000: 
</code></pre> | <pre><code>0000: 
</code></pre> | <pre><code>1428: 
</code></pre> | <pre><code>57ab
</code></pre> |
| ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------- | ----------------------------- |
| <pre><code>2001: 
</code></pre> | <pre><code>0DB8: 
</code></pre> | <pre><code>0: 
</code></pre>    | <pre><code>0: 
</code></pre>    | <pre><code>0: 
</code></pre>    | <pre><code>0: 
</code></pre>    | <pre><code>1428: 
</code></pre> | <pre><code>57ab
</code></pre> |
| <pre><code>2001: 
</code></pre> | <pre><code>0DB8: 
</code></pre> | <pre><code>0:
</code></pre>     | <pre><code> 
</code></pre>      | <pre><code>: 
</code></pre>     | <pre><code>0: 
</code></pre>    | <pre><code>1428: 
</code></pre> | <pre><code>57ab
</code></pre> |
| <p> </p><p>2001:</p>            | <p> </p><p>0DB8:</p>            | <p> </p><p>:</p>                |                                 |                                 | <p> </p><p>:</p>                | <pre><code>1428: 
</code></pre> | <pre><code>57ab
</code></pre> |

&#x20;

#### Ejemplo 3:

2001::25de::cade         &#x20;

IP no válida porque no podemos saber cuántos grupos nulos quedan en cada lado.

| <pre><code>2001: 
</code></pre> | <p> </p><p>0000:</p> | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p>           | <pre><code>25de:
</code></pre> | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p> | <pre><code>Cade
</code></pre> |
| ------------------------------- | -------------------- | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | -------------------- | ----------------------------- |
| <pre><code>2001: 
</code></pre> | <p> </p><p>0000:</p> | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p>           | <pre><code>25de:
</code></pre> | <p> </p><p>0000:</p> | <pre><code>Cade
</code></pre> |
| <pre><code>2001: 
</code></pre> | <p> </p><p>0000:</p> | <p> </p><p>0000:</p>           | <pre><code>25de:
</code></pre> | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p> | <pre><code>Cade
</code></pre> |
| <pre><code>2001: 
</code></pre> | <p> </p><p>0000:</p> | <pre><code>25de:
</code></pre> | <pre><code>0000:
</code></pre> | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p>           | <p> </p><p>0000:</p> | <pre><code>Cade
</code></pre> |

&#x20;

#### Ejemplo 4:

| 2001: | 1000: | 1001: | 1010: | 1100: | 0001: | 0101: | 0011 |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ---- |
| 2001: | 1000: | 1001: | 1010: | 1100: | 1:    | 101:  | 11   |

&#x20;

#### Ejemplo 5:

| 0010: | 1010: | 1020: | 0001: | 1000: | 0A0A | 00FF | FF00 |
| ----- | ----- | ----- | ----- | ----- | ---- | ---- | ---- |
| 0010: | 1010: | 1020: | 1:    | 1000: | A0A  | FF   | FF00 |

&#x20;

#### Ejemplo 6:

| FF02:     | 0000: | 0000: | 0000: | 0000: | 0000: | 0000: | 0500 |
| --------- | ----- | ----- | ----- | ----- | ----- | ----- | ---- |
| FF02:     | 0:    | 0:    | 0:    | 0:    | 0:    | 0:    | 500  |
| FF02::500 |       |       |       |       |       |       |      |

&#x20;

### Identificación de los tipos de direcciones

Los tipos de direcciones IPv6 pueden identificarse tomando en cuenta los rangos definidos por los primeros bits de cada dirección.

&#x20;

| <p> </p><p>::/128</p>                | <p> </p><p>Todo ceros indica ausencia de dirección, y no se asigna ningún nodo.</p>                                                                                      |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p> </p><p>::1/128</p>               | <p> </p><p>Dirección que puede usar un nodo para enviarse paquetes a sí mismo. Corresponde con 127.0.0.1 de IPv4.</p><p>No puede asignarse a ninguna interfaz física</p> |
| ::1.2.3.4/96                         | Dirección IPv4 compatible que se usaba como mecanismo de transición en las redes duales IPv4/IPv6.                                                                       |
| <p> </p><p>::ffff:0:0/96</p><p> </p> | Dirección IPv4 mapeada se usa como mecanismo de transición en terminales duales                                                                                          |
| <p> </p><p>fe80::/10</p><p> </p>     | Prefijo de enlace local, indicando que la dirección solo es vàlida en el enlace físico local                                                                             |

### &#x20;

### Ventajas y Desventajas de las IPv6

#### &#x20;

#### Ventajas:

9\)  Puede asignar una cantidad cercana a los 670 mil millones de direcciones por cada milímetro cuadrado de la superficie de la tierra

10\)        Lograría que cada persona pueda asignarle una IP única a cada uno de sus dispositivos

11\)        Mejores niveles de seguridad

12\)        Utiliza paquetes de datos de mayor tamaño

13\)        Al incorporar IPv6 una gran cantidad de direcciones, no será necesario utilizar NAT Traducción de direcciones de red

#### Desventajas

14\)        La necesidad de extender un soporte permanente requiere una dirección IPv4 o algún tipo de NAT Traducción de direcciones de red en los routers pasarela.

15\)        Las direcciones IPV6 son más difíciles de memorizar.

16\)        La mayoría de redes son [IPv4](https://es.wikipedia.org/wiki/IPv4) por lo que la implementación total de IPv6 es muy costosa y tardaría mucho tiempo

17\)        Por ahora, se requieren la implementación de mecanismos de transición para la interacción de las 2 redes.

### &#x20;

Nota:

Visita la página [http://test-ipv6.com/](http://test-ipv6.com/) y testea tu dirección IPv4 e IPv6

### Identificando el prefijo de red y de host

La duración de prefijo indica la porción de red de una dirección IPv6 mediante el siguiente formato: Dirección/duración de prefijo IPv6

La duración de prefijo puede ir de 0 a 128.

&#x20;&#x20;

<figure><img src="../.gitbook/assets/image (6) (6).png" alt=""><figcaption></figcaption></figure>

&#x20;La duración de prefijo típica es /64

Podemos identificar el prefijo de la red a partir de la cuenta de bits, o sea, la longitud del prefijo.

### Ejemplos de notación de la longitud del prefijo:

3ffe: 1944: 100: a:: /64

16   32   48   64 bits

&#x20;

Donde:

3ffe: 1944: 100: a - es la porción de red

`::`  - es la porción de host

#### &#x20;Ejemplos de cómo podemos identificar la porción de red y la porción de host

&#x20;Veamos la siguiente IP

2001 :: 1  /80            es lo mismo que

2001 : 0 : 0 : 0 : 0 : 0 : 0 : 1 &#x20;

&#x20;

Bits de red: 80 bits

Bits de host: 48 bits

Bits que identifican la porción de red: 2001:0:0:0:0

Bits que identifican la porción de host: 0:0:1

&#x20;

Veamos como obtenemos la longitud del prefijo de red:

En cada segmento de la IP tenemos 16 bits

| 2001                                          | 0                                   | 0       | 0       | 0       | 0       | 0       | 1       |
| --------------------------------------------- | ----------------------------------- | ------- | ------- | ------- | ------- | ------- | ------- |
| 16 bits                                       | 16 bits                             | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits |
| 16                                            | 32                                  | 48      | 64      | 80      | 96      | 112     | 128     |
| Porción de red                                | Porción de host                     |         |         |         |         |         |         |
| <p>16 + 16 + 16 + 16 + 16 =</p><p>80 bits</p> | <p>16 + 16 + 16 =</p><p>48 bits</p> |         |         |         |         |         |         |

&#x20;

&#x20;

Veamos la misma IP pero con otra cantidad de bits identificando la porción de red

\
2001::1 /16

Bits de red: 16

Bits de host: 112 bits

Bits que identifican la porción de red: 2001

Bits que identifican la porción de host: 0:0:0:0:0:0:1

&#x20;

Veamos como obtenemos la longitud del prefijo de red:

| 2001           | 0                                                        | 0       | 0       | 0       | 0       | 0       | 1       |
| -------------- | -------------------------------------------------------- | ------- | ------- | ------- | ------- | ------- | ------- |
| 16 bits        | 16 bits                                                  | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits | 16 bits |
| 16             | 32                                                       | 48      | 64      | 80      | 96      | 112     | 128     |
| Porción de red | Porción de host                                          |         |         |         |         |         |         |
| 16 bits        | <p>16 + 16 + 16 + 16 + 16 + 16 + 16 =</p><p>112 bits</p> |         |         |         |         |         |         |

&#x20;

&#x20;

Otro ejemplo (un poco diferente):\
2001::1 /3

Bits de red: 3

Bits de host: 125 bits

&#x20;

En este caso tenemos que convertir a binario el primer segmento de la IP, o sea:

&#x20;

2001 (hex)

0010 0000 0000 0001

&#x20;

Bits que identifican la porción de red: 2001

Bits que identifican la porción de host: 0:0:0:0:0:0:1

&#x20;

&#x20;

Veamos como obtenemos la longitud del prefijo de red:

| 2001 (hex) - el primer octeto de los 8 segmentos de la IPv6 |                                                               |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| ----------------------------------------------------------- | ------------------------------------------------------------- | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - |
| 0                                                           | 0                                                             | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |   |   |   |   |   |   |   |
|                                                             |                                                               |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| Porción de red                                              | Porción de host                                               |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 3 bits                                                      | <p>13 + 16 + 16 + 16 + 16 + 16 + 16 + 16 =</p><p>125 bits</p> |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|                                                             |                                                               |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |

## Links

1\.  [https://ipv6.mineco.gob.es/ipv6/Paginas/que-es-IPv6.aspx](https://ipv6.mineco.gob.es/ipv6/Paginas/que-es-IPv6.aspx)

2\.  [https://www.iana.org/numbers](https://www.iana.org/numbers)

3\.  [http://rubensm.com/tag/anycast/](http://rubensm.com/tag/anycast/)



&#x20;
