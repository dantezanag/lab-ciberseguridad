## TCP/IP — El lenguaje real de internet

### Qué es en una línea

Es el conjunto de reglas que usan todas las computadoras del mundo para comunicarse entre sí.

---

### La relación con OSI

Mucha gente se confunde aquí. Te lo aclaro de una vez:

```
OSI — modelo teórico de 7 capas
       creado para entender cómo funciona
       nadie lo usa directamente

TCP/IP — modelo práctico de 4 capas
          es lo que realmente usa internet
          todo dispositivo conectado lo habla
```

Es como la diferencia entre el mapa de una ciudad y las calles reales. OSI es el mapa teórico perfecto. TCP/IP son las calles reales con sus imperfecciones.

---

### Las 4 capas de TCP/IP vs OSI

```
TCP/IP                    OSI equivalente
──────────────────────────────────────────
Capa 4 — Aplicación   →  Capas 7, 6, 5
Capa 3 — Transporte   →  Capa 4
Capa 2 — Internet     →  Capa 3
Capa 1 — Acceso red   →  Capas 2, 1
```

TCP/IP simplifica las 7 capas de OSI en 4 capas prácticas.

---

### Los dos protocolos principales

**TCP — Transmission Control Protocol**

```
Garantiza entrega completa
Verifica que todo llegó
Más lento pero confiable
Se usa para: web, email, archivos

Analogía: envío certificado con acuse de recibo
          el cartero confirma que llegó
```

**UDP — User Datagram Protocol**

```
Manda y no verifica
Más rápido pero sin garantía
Se usa para: video, juegos, DNS, VoIP

Analogía: echar una carta al buzón
          no sabes si llegó pero fue rápido
```

---

### Cómo funciona TCP — el Three-Way Handshake

Este es uno de los conceptos más importantes del Security+.

Antes de enviar cualquier dato TCP hace esto:

```
TU MAC                    PROXMOX
   │                         │
   │──── SYN ───────────────▶│
   │     "quiero conectarme" │
   │                         │
   │◀─── SYN-ACK ────────────│
   │     "ok, adelante"      │
   │                         │
   │──── ACK ───────────────▶│
   │     "confirmado"        │
   │                         │
   │════ DATOS ══════════════│
   │     conexión establecida│
```

SYN → SYN-ACK → ACK — tres pasos, por eso se llama three-way handshake.

La analogía: es como saludar de mano. Tú extiendes la mano, el otro la toma, ambos aprietan. Conexión establecida.

---

### Direcciones IP — lo más importante

Cada dispositivo en una red tiene una dirección IP. Es como la dirección de tu casa.

**IPv4 — el formato actual**

```
192.168.4.1
10.0.0.10
```

Cuatro números del 0 al 255 separados por puntos.

**Dos tipos de IPs que necesitas conocer:**

```
IP PÚBLICA
───────────
La que ve internet
La que tiene tu Eero
En tu caso: 47.149.159.124
Única en todo el mundo
La asigna Frontier

IP PRIVADA
───────────
La que existe solo dentro de tu red
Nadie fuera puede verla directamente
Rangos reservados:
10.0.0.0 — 10.255.255.255    ← la que usamos
172.16.0.0 — 172.31.255.255
192.168.0.0 — 192.168.255.255
```

---

### Puertos — las puertas del edificio

Si la IP es la dirección del edificio, el puerto es el número del apartamento.

```
IP: 10.0.0.10 → edificio Proxmox
Puerto 8006   → apartamento interfaz web
Puerto 22     → apartamento SSH
Puerto 443    → apartamento HTTPS
Port 80       → apartamento HTTP
```

Puertos importantes que el Security+ pregunta:

```
22   → SSH — acceso remoto seguro
23   → Telnet — acceso remoto sin cifrar ⚠️
25   → SMTP — envío de correo
53   → DNS — resolución de nombres
80   → HTTP — web sin cifrar
443  → HTTPS — web cifrada
3389 → RDP — escritorio remoto Windows
8006 → Proxmox web interface
```

---

### Cómo aplica en tu lab

```
Tu Mac quiere acceder a Proxmox:

Mac IP: 10.0.0.30
        │
        │ "quiero hablar con 10.0.0.10
        │  en el puerto 8006"
        │
        ▼
Switch XGS-1008
        │
        ▼
HP Proxmox IP: 10.0.0.10
Puerto: 8006 ← interfaz web de Proxmox
        │
        ▼
Three-way handshake TCP
        │
        ▼
Ves la interfaz de Proxmox en tu navegador
```

---

### En el Army

Los analistas del Army usan TCP/IP para:

```
Analizar capturas de tráfico
└── Ver cada paquete que entra y sale
└── Identificar conexiones sospechosas
└── Detectar port scanning — atacante buscando puertos abiertos
└── Detectar SYN flood — ataque de denegación de servicio
```

Un SYN flood es cuando un atacante manda miles de SYN sin completar el handshake — colapsa el servidor. Eso lo verás en Wazuh en la Fase 6.

---

### Cómo lo pregunta el Security+

```
"Which protocol guarantees delivery?"
→ TCP

"Which protocol is faster but unreliable?"
→ UDP

"What port does HTTPS use?"
→ 443

"What are the three steps of TCP handshake?"
→ SYN, SYN-ACK, ACK

"Which port is used for SSH?"
→ 22

"A technician captures traffic and sees
many SYN packets without ACK responses.
What type of attack is this?"
→ SYN flood — DoS attack
```