---
description: Uncomplicated firewall de ubuntu
---

# UFW

### ¿Qué es UFW?

El UFW o Uncomplicated Firewall es una interfaz de gestión de firewall simplificada que oculta la complejidad de las tecnologías de filtrado de paquetes de nivel inferior del tipo de `iptables` y `nftables`.

UFW viene instalado por defecto en Ubuntu. Si se desinstaló por alguna razón, puede instalarlo con `sudo apt install ufw2`.

#### ¿Lo tenemos instalado?

Para asegurarnos que tenemos instalado ufw en nuestro equipo podemos utilizar alguno de los siguientes comandos:

```jsx
whereis ufw 
```

```jsx
dpkg -l | grep ufw
```

```jsx
ufw status
```

```jsx
dpkg -s ufw
```

Para instalar UFW si no lo está:

```jsx
sudo apt install ufw
```

Si nuestro equipo de Ubuntu tiene IPv6 habilitado, podemos comprobar que UFW esté configurado para admitir IPv6 de modo que se puedan administrar las reglas de firewall tanto para IPv4 como IPv6. Para ello debemos hacer:

```jsx
sudo nano /etc/default/ufw
```

y habilitamos IPv6:

```jsx
IPV6= yes
```

#### Políticas predeterminadas

Los valores predeterminados de las reglas de UFW que garantizan las reglas mínimas que establecen que hacia afuera se permita todo el tráfico pero hacia dentro no acceda nadie:

```jsx

sudo ufw default deny incoming

sudo ufw default allow outgoing
```

Estas reglas controlan el manejo del tráfico que no coincida de forma explícita con otras reglas. Por defecto, UFW está configurado para denegar todas las conexiones entrantes y permitir todas las conexiones salientes. Esto significa que quien intente establecer conexión con su servidor no podrá hacerlo, mientras que cualquier aplicación dentro del servidor podrá llegar al mundo exterior.

#### Permitiendo el acceso a través de SSH

Para configurar su servidor de modo que permita las conexiones SSH entrantes, puede utilizar este comando:

```bash
sudo ufw allow ssh
```

Esto crea reglas de firewall que permiten todas las conexiones en el puerto `22`, que es el que escucha el demonio SSH por defecto.

UFW registra el significado del puerto `allow ssh` porque está enumerado como servicio en el archivo `/etc/services`.

Pero, podemos escribir la regla equivalente especificando el puerto en vez del nombre del servicio. Por ejemplo, este comando funciona como el anterior:

```bash
sudo ufw allow 22
```

{% hint style="info" %}
Si configuraste el demonio SSH para utilizar un puerto diferente, debes especificar el puerto apropiado.
{% endhint %}

#### Permitiendo el acceso a HTTP / HTTPS

Para configurar su servidor de modo que permita las conexiones SSH entrantes, puede utilizar este comando:

```bash
sudo ufw allow http
```

Esto crea reglas de firewall que permiten todas las conexiones en el puerto `80`, que es el que escucha el demonio SSH por defecto.

UFW registra el significado del puerto `allow http` porque está enumerado como servicio en el archivo `/etc/services`.

Pero, podemos escribir la regla equivalente especificando el puerto en vez del nombre del servicio. Por ejemplo, este comando funciona como el anterior:

```bash
sudo ufw allow 80
```



### Links&#x20;

* [https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)
* [https://serverspace.io/es/support/help/basic-commands-ufw/](https://serverspace.io/es/support/help/basic-commands-ufw/)
* [https://help.ubuntu.com/community/UFW#Advanced\_Example](https://help.ubuntu.com/community/UFW#Advanced_Example)&#x20;

To be continued ....
