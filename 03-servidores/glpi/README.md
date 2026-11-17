# GLPI 11 sobre Debian 13 — mesa de servicio e inventario

Tres piezas sobre el mismo despliegue, en orden de avance:

| Pieza | Qué es | Autoría | Fecha | Evidencia |
|---|---|---|---|---|
| Tutorial en video | Implementación completa de GLPI 11 sobre Debian 13, publicada en YouTube | individual | publicado 16-05-2026 | [video](https://www.youtube.com/watch?v=KdSL-XqNMoA) · [anexo técnico](debian-13-tutorial.md) |
| Informe de laboratorio | Guía de instalación paso a paso y análisis del proceso | individual | 06-05-2026 | [ficha](informe-glpi-individual.md) |
| Agente de inventario | Agente GLPI en Windows reportando al servidor | grupal (J.S.R.M., J.H.G., J.S.P., A.S.P.S.) | 03-06-2026 | [ficha](informe-grupal-agente.md) |

El video es la evidencia principal: muestra la instalación corriendo, no solo el resultado. La fecha del
montaje original no quedó registrada; la fecha del video es la de publicación (16 de mayo de 2026) y la del
informe, la de entrega.

## Mi aporte

El despliegue del servidor (video, anexo e informe) es individual. En el trabajo del agente de inventario
participé como uno de los cuatro integrantes del equipo.

## Alcance

- Laboratorio virtual, una sola instancia, sin alta disponibilidad.
- El paquete Docker que se armó en paralelo **no** se publica: quedó incompleto (MariaDB arrancaba pero el
  asistente de GLPI no llegaba a completarse). Se conserva en local como material de trabajo.
