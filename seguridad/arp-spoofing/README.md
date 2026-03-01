---
description: Simulación de ataques
icon: person-digging
---

# ARP Spoofing

En esta práctica trabajaremos una serie de aspectos que nos permite dominar tanto la mecánica de explotación como la arquitectura de defensa necesaria en infraestructuras críticas.&#x20;

Vamos a utilizar [GNS3](../../redes/gns3/gns3.md) para simular ataques de <mark style="color:purple;">ARP Spoofing</mark> y validaremos cómo una i<mark style="color:purple;">nspección dinámica de ARP</mark> (**DAI - Dynamic ARP Inspection**) puede mitigar esta amenaza en tiempo real.

Como profesor especialista en redes y ciberseguridad, procedo a explicarte la arquitectura y relevancia del sistema SCADA, basándome en el material técnico y académico que estamos utilizando.

#### ¿Qué es el sistema SCADA?

El término **SCADA** hace referencia  a las siglas de **Supervisory Control and Data Acquisition** y se trata de un software diseñado para aplicaciones que requieren el control de máquinas,  monitorización de procesos mediante sensores y autómatas programables ([PLC](https://es.wikipedia.org/wiki/Controlador_l%C3%B3gico_programable)).

El sistema **SCADA** se sitúa en el **Nivel de Información**, dentro de la  jerarquía de las redes industriales, siendo éste el nivel superior del sistema de automatización y desde esta capa recolecta grandes volúmenes de datos procedentes del nivel de control facilitando la supervisión global.

#### ¿Cómo funciona?

El funcionamiento de un sistema SCADA se basa en una estructura de comunicación jerárquica:

1. **Adquisición de datos:** El software SCADA actúa como un cliente que establece comunicación con los dispositivos de campo a través de controladores como los **PLCs**.
2. **Protocolos de comunicación:** Para obtener los datos, el SCADA utiliza protocolos industriales específicos. En nuestros laboratorios, el más común es **ModbusTCP**, donde el SCADA lee los registros de memoria del PLC (entradas, salidas y registros de retención) para obtener valores de sensores.
3. **Procesamiento y Visualización:** Una vez recibidos los datos, el sistema permite diseñar **vistas gráficas** (HMI - Human Machine Interface) que muestran gráficos de líneas o componentes visuales con los valores actualizados en tiempo real.
4. **Supervisión y Alerta:** El sistema está configurado para que el operador conozca en todo momento si la infraestructura funciona correctamente y sea notificado de inmediato ante cualquier anomalía o cambio no deseado.

#### Uso

Los sistemas SCADA son piezas fundamentales en lo que denominamos **infraestructuras críticas**. Sus aplicaciones incluyen:

* **Sectores estratégicos:** Gestión de redes de energía eléctrica, centrales nucleares, embalses de agua e infraestructuras sanitarias.
* **Procesos industriales:** Monitorización de variables como temperatura, presión, humedad o intensidad luminosa en fábricas y plantas de tratamiento.
* **Mantenimiento y Resiliencia:** Se utilizan para prevenir fallos catastróficos, como explosiones o interrupciones masivas de servicios básicos, garantizando el buen funcionamiento de las instalaciones.

#### Perspectiva de Ciberseguridad

Para vuestro perfil de especialistas, es crítico notar que el sistema SCADA es un objetivo de alto valor para los atacantes. Históricamente, muchos protocolos industriales como **ModbusTCP** se diseñaron priorizando la funcionalidad sobre la seguridad, lo que permite que un atacante que logre acceso a la red pueda:

* Realizar **sniffing** y leer datos de los sensores en texto plano.
* Ejecutar ataques de **sobrescritura de valores** o **manipulación de paquetes** en tiempo real para engañar al monitor SCADA con datos falsificados.
* Provocar una **denegación de servicio (DoS)** que interrumpa la visibilidad del operador sobre la planta.

Por ello, la tendencia actual en 2025 es integrar estos sistemas bajo arquitecturas de **Confianza Cero (Zero Trust)** y aplicar protocolos de cifrado como **TLS** para asegurar la integridad de la información transmitida.

#### **(1) Escenario en GNS3**

Debemos tener instaladas  las siguientes VM  para desplegar una topología simple en  GNS3:

* **Víctima A**: Un PLC o host con Debian 11 (IP: 192.168.0.1).
* **Víctima B**: El Sistema SCADA con Debian 11 (IP: 192.168.0.4).
* **Atacante**: Una máquina con Kali Linux (IP: 192.168.0.5).
* **Infraestructura**: Un Switch Cisco que soporte DHCP Snooping y DAI.

#### (2) Fase de Ataque: Simulación de ARP Spoofing

El objetivo es engañar a los dos hosts para que asocien la dirección IP del otro con la dirección MAC del atacante, situándonos en medio de la comunicación (Man-in-the-Middle).

**A. Ataque con Ettercap (Método GUI)**:

1. Iniciad Ettercap en Kali Linux.
2. Ejecutad "Scan for hosts" para descubrir las direcciones MAC del PLC y del SCADA.
3. Añadid la IP del PLC a Target 1 y la del SCADA a Target 2.
4. Seleccionad "ARP Poisoning" y marcad la opción para interceptar conexiones remotas.

**B. Ataque con Scapy (Método Scripting):**&#x20;

Para automatizar el proceso, utilizaremos un script de Python que envíe paquetes ARP response falsificados de forma continua:&#x20;

```
from scapy.all import *
# Función simplificada para MitM
# ip1: PLC, ip2: SCADA
arp_mitm("192.168.0.1", "192.168.0.4")
```

**Validación del ataque**&#x20;

Ejecutar <mark style="color:purple;">arp -a</mark> en las víctimas. Debemos observar que ambas direcciones IP ahora tienen la misma dirección MAC, la de la máquina Kali.



#### (**3) Fase de Análisis: Captura de Tráfico (Sniffing)**

Una vez posicionado en el medio, activad el reenvío de IP en Kali (sysctl net.ipv4.ip\_forward=1) para que la comunicación no se corte. Utilizad Wireshark para capturar los paquetes ModbusTCP que viajan entre el PLC y el SCADA; podréis ver los valores de los sensores en texto plano, demostrando la inseguridad intrínseca del protocolo.



#### (4) Fase de mitigación: Configuración de DAI

La Inspección dinámica de ARP (DAI) es una medida de seguridad que valida los paquetes ARP en la red. DAI intercepta todas las peticiones y respuestas ARP en puertos no confiables y las verifica contra una base de datos de confianza.

**Pasos de configuración en el Switch (IOS)**:

1. Habilitar DHCP Snooping: DAI requiere la base de datos de enlaces (binding database) generada por DHCP Snooping para saber qué IP corresponde a qué MAC
2. Configurar DAI: Activad la inspección en la VLAN correspondiente.
3. Definir Puertos de Confianza: El puerto conectado al router o servidores legítimos debe marcarse como "trust". Los puertos de los usuarios (y del atacante) se mantienen como "untrusted" por defecto.

#### (5) Validación de la mitigación en tiempo real

Una vez configurado DAI, intentad lanzar de nuevo el ataque desde Kali Linux:

* Resultado esperado: El switch detectará que el atacante está enviando respuestas ARP con una identidad falsificada que no coincide con la base de datos de DHCP Snooping.
* Evidencia: El switch descartará los paquetes maliciosos y generará un log de error. Las tablas ARP de las víctimas permanecerán intactas con las MACs correctas.
* Monitoreo con btop: Observad que el uso de ancho de banda en la víctima no presenta las curvas anómalas que vimos durante el escaneo previo, ya que el tráfico malicioso ni siquiera llega al host final.

Conclusión del Laboratorio: Esta actividad demuestra que, aunque protocolos como ARP son vulnerables por diseño desde 1982, la implementación de controles inteligentes en la capa de acceso como DAI permite mitigar estas amenazas de forma proactiva, alineándonos con los principios de Zero Trust.

### Instalación y configuración



#### Links

Este compendio de fuentes permite un análisis más profundo tanto de los vectores de ataque emergentes como de las estrategias de mitigación y resiliencia exigidas en el horizonte tecnológico actual.

#### Investigaciones sobre IA, 5G y SDN

* AI-enabled cybersecurity framework for future 5G wireless infrastructures (PMC):[ https://pmc.ncbi.nlm.nih.gov/articles/PMC12920638/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920638/).
* FloRa: Flow Table Low-Rate Overflow Reconnaissance and Detection in SDN (arXiv):[ https://arxiv.org/pdf/2410.19832](https://arxiv.org/pdf/2410.19832) o mediante su versión final en IEEE Transactions:[ https://doi.org/10.1109/TNSM.2024.3446178](https://doi.org/10.1109/TNSM.2024.3446178).
* Securing Software-Defined Networks (SDN) Against Emerging Cyber Threats in 5G and Future Networks (ResearchGate):[ https://www.researchgate.net/publication/389946028\_Securing\_Software-Defined\_Networks\_SDN\_Against\_Emerging\_Cyber\_Threats\_in\_5G\_and\_Future\_Networks\_-A\_Comprehensive\_Review](https://www.researchgate.net/publication/389946028_Securing_Software-Defined_Networks_SDN_Against_Emerging_Cyber_Threats_in_5G_and_Future_Networks_-A_Comprehensive_Review).
* Securing the SDN Data Plane in Emerging Technology Domains (MDPI):[ https://www.mdpi.com/1999-5903/17/11/503](https://www.mdpi.com/1999-5903/17/11/503).

#### Normativa NIST y Arquitectura Zero Trust (ZTA)

* About NIST 800-207 compliance in 2025 (Thoropass):[ https://www.thoropass.com/blog/about-nist-800-207-compliance-in-2025](https://www.thoropass.com/blog/about-nist-800-207-compliance-in-2025).
* Adopting Zero Trust in 2025: A Practical Guide (Seraphic):[ https://seraphicsecurity.com/learn/zero-trust/adopting-zero-trust-in-2025-a-practical-guide/](https://seraphicsecurity.com/learn/zero-trust/adopting-zero-trust-in-2025-a-practical-guide/).
* What is the NIST SP 800-207 cybersecurity framework? (CyberArk):[ https://www.cyberark.com/what-is/nist-sp-800-207-cybersecurity-framework/](https://www.cyberark.com/what-is/nist-sp-800-207-cybersecurity-framework/).
* 5G Network Security Design Principles (NIST Technical Series):[ https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.36E.ipd.pdf](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.36E.ipd.pdf).
* NIST SP 800-207 Compliance Software (ISMS.online):[ https://www.isms.online/nist/nist-sp-800-207/](https://www.isms.online/nist/nist-sp-800-207/).
* What is NIST Compliance? Guide & Checklist (Veza):[ https://veza.com/blog/nist-compliance/](https://veza.com/blog/nist-compliance/).

#### Informes de Amenazas y Educación DDoS

* Ataques DDoS hipervolumétricos | Informe del 2.º trimestre de 2025 (Cloudflare):[ https://blog.cloudflare.com/es-es/ddos-threat-report-for-2025-q2/](https://blog.cloudflare.com/es-es/ddos-threat-report-for-2025-q2/).
* ¿Qué es un ataque de denegación distribuida de servicio (DDoS)? (IBM Think):[ https://www.ibm.com/mx-es/think/topics/ddos](https://www.ibm.com/mx-es/think/topics/ddos).
* ¿Qué es un ataque DDoS? Definición y motivaciones (Azion):[ https://www.azion.com/es/learning/ddos/que-es-un-ataque-ddos/](https://www.azion.com/es/learning/ddos/que-es-un-ataque-ddos/).

#### Vulnerabilidades y Diseño de Red

* CVE-2024-7246: gRPC Information Disclosure Vulnerability (SentinelOne):[ https://www.sentinelone.com/vulnerability-database/cve-2024-7246/](https://www.sentinelone.com/vulnerability-database/cve-2024-7246/).
* Network Security by Design: Key technologies and practices (Damovo):[ https://www.damovo.com/blog/network-security-by-design-best-practices-and-technologies/](https://www.damovo.com/blog/network-security-by-design-best-practices-and-technologies/).
