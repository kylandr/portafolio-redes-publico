# Seguridad de la información — endurecimiento, VPN, cifrado y plan

Cuatro trabajos individuales sobre el mismo hilo: proteger información confidencial en una red corporativa.

| Trabajo | Qué demuestra | Fecha | Evidencia |
|---|---|---|---|
| [Túnel VPN site-to-site (OpenVPN)](evidencias/05_ficha_vpn_site_to_site.md) | PKI propia con Easy-RSA, AES-256-GCM, rutas entre sedes y auditoría con tshark de que ya no hay texto plano | nov-2026 | ficha |
| [Cifrado en reposo (VeraCrypt, EFS, OpenSSL)](evidencias/06_ficha_cifrado_data_at_rest.md) | Volumen cifrado, EFS y cifrado de archivos, con prueba de acceso denegado a un segundo usuario | nov-2026 | ficha |
| [Hardening de red (FinanTech)](evidencias/03_ficha_hardening_finantech.md) | Rediseño tras un escaneo lateral: DMZ, ACLs y aislamiento del área contable | oct-2026 | ficha |
| [Plan de seguridad (Comercializadora XYZ)](evidencias/04_ficha_plan_seguridad_xyz.md) | Riesgos priorizados y controles con responsables sobre infraestructura mixta Windows/Linux/pfSense | ago-2026 | ficha |

## Mi aporte

El hardening y el plan de seguridad quedan como ficha porque su valor está en el criterio y el plan, no en un
procedimiento paso a paso. Los dos informes de la línea criptográfica (VPN y cifrado) quedan **solo como
ficha**: ambos declaran «tres roles operativos» sin nombrar coautores, así que su autoría no se puede afirmar
desde el documento. Se reincorporarán los PDF cuando el autor la confirme.

## Alcance

- Todo en laboratorio virtual (VirtualBox), sin infraestructura real ni ISP intermedio.
- No hay pruebas de penetración: las pruebas son negativas controladas (certificado inválido, usuario sin
  permisos).
- Las claves, certificados y contraseñas de los laboratorios no se publican.
