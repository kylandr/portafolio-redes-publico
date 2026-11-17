# Extracto de la configuración del OPNsense físico (SENA)

**Equipo:** OPNsense 26.7 (FreeBSD 15.1) sobre hardware físico — NIC `re0` (WAN) y `xl0` (trunk de VLANs)
**Respaldo del que sale este extracto:** `08_config-OPNsense-SENA-FISICO-APLICADO-COMPLETO.xml`
**Herramienta:** System → Configuration → Backups del propio equipo
**Publicado aquí:** solo los datos de arquitectura (interfaces, VLAN, reglas, fechas de guardado)

> El archivo XML completo **no se publica**: contiene el certificado del WebGUI con su clave privada,
> el hash de la contraseña de `root` y las claves de WireGuard. Este documento es la parte publicable.

## Identidad del equipo y fechas de configuración

| Dato | Valor |
|---|---|
| Hostname / dominio | `OPNsense-SENA.sena.lab` |
| Versión | OPNsense 26.7-amd64 / FreeBSD 15.1-RELEASE-p1 |
| VLANs guardadas | `2026-11-12 21:22` |
| DHCP (Dnsmasq) guardado | `2026-11-12 21:23` |
| Reglas de firewall guardadas | `2026-11-12 22:36` |

## Interfaces

| Interfaz OPNsense | Dispositivo | Dirección | Descripción |
|---|---|---|---|
| `wan` | `re0` | DHCP | Salida a Internet |
| `lan` | `xl0` | 192.168.1.1/24 | Interfaz base del trunk (gestión/administración) |
| `opt1` | `vlan01` (tag 10 sobre `xl0`) | 10.50.10.1/24 | ADMIN |
| `opt2` | `vlan02` (tag 20 sobre `xl0`) | 10.50.20.1/24 | USERS |
| `opt3` | `vlan03` (tag 30 sobre `xl0`) | 10.50.30.1/24 | DMZ |
| `opt4` | `vlan04` (tag 40 sobre `xl0`) | 10.50.40.1/24 | GUEST |

Las cuatro VLAN están definidas como `802.1q` sobre el padre `xl0` (`VLAN10_ADMIN`, `VLAN20_USERS`,
`VLAN30_DMZ`, `VLAN40_GUEST`), que es exactamente el troncal que se levanta contra el switch Huawei.

## Reglas de firewall guardadas en el equipo físico (21 reglas)

| Zona | Acción | Regla |
|---|---|---|
| LAN (base) | `pass` | Default allow LAN to any rule |
| LAN (base) | `pass` | Default allow LAN IPv6 to any rule |
| ADMIN | `pass` | ADMIN_Allow_All |
| USERS | `pass` | USERS_Allow_DNS_GW |
| USERS | `pass` | USERS_Allow_ICMP_GW |
| USERS | `pass` | USERS_Allow_DMZ_Web |
| USERS | `pass` | USERS_Allow_DMZ_ICMP |
| USERS | `block` | USERS_Block_Admin_FW |
| USERS | `block` | USERS_Block_Internal_Zones |
| USERS | `pass` | USERS_Allow_Internet |
| DMZ | `pass` | DMZ_Allow_DNS_GW |
| DMZ | `pass` | DMZ_Allow_ICMP_GW |
| DMZ | `block` | DMZ_Block_Lateral_Movement |
| DMZ | `pass` | DMZ_Allow_Internet |
| GUEST | `pass` | GUEST_Allow_DNS_GW |
| GUEST | `pass` | GUEST_Allow_ICMP_GW |
| GUEST | `block` | GUEST_Block_Internal_Networks |
| GUEST | `pass` | GUEST_Allow_Internet_Web |
| GUEST | `pass` | GUEST_Allow_Internet_ICMP |
| DMZ | `block` | DMZ_Block_Admin_FW |
| GUEST | `block` | GUEST_Block_Admin_FW |

## Otros elementos de la configuración

- **NAT:** modo `automatic` (outbound), sin reglas manuales añadidas.
- **Dnsmasq:** activo, escuchando en `lan,opt1,opt2,opt3,opt4`, puerto 53053, dominio `sena.lab`.
- **Traffic Shaper:** 2 *pipes* y 2 *schedulers* FQ_CoDel (los del límite 5M/2M de GUEST).
- **Certificado WebGUI:** presente (por eso el XML queda fuera del repositorio).

## Qué prueba este documento

1. El OPNsense **físico** tiene las cuatro VLAN creadas sobre `xl0` —no en un adaptador virtual— con los
   gateways `10.50.X.1`.
2. La política de segmentación se aplicó en el equipo físico: 21 reglas guardadas, 15 `pass` y 6 `block`,
   con las reglas de bloqueo lateral (`USERS_Block_Admin_FW`, `DMZ_Block_Lateral_Movement`,
   `GUEST_Block_Internal_Networks`, `DMZ_Block_Admin_FW`, `GUEST_Block_Admin_FW`).
3. Las fechas de guardado (12-nov-2026, 21:22 a 22:36) sitúan la aplicación en el equipo físico esa noche.

## Qué NO prueba

- No prueba que se ejecutaran *pings*/pruebas de aislamiento en el equipo físico: el respaldo demuestra
  configuración guardada, no tráfico observado. Para eso hacen falta capturas o logs del día del montaje
  (ver la sección "Evidencia que falta" del README del proyecto).
