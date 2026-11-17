# Telefonía IP — Issabel sobre Proxmox, Cisco CME y diseño de solución

| Trabajo | Qué demuestra | Autoría | Evidencia |
|---|---|---|---|
| **Laboratorio Issabel sobre Proxmox VE** | 5 centrales Issabel (Asterisk/FreePBX) virtualizadas, 6 routers Cisco con OSPF área 0 y troncales IAX2 entre sedes: IVR, grupos de timbrado y llamadas entre los 5 grupos | individual | [ficha](evidencias/01_issabel_proxmox.md) + [video de la marcación entre grupos](https://www.youtube.com/shorts/ku6mSzqpTMU) |
| Cisco CME e IP Communicator | Manual de configuración de telefonía en el router con DHCP/CME y softphones registrados | grupal | [ficha](evidencias/02_cisco_cme_telefonia.md) |
| Diseño básico de solución VoIP | Dimensionamiento de una solución para 8 puestos y sede principal (servidor PBX, teléfonos SIP, PoE) | grupal | [ficha](evidencias/02_cisco_cme_telefonia.md) |

## El laboratorio más completo

El de Issabel es el que tiene más valor: virtualización en Proxmox VE, enrutamiento dinámico con OSPF entre
seis routers y cinco centrales, troncales IAX2 entre sedes simuladas, menús IVR y grupos de timbrado. La
prueba funcional quedó grabada en un video corto (marcación entre extensiones de distintos grupos, respuesta
del IVR y audio estable).

## Mi aporte

Issabel: individual (virtualización, OSPF, troncales y servicios de cada central). CME y diseño de solución:
grupal, con mi participación en la configuración del router y en el dimensionamiento.

## Alcance

- Todo es laboratorio (Proxmox VE y Packet Tracer / equipos del aula); no hay troncales SIP reales de un
  operador.
- No se publican credenciales de extensiones ni de administración de las centrales.
