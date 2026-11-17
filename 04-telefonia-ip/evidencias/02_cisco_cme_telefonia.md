# Ficha — Telefonía IP con Cisco CME e IP Communicator (trabajo grupal)

**Documento fuente:** `MANUAL DE CONFIGURACION DE TELEFONIA VOIP.pdf` y `Diseno-Basico-de-una-Solucion-VoIP.pdf`
**Autoría:** grupal — manual: A.S.P.S., S.A.G., Y.R. y D.S.G.; diseño de la solución: B.E.V., A.S.P.S., H.H.,
A.R. y C.C.P.
**Instructor:** Juan Carlos Sanabria A.
**Fecha del documento:** 2026 (no registrada con día)

## Qué es

Dos entregables del módulo de voz sobre IP: un manual de configuración de telefonía con **Cisco CallManager
Express (CME)** en el router y softphones **Cisco IP Communicator**, y un diseño básico de solución VoIP para
8 puestos de trabajo más una sede principal.

## Objetivo

Que los usuarios de la red puedan llamarse entre sí por IP, sustituyendo la línea telefónica tradicional, y
dimensionar qué se necesita para montarlo en una oficina pequeña.

## Entorno

- Router Cisco con servicio CME y DHCP para los teléfonos.
- Softphones Cisco IP Communicator en los puestos.
- Red del laboratorio (VLANs del curso).

## Qué se hizo

**Manual (CME):**

1. Configuración del router: servicio de telefonía, DHCP con opción TFTP para los teléfonos.
2. Creación de las líneas (ephone/dn) y asignación a los puestos.
3. Instalación y registro de los softphones IP Communicator contra el CME.
4. Prueba de llamada entre extensiones.

**Diseño de la solución:**

1. Inventario de equipos necesarios: servidor PBX IP (Asterisk/FreePBX), 8 teléfonos IP SIP, switches con
   PoE, gateway hacia la red pública.
2. Dimensionamiento para 8 puestos y la sede principal, con función de cada componente.

## Resultados comprobados

- Llamadas entre extensiones registradas en el CME mediante IP Communicator (procedimiento documentado en el
  manual).
- Diseño con lista de equipos, cantidades y función de cada uno, listo para cotizar o montar.

## Límites

- El manual documenta el procedimiento; no incluye capturas de llamadas ni de registros del CME.
- El diseño es una propuesta: no hay implementación de la solución de 8 puestos.
- La evidencia de llamadas reales del módulo está en el laboratorio Issabel (ficha 01 de esta carpeta).
