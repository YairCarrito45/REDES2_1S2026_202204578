## Estiben Yair Lopez Leveron - 202204578
## Descripción del Proyecto

**Chapin Red** es una organización sin fines de lucro dedicada a la planificación y ejecución de proyectos de ayuda humanitaria en comunidades vulnerables. Para llevar a cabo sus operaciones, la organización cuenta con cuatro sedes distribuidas geográficamente dentro de una misma área metropolitana, cada una con funciones específicas: coordinación de proyectos, administración, logística y operaciones en campo.

Ante el crecimiento de la organización y la necesidad de una comunicación eficiente entre sus sedes, Chapin Red requiere una infraestructura de red robusta, segura y escalable. La red existente presentaba los siguientes problemas:

- Falta de segmentación de red
- Ausencia de control de acceso entre departamentos
- Todos los dispositivos en el mismo dominio de broadcast
- Sin redundancia en enlaces críticos

Este proyecto documenta el diseño e implementación de dicha infraestructura utilizando **Cisco Packet Tracer**, resolviendo cada uno de estos problemas mediante las siguientes tecnologías:

| Componente | Tecnología |
|---|---|
| Conexión entre edificios | Fibra óptica (GigabitEthernet - GLC-LH-SMD) |
| Protocolo de enrutamiento | OSPF (Open Shortest Path First) — carné par |
| Segmentación | 5 VLANs distribuidas entre edificios |
| Alta disponibilidad | LACP (edificio izquierdo) y PAgP (edificio derecho) |
| Asignación de IPs | DHCP dinámico — sin IPs estáticas en dispositivos finales |
| Control de acceso | ACLs entre VLANs |
| Prevención de bucles | STP (Spanning Tree Protocol) |

---

## Edificios y Roles

| Edificio | Rol | Switches | VLANs |
|---|---|---|---|
| Edificio Izquierdo | Arquitectura jerarquica 3 capas | Core + Distribucion (3560) + Acceso (2960) | Naranja + Verde |
| Edificio Derecho | Red con PAgP | MSW 3650 | Naranja + Verde |
| Edificio Administracion | VLAN ADMIN | MSW 3650 | ADMIN |
| Edificio Central (MAN) | Nucleo de interconexion + Servidores DHCP | MSW 3650 | — |