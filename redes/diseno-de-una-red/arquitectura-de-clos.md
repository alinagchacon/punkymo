---
description: Diseño de redes
---

# Arquitectura de Clos

**Arquitectura de Clos o leaf-spine** **-** Se trata de un tipo  de diseño escalable y optimizado para centros de datos de alto rendimiento y cargas de trabajo de aprendizaje automático o IA.

Este tipo de arquitectura consta de dos capas principales:

1. switches leaf&#x20;
2. switches spine

**Switches leaf**&#x20;

* se conectan  a los puntos finales como servidores, sistemas de almacenamiento y GPU.&#x20;
* establecen enlaces ascendentes con cada switch spine.&#x20;
* todos los conmutadores leaf son interconectados a través de los conmutadores spine.
* Todo ello permite crear una malla eficiente que ofrece rutas de baja latencia y alto ancho de banda con sobreasignación consistente o determinista.

Este tipo de diseño:&#x20;

* facilita un tráfico eficiente de este a oeste,&#x20;
* admite el enrutamiento de rutas múltiples de igual coste (ECMP)
* permite la escalabilidad horizontal al permitir la integración de conmutadores leaf o spine adicionales sin interrupciones.&#x20;

Es un tipo de arquitectura  fundamental en los CPD modernos, donde el rendimiento constante y la tolerancia a fallos son esenciales, y cada vez se exploran más para su uso en redes de campus con el fin de satisfacer las crecientes necesidades de rendimiento y flexibilidad.

### El diseño de redes para entornos HPC

En los entornos de alto rendimiento - HPC, el diseño de la red se concibe para lograr:&#x20;

* una ultra baja latencia,&#x20;
* un ancho de banda elevado y&#x20;
* una fluctuación mínima

con el fin de admitir cargas de trabajo de computación paralela en miles de nodos.

La arquitectura HPC - a  diferencia de las redes empresariales tradicionales - a menudo dependen de interconexiones especializadas como **InfiniBand** o **Ethernet de alta velocidad** para lograr el alto rendimiento que necesitan para el modelado científico, las simulaciones o el entrenamiento de IA.&#x20;

El diseño prioriza:&#x20;

* topología sin bloqueo&#x20;
* flujo de datos eficiente y
* comunicación determinista&#x20;

que  garantizan un rendimiento constante y la sincronización de tareas en todos los clústeres de computación.<br>

**Links**

* [https://www.supermicro.com/es-es/glossary/ai-workloads](https://www.supermicro.com/es-es/glossary/ai-workloads)
* [https://www.supermicro.com/es-es/glossary/gpu](https://www.supermicro.com/es-es/glossary/gpu)
* [https://www.supermicro.com/es-es/solutions/high-performance-computing](https://www.supermicro.com/es-es/solutions/high-performance-computing)
* [https://www.supermicro.com/es-es/glossary/ultra-low-latency](https://www.supermicro.com/es-es/glossary/ultra-low-latency)
