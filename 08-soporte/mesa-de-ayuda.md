# Mesa de ayuda (helpdesk) — diseño del proceso

**Documento fuente:** `PROPUESTA DE DISEÑO E IMPLEMENTACIÓN DE MESA DE AYUDA.pdf`
**Autoría:** grupal — J.S.P., J.H.G. y A.S.P.S.
**Fecha del documento:** 2026

## Qué es

Propuesta de diseño de una mesa de ayuda: cómo entra un caso, quién lo atiende, cómo se escala y cómo se
mide el cumplimiento del SLA.

## Objetivo

Pasar de "el técnico que todos buscan en el pasillo" a un proceso con registro, prioridad y seguimiento.

## Qué se hizo

1. **Fase 1 — Análisis del problema:** síntomas de la ausencia de mesa de ayuda (casos sin registrar, sin
   prioridad, sin histórico).
2. **Fase 2 — Flujo de atención:** canales de entrada, clasificación y asignación al técnico.
3. **Gestión de casos:** seguimiento con hoja de control revisada dos veces al día (mañana y tarde) por el
   analista administrativo, con aviso al técnico cuando un ticket está por vencer según su SLA.
4. **Escalamiento:** si el técnico de helpdesk pasa más de una hora sin resolver, el caso sube al siguiente
   nivel con el diagnóstico ya documentado.
5. Definición de prioridades y tiempos de respuesta por tipo de caso.

## Resultados

- Proceso documentado con responsables por etapa y criterios de escalamiento medibles.
- Base para implementarlo sobre una herramienta real (GLPI, ver el proyecto de servidores).

## Límites

- Es un diseño de proceso: no incluye la herramienta configurada ni métricas históricas de operación.
