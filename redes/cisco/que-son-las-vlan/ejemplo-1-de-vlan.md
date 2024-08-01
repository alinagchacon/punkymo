---
description: de un modo simple
---

# Ejemplo 1 de VLAN

En esta sección configuraremos  4 VLAN. Dichas VLAN tienen comunicación entre sí. Al final, vamos a configurar un pool de DHCP para cada una de las VLAN en el Router.&#x20;

La idea que vamos a desarrollar para comprender la configuración de una VLAN es la siguiente:

1. Simplificaremos la configuración utilizando un único PC que equivale a una red.
2. Para poder visualizarlo de un modo más cómodo hemos utilizado colores, por lo que cada VLAN equivale a un color, esto es, un equipo.
3. En la configuración se tienen dos switches, donde se establece el **enlace troncal**.
4. Todas las configuraciones tanto del switch como del router las haremos desde el modo terminal: CLI
5. La VLAN nativa será la 300 (blau), sin embargo, por seguridad no debería ser ni la 1 (que es la vlan por defecto) ni una vlan en uso como es en este caso. Lo haremos así para "simplificar".\


<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption><p>Configurando VLANs</p></figcaption></figure>

### Configuración de la topología

Ubicar en el entorno de trabajo de Cisco

1. 2 switches 2960
2. 1 Router
3. 4 PC
4. 1 servidor\


**¿Qué vamos a hacer?**

1. Renombrar los switches como SW-1 y SW-2
2. Conecta los dos switches a través de la Interface Gigabit Ethernet 0/2
3. Conecta el switch SW-1 con el router a través de la Gigabit Ethernet 0/1
4. Conecta los equipos y dispositivos según muestra la imagen anterior.

### Paso 1 - Establecer las VLAN y los puertos

Lo primero que debemos hacer es establecer las VLAN, esto es: para las redes verde y amarillo vamos a utilizar una IP de clase C. Para la red rosa, una clase A y para la red azul una clase B.

| VLAN                                                | Características                                                                               |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| <mark style="color:green;">VLAN 100 (verde)</mark>  | <p>Red – 192.168.56.0 /24<br>IP - 192.168.56.2<br>Gateway – 192.168.56.1<br>DNS - 8.8.8.8</p> |
| <mark style="color:orange;">VLAN 200 (rosa)</mark>  | <p>Red – 10.10.0.0 /8<br>IP – 10.10.0.2<br>Gateway – 10.10.0.1<br>DNS - 8.8.8.8</p>           |
| <mark style="color:blue;">VLAN 300 (blau)</mark>    | <p>Red – 172.10.10.0 /16<br>IP - 172.10.10.2<br>Gateway – 172.10.10.1<br>DNS - 8.8.8.8</p>    |
| <mark style="color:yellow;">VLAN 400 (groc)</mark>  | <p>Red – 192.168.88.0 /24<br>IP - 192.168.88.2<br>Gateway – 192.168.88.1<br>DNS - 8.8.8.8</p> |

Aunque vamos a configurar el servicio de DHCP y DNS en el router yo siempre os propongo que configuremos los parámetros de red de manera manual, o sea, que las IP de cada PC sean estáticas.&#x20;

Una vez que todo esté configurado correctamente, que tengamos conectividad, haremos la configuración del servicio de DHCP y DNS para cada VLAN. Por tanto, nos disponemos a configurar cada dispositivo.

Lo siguiente sería establecer la cantidad de puertos de acceso para cada VLAN en los swtiches. Dado que tenemos 4 VLAN podemos hacerlo igual en cada switch:

* Los puertos que van del FastEthernet 0/1 al 0/11 corresponden a una vlan
* Los puertos que van del FastEthernet 0/12 al 0/24 se corresponden a otra vlan

### Paso 2 - Configurar los PC&#x20;

Lo primero será configurar las IP de cada PC teniendo en cuenta la VLAN donde estarán.\


<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption><p>Configurando los PC</p></figcaption></figure>

