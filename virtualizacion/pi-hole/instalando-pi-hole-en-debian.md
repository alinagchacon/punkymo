---
description: Pi-hole
---

# Instalando Pi-hole en Debian

En esta ocasión vamos a utilizar una VM que tiene instalada:&#x20;

* la distribución de Debian 11 Bullseye&#x20;
* conectada a una red NAT

Como estoy utilizando una VM que ya tenía instalada debemos verificar que esté el sistema actualizado y que la IP que utilice sea estática dado que vamos a configurar el servicio DNS y de DHCP. Para ello ejecutamos los comandos:

```
$ sudo apt update
$ sudo apt upgrade
```

Ahora verificamos la instalación de red. Para ello debemos configurar la IP estática. Nos debe quedar algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (244).png" alt=""><figcaption><p>Configuración del archivo /etc/network/interfaces</p></figcaption></figure>

Como puedes ver, he asignado la IP 192.168.2.50/24 al servidor de Debian donde vamos a instalar el Pi-hole. Esta dirección IP se corresponde con la red NAT "NatNet".

<figure><img src="../../.gitbook/assets/image (245).png" alt=""><figcaption><p>Configuración de la red Nat "NatNet"</p></figcaption></figure>

Recuerda que debemos actualizar la configuración:

```
$ systemctl restart networking.service
```

También vamos a editar el fichero `/etc/resolv.conf` y agregamos un par de servidores externos:

<figure><img src="../../.gitbook/assets/image (246).png" alt=""><figcaption><p>Fichero /etc/resolv.conf</p></figcaption></figure>

Recuerda que cualquier modificación que hagas necesitas la actualización del servicio. Como en mi caso lo tengo todo debidamente actualizado me dispongo a verificar que tenga instalado la herramienta `curl`.

`curl` es una herramienta de línea de comandos que sirve para realizar transferencias de datos a partir de una URL. Su nombre viene de `Client for URLs` . Con este comando se puede enviar o recibir datos a través de protocolos, como HTTP, HTTPS, FTP, FTPS, SCP, SFTP, LDAP, entre otros.

Para verificar si tenemos o no instalado curl podemos hacer:

```
$ which curl
$ whereis curl
```

En caso de no tenerlo instalado, pues toca instalarlo, así que vamos allá:

```
$ apt install curl
```

## Instalando Pi-hole

Una vez hecho esto, vamos a instalar pi-hole:

```
$ curl -sSL https://install.pi-hole.net | bash
```

Con este comando se conectará a Internet y comenzará la descarga del paquete `pi-hole,` iniciando el asistente de instalación, que es muy simple la verdad. \


Lo primero que nos dice es que el servidor será configurado como un `bloqueador de anuncios`. Una vez seguimos adelante, nos advierte que el servidor tiene que tener IP estática con lo cual, estaríamos a tiempo de rectificar nuestra instalación si no lo hemos hecho antes.

El siguiente paso sería seleccionar el servidor DNS. Si en este punto seleccionamos Google por defecto, después podremos modificarlo y establecer nuestro propio servidor.

<figure><img src="../../.gitbook/assets/image (247).png" alt=""><figcaption><p>Establecer el servidor DNS</p></figcaption></figure>

A continuación tenemos que seguir con la selección del asistente. Por ejemplo, si queremos usar una lista de bloqueo de anuncios, podemos hacerlo teniendo en cuenta que Pi-hole nos brinda por defecto `StevenBlack.`

También tenemos que instalar la consola de administración y gestión web, para lo cual instala el servidor _`lighttpd`_ y los módulos `PHP` correspondientes.&#x20;

Por si no lo sabías `lighttpd` es un servidor web de código abierto, ligero que ha sido diseñado para ser rápido y eficiente. Su nombre proviene de la combinación de "lighty" y  "HTTP daemon". No es tan utilizado como Apache o Nginx, pero es conocido por su rendimiento y que utiliza pocos  recursos y tiene una configuración flexible.

Una vez terminada la instalación nos muestra una ventana como la siguiente donde nos facilita una contraseña de acceso.

<figure><img src="../../.gitbook/assets/image (248).png" alt=""><figcaption><p>Fin de la instalación de Pihole</p></figcaption></figure>

Para acceder a la web de configuración de Pi-hole tenemos que llamar a la IP del servidor y en algún caso, toca poner al final /admin. Esto es:

```
http://192.168.2.50/admin
```

<figure><img src="../../.gitbook/assets/image (249).png" alt=""><figcaption><p>Dashboard de administración de Pi-hole</p></figcaption></figure>

## Configurando Pi-hole

Ahora es el momento de la configuración de los servicios de DNS y DHCP, así como testear el bloqueo de anuncios.

Podemos establecer el dominio para nuestro servidor y el servidor principal `haven.local.`

Podemos configurar el servicio de DHCP:

<figure><img src="../../.gitbook/assets/image (250).png" alt=""><figcaption><p>Configuración del servicio de DHCP en Pi-hole</p></figcaption></figure>

Puedes verificar y/o adicionar Adlists:

<figure><img src="../../.gitbook/assets/image (251).png" alt=""><figcaption><p>Añadir una AdList</p></figcaption></figure>

Una vez tengamos nuestros servicios configurados debemos testear que todo funcione correctamente. Para ello, debemos disponer de una VM que sirva de cliente y que, lógicamente, esté conectada a la misma red NAT: NatNet donde tenemos el servidor. En mi caso también es una VM con una distribución de Debian 11.

Recuerda que el equipo cliente tiene que estar configurado para tomar los parámetros de red por DHCP.&#x20;

Podemos verificar que el cliente está conectado a nuestro servidor:

<figure><img src="../../.gitbook/assets/image (252).png" alt=""><figcaption><p>Currently active DHCP leases en el servidor de Pi-hole</p></figcaption></figure>

Nota: Este tutorial lo tengo que completar. Aquí he mostrado los puntos más básicos.

<mark style="color:red;">To be continued ...</mark>
