# Routers

### Configuraciones básicas

Los routers y switches de Cisco se asemejan mucho. Ambos admiten S.O modales y estructuras de comandos similares, y muchos comandos son iguales. Además, los pasos de configuración inicial son similares para ambos dispositivos.&#x20;

Por ejemplo, las siguientes tareas de configuración siempre deben realizarse y es igual para un switch que para un router.&#x20;

Vamos a asignar un nombre al dispositivo para distinguirlo de otros routeres y configure contraseñas, como se muestra en el ejemplo.

<pre><code><strong>Router# configure terminal
</strong><strong>Enter configuration commands, one per line.  End with CNTL/Z.
</strong><strong>Router(config)# hostname R1
</strong><strong>R1(config)# enable secret class
</strong><strong>R1(config)# line console 0
</strong><strong>R1(config-line)# password cisco
</strong><strong>R1(config-line)# login
</strong><strong>R1(config-line)# exit
</strong><strong>R1(config)# line vty 0 4
</strong><strong>R1(config-line)# password cisco
</strong><strong>R1(config-line)# login
</strong><strong>R1(config-line)# exit
</strong><strong>R1(config)# service password-encryption
</strong><strong>R1(config)#
</strong></code></pre>

Si queremos configurar un banner para proporcionar notificaciones legales de acceso no autorizado, como se muestra en el ejemplo.

<pre><code><strong>R1(config)# banner motd #Authorized Access Only!#
</strong><strong>R1(config)#
</strong></code></pre>

Para guardar los cambios en un router, como se muestra en el ejemplo.

<pre><code><strong>R1# copy running-config startup-config
</strong><strong>Destination filename [startup-config]?
</strong><strong>Building configuration...
</strong><strong>[OK]
</strong></code></pre>



### Configuración de interfaces

Los routers admiten redes LAN y WAN, con lo cual pueden conectar diferentes tipos de redes y, por lo tanto, admiten muchos tipos de interfaces.&#x20;

**Ejemplo**&#x20;

Los ISR G2 tienen 1 o 2 interfaces Gigabit Ethernet integradas y ranuras para tarjetas de interfaz WAN de alta velocidad (HWIC) para admitir otros tipos de interfaces de red, incluidas las interfaces seriales, DSL y de cable.



Para que una interfaz esté disponible, debe cumplir los siguientes requisitos:

* **Configurado con al menos una dirección IP:** Podemos utilizar los comandos de configuración de **ip address** _ip-address subnet-mask_ y **ipv6 address** _ipv6-address/prefix_ interface.
* **Activado:** Las interfaces LAN y WAN no están activadas de manera predeterminada (shutdown). Para habilitarla tenemos que hacerlo mediante el comando **no shutdown**. La interfaz también debe estar conectada a otro dispositivo (un hub, un switch u otro router) para que la capa física se active.
* **Descripción:** De modo opcional la interfaz  se puede configurar con una breve descripción de hasta 240 caracteres. Es aconsejable configurar una descripción en cada interfaz. En las redes de producción, los beneficios de las descripciones de la interfaz se obtienen rápidamente, ya que son útiles para solucionar problemas e identificar una conexión de terceros y la información de contacto.

**Ejemplo**&#x20;

Configuración de las interfaces en R1.

<pre><code><strong>R1(config)# interface gigabitethernet 0/0/0
</strong><strong>R1(config-if)# ip address 192.168.10.1 255.255.255.0
</strong><strong>R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
</strong><strong>R1(config-if)# description Link to LAN 1
</strong><strong>R1(config-if)# no shutdown
</strong><strong>R1(config-if)# exit
</strong><strong>
</strong><strong>R1(config)# interface gigabitethernet 0/0/1
</strong><strong>R1(config-if)# ip address 192.168.11.1 255.255.255.0
</strong><strong>R1(config-if)# ipv6 address 2001:db8:acad:2::1/64
</strong><strong>R1(config-if)# description Link to LAN 2
</strong><strong>R1(config-if)# no shutdownR1(config-if)# exit
</strong><strong>
</strong><strong>R1(config)# interface serial 0/0/0
</strong><strong>R1(config-if)# ip address 209.165.200.225 255.255.255.252
</strong><strong>R1(config-if)# ipv6 address 2001:db8:acad:3::225/64
</strong><strong>R1(config-if)# description Link to R2
</strong><strong>R1(config-if)# no shutdownR1(config-if)# exit
</strong><strong>R1(config)#
</strong></code></pre>



### Interfaces de bucle invertido IPv4

Una configuración común en los routers Cisco es la habilitación de una interfaz loopback. Esta interfaz lógica interna del router no está asignada a un puerto físico y nunca se puede conectar a ningún otro dispositivo. Se la considera una interfaz de software que se coloca automáticamente en estado "up" (activo), siempre que el router esté en funcionamiento.

Esta interfaz de loopback es útil para probar y administrar dispositivos Cisco IOS, dado que asegura que por lo menos una interfaz esté siempre disponible. Se puede utilizar con fines de prueba, como es el caso de los procesos de routing interno, mediante la emulación de redes detrás del router.

Las interfaces de loopback también se utilizan en entornos de laboratorio para crear interfaces adicionales. Por ejemplo, puede crear varias interfaces de bucle invertido en un router para simular más redes con fines de práctica de configuración y pruebas.&#x20;

El proceso de habilitación y asignación de una dirección de loopback es simple:

<pre><code><strong>Router(config)# interface loopback number 
</strong>Router(config-if)# ip address ip-address subnet-mask 
</code></pre>

Es posible habilitar varias interfaces loopback en un router. Lógicamente, la dirección IPv4 para cada interfaz  debe ser única y no debe ser utilizada por ninguna otra interfaz, como se muestra en la configuración de ejemplo.

<pre><code><strong>R1(config)# interface loopback 0
</strong><strong>R1(config-if)# ip address 10.0.0.1 255.255.255.0
</strong><strong>R1(config-if)# exit
</strong><strong>R1(config)#%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up
</strong></code></pre>



### Comandos de verificación&#x20;

Algunos comandos útiles para verificar rápidamente el estado de una interfaz son:

* **show ip interface brief (show ipv6 interface brief)**  - muestran un resumen de todas las interfaces, incluída la dirección IPv4 o IPv6 de la interfaz y el estado operativo actual.
* **show running-config interface&#x20;**_**interface-id -** muestra los comando aplicados a la interfaz especificada._
* _show ip route - show ipv6 route - muestra el contenido de la tabla IPv4 o IPv6 almacenada en la memoria RAM. En Cisco, en versiones posteriores a la 15, las interfaces activas aparecen en la tabla con dos entradas:_&#x20;
  * _C - conectado_&#x20;
  * _L - local_

