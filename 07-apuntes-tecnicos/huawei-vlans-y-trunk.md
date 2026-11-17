> **Procedencia:** nota de referencia de mi base de conocimiento técnico, redactada con asistencia de IA
> a partir de documentación oficial (las fuentes van al final de la nota) y revisada al usarla en los
> laboratorios. **No es un registro de laboratorio**: para eso están los informes de las otras carpetas.
> Copia tomada del respaldo de la wiki del 2026-10-06; la versión viva está en mi servidor.

# Huawei VLANs & Trunk

Este documento cubre la configuración de VLANs y enlaces Trunk en dispositivos Huawei VRP.

## Conceptos de VLAN

Las VLAN (Virtual LAN) permiten segmentar redes LAN lógicamente:

* **VLAN 1**: Default, no se puede eliminar ni renombrar
* **VLAN 1002-1005**: Reservadas para FDDI y Token Ring
* **VLAN 1006-4094**: VLANs extendidas para uso general
* **MAC Learning**: Las VLANs aíslan dominios de broadcast

Tipos de puertos:
* **Access**: Conexión a dispositivos finales (PC, impresoras)
* **Trunk**: Conexión entre switches o con routers
* **Hybrid**: Puertos flexibles que pueden ser access o trunk

## Creación de VLANs

Crear VLANs individual o múltiple:

```
# Crear VLAN individual
[SW-CENTRO] vlan 10
[SW-CENTRO-vlan10] description VLAN-VENTAS
[SW-CENTRO-vlan10] quit

# Crear múltiples VLANs
[SW-CENTRO] vlan 20,30,40
[SW-CENTRO-vlan20] description VLAN-RH
[SW-CENTRO-vlan20] quit
[SW-CENTRO-vlan30] description VLAN-IT
[SW-CENTRO-vlan30] quit
[SW-CENTRO-vlan40] description VLAN-INVitados
[SW-CENTRO-vlan40] quit

# Ver VLANs creadas
[SW-CENTRO] display vlan all
```

## Asignación a puertos

Configurar puertos access para terminales:

```
# Asignar puerto a VLAN 10
[SW-CENTRO] interface GigabitEthernet 0/0/1
[SW-CENTRO-GigabitEthernet0/0/1] port link-type access
[SW-CENTRO-GigabitEthernet0/0/1] port default vlan 10
[SW-CENTRO-GigabitEthernet0/0/1] quit

# Configurar múltiples puertos
[SW-CENTRO] interface range GigabitEthernet 0/0/2 to 0/0/10
[SW-CENTRO-if-range] port link-type access
[SW-CENTRO-if-range] port default vlan 20
[SW-CENTRO-if-range] quit

# Verificar asignación
[SW-CENTRO] display port vlan GigabitEthernet 0/0/1
```

## Configuración de trunk

Configurar enlaces trunk entre switches:

```
# Configuración trunk estándar
[SW-CENTRO] interface GigabitEthernet 0/0/24
[SW-CENTRO-GigabitEthernet0/0/24] port link-type trunk
[SW-CENTRO-GigabitEthernet0/0/24] port trunk allow-pass vlan 10 20 30
[SW-CENTRO-GigabitEthernet0/0/24] quit

# Permitir todas las VLANs
[SW-CENTRO] interface GigabitEthernet 0/0/24
[SW-CENTRO-GigabitEthernet0/0/24] port trunk allow-pass vlan all
[SW-CENTRO-GigabitEthernet0/0/24] quit

# Verificar trunk
[SW-CENTRO] display port trunk GigabitEthernet 0/0/24
```

## Modo trunk dinámico

Negociación dinámica de trunk (DTP):

```
# Habilitar negociación dinámica
[SW-CENTRO] interface GigabitEthernet 0/0/24
[SW-CENTRO-GigabitEthernet0/0/24] port trunk negotiation enable
[SW-CENTRO-GigabitEthernet0/0/24] quit

# Ver estado de negociación
[SW-CENTRO] display port dialog GigabitEthernet 0/0/24
```

Nota: No todas las versiones de VRP soportan DTP nativo.

## Troubleshooting de VLANs

Problemas comunes y soluciones:

```
# Ver tráfico de VLAN
[SW-CENTRO] display vlan 10 statistics

# Ver errores en puerto
[SW-CENTRO] display interface GigabitEthernet 0/0/1

# Verificar MAC addresses
[SW-CENTRO] display mac-address vlan 10

# Debug de VLAN (uso limitado)
[SW-CENTRO] debugging vlan 10
[SW-CENTRO] undo debugging all
```

## Mejores prácticas

* Usar VLANs segregadas por función/departamento
* Documentar numeración y propósito de cada VLAN
* Evitar VLAN 1 para usuarios
* Limitar VLANs permitidas en trunks
* Usar VLANs de gestión dedicadas
* Implementar ACLs en VLANs sensibles
