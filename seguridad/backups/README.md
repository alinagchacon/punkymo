# Backups

Una copia de seguridad o backup no deja de ser un duplicación de los datos sensibles que puede ser utilizada para restaurar los datos originales en caso de pérdida, corrupción o daño. Se trata de una práctica fundamental en la gestión de la información y la seguridad de datos, y cuyo objetivo es garantizar la disponibilidad y la integridad de la información en caso de situaciones adversas.

**¿Por qué es importante hacer copias de seguridad?**&#x20;

1. Ayudan a prevenir la pérdida de datos causada por errores humanos, fallas de hardware, ataques de malware, desastres naturales, entre otros.
2. En caso de pérdidas o que los datos se vean comprometidos, un backup permite restaurar la información a un estado anterior, recuperando así la funcionalidad y la continuidad del negocio.
3. **S**on esenciales para contrarrestar ataques de ransomware, dado que proporcionan una forma de recuperar datos sin tener que pagar rescates a los atacantes.
4. Dado que se realizan copias de seguridad de manera periódica, se puede mantener un historial de versiones de los datos, y esto facilita la recuperación de versiones específicas.
5. Existen regulaciones y normativas que requieren que las organizaciones mantengan copias de seguridad como parte de las prácticas de gestión de datos y seguridad.

#### Datos sensibles

Por otra parte, cómo sabemos cuales son los datos sensibles que debemos salvaguardar? ¿Cómo se van a organizar esos datos? ¿Cuál es la manera más eficiente de recuperarlos? ¿Qué tipo de soporte es el más adecuado para almacenar las copias de seguridad?

Los criterios para seleccionarlos son muy variados:

1. Todos los archivos
2. Solo aquellos que han sido creados o modificados a partir de una fecha concreta
3. Los de un mismo tipo
4. Los que superen un tamaño determinado
5. Los creados por determinado usuario
6. Solo los archivos de sistemaSolo los archivos de determinadas aplicaciones<br>

Podemos seleccionar los datos a salvaguardar según diversos criterios:

**Confidencialidad**

* accesible únicamente por la dirección de la organización
* accesible por el personal de la empresa
* pública<br>

**Funcionalidad**

* Información de clientes / proveedores
* Información de ventas
* Información de personal y gestión interna<br>

**Impacto**

* Daño de imagen
* Consecuencias legales
* Consecuencias económicas
* Paralización de la actividad empresarial<br>

#### Soporte de copias de seguridad

Existe un gran variedad de soportes que ofrecen gran capacidad de almacenamiento y garantía de seguridad:

* Cintas magnéticas
* Discos ópticos
* HDD y SDD
* Raid
* Servidores NAS
* SAN
* Cloud



Veamos algunas de las características de estos dispositivos:

&#x20;**Discos Duros - HDD y SSD**

* **Ventajas**:&#x20;
  * Son el estándar actual para copias de seguridad locales y operativas. Por ejemplo: imágenes de respaldo completas, diferenciales o incrementales mediante `rsync` o herramientas de imagen.&#x20;
  * Permiten ventanas de copia muy rápidas y restauraciones ágiles de servicios enteros.
* **Desventajas**:&#x20;
  * Los HDD contienen partes mecánicas propensas a fallos por caídas o vibraciones.&#x20;
  * Los SSD:&#x20;
    * tienen un alto costo por gigabyte
    * pueden tener pérdida de datos si se almacenan desconectados por largos periodos
    * tendencia a fallar sin advertencia previa

**2. Soportes Ópticos - CD, DVD, Blu-ray**

* **CD-R/RW (700 MB)**: Hoy en día obsoleto para copias de seguridad empresariales o de sistemas, debido a su capacidad marginal. Útil únicamente para almacenar claves criptográficas, certificados o pequeños scripts de rescate.
* **DVD-R/RW (4.7 GB – 8.5 GB)**: Queda relegado a pequeñas configuraciones puntuales o ISO de arranque personalizado. Incapaz de albergar volúmenes de datos modernos.
* **Ventajas del soporte óptico**:
  1. Al extraer el disco, queda totalmente fuera del alcance de un ciberataque remoto.
  2. Un disco grabado en formato `-R` no permite sobreescritura, lo que garantiza la integridad frente a modificaciones no autorizadas o borrados accidentales.
  3. Existen variantes de DVD y Blu-ray con capas fotosensibles inorgánicas capaces de conservar la información durante décadas sin degradación magnética.
* **Desventajas**: Bajas velocidades de lectura/escritura y capacidad muy reducida comparada con los tamaños de almacenamiento de servidores o centros de procesamiento de datos modernos.



