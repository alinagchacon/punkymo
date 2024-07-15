---
description: VLAN con Switch de capa 3
---

# Ejemplo 2 de VLAN

## Configuraciones de VLAN

Vamos a realizar la siguiente configuración de VLAN en Cisco Packet Tracer donde se quiere representar 3 VLAN que tengan comunicación entre sí y con un servidor externo  a la red. Además, vamos a configurar el router R0 como servidor de DHCP y DNS para las tres VLAN.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Configurando VLAN</p></figcaption></figure>

### (1) Pensar y organizar lo que se tiene que hacer

Dado que se quieren representar 3 VLAN podemos organizarlas de la siguiente manera:

* Las VLAN serán: 10, 11, 12 y la 99 como nativa
* Se configuran las VLAN en el Switch de capa 3 R3D2, en las interfaces:
  * Gi1/0/1 - VLAN 10
  * Gi1/0/2 - VLAN 11
  * Gi1/0/3 - VLAN 12
* El Switch de capa 3 se conecta con el router a través de la interfaz de red: Gi1/0/24
* El server www.google.com con IP 8.8.8.8 representa  un servidor externo a la red.

Una vez tengamos organizado los PC, switches, routers y servidor como se muestra en el esquema anterior vamos a calcular las subredes y decidir las IP de gateway para cada una.&#x20;

### (2) Calcular las subredes o decidir las IP de las redes a utilizar

Tomaremos como IP de red una clásica: `192.168.1.0/24`. Dado que solo queremos 3 VLAN necesitaremos 3 subredes pero tenemos que hacer los cálculos para 4. Esto nos lleva a:

* **subred 0**: 192.168.1.0/26 - rango válido: 192.168.1.1-192.168.1.63
* **subred 1**: 192.168.1.64/26 - rango válido: 192.168.1.65-192.168.1.127
* **subred 2**: 192.168.1.128/26 - rango válido: 192.168.1.129-192.168.1.191
* **subred 4**: 192.168.1.192/26 - rango válido: 192.168.1.193-192.168.1.255

La máscara de subred a utilizar sería: `255.255.255.192`. Recuerda que hemos "pedido" `dos` bits prestados a la parte del host.&#x20;

### (3) Configurar los PC de cada subred

Aunque vamos a configurar el servicio de DHCP y DNS en el router yo siempre os propongo que configuremos los parámetros de red de manera manual, o sea, que las IP de cada PC sean estáticas.&#x20;

Una vez que todo esté configurado correctamente, que tengamos conectividad, haremos la configuración del servicio de DHCP y DNS para cada VLAN. Por tanto, nos disponemos a configurar cada dispositivo.



| PC de VLAN 10                                                                                                                               | PC de VLAN 11                                                                                                                                | PC de VLAN 12                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| <p></p><p><strong>IP</strong>: 192.168.1.10</p><p><strong>Máscara</strong>: 255.255.255.192</p><p><strong>Gateway</strong>: 192.168.1.1</p> | <p></p><p><strong>IP</strong>: 192.168.1.74</p><p><strong>Máscara</strong>: 255.255.255.192</p><p><strong>Gateway</strong>: 192.168.1.65</p> | <p></p><p><strong>IP</strong>: 192.168.1.140</p><p><strong>Máscara</strong>: 255.255.255.192</p><p><strong>Gateway</strong>: 192.168.1.129</p> |

A continuación se muestra cómo quedaría una vez configurado, en este caso, el PC que corresponde a la VLAN 10.



<figure><img src="../../../.gitbook/assets/image (352).png" alt=""><figcaption><p>Configuración del PC de la VLAN 10</p></figcaption></figure>



### (4) Configurar el server

Aunque todavía no tenemos conectividad entre los dispositivos, podemos configurar el servidor externo. Como hemos decidido que la IP del servidor será la IP 8.8.8.8 podemos decidir que la IP de gateway en el router sea la IP 8.0.0.1. Con esta información procedemos:

<figure><img src="../../../.gitbook/assets/image (353).png" alt=""><figcaption><p>Configurando el servidor www.google.com con la IP 8.8.8.8</p></figcaption></figure>

El router R1 está conectado a dicho servidor por la interfaz de red Gi0/0/0 al que le daremos la IP 8.0.0.1 como se muestra en la imagen.

<figure><img src="../../../.gitbook/assets/image (354).png" alt=""><figcaption><p>Configuración de la interfaz de red: Gi0/0/0 del router R1</p></figcaption></figure>

Con todo esto ya estamos en posición de configurar las VLAN en el switch de capa 3.

### (5) Configurar las VLAN

Para hacer la configuración de las VLAN es bueno seguir los siguientes pasos que nos ayudan a establecer una secuencia. Los pasos serían:

1. Crear las VLAN
2. Crear las interfaces VLAN SVI (Switched Virtual Interface)
3. Configurar puertos de acceso del switch
4. Habilitar IP routing
5. Configurar el enlace troncal
6. Configurar gateway en el router R0 para cada VLAN

#### Paso 1: Crear las VLAN en el switch de capa 3&#x20;

**Lo** primero que debemos hacer es definir las VLAN en el switch, donde cada VLAN es un segmento de red lógico separado. Podemos definir las VLAN por linea de comando:

```
Switch>enable
Switch#configure terminal 

Switch(config)#vlan 10
Switch(config-vlan)#name vlan10
Switch(config-vlan)#exit

Switch(config)#vlan 11
Switch(config-vlan)#name vlan11
Switch(config-vlan)#exit

Switch(config)#vlan 12
Switch(config-vlan)#name vlan12
Switch(config-vlan)#exit

Switch(config)#vlan 99
Switch(config-vlan)#name nativa
Switch(config-vlan)#exit

Switch(config)#
```

#### Paso 2: Crear las interfaces VLAN SVI en el switch de capa 3

Ahora nos vamos a crear las interfaces de VLAN SVI, o sea  configurar interfaces virtuales en el switch de capa 3 para permitir la comunicación entre las diferentes VLAN.

Las interfaces SVI de cada VLAN deben ser capaces de enrutar el tráfico. Recuerda que: una interfaz SVI es una interfaz virtual que representa una VLAN en un switch capa 3. Como cada interfaz SVI está asociada con una VLAN específica tenemos que repetir el proceso para cada una:

```
Switch(config)#interface vlan 10
Switch(config-if)#description Gateway para VLAN 10 - 192.168.1.0/26
Switch(config-if)#ip address 192.168.1.1 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit

Switch(config)#interface vlan 11
Switch(config-if)#description Gateway para VLAN 11 - 192.168.1.64/26
Switch(config-if)#ip address 192.168.1.65 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit

Switch(config)#interface vlan 12
Switch(config-if)#description Gateway para VLAN 12 - 192.168.1.128/26
Switch(config-if)#ip address 192.168.1.129 255.255.255.192
Switch(config-if)#no shutdown 
Switch(config-if)#exit
```

#### Paso 3: Asignar los puertos físicos a cada VLAN

Una vez asignadas las VLAN, nos disponemos  a asignar los puertos físicos por donde se comunicará cada una. Para ello hacemos lo siguiente:

```
Switch(config)#interface gi1/0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface gi1/0/2
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 11
Switch(config-if)#exit

Switch(config)#interface gi1/0/3
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 12
Switch(config-if)#exit
```

#### Paso 4: Habilitar IP routing



#### Paso 5: Configurar el enlace troncal

Dado que tenemos el switch de capa 3 conectado al router R0 vamos a configurar el enlace troncal hacía el router. Para ello tenemos que configurar la interfaz de red Gi1/0/24 que conecta con el router y hacer lo siguiente:

```
Switch(config)#interface gi1/0/24
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk native vlan 99
Switch(config-if)#exit
```

#### Paso 6: Configurar gateway en el router R0 para cada VLAN