Este proceso hay que repetirlo en los PC por VLAN.

### &#x20;Paso 2 - Crear las VLAN en cada switch

Vamos a crear las vlan en cada switch. Podemos crear sólo aquellas que se conectarán a cada uno, pero igual configuraremos las 4 vlan en cada switch.

#### SW-1

Para crear las VLAN. nos vamos al terminal del Switch SW-1 y hacemos lo siguiente:

```
Switch>enable
Switch#configure terminal
Switch#hostname SW-1 

SW-1(config)#vlan 100
SW-1(config-vlan)#name verde
SW-1(config-vlan)#exit

SW-1(config)#vlan 200
SW-1(config-vlan)#name rosa
SW-1(config-vlan)#exit

SW-1(config)#vlan 300
SW-1(config-vlan)#name blau
SW-1(config-vlan)#exit

SW-1(config)#vlan 400
SW-1(config-vlan)#name groc
SW-1(config-vlan)#exit
```

#### SW-2

Repetimos el proceso en el otro switch pero renombrándolo como SW-2 y lo demás es exactamente igual.

```
Switch>enable
Switch#configure terminal
Switch#hostname SW-2 
.........................
```

### Paso 3 - Declarando el rango de cada VLAN en  los switches

Establecemos los rangos de puertos que pertenecen a cada switch como dijimos anteriormente. Definimos que:

* del 1 al 11 es la VLAN 300&#x20;
* del 12 al 24 la VLAN 400

Declaramos el rango de las interfaces que corresponderá con cada VLAN.

<pre><code><strong>SW-1#enable
</strong>SW-1#configure terminal

SW-1(config)#interface range fa0/1-11
SW-1(config)#switchport access vlan 300
SW-1(config)#switchport mode access

SW-1(config)#interface range fa0/12-24 
SW-1(config)#switchport access vlan 400
SW-1(config)#switchport mode access

SW-1(config)#end
SW-1#show running-config

</code></pre>

En el Switch SW-2 hacemos lo mismo para las vlan 100 y 200, esto es:

<pre><code><strong>SW-2#enable
</strong>SW-2#configure terminal

SW-2(config)#interface range fa0/1-11
SW-2(config)#switchport access vlan 100
SW-2(config)#switchport mode access

SW-2(config)#interface range fa0/12-24 
SW-2(config)#switchport access vlan 200
SW-2(config)#switchport mode access

SW-2(config)#end
SW-2#show running-config

</code></pre>

### Paso 4- configurando los enlaces troncales

El establecimiento del enlace troncal se tiene que hacer entre los dos switches Sw-1 y SW-2 y entre el Sw-1 y el router. También tenemos que declarar la vlan nativa. Para ello escribimos los comandos siguientes en el switch SW-1:

```
SW-1#enable
SW-1#configure terminal

SW-1(config)#interface gi0/2
SW-1(config)#switchport mode trunk
SW-1(config)#switchport trunk native vlan 300
SW-1(config)#exit

SW-1(config)#interface gi0/1
SW-1(config)#switchport mode trunk
SW-1(config)#switchport trunk native vlan 300
SW-1(config)#exit
```

En el otro switch hacemos lo mismo para la interfaz de red que comunica con el switch SW-1:

```
SW-2#enable
SW-2#configure terminal

SW-2(config)#interface gi0/2
SW-2(config)#switchport mode trunk
SW-2(config)#switchport trunk native vlan 300
SW-2(config)#exit

```



### Paso 5- Configurar el router

El router viene apagado por defecto así que nos disponemos a encenderlo. Para ello escribimos en la línea de comandos:

```
Router#enable
Router(config)#configure terminal
Router(config)#hostname R2D2
Router(config)#interface gi0/0
Router(config)#no shutdown
```

Ahora tenemos que establecer el gateway para cada VLAN. De ese modo, habrá comunicación entre todas las redes de cada VLAN. Esto no tiene por qué ser así. De hecho, se debe establecer reglas ACL para regular el tráfico entre las VLAN, aunque en este ejemplo, lo que queremos conseguir es que todas las redes tengan comunicación entre sí.

