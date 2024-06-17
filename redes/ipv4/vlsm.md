---
description: Máscaras de subred de longitud variable
---

# VLSM

## Máscaras de subred de longitud variable

En la división de subredes tradicionales se desperdician direcciones dado que se asigna la misma cantidad para cada subred.

Las subredes que necesitan menos direcciones tienen direcciones sin usar y por tanto desperdiciadas, como es el caso de los enlaces WAN que solo necesitan dos direcciones,

Por tanto,

En las subredes tradicionales se asigna el mismo número de direcciones para cada subred. Si todas las subredes tienen los mismos requisitos para la cantidad de hosts, o si la conservación del espacio de direcciones IPv4 no es un problema, estos bloques de direcciones de tamaño fijo serían eficientes. Normalmente, con direcciones IPv4 públicas, ese no es el caso.

¿Qué ocurre? Que aquellas subredes que requieren menos direcciones IP tienen direcciones sin desperdiciadas, sin utilizar.

### Para usar máscaras de longitud variable VSLM necesitamos un protocolo de enrutamiento que lo soporte.

El protocolo de enrutamiento debe trabajar tanto con la dirección de subred como con la máscara de subred.\
\
Los protocolos RIP1 e IGRP son protocolos de enrutamiento que no soportan VSLM ya que sólo aplican dos máscaras:

* una para las direcciones internas y
* otra para las direcciones externas a la red local.

RIP2, EIGRP y OSPF son protocolos de encaminamiento que sí ofrecen soporte de VLSM puesto que pueden utilizar múltiples máscaras de subred ya que, en sus tablas de encaminamiento, además de las direcciones IP, se indican las máscaras de subred que deben aplicarse a cada destino.\
\
El algoritmo de encaminamiento debe aplicar siempre el identificador de red extendido más largo (máscara más restrictiva) que concuerde con la subred destino; es decir, la "ruta mayor proporcionada”.

#### Ejemplo:

Si un router tiene las siguientes entradas en su tabla:

\- 18.0.0.0/8,

\- 18.64.0.0/16 y&#x20;

\- 18.64.159.0/24

y recibe un paquete con la IP destino: 18.64.159.223

deberá, entonces, aplicar la máscara /24 porque concuerda con la “ruta mayor proporcionada”: 18.64.159.0/24 (es decir, con el mayor número de bits).

Pero, si el equipo con dirección IP 18.64.159.223 no está en esa subsubred física, el algoritmo de encaminamiento no llevará nunca el paquete a su destino.\
\
Por tanto, hay que tener mucho cuidado al asignar direcciones IP a los equipos en un esquema VLSM. No se deben incluir equipos en subredes (menos específicas), cuando deberían estar en subsubredes (más específicas).

#### Por ejemplo

Los enlaces WAN solo necesitan dos direcciones.

Por tanto, la máscara de subred de longitud variable (VLSM) o subdivisión de subredes, permite un uso más eficiente de las direcciones.

<figure><img src="../../.gitbook/assets/image (364).png" alt=""><figcaption><p>Tomado de ...</p></figcaption></figure>

Por tanto:

* La VLSM permite dividir un espacio de red en partes desiguales
* La máscara de subred varía según la cantidad de bits que se toman prestados para una subred específica
* La red se divide en subredes y a su vez éstas se vuelven a dividir en subredes
* Proceso que se repite tanto como sea necesario para conseguir redes de diferente tamaño.
* Se recomienda comenzar por la subred más grande y terminar con las más pequeñas.

### Links

1. [https://sites.google.com/site/redeslocalesyglobales/6-arquitecturas-de-redes/6-arquitectura-tcp-ip/7-nivel-de-red/2-escasez-de-direcciones-ip-soluciones/2-vlsm](https://sites.google.com/site/redeslocalesyglobales/6-arquitecturas-de-redes/6-arquitectura-tcp-ip/7-nivel-de-red/2-escasez-de-direcciones-ip-soluciones/2-vlsm)
2. [https://ccnadesdecero.es/vlsm-mascaras-subred-longitud-variable/](https://ccnadesdecero.es/vlsm-mascaras-subred-longitud-variable/)

\
\
