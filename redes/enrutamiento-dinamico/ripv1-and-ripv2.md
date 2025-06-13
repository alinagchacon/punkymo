# RIPv1 & RIPv2

Este es un protocolo poco utilizado en la actualidad, sin embargo lo enseño en clases por lo cómodo de implementar a la hora de comunicar diferentes redes entre sí y mostrar cómo una red puede alcanzar otra red remota con la que no tenga una comunicación directa. Por tanto, es útil para comprender el routing de red básico.&#x20;

En esta situación, todos los routers se configuraron con funciones de administración básicas, y todas las interfaces identificadas en la topología de referencia están configuradas y habilitadas. No hay rutas estáticas configuradas ni protocolos de routing habilitados, por lo que el acceso remoto de red es imposible en ese momento. RIPv1 se utiliza como protocolo de routing dinámico. Para habilitar RIP, utilice el comando router rip, como se muestra en la figura 3. Este comando no inicia en forma directa el proceso del RIP. En cambio, proporciona acceso al modo de configuración del router, donde se configuran los parámetros de routing RIP. Al habilitar RIP, la versión predeterminada es RIPv1.

Para deshabilitar y eliminar RIP, utilice el comando de configuración global no router rip. Este comando detiene el proceso RIP y elimina todas las configuraciones RIP existentes.

Existen dos versiones de este protocolo: RIPv1 y RIPv2

En una topología como la siguiente donde no queremos configurar rutas estáticas, para poder alcanzar a las redes remotas, por ejemplo desde el PC desktop-1 alcanzar el PC Alpine tenemos que implementar un protocolo dinámico como RIP. &#x20;

<figure><img src="../../.gitbook/assets/image (435).png" alt=""><figcaption></figcaption></figure>

Para implementar RIP tendremos que hacer lo siguiente:

```
R1# config t
R1(config)# router rip
R1(config)# ?
```

<figure><img src="../../.gitbook/assets/image (437).png" alt=""><figcaption><p>Opciones de RIP</p></figcaption></figure>

Para habilitar el  RIP en una red, tenemos que la opción **network&#x20;**_**IP-de-red.** Este comando_ lo que hace es:

* Habilitar el RIP en todas las interfaces que pertenecen a una red específica.&#x20;
* Publicar la red especificada en las actualizaciones de enrutamiento RIP enviadas a otros routers cada 30 segundos.

{% hint style="info" %}
Nota: Cuando introducimos una dirección de subred, el IOS la convierte automáticamente a la dirección de red con clase. Por ejemplo, si escribimos: &#x20;

network 192.168.1.10

automáticamente se convierte en:

network 192.168.1.0&#x20;

en el archivo de configuración en ejecución. El IOS corrige la entrada e introduce la dirección de red con clase.
{% endhint %}

## Comprobaciones del RIP

Para verificar la configuración podemos hacer:&#x20;

```
show ip protocols
```

Y se nos mostrará algo como la imagen siguiente:

<figure><img src="../../.gitbook/assets/image (438).png" alt="" width="563"><figcaption><p>Comando show ip protocols</p></figcaption></figure>

1\. El protocolo RIP está configurado y en ejecución en el router R1.

2\. Los valores de diversos temporizadores; por ejemplo, el R1 envía la siguiente actualización de routing en 16 segundos.

3\. La versión de RIP configurada actualmente es RIPv1.

4\. El R1 realiza la sumarización en el límite de la red con clase.

5\. El R1 anuncia las redes que el R1 incluye en sus actualizaciones RIP.

6\. Los vecinos de RIP se indican mediante:&#x20;

* la dirección IP del siguiente salto,&#x20;
* la AD asociada que el R2 utiliza para las actualizaciones enviadas por ese vecino y&#x20;
* el momento en que dicho vecino recibió la última actualización.

{% hint style="info" %}
Nota: Es un comando útil para verificar las configuraciones y estado de otros protocolos de enrutamiento como es el caso de  EIGRP y OSPF.
{% endhint %}

Podemos utilizar también el comando siguiente para mostrar las rutas RIP instaladas en la tabla de enrutamiento.

```
show ip route 
```

<figure><img src="../../.gitbook/assets/image (439).png" alt="" width="563"><figcaption><p>comando show ip route</p></figcaption></figure>

Cuando ejecutamos RIP en un router, de manera predeterminada se ejecuta la versión 1, aunque  el router es capaz de interpretar mensajes de la versión v1 y v2.&#x20;

Para habilitar la versión 2 de RIP tendríamos que utilizar el comando siguiente:&#x20;

```
R1(config)# router rip
R1(config-router)# version 2
```

Si volvemos a lanzar el comando&#x20;

```
show ip protocols 
```

vemos que ahora está configurado el router R1 para enviar y recibir  únicamente mensajes de RIPv2, siendo capaz de incluir la máscara de subred haciendo que RIP sea un protocolo de enrutamiento sin clase.

{% hint style="info" %}
Nota: El hecho de configurar la version 1 habilita solo RIPv1. Si configuramos **no version** revierte el router a la configuración predeterminada, mediante la cual se envían actualizaciones de la versión 1 pero se mantiene a la escucha tanto de las actualizaciones de la versión 1 como de la versión 2.
{% endhint %}

