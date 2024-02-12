---
description: Direcciones IP
---

# IPv4

Si hablamos de IP y no concretamos la versión, estamos haciendo referencia a la versión 4 del protocolo IP de la pila TCP/IP y no la IPv6.&#x20;

Tanto la IPv4 como la IPv6 tienen partes que son comunes dentro del funcionamiento del protocolo, sin embargo, hay otras partes que son radicalmente distintas. Esto hace que se analicen por separado.

En el IPv4, es fundamental un buen diseño del direccionamiento para la buena gestión de la red.

Para que dos equipos de red se conecten por la red tienen que utilizar el protocolo TCP/IP, con lo cual hay que indicar para el origen y el destino, cuáles son sus direcciones. De ese modo se puede localizar al destinatario y  responder al emisor del mensaje con la información.&#x20;

Entonces, ¿Qué es una IP? Un conjunto de 4 números de 8 bits (denominados octetos) escritos en decimal y separados por puntos.&#x20;

#### Ejemplo:&#x20;

192.168.1.10&#x20;

La IPv4 viene acompañada de la llamada máscara de red que no es más que otro valor también representado en 4 octetos. Por tanto, las direcciones IP constan de dos "valores": la IP y la máscara. Una máscara de red es, entonces, un número de 32 bits escrito en bloques de 8 bits separados por puntos, similar a las direcciones IP.

* La IP está compuesta de dos partes: la parte que representa la red y la parte que representa al host.&#x20;
* La máscara de red es quien determina qué parte de la dirección corresponde a la red y que parte al host.&#x20;

Una IP viene a ser como el DNI de una persona, solo puede haber un valor único en su red. También pudiéramos compararlo con un código postal que nos permitiría saber la ubicación de la misma.&#x20;

Veamos el siguiente esquema que representa a la IPv4:

<mark style="color:blue;">`131. 108.122.204`</mark>

<mark style="color:blue;">`255.255.0.0`</mark>

y que es lo mismo que escribir:  <mark style="color:blue;">`131.108.122.204/16`</mark>

<figure><img src="../../.gitbook/assets/image (81) (1).png" alt=""><figcaption><p>La IPv4 131.108.122.204</p></figcaption></figure>

De los 32 bits de esta dirección IPv4, 16 bits se utilizan para representar la red y los otros 16 bits para representar al host, con lo cual se disponen de 2^16 = 65536 direcciones IPv4 para asignar a los hosts. Pero tampoco, todavía nos falta algo por tener en cuenta y es que tenemos que descontar la dirección IP que representa a la red (todos los bits a 0 para la parte del host) y la IP que representa al broadcast (todos los bits a 1 para la parte del host), por tanto, tenemos 65534 direcciones IP válidas para los dispositivos.

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt=""><figcaption><p>Máscara de red para una clase B de red</p></figcaption></figure>

¿Cómo sabemos la cantidad de bits que representan la red y al host?&#x20;

Una manera fácil de identificarlo es tomando como referencia el primer decimal por la izquierda, de los 4 que representan a la IPv4. En el ejemplo anterior sería el valor 131 y nos daría el tipo de red al que pertenece (aunque esto cambia mucho en la actualidad). Vamos por partes.



<figure><img src="../../.gitbook/assets/image (151).png" alt=""><figcaption><p>Tomado de Administración de CCNA de CISCO - Deusto Formación</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption><p>Tomado de Administración de CCNA de CISCO - Deusto Formación</p></figcaption></figure>

### Direcciones con clase

Ya vimos un ejemplo anterior de una IP 131.108.122.204/16 que tiene 2^16 bits para representar la red y otros tantos para representar al host y que estos lo podemos saber por el primer valor en decimal por la izquierda.&#x20;

Las direcciones IP se pueden clasificar teniendo en cuenta si su direccionamiento es con <mark style="color:blue;">clase</mark> o <mark style="color:blue;">sin clase</mark>.&#x20;

Decimos que es con clase cuando conocemos la longitud de la máscara en función del inicio de la dirección de red y tenemos entonces la siguiente clasificación:

* **Clase A**:&#x20;
  * Primer bit a  <mark style="color:blue;">`0.`</mark> La máscara de red es <mark style="color:blue;">`255.0.0.0`</mark> o <mark style="color:blue;">`/8`</mark>.&#x20;
  * En decimal, el primer octeto tiene que estar entre <mark style="color:blue;">`0.0.0.0`</mark> y <mark style="color:blue;">`127.255.255.255`</mark>.&#x20;
* **Clase B**:&#x20;
  * Los dos primeros bits  son <mark style="color:blue;">`10`</mark>. La máscara de red es <mark style="color:blue;">`255.255.0.0`</mark> o <mark style="color:blue;">`/16`</mark>.&#x20;
  * En decimal, el primer octeto tiene que estar entre <mark style="color:blue;">`128.0.0.0`</mark> y <mark style="color:blue;">`191.255.255.255`</mark>
* **Clase C**:&#x20;
  * Comienza por  los bits <mark style="color:blue;">`110`</mark>. La máscara de red es <mark style="color:blue;">`255.255.255.0`</mark> o <mark style="color:blue;">`/24`</mark>.&#x20;
  * En decimal, el primer octeto tiene que estar entre <mark style="color:blue;">`192.0.0.0`</mark> y <mark style="color:blue;">`223.255.255.255`</mark>
* **Clase D**:&#x20;
  * Comienza por  los bits <mark style="color:blue;">`1110`</mark>. Éstas son direcciones para Multicast. No se pueden utilizar para identificar un host.&#x20;
  * En decimal, el primer octeto tiene que estar entre <mark style="color:blue;">`224.0.0.0`</mark> y <mark style="color:blue;">`239.255.255.255`</mark>
