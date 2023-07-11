---
description: Apuntes
---

# Portainer

Se trata de una herramienta ligera y _open-source_ que permite la gestión de contenedores sobre Docker o Docker Swarm. También ofrece una interfaz gráfica para gestionar el _host_ Docker desde cualquier navegador. Adicionalmente, tiene soporte para Raspberry Pi y se puede desplegar como un simple contenedor. Es una herramienta enfocada a visualizar el estado de los contenedores. Algunos apuntes de Portainer pero lo ideal es ir a su web de documentación:

{% embed url="https://docs.portainer.io/" fullWidth="false" %}
Documentación de Portainer
{% endembed %}

&#x20;Tenemos una buena ayuda en la aplicación. No obstante, hagamos un tour por algunas de las opciones del menú.

<figure><img src="../../.gitbook/assets/image (216).png" alt=""><figcaption><p><a href="https://docs.portainer.io/user/docker/networks">https://docs.portainer.io</a></p></figcaption></figure>

**Stack**

Es una colección de servicios, generalmente relacionados con una aplicación o uso. Por ejemplo, una definición de pila de WordPress puede incluir un contenedor de servidor web (como nginx) y un contenedor de base de datos (como MySQL).\
\
Portainer le permite implementar una pila completa desde una plantilla predeterminada o una plantilla personalizada.

Debes ingresar un nombre para la pila o stack y establecer los valores de configuración necesarios que diferirán de una plantilla a otra. También puedes activar o desactivar **Habilitar control de acceso** según sea necesario.

#### Container

Portainer le permite implementar un contenedor independiente de la lista de plantillas predeterminadas.&#x20;

En el menú, puedes seleccionar **Plantillas de aplicaciones** y activar **Mostrar plantillas** de contenedor y luego seleccionar la aplicación que deseas implementar.

<figure><img src="../../.gitbook/assets/image (220).png" alt=""><figcaption><p>Container</p></figcaption></figure>

#### Images

Las images  se usan para construir contenedores. Cada imagen define las piezas necesarias para construir y configurar un contenedor y se puede reutilizar muchas veces. La sección Imágenes en Portainer te permite interactuar con las imágenes en un entorno.

Si quieres instalar una imagen basta con buscarla y clicar en Pull image para descargarla. Vamos, para no tener que hacerlo por línea de comandos de docker.

<figure><img src="../../.gitbook/assets/image (212).png" alt=""><figcaption></figcaption></figure>

En este caso, se nos muestra como sigue:

<figure><img src="../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

#### Network

Portainer admite este tipo de redes:&#x20;

* **Bridge**: Si no especifica un controlador, este tipo de red se creará de forma predeterminada. Las redes puente se utilizan normalmente cuando sus aplicaciones se ejecutan en contenedores independientes que necesitan comunicarse entre sí.&#x20;
* **macvlan:** Estas redes te permiten asignar una dirección MAC a un contenedor, haciéndolo aparecer como un dispositivo físico en su red. El demonio Docker enruta el tráfico a los contenedores en función de sus direcciones MAC. El uso del controlador macvlan es a veces la mejor opción cuando se trata de aplicaciones heredadas que esperan conectarse directamente a la red física, en lugar de enrutarse a través de la pila de red del host Docker.
* **ipvlan.** Similar a macvlan, la diferencia clave es que los puntos finales tienen la misma dirección MAC. \
  ipvlan admite los modos L2 y L3.&#x20;
  * En el modo ipvlan L2, cada punto final obtiene la misma dirección MAC pero diferentes direcciones IP.&#x20;
  * En el modo ipvlan L3, los paquetes se enrutan entre puntos finales, lo que brinda una mejor escalabilidad.
* **Overlay**: Estas redes superpuestas conectan varios demonios Docker y permiten que los servicios de enjambre se comuniquen entre sí. También se pueden usar estas redes superpuestas para facilitar la comunicación entre un servicio de enjambre y un contenedor independiente, o entre dos contenedores independientes en diferentes demonios Docker.

#### Volumen&#x20;

Un volumen es un área de almacenamiento de datos que se puede montar en un contenedor para proporcionar almacenamiento persistente. A diferencia de los montajes de enlace, los volúmenes son independientes del sistema operativo subyacente y están completamente administrados por Docker Engine.

Si se creó un volumen con una bandera externa, fuera de Portainer, significa que Portainer tiene un conocimiento limitado sobre él en comparación con uno creado dentro de Portainer.&#x20;

Una etiqueta de no utilizado significa que Portainer no puede ver ninguna aplicación que esté utilizando este volumen.&#x20;

Esta etiqueta también puede aparecer en recursos externos debido a la información limitada disponible.&#x20;

En Portainer puede ver una lista de los volúmenes en su entorno, agregar nuevos volúmenes y eliminar volúmenes existentes.
