---
description: Servidores proxies
---

# Proxy

## ¿Qué es un servidor proxy?

Un servidor proxy brinda una puerta de enlace entre los usuarios e Internet. Suele llamarse servidor intermediario, porque está entre los usuarios finales y las páginas web.

Si bien todos los servidores proxy les dan a los usuarios una dirección alternativa con la cual usar Internet, hay varios tipos diferentes, cada uno con sus propias funciones.

## Ventajas de usar un servidor proxy para las empresas

Algunas de las ventajas de configurar un servidor proxy en una empresa son:

* **Mejora de la seguridad**. Actuan como un firewall entre su sistema e Internet. Sin ellos, los piratas informáticos tienen fácil acceso a su dirección IP y pueden usarla para infiltrarse en su computadora o red.
* **Navegación, observación, escucha y compras privadas**. Permite que no lo inunden los anuncios no deseados o recopilen datos específicos de IP.
* **Acceso a contenido específico de la ubicación**. Puede designar un servidor proxy con una dirección asociada con otro país. De hecho, puede hacer que parezca que usted se encuentra en ese país y obtener acceso total a todas las computadoras de contenido de ese país con las que puede interactuar.
* **Evitar que los empleados exploren sitios inadecuados**. Puede usarlo para bloquear el acceso a sitios web que están en contra de los principios de su organización. Además, puede bloquear sitios que suelen terminar distrayendo a los empleados de tareas importantes. Algunas organizaciones bloquean los sitios de redes sociales como Facebook y otros para eliminar las tentaciones que los hacen perder tiempo.

## Tipos de servidores proxy

Todos los proxy proporcionan a los usuarios una dirección alternativa para navegar en Internet, sin embargo existen varios tipos diferentes, cada uno con sus propias funciones.



### Proxy de reenvío

Se encuentra frente a los clientes y se utiliza para obtener datos para grupos de usuarios dentro de una red interna. Cuando se envía una solicitud, el servidor proxy la examina para decidir si debe continuar y realizar una conexión.

Este tipo de proxy es adecuado para redes internas que necesitan un único punto de entrada. Brindan:

* seguridad de dirección IP para quienes están en la red
* y permite un control administrativo directo.

Sin embargo, limitan la capacidad de una organización para satisfacer las necesidades de los usuarios finales individuales.

\
\
**Ejemplo**: Veamos el ejemplo siguiente (tomado de Cloudflare) donde tenemos los 3 tipos de dispositivos que pueden estar involucrados en una comunicación vía proxy:

* A hace de PC de un usuario
* B: sería un servidor proxy de reenvío
* C: Un sitio web



<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Tomado de Cloudflare</p></figcaption></figure>

\
\
En una comunicación estándar de Internet, el PC A se comunicaría directamente con el servidor C. Sin embargo, cuando hay un proxy de reenvío, el cliente A envía las solicitudes al proxy B, y éste es quien reenviará la solicitud al servidor C.

Lo mismo a la inversa, el servidor C enviará la respuesta al proxy B, que reenviará a su vez la respuesta al cliente A.\


#### ¿Por qué se utilizaría un proxy de reenvío? 

1. **Evitar restricciones de navegación estatales o institucionales.** Algunos gobiernos e instituciones utilizan los firewalls para dar a sus usuarios acceso a una versión limitada de Internet.\
   Un proxy de reenvío permite eludir estas restricciones, ya que permiten que el usuario se conecte al proxy en lugar de hacerlo directamente a los sitios que está visitando.
2. **Bloquear acceso a determinados contenidos.** Un proxy puede configurarse para bloquear el acceso de un grupo de usuarios a determinados sitios.\
   Por ejemplo, en un colegio se puede configurar para conectarse a la web a través de un proxy que active reglas de filtrado de contenidos, y rechace el reenvío de respuestas de determinados sitios web.
3. **Proteger su identidad en línea.** En algunos casos, pudiéramos querer un mayor anonimato online;en otros casos, se trataría de eludir la dureza con la que actúan algunos gobiernos (donde viven), por ejemplo el caso de disidentes políticos, criticas al gobierno en redes sociales que pueden traer penas de cárcel, etc.\
   Cuando se utiliza un proxy de reenvío para conectarse a un sitio web la dirección IP que se utiliza será más difícil de rastrear. Solo será visible la dirección IP del servidor proxy.

### Proxy inverso

Este tipo de proxy se coloca delante de los servidores web y reenvía las solicitudes desde un navegador a los servidores web. Funciona interceptando las solicitudes del usuario en el borde de la red del servidor web. Luego envía las solicitudes al servidor de origen y recibe las respuestas de este.

Estos proxies inversos constituyen una opción muy buena para sitios web populares que necesitan equilibrar la carga de muchas solicitudes entrantes dado que ayudan a reducir el ancho de banda actuando como otro servidor web que administra las solicitudes entrantes.

La desventaja es que los proxies inversos pueden exponer potencialmente la arquitectura del servidor HTTP si un atacante logra penetrar en ella. Esto significa que es posible que los administradores de red tengan que reforzar o reposicionar su firewall si están usando un proxy inverso.

\
La diferencia entre un proxy de reenvío y un proxy inverso es sutil pero importante de comprender. Una forma de verlo sería:

* un proxy de reenvío se sitúa delante de un cliente y se asegura de que ningún servidor de origen se comunique nunca directamente con ese cliente específico.
* un proxy inverso se sitúa delante de un servidor de origen y se asegura de que ningún cliente se comunique nunca directamente con ese servidor de origen.

