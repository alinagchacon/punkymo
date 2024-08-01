# Alias

&#x20;Los alias nos sirven para minimizar el número de cambios que tienen que hacerse si un equipo, red o de puerto es modificado.&#x20;

En este caso, crearíamos un par  de alias para probar el tráfico de datos a través de una DMZ considerando solo el protocolo ICMP (ping) y el puerto 80 para la web. Nos vamos a `Firewall > Alias > Port` y creamos el primero de los aliases, esto es:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Alias para los puertos 80 / 443 </p></figcaption></figure>

Hacemos lo mismo para el puerto 53 del DNS y nos debe quedar algo como lo siguiente:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Reglas para la DMZ

Para acceder a Internet necesitamos de los protocolos http (80) y https (443) con el protocolo TCP y UDP para la resoluciones del **DNS**. Para ello nos vamos a `firewall > rules` y vamos a configurar lo siguiente:

* Action: pass
* Interfaz: DMZ
* Address Family: IPv4
* Protocol: ICMP / Subtypes: any
* Source: DMZ subnets
* Destination: Any

Si probamos vemos que es capaz de hacer un ping tanto a una IP de Internet como es el caso de la 1.1.1.1 y a la VM que tenemos conectada a nuestra LAN con la IP 192.168.56.200. Sin embargo, si quisiera conectarme por SSH a la VM de la LAN no podré hacerlo.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Haciendo pruebas de conexión</p></figcaption></figure>

### Notas

Adicionalmente creé dos reglas: una regla para la WAN y otra para la DMZ permitiendo el tráfico por el puerto 53 de DNS.&#x20;

En el caso de la WAN sería permitiendo el tráfico con destino a la DMZ por el puerto 53 de DNS.

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Regla en la interfaz WAN para permitir el tráfico de DNS a través del puerto 53</p></figcaption></figure>

En el caso de la  DMZ sería permitiendo el tráfico con origen en la DMZ y hacia cualquier red por el puerto 53.

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption><p>Regla para la interfaz DMZ para permitir el tráfico de DNS a través del puerto 53</p></figcaption></figure>



Tengo que testearlo mejor para evitar que entre en conflicto con las reglas de la VPN que tengo establecida y asegurar que:

* desde la DMZ no haya acceso a la red LAN
* desde la WAN no tengamos acceso a la red LAN
* desde la LAN podamos tener acceso tanto a la DMZ como a la WAN

¿Me ayudas a pensar?&#x20;

