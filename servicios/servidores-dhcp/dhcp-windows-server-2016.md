---
description: >-
  Algunos aspectos a tener en cuenta a la hora de la instalación del DHCP en
  Windows Server.
---

# DHCP-Windows Server 2016

### Requisitos previos

* Máquina virtual con Windows Server 2016
*   Dos adaptadores de red:

    * **Adaptador NAT**: para tener conexión a Internet. La IP que le otorga el DHCP a la MV será la 10.0.2.15 (si seleccionamos el primer adaptador). Por comodidad la llamaré WAN.
    * **Red interna**: para configurar una IP estática en el servidor. Seleccionamos un IP de red como por ejemplo: 192.168.55.5. Por comodidad la llamaré LAN.



Un detalle a tener en consideración es que nombré la VM como haven y el dominio también es haven.local, esto implica que el FQDN del Windows Server sería: haven.haven.local.&#x20;

Lo correcto hubiera sido nombrarlo como NS o NS1, de nameserver o DNS principal del dominio y entonces el FQDN sería: ns1.haven.local.

### Instalación

* Como todos los roles que se instalan en Windows Server, el DHCP se instala desde la consola **Administrador del servidor**.
* Abre la consola **Administrador del servidor** y haz clic en **Agregar roles y características**.
* En la ventana **Seleccionar tipo de instalación**, utiliza la opción por defecto.
* Selecciona el rol **Servidor DHCP**. Se tienen que instalar las características, con lo cual haz clic en **Agregar características** en la ventana.
* Sigue los pasos por defecto. En la ventana de confirmación, haga clic en **Instalar**.
* En el **Administrador del servidor**, haz clic en **Notificationes**  y, a continuación, en **Completar configuración de DHCP**.
* Se abre un asistente; haz clic en **Siguiente y sigue las opciones por defecto.**
* **Se instala pero todavía no estará configurado.**

<figure><img src="../../.gitbook/assets/image (35) (1).png" alt=""><figcaption></figcaption></figure>

### Configuración

#### Agregar un nuevo ámbito

Un **ámbito DHCP** está formado por un pool de direcciones IP como puede ser, por  ejemplo, de la 192.168.55.50 a 192.168.55.65.&#x20;

Cuando un cliente realiza una solicitud, el servidor DHCP asignará una de las direcciones libres del pool.&#x20;

* Para agregar un nuevo ámbito de DHCP debemos clicar en Herramientas - DHCP - haven.haven.local - IPv4.&#x20;
* Siguiendo los pasos del asistente, te pedirá **nombrar el ámbito** y establecer la **IP inicial y final** del mismo. En mi caso sería de la IP: 192.168.55.50 a la 192.168.55.65

<figure><img src="../../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

* Si deseas o necesitas **excluir** ciertas direcciones IP del intervalo puedes especificarlo en el asistente. Normalmente, los controladores de dominio, los servidores web, los servidores DHCP, los servidores del sistema de nombres de dominio (DNS) y otros servidores tienen direcciones IP asignadas estáticamente.&#x20;
* Puedes determinar  el **número de días**, horas y minutos para que expire una concesión de dirección IP de este ámbito. En este punto es que puedes determinar cuánto tiempo un cliente puede contener una dirección concedida sin renovarla. El valor por defecto es de 8 días.
* Escriba la dirección IP de la puerta de enlace predeterminada que deben usar los clientes que obtengan una dirección IP de este ámbito.&#x20;
* Si usas servidores DNS en la red, debes escribir el nombre de dominio en el apartado **Dominio primario**. Escribe el nombre del servidor DNS (que en mi caso coincide con el servidor DHCP). Aunque estén en el mismo servidor ambos servicios, haz clic en Resolver para asegurar que el servidor DHCP se puede comunicar con el DNS y determinar su dirección.&#x20;
* Si utilizas un servidor del servicio de nomenclatura de Internet de Windows (WINS), agrega el nombre y la IP.
* Ahora solo queda responder que **Sí, quiero activar este ámbito ahora** para activar el ámbito y permitir que los clientes obtengan concesiones de él y finalizamos el proceso.

Un punto importante; En el árbol de consola, haz clic en el nombre del servidor y, a continuación, haz clic en **Acción** - **Autorizar**.

### Algunos problemas de la instalación:

* Si no has configurado correctamente las direcciones IP del servidor, puede que no esté concediendo correctamente la IP al cliente.&#x20;
* Si un cliente DHCP no tiene una dirección IP configurada, normalmente indica que el cliente no pudo ponerse en contacto con un servidor DHCP.  Puede ser que veas la IP de APIPA que comienza por 169.254.X.Y.
  * Este problema puede ser a consecuencia de un problema de red o a que el servidor DHCP no esté disponible. Cuando se inicie el servidor DHCP y otros clientes puedan obtener direcciones válidas, compruebe que el cliente tenga una conexión de red válida y que todos los dispositivos de hardware de cliente relacionados (incluidos los cables y los adaptadores de red) funcionen correctamente.
  * Si un servidor DHCP no proporciona direcciones concedidas a los clientes, suele deberse a que el servicio DHCP no se inició. En este caso, es posible que el servidor no esté autorizado para operar en la red.
* Si necesitas reiniciar el servidor de DHCP puedes hacerlo desde la consola: <mark style="color:blue;">`net start dhcpserver`</mark>&#x20;

### Links

* https://learn.microsoft.com/es-es/troubleshoot/windows-server/networking/install-configure-dhcp-server-workgroup





