# Evidencia — Tutorial en video: "Implementación de Servidor GLPI en Debian 13 (Trixie)"

| Campo | Dato |
|---|---|
| Título | Implementación de Servidor GLPI en Debian 13 (Trixie) |
| Canal | Stiven Polania (https://www.youtube.com/@StivenPolania-lg7xo) |
| URL | https://www.youtube.com/watch?v=KdSL-XqNMoA |
| Fecha de publicación | 16 de mayo de 2026 |
| Duración | 14 min 21 s |
| Visitas al momento de armar este portafolio | 40 |
| Versión instalada en el video | GLPI 11.0.0 |
| Sistema operativo | Debian 13 (Trixie) |

## Qué documenta el video

Descripción publicada por el autor:

> Título: Informe Técnico: Instalación de GLPI en Debian 13 (Trixie).
> Este video documenta el despliegue de un servidor de gestión de activos (GLPI) en un entorno Linux.
> Se enfatiza la configuración manual de red para garantizar la estabilidad del servicio y la resolución
> de problemas de dependencias en la versión Testing de Debian.

Puntos clave que cubre:

1. **Red:** configuración de IP estática (`192.168.1.50`) y ajuste de DNS para tener salida a internet.
2. **Servicios:** implementación de Apache2, MariaDB y PHP 8.4-FPM.
3. **GLPI:** instalación de la versión 11.0.0 y configuración de *Virtual Host* para acceso remoto.

El anexo técnico con los comandos está enlazado en la descripción del video y su copia depurada está en
[`01_anexo_tecnico_instalacion_glpi.md`](01_anexo_tecnico_instalacion_glpi.md).

## Por qué esta evidencia cuenta

Es material producido íntegramente por el autor del portafolio: guion, ejecución en la máquina virtual,
narración y anexo de comandos. Cualquier persona que reclute puede ver el procedimiento completo y
reproducirlo paso a paso sin depender de una explicación mía.
