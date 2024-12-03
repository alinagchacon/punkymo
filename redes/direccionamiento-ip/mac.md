# MAC

La MAC address es un Identificador de 48 bits que designa de forma única una tarjeta de red o un puerto de un dispositivo de red. Se encuentra escrita en hexadecimal y la podemos encontrar:&#x20;

* Bloques de byte en byte separados por un guión: 00-50-1B-3C-70-08.&#x20;
* Bloques de byte en byte separados por dos puntos: 00:50:1B:3C:70:08.&#x20;
* Bloques de dos en dos bytes separados por un punto: 0050.1B3C.7008.&#x20;

La MAC está compuesta por:&#x20;

* los primeros 24 bits (OUI - Organizationally Unique Identifier)&#x20;
* los últimos 24 bits definidos por el fabricante para que las direcciones MAC sean únicas en el mundo.

Podemos diferenciar tres tipos de direcciones MAC:&#x20;

* Unicast: enviadas a un destino determinado. Constan de los 24 bits del OUI y los otros 24 del fabricante.&#x20;
* Broadcast: Los 40 bits de la dirección son todos unos en binario y que en hexadecimal sería: FFFF.FFFF.FFFF.&#x20;
*   Multicast: direcciones que tienen múltiples destinos para los mensajes. Aunque existen diferentes tipos de multicast,  nos centraremos en dos:&#x20;

    * las asociadas con IPv4, que comienzan por 0100.5EXX.XXXX, y obtienen los últimos 24 bits de la dirección de broadcast de la IPv4;&#x20;
    * las asociadas con IPv6, que comienzan por 3333.XXXX.XXXX, y cuyos últimos 32 bits se obtendrán de los últimos 32 bits de la dirección multicast de IPv6.



### Links

* [https://standards-oui.ieee.org/oui/oui.txt](https://standards-oui.ieee.org/oui/oui.txt)
