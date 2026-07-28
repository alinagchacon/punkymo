---
description: Tomado de https://redes.umh.es/cisco/CCNA/es/RSE/index.html#4.1.1.5
---

# Redes conmutadas

Se dice que una red es convergente cuando emplean sistemas de voz, telefonía IP, gateway de voz, video, videoconferencias. Dichas redes tienen algunas características como:

* Procesamiento de llamadas telefónicas, identificador y transferencia de llamadas, llamadas en espera y conferencias.
* Mensajería de voz.
* Recepción de llamadas importantes en cualquier lugar.
* Contestador automático.

Indiscutiblemente es ventajoso dado que se debe instalar y administrar una única red física, facilitando el ahorro considerable en la instalación y administración de las redes de voz, video y datos.

Esta convergencia de los servicios en la red ha dado lugar a la evolución de las redes haciéndolas más complejas. Para permitir la administración de este entorno complejo, se requiere un diseño estructurado.

## Cisco Borderless Networks

Con las crecientes demandas de las redes convergentes, la red se debe desarrollar con un enfoque arquitectónico que integre inteligencia, simplifique las operaciones y sea escalable para satisfacer demandas futuras. Uno de los más recientes desarrollos en el diseño de red es la llamada Cisco Borderless Networks o redes sin fronteras.

<figure><img src="../.gitbook/assets/image (42).png" alt="" width="563"><figcaption><p>Tomado de <a href="https://redes.umh.es/cisco/CCNA/es/RSE/index.html#4.1.1.3">https://redes.umh.es/cisco/CCNA</a></p></figcaption></figure>

Cisco Borderless Networks ofrece el tipo de estructura que permite unificar el acceso por cable y el acceso inalámbrico incluyendo seguridad, control del acceso y monitorización del rendimiento utilizando diferentes tipos de dispositivos. Se construye la red sin fronteras sobre una infraestructura jerárquica de hardware escalable y recuperable.

En la combinación de este tipo de infraestructuras de hardware con soluciones de software basadas en políticas, las redes sin fronteras ofrecen dos servicios principales:

* servicios de red y
* servicios de terminales y usuarios

administrados a través de un sistema integrado.

### Jerarquía en las redes conmutadas sin fronteras

Para poder crear una red de este tipo se requiere del uso de unos principios sólidos de diseño de red que nos permita asegurar la máxima disponibilidad, flexibilidad, seguridad y facilidad de administración. Estas redes deben funcionar teniendo en cuenta los requisitos actuales pero también los servicios y las tecnologías que pueden existir en un futuro.

A la hora de diseñar redes conmutadas sin fronteras debemos considerar lo siguiente:

* **Jerarquía**: simplifica la implementación, el funcionamiento y la administración de la red. Reduce los errores en cada nivel. facilita la comprensión de la función de cada dispositivo en cada nivel.
* **Modularidad**: ayuda a la expansión de la red y permite habilitar servicios integrados.
* **Recuperación ante fallos**: mantener la red siempre activa satisfaciendo las expectativas del usuario.
* **Flexibilidad**: comparte el tráfico mediante el uso de todos los recursos de red.

El diseño jerárquico de una red conmutada sin fronteras sienta las bases que permiten que los diseñadores de red superpongan las características de seguridad, movilidad y comunicación unificada. Hay modelos de tres y dos niveles como el que se muestra en la figura.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>En proceso de configuración por alumnos de Asix1º</p></figcaption></figure>

Las tres capas fundamentales en este tipo de diseños son:

* Acceso
* Distribución
* Núcleo

Cada capa se debe considerar como un módulo estructurado y bien definido, con sus funciones y roles específicos.

La modularidad en el diseño jerárquico asegura aún más que la red mantenga la resistencia y la flexibilidad que le permitan proporcionar servicios de red, así como admitir el crecimiento y los cambios que puedan ocurrir a lo largo del tiempo.

#### Capa de acceso

La capa de acceso representa el perímetro de la red, por donde entra o sale el tráfico de la red del campus. Tradicionalmente, la función principal de los switches de capa de acceso es proporcionar acceso de red al usuario. Los switches de capa de acceso se conectan a los switches de capa de distribución, que implementan tecnologías de base de red como el routing, la calidad de servicio y la seguridad.

