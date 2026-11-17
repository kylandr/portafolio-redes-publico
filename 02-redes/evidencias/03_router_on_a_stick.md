# Ficha — Router-on-a-Stick: enrutamiento entre VLAN con subinterfaces

**Documento fuente:** `Taller_router_on_stick.pdf` y `Router on a stich.pkt` (local)
**Autoría:** individual — A.S.P.S.
**Fecha del documento:** no registrada

## Qué es

Actividad donde se resuelve el problema clásico de las VLAN: dos segmentos no se hablan sin un dispositivo de
capa 3. La solución implementada es *router-on-a-stick*: un solo enlace físico con subinterfaces.

## Objetivo

Permitir la comunicación entre VLAN con un único enlace entre el router y el switch, entendiendo el papel del
encapsulamiento 802.1Q en cada subinterfaces.

## Entorno

Router Cisco con subinterfaces (`g0/0.100`, etc.), switch con puerto troncal hacia el router y hosts en
VLAN distintas.

## Qué se hizo

1. Configuración del puerto del switch hacia el router como troncal 802.1Q.
2. Creación de una subinterfaces por VLAN en el router, con `encapsulation dot1Q <vlan>` y la IP del gateway
   de cada segmento.
3. Activación de las interfaces y verificación de la tabla de rutas conectadas.
4. Prueba de `ping` entre hosts de VLAN distintas a través del router.

## Resultados comprobados

- Cada subinterfaces aparece *up/up* con su VLAN asociada.
- Hosts de VLAN distintas se alcanzan usando el router como gateway (ping exitoso).

## Límites

- Laboratorio en Packet Tracer.
- No hay control de tráfico entre VLAN (ACLs): solo se busca la conectividad, no la política.
