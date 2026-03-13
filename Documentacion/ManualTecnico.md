# Proyecto 1 --- Chapin Red

### Redes de Computadoras 2

Universidad de San Carlos de Guatemala\
Facultad de Ingeniería --- Ingeniería en Ciencias y Sistemas

**Estudiante:** Estiben Yair López Leverón\
**Carné:** 202204578\
**Semestre:** 1S 2026

------------------------------------------------------------------------

# 1. Descripción del Proyecto

**Chapin Red** es una empresa dedicada a proyectos de ayuda humanitaria
que opera desde cuatro edificios distribuidos geográficamente dentro de
una misma área metropolitana.

El objetivo del proyecto es diseñar e implementar una **infraestructura
de red corporativa multi-edificio** utilizando **Cisco Packet Tracer**,
aplicando conceptos de redes empresariales.

La red implementa:

-   Arquitectura jerárquica de tres capas
-   Segmentación mediante VLANs
-   Enrutamiento dinámico con OSPF
-   Agregación de enlaces con LACP y PAgP
-   Servidores DHCP
-   Políticas de seguridad con ACLs
-   Conexión MAN entre edificios

------------------------------------------------------------------------

# 2. Topología de Red

La red está compuesta por **cuatro edificios interconectados** mediante
switches multicapa Cisco.

## Topología de Red
![alt text](image.png)

## Dispositivos utilizados

  Switch   Modelo       Rol                  Edificio
  -------- ------------ -------------------- ----------------
  MS1      Cisco 3650   Switch central MAN   Central
  MS7      Cisco 3650   Switch MAN           Izquierdo
  MS2      Cisco 3650   Switch MAN           Derecho
  MS6      Cisco 3650   Switch MAN           Administración
  MS9      Cisco 3560   Core                 Izquierdo
  MS8      Cisco 3560   Distribución         Izquierdo
  MS4      Cisco 3560   Core/Distribución    Derecho
  MS5      Cisco 3560   Distribución         Derecho
  SW1      Cisco 2960   Acceso               Izquierdo
  SW2      Cisco 2960   Acceso               Izquierdo
  SW3      Cisco 2960   Acceso               Derecho
  SW4      Cisco 2960   Acceso               Derecho

------------------------------------------------------------------------

# 3. Planificación y Subnetting

## Redes base

De acuerdo con los últimos dos dígitos del carné **78**, se asignan las
siguientes redes:

  Red           Dirección         Uso
  ------------- ----------------- ----------------------------------
  Red VLAN      192.188.78.0/24   VLANs de usuarios
  Red Routing   10.4.78.0/24      Enlaces entre switches multicapa

------------------------------------------------------------------------

# 4. Subnetting de VLANs (VLSM)

Se utilizó **VLSM** para dividir la red en cinco subredes.

  -----------------------------------------------------------------------------------------------
  VLAN        Nombre      Red                 Máscara           Gateway          Broadcast
  ----------- ----------- ------------------- ----------------- ---------------- ----------------
  10          Naranja IZQ 192.188.78.0/27     255.255.255.224   192.188.78.1     192.188.78.31

  20          Verde IZQ   192.188.78.32/27    255.255.255.224   192.188.78.33    192.188.78.63

  30          Naranja DER 192.188.78.64/27    255.255.255.224   192.188.78.65    192.188.78.95

  40          Verde DER   192.188.78.96/27    255.255.255.224   192.188.78.97    192.188.78.127

  99          ADMIN       192.188.78.128/28   255.255.255.240   192.188.78.129   192.188.78.143
  -----------------------------------------------------------------------------------------------

------------------------------------------------------------------------

# 5. Subnetting de Enlaces (/30)

Los enlaces entre dispositivos de capa 3 utilizan subredes **/30**.

  Enlace       Red             IP A         IP B
  ------------ --------------- ------------ ------------
  MS1 -- MS7   10.4.78.0/30    10.4.78.1    10.4.78.2
  MS1 -- MS2   10.4.78.4/30    10.4.78.5    10.4.78.6
  MS1 -- MS3   10.4.78.8/30    10.4.78.9    10.4.78.10
  MS7 -- MS6   10.4.78.12/30   10.4.78.13   10.4.78.14
  MS2 -- MS6   10.4.78.16/30   10.4.78.17   10.4.78.18
  MS3 -- MS6   10.4.78.20/30   10.4.78.21   10.4.78.22

