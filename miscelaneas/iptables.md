# IPTables

Se trata de una herramienta de Linux que permite el filtrado de los paquetes de red, determinando qué paquetes de datos permitimos que lleguen hasta el servidor y cuáles no. Es una herramienta  necesaria que facilita la administración de firewalls en sistemas Linux. Como otros firewall,  funciona a través de reglas.&#x20;

Las **reglas** permiten aceptar, rechazar, o descartar (drop) paquetes basados en criterios como la dirección IP de origen o destino, el puerto, el protocolo, etc.

Adicionalmente, iptables opera sobre diversas **tablas** diseñadas para diferentes propósitos:

* **filter**: tabla predeterminada utilizada para el filtrado de paquetes.
* **nat**: se utiliza para la traducción de direcciones de red (Network Address Translation).
* **mangle**: Permite modificar campos específicos en los encabezados de los paquetes.
* **raw**: Utilizada para configurar excepciones de seguimiento de conexiones.
* **security**: Utilizada para políticas de seguridad basadas en SELinux.

Dentro de cada tabla, existen cadenas ya predefinidas que determinan en qué punto del procesamiento del paquete se aplican las reglas. Estas cadenas son: INPUT, OUTPUT, FORWARD, PREROUTING y POSTROUTING.&#x20;

Cada cadena contiene una lista de reglas que se procesan secuencialmente. Una regla especifica:&#x20;

* los criterios de coincidencia para los paquetes:  dirección IP, puerto, protocolo y&#x20;
* la acción a tomar: ACCEPT, DROP, REJECT, MASQUERADE, etc.

Las acciones que se pueden aplicar a los paquetes son:

* **ACCEPT**: Permite que el paquete continúe su ruta.
* **DROP**: Descarta el paquete silenciosamente.
* **REJECT**: Descarta el paquete y envía una respuesta de error al remitente.
* **MASQUERADE**: Reemplaza la dirección IP de origen del paquete con la dirección IP de la interfaz de salida.
* **SNAT**: Source NAT, modifica la dirección IP de origen del paquete.
* **DNAT**: Destination NAT, modifica la dirección IP de destino del paquete.
* **LOG**: Registra los paquetes que coinciden con la regla.

**Ejemplos**:

Supongamos que queremos agregar una regla que permita todo el tráfico SSH entrante:

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Supongamos que queremos bloquear todo el tráfico HTTP saliente:

```bash
iptables -A OUTPUT -p tcp --dport 80 -j DROP
```

Mediante  instrucciones se le indica al firewall el tipo de paquetes a los que se debe permitir entrar, así como los puertos por donde se pueden recibir dichos paquetes, el protocolo para el envío de datos y cualquier otra información relacionada con el intercambio de datos entre redes.&#x20;

Cuando en el sistema se recibe o se envía un paquete, se recorren todas las  reglas en orden hasta encontrar aquella regla que cumpla las condiciones. Una vez localizada la regla, ésta se activa y ejecuta la acción que tenga establecida sobre el paquete en cuestión.

**Nota**: Las reglas definidas con iptables no son persistentes por defecto y se pierden después de un reinicio. Para hacerlas persistentes, es necesario guardar las reglas en un archivo de configuración y restaurarlas al inicio del sistema.
