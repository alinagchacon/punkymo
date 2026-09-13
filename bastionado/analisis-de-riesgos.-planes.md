---
description: Bloque 1
---

# Análisis de riesgos. Planes

cLinks - Recursos didácticos y fuentes oficiales

### Análisis de riesgos y planes de securización

En este primer bloque del módulo debemos sentar las bases estratégicas, metodológicas y organizativas antes de la ejecución técnica del bastionado (_hardening_).&#x20;

Los recursos seleccionados cubren:&#x20;

* las metodologías de análisis de riesgos formales (MAGERIT, ISO 27005) -
* los marcos operativos de bastionado (CCN-STIC, CIS Controls) y
* los estándares modernos de reutilización segura de hardware en el ámbito de la Economía Circular (IEEE 2883-2022).

### Recursos de INCIBE&#x20;

INCIBE ofrece material divulgativo y técnico adaptado tanto a entornos pyme como a la formación técnica especializada:

1. [**Gestión de riesgos: Una guía de aproximación para el empresario**](https://www.incibe.es/empresas/guias/gestion-riesgos-guia-empresario)
   * Este documento  nos permite comprender el ciclo de vida del riesgo, la catalogación de activos (información, procesos, sistemas) y la estimación de impactos (financiero, reputacional, legal).&#x20;
   * Sirve como lectura base.
2. [**Guía de Gestión de Crisis de Ciberseguridad en Empresas**](https://www.incibe.es/empresas/blog/guia-de-gestion-de-crisis-de-ciberseguridad-en-empresas)
   * Este documento desarrolla la estructuración de la respuesta ante incidentes, los niveles de gravedad, canales de escalado y comunicación durante una crisis de seguridad.
   * Sirve como lectura esencial para el desarrollo de los niveles, escalados y protocolos de atención a incidencias.
3. [**Kits de Políticas de Seguridad para la PYME**](https://www.incibe.es/empresas/formacion/kit-concienciacion)&#x20;
   * Se trata de una colección de documentos tipo para la implantación de políticas de contraseñas, control de acceso remoto, uso de dispositivos corporativos e instalación de software autorizado.
   * Este kit permite al alumnado analizar y adaptar políticas reales a un escenario empresarial específico.
4. **Guías temáticas de ciberseguridad en entornos industriales (OT/ICS)**
   * Se trata de manuales orientados a la protección de infraestructuras donde conviven sistemas TI tradicionales con redes OT e Industria 4.0.
   * Sirve de fundamento conceptual para integrar los principios de la Industria 4.0.
   * **Links**
     * [https://enredandoconredes.com/2024/09/14/ciberseguridad-industrial-guias-y-articulos-de-incibe-cert/](https://enredandoconredes.com/2024/09/14/ciberseguridad-industrial-guias-y-articulos-de-incibe-cert/)
     * [https://www.incibe.es/index.php/empresas/blog/mapa-funcional-de-referencias-de-ciberseguridad-para-los-sectores-energetico-y](https://www.incibe.es/index.php/empresas/blog/mapa-funcional-de-referencias-de-ciberseguridad-para-los-sectores-energetico-y)
     * [https://www.ismsforum.es/ficheros/descargas/guia-entornos-industriales-20231686772410.pdf](https://www.ismsforum.es/ficheros/descargas/guia-entornos-industriales-20231686772410.pdf)

### El CCN-CERT y el ENS - Esquema Nacional de Seguridad

El Centro Criptológico Nacional (CCN) publica las Guías de Seguridad de las Tecnologías de la Información y la Comunicación (**Serie CCN-STIC**), imprescindibles para la administración pública y empresas del sector estratégico en España:

1. **Serie CCN-STIC 800 (Esquema Nacional de Seguridad - RD 311/2022)**
   * **CCN-STIC-802:** _Glosario de términos y acrónimos del ENS_.
   * **CCN-STIC-803:** _Guía de valoración de sistemas en el Esquema Nacional de Seguridad_ (Permite clasificar los sistemas en categorías BÁSICA, MEDIA o ALTA evaluando las dimensiones de confidencialidad, integridad, disponibilidad, autenticidad y trazabilidad).
   * **CCN-STIC-804:** _Medidas de seguridad del Esquema Nacional de Seguridad_.
   * Esta serie es una referencia obligatoria para entender cómo se gradúan los controles técnicos en función de los requerimientos normativos.<br>
2.  **Metodología MAGERIT v3 y Herramienta PILAR**

    * Se trata de una &#x6D;_&#x65;todología de Análisis y Gestión de Riesgos de los Sistemas de Información_, desarrollada por el Consejo Superior de Administración Electrónica.
    *   Proporciona la estructura matemática y procedimental para identificar activos, amenazas y salvaguardas:

        $$
        \text{Riesgo Intríseco} = (\text{Valor del Activo} \times \text{Vulnerabilidad}) \times \text{Probabilidad de la Amenaza}
        $$
    * Uso de _PILAR / MicroPILAR_ para simulación de análisis de riesgos.
    * **Link** - [https://www.pilar-tools.com/magerit/index.html](https://www.pilar-tools.com/magerit/index.html)&#x20;

    <br>
3. **Guías CCN-STIC de Perfilado y Bastionado (Serie 500 y 800)**
   * Guías específicas de bastionado de sistemas operativos (Windows Server, distribuciones Linux como RedHat/Debian) y elementos de red (Cisco, Fortinet).

### Estándares y Marcos internacionales - ISO, NIST, CIS

1. **ISO/IEC 27001:2022 e ISO/IEC 27005:2022**
   * **ISO 27001 (Anexo A):** Catálogo de controles de seguridad organizativos, de personas, físicos y tecnológicos.
   * **ISO 27005:** Directrices internacionales para la gestión de riesgos de la seguridad de la información.
   * Se trata de un marcador global de buenas prácticas para auditoría y cumplimiento de normas internacionales.<br>
2. **NIST Cybersecurity Framework (CSF 2.0)**
   * Organizado en 6 funciones principales:&#x20;
     * _Gobernar (GV),_&#x20;
     * _Identificar (ID),_&#x20;
     * _Proteger (PR),_&#x20;
     * _Detectar (DE),_&#x20;
     * _Responder (RS) y_&#x20;
     * _Recuperar (RC)_.
   * Aporta una visión de la ciberseguridad corporativa accesible y ágil para estructurar el plan de medidas técnicas.<br>
3. **CIS Critical Security Controls v8 (Center for Internet Security)**
   * Es una lista de 18 controles de ciberseguridad defensiva, del tipo&#x20;
     * Control 1: Inventario de activos
     * Control 4: Configuración segura de activos de red y SO.
   * Se trata de una guía práctica para justificar los parámetros de bastionado técnicos antes de configurarlos en la consola CLI.
   * Link - [https://www.cisecurity.org/controls/v8](https://www.cisecurity.org/controls/v8)

### EconomíacCircular y Sanitización segura de hardware (Industria 4.0)

La economía circular en el ámbito de las TIC busca maximizar el ciclo de vida útil del hardware (reutilización defensiva) garantizando que no existan fugas de información confidencial:

1. **Estándar IEEE 2883-2022 (**_**Standard for Sanitizing Storage**_**)**
   * Es un estándar técnico internacional actualizado para la desclasificación y sanitización de almacenamiento moderno (SSD, NVMe, HDD, memorias Flash).
   * **Niveles definidos:**
     * **Clear (Limpieza lógica):** Sobreescritura mediante comandos de la interfaz para evitar recuperación no especializada (reutilización interna).
     * **Purge (Purga profunda):** Cifrado criptográfico de claves (_Cryptographic Erase - CE_) o borrado por comandos de firmware para evitar análisis forense de laboratorio (reutilización externa).
     * **Destruct (Destrucción física):** Incineración o trituración para medios con máxima confidencialidad.
2. **NIST SP 800-88 Rev. 1 (**_**Guidelines for Media Sanitization**_**)**
   * Se trata de una guía complementaria para el tratamiento de soportes magnéticos y ópticos antes del reciclaje o reacondicionamiento corporativo.
3. **Casos de Reutilización Defensiva de Hardware:**
   * Transformación de estaciones de trabajo obsoletas retiradas de producción en sensores de detección de intrusiones (**HIDS/NIDS** con Suricata/Wazuh).
   * Reutilización de equipos para routers/firewalls de laboratorio mediante sistemas de código abierto (pfSense / OPNsense).

### 6. Cuadro Resumen de Cobertura de Contenidos

| **Contenido del Currículo**               | **Estándar / Fuente Principal** | **Documento / Herramienta Recomendada**                  |
| ----------------------------------------- | ------------------------------- | -------------------------------------------------------- |
| **1.1 Análisis de riesgos**               | MAGERIT v3 / ISO 27005          | Herramienta PILAR / Guía INCIBE Gestión de Riesgos       |
| **1.2 Economía Circular / Industria 4.0** | IEEE 2883-2022 / NIST SP 800-88 | Especificaciones de sanitización (Clear/Purge)           |
| **1.3 Pla de mesures tècniques**          | NIST CSF 2.0 / CIS Controls v8  | Matriz de Controles Prioritarios CIS                     |
| **1.4 Polítiques de securització**        | ISO/IEC 27001 (Anexo A)         | Kit de Políticas para PYMEs de INCIBE                    |
| **1.5 Guies de bones pràctiques**         | CIS Benchmarks / CCN-STIC       | Guías de bastionado del CCN-CERT                         |
| **1.6 Estàndards de securització**        | ENS (RD 311/2022) / ISO 27001   | Serie CCN-STIC-800                                       |
| **1.7 Procediments e instruccions**       | ITIL v4 / ISO 20000             | Plantillas de PNT (Procedimiento Normalizado de Trabajo) |
| **1.8 Nivells y escalats de incidències** | RFC 2350 / INCIBE-CERT          | Guía de Gestión de Crisis de INCIBE                      |
