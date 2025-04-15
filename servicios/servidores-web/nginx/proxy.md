# 🚧 Proxy

### ¿Que es un proxy?

¿Qué es un proxy, servidor proxy o proxy web? Pues un servidor que se sitúa delante de un grupo de equipos cliente. Cuando los clientes realizan solicitudes a sitios y servicios en Internet, el servidor proxy intercepta esas peticiones y es quien se comunica con los servidores web en nombre de esos clientes, como un intermediario.

Le decimos proxy inverso a un servidor que se sitúa delante de los servidores web y reenvía las solicitudes del cliente a esos servidores web. Los proxies inversos se implementan para mejorar la seguridad, rendimiento y la fiabilidad.

En una comunicación estándar cliente - servidor, el cliente se comunica directamente con el servidor, de modo que el cliente enviaría las solicitudes al servidor y éste le respondería de manera directa al cliente.&#x20;

Cuando interviene un proxy en la comunicación, el cliente enviaría las solicitudes al proxy y sería éste quien se las reenviaría al servidor en cuestión. Lo mismo con la respuesta del servidor hacia el cliente: siempre pasarían por el proxy.

Veamos el diagrama siguiente donde tenemos los dispositivos involucrados en una comunicación vía proxy:&#x20;

* PC o cliente: el cliente que solicita la web. Le llamamos (A)
* Proxy: el servidor proxy intermediario, al que llamamos (B)
* Servidores web: a los que llamamos (C)

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Proxy de reenvío</p></figcaption></figure>

Depende de la ubicación del servidor proxy: delante del cliente o delante del servidor web estaríamos ante un servidor proxy de reenvío o un reverse proxy.

Un proxy inverso se sitúa delante de los servidores que pueden ser uno o varios  e intercepta las solicitudes de los clientes, pero dentro del perímetro de la red.



<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Proxy inverso</p></figcaption></figure>

Podemos decir que un proxy de reenvío al situarse delante del cliente, se asegura que ningún servidor se comunique directamente con él.  Por el contrario, un proxy inverso se sitúa delante del servidor y se asegura que ningún cliente se comunique directamente con ese servidor.

Un proxy inverso tiene una serie de ventajas entre las que tenemos:&#x20;

* load balancing - un sitio web puede recibir muchísimas peticiones de usuarios cada día, no ser capaz de manejar todo ese tráfico. Entonces, el sitio web puede estar distribuido entre un conjunto de servidores diferentes, todos ellos manejando solicitudes para el mismo sitio, proporcionando un equilibrio de carga, que distribuye el tráfico entrante de manera uniforme entre los diferentes servidores para evitar que un solo servidor se sobrecargue.&#x20;
* protección - un sitio web no necesariamente tiene por qué revelar la IP de su servidor, lo que dificulta posibles ataques contra el servidor.
* equilibrio de carga global (GSLB) - es una forma de equilibrio de carga, dado que un sitio web puede estar distribuido en varios servidores por el mundo y el proxy inverso enviará a los clientes al servidor que esté geográficamente más cercano. Esto permite reducir  las distancias que deben recorrer las solicitudes y las respuestas,  minimizando el tiempo de carga.



### Configuración de un reverse proxy con Nginx

Las condiciones sobre las que estoy trabajando:

* Utilizaré para ello dos VM conectadas en red NAT.&#x20;
* En ambas VM tengo configurado Nginx.
* Un Ubuntu Server con dos sitios web: punky.com:8080 (php) y kirby.com:8081
* En Ubuntu Desktop con la instalación de Nginx.

En el archivo de configuración de Nginx:

```
// Some code
```





