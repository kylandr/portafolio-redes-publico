> **Procedencia:** nota de referencia de mi base de conocimiento técnico, redactada con asistencia de IA
> a partir de documentación oficial (las fuentes van al final de la nota) y revisada al usarla en los
> laboratorios. **No es un registro de laboratorio**: para eso están los informes de las otras carpetas.
> Copia tomada del respaldo de la wiki del 2026-10-06; la versión viva está en mi servidor.


# Active Directory

> **TL;DR**: Active Directory Domain Services (AD DS) es el servicio de directorio de Microsoft para gestion centralizada de usuarios, equipos y recursos en una red empresarial.

## Definicion

Active Directory Domain Services (AD DS) permite a los administradores crear y gestionar dominios, forestas y scopes de replication. Un **Domain Controller (DC)** es el servidor que ejecuta AD DS y almacena la base de datos del directorio (NTDS.dit).

### Conceptos clave

- **Forest**: Coleccion de un dominio raiz y todos sus dominios hijos
- **Domain**: Unidad organizativa con su propia base de datos de objetos
- **Organizational Unit (OU)**: Contenedor dentro de un dominio para organizar objetos
- **Global Catalog**: Repositorio de todos los objetos del bosque con atributos parciales
- **FSMO Roles**: 5 roles de Operations Master criticios para el funcionamiento del dominio

## Como funciona

### Estructura de deployment

1. **Nuevo bosque**: Primer DC en el ambiente (`New-ADForest`)
2. **Nuevo dominio hijo**: Agregar dominio a un bosque existente
3. **Nuevo domain controller**: Agregar otro DC al dominio existente

### Niveles funcionales

| Nivel | Version minima | Caracteristicas |
|-------|---------------|-----------------|
| 2003 | Windows Server 2003 | Base, sin nuevas features |
| 2008 R2 | Windows Server 2008 R2 | RODC support |
| 2012 | Windows Server 2012 | Group Policy Preferences avanzados |
| 2016 | Windows Server 2016 | AdminSDHolder protection |
| 2025 | Windows Server 2025 | Features mas recientes |

### FSMO Roles

| Role | Funcion |
|------|---------|
| Schema Master | Controla cambios en el esquema AD |
| Domain Naming Master | Controla addition/removal de dominios |
| PDC Emulator | Compatibilidad con versiones anteriores, password sync |
| RID Master | Asignacion de IDs de seguridad |
| Infrastructure Master | Actualizacion de referencias entre dominios |

## Comandos / Configuracion

### Instalar AD DS via PowerShell

```powershell
# Instalar rol AD DS
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# Promover a Domain Controller (nuevo bosque)
Install-ADDSDomainController -DomainName "corp.contoso.com" `
    -InstallDns -Credential (Get-Credential) `
    -SafeModeAdministratorPassword (ConvertTo-SecureString "<CAMBIAR_ESTA_CLAVE>" -AsPlainText -Force)

# Crear nuevo bosque
Install-ADDSDomain -NewDomainName "sub.corp.contoso.com" `
    -ParentDomainName "corp.contoso.com" `
    -CreateDnsDelegation

# Verificar installacion
Get-WindowsFeature AD-Domain-Services | Select-Object Name, Installed
```

### Gestion de usuarios y OUs

```powershell
# Crear OU
New-ADOrganizationalUnit -Name "Departamentos" -Path "DC=corp,DC=contoso,DC=com"

# Crear usuario
New-ADUser -Name "Juan Perez" -SamAccountName "jperez" `
    -UserPrincipalName "jperez@corp.contoso.com" `
    -Path "OU=Departamentos,DC=corp,DC=contoso,DC=com" `
    -Enabled $true

# Listar usuarios
Get-ADUser -Filter * -SearchBase "OU=Departamentos,DC=corp,DC=contoso,DC=com"

# Reset password
Reset-ADUserPassword -Identity "jperez" -NewPassword (Read-Host -AsSecureString)
```

### Verificar replication

```powershell
# Estado de replication
repadmin /replsummary

# Forzar replication entre DCs
repadmin /syncall /AdeP

# Verificar health del DC
dcdiag /v /c /d /e
```

## Troubleshooting

| Problema | Causa probable | Solucion |
|----------|---------------|----------|
| DC no puede replicar | DNS incorrecto | Verificar que el DC apunte a si mismo como DNS |
| FSMO roles perdidos | Fallo de hardware | Transferir roles via `ntdsutil` |
| Error de password sync | PDC Emulator caido | Verificar rol PDC con `netdom query fsmo` |
| Usuario no puede loguearse | GC no disponible | Verificar Global Catalog en el sitio correcto |
| Kerberos falla | Reloj desincronizado | Sincronizar hora con `w32tm /resync` |

## Ver tambien

- [[wiki/windows-server/dns-dhcp]]
- [[wiki/windows-server/powershell]]
- [[wiki/windows-server/mantenimiento]]

## Fuentes

- [AD DS Deployment Requirements](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/ad-ds-deployment-requirements)
- [AD DS Installation Wizard](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-installation-and-removal-wizard-page-descriptions)
