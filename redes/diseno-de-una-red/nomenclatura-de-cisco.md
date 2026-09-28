---
description: routers, switches y IOS
hidden: true
---

# Nomenclatura de Cisco

### Arquitectura y Selección de Dispositivos (Serie Cisco ISR)

A la hora de diseñar e implementar redes, la selección de hardware y software se considera un aspecto crítico.&#x20;

Los routers <mark style="color:violet;">ISR de Cisco - Router de Servicios Integrados</mark>  - proporcionan la base física para desplegar protocolos de enrutamiento dinámico como RIP, OSPF y BGP. También permiten integrar mecanismos de seguridad como firewalls y sistemas de detección y prevención de intrusiones - IDS/IPS.

<table data-header-hidden data-search="false"><thead><tr><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Serie / Modelo</strong></td><td><strong>Interfaces Base</strong></td><td><strong>Módulos de Expansión</strong></td><td><strong>RAM / Flash (Base)</strong></td><td><strong>Casos de uso</strong> </td></tr><tr><td>Cisco 1841 (2004)</td><td>2x 10/100 (FE)</td><td>2 HWIC, 1 AIM</td><td>256 MB / 64 MB</td><td>Laboratorios de enrutamiento dinámico (OSPF/BGP) en GNS3.</td></tr><tr><td>Cisco 1941 (2009)</td><td>2x Gigabit (GE)</td><td>2 EHWIC, 1 ISM</td><td>512 MB / 256 MB</td><td>Terminación de túneles VPN IPsec y conectividad Wi-Fi segura.</td></tr><tr><td>Cisco 2901 (2009)</td><td>2x GigabitEthernet</td><td>4 EHWIC, 2 DSP, 1 ISM</td><td>512 MB / 256 MB</td><td>Despliegues SD-WAN básicos y calidad de servicio (QoS).</td></tr><tr><td>Cisco 2911 (2009)</td><td>3x GigabitEthernet</td><td>4 EHWIC, 1 SM</td><td>512 MB / 256 MB</td><td>Segmentación de red y diseño de Zona Desmilitarizada (DMZ).</td></tr><tr><td>Cisco 4321 (2014)</td><td>2x GE / SFP</td><td>2 NIM, 1 ISC</td><td>4 GB / 4 GB</td><td>Análisis de tráfico e IDS/IPS basado en firmas (Snort).</td></tr><tr><td>Cisco 4331 (2014)</td><td>3x GE / SFP</td><td>2 NIM, 1 SM, 1 ISC</td><td>4 GB / 4 GB</td><td>Virtualización de funciones de red (NFV) y orquestación.</td></tr></tbody></table>

#### Análisis de dispositivos

#### Plataformas de emulación y perímetro básico

* Cisco 1841:&#x20;
  * Dispositivo modular de primera generación&#x20;
  * ofrece incrustado encriptación basada en hardware&#x20;
  * ofrece una mejora adicional del rendimiento de VPN con un módulo de aceleración de VPN opcional
  * sistema de prevención de intrusiones (IPS)
  * funciones de cortafuegos
  * Se virtualiza habitualmente en GNS3 para enseñar políticas de seguridad y control de acceso.



{% hint style="info" %}
Este modelo 1841 de la serie 1800, centralizó el enrutamiento y sentó las bases para acelerar por hardware túneles VPN básicos. Arquitectura que fue pionera en garantizar la confidencialidad, integridad y disponibilidad (CIA) a nivel perimetral. Sin embargo, su CPU monolítica presentaba un menor endurecimiento de redes frente a ataques de denegación de servicio (DoS/DDoS) sostenidos.
{% endhint %}



* Cisco 1941:&#x20;
  * Soporte de aceleración criptográfica en hardware, esencial para el despliegue de redes privadas virtuales - VPN.&#x20;
  * Permite configurar seguridad en VPN con IPSEC utilizando los modos túnel y transporte, así como protocolos AH y ESP.&#x20;
  * A nivel de WIFI  soporta protocolos de seguridad como WPA, WPA2 y WPA3, junto con autenticación mediante 802.1X.
  * OSPF, IS-IS, BGP, EIGRP, DVMRP, PIM-SM, IGMPv3, GRE, PIM-SSM, enrutamiento IPv4 estático, enrutamiento IPv6 estático.
  * Compatibilidad con MPLS, compatibilidad con Syslog, compatibilidad con IPv6,&#x20;
  * Cola justa ponderada basada en clases (CBWFQ).
  * Detección temprana aleatoria ponderada (WRED).<br>
*   Cisco 2901:&#x20;

    * Plataforma escalable que facilita el hardening de redes  y la gestión de redes complejas.&#x20;
    * Capacidad de procesamiento que permite implementar reglas y filtrado de tráfico a nivel de firewall.
    * **Procesador:** Multinúcleo de alto rendimiento diseñado para satisfacer demandas de WAN de alta velocidad y ejecutar múltiples servicios simultáneos.
    * **Memoria DRAM:** 512 MB por defecto (máximo de 2 GB, DDR2 ECC).
    * **Aceleración de Cifrado:** Hardware integrado para **IPSec y SSL**, mejorando la escalabilidad de VPNs y reduciendo la carga del procesador.
    * Admiten tarjetas HWIC, WIC, VIC y VWIC de generaciones anteriores, ofreciendo **protección de inversión**.
    * Protección de firewall, compatibilidad con VPN, compatibilidad con MPLS, compatibilidad con Syslog, compatibilidad con IPv6,&#x20;
    *   Cola justa ponderada basada en clases (CBWFQ),

        Detección temprana aleatoria ponderada (WRED),&#x20;
    * Agente de administración de servicios web (WSMA), NetFlow



