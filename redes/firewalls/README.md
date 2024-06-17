---
description: Algunas consideraciones
---

# Firewalls

Se trata de otro dispositivo más de seguridad dentro de una red que permite monitorizar el tráfico entrante y saliente así como decidir si debe permitir o bloquear determinado tráfico en función de las restricciones de seguridad predefinidas.

LLevan siendo, desde hace más de 25 años, la primera línea de defensa en temas de seguridad de la red. Permiten establecer una barrera entre las redes internas seguras, controladas y fiables y las redes externas poco fiables como Internet.&#x20;

Los firewall pueden ser de hardware, de software o ambos tipos.

<figure><img src="../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption><p>Firewall</p></figcaption></figure>

### Tipos de firewall

Tomando nota de los que explica Cisco en su web ([https://www.cisco.com/c/es\_es/products/security/firewalls/what-is-a-firewall.html](https://www.cisco.com/c/es\_es/products/security/firewalls/what-is-a-firewall.html)) tenemos diferentes tipos de firewall entre los cuales podemos ver:

1. **Firewall proxy:** Funciona en fases iniciales y hace la función de gateway entre una red y otra para una aplicación determinada. Pueden aportar otras funciones como: contenido de caché y seguridad, ya que evitan conexiones directas desde fuera de la red.&#x20;
2. **Stateful inspection firewall:** Se le considera un firewall **tradicional** y permite bloquear el tráfico según criterios basados en el estado, el puerto y el protocolo. Monitoriza toda la actividad desde la apertura de una conexión hasta que se cierra.
3. **Firewall para gestión unificada de amenazas (UTM):** Combina de manera independiente funciones de un stateful inspection firewall, con prevención de intrusiones y antivirus. También puede incluir otros servicios como la gestión en la nube.&#x20;
4. **Firewall de última generación (NGFW):** En la evolución de los firewalls han dejado de ser un simple paquete de filtrado y una inspección de estados. Muchas compañías están implementando firewalls de última generación para detener amenazas modernas como malware avanzado y ataques en la capa de aplicación. Algunas de las ventajas que debe incluir un firewall de última generación son:
   1. Funciones estándar de firewall como es la inspección de estados
   2. Prevención de intrusiones
   3. Detección y control de aplicaciones que puedan generar riesgos
   4. Técnicas que permitan hacer frente a los cambios en las amenazas para la seguridad

### Diseño

El diseño de un firewall tiene como objetivo el poder permitir o denegar el tráfico según el origen, el destino y el tipo de tráfico. Hay diseños simples que solamente diseñan una red externa y una interna, y que son determinadas por dos interfaces en un firewall (o router).

#### Privado y público

<figure><img src="../../.gitbook/assets/image (6) (5).png" alt=""><figcaption><p>Pública / Privada</p></figcaption></figure>



Normalmente, un firewall con dos interfaces se configura del siguiente modo:

* El tráfico procedente de la red privada se autoriza e inspecciona mientras viaja hacia la red pública. También se autoriza el tráfico inspeccionado que regresa de la red pública y está relacionado con el tráfico que se originó en la red privada.
* Generalmente, se bloquea el tráfico procedente de la red pública que viaja hacia la red privada.

#### Zona perimetral

Se denomina así:  zona perimetral (DMZ, Demilitarized Zone) al diseño de firewall donde, por norma general, hay una interfaz interna conectada a la red privada, una interfaz externa conectada a la red pública y una interfaz de DMZ .

<figure><img src="../../.gitbook/assets/image (31) (2).png" alt=""><figcaption><p>Red Interna + DMZ + Internet</p></figcaption></figure>

* El tráfico procedente de la red privada se inspecciona mientras viaja hacia la red pública o la DMZ. Este tráfico se permite casi sin restricciones. También se permite el tráfico inspeccionado que regresa a la red privada desde la DMZ o la red pública.
* El tráfico procedente de la DMZ y que viaja hacia la red privada, se bloquea con frecuencia.
* Siempre y cuando cumpla con los requisitos de servicio,  se permite el tráfico procedente de la DMZ y que viaja hacia la red pública.

#### Firewall de políticas basados en zonas - ZPF

En los <mark style="color:blue;">`Zone-based policy firewalls ZPF`</mark> o firewalls de políticas basadas en zonas  se utiliza el concepto de <mark style="color:blue;">`zonas`</mark> que ofrecen mayor flexibilidad. Se dice que es una zona a un grupo que contiene al menos una interfaz y que tiene funciones o características similares. Las zonas ayudan a especificar dónde se debe implementar una regla o política de firewall.&#x20;

#### Ejemplo

Las políticas de seguridad para las redes LAN1 y LAN2 son similares y pueden agruparse en una zona para las configuraciones de firewall. De modo predeterminado, se tiene que en la misma zona, el tráfico entre interfaces no está sujeto a ninguna política y puede pasar libremente.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (4) (2).png" alt=""><figcaption><p>Dos redes LAN internas pertenecientes a la misma zona</p></figcaption></figure>

Por el contrario, se bloquea todo el tráfico de zona a zona y con el fin de permitir el tráfico entre zonas, se debe configurar una política que permita o inspeccione dicho tráfico.

La única excepción a esta política predeterminada de denegar todo (deny any) es la auto-zona del router. Esta zona autónoma del router es el propio router y además incluye todas las direcciones IP de las interfaces del mismo.&#x20;

Las configuraciones de políticas que incluyen la zona autónoma se aplican al tráfico hacia el router y desde de él. No hay ninguna política para este tipo de tráfico de manera predeterminada. El tráfico que debe tenerse en cuenta al diseñar una política para la zona autónoma incluye el tráfico del <mark style="color:blue;">`plano de control`</mark> y del <mark style="color:blue;">`plano de administración`</mark> como SSH, SNMP y otros protocolos de enrutamiento.

### Defensa por capas

Ya hemos visto lo que sería la defensa en capas de una red, donde la seguridad:

* **principal de la red** - es proteger contra software malicioso y anomalías de tráfico, así como aplicar políticas de red y garantizar su supervivencia.
* **perimetral** - es proteger los límites entre zonas.
* **de las comunicaciones** - es proporcionar seguridad de la información
* **de terminales** - es proporcionar identidad y cumplimiento de políticas de seguridad de dispositivos.

La defensa en capas utiliza diferentes tipos de firewalls que se pueden combinar (en capas) para agregar mayor seguridad (o seguridad en profundidad) a una organización.&#x20;

Las políticas pueden aplicarse entre las capas y dentro de ellas. Siendo estos puntos de aplicación de las políticas las que determinan si el tráfico se reenvía o se descarta.&#x20;

#### Ejemplo:&#x20;

El tráfico que proviene de la red no confiable primero encuentra un filtro de paquetes en el router de borde.&#x20;

Si la política lo permite, el tráfico pasa al firewall de detección o al sistema de host de bastión que aplica más reglas al tráfico y descarta los paquetes sospechosos. Un host de bastión es un PC reforzado que se encuentra en la DMZ generalmente.&#x20;

Más adelante, el tráfico va a un router de detección interior. El tráfico pasa al host de destino interno solo después de pasar correctamente por todos los puntos de aplicación de políticas entre el router externo y la red interna. Este tipo de configuración de DMZ es lo que se denomina configuración de <mark style="color:blue;">`subred filtrada`</mark>.

Está claro que un enfoque de defensa por capas no garantiza la seguridad de una red interna segura. A la hora de construir una defensa completa en profundidad, un administrador de red debe considerar muchos factores como pueden ser que los firewalls NO:

* **detienen** el tipo de intrusiones que provienen de hosts dentro de una red o zona.
* **protegen** contra las instalaciones de puntos de acceso fraudulentos.
* **reemplazan** los mecanismos de respaldo y recuperación de desastres como resultado de ataques o fallas de hardware.
* **pueden** sustituir a usuarios y administradores informados.

#### Consideraciones para la defensa de red en capas

La siguiente lista es una lista parcial de prácticas razonables que puede servir como punto de partida para una política de seguridad de firewall.

* Coloca firewalls en los límites de seguridad. Dado que son una parte fundamental de la seguridad de la red, aunque no se puede confiar exclusivamente en un firewall para la seguridad de una red en una organización.
* Deniega todo el tráfico por defecto.
* Permite solo los servicios que sean necesarios.
* Asegúrate de controlar el acceso físico al firewall.
* Monitoriza los registros de firewall de manera regular.

Recuerda que los firewalls protegen principalmente contra ataques técnicos que se originan desde el exterior.

### Redes seguras con firewall

Hay varios tipos de firewalls:

* de **filtrado de paquetes (sin estado)** que proporcionan filtrado de Capa 3 y, a veces, de Capa 4.
* **inspección de estado** de firewall que permiten o bloquean el tráfico basado en estado, puerto, y protocolo.
* de **gateway de aplicación (firewall proxy)** que filtran la información de capa 3, 4, 5 y 7.
* de **próxima generación** que proporcionan servicios adicionales más allá de los gateway de aplicaciones, como la prevención de intrusiones integrada, el conocimiento y control de las aplicaciones para ver y bloquear las aplicaciones de riesgo, el acceso a futuras fuentes de información y las técnicas para hacer frente a las amenazas de seguridad en desarrollo.

### En resumen

* Una arquitectura de seguridad común define los límites del tráfico que entra y sale de la red. Si miramos una topología con acceso a redes externas o públicas, debemos ser capaces de determinar la arquitectura de seguridad.&#x20;
* Algunos diseños son muy simples y diseñan una red externa y una interna determinadas por dos interfaces en un firewall. Sin embargo, las redes que requieren un acceso público a servicios a menudo tienen una DMZ a la que puede acceder el público, bloqueando el acceso a la red interna.&#x20;
* Los ZPF emplean el concepto de las zonas para brindar más flexibilidad. Una zona es un grupo de una o más interfaces que tienen funciones similares, características y requerimientos de seguridad.&#x20;
* Un enfoque de seguridad por capas utiliza firewalls y otras medidas de seguridad para proporcionar seguridad en diferentes capas funcionales de la red.

### Algunas consideraciones a tener en cuenta

#### Propiedades comunes a todos los firewall

Todos los firewalls comparten algunas propiedades comunes:

* Resisten ataques de red.
* Son el único punto de tránsito entre las redes corporativas internas y las redes externas porque todo el tráfico circula por ellos.
* Aplican la política de control de acceso.

#### Ventajas

Los firewalls en una red brindan beneficios tales como:

* **Evitar** la exposición de hosts, recursos y aplicaciones confidenciales a usuarios no confiables.
* **Sanear** el flujo de protocolos, lo que evita el aprovechamiento de las fallas de protocolos.
* **Bloquear** los datos maliciosos de servidores y clientes.
* **Simplificar** la administración de la seguridad, ya que la mayor parte del control del acceso a redes se deriva a unos pocos firewalls de la red

#### Limitaciones del firewall

No todo son ventajas puesto que los firewalls también tienen algunas limitaciones como son:

* Si está mal configurado puede tener graves consecuencias para la red, y convertirse en un punto único de falla (por ejemplo).
* Los datos de muchas aplicaciones no se pueden transmitir con seguridad mediante firewalls.
* Los usuarios pueden buscar maneras de esquivar el firewall para recibir material bloqueado, lo que expone a la red a posibles ataques.
* Puede reducirse la velocidad de la red.
* El tráfico no autorizado se puede tunelizar u ocultar como tráfico legítimo a través del firewall.

### **Links**

* [https://www.cloudflare.com/es-es/learning/security/what-is-a-firewall/](https://www.cloudflare.com/es-es/learning/security/what-is-a-firewall/)
* [https://www.cisco.com/c/es\_es/products/security/firewalls/what-is-a-firewall.html](https://www.cisco.com/c/es\_es/products/security/firewalls/what-is-a-firewall.html)

