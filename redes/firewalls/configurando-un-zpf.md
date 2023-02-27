---
description: Tutorial
---

# Configurando un ZPF

Vamos a configurar un firewall ZPF en una topología como la siguiente:

&#x20;   &#x20;

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Configurando un firewall de zona</p></figcaption></figure>

#### Configuraciones iniciales

1. Ten en cuenta el modelo del Router que debe ser: ISR4331&#x20;
2. Configura las direcciones IP estáticas a los equipos de la red 192.168.1.0/24 y al servidor web de la red pública, por ejemplo: 10.0.0.5/8. &#x20;
3. Igualmente, configura las interfaces de red del router para cada una de las redes: privada y pública. En mi caso utilicé: 192.168.1.1/24 para la red privada y la 10.0.0.1/8 para la red pública.
4. Comprueba que hay tráfico entre ambas redes, tanto por ICMP como por HTTP.

#### Configurando las zonas en el router&#x20;

* &#x20;****&#x20;

```
Router(config)#zone security PRIVATE
Router(config-sec-zone)#exit
Router(config)#zone security PUBLIC
Router(config-sec-zone)#exit
```

* **Paso 2**: Identifique el tráfico con un mapa de clase.

```
Router(config)#class-map type inspect match-any HTTP-TRAFFIC
Router(config-cmap)#match protocol http
Router(config-cmap)#match protocol https
Router(config-cmap)#match protocol dns
```

* **Paso 3**: Defina una acción con un mapa de políticas.

```
Router(config)#policy-map type inspect PRIV-TO-PUB-POLICY
Router(config-pmap)#class type inspect HTTP-TRAFFIC
Router(config-pmap-c)#inspect 
```

* **Paso 4**: Identifique un par de zonas y relaciónelo con un mapa de políticas.

```
Router(config)#zone-pair security PRIV-PUB source PRIVATE destination PUBLIC
Router(config-sec-zone-pair)#service-policy type inspect PRIV-TO-PUB-POLICY
```

* **Paso 5**: Asigne zonas a las interfaces correspondientes.

```
Router(config)#interface GigabitEthernet 0/0/0
Router(config-if)#zone-member security PRIVATE
Router(config-if)#exit
Router(config)#interface GigabitEthernet 0/0/1
Router(config-if)#zone-member security PUBLIC

```

La política de servicio estará activa. Se inspeccionará el tráfico HTTP, HTTPS y DNS que proviene de la zona PRIVATE y está destinado a la zona PUBLIC. El tráfico que proviene de la zona PUBLIC y está destinado a la zona PRIVATE solo se permitirá si forma parte de las sesiones iniciadas originalmente por los hosts de la zona PRIVATE.

### Verificar la configuración de un ZPF

Verifica la configuración de ZPF viendo la configuración en ejecución. Observa que el mapa de clase aparece primero. Luego, el mapa de políticas hace uso del mapa de clases. Además, observa que en la clase resaltada class-default dejará caer todo el resto del tráfico que no sea miembro de la clase HTTP-TRAFFIC.

Las configuraciones de zona siguen las configuraciones del mapa de políticas con el nombramiento de zonas, el emparejamiento de zonas y la asociación de una política de servicio al par de zonas. Por último, las interfaces son zonas asignadas.

```
Router#show run
```

<figure><img src="../../.gitbook/assets/image (18) (3).png" alt=""><figcaption><p>show run</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>show run</p></figcaption></figure>

Otros comandos útiles para verificar:

```
Router#show zone security 
Router#show zone-pair security
```
