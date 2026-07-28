---
description: Apuntes
---

# Funcionamiento

## Funcionamiento de ZPF

Las políticas identifican las acciones que ZPF realizará en el tráfico de red. Se pueden configurar tres acciones posibles para procesar el tráfico por protocolo, zonas de origen y destino (pares de zonas) y otros criterios.

* Inspecciona - Realiza la inspección de paquetes de Cisco IOS stateful.
* Descart - Esto es análogo a una declaración Denegar en una ACL. Hay disponible una opción log para registrar los paquetes rechazados.
* Pasa - Esto es análogo a una declaración permitir en una ACL. La acción Pasa no realiza un seguimiento del estado de las conexiones o sesiones dentro del tráfico.

## Reglas para el tráfico

Las reglas dependen de si las interfaces de entrada y salida son miembros de la misma zona:

* Si ninguna de las interfaces es miembro de la zona, la acción resultante es pasar el tráfico.
* Si ambas interfaces son miembros de la misma zona, la acción resultante es pasar el tráfico.
* Si una interfaz es miembro de una zona, pero la otra no, la acción resultante es eliminar el tráfico, independientemente de si existe un par de zonas.
* Si ambas interfaces pertenecen al mismo par de zonas y existe una política, la acción resultante es inspeccionar, permitir o descartar según lo definido por la política.

La siguiente  tabla resume estas reglas.

| ¿Interfaz de origen miembro de la zona? | ¿Interfaz de destino miembro de la zona? | ¿Existe un par de zonas? | ¿Existe una política? | Resultado   |
| --------------------------------------- | ---------------------------------------- | ------------------------ | --------------------- | ----------- |
| NO                                      | NO                                       | NO Disponible            | No disponible         | Pasa        |
| SI                                      | NO                                       | NO DISPONIBLE            | NO DISPONIBLE         | DESCARTA    |
| NO                                      | SI                                       | NO DISPONIBLE            | NO DISPONIBLE         | DESCARTA    |
| SÍ (privado)                            | SÍ (privado)                             | NO DISPONIBLE            | NO DISPONIBLE         | PASA        |
| SÍ (privado)                            | SÍ (público)                             | NO                       | NO DISPONIBLE         | DESCARTA    |
| SÍ (privado)                            | SÍ (público)                             | SÍ                       | NO                    | PASA        |
| SÍ (privado)                            | SÍ (público)                             | SÍ                       | SÍ                    | INSPECCIONA |



## Reglas para el tráfico a la Self Zone  (en el router)

Se denomina <mark style="color:blue;">`self zone`</mark> al propio router e incluye todas las direcciones IP asignadas a las interfaces del mismo.&#x20;



* Este es el tráfico que se origina en el router o se dirige a una interfaz de router. Específicamente, el tráfico es para la administración de dispositivos como por ejemplo SSH, o para el control de reenvío de tráfico, como el tráfico del protocolo de routing.&#x20;
* Las reglas para un ZPF son diferentes para la self zone.&#x20;
* Las reglas dependen de si el router es el origen o el destino del tráfico, como se muestra en la tabla.&#x20;
* Si el router es el origen o el destino, entonces se permite todo el tráfico.&#x20;
* La única excepción es si el origen y el destino son un par de zonas con una política de servicio específica. En ese caso, la política se aplica a todo el tráfico.

| ¿Interfaz de origen miembro de la zona? | ¿Interfaz de destino miembro de la zona? | ¿Existe un par de zonas? | ¿Existe una política? | Resultado   |
| --------------------------------------- | ---------------------------------------- | ------------------------ | --------------------- | ----------- |
| SÍ (self zone)                          | SÍ                                       | NO                       | NO DISPONIBLE         | PASA        |
| SÍ (self zone)                          | SÍ                                       | SÍ                       | NO                    | PASA        |
| SÍ (self zone)                          | SÍ                                       | SÍ                       | SÍ                    | INSPECCIONA |
| SÍ                                      | SÍ (self zone)                           | NO                       | NO DISPONIBLE         | PASA        |
| SÍ                                      | SÍ (self zone)                           | SÍ                       | NO                    | PASA        |
| SÍ                                      | SÍ (self zone)                           | SÍ                       | SÍ                    | INSPECCIONA |

## Consideraciones sobre la configuración de un ZPF

Al configurar un ZPF con la CLI (en Cisco Packet Tracer), hay varios detalles a considerar:

* El router nunca filtra el tráfico entre las interfaces en la misma zona.
* Una interfaz no puede pertenecer a varias zonas. Para crear una unión de zonas de seguridad, hay que especificar una nueva zona y un mapa de políticas y pares de zonas adecuados.
* **ZPF** puede coexistir con un **firewall clásico**, aunque no se pueden utilizar en la misma interfaz. Elimine el comando de configuración de la interfaz `ip inspect` antes de aplicar el comando de seguridad `zone-member`.
* El tráfico nunca puede fluir entre una interfaz asignada a una zona y una interfaz que no ha sido asignada a una zona. La aplicación del comando de configuración `zone-member` siempre da como resultado una interrupción temporal del servicio hasta que el otro miembro de zona esté configurado.
* La política predeterminada entre zonas es descartar todo el tráfico, a menos que la política de servicio configurada específicamente para el par de zonas lo permita.
* El comando `zone-member` no protege el router en sí (el tráfico hacia y desde el router no se ve afectado) a menos que los pares de zona se configuren con la zona automática predefinida.

### &#x20;En resumen

Las ZPF utilizan políticas definidas por el usuario para actuar sobre el tráfico específico que viaja desde una zona de origen a una zona de destino. Se pueden especificar tres acciones:

* **Inspecciona** - El ZPF realiza una inspección de paquetes con estado.
* **Descarta** - El tráfico no está autorizado a viajar al destino. Los paquetes rechazados se pueden registrar.
* **Pasa** - El tráfico está autorizado a viajar a la zona de destino. Esto no realiza un seguimiento del estado de las conexiones o sesiones.

Las reglas predeterminadas se aplican al tráfico en tránsito según la configuración de las interfaces de entrada y salida y la existencia de políticas.&#x20;

#### Ejemplo&#x20;

* Si ninguna de las interfaces de entrada o salida está definida como miembro de una zona, se permite que el tráfico salga por la interfaz de salida.&#x20;
* De manera similar, si ambas interfaces son miembros de la misma zona, se permite el paso del tráfico.&#x20;
* Sin embargo, si una de las interfaces es miembro de una zona y la otra no, el tráfico se descartará.&#x20;

#### Self-zone

* Es una zona especial.&#x20;
* Es el propio router.&#x20;
* Las interfaces del router funcionan como el origen o el destino del tráfico.&#x20;
* El tráfico de esta zona es para la administración del dispositivo o para el control de reenvío de tráfico.&#x20;
* Existen reglas sobre cómo se manejará el tráfico en la self zone.

#### Configuración de un ZPF

Hay cinco pasos:

1. Se crean las zonas.
2. Se crean uno o más mapas de clase para especificar el tráfico que debe asociarse con una política.
3. Se crean políticas que asocian el tráfico del mapa de clases con las acciones de aprobación, descarte o inspección.
4. Se crean pares de zonas que se asociarán con los mapas de políticas.
5. Se asocian las interfaces con zonas.&#x20;

Y finalmente, la política de ZPF está activa.

