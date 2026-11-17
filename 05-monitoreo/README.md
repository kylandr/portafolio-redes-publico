# Monitoreo — Zabbix, SNMP y análisis de flujo

Lo que un administrador necesita para dejar de apagar incendios: inventario, métricas, alertas y registro
centralizado.

| Trabajo | Qué demuestra | Autoría | Fecha | Evidencia |
|---|---|---|---|---|
| **[Zabbix 7.0 + rsyslog](evidencias/01_ficha_zabbix.md)** | Servidor de monitoreo completo: agentes y SNMP, integración de pfSense, tableros, alertas y tres simulaciones de incidentes | grupal (A.S.P.S., D.F.R.C.) | 2026 | ficha |
| [SNMP sobre Windows](evidencias/02_ficha_snmp.md) | Consulta de CPU, RAM, discos y software instalado con `snmpget`/`snmpwalk` | individual | abr-2026 | ficha |
| [ntopng en pfSense](evidencias/03_ficha_ntopng.md) | Clasificación de tráfico por protocolo y aplicación, consumo por host | grupal (A.S.P.S., D.F.R.C.) | 2026 | ficha |

## Zabbix — el trabajo con más peso

- **Escenario:** *TecnoSoluciones S.A.S.* ya tiene pfSense, pero no detecta caídas de servidores ni tiene
  histórico del comportamiento de la infraestructura.
- **Servidor:** Zabbix 7.0.28 desde el repositorio oficial para Debian 12, con MariaDB y frontend web.
- **Recolección:** agentes Zabbix en los equipos y SNMP contra pfSense; plantillas y grupos de hosts.
- **Registro centralizado:** servidor `rsyslog` en Debian 12 recibiendo los logs de la infraestructura.
- **Paneles:** CPU, memoria, disco, tráfico de pfSense, equipos fuera de línea y alertas activas.
- **Simulación de incidentes:** tres escenarios, incluido apagar un servidor y medir cómo se detecta la
  caída y cuánto tarda la alerta.
- **Reparto de roles:** infraestructura (máquinas virtuales en Proxmox), monitoreo (Zabbix, hosts, tableros)
  y operaciones (SNMP, pruebas, documentación).

## Mi aporte

En Zabbix y en ntopng el trabajo fue en pareja (A.S.P.S. y D.F.R.C.). El de SNMP es individual.

## Alcance

- Todo el laboratorio es virtual (Debian 12 bajo Proxmox), con pfSense como equipo monitoreado.
- Las simulaciones de incidentes son controladas (apagar una VM), no fallos reales de infraestructura.