------------------------------------------------------------------------

# 6. VLANs

  VLAN   Nombre                               Departamento
  ------ ------------------------------------ ----------------
  10     VLAN_Naranja_EdificioIZQ_202204578   Proyectos
  20     VLAN_Verde_EdificioIZQ_202204578     Coordinación
  30     VLAN_Naranja_EdificioDER_202204578   Proyectos
  40     VLAN_Verde_EdificioDER_202204578     Coordinación
  99     VLAN_ADMIN_202204578                 Administración

------------------------------------------------------------------------

# 7. VTP (VLAN Trunking Protocol)

  Parámetro    Valor
  ------------ -----------
  Dominio      ChapinRed
  Contraseña   chapin123
  Versión      2

------------------------------------------------------------------------

# 8. Agregación de Enlaces

## LACP

Configuración utilizada para agregación en el edificio izquierdo.

## PAgP

Configuración utilizada para agregación en el edificio derecho.

------------------------------------------------------------------------

# 9. Enrutamiento Dinámico

Debido a que el carné **202204578 es par**, se utiliza **OSPF**.

Configuración base:

router ospf 1 network \[red\] \[wildcard\] area 0

------------------------------------------------------------------------

# 10. DHCP

Se configuraron dos servidores DHCP.

**DHCP1 --- Edificio Izquierdo** - VLAN10 - VLAN20

**DHCP2 --- Edificio Derecho** - VLAN30 - VLAN40 - VLAN99

------------------------------------------------------------------------

# 11. ACLs

Las ACLs implementan las políticas de seguridad:

-   Naranja ↔ Naranja permitido
-   Verde ↔ Verde permitido
-   Naranja ↔ Verde bloqueado
-   VLANs → ADMIN bloqueado
-   ADMIN → todas permitido

------------------------------------------------------------------------

# 12. Comandos de Verificación

show vlan brief\
show vtp status\
show interfaces trunk\
show etherchannel summary\
show ip route\
show ip ospf neighbor\
show access-lists

------------------------------------------------------------------------

## VTP - VLAN Trunking Protocol

VTP permite sincronizar automáticamente la configuración de VLANs entre todos los switches del dominio, evitando configurarlas manualmente en cada dispositivo.

## Configuracion Vlans

**configuracion MS1 Vlan**
```
enable
configure terminal

vlan 10 
name VLAN_Naranja_EdificioIZQ_202204578
exit

vlan 20
name VLAN_Verde_EdificioIZQ_202204578

vlan 30
name VLAN_Naranja_EdificioDER_202204578
exit

vlan 40
name VLAN_Verde_EdificioDER_202204578
exit

vlan 99
name VLAN_ADMIN_202204578
exit

end
write memory

```


### trunks por switch
**MS1 a DCHP1, DHCP2, MS7 Y MS2**
```
interface gigabitEthernet 1/1/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit


interface gigabitEthernet 1/1/1  ← hacia MS2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit


! Hacia DHCP1
interface gigabitEthernet 1/0/1
switchport mode access
switchport access vlan 99
no shutdown
exit

! Hacia DHCP2
interface gigabitEthernet 1/0/2
switchport mode access
switchport access vlan 99
no shutdown
exit
```

**MS7 a MS1, MS2, MS6, MS9 Y MS8**
```
enable
configure terminal

! Hacia MS1 (Red MAN)
interface gigabitEthernet 1/1/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS2 (Red MAN)
interface gigabitEthernet 1/1/3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS6 (Red MAN)
interface gigabitEthernet 1/1/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS9 (Edificio Izquierdo)
interface gigabitEthernet X/X/X
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS8 (Edificio Izquierdo)
interface gigabitEthernet X/X/X
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```

**MS2 a MS6, MS1, MS7, MS3**

