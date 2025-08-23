---
description: en Ubuntu
hidden: true
---

# Rya-Mininet

### Conceptos básicos

**SDN**:

* Permite eliminar muchas de las limitaciones de las infraestructuras de red actuales.&#x20;
* Separa el plano de datos del plano de control
* Centraliza el estado de la red y la capacidad de toma de decisiones.&#x20;
* Centraliza  la programación en el plano de control (Controlador SDN)
* Simplifica la operación en el plano de datos (Dispositivos de red SDN)&#x20;
* Permite que la infraestructura subyacente sea abstraída para que aplicaciones y servicios puedan tratar a la red como una entidad lógica o virtual.

Al separar la lógica de control de los dispositivos de red, SDN permite la programabilidad de la misma, su administración simplificada y autónoma . Además brinda oportunidades para los operadores, la red y los proveedores de servicios.

La Open Networking Foundation define una arquitectura de alto nivel para SDN con tres capas o planos principales, como se muestra en la figura.

<figure><img src="../../.gitbook/assets/image.png" alt="Arquitectura SDN" width="563"><figcaption></figcaption></figure>

Los dispositivos SDN:&#x20;

* contienen componentes para decidir qué hacer con el tráfico entrante.&#x20;
* el controlador SDN programa los dispositivos de red y presenta una abstracción de la infraestructura de red subyacente a las aplicaciones SDN.
* el controlador permite que una aplicación SDN defina los flujos de tráfico y las rutas en los dispositivos de red.

La comunicación entre capas es posible gracias a la SouthBound y la NorthBound API:

* **Southbound API** - usada para la comunicación entre el controlador SDN y los elementos de red como: switches, routers, etc. Pueden ser de código abierto o propietarias: OpenFlow, NetConf1, Lisp2, OpFlex3, etc.
* **Northbound API** - son Rest APIs utilizadas para la comunicación entre el controlador SDN y los servicios y aplicaciones que corren por encima de la red, en la capa de aplicación. Están integradas dentro del controlador SDN, un ejemplo es el controlador Ryu.

### Instalación

La instalación se va a hacer en una VM con Ubuntu Desktop 22.04.5 (jammy).&#x20;

<mark style="color:purple;">**Mininet**</mark>

Es un emulador de red que crea una red de hosts virtuales, switches, controladores y enlaces. Los hosts de Mininet ejecutan software de red Linux estándar, y sus conmutadores son compatibles con OpenFlow para un enrutamiento personalizado flexible y redes definidas por software.

Además, Mininet facilita la investigación, el desarrollo, el aprendizaje, la creación de prototipos, las pruebas, la depuración y cualquier otra tarea que pueda beneficiarse de tener una red experimental completa en una computadora portátil u otra PC.

* Proporciona un banco de pruebas de red sencillo y económico para el desarrollo de aplicaciones OpenFlow.
* Permite que varios desarrolladores simultáneos trabajen de forma independiente en la misma topología.
* Admite pruebas de regresión a nivel de sistema, repetibles y fáciles de empaquetar.
* Permite realizar pruebas de topología complejas sin necesidad de cablear una red física.
* Incluye una CLI que reconoce la topología y OpenFlow para depurar o ejecutar pruebas en toda la red.
* Admite topologías personalizadas arbitrarias e incluye un conjunto básico de topologías parametrizadas.
* Se puede usar de inmediato sin necesidad de programar.
* Además, proporciona una API de Python sencilla y extensible para la creación y experimentación de redes.
* Mininet proporciona una manera sencilla de obtener el comportamiento correcto del sistema (y, en la medida en que lo permita su hardware, el rendimiento) y de experimentar con topologías.

Las redes Mininet ejecutan código real, incluyendo aplicaciones de red estándar de Unix/Linux, así como el kernel y la pila de red de Linux (incluidas las extensiones del kernel disponibles, siempre que sean compatibles con los espacios de nombres de red).

Gracias a esto, el código que se desarrolla y prueba en Mininet, para un controlador OpenFlow, un conmutador modificado o un host, puede trasladarse a un sistema real con cambios mínimos para realizar pruebas reales, evaluar el rendimiento e implementarlo. Es importante destacar que esto significa que un diseño que funciona en Mininet generalmente puede trasladarse directamente a conmutadores de hardware para el reenvío de paquetes a velocidad de línea.

```
sudo apt install mininet
```

Se puede comprobar la funcionalidad de Mininet haciendo lo siguiente:

```
sudo mn --test pingall
```

<mark style="color:purple;">**Ryu**</mark>

Ryu es un component-based SDN framework, o sea, es un entorno de trabajo que proporciona componentes software que se utilizan en SDN, entre ellos un controlador, con una API bien definida que facilita a los desarrolladores la creación de nuevas aplicaciones de administración y control de red.

Actualizamos los paquetes del sistema:

```
sudo apt update && sudo apt upgrade -y
```

{% hint style="warning" %}
Nota: antes de continuar con la instalación es bueno aclarar que justamente me puse a probar en una VM que ya tenía con un Ubuntu Desktop 22.04 y con Python3.10. Después de varios intentos para hacer la instalación (sencilla de por sí) leí que ryu tenía conflictos precisamente con Python3.10 y lo recomendable era bajar a la 3.9. Como la información es del 2023 pongo en duda que sea la única forma de hacerlo. Ya debe estar corregido (pero ahora no me sobra el tiempo para investigar más). Puedes verlo en: [https://github.com/faucetsdn/ryu/issues/169](https://github.com/faucetsdn/ryu/issues/169)
{% endhint %}

Podemos hacer la instalación de dos tipos pero aquí solo muestro una:

Nos aseguramos tener pip, el sistema de gestión de paquetes para instalar, actualizar y desinstalar paquetes de software escritos en Python. &#x20;

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get install virtualenv python3.9 python3.9-distutils
```

Creamos el entorno virtual

```
virtualenv -p`which python3.9` ryu-python3.9-venv
source ryu-python3.9-venv/bin/activate
echo $VIRTUAL_ENV #check if we are indeed in the virtual environment

pip install ryu
pip uninstall eventlet
pip install eventlet==0.30.2

ryu-manager --help
```



<mark style="color:purple;">**iPerf**</mark>

Es una herramienta cliente-servidor que sirve para medir el ancho de banda de la red. Una máquina actúa como servidor a la espera de tráfico y otra como cliente que envía tráfico para evaluar la velocidad de conexión entre ambos. Es una herramienta de línea de comandos para TCP y UDP útil para diagnosticar problemas y entender el rendimiento de la red.&#x20;

<mark style="color:purple;">**Ingeniería de tráfico**</mark>

Se trata de un mecanismo que permite optimizar el rendimiento de la red de datos, analizando dinámicamente, prediciendo y regulando el comportamiento de los datos transmitidos por la red.

La ingeniería de tráfico estudia la monitorización y gestión del tráfico de red y diseña mecanismos de enrutamiento razonables para guiar el tráfico de red a fin de mejorar la utilización de los recursos\
de red y cumplir mejor los requisitos de la calidad de servicio (QoS) de la misma.

### Links

* [https://mininet.org/overview](https://mininet.org/overview)
*



