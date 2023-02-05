---
description: Instalación de un servidor DNS
---

# Servicio DNS

Como podéis suponer, no hay nada escrito aquí que sea 100% mío. Es una recopilación de información sobre el servicio de DNS. Tengo que referenciar cada una de las imágenes que he utilizado que no es de confección propia, me falta mucho para finalizar y dejarlo como un contenido que pueda servir de verdad para mis clases. Al menos, en la forma que yo lo veo.

Por tanto, debo reconocer que he tomado “prestado” contenidos de Incibe, Kaspersky, Amazon, Cisco, incluso material de la UPC, el Tanenbaum, etc.

Quisiera que fuera considerado como mis apuntes para aprender de quienes verdaderamente saben del tema. Veréis que casi todo es tomado del documento “**Introducció al sistema DNS Grau en Enginyeria Telemàtica Grau en Enginyeria de Sistemes de Telecomunicación” de Frederic Raspall**.

#### Introducción

Todos los equipos con acceso a Internet se comunican entre sí a través de la IP, que vendría a ser el DNI de los dispositivos. Cuando abres el navegador y escribes el nombre de un sitio web, utilizas un nombre como puede ser amazon.es, ifp.es, fnac.es con lo cual no necesitas recordar y escribir la IP del sitio, del mismo modo que nos llamamos por nuestros nombres y no por el número de DNI que nos identifica.

Podemos decir que desde el momento en el que un usuario teclea un dominio en su navegador, se ejecutan una serie de peticiones internas que acaban traduciendo el nombre de dominio a una dirección IP, que es la dirección del servidor que aloja la página web.

Para tener una dirección IP, internamente se hace una consulta a un servidor DNS que contiene la información del dominio. Así pues, el primer paso es identificar cuáles son los servidores DNS del dominio. Una vez identificado el servidor que tiene la información correcta, se le pregunta a él directamente la dirección IP del servidor web.

Una vez se ha 'resuelto' la dirección IP, se procede a preguntarle al servidor (petición 'HTTP' o 'HTTPS') y éste responde con el contenido de la página web.

Por tanto, el servicio de DNS es un servicio distribuido a nivel global que permite convertir los nombres de los sitios en las direcciones IP que les corresponde.

Cada vez que un usuario registra un dominio, se crea una entrada WHOIS en el registro correspondiente y esta queda almacenada en el DNS como un “resource record”. La base de datos de un servidor DNS se convierte, así, en la compilación de todos los registros de la zona del espacio de nombres de dominio que gestiona.

El servicio de DNS se encuentra en la capa de aplicación del modelo OSI. Esta capa es la más cercana al usuario y contiene todos los protocolos de alto nivel. Entre los protocolos más antiguos están el de terminal virtual (TELNET), el de transferencia de archivos (FTP) y el de correo electrónico (SMTP). Con el tiempo se le han añadido otros protocolos como el DNS y el HTTP.

Este protocolo de red emplea la capa de transporte TCP, UDP y utiliza el puerto 53.

Veamos algunos ejemplos de la presencia del DNS en nuestros dispositivos.

**Ejemplo:** &#x20;

En windows**:**

<mark style="color:blue;">`cmd > ipconfig /all`</mark>

<figure><img src="../../.gitbook/assets/image (165).png" alt=""><figcaption><p>ipconfig /all</p></figcaption></figure>

**Ejemplo:**

En Linux:

<mark style="color:blue;">`cat /etc/resolv.conf`</mark>

&#x20;****&#x20;

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption><p>resolv.conf</p></figcaption></figure>

**Ejemplo**:

El nombre de dominio [www.ifp.es](http://www.ifp.es) tiene como IP la 213.192.253.78

Una forma de verlo sería desde el <mark style="color:blue;">`cmd > ping`</mark> [<mark style="color:blue;">`www.ifp.es`</mark>](http://www.ifp.es)<mark style="color:blue;">``</mark>

<figure><img src="../../.gitbook/assets/image (70) (1).png" alt=""><figcaption><p>ping www.ifp.es</p></figcaption></figure>

O bien utilizando el comando <mark style="color:blue;">`nslookup`</mark> que veremos más adelante:

<figure><img src="../../.gitbook/assets/image (95).png" alt=""><figcaption><p>nslookup</p></figcaption></figure>

**Ejemplo:**

Se puede ver la caché con el comando

<mark style="color:blue;">`cmd > ipconfig /displaydns`</mark>

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

Si observas en la imagen del nslookup verás que dice “Respuesta no autoritativa”.  El hecho es que existen dos tipos de servicios de DNS:

**DNS autoritativo**: este servicio proporciona un mecanismo de actualización para administrar los nombres de DNS públicos. Tiene la autoridad final sobre el dominio y es responsable de brindar respuestas a los servidores de DNS recurrente con la información de la dirección IP.

**DNS recurrente**: conocido como <mark style="color:blue;">`resolver`</mark> o solucionador es a donde los clientes generalmente se conectan para realizar las consultas. Este servicio funciona como un intermediario que obtiene la información del DNS para brindarla al usuario que lo solicita. Si conoce la respuesta por tenerla almacenada en caché suministra la información al usuario. En caso de no tenerla, pasa la consulta a uno o más servidores de DNS **autoritativo** para encontrar la información.

Por tanto, el DNS funciona como una agenda telefónica que hace una correspondencia entre los nombres y las IP, por tanto, es una base de datos distribuida con información sobre hosts y servicios. Esto permite un control local de los "segmentos" de la base de datos y, a su vez, permite que cada segmento sea accesible a toda la red a través de un esquema cliente-servidor.

#### ¿Cómo funciona el DNS?

Veamos la siguiente imagen tomada de [¿Qué es DNS? – Introducción a DNS - AWS (amazon.com)](https://aws.amazon.com/es/route53/what-is-dns/)

El esquema nos permite ver cómo los servicios de DNS **recurrente** o **autoritativo** funcionan para dirigir al usuario al sitio web que desea visitar.\


<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption><p>DNS - amazon.com</p></figcaption></figure>

Por lo que la arquitectura del DNS incluye los siguientes elementos:

* **Espacio de nombres de dominio:** que permite nombrar hosts e indexar.
* **Resource Records (RR)**: contienen la información en sí, los datos, de la base de datos.
* **Servidores** de nombres (**nameservers**): servidores de información de la base de datos, es decir de RR.
* **Resolver**: entidades demandantes de información, clientes. Constituyen la interfaz con las aplicaciones (p.ej. un navegador o un cliente de correo), que piden al resolver **resolver** un nombre o dirección.
* Un **protocolo** para solicitar/responder datos en la base de datos del tipo petición-respuesta. Los resolver o solucionadores son siempre clientes, mientras que los **nameservers** pueden ser clientes y servidores simultáneamente.

#### ¿Cómo se construye el nombre de un sitio o dominio?

Se denomina **nombre absoluto** o **FQDN** (Fully-qualified domain name) **** que está **** asociado a un nodo a la concatenación de etiquetas desde el nodo hasta la raíz, separadas por puntos y siguiendo un orden inverso al del sistema de archivos de Linux.

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption><p>nombre absoluto o FQDN (Fully-qualified domain name). Imagen tomada de Introducció al sistema DNS Grau en Enginyeria Telemàtica Grau en Enginyeria de Sistemes de Telecomunicación” de Frederic Raspall</p></figcaption></figure>

#### RR - **Resource Records**

Los RR no sólo contienen direcciones IP. Habitualmente, los nodos “hoja”, denominados así porque no tienen descendientes contienen RR tipo A, que almacenan direcciones IPv4. Las direcciones IPv6 serían RR tipo AAA.

Los nodos interiores pueden contener cualquier tipo de RR y los hay de varias clases:

Internet (IN), Chaosnet (CH) y Hesiod (HS), aunque la clase Internet es prácticamente la única que se utiliza.

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption><p>Tomado de Introducció al sistema DNS Grau en Enginyeria Telemàtica Grau en Enginyeria de Sistemes de Telecomunicación” de Frederic Raspall</p></figcaption></figure>

#### Dominios y subdominios

Un dominio es un sub-árbol del espacio de nombres del DNS. Esto es, se trata de todo el conjunto de nodos hijos o descendientes de un cierto nodo “padre”, con lo cual el nombre de un dominio es el nombre de dominio del nodo padre.

Se dice que un dominio es un subdominio de otro, siempre y cuando el primero forme parte del segundo.

Por ejemplo, el dominio .edu hace referencia a todo el árbol que “cuelga” del nodo .edu.

Pero el dominio .upc.edu es un sub-árbol bajo el dominio del nodo .edu.

Bajo el dominio del nodo .edu tenemos también, por ejemplo, ub.edu, la uab.edu (en Alabama, Birmingham) y no la Autónoma de Barcelona que sería uab.cat porque no puede haber dos hijos con el mismo nombre. &#x20;

El dominio.upc.edu., es el sub-árbol por debajo del nodo upc.edu y es un sub-dominio del dominio .edu. El dominio fib.upc.edu. engloba todos los nodos que se encuentran por debajo del nodo padre fib.epc.edu. (con lo cual, todos los nombres acaban en fib.upc.edu.).

Este dominio es un sub-dominio de upc.edu. y de edu. Si tuviéramos un dominio gia.fib.upc.edu. sería un dominio que pertenecería a todos los dominios anteriores: gia.fib.upc.edu, .upc.edu. y .edu.

<figure><img src="../../.gitbook/assets/image (189).png" alt=""><figcaption><p>Tomado de Introducció al sistema DNS Grau en Enginyeria Telemàtica Grau en Enginyeria de Sistemes de Telecomunicación” de Frederic Raspall</p></figcaption></figure>

#### Ventajas

* Elimina los problemas del sistema basados en el fichero HOSTS.TXT
* El hecho de que el espacio de nombres sea jerárquico permite "particionar" y gestionar los diferentes subdominios de modo independiente.
* El espacio de nombres jerárquico también resuelve el problema de las colisiones o nombres duplicados porque garantiza que todos los nombres hermanos o hijos de un mismo nodo tengan etiquetas o nombres diferentes. Esto es, no puede haber dos nodos con el mismo FQDN, aunque pueden tener la misma etiqueta pero sin causar ambigüedades.
*   Por ejemplo: [www.upc.edu](http://www.upc.edu)., [www.fib.upc,edu](http://www.fib.upc,edu)., [www.eetac.upc.edu](http://www.eetac.upc.edu)., etc.

    &#x20;

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption><p>Tomado de Introducció al sistema DNS Grau en Enginyeria Telemàtica Grau en Enginyeria de Sistemes de Telecomunicación” de Frederic Raspall</p></figcaption></figure>

#### Delegación de dominios
