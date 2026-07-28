# DNS - Debian 13 Trixie

**Características de la VM**:

* Sistema Operativo: Debian GNU/Linux 13 (Trixie)
* Red: Red NAT
* HDD: 25GB
* RAM: 2GB

**IP estática en el servidor**

Configuramos la IP estática en el archivo /etc/network/interfaces

<figure><img src="../../../.gitbook/assets/image (19).png" alt="" width="563"><figcaption></figcaption></figure>

**Instalación del Bind9**

Instalamos el DNS con Bind9, que es el programa de software de servidor de nombres de dominio (DNS) más utilizado en Internet, específicamente en Unix y Linux.

Según palabras de [sysadminok](https://www.sysadminok.es/blog/hosting/servidor-dns-con-bind9-configuracion-y-seguridad/), BIND - Berkeley Internet Name Domain - **es el software de servidor DNS más utilizado en la historia de Internet.** Su origen está en los años 80, en la Universidad de California, Berkeley. Lleva más de 40 años siendo el caballo de batalla que ha mantenido gran parte de la resolución de nombres en la red.

```
sudo apt bind9 bind9-utils 
```

**Configuración del DNS**

En primer logar tenemos que editar los siguientes archivos:

* named.conf.local
* named.conf.options

**Archivo named.conf.local**

Nos vamos al directorio /etc/bind/ y editamos el archivo: <mark style="color:purple;">named.conf.local</mark> con los archivos de zona directa e inversa. Si os fijáis ambos archivos están en un directorio que debemos crear: **zones**. No es obligatorio crearlo.

<figure><img src="../../../.gitbook/assets/image (20).png" alt="" width="464"><figcaption></figcaption></figure>

Con el comando siguiente podemos verificar la sintaxis de este archivo:

```
named-checkconf
```

**Ficheros de zona**

Ahora vamos a crear los ficheros de zona que hemos indicado en la configuración. A diferencia de Ubuntu, no tenemos ningún archivo que nos sirva de plantilla, pero la estructura es la misma.

Para la **zona directa** creamos el fichero:

```
/etc/bind/zones/db.haven.local
```

El siguiente pantallazo muestra la configuración.

<figure><img src="../../../.gitbook/assets/image (21).png" alt="" width="563"><figcaption></figcaption></figure>

Donde:

* TTL es el "time to live" por defecto para los registros de la zona, cuyo valor por defecto es en segundos 86400 que equivalen a 24 horas e indica el tiempo que los resolvers pueden **mantener en caché** la información.
* **@** hace referencia al dominio principal: haven.local.
* **ns1.haven.local.** es el servidor DNS primario.
* **hostmaster.haven.local.** es el correo del administrador cambiando el “@” por un "."
* **Serial** es el número de versión de la zona. Valor que se debe incrementar cada vez que se modifica la zona.
* **Refresh** indica cada cuánto un esclavo consulta al maestro para ver si hay cambios (segundos).
* **Retry** en caso que falle la conexión al maestro, cada cuánto reintenta.
* **Expire** es el tiempo máximo que un esclavo mantiene la zona si no logra contactar con el maestro.
* **Minimum TTL** tiempo que los resolvers cachean las respuestas negativas (NXDOMAIN).

Para la **zona inversa** creamos el fichero correspondiente:

```
nano /etc/bind/zones/db.6.168.192
```

Y la sintaxis de la configuración es similar al del archivo de zona directa. Esto es:

<figure><img src="../../../.gitbook/assets/image (22).png" alt="" width="563"><figcaption></figcaption></figure>

Donde:

* **in-addr.arpa** es el dominio especial para resoluciones inversas IPv4.
* **IN NS ns.** hace referencia al **servidor DNS autoritativo** de la zona inversa.
* **PTR** asocia una dirección IP con un nombre de dominio.

Con el comando siguiente podemos verificar la sintaxis del archivo de zona directa:

```
sudo named-checkzone haven.local /etc/bind/zones/db.haven.local
```

y del archivo de zona inversa:

```
sudo named-checkzone 6.168.192.in-addr-arpa /etc/bind/zones/db.6.168.192
```

**Configurando el archivo named.con.options**

Este es un archivo importante, dado que sirve para definir **opciones globales** que afectan al comportamiento del servicio, tanto si es un DNS **recursivo** como si es **autoritativo**.

<figure><img src="../../../.gitbook/assets/image (23).png" alt="" width="563"><figcaption></figcaption></figure>

Donde:

* **/var/cache/bind** es el directorio por defecto de trabajo de BIND, donde guarda archivos de caché o secundarios.
* **listen-on** define en qué interfaces/IP escuchará el servidor.
* **recursion** indica si el servidor hace **resolución recursiva,** o sea si buscará respuestas en otros DNS.
  * **yes** indica el modo caché/recursivo, útil en redes locales. Se deshabilita (no) en servidores DNS público.
* **allow-recursion** lista las IP o redes que tienen permiso para usar la recursión. En nuestro caso, la red local.
* **forwarders** lista los servidores DNS a los que se reenviarán aquellas consultas que el servidor no pueda resolver.

Ya casi finalizamos, pero antes de poner en marcha el servicio modifiquemos el fichero <mark style="color:blue;">`/etc/default/named`</mark> donde especificaremos la opción<mark style="color:blue;">`-4`</mark> como argumento para el usuario **bind,** que se crea automáticamente durante la instalación del servicio bind9.

La opción <mark style="color:blue;">`-4`</mark> nos sirve para forzar el uso de IPv4 siempre y evitar mensajes de error de red inalcanzable por direccionamiento IPv6.

<figure><img src="../../../.gitbook/assets/image (24).png" alt="" width="458"><figcaption></figcaption></figure>

Ya estamos listos para reiniciar el servicio:

```
sudo systemctl restart bind9
sudo systemctl status bind9
```

Y probamos que el servicio funcione correctamente con el comando nslookup. Podemos verificar que el archivo /etc/resolve.conf apunte al servidor DNS recién creado.

<figure><img src="../../../.gitbook/assets/image (25).png" alt="" width="317"><figcaption></figcaption></figure>

Así tendríamos configurado nuestro servidor DNS primario.
