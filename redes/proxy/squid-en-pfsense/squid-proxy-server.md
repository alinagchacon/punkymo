---
description: pfSense
---

# Squid Proxy Server

Para comenzar la configuración de Squid nos vamos a  `Services - Squid Proxy Server - Local Cache`. Al inicio del todo nos encontramos con los ajustes generales, esto es: `Squid Cache General Settings`.

Primero vamos a configura la cache local porque nos darían errores si no tenemos definida la cache local.&#x20;

En esta sección veremos aspectos como  el tipo de política qué decide que objetos se almacenarán o no en la caché, a partir de qué punto se comienza a liberar espacio de la caché, qué IP o dominios no se almacenarán nunca en caché, etc.

<details>

<summary>Aquí te dejo una explicación de los diferentes apartados de esta sección</summary>

**Cache Replacement Policy**. Es la política donde se decide qué objetos permanecerán en la caché y cual se remplazará. En este apartado tenemos cuatro opciones a seleccionar la que más se ajuste a nuestras necesidades.

Por defecto está LFUDA que mantiene los objetos más solicitados en cache independientemente de su tamaño y buena opción para comenzar.

**Low-Water Mark in %**. Es el indice de advertencia donde empieza a liberar la cache si la swap alcanza el 90%. En este punto se aconseja ponerlo al 80% en caso de no utilizar discos SSD.

**High-Water Mark in %.** Es el indice crítico a partir del cual libera la cache. Al igual que lel caso anterior se aconseja poner la marca al 85% en caso de no utilizar discos SSD.

**Do Not Cache**. En cada linea de este espacio pondríamos direcciones IP o dominios que no deben ser cacheados.

**Enable Offline Mode**, Este modo hace que el servidor proxy nunca intente validar los objetos almacenados en caché. Este modo sin conexión brinda acceso a más información almacenada en caché de la que normalmente se permite (por ejemplo, versiones caducadas en caché donde, de lo contrario, se debería haber contactado al servidor de origen).\
\
**External Cache Managers**. En caso de utilizar administradores de cache externos.

</details>

En resumen, en esta primera sección y para probar, dejaremos las opciones por defecto:

* **Cache Replacement Policy**: por defecto Heap LFUDA
* **Low-Water Mark in %**: 80%
* **High-Water Mark in %**: 85%

### Hard Disk cache settings

Esta es la siguiente sección en la configuración de la `Local cache`. En la misma tenemos para configurar aspectos como el espacio de la cache, el sistema de almacenamiento de la caché, niveles de directorios, etc.

<details>

<summary>Aquí te dejo una explicación de los diferentes apartados de esta sección</summary>

**Hard Disk Cache Size**. En esta sección se define el espacio de disco en MB que utilizaremos para la cache, por defecto 100 MG.

**Hard Disk Cache System**. El tipo de sistema de cache.&#x20;

