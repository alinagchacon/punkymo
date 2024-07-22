# NAT

El protocolo de red NAT (Network Address Translation) surge principalmente por la escasez de direcciones IPv4. Las direcciones IPv4 se encuentran formadas por 32 bits y permiten tener un total de 4.3 millones de direcciones IP únicas.  Al inicio parecía una cantidad razonable pero a medida que Internet creció se hizo evidente que este espacio de direcciones no sería suficiente para asignar una dirección IP única a cada dispositivo conectado a la red mundial.&#x20;

La escasez de direcciones IPv4 fue uno de los impulsores más importantes para la adopción de NAT dado que:

* Múltiples dispositivos en una red local pueden compartir una sola dirección IP pública. Detalle importante para reducir la demanda de direcciones IPv4 públicas.
* Proporciona seguridad al ocultar las direcciones IP internas de la red local del mundo exterior dado que las direcciones IP privadas no son enrutables en Internet, por tanto los dispositivos de una LAN están ocultos y protegidos de accesos directos desde el exterior. Sería el router el que ejecuta el protocolo NAT u actúa como un intermediario, dificultando el acceso a los dispositivos internos.
* Proporciona una capa adicional de control de acceso, ya que solo los paquetes que cumplen con ciertas reglas pueden ser enrutados entre la red interna y externa.
* Las direcciones IP privadas pueden ser reutilizadas en diferentes redes locales sin riesgo de conflicto. Esto facilita la administración de redes internas y la creación de subredes.
* NAT permite una configuración más flexible y dinámica de las redes, especialmente cuando se combina con DHCP (Dynamic Host Configuration Protocol) para la asignación automática de direcciones IP internas.



Por otra parte, aunque IPv6 se diseñó para solucionar la escasez de direcciones IP puesto que proporciona un espacio de direcciones mucho más grande (2^128), su adopción ha sido lenta debido a la infraestructura existente y la compatibilidad. Por esta razón NAT sigue siendo relevante y ampliamente utilizado, especialmente en redes IPv4.

### Direcciones IP privadas y públicas

Los rangos de direcciones IP que podemos utilizar para asignar a los hosts son de las clases A,B y C. Las mismas se pueden clasificar teniendo en cuenta dos criterios:&#x20;

* **Direcciones IP privadas**: no se pueden enrutar en Internet, pero sí en redes privadas.&#x20;
* **Direcciones IP públicas**: son enrutables en Internet

Cada clase A, B y C tiene un segmento de red privado y son:

* Clase A: La IP <mark style="color:blue;">10.0.0.0</mark> a <mark style="color:blue;">10.255.255.255</mark>
* Clase B: La <mark style="color:blue;">172.16.0.0</mark> a <mark style="color:blue;">172.31.255.255</mark>.&#x20;
* Clase C: La IP <mark style="color:blue;">192.168.0.0</mark> a <mark style="color:blue;">192.168.255.255</mark>.

NAT mantiene una tabla de traducción que asocia las direcciones IP privadas con las direcciones IP públicas y puertos para realizar la conversión bidireccional de las direcciones.

### Tipos de NAT

Los diferentes tipos de NAT que se pueden aplicar son:



#### NAT estática <a href="#nat-estatica" id="nat-estatica"></a>

Traducen una dirección privada en una misma dirección pública Por ejemplo, un dispositivo tiene una dirección IP privada del tipo 192.168.1.13 y sale a internet con la IP pública 200.167.220.220. En este caso se realiza una traducción de uno a uno. Puede ser muy útil en aquellos dispositivos que deben tener siempre la misma dirección para que sea accesible desde internet, por ejemplo, un servidor web.

#### NAT dinámica <a href="#nat-dinamica" id="nat-dinamica"></a>

El router tiene un grupo de direcciones IP públicas y a cada dirección privada le pertenece al menos una dirección IP pública del grupo. Parecido al Nat estático, pero ofrece la posibilidad de asignar varias direcciones IP.

Tiene como ventaja que se pueden ocultar las direcciones internas de la LAN asociándolas con la IP pública, sin embargo solo se pueden traducir tantas IP privadas como IP públicas se tengan contratadas.

#### NAT con sobrecarga <a href="#nat-con-sobrecarga" id="nat-con-sobrecarga"></a>

Es la opción que tenemos en nuestros hogares puesto que nos conectamos a través de un router, proporcionado por el ISP que hayamos contratado. Los dispositivos se conectan a internet a través de una única IP pública.

Cuando un dispositivo de la red interna quiere establecer una conexión, el router guarda su IP privada, asocia su puerto de origen con una IP pública y un puerto al azar (por encima de 1024). Así, cuando llega información a ese puerto, el router comprueba la tabla y lo reenvía a la IP privada y al puerto que corresponda.

#### PAT <a href="#pat" id="pat"></a>

Este método permite que diferentes IP privadas se conecten a internet usando una única IP pública. En este caso la traducción se realiza usando puertos.

Esta forma permite ocultar las direcciones IP privadas que tenemos en nuestra LAN, lo que brinda mayor seguridad.  PAT es un modo de NAT dinámico.

#### Ejemplo:

Supongamos que tienes una red local (en casa) con varios dispositivos: PC, teléfonos, etc. con direcciones IP privadas del tipo, 192.168.1.0/24. Tienes un router con una dirección IP pública del tipo 203.0.113.5. Entonces, NAT permitiría que todos los dispositivos en la LAN puedan acceder a Internet usando la dirección IP pública del router.&#x20;

Cuando un dispositivo de la LAN envía un paquete a Internet, el router cambia la dirección IP de origen del paquete a la dirección IP pública del router. En el momento en que la respuesta regresa, el router utiliza la tabla de traducción para redirigir el paquete de vuelta al dispositivo correcto en la red local.



\