```

enable
configure terminal
interface gigabitEthernet 1/1/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

interface gigabitEthernet 1/1/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

interface gigabitEthernet 1/1/3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit
exit
write memory
```

**MS6 a MS2, MS7**
```
enable
configure terminal
interface gigabitEthernet 1/1/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit


interface gigabitEthernet 1/1/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit


! Hacia PC0 → access VLAN 99 Admin
interface GigabitEthernet1/0/10
switchport mode access
switchport access vlan 99
no shutdown
exit



exit
write memory

```

### Configuración VTP

| Parámetro | Valor |
|-----------|-------|
| Dominio | ChapinRed |
| Contraseña | 123 |
| Versión | 2 |

### Roles VTP

| Switch | Rol VTP | Motivo |
|--------|---------|--------|
| MS1 ,MS7, MS2, | **Server** | Switch central, propaga VLANs a toda la red |
| MS9, MS8, MS4, MS5, MS6| Client | Switches internos de edificios |
| SW1, SW2, SW3, SW4 | Client | Switches de acceso |

### Comandos VTP

**En MS1 (Servidor):**
```
enable
configure terminal
hostname MS1
vtp mode server
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```


**En MS7 (Servidor)**
```
enable
configure terminal
hostname MS7
vtp mode server
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```


**En MS2 (Servidor)**
```
enable
configure terminal
hostname MS2
vtp mode server
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS6 (cliente)**
```
enable
configure terminal
hostname MS6
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS9 (cliente)**
```
enable
configure terminal
hostname MS9
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS8 (cliente)**
```
enable
configure terminal
hostname MS8
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS3 (cliente)**
```
enable
configure terminal
hostname MS3
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS4 (cliente)**
```
enable
configure terminal
hostname MS4
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En MS5 (cliente)**
```
enable
configure terminal
hostname MS5
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En SW1 (cliente)**
```
enable
configure terminal
hostname SW1
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En SW2 (cliente)**
```
enable
configure terminal
hostname SW2
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En SW3 (cliente)**
```
enable
configure terminal
hostname SW3
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**En SW4 (cliente)**
```
enable
configure terminal
hostname SW4
vtp mode client
vtp domain ChapinRed
vtp password chapin123
vtp version 2
end
write memory
```

**Verificación:**
```
show vtp status
```

---

## Agregación de Enlaces (LACP y PAgP)

La agregación de enlaces combina múltiples conexiones físicas en un único enlace lógico, aumentando el ancho de banda y proporcionando redundancia.

### LACP — Edificio Izquierdo (5 enlaces)

LACP (Link Aggregation Control Protocol) es el estándar IEEE 802.3ad, compatible con cualquier fabricante.


### COMANDO LACP

**MS9**
```
enable
configure terminal

! Hacia MS7 → channel-group 1
interface range GigabitEthernet1/0/1 - 3
channel-group 1 mode active
no shutdown
exit
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS8 → channel-group 3
interface range GigabitEthernet1/0/7 - 9
channel-group 3 mode active
no shutdown
exit
interface port-channel 3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia SW1 → channel-group 5
interface range GigabitEthernet1/0/4 - 5
channel-group 5 mode active
no shutdown
exit
interface port-channel 5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```


**MS7**
```
enable
configure terminal

! Hacia MS9 → channel-group 1
interface range GigabitEthernet1/0/1 - 3
channel-group 1 mode active
no shutdown
exit
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS8 → channel-group 2
interface range GigabitEthernet1/0/4 - 6
channel-group 2 mode active
no shutdown
exit
interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```

**MS8**
```
enable
configure terminal

! Hacia MS9 → channel-group 3
interface range GigabitEthernet1/0/7 - 9
channel-group 3 mode active
no shutdown
exit
interface port-channel 3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS7 → channel-group 2
interface range GigabitEthernet1/0/4 - 6
channel-group 2 mode active
no shutdown
exit
interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia SW2 → channel-group 4
interface range GigabitEthernet1/0/1 - 2
channel-group 4 mode active
no shutdown
exit
interface port-channel 4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```

