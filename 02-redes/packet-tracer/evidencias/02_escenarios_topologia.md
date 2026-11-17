# Topologías de los dos escenarios (reconstrucción a partir del informe)

Diagramas redibujados por el autor del portafolio a partir de la descripción y las tablas del informe
grupal, para publicar sin exponer el documento original.

## Escenario 1 — Red WAN empresarial ↔ industrial

```mermaid
graph LR
    CLOUD["Cloud-PT<br/>(emula el ISP)"] ---|"enlace serial DTE"| R_EMP["Router Cisco 2911<br/>Sede empresarial"]
    CLOUD ---|"enlace serial DTE"| R_IND["Router Cisco 2911<br/>Sede industrial"]
    R_EMP --- SW_EMP["Switch Cisco 2960-24T"]
    SW_EMP --- PC1["PC1 (oficina)"]
    SW_EMP --- WRT["Router inalámbrico WRT300N"]
    WRT -.->|"WiFi"| TEL["Smartphone empresarial"]
    SW_EMP --- PC2["PC2"]
    R_IND --- SW_IND["Switch Cisco 2960-24TT"]
    SW_IND --- SRV["Server-PT<br/>servidor IoT + DHCP<br/>(IP estática)"]
    SW_IND --- AP["AP industrial DLC100<br/>IP estática · WPA2-PSK<br/>DHCP para IoT"]
    AP -.->|"WiFi"| IOT["Dispositivos IoT<br/>ventiladores, lámparas,<br/>sensores"]
```

## Escenario 2 — Red LAN segmentada con atacante

```mermaid
graph TD
    R_EMP["Router Cisco 2911<br/>subinterfaces · DHCP · NAT · OSPF"]
    R_EMP -->|"VLAN 100"| SW["Switch Cisco 2960-24T"]
    R_EMP -->|"VLAN 200"| SW
    SW -->|"VLAN 100 (oficina)"| PC1["PC1"]
    SW -->|"VLAN 100"| APE["AP inalámbrico<br/>empresarial"] -.-> LAP["Laptop0"]
    SW -->|"VLAN 100"| PRN["Impresora"]
    SW -->|"VLAN 200 (IoT)"| SRV["Servidor IoT<br/>(IP estática)"]
    SW -->|"VLAN 200"| AP["AP industrial DLC100"]
    SW -->|"VLAN 200"| PC2["PC2 (soporte)"]
    AP -.-> IOTS["Sensor infrarrojo ·<br/>ventana automática ·<br/>cafetera"]
    R_EMP <-->|"OSPF"| R_ATK["Router atacante Cisco 2911"]
    R_ATK --- PC_ATK["PC atacante<br/>(pruebas de intrusión)"]
```

## Endurecimiento aplicado (matriz del informe)

| Dispositivo | Modelo | Interfaces deshabilitadas | Motivo |
|---|---|---|---|
| Switch empresarial | Cisco 2960-24T | FastEthernet 0/2 – 0/24 | Evitar conexiones no autorizadas por puertos libres |
| Switch industrial | Cisco 2960-24TT | FastEthernet 0/3 – 0/24 | Reducir superficie de ataque en red IoT |
| Router empresarial | Cisco 2911 | GigabitEthernet 0/1 – 0/2, serial 0/3/1 | Deshabilitar interfaces sin función en la topología |
| Router industrial | Cisco 2911 | GigabitEthernet 0/1, serial 0/3/1 | Minimizar puertos abiertos |
| Switch segmentado (VLAN 100/200) | Cisco 2960-24T | FE 0/3–0/6 y 0/8–0/12 (VLAN 100); FE 0/16–0/24 (VLAN 200) | Aislar accesos no autorizados entre segmentos |
| Router segmentado | Cisco 2911 | GigabitEthernet 0/2 | Mantener solo las interfaces necesarias para enrutar VLAN |

Las tablas de direccionamiento IP y de enrutamiento del informe original no se reproducen aquí: sus IP
exactas están en el documento grupal, que se conserva privado.
