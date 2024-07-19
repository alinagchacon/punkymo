# Qemu agent

Se trata de un demonio o servicio que se instala en el equipo invitado. Por tanto, es un servicio auxiliar que se utiliza para intercambiar información entre el anfitrión y el invitado y ejecuta comandos en el invitado.

En Proxmox, el **qemu-guest-agent** se utiliza para tres cosas:

* apagar correctamente el invitado (VM o contenedor) en en lugar de depender de comandos ACPI o políticas de Windows.
* Para congelar el sistema de archivos invitado al realizar una copia de seguridad/instantánea.&#x20;
* Si el agente invitado está habilitado y en ejecución, llama a guest-fsfreeze-freeze y guest-fsfreeze-thaw para mejorar la "consistencia".
* En la fase en la que el invitado (VM) se reanuda después de una pausa (por ejemplo, después de una instantánea), sincroniza inmediatamente su hora con el hipervisor usando qemu-guest-agent (como primer paso).

Incluso, puede ser útil para algo más simple que todo esto: que muestre información en el dashboard, como la IP de la VM:

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt="" width="563"><figcaption><p>Información de una VM en el dashboard de Proxmox</p></figcaption></figure>

### Instalar qemu agent

Para ello basta con instalar de la manera habitual e iniciar el servicio y habilitarlo. Esto es:

```
sudo apt install qemu-guest-agent
sudo systemctl start qemu-guest-agent
sudo systemctl enable qemu-guest-agent
```

También se debe habilitar el qemu agent en cada VM y en las opciones de la VM en Proxmox.

### Links

* [https://pve.proxmox.com/wiki/Qemu-guest-agent](https://pve.proxmox.com/wiki/Qemu-guest-agent)&#x20;
