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



### Links

* [https://docs.suricata.io/en/suricata-7.0.2/quickstart.html](https://docs.suricata.io/en/suricata-7.0.2/quickstart.html)
