# Ficha — Inventario y métricas de un equipo Windows por SNMP

**Documento fuente:** `INFORME SNMP.pdf` (original en la carpeta de informes del autor)
**Autoría:** individual — A.S.P.S.
**Fecha del documento:** 24 de abril de 2026

## Qué es

Pruebas de gestión de red con SNMP sobre un equipo Windows: consulta de métricas de hardware y software
desde una estación de gestión con `snmpget` y `snmpwalk`, y consolidación en un inventario técnico.

## Objetivo

Validar la conectividad con un agente SNMP y extraer CPU, memoria, almacenamiento y software instalado,
demostrando que el equipo puede integrarse en un sistema de monitoreo.

## Entorno

| Elemento | Detalle |
|---|---|
| Nodo gestionado | Windows con agente SNMP habilitado, comunidad configurada y UDP/161 permitido |
| Equipo inventariado | DELL, Intel i5-1335U, 16 GB RAM, 491 GB de disco |
| Estación de gestión | Consola con `snmpget` / `snmpwalk` |

## Qué se hizo

1. Consulta básica de sistema para verificar conectividad con el agente.
2. Lectura de `sysName` con `snmpget`.
3. Consulta de carga de CPU y descripción del procesador por núcleo.
4. Consulta de memoria: capacidad total y uso en unidades de asignación.
5. Consulta de discos: particiones, capacidad y espacio libre por unidad lógica.
6. Recorrido del árbol MIB con `snmpwalk` para listar aplicaciones, parches y controladores instalados.
7. Cuestionario conceptual: `snmpget` vs `snmpwalk`, y efecto de cambiar la comunidad solo en el gestor.

## Resultados comprobados

- El agente respondió a las consultas desde la estación de gestión (capturas de las salidas incluidas en el
  documento original).
- Inventario obtenido por SNMP: equipo DELL, CPU i5-1335U, 16 GB de RAM y 491 GB de almacenamiento.
- Listado de software instalado recuperado mediante recorrido de MIB.
- Se comprobó que la comunidad configurada y la regla de firewall permiten el flujo por UDP/161.
- Hallazgo del cuestionario: al cambiar la comunidad solo en el gestor, la consulta siguió funcionando
  incluso reiniciando el servicio SNMP en el equipo (el autor lo deja como observación, sin analizar la causa).

## Límites y lo que no está demostrado

- No se transcriben los OIDs literales ni la salida cruda de los comandos: la reproducibilidad depende de las
  capturas del documento original.
- Es un sondeo puntual, no monitoreo continuo: no hay triggers, umbrales ni integración con una plataforma.
- El nombre de la comunidad no se publica.
