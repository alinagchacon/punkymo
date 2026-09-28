# Diseño de una red

#### **¿Por qué es importante?**

Un buen diseño de red  y una topología efectiva pueden hacer la diferencia en el funcionamiento de toda una red. El diseño de red:

* Sirve como base para una monitorización efectiva del funcionamiento de las redes.
* Permite la comprensión de cómo encajan los componentes de la red y por ende permite supervisar y mantener la calidad del servicio - QoS
* Con una topología o mapa de red, se pueden identificar los dispositivos defectuosos y dar soluciones efectivas de forma precisa y rápida.

### El diseño de una red y el modelo OSI

Todo diseño de red debe iniciarse con el modelo OSI, teniendo en cuenta específicamente las cuatro primeras capas que son las más importantes:&#x20;

**Capa física**&#x20;

Esta capa hace referencia al hardware y equipamiento que forma parte de la red: todo lo físico que se puede manipular. Esto incluye PC, cables, routers, printers, portátiles, así como las señales inalámbricas y eléctricas.&#x20;

Es  una de las capas más importantes para el proceso de diseño de red, especialmente en grandes entornos a escala empresarial, pues la ubicación del equipo afectará la calidad del servicio.

{% hint style="info" %}
El objetivo de esta capa es garantizar que los dispositivos estén conectados correctamente y que no haya demasiada distancia entre cada uno.&#x20;
{% endhint %}

**Capa de enlace de datos - enlace lógico**

Busca que la transferencia sea fiable dentro del circuito de transmisión de datos. Se trata de una capa intermedia que recibe peticiones de la capa de red y utiliza los servicios ofrecidos por la física.

**Capa de red**

Se encarga de determinar el direccionamiento lógico y la ruta directa de cada comunicación. Por tanto, permite la conexión entre dispositivos que están ubicados en redes diferentes y es gracias a esta capa que se logra la comunicación con otros dispositivos cuando no hay una conexión directa, consiguiendo así la aparición de comunicaciones de alcance mundial.

**Capa de transporte**

Esta capa determina cómo se descompone y se entrega la información en la red. Es quien separa los datos en paquetes y los vuelve a ensamblar teniendo en cuenta la dirección en la que viajan antes de enviarlos a la capa 3 de red.

Por tanto, conocer el modelo OSI ayuda a comprender el funcionamiento de cada capa y haga que el diseño de una red sea funcional y cubra las necesidades de los usuarios.&#x20;

#### Diferentes entornos de red

Un diseño de red bien estructurado puede:&#x20;

En **entornos empresariales**:

* respaldar las aplicaciones críticas del negocio.&#x20;
* aplicar políticas de seguridad
* permitir la gestión centralizada de usuarios, dispositivos y datos a través de un marco de zero trusth.&#x20;

**Ejemplo -** Una empresa multinacional puede necesitar de un diseño híbrido que conecte los sistemas locales con los servicios en la nube, lo que permitiría  una comunicación segura y redundancia entre las diferentes ubicaciones.

En **centros de datos**:&#x20;

* fundamental para garantizar alta disponibilidad, resiliencia y rendimiento óptimo.
* antes de implementar una infraestructura, se debe adecuar el CPD para la instalación del hardware mediante una planificación que abarque tanto la conectividad de red, como la distribución de energía, los sistemas de refrigeración y la disposición física de los racks.&#x20;

Todo este proceso inicial  es esencial para soportar tecnologías avanzadas como la virtualización, la contenerización y las cargas de trabajo intensivas en datos, asegurando que el entorno sea escalable, de alto rendimiento y eficiente desde el principio.

En cuanto a los **proveedores de servicios en la nube:**

* utilizan principios de diseño de redes para construir entornos multiusuario escalables y seguros.&#x20;
* emplean redes virtuales, redes definidas por software - SDN y
* emplean protocolos de enrutamiento dinámico para:&#x20;
  * optimizar el flujo de tráfico y&#x20;
  * aislar las cargas de trabajo.

Cada vez más las empresas adoptan CPD modernos, con lo cual el diseño de redes aún más en el software y la automatización. Integrar herramientas de orquestación, sistemas de monitorización y marcos de seguridad en la infraestructura de red permite:&#x20;

