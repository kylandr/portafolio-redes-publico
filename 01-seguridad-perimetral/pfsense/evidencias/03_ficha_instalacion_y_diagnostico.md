# Ficha — Instalación, servicios y diagnóstico de pfSense

**Documento fuente:** `INFORME PFSENSE.docx` (30 páginas)
**Autoría:** individual — A.S.P.S.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 30 de julio de 2026

## Qué es

Informe de la instalación de pfSense en máquina virtual y del recorrido completo de la interfaz de
administración: dashboard, interfaces, servicios de red y herramientas de diagnóstico.

## Objetivo

Dejar documentado el despliegue del firewall y aprender a operar sus módulos diarios (interfaces, DHCP,
reglas, estado y diagnóstico) antes de entrar en segmentación y filtrado.

## Entorno

- pfSense sobre VM (disco virtual, NICs virtuales `vtnet0`/`vtnet1`).
- WAN por DHCP del entorno virtual; LAN como red interna de administración.
- Consola web en el puerto de gestión, acceso por navegador.

## Qué se hizo

1. Instalación desde ISO: selección de disco, particionado ZFS/UFS y arranque del instalador.
2. Asignación de interfaces (WAN/LAN) y configuración inicial de IP.
3. Recorrido del dashboard: versión, CPU/RAM, estado de interfaces y gateways.
4. Configuración de DHCP para la LAN y revisión del cambio a **Kea DHCP** (alerta del sistema que se
   interpretó y se dejó documentada).
5. Creación de reglas de firewall básicas y revisión de la tabla de estados.
6. Diagnóstico: `ping`, `traceroute`, revisión de logs y estados desde `Diagnostics`.

## Resultados comprobados

- Instalación finalizada y sistema operativo en marcha con las interfaces reconocidas correctamente.
- Servicio DHCP entregando direccionamiento en la LAN.
- Pruebas de conectividad y trazado de ruta ejecutadas desde `Diagnostics`, con auditoría de logs.
- Reglas aplicadas y verificadas contra la tabla de estados.

## Límites y lo que no está demostrado

- Es un informe de instalación y operación: no hay segmentación por VLAN ni filtrado de contenido.
- No incluye mediciones de rendimiento ni pruebas de carga.
