# NAT

Un router conecta una red con otra. El NAT actúa desde el router y su función es traducir las direcciones IP privadas (internas de la red) a la dirección IP pública asignada o conjunto de direcciones IP asignadas, porque pudiera ser un "paquete" de direcciones IP.

(La traducción de direcciones de red, también llamado enmascaramiento de IP o NAT (del inglés Network Address Translation), es un mecanismo utilizado por routers IP para intercambiar paquetes entre dos redes que asignan mutuamente direcciones incompatibles.)

**¿Por qué se hace esto?**

Hace bastantes años se dieron cuenta que nos quedaríamos sin direcciones IP para asignar. Piensa en las IPv4 de 32 bits. De hecho, fue alrededor del año 2011 que se entregó a Asia el último paquete de direcciones libres que quedaban por asignar en el mundo.

Al aumentar drásticamente el número de equipos conectados a Internet se empezaron a agotar las direcciones IP. Por eso, se inventaron el NAT. Mientras tanto, han puesto en funcionamiento las IPv6 con un largo de 128bits y que, se supone, no se agotarán. Creo que comenzaron a funcionar con éstas IPv6 organismos gubernamentales y militares, ya se ha ido expandiendo el uso y todos tenemos asignada una IPv6 incluso en casa y ésta si que es pública tanto dentro como fuera.

Todos los equipos que puedas tener conectados en casa, tienen direcciones IP de clase C, esto es: 192.168.X.Y La clase C es también llamada "privada" porque nunca "navegamos" en Internet desde IP de este tipo.

Haz la siguiente comprobación. Intenta comprobar las direcciones IP de los equipos de casa: Móviles, PCs. Deberían tener 192.168.1.100 (por ejempplo). Sin embargo, cuando navegas por Internet, no sales desde esa IP sino que se te asigna una IP pública para navegar, pero lo mejor es que todos los equipos de casa salen a navegar a Internet con la misma IP pública.

En el aula le pido a los estudiantes que comprueben su IP en los equipos, siempre es 192.168.X.Y y navegamos en Internet todos con la misma dirección IP pública (clase A que va desde 1 a 127 (mirando el primer octeto de la IPv4).

1 - 127 clase A&#x20;

128 - 191 clase B&#x20;

192 - 223 clase C&#x20;

224 - 239 clase D (multidifusión, no se asignan IP)&#x20;

240 - 255 clase E (experimental)

El router ya viene así, no necesitas configurarlo aunque si se pueden configurar y redirigir el tráfico a un equipo determinado. No lo tengo claro del todo.

En casa no necesitas hacer nada de esto porque ya tienes un router que funciona de servidor DHCP y te asigna el rango de IP a tu equipo. Puedes y debes hacer otras reconfiguraciones como: cambiar el password de fábrica, crear subredes para dejar una "privada" y otra para los amigos que vengan a casa y otros detalles que son interesantes y puedes ver directamente desde tu equipo y tecleando la IP del router: 192.168.1.1 (ó 192.168.0.1)

Por tanto, el router ya te provee las IPs y generalmente asigna la misma IP a cada equipo.

Tu equipo no es visible desde fuera, aunque yo me he conectado por SSH desde el trabajo a mi casa.

<mark style="color:red;">Falta lo que hago en asix1 ...</mark>
