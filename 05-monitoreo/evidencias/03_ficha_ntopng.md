# Ficha — Análisis de flujo en tiempo real con ntopng

**Documento fuente:** `plantilla_actividad_3_ntopng.docx` (Actividad 3 de la ficha 3171046)
**Autoría:** grupal — A.S.P.S. y D.F.R.C.
**Asignatura:** Gestión de Redes de Datos
**Fecha del documento:** 2026 (sin día registrado)

## Qué es

Informe de la configuración de `ntopng` como paquete de pfSense para clasificar el tráfico de la red de
pruebas por protocolo y aplicación, y detectar anomalías de consumo.

## Objetivo

Dar visibilidad al tráfico: identificar qué host consume ancho de banda, clasificar los flujos (HTTP, DNS,
etc.) e interpretar los gráficos para detectar comportamientos anómalos.

## Entorno

- pfSense con el paquete `ntopng` instalado.
- Interfaz de administración de ntopng (HTTP/HTTPS) con arranque automático de `redis` y del demonio.
- Interfaces LAN y WAN seleccionadas para la captura.
- Red de pruebas del laboratorio del curso.

## Qué se hizo

1. Instalación y configuración general de ntopng: interfaces a monitorear, puerto de la interfaz web y
   arranque automático del servicio junto con Redis.
2. Navegación por los paneles de flujos, hosts y protocolos.
3. Clasificación del tráfico por protocolo y por aplicación.
4. Interpretación de los gráficos de consumo por host.
5. Revisión de alertas y gráficos de comportamiento para detección de anomalías.

## Resultados comprobados

- Tráfico clasificado por protocolo y aplicación en la interfaz de ntopng (figuras del informe original).
- Identificación de los hosts con mayor consumo por interfaz monitoreada.
- Alertas y gráficos de comportamiento revisados e interpretados contra los requerimientos de la actividad.

## Límites

- La captura se hizo sobre la red de pruebas del curso, no sobre tráfico de producción.
- No se documentan umbrales ni alertas configuradas a medida: se usan las del propio ntopng.
- No hay medición de rendimiento del firewall con ntopng activo.
