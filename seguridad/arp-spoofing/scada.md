# SCADA

Instalación y configuración básica de SCADA en una VM con **Debian 11.**

#### (1) Requisitos e instalación

El proceso de instalación de Scada-LTS en Debian se simplifica mediante el uso de sus paquetes preconfigurados.

1. **Obtención del software:** Debes descargar el archivo `.zip` de la última versión desde el repositorio oficial del proyecto - [https://github.com/SCADA-LTS/linux-installer](https://github.com/SCADA-LTS/linux-installer)&#x20;
2. **Preparación de los archivos:** Una vez descargado, descomprime el contenido en una carpeta local de tu sistema.
3. **Ejecución del instalador:** Accede a la carpeta mediante la terminal y ejecuta el fichero de instalación ubicado en su interior:   `./mysql_start.sh` \
   \
   Este script configurará las dependencias necesarias, incluyendo el entorno de ejecución de Java y el servidor de aplicaciones (usualmente Tomcat).
4. En un punto del script te preguntará por los datos de conexión a la DB: esto es

* Port: 3306
* Username: root
* Password: root
* Root password: root
* A continuación, esperamos la  confirmación de que se la configurado correctamente la base de datos:\
  \~/linux-installer-1.2.0/mysql/server/bin/mysqld: ready for connections. Version: '8.0.x' socket: '/tmp/mysql.sock' port: 3306 MySQL Community Server - GPL.
*   Abrimos la segunda terminal en la misma carpeta y ejecute el script <mark style="color:purple;">./tomcat\_start.sh</mark>. Al igual que con el primer script, deberá proporcionar información, como se muestra a continuación:

    <br>
* Enter port: 8080
* Enter username: tcuser
* Enter password: tcuser
* Enter database port: 3306
* Enter database host: localhost
* Enter database username: root
* Enter database password: root<br>

Después de esto debemos tener el acceso al Scada-LTS via web, a través del navegador y escribiendo:  <mark style="color:purple;">localhost:8080/Scada-LTS.</mark>

#### (2) Interfaz de gestión

Una vez finalizada la instalación y verificada la ejecución del servicio, el sistema SCADA se gestiona a través de una interfaz web.

* **URL de acceso:** Abre el navegador en la máquina Debian e introduce la dirección: `http://localhost:8080/Scada-LTS`.
* **Credenciales por defecto:** Para la primera sesión, utiliza las credenciales estándar:
  * **Usuario:** `admin`.
  * **Contraseña:** `admin`.

#### (3) Configuración de fuentes de datos (data sources)

Para que el SCADA sea funcional, debe comunicarse con los dispositivos de campo (como los PLCs que configuramos en GNS3).

1. **Añadir Fuente:** En el panel superior, selecciona el icono de **Data Sources**.
2. **Protocolo Modbus:** Dado que nuestros PLCs operan habitualmente con **ModbusTCP**, selecciona este tipo de fuente.
3. **Parámetros de red:** Debes indicar la dirección IP del PLC y el puerto que tiene abierto (por defecto el **502**) para establecer la conexión.
4. **Definición de Puntos (Points):** Es necesario registrar qué variables deseas leer del PLC, especificando su nombre, tipo de datos y el _offset_ (la dirección de memoria en el PLC donde reside el dato del sensor).

#### (4) Vistas gráficas (HMI)

El último paso es diseñar la interfaz que utilizará el operador para monitorizar la planta.

* **Diseño Visual:** Accede al apartado de **Graphical Views**.
* **Componentes:** Puedes añadir gráficos de líneas para ver la evolución temporal de los sensores (por ejemplo, temperatura o presión) y componentes visuales que indiquen los valores exactos actualizados en tiempo real.

{% hint style="info" %}
ScadaBR, al igual que muchos sistemas industriales, envía la información en texto plano por defecto si se usa ModbusTCP estándar. Durante vuestras pruebas, verificad que los servicios administrativos no sean visibles desde la red externa (_Outside_) y que solo el **Bastion Host** tenga permisos para gestionar esta infraestructura.
{% endhint %}

