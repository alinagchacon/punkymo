---
description: Rya, Mininet, OpenFlow en Ubuntu Desktop
---

# SDN

### **Ingeniería de tráfico**

La ingeniería de tráfico o TE es el proceso de optimizar el rendimiento y la eficiencia de una red mediante el control dinámico del enrutamiento y la asignación de recursos. Los métodos tradicionales de TE se basan en protocolos y configuraciones fijas que a menudo son complejas, inflexibles y difíciles de adaptar a las cambiantes demandas de tráfico y condiciones de red.

Por tanto, se trata de un mecanismo que permite optimizar el rendimiento de la red de datos, analizando dinámicamente, prediciendo y regulando el comportamiento de los datos transmitidos por la red.

La ingeniería de tráfico estudia la monitorización y gestión del tráfico de red y diseña mecanismos de enrutamiento razonables para guiar el tráfico de red a fin de mejorar la utilización de los recursos de red y cumplir mejor los requisitos de la calidad de servicio (QoS) de la misma.

### Redes definidas por software

Las redes definidas por software - SDN son una arquitectura que centraliza la manipulación de todos los dispositivos de red en un único componente. Todos los dispositivos de red son operados a través de este componente SDN.

La gestión de la transmisión de paquetes la realiza la misma entidad central y los dispositivos de red se comportan como **dispositivos de reenvío**, que reciben y reenvían paquetes. En términos técnicos, las SDN diseccionan los planos de control de los dispositivos de red y los centralizan en un único componente, dejando únicamente los planos de datos responsables del reenvío de paquetes.

#### Conceptos básicos

Al separar la lógica de control de los dispositivos de red, SDN permite la programabilidad de la misma, su administración simplificada y autónoma. SDN:

* Permite eliminar muchas de las limitaciones de las infraestructuras de red actuales.
* Separa el plano de datos del plano de control
* Centraliza el estado de la red y la capacidad de toma de decisiones.
* Centraliza la programación en el plano de control (Controlador SDN)
* Simplifica la operación en el plano de datos (Dispositivos de red SDN)
* Permite que la infraestructura subyacente sea abstraída para que aplicaciones y servicios puedan tratar a la red como una entidad lógica o virtual.

La Open Networking Foundation define una arquitectura de alto nivel para SDN con tres capas o planos principales, como se muestra en la figura.

<figure><img src="../.gitbook/assets/image (34).png" alt="Arquitectura SDN" width="563"><figcaption></figcaption></figure>

Los dispositivos SDN:

* contienen componentes para decidir qué hacer con el tráfico entrante.
* el controlador SDN programa los dispositivos de red y presenta una abstracción de la infraestructura de red subyacente a las aplicaciones SDN.
* el controlador permite que una aplicación SDN defina los flujos de tráfico y las rutas en los dispositivos de red.

La comunicación entre capas es posible gracias a la SouthBound y la NorthBound API:

* **Southbound API** - usada para la comunicación entre el controlador SDN y los elementos de red como: switches, routers, etc. Pueden ser de código abierto o propietarias: OpenFlow, NetConf1, Lisp2, OpFlex3, etc.
* **Northbound API** - son Rest APIs utilizadas para la comunicación entre el controlador SDN y los servicios y aplicaciones que corren por encima de la red, en la capa de aplicación. Están integradas dentro del controlador SDN, un ejemplo es el controlador Ryu.

Uno de los principales beneficios de SDN para la ingeniería de tráfico - TE es que permite un control más granular y dinámico sobre los recursos de red, como el ancho de banda, la latencia y la confiabilidad.

SDN permite a los operadores de red especificar políticas y objetivos de alto nivel para la red, y luego traducirlos automáticamente en reglas y acciones de bajo nivel para los dispositivos de red. Esto reduce la complejidad y la sobrecarga de la configuración y el ajuste manuales, y mejora la capacidad de respuesta y adaptabilidad de la red a los patrones de tráfico cambiantes y las condiciones de la red. SDN también permite a los operadores de red aprovechar más información e inteligencia de la red, como estadísticas de tráfico en tiempo real, topología y estado, y usarla para optimizar las decisiones y acciones de TE.

#### Desafíos

A pesar de todo, hay que decir que la implementación de SDN para TE también conlleva algunos desafíos que deben considerarse, como son:

