# MySQL

MySQL es el sistema de gestión de bases de datos relacional de código abierto y el más extendido que se tiene actualmente. Fue inicialmente desarrollado por MySQL AB, más tarde adquirida por Sun MicroSystems y finalmente comprada por Oracle Corporation, que ya tenía un motor propio InnoDB para MySQL.\


Este sistema de gestión de bases de datos cuenta con una doble licencia:&#x20;

* Por una parte es de código abierto,&#x20;
* Por otra parte, cuenta con una versión comercial gestionada por la compañía Oracle

Entre las características más relevantes de MySQL tenemos:

1. **Código abierto**. muy accesible por lo que una gran cantidad de programadores de desarrollo web lo utilizan.
2. **BD Relacional.** los datos están organizados en tablas relacionadas, lo que facilita la organización y la integridad de los mismos.
3. **Arquitectura Cliente-Servidor**: basa su funcionamiento en el modelo cliente - servidor.&#x20;
4. **Compatibilidad con SQL**: MySQL se basa en SQL, que es un lenguaje de consulta estructurado - Structured Query Language, por lo que es compatible con el lenguaje SQL.
5. **Vistas y procedimientos**: permite configurar vistas personalizadas y almacenar procedimientos que incrementan la eficacia de las implementaciones.
6. **Compatibilidad.** funciona en Windows, Linux y macOS.
7. **Triggers.**  permite automatizar tareas dentro de BD (al producirse un evento otro es lanzado).&#x20;
8. **Transacciones**. Permite trabajar con transacciones, fundamental para todas aquellas aplicaciones que requieren seguridad en la gestión de los datos. Cuando hablamos de transacciones estamos hablando de la actuación de varias operaciones en la BD. El sistema avala que todos los procedimientos se puedan establecer de manera correcta o por el contrario no realiza ninguna de ellas.&#x20;
9. **Rendimiento y escalabilidad.** alta  capacidad para la manipulación de grandes volúmenes de datos, bueno para aplicaciones de alto rendimiento.&#x20;
10. **Comunidad activa.** cuenta con una gran comunidad que desarrolla y mejora continuamente el sistema, gracias a ser de código abierto.

MySQL es muy utilizado en entornos de desarrollo como es el caso de la llamada pila LAMP: Linux, Apache, MySQL, PHP/Python/Perl. Se utiliza en combinación con otros lenguajes como PHP siendo muy popular en los entornos web.



## Instalación

Para instalar MySQL voy a utilizar una VM con Ubuntu Server 20.04.6 (Focal). De hecho, la VM que tengo  como cliente dentro de la VM de Proxmox.

<figure><img src="../.gitbook/assets/image (394).png" alt="" width="563"><figcaption><p>La instalación de MySQL se hará sobre la VM Cliente con IP 10.10.10.2/24</p></figcaption></figure>

