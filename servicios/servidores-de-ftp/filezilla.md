---
description: En Windows Server
---

# FileZilla

{% embed url="https://wiki.filezilla-project.org/Network_Configuration" %}

{% embed url="https://wiki.filezilla-project.org/FTPS_using_Explicit_TLS_howto_(Server)" %}

Voy a utilizar el servidor de Windows Server 2016 que tengo instalado y que brinda servicio de DNS y DHCP.&#x20;

Características:

* Windows Server 2016
* FileZilla server, versión1.5.1&#x20;

| Servidor      | IP                          | OBS.                                          |
| ------------- | --------------------------- | --------------------------------------------- |
| trantor.local |                             | dominio del servidor                          |
| IP LAN        | 192.168.5.100               | La IP estática del servidor                   |
| IP WAN        | 192.168.X.Y                 | La IP otorgada por el DHCP del router de casa |
| DNS           |                             |                                               |
| DHCP          | 192.168.5.20 - 192.168.5.40 | rango de IP                                   |

### Configuración del FileZilla



![](<../../.gitbook/assets/image (23) (1).png>)



Los grupos a crear son:

![](<../../.gitbook/assets/image (94).png>)

Crear dos usuarios para los dos grupos previamente creados y asignar un usuario a cada uno. FileZilla Server utiliza un sistema de archivos virtual bajo el cual los archivos y directorios estarán disponibles para aquellos usuarios conectados al servidor FTP.&#x20;

La llamada <mark style="color:blue;">`ruta virtual`</mark> es la ubicación en el sistema de archivos virtual al que se asignará el <mark style="color:blue;">`directorio nativo`</mark> y su contenido. De este modo, lo que se visualiza a través de FTP es independiente de la estructura del sistema de archivos nativo subyacente.

![](<../../.gitbook/assets/image (166).png>)

![](<../../.gitbook/assets/image (126).png>)

Importante tener control del log file. En caso de errores, ahí es donde tenemos que entrar a analizar qué es lo que puede estar fallando. Esto es:

<mark style="color:blue;">`C:\Program Files\FileZilla Server\Logs\filezilla-server.log`</mark>



### Network Configuration Wizard

El servicio de FTP soporta dos modos de establecimiento de la conexión: modo activo y modo pasivo.

El modo pasivo es el modo recomendado para la mayoría de los clientes. En este modo, el equipo cliente pregunta al servidor de FileZilla a qué puerto del servidor debe conectarse. Por el contrario, el modo activo no requiere de configuración en el lado del servidor.

En el modo pasivo es necesario establecer un rango de puertos a utilizar por el servidor para las conexiones de datos. Me voy a quedar con el rango de puertos por defecto de <mark style="color:blue;">`49512`</mark> al <mark style="color:blue;">`65534`</mark>.&#x20;

Las recomendaciones del asistente nos dice que en caso de tener el servidor de FileZilla conectado a la red externa a través de un router <mark style="color:blue;">`NAT`</mark>, entonces debemos especificar  la IP externa o el nombre de host desde el que se puede acceder al servidor de FileZilla. En este punto podemos ingresar la IP pública, el nombre de host o dejarlo vacío en cuyo caso FileZilla utilizará la IP local.

Utilizaremos la IP estática del servidor: <mark style="color:blue;">`192.168.5.100`</mark> y clicamos en Usar la IP local para la conexiones locales, que es lo recomendado y justo lo que probaremos.

Lo siguiente que debemos hacer será abrir los puertos necesarios en el firewall.&#x20;

### Firewall de Windows

Vamos a crear una nueva regla de entrada y salida que permita la conexión del protocolo <mark style="color:blue;">`TCP`</mark> a los puertos del rango previamente establecido, esto es: del <mark style="color:blue;">`49512`</mark> al <mark style="color:blue;">`65534`</mark>.&#x20;

Una vez hecho esto ya estamos en condiciones de testear la conexión con el servidor.&#x20;

#### Prueba de conectividad desde el propio servidor de Windows Server 2016

Aunque no tiene mucho sentido realizar la prueba de conectividad desde el propio servidor, al menos no sirve para testear la configuración básica del servicio de FTP, sin certificado de seguridad:

Para ello, desde el propio CMD podemos hacer:

<mark style="color:blue;">`ftp 192.168.5.100`</mark>

