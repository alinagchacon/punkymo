---
description: under construction
---

# 🚧 Suricata

Suricata es un sistema de detección y prevención de intrusiones en la red, IDPS. Es un sistema de código abierto, desarrollado por una comunidad de seguridad informática.&#x20;

Se considera de un alto rendimiento y una alta capacidad para examinar el tráfico de la red en tiempo real, es también capaz de identificar patrones maliciones y responder a amenazas en modo activo. Es un sistema versatilidad y potente que lo hacen muy popular en la protección de redes.

### Modos activo y pasivo

Suricata tiene dos modos de funcionamiento: pasivo y activo. Veamos.

**Modo activo**

En este modo Suricata toma medidas inmediatas para prevenir o mitigar amenazas identificadas. bEs capaz de loquear conexiones, ejecutar acciones según las reglas de seguridad configuradas y enviar alertas al sistema.&#x20;

El modo activo es vital para dar una respuesta rápida y en tiempo real a las amenazas. No obstante, puede afectar la entrega de servicios si no se configura correctamente.

**Modo pasivo**

En este modo, Suricata actúa como un simple observador. Va analizando  el tráfico de red pero sin interferir en la entrega de datos. Es un modo interesante  para la monitorización y la recopilación de información sobre posibles amenazas pero sin afectar el rendimiento del tráfico.&#x20;

Es un enfoque útil en aquellos entornos  de trabajo donde una intervención activa pudiera ser riesgosa o no está permitida.

### Funcionamiento

Suricata, para su funcionamiento, usa una serie de técnicas que son las que le permiten el análisis del tráfico de la red y detectar actividades maliciosas. Para su funcionamiento requiere de:

* **Motor de reglas:** Son reglas flexibles que permiten definir patrones y comportamientos específicos asociados con amenazas conocidas o comportamientos anómalos.&#x20;
* **Análisis de protocolos:** Analiza protocolos de red como es el caso de TCP, UDP, ICMP, así como HTTP y DNS. Examina el tráfico en busca de anomalías y patrones maliciosos.
* **Inspección de contenido:** Puede realizar una inspección profunda del contenido del tráfico, identificando amenazas basadas en patrones de cadenas, _exploits_ conocidos y firmas de _malware_.
* **Decodificación de protocolos:** Es capaz de decodificar protocolos en capas, lo que significa que puede analizar y comprender el tráfico en diferentes niveles, desde la capa de red hasta la capa de aplicación.
* **Captura de archivos:** Es capaz de apturar archivos transmitidos a través de la red para su análisis posterior. Esto es crucial para identificar y examinar posibles amenazas basadas en archivos maliciosos.

### Instalación

La prueba de instalación de Suricata la vamos a hacer en un Ubuntu Server 22.04 al que previamente hemos actualizado:

```
sudo apt update && sudo apt upgrade -y
```

La herramienta jq permite mostrar la información de la salida JSN de EVE de Suricata.

```
udo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq
```

Una vez realizada la instalación, podemos comprobar la versión y el estado del servicio:

```
sudo suricata --build-info
sudo systemctl status suricata
```

### Configuración básica

