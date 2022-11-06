---
description: Servicio de DHCP
---

# DHCP

Un servidor DHCP (Protocolo de Configuración Dinámica de Equipos) no deja de ser un imprescindible en los entornos empresariales. Este servicio nos permite definir rangos de direcciones IP para los equipos clientes que se conecten a nuestra red y poder, de este modo, administrar de manera centralizada todas las direcciones IP que pertenecen al dominio. Así  tendremos la seguridad que todos los equipos y dispositivos que cuelguen de nuestra red tendrán de forma automática su dirección IP, evitando ir a cada máquina a definir la dirección de forma manual.&#x20;

Además de la dirección IP mediante un servidor DHCP proveemos los siguientes parámetros:

* Máscara de subred
* Dirección IP
* Puerta de enlace
* Servidores DNS, entre otros.

Para hacer uso de este servidor necesitaremos trabajar con el protocolo DHCP. Este protocolo de red se basa en un **modelo cliente-servidor.**

#### **Puertos**

La IANA asigna al protocolo DHCP, los puertos UDP 67 y 68 para IPV4 y para IPv6, los puertos 546 y 547. La IETF define en el RFC 2131 las normas del protocolo DHCP. Puedes obtener mayor información en los siguientes enlaces:

* [https://](https://tools.ietf.org/html/rfc2131)[tools.ietf.org/html/rfc2131](https://tools.ietf.org/html/rfc2131)
* [https://](https://www.iana.org/)[www.iana.org](https://www.iana.org/)

El DHCP funciona sobre un <mark style="color:blue;">servidor central</mark> (servidor, estación de trabajo o incluso un PC) el cual asigna direcciones IP a otras máquinas de la red. Este protocolo puede entregar información IP en una LAN o entre varias VLAN.&#x20;

Esta tecnología <mark style="color:blue;">reduce</mark> el trabajo de un administrador, que de otra manera tendría que visitar todos los ordenadores o estaciones de trabajo uno por uno, para introducir la configuración IP consistente en IP, máscara, Gateway, DNS, etc.

Este  es un <mark style="color:blue;">protocolo no autenticado</mark> porque al conectarse un usuario a una red no necesita proporcionar credenciales para obtener una concesión. Es posible que un usuario no autenticado obtenga una concesión para cualquier cliente DHCP siempre que haya un servidor DHCP disponible para proporcionarla. De este modo, el usuario podrá disponer de todos los valores brinde el servidor DHCP con la concesión.

### Seguridad mínima

Para brindar unos mínimos de seguridad deberíamos seguir unas pautas como las siguientes:

* Asegurar que las personas no autorizadas no tengan acceso físico o inalámbrico a la red. Habilitar el registro de auditoría en todos los servidores DHCP de la red.&#x20;
* Comprobar periódicamente los archivos de registro (de la auditoría)&#x20;
* En los archivos de registro de auditoría podrás encontrar toda la información que necesites para localizar el origen de cualquier ataque realizado contra el servidor DHCP.&#x20;
* La ubicación predeterminada de los registros de auditoría de es <mark style="color:blue;">`%windir%\System32\Dhcp`</mark>

### El servicio de DHCP

Configurar y administrar un servidor DHCP no es complicado aunque como todo, requiere ciertos conocimientos previos. Los términos más importantes a tener en cuenta a la hora de configurar este servicio son:

* **Ámbito DHCP:** Se define como el grupo administrativo de usuarios o equipos de una subred.&#x20;
* **Rango DHCP:** Se trata del grupo de IP para realizar una función determinada dentro de la organización.&#x20;
* **Concesión:** Establece el tiempo en el cual un equipo puede usar de forma activa una dirección IP en el dominio o grupo de trabajo.&#x20;
* **Reserva de direcciones IP:** En caso necesario podemos reservar una o diversas direcciones IP para fines administrativos. Por ejemplo, para asignar siempre la misma IP a una impresora.

Para instalar el rol DHCP en nuestro servidor vamos a utilizar **Windows Server 2016.**

### Algunos aspectos a tener en cuenta:

#### APIPA

Cuando el DHCP es incapaz de asignar una dirección IP, se utiliza un proceso llamado <mark style="color:blue;">`Automatic Private Internet Protocol Addressing`</mark>.

<mark style="color:blue;">`APIPA`</mark> – Direccionamiento Privado Automático del Protocolo de Internet, es un protocolo que utilizan los sistemas operativos para obtener la configuración de red cuando está ajustado para obtener una dirección IP de manera dinámica y, al iniciar, este no encuentra un servidor DHCP.

#### Negociación DORA

La asignación automática de direcciones mediante el protocolo de configuración dinámica de host tiene lugar en cuatro pasos consecutivos:

* **Primero**: El cliente DHCP envía un paquete <mark style="color:blue;">`DHCPDISCOVER`</mark> a la IP  <mark style="color:blue;">`255.255.255.255`</mark> desde la IP <mark style="color:blue;">`0.0.0.0`</mark>. De este modo, el cliente establece contacto con todos los integrantes de la red con el propósito de localizar servidores DHCP disponibles e informar sobre su petición.
* **Segundo**: Todos los servidores DHCP que escuchan peticiones en el puerto <mark style="color:blue;">`67`</mark> responden a la solicitud del cliente con un paquete <mark style="color:blue;">`DHCPOFFER`</mark>, que contiene una dirección **IP libre**, la dirección **MAC** del cliente y la máscara de subred, así como la  dirección **IP** y el **ID** del servidor.
* **Tercero**: El cliente DHCP escoge un paquete y contacta con el servidor correspondiente con <mark style="color:blue;">`DHCPREQUEST`</mark>. El resto de servidores también reciben este mensaje de forma que quedan informados de la elección.
* **Cuarto**: El servidor confirma los parámetros TCP/IP y los envía de nuevo al cliente, esta vez con el paquete <mark style="color:blue;">`DHCPACK`</mark> (<mark style="color:blue;">acknowledged</mark> o reconocido). El cliente guarda localmente los datos que ha recibido y se conecta con la red. Si el servidor no contara con ninguna dirección más que ofrecer o durante el proceso la IP fuera asignada a otro cliente, entonces respondería con <mark style="color:blue;">`DHCPNAK`</mark> (DHCP <mark style="color:blue;">not acknowledged</mark> o <mark style="color:blue;">no reconocido</mark>).

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption><p>Wireshark</p></figcaption></figure>

#### Asignación de IP

De manera general, existen tres modos de asignar direcciones IP a un equipo cliente.&#x20;

**Manual**: El administrador asigna una IP concreta a través de una tabla asociando las IP a la dirección MAC de cada equipo.

**Automática**: El servidor DHCP asigna una IP automáticamente tras una petición de un cliente DHCP y este tiene la misma hasta que la libera.

**Dinámica**: Es igual que la automática, pero la IP asignada no tiene una concesión indefinida, sino que tiene un límite de tiempo. Esto es:

* Los parámetros que envía el servidor DHCP son válidos para un periodo <mark style="color:blue;">determinado</mark> de tiempo definido por el administrador que se conoce como concesión (lease time).
* De este modo se indica el tiempo que puede acceder un dispositivo a la red con esa dirección.
* Antes de que se agote el tiempo, los clientes deben solicitar una extensión de la concesión enviando una nueva DHCPREQUEST.&#x20;
* Si no lo hace, no tiene lugar el DHCP refresh y, en consecuencia, el servidor la libera

### Alta disponibilidad

Si el servicio DHCP se detiene y no se de dan más concesiones DHCP y los equipos clientes van perdiendo el acceso a la red puede haber un problema, lógicamente. Para evitar esta situación, es posible instalar un segundo servidor DHCP y compartir el rango de direcciones IP distribuidas que puede ser del 80% en el primer servidor y del 20% en el segundo.&#x20;

Una segunda solución sería instalar un **clúster DHCP**.

A partir de Windows Server 2012, es posible trabajar con dos servidores DHCP sin tener que montar un clúster. Si alguno de los servidores no se encuentra en línea, los equipos cliente pueden seguir recibiendo rangos DHCP.

Durante la configuración del equilibrio de carga, se realiza una división del rango de direcciones en función del porcentaje configurado por el administrador. Cada uno tiene la responsabilidad de la parte del ámbito que gestiona. La administración se realiza por uno de los dos servidores. Hay que tener en cuenta que las reservas deben crearse en ambos servidores.

La conmutación por error DHCP puede contener dos servidores como máximo; además, puede habilitarse únicamente para ámbitos IPv4.



