# Pi-hole

Se trata de un Adblock a nivel de red, que en lugar de tener que estar instalado en cada navegador de cada dispositivo en una red, se diría que está al mismo nivel que el router con lo cual todo dispositivo conectado a  la red  se beneficia de evitar la publicidad intrusiva y conexión a sitios peligrosos.

Por tanto, se trata de una aplicación de bloqueo de publicidad y de rastreadores en Internet a nivel de red en Linux que actúa como un sumidero de DNS​ y opcionalmente como un servidor DHCP, utilizado en  redes privadas.[ ](https://es.wikipedia.org/wiki/Pi-hole#cite\_note-:4-1)También está diseñado para uso en como la Raspberry Pi, pero se puede utilizar en entornos que ejecuten distribuciones Linux.

Pi-hole  es capaz de bloquear anuncios de sitios web, así como anuncios en televisores inteligentes y sistemas operativos para dispositivos móviles.

El proyecto Pi-hole fue creado por Jacob Salmela como una alternativa de código abierto al AdTrap en 2014​ y alojado en GitHub. ​

Pi-hole utiliza un dnsmasq modificado denominado FTLDNS, cURL, lighttpd, PHP y el panel de control AdminLTE para bloquear peticiones DNS para dominios conocidos de seguimiento y publicidad.&#x20;

En la [wikipedia ](https://es.wikipedia.org/wiki/Pi-hole)nos encontramos con lo siguiente:

La aplicación sirve como un servidor DNS para una red privada (reemplazando cualquier servidor DNS preexistente proporcionado por otro dispositivo o el propio ISP), con la capacidad de bloquear anuncios y rastreadores de dominios para los dispositivos de los usuarios.&#x20;

Obtiene listas de dominios publicitarios y de seguimiento a partir de fuentes predefinidas que pueden ser modificadas por el usuario con las que el Pi-hole compara las consultas DNS. Si se encuentra una coincidencia dentro de cualquiera de las listas, o de la lista negra del usuario, Pi-hole negará resolver el dominio solicitado y responderá al dispositivo solicitante con una página web en blanco.​

Debido a que Pi-hole bloquea dominios a nivel de red, es capaz de bloquear anuncios, como banners publicitarios en una página web, pero también puede bloquear anuncios en lugares no convencionales, como en Android, iOS y smart TV.​

Igualmente, los dominios pueden incluirse en una lista blanca de modo manual en caso de que la función de un sitio web se vea afectada por el bloqueo de dominios. Pi-hole también puede funcionar como una herramienta de monitorización de red que puede ayudar en la resolución de problemas de peticiones DNS y en la resolución de problemas de redes defectuosas.​

