# OPNsense 26.7 — segmentación 802.1Q, firewall, IPS y QoS (laboratorio virtual + montaje físico)

## Qué es

Implementación de un firewall perimetral **OPNsense 26.7** sobre FreeBSD 15.1 con cuatro redes segmentadas
por VLAN sobre un único troncal IEEE 802.1Q, políticas de firewall *zero-trust*, prevención de intrusiones
con Suricata, shaping FQ_CoDel, servidor web en DMZ y observabilidad con Grafana y Open-AudIT.

Se hizo en dos etapas: primero un **laboratorio virtual** completo (con clientes en *namespaces* de Debian,
donde se ejecutaron 11 pruebas instrumentadas) y después el **montaje en hardware físico** del centro de
formación, con el troncal contra un switch Huawei.

## Autoría

- **Montaje físico e informe final:** trabajo en pareja — A.S.P.S. y L.C.Z. (el informe original lista a los
  dos autores; el nombre completo del compañero queda en iniciales hasta que él autorice publicarlo).
- **Laboratorio virtual, arquitectura, pruebas instrumentadas y documentación:** individual.
- Los resultados del laboratorio virtual y los del montaje físico se presentan **separados** en este
  documento, porque no son la misma evidencia.

## Entorno

**Laboratorio virtual**

| Elemento | Detalle |
|---|---|
| Firewall | OPNsense 26.7 en VirtualBox (FreeBSD 15.1), WAN `em0` por NAT, troncal `em1` |
| Clientes | VM `debianBase` con cuatro *network namespaces* (`ns_admin`, `ns_users`, `ns_dmz`, `ns_guest`) |
| Pruebas | 11 pruebas con `tcpdump`, `nc`, `curl`, `dig`, `pfctl`, `ipfw`, `traceroute` |

**Montaje físico**

| Elemento | Detalle |
|---|---|
| Firewall | OPNsense 26.7 sobre mini-PC (Intel i5-1335U), NIC `re0` (WAN, DHCP) y `xl0` (trunk) |
| Conmutación | Switch Huawei CloudEngine **S5731-H48P4XC**, VRP 5.170 — trunk `GE0/0/2`, accesos `GE0/0/3-48`, `Vlanif10` = 10.50.10.2 |
| DMZ | Ubuntu Server 24.04 LTS, `10.50.30.145`, servicio `sena-dmz.service` en puerto 80 |
| Wi-Fi invitados | AP TP-Link en modo puente, VLAN 40, `port-isolate enable group 1` |
| Cliente de pruebas | Puesto Windows `10.50.20.174` en VLAN 20 |
| NOC | Grafana 13.3.1 + VictoriaMetrics + Open-AudIT en la estación de administración (VLAN 10) |

## Qué se hizo

**Físico (lo que confirma el respaldo del equipo):**

1. Reinstalación del equipo y configuración base, con respaldo tomado antes de tocar nada
   (`07_config-OPNsense-fisico-SENA-REINSTALADO-BASE.xml`).
2. Creación de las cuatro VLAN `802.1q` sobre `xl0` (`VLAN10_ADMIN`, `VLAN20_USERS`, `VLAN30_DMZ`,
   `VLAN40_GUEST`) y asignación como `opt1`–`opt4` con gateways `10.50.10.1/20.1/30.1/40.1`.
3. DHCP multi-ámbito con Dnsmasq (puerto 53053, dominio `sena.lab`) en las cuatro interfaces.
4. Política de firewall: **21 reglas guardadas** (15 `pass`, 6 `block`), con bloqueo lateral entre zonas y
   bloqueo hacia el propio firewall (`USERS_Block_Admin_FW`, `DMZ_Block_Admin_FW`, `GUEST_Block_Admin_FW`,
   `DMZ_Block_Lateral_Movement`, `GUEST_Block_Internal_Networks`).
5. Troncal 802.1Q puro contra el Huawei (sin VLAN 1 nativa en el enlace), DHCP Snooping y aislamiento de
   puertos en el switch.
6. Servidor en DMZ resolviendo `portal.sena.lab` e `intranet.sena.lab`, Suricata en modo IPS *inline* y dos
   *pipes*/dos *schedulers* FQ_CoDel para el límite de invitados (5 Mbit/s bajada, 2 Mbit/s subida).
7. NOC con telemetría del firewall exportada a VictoriaMetrics y visualizada en Grafana; inventario con
   Open-AudIT.

**Virtual (instrumentado con logs):** las mismas políticas y servicios, con 11 pruebas ejecutadas desde los
namespaces, incluidas capturas de troncal, matriz de firewall, aislamiento a `(self)`, NAT sin evasión,
DNSBL, IDS/IPS, NetFlow, QoS medido y persistencia tras reinicio.

## Qué se comprobó, y dónde

