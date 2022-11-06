---
description: Interne
---

# Introducción Redes

Ningún país, organización o individuo es dueño de Internet. No se controla de manera centralizada ni política ni tecnológicamente hablando. Y eso es lo mejor que tiene.

El impacto que tiene Internet en la sociedad va más allá de toda su infraestructura tecnológica de cables y equipamiento, es un fenómeno global de nuestra época que ha trascendido en lo social, lo económico, lo político y lo cultural.

Redes por cables, redes inalámbricas, nubes.

**¿Dónde está la nube?** Está claro que detrás de Internet, detrás de la nube hay una compleja infraestructura de equipos, dispositivos, aplicaciones, y un largo etcétera que controlan las diferentes partes del todo para brindar los servicios que utilizamos.

Sin embargo, mirando el cableado submarino cabría pensar que más que ser “etérea” Internet se mueve por el fondo del mar.

<table><thead><tr><th>En los años 70</th><th>En la actualidad</th><th data-hidden></th></tr></thead><tbody><tr><td><img src="../.gitbook/assets/image (91).png" alt=""></td><td><img src="../.gitbook/assets/image (30).png" alt=""></td><td></td></tr></tbody></table>

#### La columna vertebral de Internet

La "columna vertebral" de Internet está compuesta por muchas redes diferentes. El término sirve para referirse a las grandes redes que se interconectan entre ellas y pueden tener ISP individuales como clientes.

Los proveedores troncales proporcionan instalaciones de conexión en muchas ciudades para sus clientes, y se conectan con otros proveedores troncales en los llamados **IXP** (Internet Exchange Point) o punto neutro, como:

* el CATNIX de Barcelona
* el ESPANIX de Madrid&#x20;
* el GALNIX de Santiago de Compostela.

El más grande de estos IXP en términos de tasa de transferencia y rutas accesibles es el **ChtIX** en Roubaix Valley, Francia.

#### ¿Quiénes conforman la backbone de Internet?

Entidades comerciales, militares, educativas y gubernamentales. Algunas de las compañías que ofrece conectividad troncal son:

* UUnet – de Verizon
* British Telecom
* Telefónica
* AT\&T
* OVH

#### ¿Y qué es un IXP o punto de intercambio de Internet?

También llamado punto neutro, no deja de ser una infraestructura física que permite a los ISP el intercambio de tráfico de Internet entre sus redes.

Los puntos neutros reducen parte del tráfico de un ISP que debe ser entregada a su proveedor de conexión. Esto,

* reduce costos
* aumenta el número de rutas "aprendidas" a través del punto neutro
* mejora la eficiencia de enrutamiento y
* la tolerancia a fallos

Un <mark style="color:blue;">punto neutro</mark> es uno o varios switch o conmutadores a los que se conectan los diferentes <mark style="color:blue;">ISP</mark> que participan en la conexión. Anteriormente se conectaban vía concentradores con enlace de fibra óptica entre los repetidores o anillos <mark style="color:blue;">FDDI</mark>. Finalmente, migraron a los conmutadores Ethernet y FDDI tan pronto estos estuvieron disponibles en 1993 – 1994.

La conmutación <mark style="color:blue;">ATM</mark> se llegó a utilizar muy brevemente en algunos puntos neutros al final de los 90 pero prevaleció Ethernet que representa el 95% de los conmutadores en Internet.

Las velocidades de los puertos Ethernet se encuentran en los puntos neutros actuales, y van desde los  10 Mbit/s en países pequeños, hasta los puertos de 10 Gbit/s en centros como los de Seúl, Nueva York, Londres, Ámsterdam, Palo Alto. Hay puertos con 100 Gbps disponibles en AMS-IX en Ámsterdam y en DE-CIX de Frankfurt.

La técnica y la logística de negocio implicados en el intercambio de tráfico entre los ISP se rige por los acuerdos de interconexión mutua (**peering**). Gracias a estos acuerdos, el tráfico se intercambia sin compensación mayoritariamente.

Si un punto neutro incurre en costos de operación, por lo general éstos son compartidos por todos los participantes. Sin embargo, existen puntos neutros más caros donde se paga una cuota mensual o anual en dependencia de la velocidad del puerto de conexión o el tráfico de datos.

#### Ventajas

La conexión directa a través de estos puntos neutros, generalmente localizados en la propia ciudad de ambas redes, evita que los datos viajen a otras ciudades, incluso continentes para pasar de una red a otra lo que evita la **latencia** y el **costo**.

Otra de las ventajas es la velocidad, sobre todo en aquellas regiones que no tienen gran desarrollo de las conexiones a larga distancia. Los ISP de este tipo de regiones pagan más caro el transporte de datos que los ISP de EUA, Europa o Japón.

#### Intercambio de tráfico en los puntos neutros

Este intercambio de tráfico se realiza a través de los llamados enrutamiento de <mark style="color:blue;">Border Gateway Protocol (BGP)</mark>.

En muchos casos, un ISP tendrá un enlace a otro ISP y puede aceptar una ruta (que normalmente es ignorada) al otro ISP a través del punto neutro; pero si el enlace directo falla, el tráfico se redirige a través del punto neutro, actuando como un enlace de respaldo.

CATNIX y ESPANIX son puntos neutros en España.

### Links

* [https://www.bgp4.as/internet-exchanges/submarine-cable/marea](https://www.bgp4.as/internet-exchanges/submarine-cable/marea)&#x20;
* [https://www.stackscale.com/es/blog/puntos-neutros-internet/](https://www.stackscale.com/es/blog/puntos-neutros-internet/)
