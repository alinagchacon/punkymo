# NAT / PAT

NAT o traducción de direcciones de red es una técnica que permite la traducción de direcciones IP privadas en direcciones IP públicas. Se trata de mapear un espacio de direcciones IP en otro modificando la información de la dirección de red en el encabezado IP de los paquetes mientras están en tránsito a través de un router.&#x20;

**¿Por qué se utiliza NAT / PAT en redes?**&#x20;

Hay varias para justificar el uso de NAT pero diría que la principal fue la escasez de direcciones IPv4 desde la década de los 80. La cantidad de direcciones IPv4 públicas es limitada, unos 4 mil millones, que, a priori parece un número grande pero lo cierto es que hay muchos más dispositivos conectados a Internet.

Adicionalmente, NAT/PAT:

* Permite que múltiples dispositivos dentro de una red privada compartan una ú nica dirección IP pública.
* Funciona como una **barrera entre las redes internas y las externas** (Internet), brindando seguridad.
* Los dispositivos internos no son accesibles desde Internet, a menos que se configure explícitamente (por ejemplo, con port forwarding  - ver [pfSense Port Forward](../firewalls/dos-firewall/pfsense/dmz.md)) lo que reduce considerablemente la exposición a ataques provenientes de redes externas.
* Permite usar rangos de IP privadas definidos por [RFC 1918](https://www.rfc-es.org/rfc/rfc1918-es.txt) (192.168.X.Y, 10.X.Y.Z, 172.16–31.X.Y), que no necesitan coordinación global.
* Resulta más fácil configurar, ampliar, modificar redes internas sin tener que depender del proveedor de servicios de Internet (ISP).



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

Lo primero que vamos a hacer es crear una topología básica como la que se muestra en la imagen. Una vez que lo tengamos bien configurado, lo copiamos y pegamos 4 veces en el mismo pkt. Al menos a mi me resulta más cómodo tener todos en un mismo espacio de trabajo.

<figure><img src="../../.gitbook/assets/image (418).png" alt=""><figcaption></figcaption></figure>

En cada servidor hemos configurado una web:

server interno - www.lan.com

server externo - www.wan.com

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

**Pasos para configurar el NAT estático**:

1. Establecer la configuración de las interfaces de red correspondientes a la red interna y la externa.
2. Crear una asignación entre la dirección local interna y las direcciones globales internas

Por tanto:

```
Router(config)#interface GigabitEthernet0/0/0
Router(config-if)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#description LAN – 192.168.1.0/24
Router(config-if)#ip nat inside
Router(config-if)#exit

Router(config)#interface GigabitEthernet0/0/1
Router(config-if)#description WAN – 80.0.0.0/8
Router(config-if)#ip address 80.0.0.1 255.0.0.0
Router(config-if)#ip nat outside
Router(config-if)#exit
```

Finalmente, utilizamos la IP del servidor de la red interna para nuestras pruebas:

```
Router(config)#ip nat inside source static 192.168.1.10 80.0.0.15 
```

### Verificando la conectividad

Si queremos visualizar las traducciones realizadas usamos el comando siguiente y nos mostraría algo como lo siguiente:

```
#show ip nat translations
```

<figure><img src="../../.gitbook/assets/image (420).png" alt=""><figcaption><p>show ip nat translations</p></figcaption></figure>

**Notas**:

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

\
**Pasos a seguir**:

1. _Definir el conjunto de direcciones globales que se debe usar para la traducción → 80.0.0.16 – 80.0.0.23_
2. _Configurar una lista de acceso ACL de tipo estándar que permita el rango de direcciones que se deben traducir._
3. _Definir las direcciones IP de cada interface de red:_ la red interna y la externa.
4. Crear el mapeo de las direcciones del pool que se han de asignar a las direcciones autorizadas por la ACL.

**Comenzando la configuración**:

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

Si hacemos:

```
show running-config
```

_Veremos algo como:_

<figure><img src="../../.gitbook/assets/image (421).png" alt="" width="333"><figcaption><p>Configuración de las interfaces de red en el router</p></figcaption></figure>

Así como el pool que hemos creado:

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

_**Pasos a seguir**:_

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