* **Clase E**:&#x20;
  * Son direcciones de investigación que no se utilizan.
  * En decimal, el primer octeto tiene que estar entre <mark style="color:blue;">`240.0.0.0`</mark> y <mark style="color:blue;">`247.255.255.255`</mark>

Las clases tienen asociadas las mismas máscaras de red para todas las direcciones de la misma clase. Esto es:

* Clase A: la máscara de red es: 255.0.0.0 e indica que el primer octeto representa la red y el resto al host, con lo cual tenemos 2^24 -2 direcciones válidas para asignar.&#x20;
* Clase B: la máscara de red es: 255.255.0.0 e indica que los dos primeros octetos representan la red y el resto al host, con lo cual tenemos 2^16 -2 direcciones válidas para asignar.
* Clase C: la máscara de red es: 255.255.255.0 e indica que los tres primeros octetos representan la red y el resto al host, con lo cual tenemos 2^8 -2 direcciones válidas para asignar.

De un modo más esquemático sería:

<mark style="color:blue;">R.R.R</mark><mark style="color:red;">.H</mark> - clase C

<mark style="color:blue;">R.R</mark><mark style="color:red;">.H.H</mark> - clase B

<mark style="color:blue;">R.</mark><mark style="color:red;">H.H.H</mark> - clase A

Estas clases de redes también tienen sus excepciones como pueden ser:

* <mark style="color:blue;">`0.X.X.X`</mark> no se utiliza. Es una IP que representa a toda la red.
* <mark style="color:blue;">`127.X.X.X`</mark> es la llamada dirección de _<mark style="color:blue;">lookback</mark>_. Se trata de la IP de la interfaz de red del dispositivo.
* <mark style="color:blue;">`169.254.X.X`</mark> denominada también APIPA es una dirección de enlace local que es auto asignada por la misma interfaz de red cuando tiene configurada una interfaz dinámica y no recibe ninguna IP de oferta por parte de un servidor de DHCP.
* La red que comienza en decimal por <mark style="color:blue;">`255`</mark> se utiliza para identificar el segmento de broadcast de todas las redes.

### Direcciones IPv4 privadas y públicas

Los rangos de direcciones IP que podemos utilizar para asignar en los hosts son de las clases A,B y C. Las mismas se pueden clasificar teniendo en cuenta dos criterios:&#x20;

* **Direcciones IP privadas**: no se pueden enrutar en Internet, pero sí en redes privadas.&#x20;
* **Direcciones IP públicas**: son enrutables en Internet y en redes públicas

Cada clase A, B y C tiene un segmento de red privado y son:

* Clase A: La IP <mark style="color:blue;">10.0.0.0</mark> a <mark style="color:blue;">10.255.255.255</mark>
* Clase B: La <mark style="color:blue;">172.16.0.0</mark> a <mark style="color:blue;">172.31.255.255</mark>.&#x20;
* Clase C: La IP <mark style="color:blue;">192.168.0.0</mark> a <mark style="color:blue;">192.168.255.255</mark>.

El resto de las direcciones de los rangos de clase A, B y C que no son privados son direcciones públicas. Éstas últimas tienen que ser direcciones únicas en el mundo y son asignadas por el proveedor de servicios de Internet (ISP). Por el contrario, las direcciones privadas pueden ser utilizadas libremente.

Para las redes de las grandes empresas que utilizan más de un proveedor de servicios, las direcciones se reservan a través de la [`IANA` ](https://www.iana.org)y se gestionan vía [<mark style="color:blue;">`RIR`</mark> ](https://www.nro.net/about/rirs/)(el [<mark style="color:blue;">`RIPE`</mark>](https://www.ripe.net) para Europa).



### Direcciones IPv4: unicast, broadcast y multicast&#x20;

Podemos encontrar tres tipos de paquetes en IPv4 en función de la selección del destino que se quiera utilizar. Éstos tipos son:&#x20;

* **Unicast**:  el receptor del paquete es un único destino.
* **Broadcast**: el tráfico se envía a todos los equipos del mismo segmento de red.
*   **Multicast**: el tráfico se envía a múltiples equipos simultáneamente. Los equipos han de estar asociados a un grupo multicast concreto. El tráfico multicast se envía a todos los destinatarios del mismo segmento de red. Pero si hay un dispositivo de capa 3 que segmenta la red, solamente se enviará a los equipos de ese segmento.



Estos conceptos los encontraremos también asociados a las direcciones IPv6 que veremos más adelante.

#### Ejemplo de broadcast

Si el destino de broadcast es para la red 192.168.1.0/24, la dirección de broadcast asociada será la 192.168.1.255/24.&#x20;



### Links

* [https://www.ibm.com/docs/es/i/7.2?topic=methods-classless-inter-domain-routing](https://www.ibm.com/docs/es/i/7.2?topic=methods-classless-inter-domain-routing)
* [https://sites.google.com/site/redeslocalesyglobales/6-arquitecturas-de-redes/6-arquitectura-tcp-ip/7-nivel-de-red/2-escasez-de-direcciones-ip-soluciones/3-cidr](https://sites.google.com/site/redeslocalesyglobales/6-arquitecturas-de-redes/6-arquitectura-tcp-ip/7-nivel-de-red/2-escasez-de-direcciones-ip-soluciones/3-cidr)&#x20;
* [https://www.ripe.net](https://www.ripe.net)
* [https://www.nro.net/about/rirs/](https://www.nro.net/about/rirs/)
* [https://www.iana.org](https://www.iana.org)&#x20;