Esto se debe a que el R1 ahora está a la escucha de actualizaciones RIPv2 únicamente. Todavía los routers R2 y R3 envían actualizaciones RIPv1, con lo cual, debemos configurar el comando de la versión2 en todos los routers. Podemos verificar que no haya ninguna ruta RIP en la tabla de enrutamiento.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption><p>Verificar que no haya rutas RIPv1</p></figcaption></figure>

La versión 2 de RIP  también resume de modo automático las redes en los límites de red principales. Podemos comprobarlo con el comando: **show ip protocols**. Sin embargo, podemos modificar el comportamiento predeterminado de RIPv2 utilizando el comando del modo de configuración del router:&#x20;

```
R1(config)# router rip 
R1(config-router)# no auto-summary 
```

Una vez que se deshabilita la sumarización automática, RIPv2 no resume las redes a su dirección con clase en routers fronterizos, lo que permite incluir todas las subredes y sus máscaras en las  actualizaciones del enrutamiento. Podemos verlo en la salida del comando: do show ip protocols | section Automatic.

<pre><code><strong>R1(config-router)#do show ip protocols | section Automatic
</strong>Automatic network summarization is not in effect
</code></pre>

### Interfaces pasivas

El protocolo RIP siempre envía las actualizaciones, aunque no exista ningún dispositivo RIP en esa red. No hay modo de que el router sepa si hay o no dispositivos conectados  por tanto, envía una actualización cada 30 segundos, con lo cual, se:

* **Desperdicia ancho de banda**: Las actualizaciones se transmiten por difusión o multidifusión, por lo que los switches también reenvían las actualizaciones por todos sus puertos.
* **Desperdicia recursos**: todos los dispositivos de la red  procesan la actualización hasta la capa de transporte, que es donde los dispositivos descartan la actualización.
* **Riesgo de seguridad**: el anuncio de las actualizaciones en una red de difusión es un riesgo de seguridad dado que pueden interceptarse, modificar y enviar de regreso al router, y dañar así la tabla de enrutamiento con métricas falsas que desorientan el tráfico.

Para evitar que las actualizaciones de enrutamiento se transmitan a través de una interfaz del router y permitir que esa red se siga anunciando a otros routers debemos utilizar el comando de configuración del router passive-interface:

```
router rip
passive-interface gi0/0
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt="" width="563"><figcaption><p>Salida del comando do show protocols</p></figcaption></figure>

Este comando detiene las actualizaciones de routing a través de la interfaz especificada, pero, la red a la que pertenece la interfaz especificada aún se anuncia en las actualizaciones de routing enviadas a otras interfaces.

No es necesario que los routers R1, R2 y R3 reenvíen las actualizaciones RIP por sus interfaces LAN. El comando **show ip protocols** nos permite verificar que la interfaz Ethernet0/0 es pasiva. Asimismo, observe que la red 192.168.1.0 aún se encuentra en **Routing for Networks**, lo cual significa que esta red aún está incluida como una entrada de ruta en las actualizaciones RIP que se envían al R2.

{% hint style="info" %}
Todos los protocolos de routing admiten el comando passive-interface.
{% endhint %}

Todas las interfaces se pueden convertir en pasivas utilizando el comando:&#x20;

```
passive-interface default
```

Y en caso de aquellas interfaces que no deben serlo, se pueden volver a habilitar con el comando:

<pre><code><strong>no passive-interface
</strong></code></pre>



### Propagación de rutas predeterminadas

Veamos el siguiente caso. El router R3 es un router perimetral que está  conectado a un ISP. Para que R3 llegue a Internet, solo se requiere una ruta estática predeterminada desde la interfaz E0/2.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

También pudiéramos configurar rutas estáticas predeterminadas en el R1 y en el R2, pero es más escalable crear la ruta estática en el router R3 y, después hacer que se propague al resto de los routers usando RIP. De este modo se le proporciona conexión a Internet al resto de redes del dominio de enrutamiento RIP. La ruta estática predeterminada se debe publicar a todos los routers que usan el protocolo de enrutamiento dinámico.

Por tanto, para propagar una ruta predeterminada en RIP, el router perimetral debe estar configurado de la siguiente manera:

Utilizar el siguiente comando para establecer la ruta estática predeterminada:

```
ip route 0.0.0.0  0.0.0.0
```

Aplicar el comando de configuración del router:&#x20;

```
default-information originate
```

Se le ordena que cree información predeterminada mediante la propagación de la ruta estática en las actualizaciones RIP. Por tanto,&#x20;

* se configura una ruta estática predeterminada completamente especificada al ISP&#x20;
* se propaga la ruta mediante RIP

Esto es, el router R3 tiene un gateway de último recurso y una ruta predeterminada configurados en su tabla de enrutamiento.



### Links

* [https://redes.umh.es/cisco/CCNA/es/RSE/index.html#3.3.3.1](https://redes.umh.es/cisco/CCNA/es/RSE/index.html#3.3.3.1)

