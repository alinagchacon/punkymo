# Firewall

El diseño de un firewall tiene como objetivo el poder permitir o denegar el tráfico según el origen, el destino y el tipo de tráfico. Hay diseños simples que solamente diseñan una red externa y una interna, y que son determinadas por dos interfaces en un firewall.

#### Privado y público

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Pública / Privada</p></figcaption></figure>



Normalmente, un firewall con dos interfaces se configura del siguiente modo:

* El tráfico procedente de la red privada se autoriza e inspecciona mientras viaja hacia la red pública. También se autoriza el tráfico inspeccionado que regresa de la red pública y está relacionado con el tráfico que se originó en la red privada.
* Generalmente, se bloquea el tráfico procedente de la red pública que viaja hacia la red privada.
