---
description: Apuntes
---

# Tipos de firewalls

### Firewall para filtrado de paquetes (sin estado)

Este tipo de firewall suele formar parte de un firewall de router, el cual autoriza o rechaza el tráfico a partir de información procedente de las capas 3 y 4 (modelo OSI). Son de los denominados firewalls <mark style="color:blue;">`sin estado`</mark> dado que utilizan una búsqueda simple en la tabla de políticas que filtra el tráfico teniendo en cuenta criterios específicos.

#### Ejemplo&#x20;

* Un servidor SMTP escucha el puerto 25 de manera predeterminada.&#x20;
* Un administrador puede configurar el firewall de filtrado de paquetes para bloquear el puerto 25 desde una estación de trabajo específica a fin de evitar que difunda un virus por correo electrónico.

Utilizar un firewall de filtrado de paquetes puede traer algunas ventajas dado que los filtros de paquetes:

* Implementan conjuntos simples de reglas de permiso o denegación.
* Tienen un bajo impacto en el rendimiento de la red.
* Son fáciles de implementar y son compatibles con la mayoría de los routers.
* Proporcionan un grado inicial de seguridad en la capa de red.
* Realizan casi todas las tareas de un firewall de gama alta a un costo mucho menor.

Hay que considerar que los filtros de paquetes no representan una solución de firewall completa, sin embargo si que son un elemento importante de una política de seguridad de firewall. Como nada es perfecto, existen algunas desventajas de usar un firewall de filtrado de paquetes:

* Son susceptibles a la **suplantación de IP**. Los atacantes pueden enviar paquetes arbitrarios que cumplan con los criterios de la ACL y pasar por el filtro.
* No filtran de manera confiable los **paquetes fragmentados**. Dado que los paquetes IP fragmentados llevan el encabezado TCP en el primer fragmento y los filtros de paquetes filtran la información del encabezado TCP, todos los fragmentos después del primero pasan sin condiciones. Las decisiones de usar filtros de paquetes suponen que el filtro del primer fragmento aplica con precisión la política.
* Utilizan ACL complejas, que pueden ser difíciles de implementar y mantener.
* No pueden filtrar dinámicamente ciertos servicios. Por ejemplo, las sesiones que utilizan negociaciones de puerto dinámicas son difíciles de filtrar sin abrir el acceso a una amplia gama de puertos.

Los filtros de paquetes no tienen estado. Examinan cada paquete individualmente y no en el contexto del estado de una conexión.

<figure><img src="../../.gitbook/assets/image (4) (2) (3).png" alt=""><figcaption><p>Filtrado de paquetes sin estado</p></figcaption></figure>

### &#x20;Firewall activo o stateful

Este tipo de firewall con estado son los más versátiles y más comúnmente usados. También conocidos como firewalls activos,  proporcionan un filtrado de paquetes utilizando la información de conexión que se mantiene en una tabla de estados. El filtrado con estado es una arquitectura de firewall que se clasifica en la capa de red. También analiza el tráfico en las capas 4 y 5 de OSI.

<figure><img src="../../.gitbook/assets/image (7) (5).png" alt=""><figcaption><p>Firewall activo o stateful</p></figcaption></figure>

&#x20;

### Ventajas y desventajas de este tipo de firewall

El uso de un firewall stateful en una red tiene varias **ventajas**:

* Se utilizan con frecuencia como medio principal de defensa, dado que filtra el tráfico no deseado o indeseable.
* Refuerzan el filtrado de paquetes proporcionando un control más estricto de la seguridad.
* Mejoran el rendimiento en comparación con los filtros de paquetes o los servidores proxy.
* Protegen contra la suplantación de identidad y los ataques de denegación de servicio determinando si los paquetes pertenecen a una conexión existente o provienen de una fuente no autorizada.
* Proporcionan más información de registro que un firewall de filtrado de paquetes.

Los firewalls stateful también tienen algunas **desventajas**:

* No pueden evitar ataques de la capa de aplicaciones (capa 7) porque no examinan el contenido real de la conexión HTTP.
* Existen protocolos que no tienen estado, como por ejemplo, UDP e ICMP que no generan información de conexión para una tabla de estado y, por lo tanto, no tienen tanto soporte para el filtrado.
* No es fácil realizar un seguimiento de las conexiones que utilizan la negociación dinámica de puertos. Algunas aplicaciones abren varias conexiones. Esto requiere un rango completamente nuevo de puertos que se deben abrir para permitir esta segunda conexión.
* No admiten la autenticación de usuarios.



| Ventajas                                                       | Desventajas                                                   |
| -------------------------------------------------------------- | ------------------------------------------------------------- |
| Medio de defensa principal                                     | No inspecciona la capa de aplicación                          |
| Filtrado de paquetes fuerte                                    | Limitado seguimiento de los protocolos sin estado (UDP, ICMP) |
| Registro de datos más rico                                     |                                                               |
| Mejor rendimiento que los filtros de paquetes                  | Difícil de defender contra la negociación dinámica de puertos |
| Defiende contra la suplantación de identidad y los ataques DoS | No tiene soporte de autenticación                             |

### Firewall del gateway de aplicaciones

Este tipo de firewall del gateway de aplicaciones o firewall proxy filtra la información en las capas 3, 4, 5 y 7 del modelo OSI.&#x20;

La mayor parte del control y filtrado del firewall se realiza en el software. Cuando un cliente necesita tener acceso a un servidor remoto, se conecta a un servidor proxy. El servidor proxy se conecta al servidor remoto en nombre del cliente. Por lo tanto, el servidor solamente ve una conexión desde el servidor proxy.

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>Firewall del Gateway de aplicaciones</p></figcaption></figure>

### Firewall de próxima generación

Los firewalls de próxima generación o <mark style="color:blue;">`NGFW`</mark> van más allá de los firewalls con estado dado que ofrecen:

* Prevención de intrusiones integrada.
* Control y reconocimiento de aplicaciones para ver y bloquear aplicaciones riesgosas.
* Rutas de actualización para incluir futuros datos de información.
* Técnicas para afrontar amenazas de seguridad en constante evolución.

<figure><img src="../../.gitbook/assets/image (5) (1) (4).png" alt=""><figcaption><p>Firewall de próxima generación</p></figcaption></figure>

Existen otros métodos de implementación de firewall que incluyen aspectos como:

* Firewall **basado en host** (servidor y personal): Una PC o servidor con software de firewall ejecutándose en él.
* Firewall **transparente**: Filtra el tráfico IP entre un par de interfaces puenteadas.
* Firewall **híbrido**: Una combinación de los distintos tipos de firewall. Por ejemplo, un firewall de inspección de aplicación combina un firewall con estado y un firewall de gateway de aplicación.
