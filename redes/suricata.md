---
description: Under construction
---

# Suricata



Suricata es un sistema de detección y prevención de intrusiones en la red, IDPS. Es un sistema de código abierto, desarrollado por una comunidad de seguridad informática.&#x20;

Es un motor de alto rendimiento con una alta capacidad para examinar el tráfico de la red en tiempo real, es también capaz de identificar patrones maliciones y responder a amenazas en modo activo. Es un sistema versatilidad y potente que lo hacen muy popular en la protección de redes.

Un IDS previene y detecta, esto es, se mantiene a la escucha  del tráfico de red, aplicando reglas y haciendo reconocimiento de patrones de atque para evitar ataques a la red.

Sirve de complemento de un firewall donde podemos tener abiertos puertos como por ejemplo: el 80 o 443 del servicio web y nos permitiría detectar intrusiones en el sistema.

Hay dos tipos de IDS:

* Pasivo: permite registrar las intrusiones y manda alertas.
* Activo: como el pasivo pero es capaz de bloquear las direcciones IP o cerrar puertos.

### Modos activo y pasivo

Suricata tiene de los dos modos de funcionamiento: pasivo y activo. Veamos.

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
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt upgrade
sudo apt install suricata -y
```

Una vez realizada la instalación, podemos comprobar la versión y el estado del servicio:

```
sudo suricata --build-info
sudo systemctl status suricata
```

Para levantar Suricata `on-boot`:

```
sudo systemctl enable suricata
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

<figure><img src="../.gitbook/assets/image (395).png" alt="" width="563"><figcaption><p>Dirección IP de la VM donde tengo instalado Suricata</p></figcaption></figure>

Como se puede ver en el pantallazo anterior, el nombre de la interfaz es `enp0s3`, por lo que debemos ir a la sección  `af-packet`  del archivo `/etc/suricata/suricata.yml` y modificar el nombre de la interfaz de red para que coincidan.&#x20;

<figure><img src="../.gitbook/assets/image (397).png" alt="" width="430"><figcaption><p>Detalle de la configuración de /etc/suricata/suricata.yml</p></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (398).png" alt=""><figcaption><p>Suricata en ejecución</p></figcaption></figure>

Y si queremos comprobar que Suricata está en ejecución podemos ver los logs:

```
sudo tail -f /var/log/suricata/suricata.log
```

Y veremos algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (399).png" alt=""><figcaption><p>tail -f /var/log/suricata/suricata.log</p></figcaption></figure>

### Creando alertas&#x20;

<mark style="color:orange;">Si queremos probar que Suricata está usando la funcionalidad IDS , debemos usar una firma. Dicha firma tiene ID 2100498 y es la que corresponde al conjunto de reglas ET Open escrita específicamente para  casos de prueba.</mark>

<mark style="color:orange;">**2100498**</mark><mark style="color:orange;">:</mark>

```
alert ip any any -> any any (msg:"GPL ATTACK_RESPONSE id check returned root"; content:"uid=0|28|root|29|"; classtype:bad-unknown; sid:2100498; rev:7; metadata:created_at 2010_09_23, updated_at 2010_09_23;)
```

<mark style="color:orange;">Esto emitirá una alerta sobre cualquier tráfico IP que contenga el contenido en su carga útil. Esta regla se puede activar fácilmente pero antes de hacerlo, iniciamos</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`tail`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">para ver las actualizaciones de</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`fast.log`</mark><mark style="color:orange;">.</mark>

#### Testeando

Si queremos testear el funcionamiento de  Suricata podemos hacer un curl a una web de testing y después visualizamos el contenido de los logs:

```
curl http://testmynids.org/uid/index.html
sudo tail -f /var/log/suricata/fast.log
```

### Más sobre las alertas

Me puedo descargar:

<pre><code><strong>wget https://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
</strong></code></pre>

descomprimir y mover los archivos a: /var/lib/suricata/rules/

Crear un documento con las reglas nuestras, por ejemplo:

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

<mark style="color:red;">Me falta ...</mark>

```
suricata -c /etc/suricata/suricata.yaml -i enp0s3
```

```
tail -f /var/log/suricata/fast.log
```

\
Firmas
------

<mark style="color:red;">Me falta ...</mark>

## Prometheus

Prometheus es un conjunto de herramientas de código abierto que permite la monitorización de sistemas. Fue desarrollado originalmente en SoundCloud y desde su creación en 2012, numerosas empresas y organizaciones lo han adoptado. El proyecto cuenta con una comunidad de desarrolladores y usuarios muy activa y fue escrito en el lenguaje de programación `Go`.&#x20;

Para su funcionamiento Prometheus recopila y almacena métricas como datos de series temporales, esto es:  se almacenan en la base de datos junto al instante de tiempo en el que el valor se ha registrado.

Estas métricas que almacena Prometheus  dependen de la aplicación o del sistema que se vaya a monitorizar. Lo que vamos a monitorizar o medir varía según la aplicación o las necesidades. Si hablamos de un servidor web, serían los tiempos de solicitud; si se trata de una base de datos, puede ser el número de conexiones o consultas activas. También pudiéramos medir el uso de CPU, de la  memoria, etc.&#x20;

Las métricas resultan fundamentales para comprender por qué una aplicación funciona de un modo. Por ejemplo, pudiéramos querer saber por qué una aplicación web va lenta. Para ello necesitaríamos conocer si la aplicación se ralentiza cuando el número de solicitudes es alto. Tendríamos que disponer de una métrica de conteo de solicitudes.

En definitiva, toda la información recogida a través de las métricas nos facilitaría  el diagnóstico de los errores en los servicios, en los sistemas o aplicaciones que se  están monitorizando.

