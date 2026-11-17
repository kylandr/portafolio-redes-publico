# Ficha — Túnel VPN site-to-site con OpenVPN y PKI propia

**Documento fuente:** `Informe_Laboratorio_VPN_Distribuciones_XYZ.pdf` — **no publicado** (ver nota de autoría)
**Autoría: PENDIENTE.** La portada solo nombra a A.S.P.S., pero el cuerpo del informe describe «tres roles
operativos» del equipo (Rol A: infraestructura y enrutamiento; Rol B: criptografía y PKI; Rol C: auditoría e
inspección de tráfico). No hay forma de saber desde el documento si el trabajo fue individual con roles
simulados o en equipo, y quiénes serían los coautores. Mientras no se aclare, este trabajo **no se publica**.
**Fecha del documento:** noviembre de 2026

## Qué es

Informe técnico y manual de configuración de una VPN site-to-site entre dos sedes (Bogotá y Neiva) con
OpenVPN, PKI propia con Easy-RSA, y una auditoría forense del tráfico para demostrar que dejó de viajar en
texto claro.

## Objetivo

Cerrar la fuga de información detectada: los datos corporativos (contabilidad, nóminas, balances) viajaban
por Internet sin cifrar y eran capturables con *sniffing*.

## Entorno

| Elemento | Detalle |
|---|---|
| Virtualización | VirtualBox 7.2 sobre Debian 13 (Trixie) |
| Sede Bogotá | LAN `192.168.10.0/24`, servidor OpenVPN |
| Sede Neiva | LAN `192.168.20.0/24`, cliente |
| WAN simulada | `200.10.10.0/28` |
| Túnel | `tun0`, `10.8.0.1/24` (punto a punto) |
| Criptografía | AES-256-GCM + autenticación SHA-256, certificados emitidos con Easy-RSA |

## Qué se hizo

1. Creación de la CA y de los certificados de servidor y cliente con Easy-RSA (petición, firma, parámetros
   Diffie-Hellman).
2. Configuración del servidor OpenVPN (`server.conf`, UDP 1194, políticas por cliente con CCD/`iroute`) y
   del cliente.
3. Habilitación de `ip_forward` en ambos extremos y publicación de rutas entre LANs.
4. Auditoría con Wireshark/tshark sobre la interfaz WAN: captura del tráfico del túnel y verificación de que
   el contenido no es legible.
5. Prueba negativa: conexión con certificado inválido, rechazada por el servidor.

## Resultados comprobados

- Túnel establecido (`tun0` con IP asignada en ambos extremos) y `ping` funcionando entre LANs
  (`192.168.20.x` → `192.168.10.10`) además del propio túnel (`10.8.0.1`).
- La captura en la WAN muestra el tráfico como OpenVPN/TLS, sin texto plano legible, frente a la captura
  previa sin cifrado.
- El intento con certificado inválido queda registrado como bloqueado en el log del servidor (`journalctl`).

## Límites

- **El informe completo no está en el repositorio:** la autoría no está confirmada y además el documento
  original incluye una contraseña de ejemplo en texto plano. Se reincorporará cuando el autor confirme la
  autoría y se limpie el documento.
- No hay medición de rendimiento del túnel (throughput, latencia bajo carga).
- La WAN es simulada: no se atravesó un ISP real ni un firewall intermedio.