#### Respaldo 3 2 1

En la gestión profesional de infraestructuras se aplica la regla de respaldo 3-2-1. Seguir esta regla  garantiza la continuidad del negocio y la recuperación operativa ante incidentes catastróficos como pueden ser el ataque de un _ransomware_ o desastres ambientales.

Para ello,&#x20;

1. se deben mantener 3 copias de los datos, estoe s: se mantiene la versión de producción activa más dos copias de seguridad de respaldo.
2. Se guardan las copias de seguridad en 2 tipos de soportes diferentes. Por ejemplo: HDD/SSD en servidor NAS y Cinta/Óptico o Nube.&#x20;
3. Se mantiene una 1 copia fuera de la ubicación física: _Offsite_ / Cloud lo que posibilita la  protección de la información ante catástrofes ambientales (incendios, inundaciones) o robos.

Por tanto,

Para operaciones diarias, o sea, una rápida recuperación de desastres, los discos duros mecánicos o cabinas de almacenamiento NAS/SAN resultan indispensables.&#x20;

Los discos ópticos (Blu-ray M-DISC) o las cintas magnéticas (LTO) se reservan para preservar datos críticos e inmutables que deban cumplir con la legislación de conservación de datos o políticas de archivo a largo plazo sin riesgo de degradación magnética o ciberataques.

#### Estrategias

Existen diversas estrategias y métodos para realizar las copias, y éstas van desde una copia completa hasta las llamadas copias incrementales y diferenciales.&#x20;

Cada tipo de copia ofrece sus propias ventajas y desventajas. La elección de la estrategia adecuada depende de factores como pueden ser:

* la cantidad de datos
* la frecuencia de cambio
* los objetivos de recuperación





#### RAID

Cuando hablamos de almacenamiento informático, tenemos que pensar en los RAID de tipo estándar. Los RAID son un conjunto  de configuraciones  - matriz redundante de discos independientes o matriz redundante de discos económicos - que emplean técnicas de  duplicación o paridad para crear  "almacenes" de datos confiables a partir de múltiples unidades de disco duro (HDD).&#x20;

Los tipos más comunes de RAID son:&#x20;

* RAID 0 &#x20;
* RAID 1  o espejo&#x20;
* RAID 0+1 o RAID 01
* RAID 1+0 o RAID 10
* RAID 5 (paridad distribuida) entre otros&#x20;

Nota: Los valores numéricos sólo sirven como identificadores y no significan rendimiento, confiabilidad, generación, jerarquía ni ninguna otra métrica.

La mayoría de los niveles RAID pueden brindar una buena protección y recuperación contra defectos de HW o sectores defectuosos de lectura de los discos, lógicamente, no brindan  protección contra la pérdida de datos por fallas  producidas por  incendio, agua, errores de los usuarios, mal funcionamiento del software o infección de malware, etc.&#x20;

Para datos valiosos, el sistema de RAID es solo un componente básico de un esquema más amplio de prevención y recuperación de pérdida de datos – no  reemplaza un plan de respaldo.

**RAID 0  - División de Datos / Striping**

* **Mecanismo:**&#x20;
  * Distribuye los bloques de datos de modo alterno entre dos o más discos sin almacenar información de paridad ni redundancia.
  * Mejora el rendimiento de las unidades de disco al dividir los datos en fragmentos y distribuirlos en los mismos.
  * No era uno de los niveles RAID originales ya que no es redundante.
  * Permite acceso rápido a los datos, ya que múltiples discos pueden leer y escribir datos de forma paralela.
  * No ofrece redundancia con lo cual, si un disco falla, se pierden todos los datos almacenados en el conjunto RAID.
  * Habitualmente se utiliza para brindar alto rendimiento de escritura: los datos se escriben en 2 o más discos de forma paralela, por lo que un mismo fichero solo está presente una vez en el conjunto.
  * También puede utilizarse como forma de crear un pequeño número de grandes discos virtuales a partir de un gran número de pequeños discos físicos.
  * Un RAID 0 puede ser creado con discos de diferentes tamaños, pero el espacio de\
    almacenamiento añadido al conjunto estará limitado por el tamaño del disco más pequeño.
* **Cantidad mínima de discos:** 2 discos.
* **Ventajas:** Rápido rendimiento en lectura y escritura paralelo. Aprovechamiento del 100% de la capacidad de almacenamiento instalada.
* **Desventajas:**&#x20;
  * Cero tolerancia a fallos. Si un solo disco del conjunto sufre una avería, se pierden la totalidad de los datos irrecuperablemente.