**SW1**
```
enable
configure terminal

! Hacia MS9 → channel-group 5
interface range GigabitEthernet0/1 - 2
channel-group 5 mode active
no shutdown
exit
interface port-channel 5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia PC1 → VLAN 10 Naranja
interface FastEthernet0/1
switchport mode access
switchport access vlan 10
no shutdown
exit

! Hacia PC2 → VLAN 10 VERDE
interface FastEthernet0/2
switchport mode access
switchport access vlan 20
no shutdown
exit

end
write memory
```

**SW2**
```
enable
configure terminal

! Hacia MS8 → channel-group 4
interface range GigabitEthernet0/1 - 2
channel-group 4 mode active
no shutdown
exit
interface port-channel 4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia Laptop0 → VLAN 10 Naranja
interface FastEthernet0/10
switchport mode access
switchport access vlan 20
no shutdown
exit

! Hacia Laptop1 → VLAN 20 Verde
interface FastEthernet0/11
switchport mode access
switchport access vlan 20
no shutdown
exit

end
write memory
```
### PAgP — Edificio Derecho (3 enlaces)

PAgP (Port Aggregation Protocol) es el protocolo propietario de Cisco.

**MS3**
```
enable
configure terminal

! Hacia MS3 → Po1
interface range GigabitEthernet1/0/1 - 3
channel-group 1 mode desirable
no shutdown
exit
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```
**MS3**
```
enable
configure terminal

! Hacia MS2 → Po1
interface range GigabitEthernet1/0/1 - 3
channel-group 1 mode desirable
no shutdown
exit
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS4 → Po2
interface range GigabitEthernet1/0/4 - 6
channel-group 2 mode desirable
no shutdown
exit
interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```


**MS4**
```
enable
configure terminal

! Hacia MS3 → Po2
interface range GigabitEthernet1/0/4 - 6
channel-group 2 mode desirable
no shutdown
exit
interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia MS5 → Po3
interface range GigabitEthernet1/0/1 - 3
channel-group 3 mode desirable
no shutdown
exit
interface port-channel 3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory

```
**MS5**
```
enable
configure terminal

! Hacia MS4 → Po3
interface range GigabitEthernet1/0/1 - 3
channel-group 3 mode desirable
no shutdown
exit
interface port-channel 3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia SW3 → trunk normal
interface GigabitEthernet1/0/10
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia SW4 → trunk normal
interface GigabitEthernet1/0/11
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

end
write memory
```

**SW3**
```
enable
configure terminal

! Hacia MS5 → trunk
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia Laptop2 → VLAN 40 Verde
interface FastEthernet0/10
switchport mode access
switchport access vlan 40
no shutdown
exit

! Hacia PC3 → VLAN 30 Naranja
interface FastEthernet0/11
switchport mode access
switchport access vlan 30
no shutdown
exit

end
write memory
```

**SW4**
```
enable
configure terminal

! Hacia MS5 → trunk
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
no shutdown
exit

! Hacia Laptop3 → VLAN 40 Verde
interface FastEthernet0/10
switchport mode access
switchport access vlan 40
no shutdown
exit

! Hacia PC4 → VLAN 30 Naranja
interface FastEthernet0/11
switchport mode access
switchport access vlan 30
no shutdown
exit

end
write memory
```

**Verificación de EtherChannels:**
```
show etherchannel summary
show etherchannel detail
```



---

## Enrutamiento Dinámico (OSPF)

OSPF (Open Shortest Path First) es el protocolo asignado para carné par (202204578). Calcula las rutas más eficientes basándose en el costo de los enlaces.

### Configuración OSPF en switches multicapa

```
configure terminal
ip routing
router ospf 1
network [red] [wildcard] area 0
end
write memory
```

**MS1**
```
enable
configure terminal

ip routing

! Hacia MS7
interface GigabitEthernet 1/1/2
no switchport
ip address 10.4.78.1 255.255.255.252
no shutdown
exit

! Hacia MS2
interface GigabitEthernet 1/1/1
no switchport
ip address 10.4.78.5 255.255.255.252
no shutdown
exit

end
write memory

enable
configure terminal

router ospf 1
network 10.4.78.0 0.0.0.3 area 0
network 10.4.78.4 0.0.0.3 area 0
end
write memory

```

