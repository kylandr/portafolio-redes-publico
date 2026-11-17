# Anexo técnico — Instalación y configuración de GLPI 11 en Debian 13 (Trixie)

**Autor:** Andrés Stiven Polanía Sánchez
**Entorno del laboratorio:** Debian 13 "Trixie" (rama testing en el momento del montaje), máquina virtual
**Documento original:** anexo técnico publicado junto al tutorial en video (ver `03_ficha_tutorial_video.md`)
**Depuración aplicada a esta copia:** se reemplazó la contraseña de la base de datos por un marcador
(`<CAMBIAR_ESTA_CLAVE>`) y se normalizó el formato. Los comandos y la secuencia son los del anexo original.

---

## Fase 1 — Configuración de red (IP estática)

Antes de la instalación se fija una dirección para evitar conflictos de acceso al servicio.

```text
# /etc/network/interfaces
auto enp0s3
iface enp0s3 inet static
    address 192.168.1.50
    netmask 255.255.255.0
    gateway 192.168.1.1
```

```text
# /etc/resolv.conf  (necesario para descargar paquetes)
nameserver 8.8.8.8
```

```bash
ip addr flush dev enp0s3 && systemctl restart networking
```

## Fase 2 — Stack LAMP y GLPI

```bash
# 1. Actualización del sistema
sudo apt update && sudo apt upgrade -y

# 2. Dependencias: Apache, MariaDB y PHP 8.4
sudo apt install -y apache2 mariadb-server php8.4 php8.4-fpm php8.4-mysql php8.4-gd \
    php8.4-intl php8.4-curl php8.4-mbstring php8.4-xml php8.4-bz2 php8.4-zip \
    php8.4-bcmath php8.4-ldap php8.4-opcache wget curl unzip

# 3. Base de datos
sudo mysql -u root
```

```sql
CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'admin'@'localhost' IDENTIFIED BY '<CAMBIAR_ESTA_CLAVE>';
GRANT ALL PRIVILEGES ON glpidb.* TO 'admin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

```bash
# 4. Descarga y descompresión de GLPI 11.0.0
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/11.0.0/glpi-11.0.0.tgz
tar -xvzf glpi-11.0.0.tgz
sudo mv glpi /var/www/html/

# 5. Permisos (paso crítico para que la aplicación funcione)
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
```

## Fase 3 — Servidor web (Virtual Host)

```apache
# /etc/apache2/sites-available/glpi.conf
<VirtualHost *:80>
    ServerName glpi.local
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Require all granted
        AllowOverride All
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/glpi_error.log
    CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
```

```bash
sudo a2enconf php8.4-fpm
sudo a2enmod rewrite
sudo a2ensite glpi.conf
sudo a2dissite 000-default.conf
sudo a2enmod proxy_fcgi setenvif
sudo systemctl restart apache2
```

---

## Notas de lo que este anexo demuestra (y lo que no)

- **Demuestra:** la secuencia completa de despliegue de un servidor GLPI 11 sobre Debian 13 con Apache2 +
  MariaDB + PHP 8.4-FPM, incluida la capa de permisos y el *virtual host* con `DocumentRoot` apuntando a
  `glpi/public` (requisito de GLPI 10 y 11).
- **No demuestra por sí solo:** el resultado final en pantalla. Eso está en el video del tutorial.
- Los datos de acceso (`admin` / contraseña) son los del laboratorio y **no** se reutilizan en ningún
  servicio real.
