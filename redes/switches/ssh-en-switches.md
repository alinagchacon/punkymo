# SSH en Switches

### Configuración de SSH

Antes de configurar el servicio de SSH, el switch debe tener configurado, como mínimo, un nombre de host único y los parámetros correctos de conectividad de red:

* **Verificar el support SSH.** Usa el comando **show ip ssh** para verificar que el switch sea compatible con SSH. Si el switch no ejecuta un IOS que admita características criptográficas, este comando no se reconoce.

<pre><code><strong>S1#  show ip ssh
</strong></code></pre>

*   **Configure el IP domain.** Configure el nombre de dominio IP de la red utilizando el comando **ip domain-name** _domain-name_ modo de configuración global. En la figura, el valor _domain-name_ es [**cisco.com**](http://cisco.com/).

    <pre><code><strong>S1(config)# ip domain-name cisco.com
    </strong></code></pre>
*   **Genera un par de claves RSA.** No todas las versiones del IOS utilizan la versión 2 de SSH de manera predeterminada, y la versión 1 de SSH tiene fallas de seguridad conocidas. Para configurar SSH versión 2, emita el comando del modo de configuración global **ip ssh version 2**. La creación de un par de claves RSA habilita SSH automáticamente. Use el comando del modo de configuración global **crypto key generate rsa**, para habilitar el servidor SSH en el switch y generar un par de claves RSA. Al crear claves RSA, se solicita al administrador que introduzca una longitud de módulo. La configuración de ejemplo en la figura 1 utiliza un tamaño de módulo de 1024 bits. Una longitud de módulo mayor es más segura, pero se tarda más en generarlo y utilizarlo.<br>

    Nota: Para eliminar el par de claves RSA, use el comando del modo de configuración global **crypto key zeroize rsa**. Después de eliminarse el par de claves RSA, el servidor SSH se deshabilita automáticamente.

    <pre><code><strong>S1(config)# crypto key generate rsa
    </strong><strong>How many bits in the modulus [512]: 1024
    </strong></code></pre>
*   **Configura autenticación de usuarios.** El servidor SSH puede autenticar a los usuarios localmente o con un servidor de autenticación. Para usar el método de autenticación local, cree un par de nombre de usuario y contraseña con el comando **username** _username_ **secret** _password_ modo de configuración global. En el ejemplo, se asignó la contraseña ccna al usuario admin.

    <pre><code><strong>S1(config)# username admin secret ccna
    </strong></code></pre>
*   **Configura las lineas vty.** Habilite el protocolo SSH en las líneas vty utilizando el comando del modo de configuración de línea **transport input ssh**. El switch Catalyst 2960 tiene líneas vty que van de 0 a 15. Esta configuración evita las conexiones que no son SSH (como Telnet) y limita al switch a que acepte solo las conexiones SSH. Use el comando **line vty** del modo de configuración global y luego el comando **login local** del modo de configuración de línea para requerir autenticación local para las conexiones SSH de la base de datos de nombre de usuario local.

    <pre><code><strong>S1(config)# line vty 0 15
    </strong><strong>S1(config-line)# transport input ssh
    </strong><strong>S1(config-line)# login local
    </strong><strong>S1(config-line)# salida
    </strong></code></pre>
*   **Habilita SSH versión 2.** De manera predeterminada, SSH admite las versiones 1 y 2. Al admitir ambas versiones, esto se muestra en la salida **show ip ssh** como compatible con la versión 2. Habilite la versión SSH utilizando el comando de configuración global **ip ssh version 2**.

    <pre><code><strong>S1(config)# ip ssh version 2
    </strong></code></pre>

