> **Procedencia:** nota de referencia de mi base de conocimiento técnico, redactada con asistencia de IA
> a partir de documentación oficial (las fuentes van al final de la nota) y revisada al usarla en los
> laboratorios. **No es un registro de laboratorio**: para eso están los informes de las otras carpetas.
> Copia tomada del respaldo de la wiki del 2026-10-06; la versión viva está en mi servidor.


# Hosts e Items en Zabbix

> **TL;DR**: Un **host** es un dispositivo o servidor monitoreado, definido con al menos una interfaz (Agent, SNMP, JMX, IPMI). Un **item** es la unidad básica de recolección de datos dentro de un host, identificado por una **key** única. Los items usan distintos tipos: Zabbix agent (passive/active), SNMP agent, simple check, entre otros.

## Host: conceptos básicos
Un host en Zabbix representa cualquier recurso a monitorear (servidor, switch, router, impresora, etc.).

### Configuración mínima de un host

| Parámetro | Descripción |
|-----------|-------------|
| **Host name** | Nombre único del host (alfanumérico, puntos, guiones, guiones bajos). Obligatorio. |
| **Groups** | Grupo o grupos de hosts al que pertenece. Obligatorio. |
| **Interfaces** | Una o más interfaces de comunicación. Al menos Agent o SNMP requerido. |

### Interfaces soportadas

| Interfaz | Puerto default | Uso |
|----------|---------------|-----|
| **Agent** | 10050 | Comunicación con Zabbix Agent instalado en el host. |
| **SNMP** | 161 | Monitoreo via protocolo SNMP (v1, v2c, v3). |
| **JMX** | 12345 | Supervisión de servicios Java via Zabbix Java Gateway. |
| **IPMI** | 623 | Monitoreo de hardware vía protocolo IPMI. |

> **Nota**: Si un item puede usar más de una interfaz, Zabbix prioriza en orden: Agent → SNMP → JMX → IPMI.

### Crear un host desde cero

1. Ve a **Data collection → Hosts** (o **Monitoring → Hosts**).
2. Haz clic en **Create host**.
3. Ingresa **Host name**, selecciona **Groups**.
4. En **Interfaces**, haz clic en **Add**, selecciona el tipo (Agent o SNMP), ingresa IP/DNS y puerto.
5. Opcional: enlaza **Templates** desde la pestaña correspondiente.
6. Guarda con **Add**.

Alternativamente, usa el **Host Wizard** para una configuración guiada paso a paso.

## Items: recolección de datos

Un **item** es la entidad que define qué dato se recolecta, cómo y con qué frecuencia. Cada item tiene una **key** única dentro del host.

### Campos principales de un item

| Campo | Descripción |
|-------|-------------|
| **Name** | Nombre descriptivo del item (soporta user macros). |
| **Type** | Tipo de item (Zabbix agent, SNMP agent, Simple check, etc.). |
| **Key** | Identificador único del item dentro del host (hasta 2048 caracteres). |
| **Interval** | Frecuencia de recolección (ej. `30s`, `1h`, `7d`). |
| **History** | Días que se conservan los valores crudos. |
| **Trends** | Días que se conservan las tendencias (agregadas). |
| **Status** | Enabled, Disabled, Not supported. |
| **Tags** | Pares nombre:valor para filtrado y organización. |

### Navegación a los items

Desde **Data collection → Hosts**, haz clic en **Items** en la fila del host deseado. La lista muestra todas las columnas anteriores. Puedes filtrar por host, nombre, key, tipo, estado, etc.

### Acciones sobre un item

- **Latest data**: ver valores recientes.
- **Create trigger**: crear una alerta basada en el item.
- **Execute now**: forzar ejecución inmediata (solo items pasivos).
- **Clear history and trends**: borrar datos históricos.
- **Copy**: copiar el item a otros hosts o plantillas.
- **Mass update**: actualizar propiedades de varios items a la vez.

## Keys de items

La **key** es el identificador central de un item. Debe ser única dentro de un host.

### Sintaxis general

```
key[parameter1,parameter2]
```

Los parámetros entre corchetes son opcionales y varían según el tipo de item.

### Ejemplos comunes de keys

| Key | Tipo | Descripción |
|-----|------|-------------|
| `agent.version` | Zabbix agent | Versión del agente instalado. |
| `system.uptime` | Zabbix agent | Tiempo activo del sistema. |
| `vfs.fs.size[/,free]` | Zabbix agent | Espacio libre en disco (por punto de montaje). |
| `net.if.in[eth0]` | Zabbix agent | Tráfico de red entrante en interfaz. |
| `system.cpu.load[all,avg1]` | Zabbix agent | Carga promedio CPU últimos 1 minuto. |
| `snmp[oid]` | SNMP agent | Valor obtenido vía OID SNMP. |
| `snmpget[host,port,oid,community]` | SNMP agent | Requiere parámetros explícitos. |
| `ping[host,packet_count]` | Simple check | Verifica conectividad ICMP. |

