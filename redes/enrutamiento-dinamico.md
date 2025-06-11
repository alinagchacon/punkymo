---
description: Routing & Switching
---

# Enrutamiento dinámico

Los protocolos de enrutamiento dinámico se vienen utilizando en el ámbito de las redes desde los años 80 ... del siglo y milenio pasados (¡suena a muy lejano pero no!). En el año 1988 se lanzó RIP v1. Éste fue uno de los primeros protocolos de enrutamiento.

Las redes evolucionaron rápidamente, se hicieron cada vez más complejas, y aparecieron nuevos protocolos de enrutamiento. RIP v1 tuvo que evolucionar a su versión RIP v2. Pero pronto se quedó obsoleto puesto que aún no se escala a las implementaciones de red de la actualidad.&#x20;

La necesidad de satisfacer redes más grandes, se desarrollaron dos protocolos:&#x20;

* **OSPF**, Open Shortest Path First.
* **IS-IS**, Intermediate System to intermediate System.&#x20;
* **IGRP**, protocolo de gateway interior  y el **EIGRP** que es la versión más avanzada y mejorada de IGRP. Ambos protocolos de Cisco con buena escalabilidad en implementaciones de redes más grandes.

Por otra parte, surgen las internetworks que no es más que la interconexión de redes. Esto es, el uso de enlaces WAN para comunicar distintas LAN distribuidas geográficamente. Para permitir esta interconexión, las redes deben contar con un bridge o un router para enlaces WAN.

Para proporcionar enrutamiento entre las internetworks, se utiliza el llamado protocolo de gateway fronterizo (BGP) entre proveedores de servicios de Internet (ISP). Dicho protocolo BGP también se usa entre los ISP y aquellos clientes privados más grandes.

Con el crecimiento de Internet cada vez más dispositivos se conectaron a la red utilizando todo el espacio de direcciones  IPv4 quedando prácticamente agotado. Esto nos llevó al surgimiento de las direcciones IPv6 que a su vez, obligaron al desarrollo de nuevas versiones de protocolos de enrutamiento.

**¿Qué es un protocolo de enrutamiento?**

Pues un conjunto de procesos, algoritmos y mensajes que sirven para:&#x20;

* intercambiar información de enrutamiento y&#x20;
* completar la tabla de routing con la elección de los mejores caminos que realiza el protocolo.&#x20;

**¿Cuál es su función?**

Pues su función principal es:

* Detectar redes remotas.
* Mantener la información de enrutamiento actualizada.
* Escoger el mejor camino hacia la red de destino.
* Encontrar el mejor camino en caso de que la ruta actual deje de estar disponible.

**¿Cómo están compuestos los protocolos de enrutamiento dinámico?**

* **Estructuras de datos**: generalmente, los protocolos de enrutamiento utilizan tablas o bases de datos para sus operaciones que se almacena en la RAM.
* **Mensajes del protocolo de enrutamiento**: los protocolos de enrutamiento usan diferentes tipos de mensajes para descubrir los routers vecinos, intercambiar información de enrutamiento y realizar otras tareas de descubrimiento de redes.
* **Algoritmo** o lista finita de pasos que se utilizan para llevar a cabo una tarea. Son usados por los protocolos de enrutamiento para facilitar información de enrutamiento y para determinar el mejor camino.

Los protocolos de enrutamiento determinan la mejor ruta para alcanzar cada red y justo esa ruta es la que se ofrece a la tabla de enrutamiento. La ruta se instala en la tabla de enrutamiento en caso de que no exista otra ruta con una distancia administrativa menor.&#x20;

Una de las ventajas de los protocolos de enrutamiento dinámico es que los routers pueden intercambianrinformación de routing cuando se produce un cambio en la topología. Este intercambio permite a los routers obtener información sobre nuevas redes, así como encontrar otras rutas alternativas.
