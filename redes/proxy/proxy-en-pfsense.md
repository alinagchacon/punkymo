---
description: Configuración de un proxy en pfSense
---

# Proxy en pfSense

En la instalación del servidor de proxy en pfSense tvamos a utilizar: Squid así como SquidGuard y Lightsquid, para aumentar la seguridad en la navegación de los usuarios.

### Squid

Es el servidor proxy open source más popular debido a:

* su gran rendimiento como proxy cache
* a los protocolos que soporta HTTP, HTTPS, GOPHER, FTP, IMAP, etc.,
* a la capacidad de limitar conexiones o ancho de banda,
* la posibilidad de usarse como un proxy transparente
* la posibilidad de utilizarlo como proxy inverso.

### **SquidGuard**

Es un sistema de filtrado que utiliza listas negras. Es una herramienta de filtrado de contenido de código abierto que fue diseñada para trabajar con Squid. Su función principal es filtrar y bloquear el acceso a sitios web basándose en reglas definidas.

Algunas de las características principales de esta herramienta son:

1. **Filtrado de contenido**: permite bloquear el acceso a sitios web basados en diferentes criterios, como direcciones URL, palabras clave en el contenido, tipos de archivo, direcciones IP, etc.
2. **Soporte para listas de control de acceso (ACL)**: Facilita a los administradores de red la posibilidad de definir políticas de acceso y determinar  qué usuarios o grupos pueden acceder a qué contenido.
3. **Integración con Squid**:  Facilita la implementación y configuración en entornos que ya utilizan Squid para funciones de proxy y caché web.
4. **Flexibilidad en la configuración**:  ofrece diversidad de opciones de configuración para adaptarse a los requisitos específicos de filtrado de contenido de cada entorno, permititiendo a los administradores personalizar el tipo de filtrado según sus necesidades.

### **Lightsquid**

Es una aplicación via web que  genera informes muy detallados a partir de los logs de Squid. Proporciona  informes y análisis de registros de acceso para Squid Proxy Server. LightSquid analiza los registros de acceso generados por Squid y los presenta en forma de informes web fáciles de entender.

#### Características de LightSquid

1. **Informes de uso de internet:** Genera informes detallados sobre el uso de Internet, incluye estadísticas sobre el tráfico web, sitios web más visitados, horarios de mayor actividad, etc.
2. **Personalización de informes:** Permite la personalización de los informes según las necesidades como seleccionar el rango de fechas, filtrar por usuarios o grupos, entre otros.
3. **Gráficos y estadísticas:** Presenta los datos de manera visual, utilizando gráficos y estadísticas para facilitar la comprensión y el análisis de los patrones de uso de Internet.

## Los primeros pasos

Lo primero que tenemos que hacer es instalar los paquetes correspondientes. Esto es:

* Lightsquid
* Squid
* squidGuard\


Para hacerlo, nos dirigimos a `System - Package Manager` y seleccionamos los paquetes en cuestión para instalar. Una vez finalizada la instalación de los tres paquetes tendríamos algo como lo siguiente en el apartado de `Installed Packages`:

<figure><img src="../../.gitbook/assets/image (314).png" alt=""><figcaption><p>Paquetes instalados en pfSense</p></figcaption></figure>

Una vez hecho esto, en el menú `Services` vamos a ver tres nuevas opciones: `Squid Proxy Server`, `Squid Reverse Proxy` y `SquidGuard Proxy Filter`.&#x20;

To be continued ...&#x20;
