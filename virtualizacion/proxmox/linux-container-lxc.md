# Linux container - LXC

Tenemos dos tipos de contenedores: de sistemas y de aplicaciones.&#x20;

* **Contenedores de sistemas** - LXC: una forma de virtualización a nivel de sistema, muy similares a las VM donde podemos instalar servicios, acceder desde SSH, actualizar, etc. Al igual que las VM se pueden ejecutar múltiples sistemas aislados en un único equipo Linux. Una diferencia notable entre los LXC y las VM es que los contenedores no incluyen todo el S.O completo porque comparten el núcleo del S.O del host. Por esta razón son más ligeros y eficientes cuando se habla del uso de los recursos.
* **Contenedores de aplicaciones** - Docker: Se utilizan para el despliegue de aplicaciones dado que permite empaquetar las aplicaciones junto con sus dependencias, configuraciones y bibliotecas necesarias lo que permite ejecutarla aislada en cualquier entorno. Se centran en encapsular solo las aplicaciones y sus dependencias directas. Docker es uno de los ejemplos más conocidos de esta tecnología.

**Algunas características adicionales de los contenedores LXC son**:

1. LXC permite crear, gestionar y supervisar contenedores.&#x20;
2. Es posible crear un contenedor con una distribución Linux específica, así como iniciarlos y detenerlos contenedores y acceder a una consola para gestionarlos.
3. Pueden ser utilizados para aislar aplicaciones, hacer pruebas de software, pruebas entornos de desarrollo o despliegue de microservicios.
4. Brinda un aislamiento adecuado para muchas aplicaciones, aunque para entornos que requieren de mayor seguridad es recomendable  el uso de contenedores como la extensión de LXC llamada LXD o Docker.

#### Algunas características de los contenedores de aplicación son:

1. Garantizan que una aplicación se ejecute de la misma manera en cualquier entorno, ya sea en un entorno de desarrollo local, un servidor de prueba, o en un entorno de producción en la nube.&#x20;
2. Cada contenedor de aplicación se ejecuta en su propio espacio aislado, haciendo que las aplicaciones en diferentes contenedores no pueden interferir entre sí. Esto hace brinda mayor seguridad y estabilidad al facilitar el aislamiento de los fallos y prevenir conflictos de dependencias.
3. Comparten el kernel del S.O del host, haciéndolos más ligeros que las VM tradicionales, reduciendo el uso de recursos y permitiendo un mayor número de contenedores en un solo host.
4. Se pueden iniciar y detener con mucha facilidad, facilitando el despliegue y la escalabilidad de aplicaciones.&#x20;

