## Modelo OSI — La base de todo en redes

### Qué es en una línea

Es un modelo de 7 capas que explica cómo viajan los datos desde una computadora hasta otra a través de una red.

### La analogía perfecta — enviar una carta internacional

Imagina que le mandas una carta a alguien en otro país.
TÚ escribes la carta          → Capa 7 — Aplicación
La metes en un sobre          → Capa 6 — Presentación
Le pones número de seguimiento → Capa 5 — Sesión
La divides si es muy grande   → Capa 4 — Transporte
Le escribes la dirección      → Capa 3 — Red
La pones en una bolsa postal  → Capa 2 — Enlace de datos
La llevan físicamente         → Capa 1 — Física

Cada capa tiene un trabajo específico. Si una falla, la comunicación se rompe.

### Las 7 capas — de arriba hacia abajo

CAPA 7 — APLICACIÓN
─────────────────────
Lo que tú ves y usas directamente
HTTP, HTTPS, FTP, DNS, Email
Ejemplo: cuando abres Chrome y vas a google.com

CAPA 6 — PRESENTACIÓN
──────────────────────
Traduce y cifra los datos
SSL/TLS vive aquí — el candado de HTTPS
Convierte datos a formato que ambos entiendan
Ejemplo: cuando ves el candado 🔒 en el navegador

CAPA 5 — SESIÓN
────────────────
Abre, mantiene y cierra conexiones
Controla cuánto dura una conversación
Ejemplo: cuando inicias sesión en Netflix
         la sesión se mantiene activa

CAPA 4 — TRANSPORTE
────────────────────
Divide los datos en trozos llamados segmentos
TCP — garantiza que todo llegue completo
UDP — manda rápido sin verificar
Ejemplo: TCP es como envío certificado
         UDP es como echar la carta al buzón

CAPA 3 — RED
─────────────
Maneja las direcciones IP
Decide el mejor camino para los datos
Los routers viven aquí — pfSense vive aquí
Ejemplo: el GPS que decide la ruta

CAPA 2 — ENLACE DE DATOS
─────────────────────────
Maneja las direcciones MAC
Transfiere datos entre dispositivos vecinos
Los switches viven aquí
Tu switch XGS-1008 vive aquí
Ejemplo: el cartero de tu barrio específico

CAPA 1 — FÍSICA
────────────────
Los cables, señales eléctricas, ondas WiFi
Bits — ceros y unos viajando físicamente
Tus cables Cat6 viven aquí
Ejemplo: la carretera física por donde viaja todo

### El truco para memorizar el orden

De arriba hacia abajo:
All People Seem To Need Data Processing
A — Application
P — Presentation
S — Session
T — Transport
N — Network
D — Data Link
P — Physical

### Cómo aplica en tu lab
Cuando tu Mac hace una petición a Proxmox:

Capa 7 → Chrome genera la petición HTTP
Capa 6 → TLS cifra la conexión HTTPS
Capa 5 → Se abre una sesión entre Mac y Proxmox
Capa 4 → TCP divide los datos en segmentos
Capa 3 → IP 10.0.0.10 — pfSense decide la ruta
Capa 2 → Tu switch XGS-1008 dirige el paquete
Capa 1 → El cable Cat6 lleva los bits físicamente

Todo eso pasa en milisegundos cada vez que abres Proxmox desde tu Mac.

### En el Army

Cuando un analista del Army investiga un ataque, usa el modelo OSI para diagnosticar exactamente dónde ocurrió:
¿El cable está bien?          → Capa 1
¿El switch está funcionando?  → Capa 2
¿Las IPs están bien?          → Capa 3
¿TCP está establecido?        → Capa 4
¿La sesión se mantiene?       → Capa 5
¿El cifrado es válido?        → Capa 6
¿La aplicación responde?      → Capa 7

Es el checklist de diagnóstico universal.

### Cómo lo pregunta el Security+

Preguntas típicas del examen:
"A user cannot access a website but can ping
the server. Which OSI layer is affected?"
→ Respuesta: Capa 7 — Aplicación
  (ping funciona = capas 1-3 OK,
   el problema es la aplicación)

"Which layer handles MAC addresses?"
→ Respuesta: Capa 2 — Enlace de datos

"SSL/TLS operates at which OSI layer?"
→ Respuesta: Capa 6 — Presentación

"Which layer do routers operate at?"
→ Respuesta: Capa 3 — Red