**MS7**
```
enable
configure terminal

ip routing

! Hacia MS1
interface GigabitEthernet 1/1/2
no switchport
ip address 10.4.78.2 255.255.255.252
no shutdown
exit

! Hacia MS2
interface GigabitEthernet 1/1/3
no switchport
ip address 10.4.78.9 255.255.255.252
no shutdown
exit

! Hacia MS6
interface GigabitEthernet 1/1/1
no switchport
ip address 10.4.78.13 255.255.255.252
no shutdown
exit

! Hacia MS9 (Po1)
interface port-channel 1
no switchport
ip address 10.4.78.33 255.255.255.252
no shutdown
exit

! Hacia MS8 (Po2)
interface port-channel 2
no switchport
ip address 10.4.78.37 255.255.255.252
no shutdown
exit

end
write memory


enable
configure terminal

router ospf 1
network 10.4.78.0 0.0.0.3 area 0
network 10.4.78.8 0.0.0.3 area 0
network 10.4.78.12 0.0.0.3 area 0
network 10.4.78.32 0.0.0.3 area 0
network 10.4.78.36 0.0.0.3 area 0
end
write memory

```

**MS2**
```
enable
configure terminal

ip routing

! Hacia MS1
interface GigabitEthernet 1/1/1
no switchport
ip address 10.4.78.6 255.255.255.252
no shutdown
exit

! Hacia MS7
interface GigabitEthernet 1/1/3
no switchport
ip address 10.4.78.10 255.255.255.252
no shutdown
exit

! Hacia MS6
interface GigabitEthernet 1/1/2
no switchport
ip address 10.4.78.17 255.255.255.252
no shutdown
exit

! Hacia MS3 (Po1)
interface port-channel 1
no switchport
ip address 10.4.78.21 255.255.255.252
no shutdown
exit

end
write memory


enable
configure terminal

router ospf 1
network 10.4.78.4 0.0.0.3 area 0
network 10.4.78.8 0.0.0.3 area 0
network 10.4.78.16 0.0.0.3 area 0
network 10.4.78.20 0.0.0.3 area 0
end
write memory

```


**MS6**
```
enable
configure terminal

ip routing

! Hacia MS7
interface GigabitEthernet 1/1/1
no switchport
ip address 10.4.78.14 255.255.255.252
no shutdown
exit

! Hacia MS2
interface GigabitEthernet 1/1/2
no switchport
ip address 10.4.78.18 255.255.255.252
no shutdown
exit

end
write memory


enable
configure terminal

router ospf 1
network 10.4.78.12 0.0.0.3 area 0
network 10.4.78.16 0.0.0.3 area 0
network 192.188.78.128 0.0.0.15 area 0
end
write memory

```

**MS3**
```
enable
configure terminal

ip routing

! Hacia MS2 (Po1)
interface port-channel 1
no switchport
ip address 10.4.78.22 255.255.255.252
no shutdown
exit

! Hacia MS4 (Po2)
interface port-channel 2
no switchport
ip address 10.4.78.25 255.255.255.252
no shutdown
exit

end
write memory

enable
configure terminal

router ospf 1
network 10.4.78.20 0.0.0.3 area 0
network 10.4.78.24 0.0.0.3 area 0
end
write memory
```


**MS4**
```
enable
configure terminal

ip routing

! Hacia MS3 (Po2)
interface port-channel 2
no switchport
ip address 10.4.78.26 255.255.255.252
no shutdown
exit

! Hacia MS5 (Po3)
interface port-channel 3
no switchport
ip address 10.4.78.29 255.255.255.252
no shutdown
exit

end
write memory


enable
configure terminal

router ospf 1
network 10.4.78.24 0.0.0.3 area 0
network 10.4.78.28 0.0.0.3 area 0
end
write memory
```

