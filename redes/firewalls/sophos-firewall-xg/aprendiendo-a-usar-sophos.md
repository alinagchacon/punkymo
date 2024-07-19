---
description: Joel López
---

# Aprendiendo a usar Sophos

## Aprender a Usar Sophos Firewall <a href="#toc159426299" id="toc159426299"></a>

Una vez tenemos el Firewall instalado debemos empezar a configurarlo para poder permitir o denegar cierto tráfico que decidiremos con las Reglas del Firewall.

### Distribución de la redes <a href="#toc159426300" id="toc159426300"></a>

En este Firewall haremos una simulación para preparar este Firewall como si fuese para usarlo en un entorno empresarial. En este Firewall tendremos 4 redes diferentes.

#### Red 1 (Port A) – Administración de los Sistemas <a href="#toc159426301" id="toc159426301"></a>

Esta red la usaremos para la administración del propio Firewall y para administrar dispositivos restringidos para los usuarios. El objetivo de esta red es que no tenga acceso a Internet para que no tenga ningún tipo de interacción con el exterior.

#### Red 2 (Port B) – Usuarios <a href="#toc159426302" id="toc159426302"></a>

Esta red la usaremos para los usuarios de la red, es decir todos los trabajadores de la empresa, cualquier usuario que esté conectado a esta red podrá acceder a Internet, pero tendrá las páginas limitadas a las de uso para la empresa, estas pueden ser: correo, intranet, OneDrive, drive, etc. Además, el tráfico desde Internet a la red estará denegado.

Como medida de seguridad más avanzada, cada trabajador de la empresa tendrá un usuario para poder acceder a más páginas de Internet. Este usuario tendrá una cuota de uso, horario y páginas web restringidas. Dependiendo del nivel del trabajador podrá tener un uso diferente. Todo el tráfico que generen estará registrado por el Firewall. Además de tener protección contra descargas maliciosas.

#### Red 3 (Port C) – DMZ <a href="#toc159426303" id="toc159426303"></a>

En esta red encontraremos los servidores web, de correo y DNS de la propia empresa. Esta red se usa para ubicar servidores que se deban acceder desde internet ya que esta se considera una zona menos segura. A estos servidores podrá acceder todos los usuarios desde cualquier red.

#### Red 4 (Port B) – WAN <a href="#toc159426304" id="toc159426304"></a>

En esta red encontraremos la configuración de la WAN, es decir la salida a internet del Firewall. Esta fue configurada durante la instalación del Firewall y fue puesta la dirección NAT de VirtualBox. En el caso de que fuese un entorno real debemos introducir la dirección IP de la puerta de enlace y una dirección IP fija para el adaptador de Red.

#### Esquema de las Redes <a href="#toc159426305" id="toc159426305"></a>

En la _imagen_ podemos ver un esquema de como estará formada la simulación de la red con el Firewall, en este esquema no está aplicada ninguna regla o política.

\


<figure><img src="../../../.gitbook/assets/image (365).png" alt=""><figcaption><p>Diagrama de la red</p></figcaption></figure>

### Configuración de la Red <a href="#toc159426306" id="toc159426306"></a>

Antes de empezar a configurar las reglas y políticas del Firewall, debemos configurar las interfaces de red. Debemos configurar desde cero las interfaces del Port B y el Port C, ya que estas no tienen configurado ningún rango de red. Además, para cada una de estas redes debemos añadir un servidor DHCP para que pueda otorgar direcciones IP a los dispositivos que se conecten. A parte de esto para tener una mayor seguridad separaremos la red de Administración (Port A) de las demás redes para poder ponerle mayores restricciones, para hacer esto cambiaremos el Port A hacia una zona diferente.

#### Interfaces de Red <a href="#toc159426307" id="toc159426307"></a>

Las Interfaces de Red que tiene el Firewall son las encargadas de conectar los distintos equipos a través de un adaptador de red. En este Firewall tenemos 4 interfaces de red, estas interfaces de red dependen de los adaptadores de red que tengamos, en VirtualBox únicamente podemos tener 4 adaptadores de red.

Para empezar a configurar las interfaces de red debemos acceder a la sección CONFIGURAR y después al apartado de RED y en la pestaña Interfaces encontraremos todas las interfaces de red que tenemos en el Firewall como se pueden ver en la _Ilustración x_.

Ahora configuraremos las interfaces de red del Port B y Port C. Las interfaces del Port A y Port D ya están configuradas ya que las hemos configurado durante la instalación del Firewall.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption><p>Interfaces de red</p></figcaption></figure>

**Configurar Red del Port B**

Para configurar la red de este puerto debemos de hacer clic sobre el nombre de Port B. Lo primero de todo es asignarle una zona, para ver las zonas que hay hacemos clic sobre el desplegable y elegimos la zona de LAN.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Seleccionando las zonas de las interfaces de red</p></figcaption></figure>

Una vez elegida la zona de LAN se habilitará la casilla de Configuración IPv4, esto hará que podamos configurar la red. En este caso la red que crearemos para esta interfaz de red será de Clase C, por lo tanto, el sufijo será /24 y la IP de la interfaz de red será la 192.168.200.10. La puerta de enlace no se puede configurar ya que está es tratada como una red Interna. La configuración nos debe quedar como en la _Ilustración_.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Configurando la zona LAN </p></figcaption></figure>