| Comprobación | Laboratorio virtual | Equipo físico |
|---|---|---|
| VLAN 802.1Q creadas sobre el troncal | Sí — `tcpdump` con tramas `0x8100` (vlan 10/20/30/40) | Sí — 4 VLAN definidas sobre `xl0` en el respaldo del equipo |
| DHCP por segmento | Sí — 4 leases válidos, 0 % de pérdida | Sí — Dnsmasq activo en las 4 interfaces (respaldo) |
| Política de firewall entre zonas | Sí — matriz completa ejecutada (permitido/bloqueado) | **Sí, configurada** — 21 reglas guardadas; sin log de tráfico adjunto |
| Bloqueo hacia el propio firewall `(self)` | Sí — timeouts en 22/443/53053 desde USERS/DMZ/GUEST | **Configurado** con reglas `*_Block_Admin_FW`; falta la prueba de tráfico |
| NAT de salida sin bypass | Sí — `pfctl -sn`, `ip route get`, `traceroute -I` | Nat `automatic` guardado; falta medición |
| Unbound + DNSBL | Sí — `portal.sena.lab` → 10.50.30.10, bloqueos → 0.0.0.0 | Servidor DMZ resuelto por el firewall según el informe final |
| Suricata IDS/IPS | Sí — `sid:9900001` (alerta) y `sid:9900002` (drop, `curl exit=28`) | Configurado según el informe final; **sin captura de `eve.json` del físico** |
| QoS FQ_CoDel | Sí — GUEST 4.56/1.69 Mbit/s vs ADMIN 7.77/15.78 Mbit/s | Pipes/schedulers guardados; medición solo reportada en texto |
| Persistencia tras reinicio | Sí — 169 reglas PF, servicios y namespaces recuperados | Respaldo del equipo y `vrpcfg.zip` en flash del switch |
| Troncal contra switch Huawei | No aplica | Sí — S5731-H48P4XC, trunk `GE0/0/2` + `Vlanif10` |

## Evidencia que falta del montaje físico

Estas pruebas aparecen **afirmadas** en el informe final, pero no encontré el archivo que las respalde en el
PC ni en Drive. No se les atribuye ningún resultado del entorno virtual:

1. **Aislamiento entre VLANs en el equipo físico:** falta salida de `ping`/`nc`/`curl` ejecutada desde el
   puesto Windows (VLAN 20) y desde GUEST contra ADMIN/DMZ y contra las IP del firewall.
2. **Registro `filterlog` del bloqueo `(self)`** en `/var/log/filter/` del equipo físico.
3. **Captura de `eve.json` del IPS** en el OPNsense físico (evento `drop` de Suricata).
4. **Medición de ancho de banda** en el físico (`ipfw queue show` y prueba de velocidad): el informe cita
   4.8 Mbit/s en VLAN 40, sin captura adjunta.
5. **Configuración del switch Huawei:** no hay `display current-configuration` ni captura de la sesión de
   consola; solo el `vrpcfg.zip` mencionado en el informe.

Si aparecen, se incorporan como evidencias `10`–`14` sin tocar nada de lo ya publicado.

## Evidencias publicadas

**Físico**

1. [`evidencias/05_fisico_config_resumen.md`](evidencias/05_fisico_config_resumen.md) — extracto de la
   configuración real del equipo: interfaces, 4 VLAN sobre `xl0`, 21 reglas y fechas de guardado.
2. [`evidencias/06_fisico_informe_montaje.md`](evidencias/06_fisico_informe_montaje.md) — informe técnico
   final del despliegue (copia depurada): inventario, matriz de políticas y resultados reportados.
3. [`evidencias/07_fisico_dashboard.png`](evidencias/07_fisico_dashboard.png),
   [`08_fisico_dhcp_leases.png`](evidencias/08_fisico_dhcp_leases.png) y
   [`09_fisico_firewall_live_view.png`](evidencias/09_fisico_firewall_live_view.png) — capturas del WebGUI
   del **equipo físico** (11-nov-2026): sistema en bare metal (Intel i5-1335U, enlaces 1000BaseT), leases
   DHCP y vista en vivo del filtro. Se tapó el usuario de sesión.

**Laboratorio virtual**

4. [`evidencias/01_topologia_y_direccionamiento.md`](evidencias/01_topologia_y_direccionamiento.md) —
   diagrama, direccionamiento y políticas comprobadas.
5. [`evidencias/02_matriz_firewall_entre_zonas.log`](evidencias/02_matriz_firewall_entre_zonas.log) —
   prueba 03: permisos y bloqueos entre zonas, con extracto del `filterlog`.
6. [`evidencias/03_aislamiento_self_y_qos_fqcodel.log`](evidencias/03_aislamiento_self_y_qos_fqcodel.log) —
   prueba 10: aislamiento a `(self)` y ancho de banda real medido.
7. [`evidencias/04_virtual_dashboard_segmentacion.png`](evidencias/04_virtual_dashboard_segmentacion.png) —
   panel del laboratorio virtual con las cuatro VLAN activas.

## Fechas

| Hito | Fecha |
|---|---|
| Instalación y capturas del equipo físico (estado base) | 11 de noviembre de 2026 |
| VLAN, DHCP y reglas guardadas en el equipo físico | 12 de noviembre de 2026, 21:22–22:36 |
| Pruebas instrumentadas del laboratorio virtual (logs 01–11) | 12 y 13 de noviembre de 2026 |
| Informe técnico final del despliegue | 13 de noviembre de 2026 |
| Publicación en este repositorio | 17 de noviembre de 2026 |

## Límites declarados

- El respaldo del equipo físico demuestra **configuración guardada**, no tráfico observado; por eso la
  columna "físico" de la tabla distingue «configurado» de «comprobado con log».
- WireGuard quedó configurada y **sin túnel** (peer de documentación RFC 5737, 0 bytes recibidos).
- No se publica el XML del firewall (certificado con clave privada, hash de `root`, claves de WireGuard).
- Las direcciones `10.50.x.x`, `192.168.x.x` y el dominio `sena.lab` son del entorno de laboratorio.