**MS5**
```
enable
configure terminal

ip routing

! Hacia MS4 (Po3)
interface port-channel 3
no switchport
ip address 10.4.78.30 255.255.255.252
no shutdown
exit

end
write memory
```

**MS9**
```
enable
configure terminal

! Hacia MS7 (Po1)
interface port-channel 1
no switchport
ip address 10.4.78.34 255.255.255.252
no shutdown
exit

! Hacia MS8 (Po3)
interface port-channel 3
no switchport
ip address 10.4.78.41 255.255.255.252
no shutdown
exit

end
write memory

enable
configure terminal

router ospf 1
network 10.4.78.32 0.0.0.3 area 0
network 10.4.78.40 0.0.0.3 area 0
network 192.188.78.0 0.0.0.31 area 0
network 192.188.78.32 0.0.0.31 area 0
end
write memory
```

**MS8**
```
enable
configure terminal

! Hacia MS7 (Po2)
interface port-channel 2
no switchport
ip address 10.4.78.38 255.255.255.252
no shutdown
exit

! Hacia MS9 (Po3)
interface port-channel 3
no switchport
ip address 10.4.78.42 255.255.255.252
no shutdown
exit

end
write memory
```

**Verificación:**
```
show ip ospf neighbor
show ip route ospf
```

---


## DHCP

El **Dynamic Host Configuration Protocol (DHCP)** permite asignar automáticamente direcciones IP, máscara de subred, puerta de enlace y otros parámetros de red a los dispositivos finales, evitando configuraciones manuales en cada host.

En esta topología se utilizan **dos servidores DHCP**, uno para cada edificio, con el objetivo de distribuir la carga y mantener una organización clara de las redes.

---

# Servidor DHCP Izquierdo (DHCP1)

Este servidor atiende las VLAN correspondientes al **edificio izquierdo**.

| Pool | Red | Gateway | Rango de Hosts |
|-----|-----|-----|-----|
| VLAN10_Naranja_IZQ | 192.188.78.0 /27 | 192.188.78.1 | 192.188.78.2 → 192.188.78.30 |
| VLAN20_Verde_IZQ | 192.188.78.32 /27 | 192.188.78.33 | 192.188.78.34 → 192.188.78.62 |

### Configuración en DHCP1 (Packet Tracer)

Pool VLAN10:

- Pool Name: `VLAN10_Naranja_IZQ`
- Default Gateway: `192.188.78.1`
- Start IP Address: `192.188.78.2`
- Subnet Mask: `255.255.255.224`
- Maximum Users: `29`

Pool VLAN20:

- Pool Name: `VLAN20_Verde_IZQ`
- Default Gateway: `192.188.78.33`
- Start IP Address: `192.188.78.34`
- Subnet Mask: `255.255.255.224`
- Maximum Users: `29`

---

# Servidor DHCP Derecho (DHCP2)

Este servidor atiende las VLAN correspondientes al **edificio derecho** y la red de administración.

| Pool | Red | Gateway | Rango de Hosts |
|-----|-----|-----|-----|
| VLAN30_Naranja_DER | 192.188.78.64 /27 | 192.188.78.65 | 192.188.78.66 → 192.188.78.94 |
| VLAN40_Verde_DER | 192.188.78.96 /27 | 192.188.78.97 | 192.188.78.98 → 192.188.78.126 |
| VLAN99_ADMIN | 192.188.78.128 /28 | 192.188.78.129 | 192.188.78.130 → 192.188.78.142 |

### Configuración en DHCP2 (Packet Tracer)

Pool VLAN30:

- Pool Name: `VLAN30_Naranja_DER`
- Default Gateway: `192.188.78.65`
- Start IP Address: `192.188.78.66`
- Subnet Mask: `255.255.255.224`
- Maximum Users: `29`

Pool VLAN40:

- Pool Name: `VLAN40_Verde_DER`
- Default Gateway: `192.188.78.97`
- Start IP Address: `192.188.78.98`
- Subnet Mask: `255.255.255.224`
- Maximum Users: `29`

Pool VLAN99:

