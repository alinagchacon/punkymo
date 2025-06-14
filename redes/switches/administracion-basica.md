---
description: Switches
---

# Administración básica

Si queremos tener acceso remoto a un switch para su configuración y administración  debemos configurarlo con una dirección IP y la máscara de subred correspondiente. Para ello, primero tenemos que conectarnos al switch por el puerto consola y asignar una dirección IP a la interfaz virtual  (SVI) del switch.

Si conoces las VLAN entonces has escuchado hablar de la interfaz virtual o SVI. Igualmente, de modo predeterminado, los switches vienen configurados para que la administración se realice a través de la  VLAN 1 que es la VLAN por defecto de administración.&#x20;

De manera predeterminada todos los puertos de un switch se asignan a la VLAN 1 y por seguridad, se recomienda utilizar una VLAN de administración distinta de la VLAN 1. Incluso, una VLAN que no tenga puertos en uso.

## Configuración de acceso

Veamos los pasos que tenemos que seguir para configurar el acceso al switch.

**Configurar la interfaz de administración -** Con una dirección IPv4 y una máscara de subred para la SVI de administración del switch desde el modo de configuración de interfaz VLAN. esto es:

```
SW1(config)# interface vlan 99
SW1(config-if)# ip address 192.168.1.11 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# end
SW1# copy running-config startup-config 
```

La vlan 99 se utiliza para acceder al modo de configuración de interfaz. Con el comando ip address se le asigna una IP a la interfaz en cuestión, en mi ejemplo, la IP 192.168.1.11.

Tener en cuenta que la SVI para la VLAN 99 no se mostrará  **up/up** hasta que no se cree como tal la VLAN 99 y que haya un dispositivo conectado a un puerto del switch que esté asociado a dicha VLAN.&#x20;

```
SW1(config)# vlan 99
SW1(config-vlan)# name admin
SW1(config-vlan)# exit
```

**Configuración del gateway predeterminado -** Si el switch se va a administrar de forma remota desde redes que no están conectadas directamente, se debe configurar con un gateway predeterminado. El gateway predeterminado es el router al que está conectado el switch. El switch reenvía los paquetes IP con direcciones IP de destino fuera de la red local al gateway predeterminado. Como se muestra en la figura 2, R1 es el gateway predeterminado para S1. La interfaz en R1 conectada al switch tiene la dirección IPv4 172.17.99.1. Esta es la dirección de gateway predeterminado para S1.

Para configurar el gateway predeterminado del switch, use el comando ip default-gateway. Introduzca la dirección IPv4 del gateway predeterminado. El gateway predeterminado es la dirección IPv4 de la interfaz del router a la que está conectado el switch. Use el comando copy running-config startup-config para realizar una copia de seguridad de la configuración.

Paso 3: Verificar la configuración

Como se muestra en la figura 3, el comando show ip interface brief es útil para determinar el estado de las interfaces virtuales y físicas. El resultado que se muestra confirma que la interfaz VLAN 99 se ha configurado con una dirección IPv4 y una máscara de subred.
