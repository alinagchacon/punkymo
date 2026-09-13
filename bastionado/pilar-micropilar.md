---
description: Análisis de Riesgos
---

# PILAR / MicroPILAR

### ¿Qué son PILAR y MicroPILAR y qué permiten hacer?

**PILAR** (_Plan de Innovación y Logística de Análisis de Riesgos_) es un conjunto de herramientas software diseñadas para automatizar la aplicación de la metodología **MAGERIT v3** (_Metodología de Análisis y Gestión de Riesgos de los Sistemas de Información_), promovida en España por el **CCN-CERT** (Centro Criptológico Nacional) y el Ministerio de Transformación Digital.<br>

#### Principales funcionalidades:

* **Inventario y modelado de activos:** Permite catalogar todos los elementos de la organización: servidores, redes, bases de datos, aplicaciones, personal, instalaciones físicas, etc..
* **Análisis de dependencias:** Modela cómo unos activos dependen de otros. Por ejemplo, si falla la red o el suministro eléctrico, qué servicios de información se ven afectados.
* **Valoración en dimensiones de seguridad (CIDAT):** Evalúa las dimensiones del **Esquema Nacional de Seguridad (ENS)** e **ISO 27001**:
  * **C**onfidencialidad
  * **I**ntegridad
  * **D**isponibilidad
  * **A**utenticidad
  * **T**razabilidad
* **Evaluación de amenazas y vulnerabilidades:** Asocia eventos dañinos potenciales como desastres, errores humanos, ataques informáticos, fallos técnicos y calcula su probabilidad e impacto.
* **Cálculo del riesgo intrínseco y residual:**&#x41;plica el modelo matemático de MAGERIT:<br>

```actionscript-3
Riesgo = Impacto x Probabilidad = (Valor_Activo x Vulnerabilidad) x Amenaza

```

* **Planes de salvaguardas y controles:** Mapea el estado actual contra estándares de seguridad (**CCN-STIC**, **ISO/IEC 27002**, **CIS Controls**, **NIST SP 800-53**) para calcular el _riesgo residual_ tras la implantación de medidas defensivas.
* **Generación de informes oficiales:** Genera la documentación requerida para procesos de auditoría del ENS y certificación ISO 27001.<br>

### &#x20;Diferencias entre PILAR y MicroPILAR

| **Criterio / Característica**                                  | **PILAR (Versión Completa / Standard)**                                                                   | **MicroPILAR**                                                                                             |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| <p><strong>Audiencia / Ámbito</strong><br></p>                 | <p>Grandes organizaciones, Administraciones Públicas, Auditores e Infraestructuras Críticas.<br></p>      | <p>Pequeñas y Medianas Empresas (PYMEs) o proyectos de escala reducida.<br></p>                            |
| <p><strong>Complejidad y Curva de Aprendizaje</strong><br></p> | <p>Alta. Requiere conocimientos avanzados en MAGERIT y gestión de proyectos GRC.<br></p>                  | <p>Media / Baja. Diseñado con interfaz simplificada y asistentes guiados.<br></p>                          |
| <p><strong>Árbol de Dependencias</strong><br></p>              | <p>Árboles de dependencias complejos multinivel con propagación de impacto y degradación.<br></p>         | <p>Dependencias simplificadas o directas entre activos y servicios.<br></p>                                |
| <p><strong>Personalización</strong><br></p>                    | <p>Permite personalizar totalmente el catálogo de activos, amenazas, salvaguardas y valoraciones.<br></p> | <p>Utiliza catálogos y plantillas predefinidas y cerradas.<br></p>                                         |
| <p><strong>Evaluación del ENS</strong><br></p>                 | <p>Soporta todas las categorías del ENS (<strong>Básica, Media y Alta</strong>).<br></p>                  | <p>Enfocado principalmente en perfiles simplificados y categorías <strong>Básica / Media</strong>.<br></p> |

### ¿Pago o gratuita?

El licenciamiento de PILAR depende directamente de quién lo utilice:

1. **Administración Pública Española e Infraestructuras Críticas:**
   * **Gratuito:** El CCN-CERT proporciona licencias gratuitas a la Administración General del Estado (AGE), comunidades autónomas, entidades locales y operadores de servicios esenciales previa solicitud formal en el portal **CCN-STIC**.<br>
2. **Sector Privado / Empresas Consultoras:**
   * **De pago:** Para empresas privadas, consultorías de ciberseguridad o uso comercial fuera de la Administración Pública, PILAR y MicroPILAR son herramientas comerciales sujetas al pago de licencias (comercializadas por la empresa desarrolladora del software).<br>
3. **Versiones de Evaluación / Educativas:**
   * Existe una versión **PILAR / MicroPILAR Personal / Student / Basic** gratuita descargable con limitaciones funcionales (por ejemplo, restricción en el número de activos que se pueden crear, imposibilidad de exportar informes completos o falta de módulos de auditoría avanzada).

### Alternativas open source&#x20;

Si buscamos herramientas 100% gratuitas y de código abierto para realizar análisis de riesgos en el aula o en la empresa sin depender de licencias, las mejores opciones actuales son:

#### 1. SimpleRisk (Open Source / Free Core)

* **Descripción:** Una de las plataformas de gestión de riesgos (GRC - Governance, Risk, and Compliance) más extendidas internacionalmente.
* **Puntos Fuertes:** Interfaz web moderna, inventario de activos, evaluación cualitativa y cuantitativa, matrices de riesgo (heatmaps) y seguimiento del tratamiento del riesgo. La versión _Core_ es gratuita y de código abierto.

#### 2. ERAMBA (Community Edition)

* **Descripción:** Plataforma GRC muy potente diseñada para gestionar cumplimiento normativo (ISO 27001, GDPR, NIST, PCI-DSS) y análisis de riesgos.
* **Puntos Fuertes:** La versión _Community_ es totalmente gratuita. Incluye mapeo de controles de seguridad, gestión de activos y flujos de trabajo de auditoría.

#### 3. MONARC (_Methodology for an Optimized Risk Analysis_)

* **Descripción:** Herramienta desarrollada por el Gobierno de Luxemburgo y distribuida como software libre.
* **Puntos Fuertes:** Basada en la norma **ISO/IEC 27005**. Permite análisis cualitativo y cuantitativo con una interfaz web multiusuario orientada a la colaboración en equipo.

#### 4. Plantillas y Calculadoras de INCIBE / CCN-CERT (Excel)

* **Descripción:** Para entornos docentes o pequeñas pymes, el INCIBE y el CCN-CERT publican kits de análisis de riesgos basados en hojas de cálculo.
* **Puntos Fuertes:** No requieren instalación de software, permiten entender la matemática subyacente de MAGERIT y son ideales como primer contacto antes de usar una plataforma GRC dedicada.<br>

