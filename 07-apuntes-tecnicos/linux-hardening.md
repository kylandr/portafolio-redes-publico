> **Procedencia:** nota de referencia de mi base de conocimiento técnico, redactada con asistencia de IA
> a partir de documentación oficial (las fuentes van al final de la nota) y revisada al usarla en los
> laboratorios. **No es un registro de laboratorio**: para eso están los informes de las otras carpetas.
> Copia tomada del respaldo de la wiki del 2026-10-06; la versión viva está en mi servidor.


# Seguridad y Hardening

## TL;DR
Mejores prácticas para asegurar servidores Linux: firewall, updates, SSH, sandboxing.

## Principios
- **Least privilege**: Solo accesos necesarios
- **Defense in depth**: Múltiples capas de seguridad
- **Updates regulares**: Parchear vulnerabilidades

## Firewall
```bash
# nftables (recomendado)
nft add rule input tcp dport 22 accept
nft add rule input tcp dport 80 accept
nft add rule input counter drop

# systemctl enable nftables
```

## SSH Hardening
```bash
# /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers usuario1 usuario2
ClientAliveInterval 300
ClientAliveCountMax 2
```

## Updates
```bash
pacman -Syu
systemd-analyze security servicio
```

## Sandboxing systemd
```ini
[Service]
PrivateTmp=true
ProtectSystem=strict
ProtectHome=true
NoNewPrivileges=true
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
```

## Límites de usuario
```bash
# /etc/security/limits.conf
* soft nproc 100
* hard nproc 200
```

## Sysctl hardening
```bash
# /etc/sysctl.d/99-hardening.conf
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.accept_source_route = 0
kernel.kptr_restrict = 2
```

## Audit
```bash
# auditctl -w /etc/passwd -p wa
# ausearch -f /etc/passwd
```

## Troubleshooting
| Problema | Comando |
|----------|---------|
| Login falla | `journalctl -u sshd` |
| Firewall bloquea | `nft list ruleset` |
| Permisos | `ls -la`, `getfacl` |

## Fuentes
- [Security - ArchWiki](https://wiki.archlinux.org/title/Security)
- [Systemd Sandboxing](https://wiki.archlinux.org/title/Systemd/Sandboxing)