- Pool Name: `VLAN99_ADMIN`
- Default Gateway: `192.188.78.129`
- Start IP Address: `192.188.78.130`
- Subnet Mask: `255.255.255.240`
- Maximum Users: `11`

---

# DHCP Relay (IP Helper)

Las solicitudes DHCP son **broadcast**, lo que significa que normalmente no pueden atravesar routers o switches capa 3.

Para permitir que los dispositivos de diferentes VLAN puedan obtener dirección IP desde los servidores DHCP, se implementa **DHCP Relay** mediante el comando:

```
ip helper-address
```

Este comando reenvía las solicitudes DHCP hacia el servidor correspondiente.

---

# Configuración DHCP Relay en los switches de capa 3

## MS9 (Edificio Izquierdo)

MS9 gestiona las VLAN del edificio izquierdo.

```bash
enable
configure terminal

interface vlan 10
ip helper-address 192.188.78.146
no shutdown
exit

interface vlan 20
ip helper-address 192.188.78.146
no shutdown
exit

end
write memory
```

---

## MS5 (Edificio Derecho)

MS5 gestiona las VLAN del edificio derecho.

```bash
enable
configure terminal

interface vlan 30
ip helper-address 192.188.78.150
no shutdown
exit

interface vlan 40
ip helper-address 192.188.78.150
no shutdown
exit

end
write memory
```

---

## MS6 (Red de Administración)

MS6 actúa como gateway de la VLAN de administración.

```bash
enable
configure terminal

interface vlan 99
ip helper-address 192.188.78.150
no shutdown
exit

end
write memory
```

---

# Configuración en los dispositivos finales

Los dispositivos finales (PCs y laptops) se configuran para obtener dirección IP automáticamente.

En Packet Tracer:

```
Desktop
IP Configuration
DHCP
```

El servidor DHCP asigna automáticamente:

- Dirección IP
- Máscara de subred
- Puerta de enlace
- DNS

---

# Comandos de verificación utilizados

Para comprobar el funcionamiento del DHCP y de la red se utilizaron los siguientes comandos:

Ver interfaces VLAN:

```
show ip interface brief
```

Ver rutas aprendidas por OSPF:

```
show ip route
```

Ver enlaces troncales:

```
show interfaces trunk
```

Ver estado de EtherChannel:

```
show etherchannel summary
```

Probar conectividad entre dispositivos:

```
ping [direccion IP]
```

---


## ACLs - Control de Acceso

Las ACLs (Access Control Lists) controlan qué tráfico puede pasar entre VLANs, implementando las políticas de seguridad de la empresa.

### Políticas requeridas

| VLAN origen | VLAN destino | ¿Permitido? |
|-------------|--------------|-------------|
| Naranja IZQ | Naranja DER | Sí |
| Naranja | Verde |  No |
| Naranja | ADMIN |  No |
| Verde IZQ | Verde DER |  Sí |
| Verde | Naranja |  No |
| Verde | ADMIN |  No |
| ADMIN | Cualquier VLAN |  Sí (puede iniciar) |
| Cualquier VLAN | ADMIN |  No (no puede iniciar hacia ADMIN) |

### Implementación técnica

```
! ACL para VLAN Naranja - solo permite comunicación entre Naranjas
ip access-list extended ACL_NARANJA
 permit ip 192.188.78.0 0.0.0.31 192.188.78.64 0.0.0.31
 permit ip 192.188.78.64 0.0.0.31 192.188.78.0 0.0.0.31
 deny ip any any

! ACL para VLAN Verde - solo permite comunicación entre Verdes
ip access-list extended ACL_VERDE
 permit ip 192.188.78.32 0.0.0.31 192.188.78.96 0.0.0.31
 permit ip 192.188.78.96 0.0.0.31 192.188.78.32 0.0.0.31
 deny ip any any

! ACL para VLAN ADMIN - bloquea tráfico entrante desde otras VLANs
ip access-list extended ACL_ADMIN_IN
 deny ip 192.188.78.0 0.0.0.31 192.188.78.128 0.0.0.15
 deny ip 192.188.78.32 0.0.0.31 192.188.78.128 0.0.0.15
 deny ip 192.188.78.64 0.0.0.31 192.188.78.128 0.0.0.15
 deny ip 192.188.78.96 0.0.0.31 192.188.78.128 0.0.0.15
 permit ip any any
```