La instalación que vamos a probar aquí es la básica que aparece en el sitio web oficial: [https://docs.suricata.io/en/suricata-7.0.2/quickstart.html](https://docs.suricata.io/en/suricata-7.0.2/quickstart.html). Por tanto,  nos vamos a focalizar en la configuración de la variable `HOME_NET` y de la `interfaz de red`.&#x20;

Esta  variable `HOME_NET` debe incluir la dirección IP de la interfaz de red que queremos monitorizar, así como las redes locales en uso.&#x20;

Vamos a acceder al directorio de configuración de suricata: /etc/suricata/ y hagamos una copia del archivo de configuración de Suricata, esto es:

```
sudo cp /etc/suricata/suricata.yml /etc/suricata/suricata.yml.BKP
```

A continuación, vamos a determinar la interfaz y la IP de red donde Suricata va a estar inspeccionando los paquetes de red:

```
ip addr
```

<figure><img src="../../.gitbook/assets/image (395).png" alt="" width="563"><figcaption><p>Dirección IP de la VM donde tengo instalado Suricata</p></figcaption></figure>

Como se puede ver en el pantallazo anterior, el nombre de la interfaz es `enp0s3`, por lo que debemos ir a la sección  `af-packet`  del archivo `/etc/suricata/suricata.yml` y modificar el nombre de la interfaz de red para que coincidan.&#x20;

<figure><img src="../../.gitbook/assets/image (397).png" alt="" width="430"><figcaption><p>Detalle de la configuración de /etc/suricata/suricata.yml</p></figcaption></figure>

Esta configuración utiliza la configuración recomendada para ejecutar el modo IDS en configuraciones básicas. Existen otras opciones de configuración, específicas para configuraciones de alto rendimiento.

### Reglas, firmas, alertas

Suricata utiliza reglas que permiten activar alertas. Por tanto, debemos instalar y actualizar dichas reglas. Para ello, podemos usar la herramienta siguiente para obtenerlas, actualizarlas y gestionarlas.

```
sudo suricata-update 
```

Nota: Recordad que solo estamos configurando el modo predeterminado, que obtiene el conjunto de reglas de ET Open.

Una vez hecho esto, tendremos instaladas las reglas en el directorio `/var/lib/suricata/rules` y el  archivo `suricata.rules`. Por tanto, podemos reiniciar el servicio:

```
sudo systemctl restart suricata
```

y tendremos suricata en ejecución

<figure><img src="../../.gitbook/assets/image (398).png" alt=""><figcaption><p>Suricata en ejecución</p></figcaption></figure>

Y si queremos comprobar que Suricata está en ejecución podemos ver los logs:

```
sudo tail -f /var/log/suricata/suricata.log
```

Y veremos algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption><p>tail -f /var/log/suricata/suricata.log</p></figcaption></figure>

### Creando alertas

Si queremos probar que Suricata está usando la funcionalidad IDS , debemos usar una firma. Dicha firma tiene ID 2100498 y es la que corresponde al conjunto de reglas ET Open escrita específicamente para  casos de prueba.

**2100498**:

```
alert ip any any -> any any (msg:"GPL ATTACK_RESPONSE id check returned root"; content:"uid=0|28|root|29|"; classtype:bad-unknown; sid:2100498; rev:7; metadata:created_at 2010_09_23, updated_at 2010_09_23;)
```

Esto emitirá una alerta sobre cualquier tráfico IP que contenga el contenido en su carga útil. Esta regla se puede activar fácilmente pero antes de hacerlo, iniciamos `tail` para ver las actualizaciones de `fast.log`.

#### Testeando

Si queremos testear el funcionamiento de  Suricata podemos hacer un curl a una web de testing y después visualizamos el contenido de los logs:

```
curl http://testmynids.org/uid/index.html
sudo tail -f /var/log/suricata/fast.log
```

### Más sobre las alertas

Me puedo descargar:

```
wget https://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
```

descomprimir y mover los archivos a: /var/lib/suricata/rules/

Crear un documento con las rules nuestras, por ejemplo:

```
alert tcp $HOME_NET any -> $EXTERNAL_NET any (msg: "un usuario accedió a Internet"; sid:1000004;)
alert icmp any any -> $HOME_NET any (msg: "ICMP conexión hecha"; sid:1000005;)
alert icmp any any -> $HOME_NET 22 (msg: "Conexión establecida"; sid:1000006;)
```

Lo añadimos en default-rule-path en el /etc/suricata/suricata.yaml

```
default-rule-path: /var/lib/suricata/rules

rule-files:
   - suricata.rules
   - emerging-exploit.rules
   - my-rules
```

### Comprobando

suricata -c /etc/suricata/suricata.yaml -i enp0s3

tail -f /var/log/suricata/fast.log

### &#x20;Firmas



### Links

* [https://docs.suricata.io/en/suricata-7.0.2/quickstart.html](https://docs.suricata.io/en/suricata-7.0.2/quickstart.html)
* [https://suricata.io](https://suricata.io)
* [https://www.digitalocean.com/community/tutorials/understanding-suricata-signatures](https://www.digitalocean.com/community/tutorials/understanding-suricata-signatures)
* [https://www.youtube.com/watch?v=oF4e90EPDug](https://www.youtube.com/watch?v=oF4e90EPDug)
