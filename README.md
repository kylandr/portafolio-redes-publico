# Portafolio técnico — redes, servidores, seguridad y virtualización

Colección de laboratorios, informes y manuales que he desarrollado durante mi formación como tecnólogo en
Gestión de Redes de Datos (SENA, ficha 3171046) y en mi trabajo independiente de laboratorio.

La mayoría son implementaciones reales en máquinas virtuales o equipos físicos, con su configuración, sus
pruebas y —cuando existe— la evidencia de que la prueba se ejecutó. Donde falta evidencia, el documento lo
dice: hay una sección de límites en cada trabajo y una lista explícita de lo que no está demostrado.

## Por dónde empezar

Los cuatro trabajos con más profundidad, evidencia y documentación (OPNsense con el montaje físico en
hardware y switch Huawei es el principal):

| Trabajo | Qué encontrarás | Volumen |
|---|---|---|
| **[OPNsense en hardware físico y laboratorio](01-seguridad-perimetral/opnsense-sena/README.md)** | Firewall segmentado en 4 VLAN sobre troncal 802.1Q contra un switch Huawei, 21 reglas guardadas en el equipo físico, Suricata IPS, QoS y 11 pruebas instrumentadas en el entorno virtual | Montaje físico + laboratorio virtual |
| **[pfSense sobre Proxmox VE](01-seguridad-perimetral/pfsense/README.md)** | Filtrado DNSBL con pfBlockerNG, anti-bypass de DNS por NAT y Limiters QoS, sobre dos bridges de Proxmox; más dos laboratorios previos de segmentación y DMZ | 3 informes, 2 completos en PDF |
| **[Zabbix 7.0 + rsyslog](05-monitoreo/README.md)** | Servidor de monitoreo con agentes y SNMP, integración de pfSense, tableros, alertas y tres simulaciones de incidentes | Ficha técnica detallada |
| **[GLPI 11 sobre Debian 13](03-servidores/glpi/README.md)** | Mesa de servicio e inventario: tutorial en video de la implementación, informe de instalación y agente de inventario en Windows | Video + informe + ficha |

## Índice por tema

### 01. Seguridad perimetral
- [OPNsense: segmentación, firewall, IPS y QoS](01-seguridad-perimetral/opnsense-sena/README.md) — laboratorio virtual instrumentado + montaje físico con switch Huawei.
- [pfSense: instalación, VLAN/DMZ, DNSBL y QoS](01-seguridad-perimetral/pfsense/README.md) — tres laboratorios de dificultad creciente, el último sobre Proxmox VE.

### 02. Redes
- [REDCIES: red IoT en Cisco Packet Tracer](02-redes/packet-tracer/redcies-iot.md) — proyecto grupal de semillero.
- [VLANs y troncal 802.1Q](02-redes/evidencias/01_vlan_y_troncal.md) · [Subneteo](02-redes/evidencias/02_subneteo_vlsm.md) · [Router-on-a-Stick](02-redes/evidencias/03_router_on_a_stick.md).

### 03. Servidores
- [GLPI 11 sobre Debian 13](03-servidores/glpi/README.md) — tutorial en video, informe de instalación y agente de inventario en Windows.
- [Servidor web y DNS en red segmentada](03-servidores/web-dns/README.md) — VLANs con Catalyst 2960, router 2911 y publicación de `gugle.net`.
- [Linux: guías y distribuciones](03-servidores/linux/README.md).

### 04. Telefonía IP
- [Issabel sobre Proxmox VE con OSPF e IAX2](04-telefonia-ip/README.md) — cinco centrales, seis routers, llamadas entre grupos (con video).
- Cisco CME con IP Communicator y diseño de solución VoIP.

### 05. Monitoreo
- [Zabbix 7.0, SNMP y ntopng](05-monitoreo/README.md).

### 06. Seguridad de la información
- [Hardening, plan de seguridad, cifrado y VPN (los dos últimos como ficha, autoría pendiente)](06-seguridad-de-la-informacion/README.md).

### 07. Apuntes técnicos
- [Notas de referencia](07-apuntes-tecnicos/README.md) — Huawei VRP, Active Directory, Zabbix y hardening de Linux.

### 08. Soporte
- [Diseño de una mesa de ayuda](08-soporte/mesa-de-ayuda.md) — flujo de atención, SLA y escalamiento.

## Cómo leerlo

- **Informe completo** (PDF o Markdown extenso): manual reproducible, con configuración y resultados.
- **Ficha**: resumen de una práctica con lo esencial — qué se hizo, en qué entorno, qué se comprobó y qué no.
- **Evidencia**: capturas, logs, extractos de configuración o archivos de simulación.

Cuando un trabajo fue en equipo, la ficha lo dice y nombra a los integrantes; donde un compañero aún no ha
autorizado aparecer con nombre completo, se usan iniciales. Los trabajos grupales de semillero llevan también
el crédito del instructor.

## Sobre la depuración de este material

Todo lo publicado pasó por una revisión para retirar credenciales, claves, certificados y datos de terceros.
Los respaldos de firewall (XML) **no** se publican: contienen el certificado del WebGUI con su clave privada,
hashes de contraseñas y claves de VPN. Se conservan en local y de ellos se extrajo solo la parte publicable
(interfaces, VLAN, reglas y fechas). Los archivos de Packet Tracer (`.pkt`, `.pka`) tampoco se publican: son
binarios de simulación que no se pueden auditar de forma fiable y no aportan nada que no esté en las fichas.

Los rangos `10.x`, `192.168.x` y los nombres de dominio usados son de laboratorio.

## Estado de publicación

Tres informes completos quedan fuera de esta versión y se publicarán cuando se resuelvan sus pendientes:

| Informe | Motivo |
|---|---|
| VPN site-to-site (OpenVPN) | Autoría no confirmada: la portada nombra a una persona pero el cuerpo describe tres roles de equipo. Además el PDF contiene una contraseña en texto plano. |
| Cifrado en reposo (VeraCrypt/EFS/OpenSSL) | Misma situación de autoría (tres roles sin coautores). |
| Zabbix 7.0 + rsyslog | Una captura muestra la contraseña de la base de datos y la portada trae el nombre completo de un compañero, que pasará a iniciales hasta que él lo autorice. |

De los tres hay ficha publicada, así que el trabajo y sus resultados quedan documentados.

## Contacto

[github.com/kylandr](https://github.com/kylandr)