---

## Comandos Principales

### Comandos de verificación generales

| Comando | Descripción |
|---------|-------------|
| `show vtp status` | Verifica configuración VTP |
| `show vlan brief` | Lista todas las VLANs |
| `show interfaces trunk` | Muestra puertos en modo trunk |
| `show etherchannel summary` | Estado de los EtherChannels |
| `show ip ospf neighbor` | Vecinos OSPF activos |
| `show ip route` | Tabla de enrutamiento |
| `show ip interface brief` | Estado de interfaces |
| `show access-lists` | Muestra las ACLs configuradas |

### Comandos de configuración de VLANs

```
vlan [ID]
name [NOMBRE]
exit
```

### Configuración de puertos Access y Trunk

```
! Puerto Access
interface FastEthernet0/X
switchport mode access
switchport access vlan [ID]
spanning-tree portfast
no shutdown

! Puerto Trunk
interface GigabitEthernet1/0/X
switchport mode trunk
switchport trunk allowed vlan [IDs]
no shutdown
```
## Configuración de interfaces de capa 3 (SVI)

### ¿Por qué estas SVIs en estos switches?

El enunciado define tres capas jerárquicas para el edificio izquierdo:
- **Core (MS9):** conecta hacia la MAN (MS7) y hacia Distribución (MS8)
- **Distribución (MS8):** conecta hacia Acceso
- **Acceso (SW1, SW2):** conecta dispositivos finales

Sin embargo, en la topología implementada **MS9 conecta directamente a SW1** 
(verificado con `show cdp neighbors`), por lo que MS9 actúa como gateway de 
los dispositivos de SW1. MS8 conecta directamente a SW2, pero dado que MS9 
ya maneja el inter-VLAN routing de ambas VLANs (10 y 20), MS8 solo necesita 
`ip routing` para participar en OSPF y reenviar tráfico.

### Sintaxis general
```
interface vlan [ID]
ip address [IP] [MASCARA]
no shutdown
```

---

### MS9 (Core - Edificio Izquierdo)
Gateway de SW1. Maneja inter-VLAN routing de VLAN 10 y VLAN 20.
```
enable
configure terminal

ip routing

! SVI VLAN 10 - Naranja IZQ - gateway de PC1
interface vlan 10
ip address 192.188.78.1 255.255.255.224
no shutdown
exit

! SVI VLAN 20 - Verde IZQ - gateway de PC2
interface vlan 20
ip address 192.188.78.33 255.255.255.224
no shutdown
exit

end
write memory
```

---

### MS8 (Distribución - Edificio Izquierdo)
Conecta a SW2 y a MS9. No actúa como gateway de VLANs de usuario
ya que MS9 maneja el inter-VLAN routing. Solo requiere `ip routing`
para participar en OSPF.
```
enable
configure terminal

ip routing

end
write memory
```

---

### MS5 (Distribución - Edificio Derecho)
Gateway de SW3 y SW4. Maneja inter-VLAN routing de VLAN 30 y VLAN 40.
```
enable
configure terminal

ip routing

! SVI VLAN 30 - Naranja DER - gateway de PC3, PC4
interface vlan 30
ip address 192.188.78.65 255.255.255.224
no shutdown
exit

! SVI VLAN 40 - Verde DER - gateway de Laptop2, Laptop3
interface vlan 40
ip address 192.188.78.97 255.255.255.224
no shutdown
exit

end
write memory
```

---

### MS6 (Edificio Administración)
Gateway de PC0 Admin. Maneja la VLAN 99 exclusiva del departamento 
de administración.
```
enable
configure terminal

ip routing

! SVI VLAN 99 - ADMIN - gateway de PC0 Admin
interface vlan 99
ip address 192.188.78.129 255.255.255.240
no shutdown
exit

end
write memory
```

*Proyecto 1 - Chapin Red | Redes de Computadoras 2 | USAC 2026*