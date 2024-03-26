---
description: Firewall de políticas basados en zonas
---

# ZPF

### Ventajas

Hay dos modelos de configuración de firewall:

* **Classic** Firewall: Es el modelo de configuración tradicional en el que la política de firewall se aplica en las interfaces.
* **Zone-based Policy Firewall** - **ZPF**: El modelo de configuración en el que las interfaces se asignan a zonas de seguridad, y la política de firewall se aplica al tráfico que se mueve entre las zonas.
* Si se añade una interfaz adicional a la zona privada, los hosts conectados a la nueva interfaz en la zona privada pueden pasar tráfico a todos los hosts de la interfaz existente en la misma zona.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (7).png" alt=""><figcaption><p>Una red simple de tres zonas</p></figcaption></figure>

### Topología Básica de Zonas de Seguridad

El modelo de configuración del firewall usando zonas proporciona estructura y facilidad de uso lo que constituye una de las principales motivaciones para que los profesionales de seguridad de redes migren a este modelo ZPF, dado que:

* El enfoque estructurado es útil para la documentación y la comunicación.&#x20;
* La facilidad de uso hace que las implementaciones de seguridad de la red sean más accesibles para una comunidad más grande de profesionales de seguridad.

Este modelo tiene **ventajas** como:

* No depende de las ACL.
* La postura de seguridad del router es bloquear, a menos que se permita explícitamente.
* Las políticas son fáciles de leer y corregir con Cisco Common Classification Policy Language - C3PL.&#x20;
  * Que es un método estructurado para crear políticas de tráfico basadas en eventos, condiciones y acciones.&#x20;
  * Lo que proporciona escalabilidad porque una política afecta cualquier tráfico dado, en lugar de necesitar varias ACL y acciones de inspección para diferentes tipos de tráfico.
* Las interfaces virtuales y físicas pueden agruparse en zonas.
* Las políticas se aplican al tráfico unidireccional entre zonas.

Al decidir si se debe implementar  el modelo Clásico de Firewall o un ZPF es importante considerar que ambos modelos de configuración pueden habilitarse simultáneamente en un router. Sin embargo, los modelos no se pueden combinar en una sola interfaz.&#x20;

#### Ejemplo

Una interfaz no puede ser configurada simultáneamente como miembro de una zona de seguridad y para la inspección de IP.

### Diseño de ZPF&#x20;

El diseño de ZPF implica varios pasos:

1. **Determinar las zonas**:&#x20;
   1. El administrador se debe centrar en la separación de la red por zonas.&#x20;
   2. Las zonas establecen las fronteras de seguridad de la red.&#x20;
   3. Una zona define una frontera donde el tráfico se somete a las restricciones de las políticas al pasar a otra región de la red.&#x20;
   4. Por ejemplo, la red pública sería una zona y la red interna sería otra zona.
2. **Establecer políticas entre zonas**:&#x20;
   1. Para cada par de zonas de **origen-destino** (por ejemplo, de la red interna a Internet externa), se definen las sesiones que los clientes en las zonas de origen pueden solicitar a los servidores en las zonas de destino.&#x20;
   2. Estas sesiones suelen ser sesiones TCP y UDP, pero también pueden ser sesiones ICMP tales como el eco ICMP.&#x20;
   3. Para el tráfico que no se basa en el concepto de sesiones, el administrador debe definir flujos de tráfico unidireccionales desde el origen hasta el destino y viceversa.&#x20;
   4. Las políticas son unidireccionales&#x20;
   5. Las políticas se definen en función de las zonas de origen y destino conocidas como pares de zonas.
3. **Diseñar la infraestructura física**:&#x20;
   1. Después de identificar las zonas y documentar los requisitos de tráfico entre ellas, el administrador debe diseñar la infraestructura física.&#x20;
   2. El administrador debe tener en cuenta los requisitos de seguridad y disponibilidad al diseñar la infraestructura física.&#x20;
   3. Esto incluye dictar la cantidad de dispositivos entre las zonas más seguras y las menos seguras y determinar los dispositivos redundantes.
4. **Identificar subconjuntos dentro de las zonas y combinar requisitos de tráfico**:&#x20;
   1. Para cada dispositivo de firewall en el diseño, el administrador debe identificar los subconjuntos de zonas que están conectados a sus interfaces y combinar los requisitos de tráfico para esas zonas.&#x20;
   2. Por ejemplo, varias zonas pueden estar asociadas indirectamente a una sola interfaz de firewall, lo que daría lugar a una política entre zonas específica del dispositivo.&#x20;

### Modelos de diseño de firewall

#### Red Interna - Red Pública

Un modelo simple declarando una zona privada y otra pública

<figure><img src="../../../.gitbook/assets/image (2) (5).png" alt=""><figcaption><p>De LAN a Internet: Modelo simple declarando una zona privada y una zona pública</p></figcaption></figure>

#### Red Interna + Servidor público - Red Pública&#x20;

Un modelo donde se declara además de una zona privada y otra pública, un servidor público

<figure><img src="../../../.gitbook/assets/image (10) (4).png" alt=""><figcaption><p>Se declara un servidor público en la red</p></figcaption></figure>

#### Red Interna + Servidor público - Red Pública - con dos firewalls

Otro modelo donde se declaran servidores públicos además de una zona privada y otra pública, pero en esta ocasión utilizando dos firewalls.

<figure><img src="../../../.gitbook/assets/image (6) (3).png" alt=""><figcaption><p>Utilizando dos firewalls</p></figcaption></figure>



#### Firewall complejo

<figure><img src="../../../.gitbook/assets/image (29) (2).png" alt=""><figcaption><p>Un diseño más complejo de firewall</p></figcaption></figure>