```
Router#enable
Router(config)#configure terminal

Router(config)#interface gi0/0.300
Router(config-if)#encapsulation dot1q 300 native
Router(config-if)#ip address 172.10.10.1 255.255.0.0
Router(config-if)#exit

Router(config)#interface gi0/0.400
Router(config-if)#encapsulation dot1q 400
Router(config-if)#ip address 192.168.56.1 255.255.255.0
Router(config-if)#exit

Router(config)#interface gi0/0.100
Router(config-if)#encapsulation dot1q 100
Router(config-if)#ip address 192.168.88.1 255.255.255.0
Router(config-if)#exit

Router(config#interface gi0/0.200
Router(config-if)#encapsulation dot1q 200
Router(config-if)#ip address 10.10.0.1 255.0.0.0
Router(config-if)#exit

```

### Comprobaciones

Para comprobar que todo está correctamente configurado siempre podemos recurrir a los comandos siguientes:

```
SW-2#show vlan brief
SW-2#show interfaces trunk
SW-2#show running-config
```

Por ejemplo, si vamos al router y hacemos `show running-config` veremos:

<figure><img src="../../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption><p>COnfiguración en el router</p></figcaption></figure>

Si ponemos el comando `show vlan brief` en el swich SW-1 veremos algo como lo siguiente:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption><p>Configuración de VLAN en un switch</p></figcaption></figure>

Si escribimos `show interfaces trunk` en el mismo switch, veremos:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption><p>Configuración del enlace troncal en un switch</p></figcaption></figure>

Para acabar de comprobar la conexión entre las 4 vlans podemos hacer ping de una a otra. Por ejemplo, la imagen siguiente muestra un ping que se ha hecho desde el PC de la VLAN 300 (blau) al servidor externo que tiene la IP 8.8.8.8.

<figure><img src="../../../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption><p>Haciendo ping al servidor externo a la red que tiene IP 8.8.8.8</p></figcaption></figure>

El servidor se configura de manera simple:

* Gateway: 8.8.8.1
* IP server: 8.8.8.8/8

### Crear un pool de DHCP

Si llegados a este punto todo funciona correctamente, podremos configurar un pool de DHCP para cada vlan. De ese modo, el propio router hace de servidor de DHCP.

Lo que tendríamos que hacer es lo siguiente:

```
Router#enable
Router#configure terminal 

Router(config)#ip dhcp pool vlan100
Router(dhcp-config)#network 192.168.88.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.88.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit

Router(config)#ip dhcp pool vlan200
Router(dhcp-config)#network 10.10.0.0 255.0.0.0
Router(dhcp-config)#default-router 10.10.0.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit

Router(config)#ip dhcp pool vlan300
Router(dhcp-config)#network 172.10.0.0 255.255.0.0
Router(dhcp-config)#default-router 172.10.10.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit

Router(config)#ip dhcp pool vlan400
Router(dhcp-config)#network 192.168.56.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.56.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#exit

Router(config)#ip dhcp excluded-address 192.168.56.1 192.168.88.1 172.10.10.1 10.10.0.1
Router(config)#end

```



Si hacemos `show running-config` vemos algo como lo siguiente. Solo tenemos que tener en cuenta que el pantallazo a continuación lo hice con otro ejemplo y por eso el servidor DNS es el 11.11.11.11 y no el 8.8.8.8.

<figure><img src="../../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption><p>Pool de DHCP en el Router</p></figcaption></figure>

Para ir poco a poco con la configuración, lo ideal es ir creando cada pool en el Router y probando que el PC de cada VLAN toma los parámetros de red otorgados por el Router hasta tenerlo todo completado.

### Conclusiones

Las VLAN  son una tecnología esencial en la administración de las redes empresariales. Recuerda que las mismas permiten segmentar una red física en diferentes redes lógicas, ofreciendo facilidades en la gestión, la seguridad y la eficiencia de la red.
