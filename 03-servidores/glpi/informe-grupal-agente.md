# Ficha — GLPI con agente de inventario en Windows (trabajo grupal)

**Documento fuente:** `INFORME ACTIVIDAD FINAL.pdf` (documento de cierre del proyecto)
**Autoría:** grupal — J.S.R.M., J.H.G., J.S.P. y A.S.P.S.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 3 de junio de 2026

## Qué es

Segundo escalón del laboratorio de GLPI: además del servidor, se despliega el **agente GLPI en Windows** para
que el inventario de las estaciones llegue solo, sin digitarlo.

## Objetivo

Cerrar el ciclo de la mesa de servicio: servidor de GLPI sobre Debian alimentado automáticamente por agentes
instalados en los equipos cliente.

## Entorno

- Servidor Debian con GLPI 11 y MariaDB (base del laboratorio anterior).
- Estaciones Windows con el agente GLPI.
- Red del laboratorio del SENA.

## Qué se hizo

1. Actualización del sistema e instalación de dependencias en el servidor.
2. Configuración de la base de datos y traslado del paquete de GLPI a Debian.
3. Instalación de GLPI desde el navegador.
4. Instalación y configuración del **agente GLPI en Windows**, apuntando al servidor.
5. Verificación del inventario recibido (equipos, hardware y software) desde el panel de GLPI.

## Resultados comprobados

- El agente reporta al servidor y el equipo aparece inventariado en GLPI con su hardware y su software.
- La instalación queda documentada paso a paso, incluida la parte de Windows.

## Límites

- Es un inventario de laboratorio, con pocas estaciones.
- No se configuran reglas de negocio, SLA ni notificaciones automáticas.