* **Uso:**&#x20;
  * Edición de vídeo en bruto
  * Renderizado 3D
  * Bases de datos temporales



<figure><img src="../../.gitbook/assets/image (953).png" alt="" width="165"><figcaption><p>Tomado de la wikipedia: <a href="https://en.wikipedia.org/wiki/Standard_RAID_levels">https://en.wikipedia.org/wiki/Standard_RAID_levels</a></p></figcaption></figure>

**RAID 1 (Espejo / Mirroring)**

* **Mecanismo:**&#x20;
  * Replica en tiempo real una copia exacta de los datos entre dos o más unidades.
  * Crea una copia exacta o espejo de un conjunto de datos en 2 o más discos.
  * Proporciona redundancia de datos y protección contra fallos de disco. Si un disco falla, el sistema continuará funcionando sin problemas utilizando el disco espejado.
  * No ofrece un aumento significativo en el rendimiento de lectura, pero puede mejorarlo, ya que los datos se pueden escribir en paralelo en ambos discos. No obstante, el rendimiento no es tan alto como RAID 0.
  * Solo puede ser tan grande como el más pequeño de sus discos.
  * No requiere cálculos de paridad. Es adecuado para sistemas que necesitan una protección sencilla contra fallos de disco.
  * Se usa mucho en entornos empresariales y servidores donde la disponibilidad de datos es crítica:
    * bases de datos,
    * servidores de correo electrónico,
    * sistemas de almacenamiento de archivos
    * aplicaciones que no pueden tolerar la pérdida de datos
* **Ventajas:** Alta tolerancia a fallos. Si un disco falla, la operación continúa sin interrupción desde la unidad espejo.
* **Desventajas:** Coste elevado en relación a la capacidad neta disponible.
* **Cantidad mínima de discos:** 2 discos.
* **Uso:** Volúmenes de sistema operativo en servidores de producción, bases de datos transaccionales de misión crítica.



<figure><img src="../../.gitbook/assets/image (954).png" alt="" width="165"><figcaption></figcaption></figure>



**RAID 10 (División de Espejos / RAID 1+0)**

* **Mecanismo:** Anida subconjuntos en RAID 1 (espejo) y sobre ellos aplica una capa superior de distribución RAID 0 (división)
* **Ventajas:** Excelente rendimiento de lectura/escritura combined con alta resiliencia. Soporta el fallo simultáneo de múltiples discos siempre que no pertenezcan al mismo subconjunto en espejo.
* **Desventajas:** Alto coste de implementación
* **Cantidad mínima de discos:** 4 discos
* **Uso:** Servidores de correo empresarial, bases de datos corporativas con alta carga I/O de lectura/escritura.



**RAID 01 (Espejo de Divisiones / RAID 0+1)**

* **Mecanismo:** Crea dos subconjuntos estructurados internamente en RAID 0 y los replica entre sí configurando un RAID 1 superior
* **Ventajas:** Buen rendimiento de velocidad en entornos con baja tasa de fallos de hardware
* **Desventajas:** Menor tolerancia operativa frente a fallos secundarios durante la reconstrucción en comparación con RAID 10. Si falla un solo disco de un subgrupo, todo ese lado del grupo RAID 0 queda fuera de servicio
* **Cantidad mínima de discos:** 4 discos
* **Uso:** Desaconsejado en producción moderna en favor del despliegue de RAID 10.



**RAID 5 (Paridad Distribuida)**

* **Mecanismo:** Fragmenta los bloques de datos intercalando un bloque de paridad calculada mediante operaciones lógicas XOR, distribuido de forma homogénea entre todas las unidades físicas
* **Ventajas:** Excelente relación entre coste, almacenamiento efectivo y redundancia de datos
* **Desventajas:** Penalización de rendimiento en operaciones de escritura debido al cálculo constante del bloque de paridad. Durante la reconstrucción tras la sustitución de un disco averiado, la matriz entra en alta sobrecarga de lectura/escritura, incrementando la probabilidad de un segundo fallo catastrófico en discos de gran capacidad
* **Cantidad mínima de discos:** 3 discos.
* **Uso:** Servidores de archivos empresariales, almacenamiento secundario de copias de seguridad, sistemas NAS/SAN corporativos



<figure><img src="../../.gitbook/assets/image (952).png" alt="" width="165"><figcaption><p>Tomado de la wikipedia: <a href="https://en.wikipedia.org/wiki/Standard_RAID_levels">https://en.wikipedia.org/wiki/Standard_RAID_levels</a> </p></figcaption></figure>





