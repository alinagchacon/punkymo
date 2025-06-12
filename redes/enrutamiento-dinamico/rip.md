# RIP

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

