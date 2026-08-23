---
description: Prevención de bucles
---

# 🚧 STP

Se trata de un protocolo de prevención o corrección de bucles que permite la redundancia y crea una topología de capa 2 sin bucles. El estándar original sería IEEE.1D para STP.

Cuando existen múltiples rutas entre dos dispositivos en una red Ethernet, y no hay implementado un árbol de expansión en los switches, se produce un bucle de capa 2. La redundancia de ruta elimina la posibilidad de un solo punto de falla.&#x20;

Un bucle de capa 2 puede:&#x20;

* provocar inestabilidad en la tabla de direcciones MAC
* saturación de enlaces y alta utilización de CPU en los switches y dispositivos finales, haciendo que la red se vuelva inutilizable.&#x20;

A diferencia de los protocolos de capa 3 (IPv4 e IPv6), la capa 2 no tiene un mecanismo para reconocer y eliminar tramas de bucle sin fin. Tanto IPv4 como IPv6 incluyen un mecanismo que limita la cantidad de veces que un dispositivo de red de capa 3 puede retransmitir un paquete. Un router disminuirá el TTL (tiempo de vida) en cada paquete IPv4 y el campo Límite de saltos en cada paquete IPv6. Cuando estos campos se reducen a 0, un router dejará caer el paquete. Los switches no poseen un mecanismo comparable para limitar el número de veces que un switch puede retransmitir una trama de capa 2.&#x20;

STP fue desarrollado específicamente como un mecanismo de prevención de bucles para Ethernet de Capa 2.

Si no tenemos STP habilitado, se pueden formar bucles de capa 2, lo que hace que las tramas de difusión, multidifusión y unidifusión desconocidos se reproduzcan sin fin. Esto puede derribar una red en un período de tiempo muy corto, a veces en pocos segundos.&#x20;

Por ejemplo, las tramas de difusión, como una solicitud ARP, se reenvían a todos los puertos del switch, excepto el puerto de entrada original. Esto asegura que todos los dispositivos en un dominio de difusión reciban la trama. Si hay más de una ruta para reenviar la trama, se puede formar un bucle infinito. Cuando se produce un bucle, la tabla de direcciones MAC en un switch cambiará constantemente con las actualizaciones de las tramas de difusión, lo que resulta en la inestabilidad de la base de datos MAC. Esto puede causar una alta utilización de la CPU, lo que hace que el switch no pueda reenviar tramas.

Las tramas de difusión no son el único tipo de tramas que son afectadas por los bucles. Si se envían tramas de unidifusión desconocidas a una red con bucles, se puede producir la llegada de tramas duplicadas al dispositivo de destino. Una trama de unidifusión desconocida se produce cuando el switch no tiene la dirección MAC de destino en la tabla de direcciones MAC y debe reenviar la trama a todos los puertos, excepto el puerto de ingreso.

### Tormentas de difusión

Una tormenta de difusión es un número muy elevado de emisiones que abruman la red en un breve período de tiempo. Una tormenta puede deshabilitar una red en segundos al abrumar switches y dispositivos finales.

Las tormentas se deben a hardware como puede ser una NIC defectuosa o a bucles de capa 2. Esto es:

* Causa física o lógica: Una tormenta de difusión puede originarse por un bucle de capa 2 o por un fallo de hardware (como una tarjeta NIC defectuosa que emite tramas corruptas).
* Diferencia IPv4 vs IPv6: En IPv6 no existe el tráfico de _broadcast_ tradicional. En su lugar, funciones como la resolución de direcciones (_ICMPv6 Neighbor Discovery_) emplean multidifusión de capa 2 (_multicast_).
* Efecto en el switch: Si un switch de capa 2 no conoce la tabla de grupos _multicast_ o no tiene STP habilitado, trata ese tráfico inundándolo por todos los puertos del dominio, generando la misma saturación que un _broadcast_.

### Algoritmo de árbol de expansión

STP se basa en un algoritmo creado por [Radia Perlman](https://es.wikipedia.org/wiki/Radia_Perlman) mientras trabajaba para [Digital Equipment Corporation](https://es.wikipedia.org/wiki/Digital_Equipment_Corporation), y que fue publicado en el artículo de 1985 "**Un algoritmo para la computación distribuida de un árbol de expansión en una LAN extendida**". Su algoritmo de árbol de expansión - STP -  crea una topología sin bucles al seleccionar un único puente raíz donde todos los demás conmutadores determinan una única ruta de menor costo.

Sin el protocolo de prevención de bucles, se producirían bucles que harían inoperable una red de switches redundantes.

Como pilar de este algoritmo, el primer paso crítico que ejecutan los switches al iniciar STP es la elección del puente raíz  o _Root Bridge_.

Para elegir al _Root Bridge_, todos los switches intercambian tramas especiales llamadas BPDU (_Bridge Protocol Data Unit_) para comparar su ID de puente, _Bridge ID_ o BID.

### ¿Cómo se crea un árbol de expansión?

Veamos este escenario donde se utilizan enlaces redundantes.&#x20;

<figure><img src="../../.gitbook/assets/imagen (3).png" alt=""><figcaption><p>Tomado de CCNA</p></figcaption></figure>

**(1) Seleccionar el puente (switch) o Root Bridge**

En la topología todos los enlaces tienen el mismo costo o ancho de banda. El STP busca el puente raíz único y todos los switches determinarán una única ruta de menor costo desde el propio switch hasta el puente (switch) raíz.

<figure><img src="../../.gitbook/assets/imagen (4).png" alt=""><figcaption><p>Tomado de CCNA</p></figcaption></figure>

**(2) Bloquear rutas redundantes**

El protocolo STP asegura que haya una sola ruta lógica entre todos los destinos en la red al bloquear intencionalmente las rutas redundantes que podrían causar un bucle. Al bloquear un puerto, se impide que los datos del usuario entren o salgan de ese puerto.

<figure><img src="../../.gitbook/assets/imagen (6).png" alt=""><figcaption><p>Tomado de CCNA</p></figcaption></figure>

**(3) Topología sin bucle**

Cuando bloqueamos un puerto el efecto que se consigue es el de convertir ese enlace en un vínculo "inutilizable" entre dos switches.

**(4) Errores que provocan nuevos cálculos**

Las rutas físicas siguen existiendo para proporcionar la redundancia, pero se deshabilitan para evitar que se generen bucles. En caso de haber una falla en un cable de red o en un switch, entonces se vuelven a recalcular las rutas y se desbloquean los puertos necesarios. Esto también puede ocurrir cuando se añade un nuevo switch a la topología.

