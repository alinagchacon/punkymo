---
description: >-
  Instalación de un servidor de correo con el objetivo de mostrar su
  funcionamiento
---

# hMailServer

Con el objetivo de mostrar el funcionamiento de un servidor de correo electrónico vamos a instalar hMailServer en una MV con una instalación previa de Windows Server 2016. &#x20;

#### ¿Qué es hMailServer?

Pues un servidor de correo electrónico en Windows que te permite configurar y gestionar buzones de correo sin necesidad de depender de un proveedor de servicios de internet o ISP.&#x20;

En un entorno real sería necesario tener registrados en el DNS los registros correspondientes de MX para anunciar los servidores de SMTP, pero esto podremos hacerlo.

### **¿Qué vamos a necesitar?**

El servidor de correos hMailServer lo configuraré dentro del mismo Windows Server 2016 donde hemos configurado previamente el servicio de DHCP y DNS. Por lo tanto, las especificaciones técnicas son las mismas. Esto es:

* Windows Server 2016
* hMailServer, que lo puedes descargar de: [https://www.hmailserver.com/](https://www.hmailserver.com/)&#x20;
* Thunderbird en un equipo cliente, que lo puedes descargar de: [https://www.thunderbird.net/es-ES/](https://www.thunderbird.net/es-ES/)&#x20;

### **La máquina virtual**

* Los servicios, previamente configurados,  de AD DS, DHCP y DNS.&#x20;
* Dos adaptadores de red:
  * **Adaptador NAT**: para tener conexión a Internet. La IP que le otorga el DHCP a la MV será la 10.0.2.15 (si seleccionamos el primer adaptador). Por comodidad la llamaré WAN.
  * **Red interna**: para configurar una IP estática en el servidor. Seleccionamos un IP de red como por ejemplo: 192.168.55.5. Por comodidad la llamaré LAN.

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

### Instalación

El detalle más importante a la hora de hacer la instalación del servidor hMailServer es solucionar el error que salta al intentar instalar .NET Framework 2.0 desde un link inexistente que provoca tener que abortar la instalación.

La imagen a continuación muestra un pantallazo de las características a seleccionar. En este caso .NET Framework  3.5  que incluye la versión 2.0 necesaria para el servidor de correo.

![](<../../.gitbook/assets/image (82).png>)

Una vez instalado el .NET Framework ya podremos instalar el hMailServer sin más contratiempos.

Siguiendo los pasos por defecto en el proceso de instalación llegaremos a una pantalla como la siguiente:

![](<../../.gitbook/assets/image (42).png>)

### **Dominios y cuentas**

Los dominios en <mark style="color:blue;">`hMailServer`</mark> deben tener conexión a un dominio de Internet. Nuestro dominio creado en el servidor de Windows, es <mark style="color:blue;">`haven.local`</mark>, por lo tanto, esto es lo que tenemos que agregar:&#x20;

<figure><img src="../../.gitbook/assets/image (217).png" alt=""><figcaption><p>Configurar el dominio</p></figcaption></figure>

Una vez establecido el dominio, puedes crear un alias del mismo, una firma y aspectos más importantes como pueden ser: el tamaño máximo del contenido, de los mensajes, el máximo de cuentas posibles, etc.

#### Cuentas

El siguiente paso es añadir cuentas en nuestro servidor. Lo habitual es que tengamos una cuenta por cada usuario,  que desea ser capaz de enviar y recibir correo electrónico de. Pero es frecuente disponer de varios buzones de correo como: _webmaster@trantor.local, info@trantor.local, hola@trantor.local_ pero un único usuario los gestiona. Para ello son los alias, que ya veremos más adelante.

Si despliegas el dominio <mark style="color:blue;">`haven.local`</mark> podremos crear las cuentas de usuarios que necesitemos. Ya tengo creadas dos cuentas para los usuarios Pepe y Xenxa, pero vamos a crear una nueva cuenta para Lola.&#x20;

Podemos establecer una contraseña que estaría cifrada. <mark style="color:red;">Detalle a tener en cuenta</mark>.&#x20;

<figure><img src="../../.gitbook/assets/image (210).png" alt=""><figcaption><p>Crear las cuentas de usuarios</p></figcaption></figure>

Otro aspecto importante en el caso de las cuentas de usuarios es la posibilidad de otorgarle permisos de usuario, de dominio o de server. En este caso, dejaremos que sea solamente como usuario:

![Los usuarios pueden tener diferentes roles](<../../.gitbook/assets/image (32) (1).png>)

### Alias

Vamos a crear un par de alias para mostrar el procedimiento. Lo interesante es probarlo más adelante. Así que clicamos en <mark style="color:blue;">`Aliases > Add`</mark> y vemos que podemos redirigir, por ejemplo, los correos de la cuenta de <mark style="color:blue;">`webmaster@haven.local`</mark> a <mark style="color:blue;">`pepe@haven.local`</mark>. Incluso, no es obligatorio que esté el correo en el mismo dominio, podemos seleccionar un correo cualquiera, por ejemplo, de <mark style="color:blue;">`gmail`</mark>. Observa que la cuenta <mark style="color:blue;">`webmaster@haven.local`</mark> no existe como tal, es un alias de <mark style="color:blue;">`pepe@haven.local`</mark>.

<figure><img src="../../.gitbook/assets/image (219).png" alt=""><figcaption><p>Alias de las cuentas de usuario: por ejemplo, mailmaster puede ser el alias de pepe</p></figcaption></figure>

### Comprobando el servicio

Antes de continuar con otros aspectos que se pueden configurar en el servidor de correos, vamos a comprobar el uso del mismo. Para ello comencemos con la instalación de un cliente de correos como  <mark style="color:blue;">`Thunderbird`</mark>. Comprobaremos el servicio dentro del propio servidor  y desde el cliente que tenemos configurado. Esto es:

1. El cliente de correos dentro del propio servidor
2. El cliente de correos en el equipo cliente

Tengamos en cuenta que los usuarios que he creado para testear el servicio son Xenxa y Pepe.

### Cliente de correo - Servidor de correo: Dentro del servidor

Instalamos el Thunderbird siguiendo el asistente por defecto. Una vez instalado, necesitamos configurar una cuenta de correos.

Vamos a Ajustes y añadimos una cuenta de correos existente. En nuestro caso, utilizaremos la cuenta de Xenxa que hemos creado en el hMailServer.

* &#x20;Xenxa Xen
* xenxa@haven.local
* Y ponemos la contraseña establecida para el usuario.

Puede ser que necesitemos ir a la configuración manual y tendríamos algo como esto, para el servidor de correo entrante que puede ser IMAP o POP3&#x20;

<figure><img src="../../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

Y para el servidor de correos saliente: SMTP

<figure><img src="../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

En el cliente de correo Thunderbird deberíamos tener configuradas las dos cuentas creadas en el hMailServer:

<figure><img src="../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

### Probar:

* Enviar un correo desde el usuario pepe@haven.local al usuario xenxa@haven.local
* Enviar un correo desde el usuario xenxa@haven.local al usuario pepe@haven.local

### Algunas consideraciones en el hMailServer:

* Habilitar las cuentas de usuarios como cuentas externas&#x20;
* Tener controladas las contraseñas habilitadas para los usuarios
* Si habilitas el Active Directory en una cuenta de usuario, el hMailServer utilizará el AD DS para validar la contraseña del usuario.

<figure><img src="../../.gitbook/assets/image (209).png" alt=""><figcaption></figcaption></figure>

###

### Cliente de correo  en Kali Linux - Servidor de correo en el servidor



A la hora de comunicar el cliente con el servidor es imprescindible habilitar los puertos de comunicación utilizados por el servicio de correo. Estos puertos son:&#x20;

* 587 (25) - SMTP
* 110 - POP3
* 143 - IMAP

Para ello nos vamos al Firewall de Windows y creamos dos reglas: una de entrada y otra de salida permitiendo la conexión a través de los puertos mencionados.

<figure><img src="../../.gitbook/assets/image (201).png" alt=""><figcaption><p>Nueva regla en el firewall</p></figcaption></figure>

Si el cliente que tenemos para comprobar los servicios de DNS, DHCP y hMailServer habilitados en el servidor es, por ejemplo, un Ubuntu o Kali podemos hacer:

sudo apt-get update

sudo apt-get install thunderbird

El procedimiento para configurar las cuentas de correo en el Thunderbird es exactamente el  mismo que en el servidor. Mejor utilizar directamente la dirección IP del servidor.

<figure><img src="../../.gitbook/assets/image (221).png" alt=""><figcaption><p>Configurando las cuentas </p></figcaption></figure>

Una vez tengamos al menos una cuenta de usuario creada en el equipo cliente, que en mi caso es una MV de Kali, ya podemos enviar un correo&#x20;

De los detalles a considerar tenemos:

* La cuenta de usuario no utiliza una conexión segura porque no tenemos habilitado un certificado de seguridad.

<figure><img src="../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

*   Para instalar el cifrado SSL:

    [https://www.ionos.es/ayuda/correo/cifrado-ssl-para-correos-electronicos/activar-el-cifrado-ssl-en-mozilla-thunderbird/](https://www.ionos.es/ayuda/correo/cifrado-ssl-para-correos-electronicos/activar-el-cifrado-ssl-en-mozilla-thunderbird/)
* Para configurar el GPG:\
  [https://support.mozilla.org/en-US/kb/openpgp-thunderbird-howto-and-faq](https://support.mozilla.org/en-US/kb/openpgp-thunderbird-howto-and-faq)
