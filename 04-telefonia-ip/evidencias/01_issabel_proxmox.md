# Laboratorio de convergencia multiservicio con Issabel (VoIP) sobre Proxmox VE

## Qué es
Informe de laboratorio individual que documenta la implementación de una infraestructura de telefonía IP corporativa distribuida, basada en 5 instancias de Issabel (Asterisk/FreePBX) virtualizadas en Proxmox VE, interconectadas mediante enrutamiento dinámico OSPF y troncales IAX2.

## Objetivo
Simular un entorno real de ISP/Telefonía integrando virtualización empresarial, enrutamiento dinámico avanzado y VoIP, logrando comunicación bidireccional entre 5 grupos de trabajo (sedes simuladas) mediante extensiones SIP/IAX2, IVR, grupos de timbrado y troncales IAX2.

## Entorno
- **Hipervisor**: Proxmox VE sobre servidor Dell Xeon con 32 GB RAM DDR4
- **Centrales telefónicas**: 5 VMs Issabel, cada una en segmento LAN distinto (172.0.x.x)
- **Red de transporte**: 6 routers Cisco interconectados por enlaces seriales /30 (punto a punto)
- **Enrutamiento**: OSPF proceso 1, Área 0 (Backbone)
- **Protocolo de troncal**: IAX2 (nativo de Asterisk, mejor desempeño en ancho de banda y NAT que SIP)
- **Ubicación**: Centro Industrial del SENA, Regional Huila, Ficha 3171046
- **Instructor**: Juan Carlos Sanabria Alarcón

## Qué se hizo
1. **Virtualización y despliegue**: Creación de 5 VMs Issabel en Proxmox VE con direccionamiento IP único por LAN.
2. **Red de transporte**: Configuración física y lógica de 6 routers Cisco con interfaces seriales /30 e implementación de OSPF Área 0 para conectividad capa 3 extremo a extremo.
3. **Servicios de telefonía por nodo**:
   - Extensiones: terminales SIP/IAX2 por departamento (figura 5 referenciada, no incluida en texto)
   - IVR: menús audibles de atención automática (figuras 6 y 7 referenciadas)
   - Grupos de timbrado: estrategias de distribución de llamadas (figura 8 referenciada)
4. **Interconexión entre sedes**: Troncales IAX2 peer-to-peer entre las 5 centrales + Rutas de Salida (Outbound Routes) asociando prefijos de marcación a cada troncal (figuras 9 y 10 referenciadas).
5. **Pruebas funcionales**: Validación de comunicación bidireccional entre los 5 grupos, flujos IVR, transferencias, estabilidad OSPF y carga de recursos en Proxmox.

## Resultados comprobados (solo lo demostrado; indica el tipo de evidencia)
- **Interoperabilidad total entre 5 grupos**: comunicación bidireccional exitosa vía troncales IAX2 — *evidencia: video YouTube Short (https://www.youtube.com/shorts/ku6mSzqpTMU) mostrando marcación entre extensiones de distintos grupos, respuesta de IVR y estabilidad de audio*.
- **Funcionamiento de IVR, grupos de timbrado y transferencias**: configuración lógica verificada en cada central — *evidencia: video mencionado*.
- **Convergencia y estabilidad de red**: OSPF mantuvo rutas convergentes, tráfico de voz sin pérdida de paquetes ni latencia apreciable — *evidencia: reporte textual del autor; sin capturas de consola ni métricas incluidas*.
- **Eficiencia de virtualización**: Proxmox VE operó con carga mínima de recursos — *evidencia: afirmación textual; sin métricas de CPU/RAM adjuntas*.

## Límites y lo que no está demostrado
- No se incluye versión de Issabel ni de Asterisk/FreePBX subyacente.
- No se aportan capturas de configuración real (archivos .conf, CLI Asterisk, GUI Issabel) — el texto referencia figuras 5–10 que no están embebidas en el documento.
- No se muestran salidas de comandos de verificación (ej. `iax2 show peers`, `sip show peers`, `ospf neighbor`, `ping`/`iperf` entre sedes).
- No se detallan parámetros de QoS, codecs negociados, jitter/latencia medidos, ni plan de numeración completo por grupo.
- No hay credenciales expuestas en el texto (no hay contraseñas que redactar).
- El manual VoIP (Cisco CME) y el diseño básico son trabajos **grupales** distintos y no forman parte de este laboratorio Issabel individual.

## Fechas
- **Plan de trabajo concertado**: marzo 2026 (entregas programadas marzo 3/26)