# Ficha — Segmentación por VLAN, DMZ y monitoreo de tráfico en pfSense

**Documento fuente:** `Informe_Laboratorio_pfSense.pdf` (24 páginas, publicado completo en esta carpeta)
**Autoría:** individual — A.S.P.S.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 14 de agosto de 2026
**Caso:** empresa ficticia *TecnoSoluciones S.A.S.*

## Qué es

Laboratorio donde se pasa de una red plana a una red segmentada con pfSense: VLANs con DHCP por segmento,
una DMZ con servidor web publicado por NAT, matriz de reglas de firewall y monitoreo de tráfico por subred.

## Objetivo

Resolver el caso de una red sin segmentar (lentitud, servidores expuestos, sin control ni reportes):
segmentar el tráfico, aislar servicios públicos en DMZ y medir quién consume el ancho de banda.

## Entorno

| Interfaz | Dirección | Función |
|---|---|---|
| WAN | `10.0.2.15/24` | Salida a Internet por NAT del hipervisor |
| LAN | `192.168.100.1/24` | Troncal de VLAN |
| DMZ | `192.168.150.1/24` | Zona desmilitarizada, servidor web `192.168.150.10` |

pfSense 2.7.2-RELEASE; VLANs para operaciones y invitados (`192.168.110.0/24` con rango DHCP
`.100`–`.200` y `192.168.120.0/24` con `.100`–`.200`); interfaz `PRUEBAS` que simula un ISP externo para
validar el NAT sin exponer el laboratorio.

## Qué se hizo

1. Instalación y asignación de las tres interfaces base (WAN, LAN, DMZ) sobre `vtnet0`/`vtnet1`.
2. Creación de VLAN sobre la LAN con su propio DHCP por segmento.
3. DMZ con servidor web y regla de *Port Forward* HTTP hacia `192.168.150.10`, aplicada sobre la interfaz
   `PRUEBAS` para simular tráfico entrante externo.
4. Matriz de reglas de firewall por zona, con descripción en cada regla para auditoría.
5. Monitoreo con **ntopng, Darkstat, BandwidthD y pfTop**, comparando consumo de tráfico por subred.

## Resultados comprobados

- Acceso al servidor web de la DMZ a través del NAT de publicación, validado desde el segmento de pruebas.
- Reglas aplicadas y comprobadas con pruebas de conectividad entre zonas (permitido/bloqueado).
- Comparación de tráfico por subred en BandwidthD: la VLAN de administración es la que más consume, por ser
  la que ejecuta la gestión del firewall.
- Aislamiento de la DMZ: solo se permite tráfico a los puertos publicados (80/443) y se bloquea el
  movimiento lateral hacia las VLAN internas.

## Límites y lo que no está demostrado

- El NAT de publicación quedó validado sobre la interfaz de pruebas, **no** sobre la WAN real: el propio
  informe lo deja como recomendación de mejora.
- No hay IDS/IPS en este laboratorio (Suricata aparece como trabajo futuro).
