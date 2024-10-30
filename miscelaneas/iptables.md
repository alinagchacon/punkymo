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

### Algunas de las opciones de iptables habituales

* **-A** —append: Añade una regla a una cadena (al final).
* **-C** —check : Busca una regla que coincida con los requerimientos de la cadena.
* **-D** —delete: Borra las reglas especificadas de una cadena.
* **-F** —flush: Elimina todas las reglas.
* **-I** —insert: Añade una regla a una cadena en una posición dada.
* **-L** —list : Muestra todas las reglas de una cadena.
* **-N** -new-chain: Crea una nueva cadena.
* **-v** —verbose: Muestra más información cuando se usa una opción de lista.
* **-X** —delete-chain: Elimina la cadena proporcionada.

**Ejemplos**:

(1) Supongamos que queremos agregar una regla que <mark style="color:purple;">**permita todo el tráfico SSH entrante**</mark>:

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```



(2) Supongamos que queremos <mark style="color:purple;">**bloquear todo el tráfico HTTP saliente**</mark>:

```bash
iptables -A OUTPUT -p tcp --dport 80 -j DROP
```

Mediante  instrucciones se le indica al firewall el tipo de paquetes a los que se debe permitir entrar, así como los puertos por donde se pueden recibir dichos paquetes, el protocolo para el envío de datos y cualquier otra información relacionada con el intercambio de datos entre redes.&#x20;

Cuando en el sistema se recibe o se envía un paquete, se recorren todas las  reglas en orden hasta encontrar aquella regla que cumpla las condiciones. Una vez localizada la regla, ésta se activa y ejecuta la acción que tenga establecida sobre el paquete en cuestión.

**Nota**: Las reglas definidas con iptables no son persistentes por defecto y se pierden después de un reinicio. Para hacerlas persistentes, es necesario guardar las reglas en un archivo de configuración y restaurarlas al inicio del sistema.



(3) Supongamos que queremos <mark style="color:purple;">**autorizar el tráfico de localhost**</mark> de modo que todo lo que venga de su sistema  pase a través del firewall (iptables). O sea, configurar el firewall de modo que acepte el tráfico para la interfaz localhost (lo) (-i). Algo necesario si se quiere para que las aplicaciones puedan comunicarse con la interfaz localhost.

```
sudo iptables -A INPUT -i lo -j ACCEPT
```



(4) Para <mark style="color:purple;">**autorizar el tráfico web HTTP**</mark>, introduzca el siguiente comando:

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```



(5)  Para <mark style="color:purple;">**autorizar el tráfico de internet HTTPS**</mark>, introduzca el siguiente comando:

```bash
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```



(6) Un método para eliminar el **número de línea de una regla**.

```bash
sudo iptables -P INPUT DROP 
```

Enumeramos todas las reglas:

```bash
sudo iptables -L --line-numbers
```

<figure><img src="../.gitbook/assets/image (393).png" alt=""><figcaption><p>enumerando las reglas de iptables</p></figcaption></figure>

```
sudo iptables -t nat -L --line-numbers
```

<figure><img src="../.gitbook/assets/image (392).png" alt=""><figcaption><p>Enumerando las reglas de iptables</p></figcaption></figure>

Buscamos la línea de la regla de iptables que necesitamos eliminar y ejecutamos el siguiente comando:

```bash
sudo iptables -D INPUT <Number>
```

<mark style="color:red;">En el siguiente pdf te dejo una ayuda escrita por Leo, Bea y Monti.</mark>  <mark style="color:red;">Gracias chic@s!</mark>

{% file src="../.gitbook/assets/REENVIOS DE PUERTOS_TAS_M.MOUTOUTO_L.DUARTE_BSUAREZ.pdf" %}
Una ayuda para todos de Leo, Bea y Monti
{% endfile %}

## Links

* [https://help.ovhcloud.com/csm/es-es-dedicated-servers-firewall-iptables?id=kb\_article\_view\&sysparm\_article=KB0043439](https://help.ovhcloud.com/csm/es-es-dedicated-servers-firewall-iptables?id=kb\_article\_view\&sysparm\_article=KB0043439)
*

