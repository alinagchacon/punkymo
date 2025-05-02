# NAT / PAT

NAT o lo que es lo mismo - traducción de direcciones de red - es una técnica que permite la traducción de direcciones IP privadas en direcciones IP públicas. Se trata de mapear un espacio de direcciones IP en otro modificando la información de la dirección de red en el encabezado IP de los paquetes mientras están en tránsito a través de un router.&#x20;

**¿Por qué se utiliza NAT / PAT en redes?**&#x20;

Hay varios aspectos que permiten justificar el uso de NAT pero diría que lo principal fue la escasez de direcciones IPv4 desde la década de los 80. La cantidad de direcciones IPv4 públicas es limitada, unos 4 mil millones, que a priori parece un número grande, pero lo cierto es que hay muchos más dispositivos conectados a Internet.

Otros aspectos que justifican el uso de NAT/PAT son:

* Permite que <mark style="color:purple;">múltiples dispositivos dentro de una red privada compartan una única dirección IP</mark> pública.
* Funciona como una <mark style="color:purple;">barrera entre las redes internas y las externas</mark> (Internet), brindando seguridad.
* Los <mark style="color:purple;">dispositivos internos no son accesibles desde Internet</mark>, a menos que se configure explícitamente (por ejemplo, con port forwarding  - ver [pfSense Port Forward](../firewalls/dos-firewall/pfsense/dmz.md)) lo que reduce considerablemente la exposición a ataques provenientes de redes externas.
* Permite usar <mark style="color:purple;">rangos de IP privadas</mark> definidos por [RFC 1918](https://www.rfc-es.org/rfc/rfc1918-es.txt) (192.168.X.Y, 10.X.Y.Z, 172.16–31.X.Y), <mark style="color:purple;">que no necesitan coordinación globa</mark>l.
* Resulta <mark style="color:purple;">más fácil configurar, ampliar, modificar redes internas</mark> sin tener que depender del proveedor de servicios de Internet (ISP).



## Conceptos necesarios para comprender NAT/PAT

Veamos algunos conceptos importantes para comprender las diferentes técnicas de NAT/PAT:

1. **Regla ACL** es una instrucción que permite denegar o permitir el tráfico de la red en el router o switch, según se establezca en una interfaz de red y según determinados criterios como son el tipo de protocolo, el puerto, el origen del tráfico, etc.
2. Una **ACL** es un conjunto de reglas que se aplican en routers o switches para controlar el tráfico de la red.
3. **Inside global** – la IP pública (traducida) del dispositivo de la red interna LAN
4. **Inside local** – la IP real del dispositivo de la red interna LAN
5. **Outside local** – la IP interna del dispositivo externo (desde el punto de vista de la red interna).
6. **Outside global** – dirección real del dispositivo de la red externa, la IP pública real.
7. **NAT estática** -
8. **NAT dinámica** -
9. **PAT – una única IP** -
10. **PAT – un conjunto de IP** -&#x20;
11. **Máscara de wildcare** -

Veamos el modo de configurar cada uno de los tipos NAT/PAT que hay.

## Configuraciones

Para poder comprender y sobre todo testear las configuraciones posibles de NAT / PAT, lo primero que vamos a hacer es crear una topología básica como la que se muestra en la imagen. Una vez que lo tengamos bien configurado y testeado, lo copiamos y pegamos 4 veces en el mismo pkt. Al menos a mi me resulta más cómodo tener todos en un mismo espacio de trabajo.

<figure><img src="../../.gitbook/assets/image (425).png" alt="" width="563"><figcaption><p>Estructura base para configurar NAT / PAT.</p></figcaption></figure>

En cada servidor hemos configurado una web:

* server interno - www.lan.com
* server externo - www.wan.com

La siguiente tabla de direccionamiento muestra los detalles de la configuración:

| <mark style="color:purple;">Dispositivo</mark> | <mark style="color:purple;">Interfaz de red</mark> | <mark style="color:purple;">Gateway</mark> | <mark style="color:purple;">Otros</mark> |
| ---------------------------------------------- | -------------------------------------------------- | ------------------------------------------ | ---------------------------------------- |
| PC-LAN                                         | Fa0 – 192.168.1.11/24                              | 192.168.1.1/24                             | DNS apuntando al Server-LAN              |
| Server-LAN                                     | Fa0 – 192.168.1.10/24                              | 192.168.1.1/24                             | DNS apuntando al Server-LAN              |
| <p><br></p><p>Router</p>                       | Gi0/0 – 192.168.1.1/24                             |                                            | <p><br></p>                              |
| Router                                         | Gi0/1 – 80.0.0.1/29                                | <p><br></p>                                |                                          |
| PC-WAN                                         | Fa0 – 80.0.0.11/8                                  | 80.0.0.1/8                                 | DNS apuntando al Server-WAN              |
| Server-WAN                                     | Fa0 – 80.0.0.10/8                                  | 80.0.0.1/8                                 | DNS apuntando al Server-WAN              |

\
NAT estática
------------

La NAT estática es una asignación de uno a uno entre una dirección IP interna y una IP externa. Permite que los dispositivos externos hagan conexión a los internos mediante la IP pública asignada de forma estática.

### **Pasos para configurar el NAT estático**

1. Establecer la configuración de las interfaces de red correspondientes a la red interna y la externa.
2. Crear una asignación entre la dirección local interna y las direcciones globales internas

Entonces, configurando o (revisando) la configuración del router, podemos añadir la descripción de la interfaz de red y además, establecemos si la red es Interna (Inside) o Externa (Outside):

```
Router(config)#interface GigabitEthernet0/0/0
Router(config-if)#ip 192.168.1.1 255.255.255.0
Router(config-if)#description LAN – 192.168.1.0/24
Router(config-if)#ip nat inside
Router(config-if)#exit

Router(config)#interface GigabitEthernet0/0/1
Router(config-if)#description WAN – 80.0.0.0/8
Router(config-if)#ip address 80.0.0.1 255.0.0.0
Router(config-if)#ip nat outside
Router(config-if)#exit
```

Finalmente, utilizamos el siguiente comando con el que indicamos que la IP  de la LAN - 192.168.1.10 (servidor interno) es la que se va a traducir de manera estática a un IP pública 80.0.0.15 que es la que se utilizará para representar al servidor interno (en nuestro ejemplo) en Internet.

#### ¿Qué hace esto en la práctica?

Cada vez que alguien desde fuera, la WAN, acceda a la IP pública **80.0.0.15**, el router redirigirá ese tráfico al dispositivo con IP privada **192.168.1.10**, y viceversa. Es útil, por ejemplo, para que un servidor local (como un servidor web o FTP) sea accesible desde fuera de la red local.

```
Router(config)#ip nat inside source static 192.168.1.10 80.0.0.15 
```

### Verificando la conectividad

Si queremos visualizar las traducciones realizadas usamos el comando siguiente y nos mostraría algo como lo siguiente:

```
#show ip nat translations
```

<figure><img src="../../.gitbook/assets/image (420).png" alt=""><figcaption><p>show ip nat translations</p></figcaption></figure>

### **Notas**

1. Observa que se muestra tanto el protocolo como los puertos utilizados en cada traducción.
2. Haz ping desde el CMD entre los diferentes dispositivos.



## NAT dinámico

En este caso, varios dispositivos de la red privada LAN tienen acceso a la pública WAN utilizando para ello un conjunto compartido de direcciones IP públicas, pero sin utilizar la misma IP pública.

Desde la red externa – WAN vamos a tomar el rango de 8 direcciones globales que van de la 80.0.0.16 a la 80.0.0.23 con la máscara 255.255.255.248 y se lo vamos a asignar a la red 192.168.1.0/24.

_**Notas**:_&#x20;

_En este caso he modificado la red externa y he utilizado un prefijo /29, esto es:_

_La red WAN – externa tiene una IP 80.0.0.0 y es de clase A. Sin embargo, he tomado 29 bits para la red, por lo que nos quedan 3 bits para los host, con lo cual podemos establecer:_

* la red WAN en el rango 80.0.0.0 – 80.0.0.7
* el pool para el NAT dinámico en el rango 80.0.0.16 – 80.0.0.23

### &#x20;**Pasos a seguir**

1. _Definir el conjunto de direcciones globales que se debe usar para la traducción → 80.0.0.16 – 80.0.0.23_
2. _Configurar una lista de acceso ACL de tipo estándar que permita el rango de direcciones que se deben traducir._
3. _Definir las direcciones IP de cada interface de red:_ la red interna y la externa.
4. Crear el mapeo de las direcciones del pool que se han de asignar a las direcciones autorizadas por la ACL.

### **Comenzando la configuración**

_**Paso 1**: Definir el conjunto de direcciones globales que se debe usar para la traducción._

```
Router(config)#ip nat pool MyPool 80.0.0.16 80.0.0.23 netmask 255.255.255.248
```

_**Paso 2**: Configurar una lista de acceso ACL de tipo estándar que permita el rango de direcciones que se deben traducir:_

```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
```

_**Paso 3**: Definir las direcciones IP de cada interface de red._

```
Router(config)#interface gi0/0/0
Router(config)#description LAN1 – 192.168.1.0/24
Router(config)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#ip nat inside
Router(config-if)#exit

Router(config)#interface gi0/0/1
Router(config)#description WAN - 80.0.0.0/29
Router(config)#ip address 80.0.0.1 255.255.255.248
Router(config-if)#ip nat outside
Router(config-if)#exit
```

\
&#xNAN;_**Paso 4**: Establecer la traducción dinámica de origen utilizando la lista de acceso – paso 2._

```
Router(config)#ip nat inside source list 1 pool MyPool
Router(config-if)#exit
```

### Verificando la conectividad

Podemos visualizar cómo ha quedado configurado el router:

```
show running-config
```

_Veremos algo como:_

<figure><img src="../../.gitbook/assets/image (421).png" alt="" width="333"><figcaption><p>Configuración de las interfaces de red en el router</p></figcaption></figure>

Así como el pool que hemos establecido:

<figure><img src="../../.gitbook/assets/image (422).png" alt="" width="563"><figcaption><p>La regla ACL establecida y el pool creado</p></figcaption></figure>

Solo nos queda comprobar el funcionamiento:

```
show ip nat translations 
```

para comprobar las conexiones realizadas, como muestra el pantallazo siguiente:

<figure><img src="../../.gitbook/assets/image (423).png" alt=""><figcaption><p>show ip nat translations</p></figcaption></figure>

## PAT - dirección única

PAT se conoce también como _`NAT con sobrecarga`_ y permite que se puede utilizar una única dirección IPv4 pública para muchas direcciones IP (privadas), incluso miles de direcciones IPv4 privadas internas.

Cuando se configura el PAT, el router mantiene información de los protocolos de nivel superior, de los puertos TCP o UDP, para traducir de la dirección global interna a la dirección local interna correcta.

Cuando se asignan varias direcciones locales internas a una dirección global interna, los números de puerto TCP o UDP de cada host interno distinguen entre las direcciones locales.

Partiendo de la misma configuración básica:

<figure><img src="../../.gitbook/assets/image (424).png" alt=""><figcaption><p>PAT - una única IP global</p></figcaption></figure>

### _**Pasos a seguir**_

1. Identificar las interfaces interna y externa
2. Definir una lista de acceso estándar ACL que permita las direcciones que se deben traducir:

```
access-list 1 permit 192.168.1.0 0.0.0.255
```

1. Especificar las opciones de ACL, interfaz de salida y sobrecarga para establecer la traducción dinámica de origen.

```
ip nat inside source list 1 interface gi0/0/1 overload
```

Nota: Se configura de modo similar al [NAT dinámic](https://ccnadesdecero.es/configuracion-nat-dinamica/)o, excepto que, en lugar de un conjunto de direcciones, se utiliza una interface para identificar la dirección IPv4 externa.

### **Configuración del Router**

```
Router(config)#interface gi0/0/0
Router(config-if)#ip nat inside 
Router(config-if)#exit

Router(config)#interface gi0/0/1
Router(config-if)#ip nat outside
Router(config-if)#exit

Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#ip nat inside source list 1 interface gi0/0/1 overload
Router(config)#exit
```

### Verificación de la conectividad

Haz pruebas de conectividad y comprueba las traducciones realizadas como se muestra en el pantallazo a continuación.

<figure><img src="../../.gitbook/assets/image (426).png" alt=""><figcaption><p>Prueba de las traducciones de NAT</p></figcaption></figure>

¿Qué se muestra cuando haces un ping del server externo IP=80.0.0.10 al server interno IP=192.168.1.10?



## PAT - Un rango de IP

En este caso, se asigna más de una dirección IPv4 pública a la red interna. Algo que haría el ISP, por ejemplo.

Las direcciones IP pueden ser parte de un conjunto, de modo similar a como lo hace el NAT dinámico, pero hay un detalle: y es que no existen suficientes direcciones públicas para realizar una asignación de uno a uno entre direcciones internas y externas. Esto quiere decir que una gran cantidad de dispositivos comparte un pequeño conjunto de direcciones.

### Pasos a seguir

1. Establecer los parámetros de las interfaces interna y externa
2. Definir el conjunto de direcciones globales que se debe usar para la traducción de sobrecarga.

```
ip nat pool MyPool 80.0.0.8 80.0.0.15 netmask 255.255.255.248
```

3. Definir una lista de acceso estándar que permita las direcciones que se deben traducir

```
access-list 1 permit 192.168.1.0 0.0.0.255
```

3. Especificar la lista de acceso y el conjunto que se definieron en los pasos anteriores para establecer la traducción de sobrecarga

```
ip nat inside source list 1 pool MyPool overload
```

Vamos a comenzar la configuración en el router.

### Configurando el router

1. Definir las redes interna y externa como hemos hecho anteriormente, ponemos una descripción de la interfaz (realmente lo podíamos haber hecho en la topología base de la que partimos al igual que el especificar si es inside o outside la red.

```
Router(config)#interface gi0/0/0
Router(config-if)#description LAN - 192.168.1.0/24
Router(config-if)#ip nat inside 
Router(config-if)#exit
                                                           
Router(config)#interface gi0/0/1
Router(config-if)#description WAN – 80.0.0.0/8
Router(config-if)#ip nat outside 
Router(config-if)#exit
```

2. Crear el pool de direcciones IP de la red pública, de hecho, una subred

```
Router(config)#ip nat pool MyPool 80.0.0.8 80.0.0.15 netmask 255.255.255.248
```

3. Definir la ACL de tipo estándar permitiendo el tráfico con origen en la red interna

```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
```

4. Relacionar la ACL y el pool indicándole el parámetro “overload” como indicativo del uso de PAT en lugar de NAT dinámico.

```
Router(config)#ip nat inside source list 1 pool MyPool overload 
```

### Verificando la conectividad

Volvemos a usar el mismo comando que nos permite visualizar las traducciones de las IP interna a pública:

<figure><img src="../../.gitbook/assets/image (429).png" alt=""><figcaption><p>show ip nat translation</p></figcaption></figure>

Incluso, si hacemos un ping desde el dispositivo 80.0.0.11  de la red pública, hacia el servidor de la red interna vemos el reply viene de la IP pública correspondiente.

<figure><img src="../../.gitbook/assets/image (428).png" alt=""><figcaption><p>El reply del ping proviene de la IP pública asignada</p></figcaption></figure>



## <mark style="color:purple;">(5) PAT - Mapeo estático</mark>

A la hora de configurar este tipo de asignación estática se tiene que indicar el socket en lugar de la IP para ambas direcciones y además, el tráfico puede ser TCP o UDP lo que se debe indicar también. Cuando configuramos el mapeo estático de un socket se denomina “publicar un servicio” en NAT aunque lo conocemos más como “abrir un puerto” en el Router.

### ¿Qué pasos debemos dar?

1. Configurar la PAT como anteriormente
2. Crear la asignación estática

Vamos a ver un ejemplo: supongamos que queremos publicar el servidor web con IP 192.168.1.10 a través del puerto 80 en la IP 80.0.0.10 en el mismo puerto 80. Podemos copiar y pegar la configuración de PAT con un rango de IP o volver a configurar el router:

```
Router(config)#interface gi0/0/0
Router(config-if)#description LAN – 192.168.1.0/24
Router(config-if)#ip nat inside
Router(config-if)#exit

Router(config)#interface gi0/0/1
Router(config-if)#description WAN – 80.0.0.0/8
Router(config-if)#ip nat outside 
Router(config-if)#exit

Router(config)#ip nat pool myPool 80.0.0.8 80.0.0.15 netmask 255.255.255.248
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#ip nat inside source list 1 pool myPool overload 
```

Hasta este punto, todo es igual, solo tenemos que agregar la siguiente línea que establece que el servidor interno: 192.168.1.10 por el puerto 80 de http se transforma en la IP pública  80.0.0.5.

```
Router(config)#ip nat inside source static tcp 192.168.1.10 80 80.0.0.5 80
Router(config)#exit
```



