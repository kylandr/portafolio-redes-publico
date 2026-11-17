# Resumen del proyecto grupal — Diseño e implementación de redes IoT (semillero REDCIES)

> **Este proyecto es de autoría grupal.** El portafolio lo incluye como resumen, con crédito al equipo y al
> instructor. El informe no individualiza aportes, así que aquí **no** se atribuye a una sola persona ningún
> resultado, sección ni prueba.

## Identificación del trabajo

| Campo | Dato |
|---|---|
| Título | Diseño e implementación de redes IoT |
| Referencia principal | `Informe Semillero de Investigacion_V13.pdf` (23 páginas) |
| Marco | Semillero de investigación REDCIES, SENA regional Huila — Centro de la Industria, la Empresa y los Servicios (CIES), Neiva |
| Programa | Gestión de Redes de Datos, ficha 3171046 |
| Instructor orientador | Edgar Eduardo Olarte Cruz |
| Aprendices autores | A.S.P.S. · J.H.G. · D.F.R.C. · J.A.C.R. |
| Periodo de trabajo | 2025 |
| Socialización de resultados | 16 de diciembre de 2025 |
| Publicación del documento | 17 de febrero de 2026 (metadatos del PDF) |
| Herramienta | Cisco Packet Tracer |

Los compañeros aparecen con iniciales en esta versión del portafolio; sus nombres completos están en la
portada del informe original y se publicarán solo si ellos lo autorizan.

## Objetivo

Simular e implementar una red IoT funcional aplicando conceptos de gestión de redes en un entorno
industrial: analizar el comportamiento del tráfico IoT para distinguir patrones normales de anómalos, y
configurar dispositivos y dominio en un entorno simulado.

## Qué construyó el equipo

Dos escenarios en Packet Tracer:

**Escenario 1 — Red WAN (empresarial ↔ industrial).** Dos sedes separadas geográficamente unidas por un
enlace serial a través de una nube `Cloud-PT` que emula el ISP, con dos routers Cisco 2911. La sede
empresarial usa DHCP del propio router para equipos de oficina, un router inalámbrico doméstico WRT300N y un
smartphone. La sede industrial cuelga de un switch 2960-24TT, con un **AP industrial DLC100** con IP estática
y WPA2-PSK que hace de controlador y servidor DHCP de los dispositivos IoT, más un servidor `Server-PT`
estático que gestiona los servicios IoT (ventiladores, lámparas, sensores).

**Escenario 2 — Red LAN segmentada con simulación de ataque.** Router 2911 con subinterfaces, VLAN 100
(oficina: PC, portátil por AP inalámbrico, impresora) y VLAN 200 (servidor IoT, AP industrial DLC100, sensor
infrarrojo, ventana automática, cafetera y un PC de soporte), OSPF entre router empresarial y router
atacante, NAT, DHCP y un **router/PC atacante** para medir el riesgo real de una intrusión.

## Cómo se comprobó el resultado

El informe documenta **25 pruebas**, todas con evidencia en pantalla dentro de Packet Tracer:

- **11 pruebas del escenario WAN:** `ping` del router industrial al servidor, al PC0, al AP industrial, al
  router empresarial y al PC1; `ping` del router empresarial al industrial y al servidor; gestión de
  dispositivos IoT desde el servidor, desde un celular en la red del AP industrial y desde un teléfono
  empresarial; y `ping` desde el teléfono industrial al router industrial.
- **14 pruebas del escenario LAN híbrido:** `ping` cruzados PC1↔PC2, PC1↔servidor, PC1→impresora,
  PC2→impresora, PC2↔servidor, PC↔router empresarial en ambos sentidos, y gestión de dispositivos IoT desde
  una laptop y desde PC2.
- **Tabla de direccionamiento** por red (WAN y LAN) y **tabla de enrutamiento**.
- **Matriz de endurecimiento:** interfaces deshabilitadas (`shutdown`) en switches 2960-24T y routers 2911,
  con el motivo de cada desactivación.

## Hallazgos y conclusión del equipo

- La topología WAN confirma que la integración empresa–industria funciona con DHCP, IP estática en el AP
  industrial y dispositivos IoT operando sin configuración manual.
- La topología segmentada con VLAN reduce el riesgo entre oficina y planta, pero **no elimina** el riesgo: el
  equipo identificó trunk sin protección (posible salto de VLAN), información expuesta por OSPF,
  dispositivos IoT con credenciales débiles o firmware desactualizado y un AP industrial vulnerable sin
  credenciales robustas.
- Recomendación prioritaria del informe: *"La prioridad de seguridad en entornos segmentados debe ser la
  implementación obligatoria de listas de control de acceso y autenticación en todos los dispositivos de
  interconexión."*

## Referencias del informe original

- Cisco Packet Tracer — Lab downloads, NetAcad (consultado el 20 de noviembre de 2025).
- "El Profe Lobato" (2024). *Simulación de IoT con Cisco Packet Tracer* (YouTube).
- Estrada, M. (2020). *Configurar la nube en Packet Tracer* (YouTube).
- OfimaticaEasy (2012). *Configuración de nube "Cloud" | Packet Tracer | Tutorial 11* (YouTube).

---

**Documentos originales:** el informe (`Informe Semillero de Investigacion_V13.pdf`) y la exportación de 33
páginas que añade las diapositivas de socialización del 16-12-2025 **no** se publican en este repositorio:
son obra de cuatro autores y contienen material institucional. Se conservan en el archivo personal del autor
y se pueden solicitar al equipo si se necesitan.
