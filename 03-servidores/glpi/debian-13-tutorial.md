# Proyecto 01 — Servidor GLPI 11 sobre Debian 13 (Trixie)

## Objetivo

Desplegar un servidor de **gestión de activos y mesa de ayuda (GLPI 11)** sobre Debian 13, con
direccionamiento estático, resolución de dependencias del stack PHP y publicación del servicio detrás
de un *virtual host* de Apache, dejando el procedimiento documentado en video para que otra persona
pueda reproducirlo.

## Mi aporte

Trabajo **individual**: laboratorio montado en máquina virtual, guion y grabación del tutorial, y anexo
técnico con la secuencia exacta de comandos (red, LAMP, GLPI, Apache).

## Tecnologías

| Capa | Componente |
|---|---|
| Sistema operativo | Debian 13 "Trixie" (rama testing en el momento del montaje) |
| Servidor web | Apache 2.4 + PHP 8.4-FPM (`proxy_fcgi`, `rewrite`) |
| Base de datos | MariaDB (base `glpidb`, `utf8mb4`) |
| Aplicación | GLPI 11.0.0 (paquete oficial `.tgz`, `DocumentRoot` en `glpi/public`) |

## Cómo se comprobó el resultado

1. **Red:** IP estática `192.168.1.50/24` aplicada y verificada con `ip addr` tras reiniciar `networking`,
   con salida a internet por DNS `8.8.8.8` antes de instalar paquetes.
2. **Dependencias:** instalación completa del stack PHP 8.4 (incluye `ldap`, `intl`, `zip`, `bcmath`), que
   es donde suele fallar GLPI 11 en Debian testing.
3. **Permisos:** `chown www-data` + `chmod 755` sobre `/var/www/html/glpi`; sin este paso la aplicación no
   arranca.
4. **Servicio publicado:** *virtual host* `glpi.conf` habilitado, `000-default` deshabilitado, Apache
   reiniciado y acceso al instalador web de GLPI desde la LAN.
5. **Registro en video:** el tutorial muestra la ejecución completa en pantalla, paso por paso, y el
   resultado del acceso al servicio.

## Evidencias

1. [`evidencias/02_ficha_tutorial_video.md`](evidencias/02_ficha_tutorial_video.md) — ficha del tutorial
   publicado: título, canal, URL, fecha, duración y puntos clave.
2. [`evidencias/01_anexo_tecnico_instalacion_glpi.md`](evidencias/01_anexo_tecnico_instalacion_glpi.md) —
   anexo técnico con los comandos, depurado (contraseña de laboratorio reemplazada por marcador).

## Fechas

| Hito | Fecha |
|---|---|
| Montaje del laboratorio | **no registrada** (anterior a la publicación del tutorial) |
| Publicación del tutorial | 16 de mayo de 2026 |
| Publicación en este portafolio | 16 de noviembre de 2026 |

## Límites declarados

- La fecha exacta del montaje no quedó documentada; no se infiere de las fechas de archivo.
- Los datos de acceso del laboratorio (`admin` / contraseña) no se publican y no se reutilizan en ningún
  servicio real.