{% hint style="info" %}
Series 1900 y 2900 (ISR G2): La introducción de procesadores multi-núcleo permitió estabilizar protocolos de enrutamiento dinámico complejos como OSPF o BGP. A nivel de seguridad, estas plataformas optimizaron el diseño y segmentación de redes, permitiendo activar sistemas de prevención de intrusos (IPS) sin causar cuellos de botella en la transferencia de datos.<br>
{% endhint %}

#### Segmentación avanzada. SDN - NFV

*   Cisco 2911:&#x20;

    * Cuenta con tres interfaces Gigabit integradas.&#x20;
    * Hardware ideal para la segmentación de redes y creación de VLANs.&#x20;
    * Permite establecer el diseño y segmentación de redes necesarios para separar la LAN, la WAN y la Zona Desmilitarizada (DMZ).&#x20;
    * En esta DMZ se realiza la implementación de servicios expuestos de forma segura.
    * **Firewall** Integrado mediante Cisco IOS.
    * **Protocolos de ruteo -** BGP, EIGRP, OSPF.
    * **Protocolos soportados -** IPv4, IPv6, IS-IS, IGMPv3, PIM SM, SSM, DVMRP, IPSec, GRE, MPLS, L2TPv3, PPP, HDLC, entre otros.
    * **Gestión avanzada -** Soporte para QoS (CBWFQ, WRED), PBR, NBAR, SNMP, NetFlow, Syslog y otros.


* Cisco 4321:&#x20;
  * Diseñado para integrarse en arquitecturas de redes definidas por software (SDN).
  * Centralizando el plano de control y el plano de datos.&#x20;
  * Posee capacidad de cómputo para ejecutar sistemas de prevención de intrusos (IPS) y sistemas de detección de intrusos (IDS) con firmas en tiempo real, como Snort.&#x20;
  * Esto es vital para mitigar ataques de denegación de servicio (DoS/DDoS) y ataques de intermediario (MITM).
* Cisco 4331:&#x20;
  * Router de alto rendimiento
  * Soporta la virtualización de funciones de red (NFV) mediante la separación del hardware y software.&#x20;
  * Permite ejecutar funciones de red virtualizadas (VNF) directamente en la infraestructura de virtualización de funciones de red (NFVI) del equipo.&#x20;
  * Es habitual desplegar firewalls virtuales como FortiGate CLI o contenedores Docker para aplicar mecanismos de seguridad avanzados.







Entre los dispositivos comunes de Cisco Packet Tracer tenemos:

* Routers: 4331, 4321, 1941, 2901, 2911
* Switches: 2960, 2950, 2950T, 3560, 3650

La nomenclatura de las versiones de routers de Cisco se puede entender de la siguiente manera:

* Estructura general del nombre

```
<Serie> - <Submodelo> - <Opciones de hardware>
```

#### Ejemplo

Veamos el caso del router: C4331-SEC/K9

| Parte | Significado                                                                                                                           |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------- |
| C     | Cisco (prefijo común para routers y switches de la marca).                                                                            |
| 4331  | Modelo específico dentro de la serie 4000 de routers ISR (Integrated Services Router). El número mayor indica más potencia/capacidad. |
| -SEC  | Bundle o paquete: en este caso, incluye licencias de seguridad como VPN, firewall, IPS.                                               |
| /K9   | Indica que incluye funciones de criptografía fuerte (requeridas para VPN, IPsec, etc.). K9 = cumple con regulaciones de exportación.  |
|       |                                                                                                                                       |

#### Otros ejemplos de opciones comunes:

* **-AX**: incluye licencias para Application Experience (QoS, App Visibility).
* **-UC**: Unified Communications (VoIP, telefonía).
* **-VSEC**: Voice + Security bundle.
* **-K9**: funciones criptográficas (casi siempre presente si el router tiene VPN).<br>

#### Nomenclatura del sistema operativo IOS / IOS XE

Los routers modernos, como el 4331, utilizan IOS XE, que es una evolución modular de IOS.

#### Ejemplo de versión:

Cisco IOS XE Software, Version 17.3.5a

| Parte  | Significado                                                                          |
| ------ | ------------------------------------------------------------------------------------ |
| 17     | Major release (versión mayor del sistema operativo).                                 |
| .3     | Versión menor (minor release).                                                       |
| .5a    | Versión de mantenimiento o parche.                                                   |
| IOS XE | Indica que es la versión modular y multitarea de IOS (a diferencia del IOS clásico). |

#### &#x20;Ejemplos completos y su interpretación

**C4451-X-AX/K9**

* Router Cisco serie 4000, modelo 4451 con módulo expandible (X)
* Incluye licencias de Application Experience (AX)
* Incluye funcionalidades de criptografía (K9)<br>

**C1111-8P**

* Router Cisco ISR serie 1000
* Modelo 1111 con 8 puertos Ethernet Power over Ethernet (PoE)<br>

**Significado de ISR, ASR, CSR, NCS:**

| Acrónimo | Significado                 | Uso principal                              |
| -------- | --------------------------- | ------------------------------------------ |
| ISR      | Integrated Services Router  | Routers para sucursales / oficinas remotas |
| ASR      | Aggregation Services Router | Routers para grandes redes o ISPs          |
| CSR      | Cloud Services Router       | Routers virtuales para entornos en la nube |
| NCS      | Network Convergence System  | Backbone de operadores (alta capacidad)    |

#### Licencias y paquetes virtuales (por software)

* IP Base: funcionalidades básicas de routing.
* SEC (Security): VPN, firewall, IPS, etc.
* APP / AX: Visibilidad de aplicaciones, QoS, NetFlow.
* UC: VoIP, CallManager Express, etc.\
  \
  Estos se activan con licencias, ya que los routers modernos tienen un sistema “pay-as-you-grow” (pagas solo por lo que necesitas).

<br>

#### Links

*