> Las keys soportadas dependen del tipo de item. Consulta la documentación específica de cada tipo.

## Tipos de items: Agent vs SNMP

### Zabbix Agent

Requiere el agente instalado en el host objetivo. Soporta dos modos:

#### Passive checks (Zabbix agent)
El servidor o proxy **solicita** datos al agente cuando toca el intervalo. El agente responde inmediatamente.

- Flujo: Server → Agent (request key) → Agent → Server (response value).
- Configuración en el agente: `Server=<IP del servidor>`.
- Ideal para: métricas puntuales, checks esporádicos.

#### Active checks (Zabbix agent (active))
El agente **inicia** la conexión, solicita la lista de items al servidor, los recolecta y envía los resultados.

- Flujo: Agent → Server (solicita lista) → Server (responde keys + delays) → Agent (recolecta y envía).
- Configuración en el agente: `ServerActive=<IP del servidor>`.
- Ideal para: hosts remotos, redes con firewall, muchos items en un solo host.
- El agente reintentan cada 60 segundos si falla la actualización de la lista.

### SNMP Agent

Recolecta datos mediante el protocolo SNMP, sin necesidad de agente instalado.

#### Versiones

| Versión | Seguridad | Autenticación | Encriptación |
|---------|-----------|---------------|--------------|
| **SNMPv1** | Baja | Ninguna (community string en texto claro) | Ninguna |
| **SNMPv2c** | Baja | Community string (texto claro) | Ninguna |
| **SNMPv3** | Alta | MD5/SHA + password | DES/AES |

#### Configuración de interfaz SNMP

1. En la configuración del host, añade interfaz tipo **SNMP**.
2. Ingresa IP/DNS, puerto (default 161), versión.
3. Para v1/v2c: ingresa community (ej. `public`).
4. Para v3: selecciona security level (noAuthNoPriv, authNoPriv, authPriv) y credentials correspondientes.
5. Especifica max repetition (default 10) para operaciones GetBulk.

#### Requisitos del servidor

Zabbix requiere soporte SNMP compilado: `--with-net-snmp` durante la instalación.

### Otros tipos de items

| Tipo | Description |
|------|-------------|
| **Simple check** | Checks básicos desde el servidor (ping, port check, HTTP check) sin agente. |
| **Zabbix internal** | Métricas internas del propio servidor Zabbix. |
| **External script** | Ejecuta scripts externos en el servidor Zabbix. |
| **Database monitor** | Consultas directas a bases de datos (requiere driver JDBC/ODBC). |
| **JMX agent** | Supervisión de objetos JMX (requiere Zabbix Java Gateway). |
| **IPMI agent** | Datos de hardware vía IPMI (requiere interfaz IPMI configurada). |
| **SSH / Telnet agent** | Ejecuta comandos remotos via SSH o Telnet. |
| **Dependent item** | Su valor depende de otro item (se ejecuta cuando cambia el item padre). |
| **HTTP agent** | Requiere interfaz HTTP configurada en el host. |

## Troubleshooting común

| Problema | Causa probable | Solución |
|----------|---------------|----------|
| Item **Not supported** | Key inválida, agente no responde, permisos SNMP | Verificar key, reiniciar agente, revisar community/string v3 |
| **Denied access** en agent | Host no autorizado en `Server=` del agente | Agregar IP del servidor Zabbix a la directiva Server |
| Item no recibe datos | Intervalo muy largo, servidor caído | Reducir interval, verificar estado del servidor |
| SNMP timeout | Firewalls bloqueando UDP 161 | Abrir puerto, verificar comunidad, usar SNMPv3 con auth |
| Agent activo no conecta | `ServerActive` incorrecto o firewall | Corregir ServerActive, verificar conectividad TCP 10050 |
| Items duplicados tras clone | Clone retiene entity IDs | Usar **Full clone** o renombrar keys manualmente |

## Ver también

- Arquitectura (`/wiki/zabbix/arquitectura.html`) · Dashboards (`/wiki/zabbix/dashboards-visualizacion.html`) · API (`/wiki/zabbix/api-troubleshooting.html`)

- [Zabbix Manual: Item Types](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes)
- [Zabbix Manual: Configuring a Host](https://www.zabbix.com/documentation/7.4/en/manual/config/hosts/host)


> Las rutas `/wiki/...` corresponden a mi servidor de notas, no a este repositorio.