Ilustración - Configurar Rango de Red Port B

Una vez hecho hacemos clic en guardar y nos saldrá una advertencia de que es posible que después de actualizar la interfaz se produzcan interrupciones a los equipos que estén conectados por esta interfaz. La advertencia se puede ver en la _Ilustración x_, para continuar debemos hacer clic en Actualizar Interfaz.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>Actualizando las interfaces</p></figcaption></figure>



**Configurar Red del Port C**

Para configurar la red del Port C es muy parecida a la del anterior puerto. Para ello debemos acceder al Port C de la misma forma y en el desplegable de la Zona de la red debemos abrir el desplegable y seleccionar DMZ.

La dirección IP que le seleccionáramos en este caso será la 172.27.1.10, el CIDR será de Clase C, es decir /24. Una vez hecho nos debe quedar como aparece en la _Ilustración x_.

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Configurar la red del puerto C</p></figcaption></figure>

#### Zonas <a href="#toc159426310" id="toc159426310"></a>

Una vez ya hemos configurado las interfaces de red debemos de conocer que son las Zonas. Estas las hemos configurado a la hora de configurar las interfaces de red, hemos usado la Zona LAN y la Zona de DMZ.

Las zonas permiten establecer ciertas configuraciones a la propia interfaz de red, dependiendo del uso que la vayamos a dar a las interfaces de red podemos darle unos u otros. Además, a la hora de poner reglas y políticas debemos de seleccionar la regla por zonas.

Como hemos podido ver en la configuración de las interfaces, podemos tener una zona para varias interfaces de red, estas zonas también permiten crear grupos de las interfaces para así cuando asignemos reglas y políticas podamos a hacerlo a la Zona y aplicárselo a más de una interfaz a la vez.

Para configurar las zonas podemos hacerlo debemos acceder a la sección CONFIGURAR y después al apartado de RED y en la pestaña Zonas. En la _Ilustración x_ podemos ver una lista de todas las zonas que están disponibles en el Firewall.

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>Zonas del Firewall</p></figcaption></figure>

En el caso de que queramos editar alguna zona para permitir o denegar algún protocolo o configuración debemos hacer clic al icono del lápiz. En este caso únicamente veremos las opciones que están disponibles ya que en este caso las opciones predeterminadas ya son correctas.

Como se puede ver en la _Ilustración x_ hemos accedido a la configuración de la zona de LAN, dentro de la configuración podemos añadir una descripción a la Zona, también podemos ver las interfaces de red a las que pertenece la zona y justo debajo podemos ver los servicios que podemos habilitar o deshabilitar.

Aislar Red de Administrador

Una vez ya conocemos el funcionamiento de las interfaces de red y de las zonas vamos a hacer una configuración de seguridad adicional para poder administrar la interfaz de Sophos con seguridad.

Para ello vamos a aislar la red de administrador en una nueva zona y en las demás zonas (LAN y DMZ) vamos a deshabilitar los servicios de administración.

Las ventajas que ofrece esto es una mayor seguridad en el Firewall ya que únicamente se podrá acceder desde los equipos que estén en la red de Administración.

#### Crear nueva zona <a href="#toc159426312" id="toc159426312"></a>

Primero de todo para tener una mayor facilidad a la hora de aplicar reglas y configuraciones de la red de administración crearemos una nueva zona.

Para crear una zona debemos acceder al apartado de las zonas y hacer clic en Añadir, después debemos asignar el nombre a la zona y de forma opcional una descripción. También debemos asegurarnos de que el tipo de zona a crear es una LAN y no una DMZ. En este caso habilitaremos todos los servicios. Debe quedar de una forma parecida a la _Ilustración x_.

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption><p>Creación de una nueva zona</p></figcaption></figure>

Después de crear la zona debemos de asignarle a la interfaz de red de administración la nueva zona creada. Para ello nos dirigimos al apartado interfaces y accedemos a la interfaz de red que usaremos para la administración que es el Port A. Ahí debemos abrir el desplegable y seleccionar la nueva zona que hemos creado. En la _ilustración x_ podemos ver cómo debe quedar.

\
\


<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption><p>Configuración de nueva Port A</p></figcaption></figure>



Una vez guardados los cambios no notaremos ningún tipo de cambio ya que de momento no hemos aplicado ninguna regla a esa zona. Posteriormente crearemos unas reglas para denegar el tráfico hacia la WAN y de esta manera que a través de esta red no se pueda acceder a internet.



#### Desactivar servicios de administración <a href="#toc159426313" id="toc159426313"></a>

Ya aislada en otra zona la red de administración, ahora debemos de denegar el acceso al portal de inicio de sesión al panel de administración a las demás zonas. En concreto debemos de denegarle el acceso al Port B y Port C. El Port D no es necesario ya que es una zona por defecto del Firewall y se usa para tener acceso a internet.

Para ello debemos acceder a la configuración de la zona a la que pertenecen, en el caso del Port B será la zona LAN y en el Port C será la DMZ.

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Desactivación servicio administración</p></figcaption></figure>

## Configuración DHCP, DNS y certificado SSL <a href="#toc159426314" id="toc159426314"></a>

<mark style="color:red;">Falta...</mark>

## Reglas y políticas <a href="#toc159426315" id="toc159426315"></a>

<mark style="color:red;">Falta...</mark>\
\