El comando `encapsulation dot1Q`  hace  referencia a la forma en que se etiquetan los paquetes  para una VLAN en redes Ethernet conmutadas. Más conocido como Dot1Q este estándar IEEE 802.1Q define el modo en que se implementan las VLAN en redes Ethernet. El `encapsulation dot1Q` implica agregar una etiqueta de VLAN a los paquetes Ethernet para indicar a qué VLAN pertenecen.

```
Router(config)#interface gi0/0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.1.1 255.255.255.192
Router(config-subif)#no shutdown 

Router(config)#interface gi0/0/0.11
Router(config-subif)#encapsulation dot1Q 11
Router(config-subif)#ip address 192.168.1.65 255.255.255.192
Router(config-subif)#no shutdown 
Router(config-subif)#exit

Router(config)#interface gi0/0/0.12
Router(config-subif)#encapsulation dot1Q 12
Router(config-subif)#ip address 192.168.1.129 255.255.255.192
Router(config-subif)#no shutdown 
Router(config-subif)#exit
```

Utilizar `encapsulation dot1Q` en un puerto de switch, hace que el switch añada una etiqueta de VLAN a cada paquete que entra o sale por ese puerto. Esta etiqueta es una parte adicional del encabezado Ethernet y contiene información sobre la VLAN a la que pertenece el paquete. El PC solo reconoce la red a la que pertenece, toda esta configuración es a nivel de swith.

Como la etiqueta `dot1Q` contiene información sobre la VLAN a la que pertenece el paquete esto le permite a los switches poder reconocer a que VLAN enviar cada paquete , así como permitir la segmentación de tráfico en redes con VLAN.

Cuando un switch recibe un paquete que lleva el `encapsulación` `dot1Q`, revisa la etiqueta de VLAN para determinar a cual pertenece dicho paquete y así poder utilizar esta información para reenviar el paquete solo a los puertos asociados con la VLAN especificada.

### Comprobando la conectividad entre todos los dispositivos

Llegados a este punto podemos comprobar si todo está correcto. Para ello podemos utilizar el comando ping para enviar paquetes entre los dispositivos.&#x20;

Para ello, podemos entrar al PC de la VLAN 10 / Command Prompt y hacemos un ping al router:

```
ping 192.168.1.1
```

<figure><img src="../../../.gitbook/assets/image (355).png" alt=""><figcaption><p>Comprobando la conexión entre los dispotivos de la VLAN 10 y el router </p></figcaption></figure>

Como se puede observar en el pantallazo anterior, hay conexión. Probamos con los siguientes dispositvos.

<figure><img src="../../../.gitbook/assets/image (356).png" alt=""><figcaption><p>Comprobando la conectivad con el router y las otras dos VLAN</p></figcaption></figure>



## Configurando el servicio DHCP y DNS en el router

<mark style="color:red;">**FALTA**</mark>



## Configuración entre los routers

Nos hemos "olvidado" de la conexión entre el router R0 y el R1 a través del cable serial. Para ello, utilizaremos una IP 10.10.10.0/24

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Red 10.10.10.0/24</p></figcaption></figure>

En el Router R0 configuramos la IP 10.10.10.1/24 para la interfaz de red Se0/1/0

```

Router>enable
Router#configure terminal
Router(config)#interface Serial0/1/0
Router(config-if)#ip address 10.10.10.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```

En el Router R1 configuramos la IP 10.10.10.2/24  para la interfaz de red Se0/1/0

```
Router>enable
Router#configure terminal
Router(config)#interface Serial0/1/0
Router(config-if)#ip address 10.10.10.2 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```

Por supuesto que todavía esto no es suficiente para establecer comuniación entre el PC de la VLAN 10 (por ejemplo) y el servidor de google que hemos puesto "afuera". Tenemos dos opciones que conozcamos en este momento: utilizar el protocolo dinámico RIP o bien el también protocolo dinámico OSPF. Vamos a configurar este último.



<mark style="color:red;">**FALTA**</mark> \


