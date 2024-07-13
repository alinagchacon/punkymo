# Qemu agent

Se trata de un demonio o servicio que se instala en el equipo invitado. Por tanto, es un servicio auxiliar que se utiliza para intercambiar información entre el anfitrión y el invitado y ejecuta comandos en el invitado.

En Proxmox, el **qemu-guest-agent** se utiliza para tres cosas:

* apagar correctamente el invitado (VM o contenedor) en en lugar de depender de comandos ACPI o políticas de Windows.
* Para congelar el sistema de archivos invitado al realizar una copia de seguridad/instantánea.&#x20;
* Si el agente invitado está habilitado y en ejecución, llama a guest-fsfreeze-freeze y guest-fsfreeze-thaw para mejorar la "consistencia".
* En la fase en la que el invitado (VM) se reanuda después de una pausa (por ejemplo, después de una instantánea), sincroniza inmediatamente su hora con el hipervisor usando qemu-guest-agent (como primer paso).

Incluso, puede ser útil para algo más simple que todo esto: que muestre la IP en el dashboard:

<figure><img src="../../.gitbook/assets/image.png" alt="" width="563"><figcaption><p>Información de una VM en el dashboard de Proxmox</p></figcaption></figure>

### Habilitar el qemu agent

Basta con instalarlo de la siguiente manera:

```
sudo apt install qemu-guest-agent
```

Una vez hecho, lo iniciamos&#x20;

```
sudo apt install qemu-guest-agent
sudo apt install qemu-guest-agent
```

### Links

* [https://pve.proxmox.com/wiki/Qemu-guest-agent](https://pve.proxmox.com/wiki/Qemu-guest-agent)&#x20;
