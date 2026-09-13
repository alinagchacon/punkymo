---
description: ¿Qué debemos saber?
---

# Bastionado de redes

## Bastionado de redes y sistemas

#### <mark style="color:violet;">Primer Trimestre: B1 + B2 — Análisis de riesgos. Redes seguras</mark>

* **Metodologías y gobernanza:** Análisis de riesgos con MAGERIT y PILAR, selección de controles (CIS Controls, ISO 27001, CCN-STIC) y principios de economía circular.
* **Seguridad en redes:** Segmentación L2/L3 (VLANs, Subnetting, DMZ, OSPF), firewalls con estado (Stateful), VPNs IPSec y seguridad Wi-Fi (WPA2/WPA3).

**Resultados de Aprendizaje:**&#x20;

* **RA1** (Planes de Securización)
* **RA4** (Diseño de Redes Seguras)
* **RA5** (Configuración NGFW/VPN)

#### <mark style="color:violet;">Segundo Trimestre: B3 + B4 — Identidades, PKI, Hardening OS</mark>

* **Gestión de Identidades:** Despliegue de Infraestructura de Clave Pública (PKI/CA), servidores RADIUS/TACACS+, 802.1X y autenticación multifactórica (MFA).
* **Bastionado de Sistemas:** Hardening de bajo nivel (BIOS/UEFI, Secure Boot, cifrado LUKS, particionado de `/tmp` con `noexec`), bastionado SSH, HIDS (Wazuh) y SIEM.

**Resultados de Aprendizaje:**&#x20;

* **RA2** (Control de Acceso MFA)
* **RA3** (Credenciales & RADIUS)
* **RA6** (Instalación Segura)
* **RA7** (Bastionado OS & HIDS)

#### <mark style="color:violet;">Tercer Trimestre: B5 + B6 — Cloud, SDN/OT. Proyecto SOC</mark>

* **Infraestructuras Avanzadas:** Arquitecturas SDN/SD-WAN, bastionado en entornos OT/IoT y despliegue Micro-Cloud (Canonical MicroK8s/MicroCloud).
* **Integración Global:** Proyecto final orquestado tipo SOC integrando la totalidad de los RAs (RA1 a RA7).

### Fundamentos técnicos

#### B1. RA1 - Análisis de riesgos. Economía circular

El riesgo nominal o intrínseco se calcula mediante la relación matemática entre activo, vulnerabilidad y amenaza:

$$\text{Riesgo} = \text{Impacto} \times \text{Probabilidad} = (\text{Activo} \times \text{Vulnerabilidad}) \times \text{Amenaza}$$

* **Activo:** Recurso con valor tangible o intangible para la organización (datos, servidores, reputación).
* **Vulnerabilidad:** Debilidad intrínseca del sistema susceptible de ser explotada.
* **Amenaza:** Evento potencial que puede causar un daño o impacto negativo.
* **Frameworks de bastionado:** Uso de guías CCN-STIC y CIS Controls para la aplicación estandarizada de contramedidas.
* **Economía circular en ciberseguridad:** Reutilización defensiva de hardware obsoleto (como sensores IDS o firewalls secundarios) previa sanitización segura de datos (borrado verificado según estándares como IEEE 2883 o DoD).

#### B2. RA4 - RA5 - Diseño de redes seguras, segmentación y perímetro.

* **Subnetting - Troncales 802.1Q:** La segmentación física y lógica minimiza los dominios de difusión y colisión. En los enlaces troncales la cabecera 802.1Q inserta una etiqueta de 4 bytes.
* **VLAN Nativa:** Debemos cambiar siempre de la VLAN ID 1 predeterminada para mitigar ataques de _VLAN Hopping_ o _Double Tagging_.
* **Stateful firewall y el enrutamiento asimétrico:** Un firewall de estado mantiene la tabla de conexiones o _State Table_ registrando paquetes SYN y autorizando sus SYN-ACK de vuelta. Si la respuesta retorna por un router secundario (enrutamiento asimétrico), el firewall no registra el SYN-ACK y la conexión cae por _Timeout_.
* **Arquitectura DMZ:** Los servidores en la DMZ **nunca deben iniciar tráfico TCP directamente hacia la LAN interna**.
* **VPN IPSec:** IKEv1/v2 Fase 1 (autenticación e intercambio Diffie-Hellman) + Fase 2 (túnel ESP/AH para cifrado de datos). **Tailscale**.

#### B3. RA2 - RA3 . Autenticación MFA, PKI y AAA

* **Factores de Autenticación (MFA):** Es la combinación de, al menos, dos factores de categorías distintas: algo que sé (por ejemplo: contraseña), algo que tengo (por ejemplo, token OTP) o algo que soy (por ejemplo biometría).
* **Infraestructura PKI (X.509):** La Autoridad de Certificación (CA) firma solicitudes CSR utilizando la **clave privada de la propia CA**. El cliente valida la autenticidad comprobando la firma con la clave pública de la CA raíz presente en su almacén local.
*   **Control de Acceso 802.1X:**&#x20;

    * Es un sistema de seguridad que pide la identidad de un dispositivo o usuario antes de permitirle el acceso a la red.&#x20;
    * El **cliente o supplicant** quiere conectarse a la red. Para ello utiliza el protocolo de red Autenticación Extensible sobre LAN - _Extensible Authentication Protocol over LAN_ que encapsula los mensajes de autenticación **EAP** para permitir que viajen a través de redes LAN, tanto cableadas como inalámbricas.
    * El **autenticador o puente** es el dispositivo de red - switch en redes cableada o la antena Wi-Fi (AP) - en redes inalámbricas, a través del cual el cliente intenta acceder. &#x20;
    * El **servidor de autenticación** es el servidor central que, habitualmente, usa RADIUS, para  guardar las contraseñas, certificados o listas de usuarios permitidos.

    El switch/AP actúa exclusivamente como **Authenticator**, encapsulando tramas EAP en paquetes RADIUS hacia el servidor central.

#### B4. RA6 - RA7 - Hardening de sistemas, HIDS y Logs

* **Bajo Nivel - UEFI:** Protección por contraseña en BIOS/UEFI, deshabilitación de arranque por USB/CD y habilitación de _Secure Boot_ para validar firmas del kernel.
* **Cifrado y particionado:** Cifrado completo con LUKS (`dm-crypt`) y montaje de `/tmp`, `/var/tmp` y `/dev/shm` con las opciones `noexec, nosuid, nodev` .
* **Hardening de SSH:** En `/etc/ssh/sshd_config`, establecer `PermitRootLogin no`, `PasswordAuthentication no`, claves Ed25519 y cambio del puerto por defecto.
* **HIDS Wazuh - SIEM:** Despliegue de agentes HIDS para monitorización de integridad de archivos (FIM), detección de rootkits y envío de registros Syslog cifrados al SIEM.

***

### Algunos comandos útiles&#x20;

#### Para auditoría y diagnóstico en Linux

```bash
# Lista todos los puertos TCP/UDP a la escucha en el host local junto a sus PIDs
ss -tulpn

# Sniffer de red en tiempo real omitiendo resolución de nombres (puertos 53 u 80)
tcpdump -nn -i any port 53 or port 80

# Determina la interfaz de red y puerta de enlace empleada por el kernel para un destino
ip route get 8.8.8.8

# Inspecciona contenido, emisor, fechas de validez y SANs de un certificado X.509 PEM
openssl x509 -in cert.pem -text -noout
```