El ecosistema de Prometheus consta de múltiples componentes, muchos de los cuales son opcionales pero dispone de 3 fundamentales que son:

* **Servidor -** que almacena los datos de las métricas. Es el servidor principal  que extrae y almacena datos de series temporales.
* **Librería cliente -** que sirve para calcular y exponer las métricas al cliente, para instrumentar el código de la aplicación.
* **Gestor de alertas -** genera las alertas basadas en reglas.



### Instalación

Para instalar, me bastó:

```
sudo apt install prometheus
```

Podemos verificar que lo tenemos en escucha por el puerto 9090.

<figure><img src="../.gitbook/assets/image (413).png" alt=""><figcaption><p>prometheus se encuentra en escucha por el puerto 9090</p></figcaption></figure>

Por tanto, si tengo la VM en modo adaptador puente, puedo acceder al servicio a través de:

<figure><img src="../.gitbook/assets/image (414).png" alt="" width="563"><figcaption><p>Panel de Prometheus</p></figcaption></figure>

**Directorio de trabajo**

Tengo prometheus en la siguiente ubicación. Ahí es donde se encuentra el archivo de configuración: prometheus.yml.

<pre><code><strong>/etc/prometheus/prometheus.yml
</strong></code></pre>

**Instalación y configuración del `node_exporter`**

> Este `node_exporter` es el agente que va a recopilar y envíar las métricas de nuestro servidor Ubuntu Server. Recopilará parámetros como CPU, RAM, sistema de archivos y estadísticas de la red.

Lo primero será crear el usuario de servicio "node\_exporter”:

```
useradd -m -s /bin/false node_exporter
```

Ahora nos descargamos el archivo del node\_exporter. Para ello, nos vamos a la página oficial de Prometheus [https://prometheus.io/download/](https://prometheus.io/download/).

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
```

Una vez tenemos el archivo descargado en nuestro linux, lo vamos a descomprimir y copiar al directorio `/usr/local/bin`:

```
tar -zxpvf node_exporter-1.5.0.linux-amd64.tar.gz
cd node_exporter-1.5.0.linux-amd64
sudo cp node_exporter /usr/local/bin/
sudo chown -R node_exporter:node_exporter /usr/local/bin/node_exporter
```

Con el siguiente paso vamos a crear el servicio `node_exporter`:

```
sudo nano /etc/systemd/system/node_exporter.service
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

Guardamos, cerramos y reiniciamos los servicios para que tome la nueva configuración del systemd y luego volver a iniciar el servicio de node\_exporter:

```
systemctl daemon-reload
systemctl start node_exporter
systemctl enable node_exporter
```

Podemos volver a comprobar que tenemos el servicio activo por el puerto 9100:

<figure><img src="../.gitbook/assets/image (415).png" alt=""><figcaption><p>Comprobamos que tenemos el servicio node_exporter escuchando por el puerto 9100</p></figcaption></figure>

Para finalizar con esta parte de la configuración añadiremos el nuevo job `node_exporter` al archivo de configuración `prometheus.yml`:

```mathml
sudo nano /etc/prometheus/prometheus.yml

# Global config
global:
  scrape_interval:     15s
  evaluation_interval: 15s
  scrape_timeout: 15s
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']
  - job_name: 'node_exporter'
    static_configs:
    - targets: ['localhost:9100'] 
```

Ahora deberíamos reiniciar el servicio “prometheus.service” y  poder visualizar las métricas por la interfaz web:

```
systemctl restart prometheus.service
```

### Grafana

Para instalar grafana, hacemos:

```
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_11.6.0_amd64.deb
sudo dpkg -i grafana-enterprise_11.6.0_amd64.deb
```

Podemos verificar que tenemos el servicio activo escuchando por el puerto 3000:

<figure><img src="../.gitbook/assets/image (416).png" alt=""><figcaption><p>Grafana por el puerto 3000</p></figcaption></figure>

Finalmente podremos acceder a la web de Grafana. Primero nos aparece un usuario y contraseña para el cual usamos las credenciales por defecto: user: admin, password: admin.

<figure><img src="../.gitbook/assets/image (417).png" alt=""><figcaption><p>Dashboard de grafana </p></figcaption></figure>

<mark style="color:red;">To be continued ...</mark>

### Links

Suricata

* [https://docs.suricata.io/en/suricata-7.0.2/quickstart.html](https://docs.suricata.io/en/suricata-7.0.2/quickstart.html)
* [https://suricata.io](https://suricata.io)
* [https://www.digitalocean.com/community/tutorials/understanding-suricata-signatures](https://www.digitalocean.com/community/tutorials/understanding-suricata-signatures)
* [https://rules.emergingthreats.net/open/suricata/](https://rules.emergingthreats.net/open/suricata/)
* [https://www.maquinasvirtuales.eu/implementar-soc-instalacion-suricata-bajo-proxmox/](https://www.maquinasvirtuales.eu/implementar-soc-instalacion-suricata-bajo-proxmox/)
* [https://www.youtube.com/watch?v=oF4e90EPDug](https://www.youtube.com/watch?v=oF4e90EPDug)

Prometheus

* [https://aprenderbigdata.com/prometheus/](https://aprenderbigdata.com/prometheus/)
* [https://prometheus.io/docs/introduction/overview/](https://prometheus.io/docs/introduction/overview/)

Grafana

* [https://grafana.com/grafana/download](https://grafana.com/grafana/download)
* [https://medium.com/@ismaelaguilera\_/instalación-y-configuración-de-prometheus-grafana-centos8-331c0e43ccc1](https://medium.com/@ismaelaguilera_/instalaci%C3%B3n-y-configuraci%C3%B3n-de-prometheus-grafana-centos8-331c0e43ccc1)