_Nota: En caso de duda podemos clicar el botón_ ![](<../../../.gitbook/assets/image (318).png>) _y nos brinda más información. También nos redirige a un link del Squid:_ [_http://www.squid-cache.org/Doc/config/cache\_dir/_](http://www.squid-cache.org/Doc/config/cache\_dir/) _para mayor información._

Las opciones son:

* **Ufs**. Es el antiguo y conocido formato de almacenamiento Squid que siempre ha estado ahí.
* **Aufs**. Utiliza subprocesos POSIX para evitar bloquear el proceso principal de Squid en la E/S del disco. (Anteriormente conocido como async-io).
* **Disco**. Utiliza un proceso separado para evitar bloquear el proceso principal de Squid en la E/S del disco.
* **Nulo**. No utiliza ningún almacenamiento. Ideal para empotrar.

**Clear Disk Cache NOW**. El botón que nos encontramos para limpiar la cache manualmente ![](<../../../.gitbook/assets/image (319).png>)nos permitiría limpiar la caché actual sin más.

**Level 1 Directories.** Es uno de los aspectos críticos para la velocidad de la cache. En este punto se puede especificar el número de directorios de nivel 1 para la caché del disco duro. Tener en cuenta que cada directorio de nivel 1 contiene 256 subdirectorios. Esto hace que un valor de 256 directorios de nivel 1 utilizará un total de 65536 directorios para la caché del disco duro. Esto ralentizará significativamente el proceso de inicio del servicio proxy, pero puede acelerar el almacenamiento en caché bajo ciertas condiciones.

**Hard Disk Cache Location**. Es el directorio donde se almacenará, seguido del tamaño mínimo y máximo de los objetos.

</details>

Teniendo en cuenta que estoy trabajando en una VM con un espacio limitado de disco duro de 20GB voy a dejar prácticamente todas las opciones por defecto salvo una:

* **Hard Disk Cache Size**: 100
* **Hard Disk Cache System**: aufs
* **Level 1 Directories**: 16
* **Minimum Object Size**: 0
* **Maximum Object Size**: 4

### Squid Memory Cache Settings

A continuación ltenemos la sección que corresponde con la cache en memoria que sería mucho más rápida que la de disco. No debemos asignar más de un 25% a pesar que la ayuda diga de utilizar un 50%. Imagina que  instalas más servicios de lo que pudieras y empiezas a tener errores porque te has quedado sin RAM. &#x20;

Y la última sección a día de hoy no creo que la utilice nadie.

Hemos configurado primero la cache porque al configurar el servicio os dará errores si no está definida. Con la cache preparada vamos a dirigirnos a la configuración general de Squid en la pestaña General.

<details>

<summary>Aquí te dejo explicación de los diferentes apartados de esta sección</summary>

**Maximum Object Size in RAM**. Este valor lo dejamos por defecto en 256k dado que para la mayoría de los escenarios está correcto.

**Memory Replacement Policy**. La política de reemplazo de memoria determina qué objetos se eliminan de la memoria cuando se necesita espacio. El valor predeterminado es: heap GDSF

* Heap GDSF. Optimiza la tasa de aciertos de objetos manteniendo en caché los objetos más pequeños y populares.&#x20;
* Heap LFUDA. Mantiene los objetos populares en caché independientemente de su tamaño y, por lo tanto, optimiza la tasa de aciertos de bytes a expensas de la tasa de aciertos.&#x20;
* Heap LRU. Funciona como LRU, pero en su lugar utiliza un heap.&#x20;
* LRU. Mantiene los objetos a los que se hace referencia recientemente (es decir, reemplaza el objeto al que no se ha accedido durante más tiempo).

</details>

En resumen, aquí lo dejamos todo por defecto. Guardamos y salimos.&#x20;

Con la cache lista nos dirigimos a la configuración general de Squid en la pestaña `General`. Esto es: `Package - Proxy Server: General Settings - General`.&#x20;



## Package - Proxy Server: General Settings - General

En este apartado lo dejo todo por defecto, aunque tendremos en cuenta de marcar en `Check to enable Squid Proxy` para habilitar el servicio.

<figure><img src="../../../.gitbook/assets/image (320).png" alt=""><figcaption><p>Pestaña General in Squid Proxy</p></figcaption></figure>

<details>

<summary>Aquí te dejo algunos aspectos que nos encontramos en este apartado</summary>

**Enable Squid Proxy**. Lo tenemos que activar,¡.&#x20;

**Keep Settings**. Si está activada preservará las configuraciones, logs, cache y definiciones antivirus de Squid si desinstalamos el paquete.

**Listen IP Version**. La versión de IP que escuchará. En nuestro caso seleccionamos IPV4.

**CARP Status Listen**. Si utilizamos CARP Squid solo funcionará cuando dicho firewall sea el Master y se detendrá cuando sea Slave. Aquí es muy importante sincronizar la cache local si la utilizamos.

**Proxy interface**. La interface donde activaremos el servicio. En nuestro caso tenemos una LAN que es la que seleccionamos y es muy importante seleccionar la de loopback para  poder activar Lightsquid.

**Proxy Port.** Es el puerto por el que escuchará squid y al que debemos conectarnos, por defecto en squid es el 3128.

**Allow Users on interface.** la marcamos para que los usuarios conectados a dicha interfaz puedan utilizar el proxy.

**Resolve DNS IPV4 First.** Si tienes problemas para acceder a contenido HTTPS mejor activarlo.

**Disable ICMP. Permite** desactivar el ping.

**Use Alternate DNS Servers for the Proxy Server.** Permite utilizar unos DNS diferentes a los que tenemos puestos en la configuración general del Firewall.

</details>

### Transparent Proxy Settings section

Este tipo de proxy permite que todo equipo conectado a las interfaces que seleccionemos pase por el proxy sin necesidad de configurar nada en los equipos que están conectados. En este apartado tenemos tres opciones interesantes que son:

**Bypass Proxy for Private Address Destination**. No reenviar tráfico a direcciones privadas. Las direcciones privadas pasan directamente a través del firewall, no a través del servidor proxy.

**Bypass Proxy for These Source IPs**. No reenviar el tráfico desde estas IP de origen, redes CIDR, nombres de host o alias a través del servidor proxy, sino déjelo pasar directamente a través del firewall. Se aplica sólo al modo transparente.&#x20;

**Bypass Proxy for These Destination IPs.** Omitir proxy para estas IP de destino No dirigir al proxy el tráfico que va a estas IP de destino, redes CIDR, nombres de host o alias, sino déjelo pasar directamente a través del firewall. Se aplica sólo al modo transparente.&#x20;

No obstante, no vamos a activar el proxy transparente en esta ocasión.

### SSL Man In the Middle Filtering

Este apartado es muy importante ya que a día de hoy la mayoría de tráfico es de tipo HTTPS. Aquí nos interesa tener activado:&#x20;

* HTTPS/SSL Interception&#x20;
* SSL/MITM Mode Splice All&#x20;

En cualquier caso necesitamos tener creado una CA o sea, una autoridad de certificado como mismo hicimos para configurar el servidor de OpenVPN. Debe quedar algo así como se muestra en la imagen:

<figure><img src="../../../.gitbook/assets/image (322).png" alt=""><figcaption><p>CA para el Squid</p></figcaption></figure>

En definitiva, lo que nos interesa en esta sección es:&#x20;

* habilitar el filtrado SSL,&#x20;
* con la opción Splice All&#x20;

La opción `Splice All` en el contexto de Squid, se refiere a una característica de optimización que permite al servidor proxy Squid "empalmar" (splice) las conexiones TCP entrantes y salientes.

Al habilitar "Splice all", Squid puede intentar realizar una transferencia directa de datos entre el cliente y el servidor remoto, sin necesidad de copiar los datos de ida y vuelta a través del proceso del servidor Squid. Esto permite mejorar el rendimiento y la eficiencia del servidor proxy, reduciendo la sobrecarga de procesamiento al evitar la duplicación de datos.

Sin embargo, esta opción puede tener implicaciones en la seguridad y la compatibilidad con algunas aplicaciones o protocolos, ya que puede interferir con ciertas funciones de inspección y manipulación de datos que Squid realiza normalmente.&#x20;

<figure><img src="../../../.gitbook/assets/image (321).png" alt=""><figcaption></figcaption></figure>

Lo último a considerar sería la habilitación de los logs de Squid. Como verás no es lo último de esta sección pero lo podemos dejar con todas las opciones por defecto.

Guardamos y salimos.

## Antivirus Clamav

[ClamAV](https://www.clamav.net/) es un programa de código abierto diseñado para detectar virus y malware  en sistemas Linux y FreeBSD. Es  un antivirus muy popular que permite escanear archivos en busca de virus, gusanos, troyanos, software espía entre otros. Es eficaz, ligero, fácil de usar y válido para utilizar tanto en entornos domésticos como empresariales.&#x20;

Clamav también se integra con servidores de correo electrónico para escanear y filtrar mensajes en busca de archivos adjuntos maliciosos.&#x20;

Entre las opciones que nos encontramos  con una opción donde podemos habilitar o no ClamAV,a sí como qué datos del cliente mandamos  a ClamAV, entre otras opciones.

En nuestro caso, siendo una prueba, solo lo voy a activar y dejar las opciones por defecto. Guardamos y salimos.

_Nota: Para mi es interesante el Clamav de la época en que monté un servidor de correos para el grupo de investigación donde trabajaba. No obstante, para esta prueba pudiéramos pasar sin él._

## ACL

Este apartado de `Access Control List (ACL)` hace referencia  a las reglas que determinan qué clientes tienen permiso para acceder a qué recursos a través del proxy Squid.

Dichas reglas `ACL` se  basan en características como las direcciones IP, nombres de dominio, direcciones MAC, etc. así que al configurar las ACL es posible especificar qué hosts o redes tienen permitido o denegado el acceso a través del proxy para ciertos recursos de Internet. Esto nos permite controlar y filtrar el tráfico de Internet que pasa a través de la red, lo que proporciona una capa extra de seguridad y control.

Algunas de estas opciones son:

* **Allowed subnets**.  las  subredes que podrán utilizar el proxy.&#x20;
* **Unrestricted IP.** Añadimos las redes o ips que no serán filtradas.
* **Banned Hosts Addresses.** Las subredes o ips que no tendrán permitido usar el proxy.
* **Whitelist.** La lista blanca de dominios de destino que serán siempre accesibles para los usuarios.
* **Blacklist.** La lista negra de dominios de destimo a los que no se podrá acceder nunca.



## Otros apartados

1. Tenemos el  apartado `Traffic Mgmt` que nos permite establecer el tamaño máximo de descarga y subida, el ancho de banda de las descargas cuando el uso del ancho de banda llegue a cierto valor especificado, etc.
2. La sección `Squid Transfer Extensions Settings` en caso de querer solo unas extensiones específicas.
3. Este otro apartado Squid Transfer Abort Settings&#x20;
4.  En el apartado `Authentication` es un sistema de autenticación que nos permite crear  reglas en función del tipo de usuario. Tenemos cuatro métodos:&#x20;

    1. Usuarios locales
    2. LDAP
    3. Radius&#x20;
    4. Portal Cautivo



    Tenemos otras opciones, donde pondremos lo que verán los usuarios en la ventana que se les mostraría, así como el número de procesos para autenticar, etc.
5. Depende del escenario nuestro puede ser interesante la opción donde podemos elegir el tipo, cada cuanto y la dirección de los servidores para sincronizar.

Continúa ....\


