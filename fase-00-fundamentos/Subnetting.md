## Subnetting — Dividir redes

### Qué es en una línea

Es el proceso de dividir una red grande en redes más pequeñas llamadas subredes. 

---

### La analogía perfecta

Imagina un edificio de apartamentos enorme con 254 apartamentos todos en el mismo pasillo abierto. Cualquier persona puede caminar hasta cualquier apartamento sin restricción.

Subnetting es como construir pisos separados con puertas y llaves entre ellos:

```
SIN SUBNETTING
───────────────
Un solo pasillo enorme
254 dispositivos viéndose entre sí
Si alguien entra — ve todo
Tu HP, Mac, Pi, smart TV, todo junto

CON SUBNETTING — tu lab
────────────────────────
Piso 1 (VLAN 10) → Administración
Piso 2 (VLAN 20) → Servidores
Piso 3 (VLAN 30) → Dispositivos personales
Piso 4 (VLAN 40) → IoT — smart TVs, bocinas
Piso 5 (VLAN 50) → Lab experimental
```

Cada piso tiene su propia puerta. pfSense decide quién puede pasar entre pisos.

---

### Entendiendo una dirección IP

Una IP tiene dos partes:

```
IP:      10.0.0.10
          │  │  ││
          │  │  │└─ Host — el dispositivo específico
          │  │  └── Host
          │  └───── Red — identifica la subred
          └──────── Red
```

La máscara de subred le dice a los dispositivos qué parte es red y qué parte es host.

---

### La máscara de subred — CIDR notation

Esta es la notación que verás en pfSense y Proxmox constantemente:

```
/24 = 255.255.255.0
      └── 254 hosts disponibles
      └── Red más común en casas y labs

/16 = 255.255.0.0
      └── 65,534 hosts disponibles
      └── Redes más grandes

/8  = 255.0.0.0
      └── 16,777,214 hosts
      └── Redes enormes
```

La analogía: el /24 es como un barrio. El /16 es como una ciudad. El /8 es como un país.

---

### Tu lab — las subredes que vas a crear

Cuando configures pfSense vas a crear exactamente esto:

```
SUBRED          CIDR        DISPOSITIVOS
─────────────────────────────────────────────
10.0.0.0/24   → Administración
               └── pfSense:   10.0.0.1
               └── Proxmox:   10.0.0.10
               └── Pi-hole:   10.0.0.20
               └── Mac:       10.0.0.30

10.0.10.0/24  → Servidores (VMs)
               └── Nextcloud: 10.0.10.10
               └── Wazuh:     10.0.10.20
               └── Jellyfin:  10.0.10.30

10.0.20.0/24  → Dispositivos personales
               └── iPhone:    10.0.20.x
               └── iPad:      10.0.20.x

10.0.30.0/24  → IoT
               └── Smart TVs: 10.0.30.x
               └── Bocinas:   10.0.30.x

10.0.50.0/24  → Lab experimental
               └── Kali:      10.0.50.10
               └── Metasploit:10.0.50.20
```

---

### Por qué esto importa para seguridad

```
SIN SUBNETTING — situación actual
───────────────────────────────────
Tu smart TV puede hablar con tu Mac
Tu Mac puede hablar con el servidor
Un dispositivo hackeado ve toda la red
Si hackean tu smart TV → tienen acceso a todo

CON SUBNETTING — después de Fase 1
────────────────────────────────────
Smart TV solo ve otros dispositivos IoT
No puede hablar con el servidor
No puede hablar con tu Mac
Si hackean tu smart TV → están atrapados
en la VLAN de IoT — no pueden salir
```

Esto se llama **network segmentation** — uno de los controles de seguridad más importantes del Security+ y del Army.

---

### Los tres rangos privados — memorízalos

```
CLASE A → 10.0.0.0/8
          10.x.x.x
          Usamos este para el lab ✓

CLASE B → 172.16.0.0/12
          172.16.x.x hasta 172.31.x.x
          Común en empresas medianas

CLASE C → 192.168.0.0/16
          192.168.x.x
          El que usa tu Eero actualmente
```

---

### Cálculo rápido de hosts

La fórmula: **2^(32-CIDR) - 2**

```
/24 → 2^(32-24) - 2 = 2^8 - 2 = 256 - 2 = 254 hosts
/25 → 2^(32-25) - 2 = 2^7 - 2 = 128 - 2 = 126 hosts
/26 → 2^(32-26) - 2 = 2^6 - 2 = 64  - 2 = 62  hosts
/30 → 2^(32-30) - 2 = 2^2 - 2 = 4   - 2 = 2   hosts
```

Para tu lab /24 es perfecto — 254 dispositivos por subred es más que suficiente.

---

### En el Army

El Army usa subnetting para:

```
Separar redes clasificadas de no clasificadas
Aislar sistemas críticos
Contener brechas de seguridad
Segmentar por departamento y nivel de acceso
```

Un analista que sabe subnetting puede diseñar redes que contienen ataques automáticamente — exactamente lo que vas a hacer en tu lab.

---

### Cómo lo pregunta el Security+

```
"How many hosts are available in a /24 network?"
→ 254

"Which IP range is private?"
→ 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

"A company wants to isolate IoT devices
from corporate systems. What should they use?"
→ Network segmentation / VLANs / Subnetting

"What is the subnet mask for /24?"
→ 255.255.255.0
```