* La escalabilidad y el rendimiento del controlador SDN, que es responsable de recopilar, procesar y difundir la información y las instrucciones de la red.
* El controlador SDN necesita manejar una gran cantidad de datos y solicitudes de los dispositivos de red, y proporcionar respuestas oportunas y consistentes.
* El controlador SDN también necesita hacer frente a la heterogeneidad y diversidad de los dispositivos, protocolos y aplicaciones de red, y garantizar la interoperabilidad y compatibilidad entre ellos.
* La seguridad y confiabilidad de la red SDN, que está expuesta a nuevas amenazas y vulnerabilidades debido a la naturaleza centralizada y programable del controlador SDN.
* El controlador SDN necesita proteger la red de ataques, errores y fallos maliciosos, y proporcionar mecanismos para la tolerancia a errores, la recuperación y la copia de seguridad.

### **Soluciones**

Para superar algunos de los desafíos de SDN para TE se han propuesto y desarrollado varias soluciones.

* Diseñar e implementar controladores SDN distribuidos y jerárquicos, que pueden dividir la red en dominios más pequeños y delegar algunas de las funciones de control a controladores locales o regionales.
  * Esto puede reducir la carga y la latencia del controlador central y mejorar la escalabilidad y el rendimiento de la red SDN.
* Adoptar e integrar diversas técnicas y herramientas de seguridad y confiabilidad, como cifrado, autenticación, autorización, auditoría, registro, monitoreo, pruebas, verificación y depuración. Estos pueden ayudar a prevenir, detectar y mitigar los riesgos y daños potenciales de la red SDN, y garantizar su integridad, disponibilidad y resiliencia.

### Ejemplos de SDN para TE

Veamos algunas de las aplicaciones prácticas y beneficios de SDN para TE, así como algunos ejemplos de cómo SDN se puede utilizar para mejorar el rendimiento y la eficiencia de TE en diferentes escenarios y contextos.

**Ejemplo 1:** Usar **SDN para habilitar el equilibrio de carga adaptable y el control de congestión** en redes de centros de datos, que pueden manejar grandes volúmenes de tráfico con diferentes demandas y requisitos. SDN puede:

* monitorear la carga de tráfico y la distribución en la red
* ajustar dinámicamente el enrutamiento y la asignación de recursos para equilibrar la carga y evitar la congestión.

Esto puede mejorar el rendimiento, la latencia y la calidad de servicio (QoS) de las aplicaciones del centro de datos.

**Ejemplo 2:** Usar **SDN para habilitar la ingeniería de tráfico flexible en redes de área amplia (WAN)**, que pueden abarcar múltiples ubicaciones geográficas y dominios. SDN puede:

* coordinar las políticas y acciones de ingeniería de tráfico entre diferentes operadores y proveedores de red,
* optimizar dinámicamente el enrutamiento y la asignación de recursos en función de diversos criterios y restricciones, como el costo, el ancho de banda, la latencia, la confiabilidad y la seguridad.

Esto puede mejorar la utilización, la eficiencia y la rentabilidad de los recursos WAN.

### Tendencias futuras de SDN para TE

Las tendencias de SDN para TE implican:

* incorporación de técnicas y algoritmos más sofisticados como el aprendizaje automático, la inteligencia artificial y la optimización para mejorar la toma de decisiones y la ejecución del controlador SDN.
* integración de diversas tecnologías y recursos de red, como la computación en la nube, la computación perimetral, la computación en la niebla, la computación móvil, las redes inalámbricas, las redes ópticas y las redes cuánticas, puede ampliar el alcance de la red SDN y admitir una gama más amplia de aplicaciones y servicios.
* desarrollo de protocolos e interfaces estandarizados como [OpenFlow](https://www.f5.com/es_es/glossary/openflow)[https://www.f5.com/es\_es/glossary/openflow](https://www.f5.com/es_es/glossary/openflow), [P4](https://www.lenovo.com/co/es/glosario/p4/?orgRef=https%253A%252F%252Fwww.google.com%252F\&srsltid=AfmBOopL78XOfWCAdUx3XPJomHHa6uU_E71WK12Zenw9CW-hmpUuAGJZ), [BGP-LS](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/configuration/xe-16-6/irg-xe-16-6-book/bgp-ls.pdf), [NETCONF](https://trbl-services.eu/blog-yang-netconf-la-pareja-perfecta-la-gestion-dispositivos-red), [YANG](https://trbl-services.eu/blog-yang-netconf-la-pareja-perfecta-la-gestion-dispositivos-red) y [API Restful ](https://www.redhat.com/es/topics/api/what-is-a-rest-api)puede permitir una mejor comunicación entre diferentes componentes y dispositivos SDN, así como una integración más fluida entre diferentes dominios de red y proveedores.

### **OpenFlow**

OpenFlow es un protocolo de comunicación que se utiliza en el ámbito de las redes definidas por software - SDN.

Es el protocolo principal de la arquitectura SDN. Los controladores utilizan el protocolo OpenFlow para comunicarse con los switches. No se trata de un protocolo específico de un proveedor, lo que implica que el controlador puede comunicarse con cualquier switch, independientemente del proveedor.

Su función principal es permitir que un controlador SDN se comunique con los dispositivos de red (switches, routers, puntos de acceso, etc.) para indicarles cómo manejar el tráfico. De este modo, en lugar de que cada switch o router tome sus decisiones de forma independiente (como es el caso de las redes tradicionales), OpenFlow les permite recibir instrucciones directas de un controlador central.

Un **switch OpenFlow:**

* Contiene una **tabla de flujo** - flow table.
* Cada entrada de la tabla especifica:
  * el tipo de tráfico que coincide, por ejemplo, paquetes con una determinada dirección IP o puerto.
  * la acción a tomar, o sea: reenviar por un puerto, modificar cabeceras, descartar, etc.
* El **controlador SDN** instala o actualiza esas reglas en los switches usando OpenFlow.

\
**Switches SDN**

Estos switches SDN son diferentes de los switches convencionales, por tanto se hace referencia a ellos como _<mark style="color:purple;">dispositivos de reenvío</mark>_, puesto que están conformados únicamente con planos de datos.

Los switches SDN pueden ser hardware o softwar, siendo **Open vSwitch (OVS)** es el switch virtual más popular utilizado en el paradigma SDN para conectar dispositivos finales.

### **Mininet**

Es un emulador de red que crea una red de hosts virtuales, switches, controladores y enlaces. Los hosts de Mininet ejecutan software de red Linux estándar y sus switches son compatibles con OpenFlow para un enrutamiento personalizado flexible.

Mininet también es una excelente manera de desarrollar, compartir y experimentar con sistemas de redes definidas por software (SDN) utilizando OpenFlow y [P4](https://p4.org/).

* Ofrece un banco de pruebas de red sencillo y económico para el desarrollo de aplicaciones OpenFlow.
* Permite que varios desarrolladores simultáneos trabajen de forma independiente en la misma topología.
* Admite pruebas de regresión a nivel de sistema, repetibles y fáciles de empaquetar.
* Permite realizar pruebas de topología complejas sin necesidad de cablear una red física.
* Incluye una CLI que reconoce la topología y OpenFlow para depurar o ejecutar pruebas en toda la red.
* Admite topologías personalizadas arbitrarias e incluye un conjunto básico de topologías parametrizadas.
* Se puede usar de inmediato sin necesidad de programar.
* Además, proporciona una API de Python sencilla y extensible para la creación y experimentación de redes.
* Mininet proporciona una manera sencilla de obtener el comportamiento correcto del sistema (y, en la medida en que lo permita su hardware, el rendimiento) y de experimentar con topologías.

Las redes Mininet ejecutan código real, incluyendo aplicaciones de red estándar de Unix/Linux, así como el kernel y la pila de red de Linux (incluidas las extensiones del kernel disponibles, siempre que sean compatibles con los espacios de nombres de red).

Gracias a esto, el código que se desarrolla y prueba en Mininet, para un controlador OpenFlow, un conmutador modificado o un host, puede trasladarse a un sistema real con cambios mínimos para realizar pruebas reales, evaluar el rendimiento e implementarlo. Es importante destacar que esto significa que un diseño que funciona en Mininet generalmente puede trasladarse directamente a conmutadores de hardware para el reenvío de paquetes a velocidad de línea.

### **Ryu**

Ryu es un componeRyunt-based SDN framework, o sea, es un entorno de trabajo que proporciona componentes software que se utilizan en SDN, entre ellos un controlador, con una API bien definida que facilita a los desarrolladores la creación de nuevas aplicaciones de administración y control de red.

#### <mark style="color:purple;">Instalando Mininet</mark>

```
sudo apt install mininet
```

Se puede comprobar la funcionalidad de Mininet haciendo lo siguiente:

```
sudo mn --test pingall
```

#### <mark style="color:purple;">Instalando Ryu</mark>

Actualizamos los paquetes del sistema:

```
sudo apt update && sudo apt upgrade -y
```

{% hint style="warning" %}
Nota: antes de continuar con la instalación es bueno aclarar que justamente me puse a probar en una VM que ya tenía con un Ubuntu Desktop 22.04 y con Python3.10. Después de varios intentos para hacer la instalación (sencilla de por sí) leí que ryu tenía conflictos precisamente con Python3.10 y lo recomendable era bajar a la 3.9. Como la información es del 2023 pongo en duda que sea la única forma de hacerlo. Ya debe estar corregido (pero ahora no me sobra el tiempo para investigar más). Puedes verlo en: [https://github.com/faucetsdn/ryu/issues/169](https://github.com/faucetsdn/ryu/issues/169)
{% endhint %}

Podemos hacer la instalación de dos tipos pero aquí solo muestro una:

Nos aseguramos tener pip, el sistema de gestión de paquetes para instalar, actualizar y desinstalar paquetes de software escritos en Python.

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get install virtualenv python3.9 python3.9-distutils
```

Creamos el entorno virtual

```
virtualenv -p`which python3.9` ryu-python3.9-venv
source ryu-python3.9-venv/bin/activate
echo $VIRTUAL_ENV #check if we are indeed in the virtual environment

pip install ryu
pip uninstall eventlet
pip install eventlet==0.30.2

ryu-manager --help
```

Para testear podemos hacer también:

```
sudo ryu-manager --version
```

#### <mark style="color:purple;">Instalando OpenFlow y Wireshark</mark>

Instalamos OpenFlow y testeamos:

```
sudo apt-get install openvswitch-switch
ovs-vsctl --version
```

Instalamos Wireshark y testeamos:

```
sudo apt-get install wireshark
wireshark --version
```

#### <mark style="color:blue;">Testeando</mark>

En el terminal abrimos tres pestañas y ejecutamos los siguientes comandos en cada una de ellas:

En el **terminal 1** vamos a crear una topología simple en mininet. La red que se crea tendrá un switch (s1) y tres hosts (h1, h2, h3) conectados a él.

```
sudo mn --topo=single,3 --controller=remote,ip=127.0.0.1 --mac --switch=ovsk,protocols=OpenFlow13
```

Todavía en este punto, los hosts son inaccesibles. Para que los hosts se comuniquen entre sí, el controlador debe implementar ciertas reglas. Podemos probar la funcionalidad de esta topología usando el comando **pingall** en la misma terminal.

En el l t**erminal 2** vamos a ejecutar una aplicación simple de switch. El archivo del programa en python es simple\_switch\_13.py que se encuentra en el directorio **/ryu/ryu/app/simple\_switch\_13.py**. Este script establece reglas y políticas en el controlador **ryu.** Las mismas son necesarias para enrutar paquetes de forma similar a la de un switch L2 de red convencional.

```
sudo ryu-manager ryu/ryu/app/simple_switch_13.py
```

Ahora podemos volver a probar la funcionalidad de esta topología usando el comando **pingall** en la terminal 1 y veremos que los hosts pueden comunicarse entre sí esta vez.

<figure><img src="../.gitbook/assets/image (904).png" alt="" width="320"><figcaption></figcaption></figure>

En una tercera terminal podemos ejecutar el comando siguiente:

```
sudo ovs-vsctl show
sudo ovs-ofctl -O OpenFlow13 dump-flows s1
```

Este comando nos permite visualizar la configuración y la tabla de flujo de Open vSwitch. Estos comandos se utilizan para inspeccionar el comportamiento y las estadísticas de los switches SDN al recibir paquetes de los hosts. Nos muestra la salida de los flows instalados en el switch s1, que está directamente conectado a h1 (10.0.0.1), h2 (10.0.0.2) y h3 (10.0.0.3).

<figure><img src="../.gitbook/assets/image (906).png" alt="" width="306"><figcaption></figcaption></figure>

Para saber los puertos de conexión y por tanto conocer la ruta completa, es necesario ejecutar el siguiente comando desde la terminal de Mininet:

```
mininet>net ports
```

<figure><img src="../.gitbook/assets/image (26).png" alt="" width="375"><figcaption></figcaption></figure>

Si levantamos Wireshark desde el inicio para hacer capturas de los paquetes podremos monitorizar el tráfico. Por ejemplo:

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

#### Extras

Si queremos acceder a las terminales de los hosts h1 y h2, por ejemplo, podemos iniciarlas desde la consola de mininet:

<pre><code><strong>mininet> xterm h1 h2  
</strong></code></pre>

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

En el pantallazo anterior podemos ver que se ha hecho un **ip a** para comprobar las direcciones IP que tienen ambos hosts h1 y h2.

### iPerf

[iPerf](https://iperf.fr/) es una herramienta cliente-servidor que sirve para hacer mediciones activas del ancho de banda máximo alcanzable en redes IP. Permite ajustar diversos parámetros relacionados con la sincronización, los búferes y los protocolos (TCP, UDP, SCTP con IPv4 e IPv6). Para cada prueba, informa el ancho de banda, la pérdida y otros parámetros.

Una máquina actúa como servidor a la espera de tráfico y otra como cliente que envía tráfico para evaluar la velocidad de conexión entre ambos. Esta herramienta de línea de comandos para TCP y UDP es útil para diagnosticar problemas y entender el rendimiento de la red.

Realmente la versión original **iperf** desarrollado en C por **NLANR/DAST** se quedó sin mantenimiento activo durante un tiempo e **iperf3** ha sido reescrita por **ESnet (Energy Sciences Network)** también en C pero con un diseño más modular y limpio.

Esta nueva versión [iperf3](https://blog.elhacker.net/2024/12/iperf-para-medir-la-velocidad-ancho-banda-red-internet-lan-wan.html):

* no es compatible hacia atrás con iperf e iperf2, lo que nos obligaría a actualizar tanto el servidor como el cliente si queremos hacer uso de este nuevo programa.
* tiene mejor precisión en las medidas de ancho de banda
* mejor manejo de **multihilo**
* brinda reportes en formato **JSON.**
* permite pruebas unidireccionales y bidireccionales.
* licencia **BSD**, más libre para integrarlo en proyectos.

#### Testeando

Entonces, teniendo estas dos terminales abiertas podemos configurar a h1 como servidor y h2 como cliente haciendo:

```
#h1 10.0.0.1 servidor
#iperf3 -s 

#h2 10.0.0.2 cliente
#iperf3 -c 10.0.0.1 
```

Como se muestra en el pantallazo siguiente, se han transferido 6.24 GB de tráfico TCP.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption><p>Tráfico generado entre los hosts 1 y 2 con iperf3</p></figcaption></figure>

Ahora vamos a buscar información sobre los puertos de s1 utilizando el comando **dump-ports** de\
**ovs-ofctl** en cuyo output figurará la cantidad de paquetes enviados y recibidos por todos los puertos, entre otros.

```
sudo ovs - ofctl -O OpenFlow13 dump - ports s1
```

<figure><img src="../.gitbook/assets/image (32).png" alt="" width="563"><figcaption><p>Información de los puertos del switch s1</p></figcaption></figure>

En la primera línea se indica que existen 4 puertos aunque muestran 3. También podemos ver que los paquetes entrantes (rx pkts) del puerto "s1 - eth1" se transmiten (tx pkts) por los puertos "s1 - eth2" y "s1 - eth2" de tal modo que los recibidos en el puerto "s1 - eth1" deben estar aproximadamente en el rango de la suma de los transmitidos por los otros puertos, como era de esperar:

```
Rx(s1 - eth1) ≈ Tx(s1 - eth2) + Tx(s1 - eth3) → 91777 ≈ 91804 + 67
```

### Links

* [https://mininet.org/overview](https://mininet.org/overview)
* [h](https://smfarjad.github.io/A-Brief-Tutorial-on-SDN-using-Ryu-Controller)[ttps://smfarjad.github.io/A-Brief-Tutorial-on-SDN-using-Ryu-Controller](https://smfarjad.github.io/A-Brief-Tutorial-on-SDN-using-Ryu-Controller)
* [https://es.wikipedia.org/wiki/OpenFlow](https://es.wikipedia.org/wiki/OpenFlow)
* [https://opennetworking.org](https://opennetworking.org)
* [https://mininet.org/walkthrough](https://mininet.org/walkthrough)
* [https://p4.org](https://p4.org)
* [https://github.com/p4lang/tutorials/blob/master/P4\_tutorial.pdf](https://github.com/p4lang/tutorials/blob/master/P4_tutorial.pdf)
* [https://iperf.fr/iperf-doc.php](https://iperf.fr/iperf-doc.php)
* [https://blog.elhacker.net/2024/12/iperf-para-medir-la-velocidad-ancho-banda-red-internet-lan-wan.html](https://blog.elhacker.net/2024/12/iperf-para-medir-la-velocidad-ancho-banda-red-internet-lan-wan.html)
