---
description: Los servicios de AD DS, DNS y DHCP
---

# DNS - Windows Server 2016

A la hora de instalar el servicio de DNS en un Windows 2016 en una MV lo primero que hago es  configurar dos adaptadores. Con uno nos conectaremos a Internet, el otro lo configuraremos con una IP estática, dado que  lo más recomendable en un servidor, independientemente de los roles que instalemos, es que éste cuente con una dirección IP fija. Por tanto,

1. **Adaptador NAT**: para tener conexión a Internet. La IP que le otorga el DHCP a la MV será la 10.0.2.15 (si seleccionamos el primer adaptador). Por comodidad la llamaré WAN.
2. **Red interna**: para configurar una IP estática en el servidor. Seleccionamos un IP de red como por ejemplo: 192.168.55.5. Por comodidad la llamaré LAN.

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

Dos cuestiones adicionales:

* Antes de instalar el DNS he instalado el Active Directory (AD DS)&#x20;
* Llamaré haven.local mi dominio de pruebas para este servidor.&#x20;

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Un detalle a tener en consideración es que nombré la VM como haven y el dominio también es haven.local, esto implica que el FQDN del Windows Server sería: haven.haven.local.&#x20;

Lo correcto hubiera sido nombrarlo como NS o NS1, de nameserver o DNS principal del dominio y entonces el FQDN sería: ns1.haven.local.

Dentro del trabajo con DNS usaremos términos como:

* **Zona de búsqueda directa:** Nos brindará como resultado la dirección IP del recurso que hemos solicitado.
* **Zona de búsqueda inversa:** Este tipo de zona busca un nombre de un equipo en función de su dirección IP.
* **Reenviador DNS:** Este es un servidor que ha sido designado por otros servidores para resolver nombres de dominios externos o fuera del dominio local.

Para instalar nos dirigimos a:

1. Administrador - Agregar roles y características. Seguimos las opciones de bienvenida y seleccionamos la opción Instalación basada en características y roles.
2. Debemos seleccionar el servidor donde instalaremos el servicio DNS.
3. En la ventana de Roles de servidor debemos activar Servidor DNS
4. Clicamos en la siguiente ventana de Agregar características para que los complementos del servicio DNS sean añadidos.
5. Seguimos adelante y veremos un resumen del rol a instalar.
6. Una vez que nuestro servicio DNS ha sido instalado de manera correcta, pulsamos cerrar para salir del asistente.
7. Una vez instalado el rol DNS podemos ver en el panel del Administrador del servidor que ya podemos acceder a la configuración del DNS en Windows Server 2016.

### Configurando el servicio de DNS

Tenemos que instalar la zona directa e inversa del DNS. Para ello, vamos a Herramientas - DNS&#x20;

Cuando promovemos nuestro servidor a controlador de dominio se crean las respectivas zonas directas. Pero podemos crear una nueva zona para extender las gestiones de nuestro servidor. Para ello, hacemos clic derecho sobre el nombre de la zona y seleccionar la opción Zona nueva o puedes ir al menú  Acción - Zona nueva.

Recuerda que:

* La zona directa resuelve los nombres de dominio a direcciones IP.
* La zona inversa  a partir de las  direcciones IP encuentra los nombres de dominio.&#x20;

En definitiva, para crear nuestro archivo de zona directa, vamos a seguir las pautas del asistente, para lo cual nos pedirá definir:

* El tipo de **zona** a crear: seleccionaremos zona principal
* El modo en que se han de **replicar** los datos de la zona: seleccionamos el modo "para todos los servidores DNS que se ejecutan en controladores de dominios en este dominio: haven.local"
* Nombre de la zona: podemos proporcionar un nombre cualquiera.
* Tipo de actualizaciones: seleccionamos la opción de "permitir solo actualizaciones dinámicas seguras (recomendado para Active Directory). Opción solo disponible para zonas integradas en el Active Directory.
* Finalmente veremos un resumen de la zona creada.

Hemos creado la nueva zona del servidor DNS y podemos agregar hosts, registros, alias, MX, etc.&#x20;

De la misma forma que hemos creado el archivo de zona directa podemos establecer una nueva zona inversa para aumentar la capacidad de nuestro DNS en Windows Server 2016.

Deberíamos obtener algo así:

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

Ahora es momento de crear  un reenviador condicional para que tenga la propiedad de realizar consultas a otros servidores DNS fuera del dominio. Para ello vamos a clicar en <mark style="color:blue;">`Reenviadores condicionales`</mark> y seleccionar la opción `Nuevo reenviador condicional.` Seguimos las pautas del asistente y podemos usar el DNS público de Google,  8.8.8.8.&#x20;

En la imagen a continuación, se muestra además del servidor de Google, la IP 192.168.1.1 correspondiente al LivexPlus, o sea, el router que hace de servidor de DHCP y DNS.

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>



Una manera de asegurarnos del buen funcionamiento del servicio DNS es utilizar el comando NSLOOKUP y debería mostrarse como sigue:

<figure><img src="../../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

Para los dispositivos del dominio la respuesta que brinda el servidor DNS es autoritativa y para las consultas externas es no autoritativo.



