# Ficha — Zabbix 7.0 para el monitoreo integral de una red empresarial

**Documento fuente:** informe de laboratorio del proyecto (26 páginas)
**Autoría:** grupal — A.S.P.S. y D.F.R.C.
**Instructor:** Juan Carlos Sanabria A.
**Ficha de formación:** 3171046 — SENA Sede Industrial, Neiva, Huila
**Fecha del documento:** 2026 (el laboratorio se ejecutó el 30 de julio de 2026 según los registros)

## Qué es

Implementación de un servidor de monitoreo centralizado con **Zabbix 7.0.28** sobre Debian 12, integrado con
el firewall pfSense de la empresa del caso (*TecnoSoluciones S.A.S.*) y complementado con un servidor de
registros centralizado (**rsyslog**). El problema de partida: el firewall controla el tráfico, pero nadie
detecta a tiempo la caída de un servidor ni tiene histórico del comportamiento de la infraestructura.

## Objetivo

Implementar Zabbix, configurar el monitoreo por agentes y por SNMP, integrar pfSense, construir paneles de
supervisión, definir alertas ante eventos críticos y analizar los indicadores de rendimiento obtenidos.

## Entorno

| Elemento | Detalle |
|---|---|
| Servidor de monitoreo | Zabbix 7.0.28 (repositorio oficial para Debian 12), frontend web y base de datos MariaDB |
| Equipo monitoreado | pfSense (por SNMP) y servidores/estaciones con agente Zabbix |
| Registro centralizado | rsyslog en Debian 12, recibiendo logs de la infraestructura (incluido el DHCP Kea de pfSense) |
| Virtualización | Máquinas virtuales del laboratorio (Proxmox) |
| Paneles | CPU, memoria, disco, tráfico de pfSense, equipos fuera de línea y alertas activas |

## Qué se hizo

1. Instalación del repositorio y paquetes de Zabbix 7.0 para Debian 12 (`wget`, `dpkg -i zabbix-release…`,
   `apt update`), base de datos MariaDB y asistente web de configuración.
2. Creación de hosts y grupos, y despliegue de **agentes** en los equipos a monitorear.
3. Configuración de **SNMP en pfSense** y de los ítems correspondientes para leer interfaces y tráfico.
4. Instalación de **rsyslog** como servidor de logs centralizado y verificación de la recepción de eventos.
5. Construcción del tablero global: estado del servidor Zabbix, equipos habilitados, plantillas, métricas,
   iniciadores en problema y disponibilidad por equipo.
6. **Tres simulaciones de incidentes**, entre ellas apagar un servidor para medir cuánto tarda en detectarse
   la caída y cuánto tarda en dispararse la alerta según el intervalo de sondeo configurado.
7. Análisis de los indicadores obtenidos y conclusiones sobre el paso de una gestión reactiva a una proactiva.

## Resultados comprobados

- Servidor Zabbix 7.0.28 en marcha y accesible por su interfaz web, con base de datos MariaDB conectada.
- Inventario visible en el tablero: 6 equipos habilitados, 356 plantillas disponibles, 838 métricas
  (792 habilitadas) y 414 iniciadores con 1 en estado de problema.
- pfSense monitoreado por SNMP y por agente: tráfico de interfaces y métricas del sistema en los paneles.
- Logs de la infraestructura recibidos en el servidor rsyslog (se ven eventos del DHCP Kea de pfSense).
- Detección de incidentes simulados: al apagar un equipo, el estado cambia y se genera el evento; el tiempo
  depende del intervalo de sondeo del host.

## Límites y lo que no está demostrado

- Las alertas se evaluaron con simulaciones controladas (apagar una VM), no con fallos de producción.
- No hay monitoreo de una red real de empresa ni integración con un sistema de notificación externo
  (correo/Telegram) documentada con evidencias.
- El reparto de trabajo fue por roles (infraestructura, monitoreo y operaciones), sin métricas de esfuerzo.

## Reparto del trabajo

El informe define tres roles: administrador de infraestructura (máquinas virtuales en Proxmox), administrador
de monitoreo (Zabbix, hosts, plantillas y tableros) y analista de operaciones (SNMP en pfSense, pruebas y
documentación de incidentes). El documento no detalla qué integrante asumió cada rol.
