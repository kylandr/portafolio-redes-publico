# Proyecto 03 — Redes IoT en Cisco Packet Tracer (proyecto grupal, semillero REDCIES)

## Objetivo del equipo

Analizar cómo se comportan las redes IoT en un entorno industrial simulado: construir dos escenarios en
Cisco Packet Tracer (una red WAN empresa–planta y una LAN segmentada con atacante), verificar conectividad y
segmentación, e identificar vulnerabilidades con sus medidas de mitigación.

## Mi aporte (delimitado a lo que puedo confirmar)

- Integrante del semillero REDCIES y **coautor del informe grupal**, según la portada del documento (cuatro
  aprendices de la ficha 3171046, bajo orientación del instructor Edgar Eduardo Olarte Cruz).
- **Lo que no puedo confirmar:** el informe no separa aportes por persona, así que no me atribuyo secciones,
  pruebas ni configuraciones concretas. Los resultados se presentan como **del equipo**.
- Este resumen y los diagramas redibujados sí son míos.

## Tecnologías

Cisco Packet Tracer · routers Cisco 2911 (subinterfaces, NAT, DHCP, OSPF) · switches Cisco 2960-24T/24TT con
VLAN y `shutdown` de puertos · AP industrial DLC100 (WPA2-PSK, DHCP) · router inalámbrico WRT300N ·
`Cloud-PT` con enlace serial · servidor de servicios IoT.

## Cómo se comprobó el resultado (equipo)

- **25 pruebas documentadas con evidencia en pantalla:** 11 en el escenario WAN (pings cruzados entre router
  industrial/empresarial, servidor, PC0, PC1, AP industrial y teléfonos; gestión de IoT desde servidor y
  dispositivos móviles) y 14 en el escenario LAN híbrido (pings PC1↔PC2, PC1|PC2↔servidor, impresora y
  router, y gestión de IoT desde laptop y PC2).
- Tabla de direccionamiento por red, tabla de enrutamiento y matriz de interfaces deshabilitadas.
- Análisis comparativo router doméstico vs. AP industrial y conclusiones sobre las brechas detectadas.

## Evidencias

1. [`evidencias/01_resumen_proyecto_grupal.md`](evidencias/01_resumen_proyecto_grupal.md) — resumen del
   informe (referencia principal: `Informe Semillero de Investigacion_V13.pdf`), con identificación,
   objetivos, resultados del equipo y hallazgos.
2. [`evidencias/02_escenarios_topologia.md`](evidencias/02_escenarios_topologia.md) — diagramas de los dos
   escenarios (redibujados) y la matriz de endurecimiento.

## Fechas

| Hito | Fecha |
|---|---|
| Periodo del trabajo del semillero | 2025 (el informe está fechado "Neiva, 2025" y cita material consultado el 20-11-2025) |
| Socialización de resultados | 16 de diciembre de 2025 |
| Publicación del documento grupal (PDF V13) | 17 de febrero de 2026 |
| Redacción de este resumen para el portafolio | 16 de noviembre de 2026 |

## Sobre el material original

El informe grupal (23 páginas, V13) no se publica aquí: es obra de cuatro autores y contiene material
institucional del SENA. Tampoco se incluyen capturas del documento; en su lugar hay diagramas redibujados y
un resumen con crédito al equipo y al instructor. Los compañeros aparecen con iniciales mientras se confirma
si quieren figurar con nombre completo.
