# REDES2_1S2026_202204578

# 🌐 Proyecto 1 - Chapin Red
### Redes de Computadoras 2 | Universidad San Carlos de Guatemala
**Carné:** 202204578  
**Repositorio:** `REDES2_1S2026_202204578`  
**Facultad de Ingeniería** — Ingeniería en Ciencias y Sistemas

---

##  Descripción del Proyecto

**Chapin Red** es una empresa dedicada a organizar y ejecutar proyectos de ayuda humanitaria en comunidades vulnerables. Este proyecto consiste en el diseño e implementación de una red corporativa multi-edificio en **Cisco Packet Tracer**, que conecta las cuatro sedes de la organización mediante una infraestructura robusta, segura y eficiente.

La red implementada resuelve los principales problemas de la infraestructura actual de Chapin Red:

-  Falta de segmentación de red
-  Ausencia de control de acceso entre departamentos
-  Todos los dispositivos en el mismo dominio de broadcast
-  Sin redundancia en enlaces críticos

---

##  Arquitectura General

La solución implementa una red que interconecta **4 edificios** dentro de una red metropolitana (MAN), utilizando:

| Componente | Tecnología |
|---|---|
| Conexión entre edificios | Fibra óptica (GigabitEthernet - GLC-LH-SMD) |
| Protocolo de enrutamiento | **OSPF** (Open Shortest Path First) |
| Segmentación | 5 VLANs distribuidas entre edificios |
| Alta disponibilidad | LACP (edificio izquierdo) y PAgP (edificio derecho) |
| Asignación de IPs | DHCP dinámico (sin IPs estáticas en dispositivos finales) |
| Control de acceso | ACLs entre VLANs |
| Prevención de bucles | STP (Spanning Tree Protocol) |

---

##  Edificios y Roles

| Edificio | Rol | Switches | VLANs |
|---|---|---|---|
| **Edificio Izquierdo** | Arquitectura jerárquica 3 capas | Core + Distribución (3560) + Acceso (2960) | Naranja + Verde |
| **Edificio Derecho** | Red con PAgP | MSW 3650 | Naranja + Verde |
| **Edificio Administración** | VLAN ADMIN | MSW 3650 | ADMIN |
| **Edificio Central (MAN)** | Núcleo de interconexión | MSW 3650 | — |

---

##  Direccionamiento IP

### Redes Base Asignadas (Carné: 202204578 → X = 78)

| Red | Propósito |
|---|---|
| `192.188.78.0/24` | VLANs de usuarios |
| `10.4.78.0/24` | Enlaces de enrutamiento punto a punto |

### VLANs — Subnetting de `192.188.78.0/24`

| # | VLAN | Edificio | ID VLAN | Red |
|---|---|---|---|---|
| 1 | Naranja | Izquierdo | 10 | `192.188.78.0/27` |
| 2 | Verde | Izquierdo | 20 | `192.188.78.32/27` |
| 3 | Naranja | Derecho | 30 | `192.188.78.64/27` |
| 4 | Verde | Derecho | 40 | `192.188.78.96/27` |
| 5 | ADMIN | Administración | 99 | `192.188.78.128/27` |

### Enlaces de Enrutamiento — Subnetting de `10.4.78.0/24` (/30)

| Enlace | Red | IP Switch A | IP Switch B |
|---|---|---|---|
| MSW4 ↔ MSW1 | `10.4.78.0/30` | `10.4.78.1` | `10.4.78.2` |
| MSW4 ↔ MSW3 | `10.4.78.4/30` | `10.4.78.5` | `10.4.78.6` |
| MSW4 ↔ MSW2 | `10.4.78.8/30` | `10.4.78.9` | `10.4.78.10` |
| Core ↔ Distribución | `10.4.78.12/30` | `10.4.78.13` | `10.4.78.14` |

---

##  Convención de Nombres VLANs (VTP)

```
VLAN_[Color]_Edificio[IZQ/DER]_202204578
```

Ejemplos:
- `VLAN_Naranja_EdificioIZQ_202204578`
- `VLAN_Verde_EdificioDER_202204578`
- `VLAN_Admin_202204578`

---

##  Estructura del Repositorio

```
REDES2_1S2026_202204578/
│
└── Proyecto1/
    ├── README.md          ← Este archivo
    └── chapin_red.pkt     ← Archivo Cisco Packet Tracer
```

---

##  Herramientas Utilizadas

- **Cisco Packet Tracer** — Simulación de la red
- **GitHub** — Control de versiones y entrega
- **UEDI** — Plataforma de entrega oficial

---

*Universidad San Carlos de Guatemala — Facultad de Ingeniería*  
*Redes de Computadoras 2 — Primer Semestre 2026*