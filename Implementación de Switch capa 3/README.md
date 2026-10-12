## En este laboratorio se usó una topología casi identica del laboratorio "Implementación de red LAN" pero esta vez usando un switch central de capa 3 para el enrutamiento inter-VLANs y menos host finales en dos de las VLANs.
---
## Objetivos
- Entender la diferencia entre usar un router o un switch de capa 3 para el enrutamiento inter-VLANs.
- Entender las ventajas de un switch de capa 3.
---

## Procedimiento e Implementación

## 1. Planificación y Esquema de subredes
Para este laboratorio se conservan las cuatro subredes:

| Red / Subred | Prefijo | Uso / Departamento |
| :--- | :---: | :--- |
| `192.168.10.0/29` | `/29` | VLAN 10 (Administración / ADMIN) |
| `192.168.10.8/29` | `/29` | VLAN 20 (Ventas / SALES) |
| `192.168.10.16/29` | `/29` | VLAN 30 (Soporte / SUPPORT) |
| `192.168.10.24/29` | `/29` | VLAN 99 (VLAN de ADMIN) |

- Además se agrego 10.0.0.0/30 para el enlace punto a punto entre MLS1 y R1.
- Se configuró una default route para el tráfico fuera de la LAN.
- El enlace entre R1 y R2 (ISP) conserva las misma subred 203.0.113.0/30
- R1 sigue realizando el routing externo hacia el ISP (R2).
- Se redujo el número a un solo PC en las VLANs 20 y 30.
---
## 2. Asignación de puertos en switches
Se realizó las configuraciones de puertos de acceso y troncales para transportar correctamente el tráfico etiquetado 802.1Q.
---
## Switch SW2
- **Puertos troncales:** `F0/1` hacia MLS1.
- **Puertos de acceso:** `F0/2` y `F0/3` → VLAN 10 (PC1 y PC2)
## Switch SW3
- **Puertos troncales:** `F0/1` hacia MLS1.
- **Puertos de acceso:** - `F0/2` → VLAN 20.
                         - `F0/3` → VLAN 30.
## Switch MLS1
- **Puertos troncales:** `G1/0/2` hacia SW2 y G1/0/3 hacia SW3.
- **Puerto enrutado:** `G1/0/1` hacia R1.
---
## 3. Configuración de enrutamiento en MLS1
- Se configuro, crearon y habilitaron las VLANs 10, 20, 30 y 99 asignándoles sus respectivas direcciones de red y sus nombres en el switch de capa 3 MLS1.
- Se configuro la interfaz `G1/0/1` para tener comunicación con el ISP (R2).
- Se asignó una default route via 10.0.0.2
---
## 4. Configuración R1
- Se agregaron rutas estáticas hacia las subredes que contienen las distintas VLANs para asegurar la comunicación bidireccional hacia el ISP.
- Se configuro una default route via 203.0.113.2 (`R2 G0/0`).
---
## 5. Configuración R2 (ISP)
- Se configuro una default route via 203.0.113.1 (`R1 G0/0`)
---
## Pruebas y verificación de conectividad
Para verificar las nuevas implementaciones y la correcta comunicación se realizaron exitosamente las siguientes pruebas:
- [x] **Conectividad intra-VLAN:** Verificación de tráfico entre hosts de la misma VLAN.
- [x] **Conectividad inter-VLAN:** Verificación de enrutamiento a través de R1 entre diferentes VLANs.
- [x] **Alcanzar los gateways en MLS1:** Hacer ping para alcanzar los gateways de cada PC.
- [x] **Alcanzar la VLAN 99 de gestión desde los PCs:**  Hacer ping para alcanzar la VLAN de gestión en cada switch
- [x] **Enlace MLS1-R1:** - Alcanzar la interfaz `G1/0/1: 10.0.0.1` de MLS1 desde R1.
                          - Alcanzar la interfaz `G0/1: 10.0.0.2` de R1 desde MLS1.
- [x] **Conectividad hacia el ISP(R2):** Alcanzar al ISP (R2) 203.0.113.2 desde R1.
- [x] **Conectividad completa hacia ISP:** Alcanzar al ISP(R2) desde cada PC.
---
## Aprendizaje
- Al utilizar un switch de capa 3 se evita tener que enviar el tráfico hasta el R1, lo que puede reducir el riesgo de cuellos de botella en el el enlace Router-On-a-Stick.(Esto en organizaciones dónde existen más VLANs y la carga de tráfico es mucho mayor).
- Se puede designar a R1 como responsable solo de reenviar el tráfico fuera de la LAN, lo que permite mejorar el desempeño de la red.