Para satisfacer las demandas de las aplicaciones de red y de los usuarios finales, las plataformas de switching de última generación ahora proporcionan servicios más convergentes, integrados e inteligentes a diversos tipos de terminales en el perímetro de la red. La incorporación de inteligencia en los switches de capa de acceso permite que las aplicaciones funcionen de manera más eficaz y segura en la red.

#### Capa de distribución

La capa de distribución interactúa entre la capa de acceso y la capa de núcleo para proporcionar muchas funciones importantes, incluidas las siguientes:

* Agregar redes de armario de cableado a gran escala.
* Agregar dominios de difusión de capa 2 y límites de routing de capa 3.
* Proporcionar funciones inteligentes de switching, de routing y de política de acceso a la red para acceder al resto de la red.
* Proporcionar una alta disponibilidad al usuario final mediante los switches de capa de distribución redundantes, y rutas de igual costo al núcleo.
* Proporcionar servicios diferenciados a distintas clases de aplicaciones de servicio en el perímetro de la red.

#### Capa de núcleo

Se trata de la parte troncal de la red que conecta las diferentes capas y la que proporciona el aislamiento de fallas y la conectividad de backbone de alta velocidad.

No siempre es necesario mantener capas principales y de distribución separadas, sobre todo en aquellos casos donde no existe red física o que haga falta escalar la red. Se puede implementar un diseño alternativo de dos niveles, conocido como `diseño de red de núcleo contraído`.

<figure><img src="../.gitbook/assets/image (36).png" alt="" width="563"><figcaption><p>Tomado de: <a href="https://redes.umh.es/cisco/CCNA/es/RSE/index.html#4.1.1.5">https://redes.umh.es/cisco/CCNA/es/RSE/index.html#4.1.1.5</a></p></figcaption></figure>

Las redes conmutadas han evolucionado muchísimo en los últimos años. Las redes conmutadas de capa 2 dependían de Ethernet y del uso de repetidores hub para propagar el tráfico de la red a través de una organización. Las redes conmutadas proporcionan flexibilidad y administración de tráfico entre otras:

* Compatibilidad con tecnología de redes
* Conectividad inalámbrica
* Compatibilidad con tecnologías como telefonía IP
* Compatibilidad con los servicios de movilidad
* Calidad de servicio
* Seguridad adicional

### Consideraciones

A la hora de implementar estos tipos de redes tenemos que considerar el uso de diferentes tipos de switches y que sean los adecuados según los requisitos de la red.

Cuando seleccionamos el tipo de switch y tener que diseñar la red debemos considerar entre una configuración **fija** o una **modular**, así como entre un dispositivo **apilable** o **no** **apilable**. También hay que tener en cuenta el tamaño de los switches en unidades de rack.

**Switches de configuración fija** - Este tipo de switches no admiten características u opciones más allá de las que vienen de fábrica. Si hablamos de un switch gigabit de 24 puertos, éste no admitiría puertos adicionales.

**Switches de configuración modular -** Éstos ofrecen más flexibilidad a la hora de configurarlos. Estos switches permiten la instalación de diferentes tipos de módulos según lo que necesitemos, esto es: puertos Ethernet, fibra óptica, uplinks, alimentación PoE, etc. Tampoco están limitados a un determinado número de puertos o funciones. Nos permite expandirlo según vaya creciendo la red.

**Switches de configuración apilable -** Este tipo de switches se pueden interconectar mediante un cable especial que proporciona un rendimiento de ancho de banda alto entre los switches. Por tanto, es posible conectar varios switches entre sí utilizando stacking cables. Al tener conectados los switches, éstos se gestionan como una sola unidad, o sea, tienen una única IP, una misma configuración, una misma consola que permite simplificar la administración y mejorar el rendimiento y redundancia de la red.

{% hint style="info" %}
Nota: La tecnología Cisco StackWise permite la interconexión de hasta nueve switches. Operan como un switch único más grande. Los switches apilables son convenientes cuando la tolerancia a fallas y la disponibilidad de ancho de banda son críticas y resulta costoso implementar un switch modular. Al conectar estos switches, la red se puede recuperar si falla un solo switch. Los switches apilables usan un puerto especial para las interconexiones. Muchos switches apilables de Cisco también admiten la tecnología StackPower, lo que permite compartir la alimentación entre todos.
{% endhint %}
