---
description: Tomado de https://redes.umh.es/cisco/CCNA
---

# Tablas de enrutamiento

## IPv6 y la tabla de enrutamiento

Para el direccionamiento IPv6, los componentes de la tabla de enrutamiento son muy similares a los de IPv4. La tabla de enrutamiento se completa con las interfaces que están conectadas directamente, con las rutas estáticas y con las rutas descubiertas de forma dinámica.

IPv6 es un protocolo sin clase, con lo que todas las rutas rutas finales de nivel 1. No hay rutas principales de nivel 1 para rutas secundarias de nivel 2.

<figure><img src="../../../.gitbook/assets/image (48).png" alt="" width="563"><figcaption><p>Topología con IPv6. Tomado de <a href="https://redes.umh.es/cisco/CCNA/es/RSE/index.html#3.3.4.1">https://redes.umh.es/cisco/CCNA</a></p></figcaption></figure>

En el esquema de red de la imagen podemos ver que los tres routers: R1, R2 y R3 están configurados en malla, o sea, tienen caminos redundantes hacia el resto de redes. En los tres routers se ha configurado el protocolo EIGRP para IPv6.

### Entradas conectadas directamente

En la tabla de routing que se muestra podemos ver la red conectada y las entradas en la tabla de las interfaces conectadas directamente. En las entradas de las rutas conectadas directamente se muestra la siguiente información:

* **Origen de la ruta**: identificación del modo en que se descubrió la ruta. Estas interfaces tienen dos códigos de origen de ruta:
  * C - red conectada directamente
  * L - ruta local
* **Red conectada directamente**: la dirección IPv6 de la red conectada directamente.
* **Distancia administrativa**: IPv6 utiliza las mismas distancias que IPv4. El valor 0 indica el mejor origen y el más confiable.
* **Métrica**: el valor asignado para llegar a la red remota. Los valores más bajos indican las rutas preferidas.
* **Interfaz de salida**: la interfaz de salida que se utiliza para reenviar paquetes a la red de destino.

<figure><img src="../../../.gitbook/assets/image (49).png" alt="" width="563"><figcaption><p>Tomado de: <a href="https://redes.umh.es/cisco/CCNA/es/RSE/index.html#3.3.4.2">https://redes.umh.es/cisco/CCNA/es/RSE/index.html#3.3.4.2</a></p></figcaption></figure>

### Ejemplo 1

A partir de la red especificada, identificar las partes de una entrada en la tabla de enrutamiento.

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="563"><figcaption></figcaption></figure>

### Ejemplo 2

A partir de la red especificada, identificar las partes de una entrada en la tabla de enrutamiento.

<figure><img src="../../../.gitbook/assets/image (51).png" alt="" width="563"><figcaption></figcaption></figure>

### Ejemplo 3

A partir de la red especificada, identificar las partes de una entrada en la tabla de enrutamiento.

<figure><img src="../../../.gitbook/assets/image (52).png" alt="" width="563"><figcaption></figcaption></figure>