Veamos un ejemplo tomado de Cloudflare:

* D: Equipos personales de usuarios.
* E: Un servidor de proxy inverso
* F: Uno o más servidores web



<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>Tomado de cloudflare</p></figcaption></figure>

\
Lo habitual es que todas las solicitudes de D vayan directamente a F, y F enviaría las respuestas directamente a D. Sin embargo, con un proxy inverso, todas las solicitudes de D irán directamente a E, y E las enviaría a F, por lo que también recibiría las respuestas de F. y se las pasaría a D.

#### Algunas de las ventajas de tener un proxy inverso

1. **Load balancing.** Si tenemos un sitio web muy popular, que recibe millones de usuarios cada día, pero que no es capaz de manejar el tráfico entrante con un único servidor entonces, se puede configurar el sitio de modo que pueda estar distribuido entre un conjunto de servidores diferentes, todos ellos manejando solicitudes para el mismo sitio.\
   \
   En esta situación un proxy inverso puede brindar equilibrio de carga, distribuyendo el tráfico entrante de manera uniforme entre los diferentes servidores evitando la sobrecarga de uno solo.\
   \
   En caso de que un servidor falle los otros servidores pueden intervenir para gestionar el tráfico.\

2. **Protección ante ataques.** Si tenemos un proxy inverso instalado, nuestro sitio web no necesitaría revelar nunca la dirección IP de su servidor. Por tanto, tener un proxy inverso dificulta los ataques de tipo DDoS.
3. **Equilibrio de carga global (GSLB).**-Es una forma de equilibrio de carga, donde un mismo sitio web puede estar distribuido en varios servidores por todo el mundo y el proxy inverso enviará a los clientes a aquel servidor que esté geográficamente más cerca. De este modo, se reducen las distancias que deben recorrer las solicitudes y las respuestas, minimizando los tiempos de carga.
4.  **Almacenamiento en caché.** Se trata del [almacenamiento en caché](https://www.cloudflare.com/learning/cdn/what-is-caching/) del contenido, dando lugar a mayor rapidez de gestión.\
    \
    **Ejemplo**: Supongamos que un usuario vive aquí en Barcelona y visita un sitio web con proxy inverso con servidores web en Los Ángeles. El usuario puede conectarse a un servidor proxy inverso local aquí, que tiene que comunicarse con el servidor en Los Ángeles. En este caso el servidor proxy podrá almacenar los datos de la respuesta en caché, de manera temporal. Esto haría que otros usuarios en esta ciudad que puedan navegar por el sitio van a ver la versión almacenada en caché localmente desde el servidor proxy inverso de Barcelona, lo que resulta en un rendimiento mucho más rápido.

    Normalmente, cuando se configura un sitio web en WordPress se instala algún plugin de caché para mejorar la velocidad de respuesta a las peticiones de la web. Ver

    [https://kinsta.com/es/blog/plugins-cache-wordpress/](https://kinsta.com/es/blog/plugins-cache-wordpress/)
5. **Encriptación SSL.**- El tema de encriptar y desencriptar las comunicaciones vía SSL o TLS para cada cliente puede ser costoso para los servidores de origen. Entonces, un proxy inverso se pudiera configurar para desencriptar todas las solicitudes entrantes y encriptar todas las respuestas salientes.

### Proxy Transparente

Este tipo de proxy puede brindar a los usuarios una experiencia idéntica a la que tendrían si usaran su PC personal. De ahí la denominación de `transparente`. También se puede usar sin que lo sepan los usuarios que están conectados a la red.

Los proxies transparentes son adecuados para las empresas que desean utilizar un proxy sin que los empleados sepan que están usando uno. Tienen como ventaja la de proporcionar una experiencia de usuario perfecta. Sin embargo, este tipo de proxies son más susceptibles a ciertas amenazas de seguridad, como los ataques de denegación de servicio de SYN-flood.



### Proxy anónimo

Trata de hacer que la actividad de Internet sea imposible de rastrear. Funciona accediendo a Internet en nombre del usuario mientras oculta su identidad y la información de su equipo. Este tipo de proxy resultaría adecuado para aquellos usuarios que desean tener anonimato completo mientras acceden a Internet.

Existen otros tipos de proxy como es el caso de los proxies residenciales, de distorsión, de alto anonimato, de centro de datos, públicos, compartidos, rotativos, etc.



## LINKS

1. [https://www.fortinet.com/lat/resources/cyberglossary/proxy-server](https://www.fortinet.com/lat/resources/cyberglossary/proxy-server)
2. [https://www.mcafee.com/blogs/es-es/privacy-identity-protection/que-es-un-proxy/](https://www.mcafee.com/blogs/es-es/privacy-identity-protection/que-es-un-proxy/)
3. [https://cursosdedesarrollo.com/2022/01/nginx-proxy-manager-o-la-manera-sencilla-de-manejar-acceso-a-tus-servicios-docker/](https://cursosdedesarrollo.com/2022/01/nginx-proxy-manager-o-la-manera-sencilla-de-manejar-acceso-a-tus-servicios-docker/)
4. [https://www.ionos.com/digitalguide/server/configuration/squid-the-license-free-caching-server-for-your-web-project/](https://www.ionos.com/digitalguide/server/configuration/squid-the-license-free-caching-server-for-your-web-project/)
