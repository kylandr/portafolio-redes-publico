# Ficha — Instalación de GLPI 11 sobre Debian 13 (informe de laboratorio)

**Documento fuente:** `INFORME GLPI.pdf` (13 páginas)
**Autoría:** individual — A.S.P.S.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 6 de mayo de 2026

## Qué es

Informe de la instalación de GLPI 11 (gestión de activos y mesa de servicio) sobre Debian 13, con la guía
paso a paso, las respuestas al cuestionario del instructor y el análisis final.

## Objetivo

Desplegar un sistema de gestión de inventario y tickets sobre Linux, dejando documentado cada paso para que
otra persona pueda reproducirlo.

## Entorno

- Debian 13 (Trixie) en máquina virtual.
- Servidor web y PHP del stack de GLPI, base de datos MariaDB.
- GLPI 11 descargado del sitio oficial e instalado por navegador.

## Qué se hizo

1. Actualización del sistema e instalación de dependencias (Apache, PHP con sus extensiones, MariaDB).
2. Creación de la base de datos y del usuario de GLPI.
3. Despliegue del código de GLPI en el directorio del servidor web y ajuste de permisos.
4. Asistente de instalación web: verificación de requisitos, conexión a la base de datos y creación del
   usuario administrador.
5. Primer inicio de sesión, revisión del inventario y de los módulos de asistencia.
6. Respuestas al cuestionario del instructor y análisis crítico del proceso.

## Resultados comprobados

- GLPI accesible desde el navegador con el asistente de instalación completado y la base de datos conectada.
- Inventario y módulo de tickets disponibles tras el primer ingreso.
- Guía reproducible con los comandos y las pantallas de cada paso.

## Límites

- No incluye agentes desplegados en estaciones: el inventario se carga a mano en este primer laboratorio.
- No hay integración con correo ni con directorio (LDAP/AD) en este informe.
