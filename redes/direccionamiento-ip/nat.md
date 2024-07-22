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

**Source Network Address Translation - SNAT:** Conocido como NAT de salida, SNAT cambia la dirección IP de origen de los paquetes que salen de la red local hacia Internet.

**Destination Network Address Translation - DNAT:**  También conocido como NAT de entrada, cambia la dirección IP de destino de los paquetes que llegan desde Internet hacia la red local.

**Masquerade**: Se trata de una forma especial de SNAT que se utiliza cuando la dirección IP pública puede cambiar (por ejemplo, conexiones de tipo dial-up o DHCP).

**Port Address Translation - PAT**: Conocido como NAT sobrecargado, PAT permite que múltiples dispositivos compartan una sola dirección IP pública al asignar diferentes números de puerto para cada sesión.

#### Ejemplo:

Supongamos que tienes una red local (en casa) con varios dispositivos: PC, teléfonos, etc. con direcciones IP privadas del tipo, 192.168.1.0/24. Tienes un router con una dirección IP pública del tipo 203.0.113.5. Entonces, NAT permitiría que todos los dispositivos en la LAN puedan acceder a Internet usando la dirección IP pública del router.&#x20;

Cuando un dispositivo de la LAN envía un paquete a Internet, el router cambia la dirección IP de origen del paquete a la dirección IP pública del router. En el momento en que la respuesta regresa, el router utiliza la tabla de traducción para redirigir el paquete de vuelta al dispositivo correcto en la red local.

