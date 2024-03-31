---
description: pfSense
---

# SquidGuard Proxy

Para comenzar con la configuración de esta parte del squid nos dirigimos a `Services - SquidGuard Proxy Filter` donde nos encontramos varias secciones.

## General Settings

Una vez que clicamos en SquidGuard Proxy Filter en Services caemos en esta pestaña donde nos muestra un botón que tendríamos que clicar cada vez que hagamos un cambio en la configuración del servicio:

<figure><img src="../../../.gitbook/assets/image (325).png" alt="" width="352"><figcaption><p>General Settings</p></figcaption></figure>

En esta sección tenemos la opción de utilizar `LDAP`, de configurar opciones del servicio en dependencia  de nuestro hardware y la cantidad de usuarios. También podemos configurar el número de procesos hijos que estarán disponibles en el arranque, los logs del sistema, de modo que no nos quedemos sin espacio en disco, y la sección `Blacklist` options que no puede faltar.

### Target categories

En este apartado vamos a crear dos nuevos items: Permitidos y Bloqueados

<figure><img src="../../../.gitbook/assets/image (326).png" alt=""><figcaption><p>Permitidos y Bloqueados</p></figcaption></figure>

En Permitidas he puesto los dominios que voy a permitir acceder. La red 192.168.56.0 /124  de la LAN y la 172.27.1.0/24 de la DMZ.

<figure><img src="../../../.gitbook/assets/image (327).png" alt="" width="375"><figcaption><p>Permitidas</p></figcaption></figure>



En `Domain List` podemos escribir aquellas IP o dominios a permitir o bloquear según sea la lista. Tienen que estar separados por un espacio.&#x20;

En el ejemplo he utilizado las IP de los nodos de Tor que han sido obtenidas de la URL:&#x20;

<pre><code><strong>https://www.dan.me.uk/torlist/ 
</strong></code></pre>

El problema es que esta lista viene una por línea y necesitamos que estén separadas por un espacio. Para solucionar esto vamos a utilizar el comando `sed.` Copiamos y pegamos las IP en un documento por ejemplo lista.txt y ejecutamos:

```
sed ':a;N;$!ba;s/\n/ /g' lista.txt > lista_espacios.txt
```

Ahora ya podemos abrir el fichero `lista_espacios.txt`, copiar y pegar ya que lo tendremos sin salto de lineas y con un espacio entra las IP.

Entonces, en el apartado de URL list podemos escribir las URL a bloquear.

<figure><img src="../../../.gitbook/assets/image (328).png" alt="" width="375"><figcaption><p>Bloquedas</p></figcaption></figure>



### Blacklist

Podemos descargar el archivo de blacklists de la Universidad de Francia:&#x20;

```
http://dsi.ut-capitole.fr/blacklists/download/blacklists_for_pfsense.tar.gz
```

Así que, nos quedaría de la siguiente manera:

<figure><img src="../../../.gitbook/assets/image (329).png" alt=""><figcaption></figcaption></figure>

Podemos configurar franjas horarias para las reglas para flexibilizar el acceso a Internet.&#x20;

Podemos configurar las `Common ACL` y las `Groups ACL`. Veremos las `Target Rule List`  esto es:

* las Target categories&#x20;
*   las 74 categorías descargadas. Debemos elegir entre:&#x20;

    * whitelist (permitida aunque la encuentre en otra categoría bloqueada),&#x20;
    * allow (permitida siempre y cuando no aparezca en otra categoría) &#x20;
    * deny (no permitida)

    \


    <figure><img src="../../../.gitbook/assets/image (330).png" alt=""><figcaption><p>Common ACLs</p></figcaption></figure>



### Groups ACL&#x20;

Groups ACL y las Common ACL son casi lo mismo aunque tenemos que tener en cuenta que las Common ACL tienen prioridad sobre las de Grupo, esto es: si en las common ACL  tenemos una categoría determinada bloqueada aunque en Groups ACL la permitamos seguirá estando bloqueada.&#x20;

<figure><img src="../../../.gitbook/assets/image (331).png" alt=""><figcaption><p>Clientes a quienes les puede afectar las reglas ACL</p></figcaption></figure>

Podemos configurar a qué rango de IP (clientes) les afectan las reglas. Yo he optado por poner todo el rango de direcciones válido de la red LAN (no debería ser).

Las `Target rules List` es donde podemos seleccionar el comportamiento de si es whitelist, allow o deny en función de nuestras necesidades.

Hay una opción curiosa y es la de no permitir escribir la IP en lugar del nombre de dominio para no saltarse las reglas ACL.

<figure><img src="../../../.gitbook/assets/image (332).png" alt=""><figcaption><p>No permitir IP</p></figcaption></figure>

Del resto de opciones, lo más importante es activar el servicio de logs para encontrar los fallos. Guardamos y salimos:

<figure><img src="../../../.gitbook/assets/image (333).png" alt=""><figcaption></figcaption></figure>

Ya tenemos SquidGuard funcionando y recuerda que a cada cambio que hagamos en la configuración  tenemos que hacer  `Apply` en la pestaña `General` y `Reiniciar`  el servicio en la pestaña L`ogs` en la flecha circular.

### Squid Proxy Reports

Nos vamos a Status - Squid Proxy Reports y prácticamente sin modificar nada podremos acceder a los reports del squid como se muestra a continuación.

<figure><img src="../../../.gitbook/assets/image (336).png" alt=""><figcaption><p>LightSquid report</p></figcaption></figure>

No es simple de configurar. Hay montones de detalles a considerar. Tampoco estoy utilizando una VM con suficiente RAM y espacio en disco con lo que he ido recibiendo mensajes de error de la swap de disco.&#x20;

<figure><img src="../../../.gitbook/assets/image (338).png" alt=""><figcaption><p>Mensaje de error</p></figcaption></figure>

A partir de este punto sería interesante poder optimizar la configuración y testear el servicio, sobre todo el tema de las whitelist, blacklist, etc.\


## Links

* [https://docs.netgate.com/pfsense/en/latest/packages/cache-proxy/squidguard.html](https://docs.netgate.com/pfsense/en/latest/packages/cache-proxy/squidguard.html)&#x20;
* [http://www.squid-cache.org](http://www.squid-cache.org)
