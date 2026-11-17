# Ficha — Hardening de red y control de acceso (FinanTech Solutions)

**Documento fuente:** `Informe_Laboratorio_Hardening_FinanTech.docx` (~1.800 palabras)
**Autoría:** individual — A.S.P.S.
**Fecha del documento:** 16 de octubre de 2026

## Qué es

Informe técnico y manual de configuración de un endurecimiento de red tras un incidente: un escaneo lateral
no autorizado lanzado desde un servidor público hacia la red contable de la empresa.

## Objetivo

Reestructurar la red corporativa para aislar las áreas críticas, publicar solo lo necesario en una DMZ y
aplicar políticas de acceso estrictas por segmento.

## Entorno

- Caso: *FinanTech Solutions* (área contable y servidores públicos).
- Firewall/segmentación según el diseño del curso, con DMZ y ACLs por zona.
- Práctica acompañada de archivos de Packet Tracer (`Laboratorio-Hardening1.pka` y `2.pka`, conservados en
  local, no publicados por contener credenciales de dispositivos).

## Qué se hizo

1. Análisis del incidente y de la superficie de exposición (qué servidor era alcanzable desde dónde).
2. Rediseño de la topología: separación de la red contable y creación de una DMZ para los servicios públicos.
3. Definición de políticas de acceso entre zonas y aplicación de listas de control de acceso.
4. Endurecimiento de los servicios expuestos (solo los puertos necesarios) y de los dispositivos de
   interconexión.
5. Verificación del aislamiento resultante entre el segmento público y la red crítica.

## Resultados comprobados

- La red contable deja de ser alcanzable desde el segmento público; solo se permite el tráfico a los puertos
  publicados en la DMZ.
- Las ACLs aplicadas bloquean el movimiento lateral que originó el incidente.
- El documento deja la matriz de políticas y los comandos de configuración como manual reproducible.

## Límites y lo que no está demostrado

- Es un ejercicio sobre caso y topología de laboratorio; no hay medición de impacto en una red productiva.
- Las pruebas son funcionales (tráfico permitido/bloqueado), no una auditoría formal con herramientas de
  escaneo.
