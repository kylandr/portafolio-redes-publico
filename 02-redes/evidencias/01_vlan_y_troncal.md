# Ficha — VLANs y enlace troncal 802.1Q en switches

**Documento fuente:** `LAB VLAN Stiven Polania Sanchez.pdf` y el archivo de Packet Tracer `VLAN Stiven
Polania.pkt` (conservado en local)
**Autoría:** individual — A.S.P.S.
**Fecha del documento:** no registrada

## Qué es

Práctica de laboratorio de conmutación: crear VLANs, asignar puertos de acceso y unir dos switches con un
enlace troncal 802.1Q.

## Objetivo

Armar la red, configurar los parámetros básicos de los dispositivos, crear las VLAN por área y comprobar que
el troncal transporta todas las VLAN entre switches.

## Entorno

| VLAN | Nombre | Puertos | Red |
|---|---|---|---|
| 100 | Docentes | Fa0/1–5 | 192.168.100.0/24 |
| 110 | Estudiantes | Fa0/6–10 | 192.168.110.0/24 |
| 120 | Dirección | resto de accesos | 192.168.120.0/24 |

Switches SW-01 y SW-02 con enlace troncal entre ellos; direccionamiento de gestión por VLAN
(`.10` en cada segmento).

## Qué se hizo

1. Configuración básica de los switches (hostname, contraseñas de acceso, banner).
2. Creación de las tres VLAN y asignación de puertos de acceso por área.
3. Configuración del puerto troncal en modo `trunk` con encapsulación 802.1Q en ambos switches.
4. Asignación de direcciones de gestión y verificación de VLAN con `show vlan brief` y del troncal con
   `show interfaces trunk`.
5. Prueba de conectividad entre hosts de la misma VLAN en switches distintos, y comprobación del aislamiento
   entre VLAN diferentes.

## Resultados comprobados

- Las tres VLAN aparecen activas y con sus puertos asignados (`show vlan brief`).
- El troncal muestra las VLAN 100, 110 y 120 en estado *forwarding* (`show interfaces trunk`).
- Hosts de la misma VLAN se alcanzan a través del troncal; hosts de VLAN distintas no, hasta que existe un
  dispositivo de capa 3.

## Límites

- Todo el laboratorio es Packet Tracer: no hay switches físicos ni medición de tráfico real.
- No se documenta STP ni agregación de enlaces: el alcance es VLAN + troncal.