Nos pedirá usuario y contraseña y como vemos nos ha dado un mensaje de bienvenida que ya había establecido anteriormente.

![](<../../.gitbook/assets/image (181).png>)

#### Prueba de conectividad desde el cliente Windows 10 conectado al servidor

Desde el cliente podemos testear la conexión tanto desde el cliente de FileZilla como desde la línea de comandos, haciendo exactamente lo mismo que en el caso anterior.&#x20;

A modo de mostrar la conexión establecida os muestro un pantallazo de Wireshark, instalado en el cliente de Windows 10, que muestra:

* IP del equipo cliente: 192.168.5.20
* IP del servidor de FTP: 192.168.5.100
* Usuario: pepa

![](<../../.gitbook/assets/image (54).png>)

Esta configuración tiene un grave inconveniente como podéis observar y es que no tiene seguridad y los datos se transmiten en texto plano. Esto sería inadmisible en un entorno real, con lo cual tenemos que configurar el servicio.

<mark style="color:red;">¿Pudiéramos conectarnos desde una app en nuestro móvil?</mark>

## Certificado TLS / SSL

1. [https://docs.microsoft.com/es-es/azure/vpn-gateway/vpn-gateway-certificates-point-to-site](https://docs.microsoft.com/es-es/azure/vpn-gateway/vpn-gateway-certificates-point-to-site)
2. [https://www.goanywhere.com/es/blog/sftp-vs-ftps-cuales-son-las-principales-diferencias](https://www.goanywhere.com/es/blog/sftp-vs-ftps-cuales-son-las-principales-diferencias)



### Creación de certificado raíz auto firmado y generación de certificado cliente con PowerShell en Windows 10  o Windows Server 2016



En el PowerShell, vamos a crear un certificado raíz firmado de modo automático con el nombre "<mark style="color:blue;">`FTPCert`</mark>" y que  se instalará automáticamente en <mark style="color:blue;">`Certificates-Current User\Personal\Certificates.`</mark> Para ver el certificado escribe _<mark style="color:blue;">`certmgr.msc`</mark>_, o bien busca _<mark style="color:blue;">`Administrar certificados de usuario`</mark>_.

<mark style="color:blue;">``$cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature -Subject "CN=FTPCert" -KeyExportPolicy Exportable -HashAlgorithm sha256 -KeyLength 2048 ` -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign``</mark>

#### Generación de un certificado de cliente <a href="#clientcert" id="clientcert"></a>

Cada equipo cliente que se conecta debe tener instalado un certificado (de cliente). Es posible generarlo desde un certificado raíz auto firmado y, más tarde, exportar e instalarlo, puesto que de no encontrarlo se producirá un error de autenticación.

#### Generación de certificado de cliente a partir de un certificado auto firmado&#x20;

Desde el mismo certificado raíz se pueden generar varios certificados de cliente. En este caso, el certificado de cliente se instala automáticamente en el equipo que se usó para generar el certificado. Para instalar un certificado de cliente en otro equipo, es posible exportar el certificado.

El ejemplo siguiente genera un certificado de cliente con el nombre "FTPChildCert". Puedes modificar el nombre del certificado secundario, modificando el valor CN.&#x20;

El certificado de cliente que se genera se instala en <mark style="color:blue;">`Certificates - Current User\Personal\Certificates`</mark>.

<mark style="color:blue;">`New-SelfSignedCertificate -Type Custom -DnsName FTPChildCert -KeySpec Signature -Subject "CN=FTPChildCert" -KeyExportPolicy Exportable -HashAlgorithm sha256 -KeyLength 2048 -CertStoreLocation "Cert:\CurrentUser\My" -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")`</mark>



#### Si cerraste la consola o vas a crear más certificados&#x20;

Si vas a crear otros certificados de clientes, o en caso de no usar la misma sesión de PowerShell que has utilizado previamente para crear el certificado raíz auto firmado, entonces:

<mark style="color:purple;">Identifica el certificado raíz auto firmado que se instaló en el equipo</mark>. Para ello haz lo siguiente y verás una lista de certificados instalados en el equipo.

<mark style="color:blue;">`Get-ChildItem -Path "Cert:\CurrentUser\My"`</mark>



<mark style="color:purple;">Busca el nombre del firmante de la lista devuelta.</mark> Después, busca la huella digital que se encuentra en un archivo de texto. El nombre CN es el nombre del certificado raíz auto firmado a partir del que va a generar un certificado secundario. En nuestro caso, "<mark style="color:blue;">`FTPCert`</mark>".

<mark style="color:purple;">Declara una variable para el certificado raíz con la huella digital del paso anterior</mark>. Reemplaza la huella digital con la del certificado raíz a partir del que va a generar un certificado secundario.

<mark style="color:blue;">`$cert = Get-ChildItem -Path "Cert:\CurrentUser\My\AED812AD883826FF76B4D1D5A77B3C08EFA79F3F"`</mark>

<mark style="color:purple;">Modifica y ejecuta</mark> el comando siguiente para generar un certificado de cliente. El resultado es un certificado de cliente con el nombre "<mark style="color:blue;">`FTPCert`</mark>" y se instala automáticamente en la ruta del equipo <mark style="color:blue;">`Certificates - Current User\Personal\Certificates`</mark>.

<mark style="color:blue;">`New-SelfSignedCertificate -Type Custom -DnsName FTPCert -KeySpec Signature -Subject "CN=FTPChildCert" -KeyExportPolicy Exportable -HashAlgorithm sha256 -KeyLength 2048 -CertStoreLocation "Cert:\CurrentUser\My" -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")`</mark>

### Exportar la clave pública del certificado raíz (.cer) <a href="#cer" id="cer"></a>

Una vez creado el certificado raíz, se puede exportar el archivo .cer de clave pública. Este archivo se cargará más adelante en Azure. Esto es:

1. Abre el <mark style="color:blue;">`Administrar certificados de usuario`</mark>.&#x20;
2. Busca el certificado raíz auto firmado en <mark style="color:blue;">`Certificados - Usuario actual\Personal\Certificados`</mark>&#x20;
3. Haz clic en <mark style="color:blue;">`Todas las tareas`</mark> y, luego, en <mark style="color:blue;">`Exportar`</mark>.&#x20;
4. Abre el <mark style="color:blue;">`Asistente para exportar certificados`</mark>.&#x20;
5. Sigue las indicaciones del Asistente y selecciona <mark style="color:blue;">`No exportar la clave privada`</mark>
6. En <mark style="color:blue;">`Formato de archivo de exportación`</mark>, selecciona <mark style="color:blue;">`X.509 codificado base 64 (.CER)`</mark>
7. Haz clic en clic en <mark style="color:blue;">`Examinar`</mark> para buscar una ruta adecuada.



#### Exportar el certificado raíz auto firmado y la clave privada  <a href="#export-the-self-signed-root-certificate-and-private-key-to-store-it-optional" id="export-the-self-signed-root-certificate-and-private-key-to-store-it-optional"></a>

En caso de querer exportar el certificado auto firmado para tener una copia de seguridad necesitas   seguir los pasos anteriores pero exportando el archivo <mark style="color:blue;">`.pfx`</mark>.



#### Exportación del certificado de cliente <a href="#clientexport" id="clientexport"></a>

Al generar un certificado cliente, éste se instala automáticamente en el equipo utilizado para generarlo. Pero en caso de querer instalarlo en otro equipo cliente, éste se debe exportar.

1.  <mark style="color:blue;">`Exportar`</mark> certificado cliente:

    1. Abre el **Administrar certificados de usuario**.&#x20;
    2. Haz clic en **Todas las tareas** y en **Exportar** para abrir el **Asistente para exportar certificados**.
    3. Selecciona **Exportar la clave privada.**
    4. Deja seleccionados los valores predeterminados
    5. &#x20;En **Seguridad** , debes usar una contraseña para proteger la clave privada.&#x20;
    6. Establece la ubicación



### Instalar certificado de cliente

Una vez exportado el certificado:

1. Busca y copia el archivo _.pfx_ en el equipo cliente.
2. Haz doble clic en el archivo _.pfx_ para instalarlo. Deja la **Ubicación del almacén** como **Usuario actual** y selecciona **Siguiente**.
3. En  **File to import**  no hagas cambios.&#x20;
4. En **Protección de clave privada**, escribe la contraseña del certificado
5. En el **Almacén de certificados**, deja la ubicación predeterminada
6. **Finalizar**

