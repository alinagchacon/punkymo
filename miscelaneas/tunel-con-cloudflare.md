# Túnel con Cloudflare

Información extraída de: [https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/](https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/)&#x20;

Si queremos usar Cloudflare como servidor DNS primario de modo que Cloudflare tendrá los servidores de nombre authoritative para nuestro dominio.



## Antes de empezar

Ante de poder actualizar los nameservers del dominio tenemos que asegurarnos de:

* Tener comprado un dominio como por ejemplo: www.kirby.com&#x20;
  * Si compras un dominio en Cloudflare, no necesitas hacer nada de lo que viene a continuación pero si no es el caso, entonces sigue leyendo.
* Crea una cuenta en Cloudflare.
* Tener deshabilitado el DNSSEC en el sitio donde hayas registrado (comprado) el dominio.

## Agrega tu sitio a Cloudflare&#x20;

Una vez tengas tu dominio y una cuenta creada en Cloudflare entonces:

* Añadimos nuestro sitio en el panel de control de Cloudflare.

## Los registros DNS

Tenemos que revisar los registros DNS para ver cuando se comienza a utilizar los servidores de nombres de Cloudflare y veamos que la zona esté completamente configurada. Solo entonces, Cloudflare se convertirá en nuestro proveedor de DNS principal y que los registros DNS de Cloudflare apuntan a nuestro dominio.

Podemos hacer uso de dos herramientas que nos brindan información que nos permite  asegurarnos que todo funciona correctamente:

* [https://www.zonemaster.net/en/run-test](https://www.zonemaster.net/en/run-test)&#x20;
* [https://www.whatsmydns.net](https://www.whatsmydns.net)&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Propagación del DNS para el dominio: tallerdekirby.es </p></figcaption></figure>



## Obtener nombres de servidores de nombres

&#x20;Ahora podemos iniciar sesión en el panel de Cloudflare  y:&#x20;

* Seleccionar nuestra cuenta y dominio.&#x20;
* En Descripción general, buscamos los servidores de nombres.&#x20;
* Los reemplazamos con los servidores de nombres de Cloudflare.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>Servidores DNS en Cloudflare</p></figcaption></figure>

## Actualizando en el  registrador de dominio

Nos toca modificar los servidores DNS de nuestro dominio, en el registrador de dominio (en mi caso, OVH). Para ello:

* Iniciamos sesión en la cuenta de administrador de su registrador de dominio.&#x20;
* Eliminamos los servidores de nombres autorizados existentes.
* Agreguamos los servidores de nombres proporcionados por Cloudflare.



## Verificando los cambios

Siempre te dicen que debemos esperar hasta 24 horas en lo que el registrador actualiza y propagan los nuevos servidores de nombres. Lo cierto es que no es necesario esperar tanto tiempo. Una vez que el dominio esté activo recibiremos un correo electrónico de Cloudflare.&#x20;

* Si nos vamos al dashboard de OVH (en mi caso) veremos al cabo de un rato que el dominio tendrá el estado `Activo` en la página.
* Las herramientas en línea como la mostrada anteriormente:  [https://www.whatsmydns.net/](https://www.whatsmydns.net/) nos mostrarán los servidores de nombres asignados por Cloudflare.&#x20;
  * La mayoría de estas herramientas utilizan resultados de consultas en caché, por lo que pueden tardar más en mostrar los servidores de nombres actualizados.
* También podemos utilizar herramientas por línea de comandos que nos mostrarán los servidores DNS asignados por Cloudflare:&#x20;

<pre><code><strong>dig tallerdekirby.es +traza @1.1.1.1
</strong>dig tallerdekirby.es +traza @8.8.8.8
</code></pre>

_O podemos utilizar el comando nslookup:_

```
nslookop tallerdekirby.es 1.1.1.1
nslookop tallerdekirby.es 8.8.8.8
```

## Habilitando DNSSEC

Finalmente, volvemos a habilitar DNSSEC para protegernos contra la suplantación de dominio.

