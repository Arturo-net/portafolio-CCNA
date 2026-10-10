## En este laboratorio de práctica implemente una red LAN simulando una pequeña empresa, separando los departamentos con VLANs independientes y realizando la comunicación inter VLAN mediante un router.

## Objetivos
---
- Switching
- Solidificar habilidades en direccionamiento IPv4
- Comprender las configuraciones para crear VLANs
- Entender el enrutamiento dentro de una LAN

---

## Procedimiento e Implementación

---

### 1. Planificación de Red y Esquema de Subredes

Se diseñó la segmentación de red utilizando subredes `/29` para optimizar el uso de direcciones IPv4, junto con un enlace punto a punto `/30` para la conexión hacia R2 (ISP).

| Red / Subred | Prefijo | Uso / Departamento |
| :--- | :---: | :--- |
| ```203.0.113.0/30``` | ```/30``` | Enlace Punto a Punto (R1 a R2 / ISP) |
| ```192.168.10.0/29``` | ```/29``` | VLAN 10 (Administración / ADMIN) |
| ```192.168.10.8/29``` | ```/29``` | VLAN 20 (Ventas / SALES) |
| ```192.168.10.16/29``` |```/29```| VLAN 30 (Soporte / SUPPORT) |
| ```92.168.10.24/29``` | ```/29``` | VLAN 99 (VLAN de Administración) |

---

### 2. Asignación de Puertos en Switches

Se configuraron los puertos de acceso y troncales para garantizar la correcta segmentación y transporte del tráfico etiquetado (802.1Q).

#### **Switch SW2**
- **Puertos Troncales:** ```F0/1``` (Hacia SW1)
- **Puertos de Acceso:**
  - ```F0/2``` y ```F0/3``` → VLAN 10 (PC1 y PC2)
  - ```F0/4``` → VLAN 20 (PC3)

#### **Switch SW3**
- **Puertos Troncales:** ```F0/1``` (Hacia SW1)
- **Puertos de Acceso:**
  - ```F0/4``` → VLAN 20 (PC4)
  - ```F0/2``` y ```F0/3``` → VLAN 30 (PC5 y PC6)

---

### 3. Configuración de Enrutamiento (ROAS) e ISP

- **Router-on-a-Stick (ROAS):** Configurado en la interfaz subinterfaz ```G0/1``` de **R1** para posibilitar el enrutamiento Inter-VLAN entre las VLANs 10, 20, 30 y 99.
- **Enlace ISP (R2):** Se configuró la interfaz hacia **R2** (ISP) permitiendo alcanzr el tráfico hacia redes externas desde la infraestructura interna.

---

### 4. Pruebas y Verificación de Conectividad

Para validar la correcta implementación y aislamiento del tráfico, se realizaron exitosamente las siguientes pruebas de conectividad mediante `ping`:

- [x] **Conectividad Intra-VLAN:** Verificación de tráfico entre hosts de la misma VLAN.
- [x] **Conectividad Inter-VLAN:** Verificaci[on de enrutamiento a través de R1 entre diferentes VLANs.
- [x] **Gestión de Switches:** ping exitoso desde los PCs hacia la IP de administración de cada Switch (VLAN 99).
- [x] **Alcanzabilidad del Gateway:** Verificación de comunicación desde los PCs hacia R1.
- [x] **Prueba End-to-End (ISP):** Confirmación de ping exitoso desde la red LAN interna hacia R2 (ISP).

## Aprendizaje
- No es necesario crear todas las VLANs de la topología en cada switch, basta con crear las VLAN a las que estan conectados.
- Es necesario que el router conectado al ISP tenga una ruta hacia cada una de las VLANs.
- Es necesario configurar una default gateway para cada switch en el caso de que tengan una interfaz de administración.
