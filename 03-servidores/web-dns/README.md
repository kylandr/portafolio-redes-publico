# Servidor web y DNS en una red segmentada con VLAN

**Documento fuente:** `MANUAL DE CONFIGURACION DE RED LOCAL Y SERVIDOR WEB Y DNS.pdf`
**Autoría:** grupal — J.S.P.H., L.D.P.A., A.S.P.S., J.J.R.M. y J.A.C.R.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 2026

## Qué es

Manual de configuración de una red local segmentada con VLANs que da servicio a un servidor web interno
publicado como `gugle.net`, con resolución DNS propia.

## Objetivo

Integrar en un solo diseño conmutación, enrutamiento, servicios y publicación interna: que un nombre
resuelva y sirva contenido desde una red segmentada.

## Entorno

| Elemento | Detalle |
|---|---|
| Enrutamiento | Routers Cisco 2911 |
| Conmutación | Switches Catalyst 2960 con VLANs |
| Inalámbrico | Access point TP-Link |
| Servidor | Servidor web y DNS interno para `gugle.net` |
| Documentación | Manual paso a paso con diagramas de la topología |

## Qué se hizo

1. Diseño de la red local con segmentación por VLAN.
2. Configuración de switches Catalyst 2960 (VLANs y puertos) y del router 2911 (gateway por segmento).
3. Integración del access point TP-Link a la red segmentada.
4. Configuración del servidor web y del servicio DNS para `gugle.net`.
5. Verificación de resolución de nombre y acceso al sitio desde los clientes.

## Resultados comprobados

- El nombre `gugle.net` resuelve por el DNS interno y el sitio se sirve a los clientes de la red.
- La topología funciona con segmentación por VLAN y enrutamiento entre ellas.

## Límites

- Entorno de laboratorio del centro de formación; no se publica documentación de la red real del SENA.
- No hay medición de rendimiento ni endurecimiento del servidor web.
