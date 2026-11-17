# pfSense — instalación, segmentación, DMZ, filtrado DNSBL y QoS

Tres laboratorios sobre pfSense, todos individuales, con el mismo caso de estudio (*TecnoSoluciones /
TechSoluciones S.A.S.*) y dificultad creciente.

| Laboratorio | Qué demuestra | Fechas | Evidencia |
|---|---|---|---|
| 1. Instalación, servicios y diagnóstico | Despliegue del firewall, DHCP, reglas básicas y herramientas de diagnóstico | jul-2026 | [ficha](evidencias/03_ficha_instalacion_y_diagnostico.md) |
| 2. VLAN, DMZ y monitoreo | Segmentación con DHCP por VLAN, publicación de servidor web en DMZ y monitoreo con ntopng/Darkstat/BandwidthD | ago-2026 | [informe completo (PDF)](evidencias/02_informe_pfsense_tecno_soluciones.pdf) · [ficha](evidencias/04_ficha_vlan_dmz_y_monitoreo.md) |
| 3. Seguridad perimetral en **Proxmox VE** | Filtrado DNSBL con pfBlockerNG, anti-bypass de DNS y Limiters QoS para streaming | oct-2026 | [informe completo (PDF)](evidencias/01_informe_pfsense_proxmox.pdf) |

## Laboratorio 3 — el más completo (pfSense sobre Proxmox VE)

Este es el que más valor tiene por sí solo, así que va publicado completo. Resumen de lo que implementa:

- **Virtualización:** Proxmox VE con dos bridges — `vmbr0` (WAN, `192.168.8.72/24`) y `vmbr1` (LAN aislada,
  `192.168.18.1/24`, sin NIC física), más un contenedor LXC Debian como cliente (`192.168.18.10`).
- **Filtrado de contenido:** `pfBlockerNG-devel` + Unbound DNSBL con feeds UT1 (adult, gambling, games) y un
  grupo propio, redirigiendo dominios bloqueados a un *sinkhole* local `10.10.10.1`.
- **Anti-bypass:** regla NAT de *Port Forward* que intercepta las consultas DNS hacia servidores externos
  (`8.8.8.8`, `1.1.1.1`) y las fuerza al resolver local; el DHCP entrega el DNS controlado al cliente.
- **QoS:** *Limiters* `YouTube_360p_In` a 1.2 Mbit/s y `YouTube_360p_Out` a 256 Kbit/s con máscaras por
  destino/origen, para degradar el video a 360p o menos.

**Resultados reportados en el informe:** las consultas a `bet365.com`, `roblox.com`, `betplay.com.co` y
`wplay.co` devuelven la IP del sinkhole `10.10.10.1`, mientras `facebook.com` resuelve su IP pública real;
la reproducción de YouTube se midió en 427 Kbps con resolución 256x144 (códec AV1), dentro del límite; la
inspección de logs se hizo por SSH en el propio firewall.

## Mi aporte

Individual en los tres: caso de estudio, diseño de topología y direccionamiento, implementación en pfSense
(GUI, reglas y paquetes), ejecución de las pruebas y redacción de los informes.

## Alcance

- Todo es laboratorio virtual (VirtualBox en los dos primeros, Proxmox VE en el tercero).
- El NAT de publicación del laboratorio 2 quedó validado sobre una interfaz de pruebas, no sobre una WAN
  real.
- No hay capturas de auditoría externa ni pruebas de penetración: el alcance es configuración y verificación
  funcional.