* una configuración coherente y estandarizada,&#x20;
* tener visibilidad en tiempo real,&#x20;
* una resolución proactiva de los problemas,&#x20;

Todos ellos son elementos imprescindibles para mantener la disponibilidad y el rendimiento en entornos dinámicos.

### ¿Cuáles son los desafíos en el diseño de redes?

El diseño de una red robusta y eficiente implica abordar una amplia gama de desafíos técnicos y operativos. En la medida que los entornos se vuelven más complejos, los diseñadores deben adaptarse a los objetivos empresariales y las demandas tecnológicas que están en constante evolución:

* **Adaptación a los requisitos de aplicación en constante evolución**&#x20;
  * las aplicaciones se vuelven más distribuidas y consumen más datos, con lo cual el diseño de red debe admitir patrones de tráfico dinámicos, transmisión de baja latencia y conmutación por error sin interrupciones en caso de fallos de hardware o software.&#x20;
  * Si no se tienen en cuenta estas demandas cambiantes las redes pudieran tener un rendimiento deficiente y limitar el rendimiento de las aplicaciones, así como la experiencia de los usuarios.
* **Integración de seguridad**
  * es fundamental, aunque complejo integrar la seguridad en cada capa de la red.&#x20;
  * se debe tener en cuenta el acceso seguro, el cifrado, la segmentación y los requisitos de cumplimiento sin generar cuellos de botella en el rendimiento.
* **Consideraciones medioambientales y de eficiencia energética**
  * la eficiencia energética se ha convertido en una prioridad fundamental del diseño. en la medida que las redes se van adaptando a soportar cargas de trabajo exigentes, como es el caso de los entrenamientos de la IA.
  * cada vez más se va adoptando hardware de bajo consumo, refrigeración optimizada y enrutamiento con conciencia energética para reducir el impacto ambiental y los costos operativos.
  * Tener en consideración los objetivos de sostenibilidad y requisitos normativos en constante evolución son prácticas de diseño que las empresas necesitan cumplir.
* **Redundancia y tolerancia a fallos**
  * Garantizar la disponibilidad continua de la red requiere una planificación minuciosa de las rutas de conmutación por error, el hardware redundante y el enrutamiento dinámico.&#x20;
  * Dependiendo de la criticidad de las aplicaciones, también puede integrarse un centro de datos secundario en la solución.&#x20;
  * La falta de una redundancia adecuada en el diseño de la red puede provocar tiempos de inactividad críticos durante fallos de hardware o tareas de mantenimiento.
* **Complejidad de entornos multisitio o híbridos**
  * Conectar varias ubicaciones, así como integrar sistemas locales con infraestructura en la nube añade complejidad. Por tanto, se hace más difícil lograr:&#x20;
    * políticas coherentes
    * conectividad fiable
    * gestión centralizada.&#x20;
  * Las soluciones de automatización y orquestación simplifican el diseño, la implementación y las operaciones continuas.



**Links**

* [https://www.supermicro.com/es-es/glossary/cloud-infrastructure](https://www.supermicro.com/es-es/glossary/cloud-infrastructure)
* [https://www.itprc.com/the-ultimate-guide-to-network-design/](https://www.itprc.com/the-ultimate-guide-to-network-design/)
* [https://www.supermicro.com/es-es/glossary/cloud-based-services](https://www.supermicro.com/es-es/glossary/cloud-based-services)
* [https://www.supermicro.com/es-es/glossary/sdn](https://www.supermicro.com/es-es/glossary/sdn)
* [https://www.supermicro.com/es-es/article/ai-training-5-tips-reduce-environmental-impact](https://www.supermicro.com/es-es/article/ai-training-5-tips-reduce-environmental-impact)
* [https://learn-more.supermicro.com/data-center-stories/the-importance-of-green-computing-in-data-centers](https://learn-more.supermicro.com/data-center-stories/the-importance-of-green-computing-in-data-centers)
* [https://www.supermicro.com/es-es/glossary/on-premises](https://www.supermicro.com/es-es/glossary/on-premises)
