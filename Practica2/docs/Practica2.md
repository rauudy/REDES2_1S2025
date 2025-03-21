# Practica 2

## GRUPO 37

| Nombre                                 | Carnet      |
|----------------------------------------|-------------|
| Alejandro René Caballeros González     | 201903549   |
| Raudy David Cabrera Contreras	         | 201901973   |
| Christtopher Jose Chitay Coutino       | 201113851   |


## Objetivos

- 

## Topologia
![imagen](img/topologia.png)


## Configuraciones

### VLANs
37 = 3+7= 10 = 1+0 = 1

Calculo de No. VLAN: 
ADMIN – 10 + 1 = 11
ESTUDIANTES – 20 + 1 = 21
WEB_SERVERS – 30 + 1 = 31
DHCP_SERVERS – 40 + 1 = 41


| **VLAN** | **Color**         | **No.**       | 
|--------------------|--------------------|----------------------|
| ADMIN | **Azul**  | 11    | 
| ESTUDIANTES | **Rosado** | 21   | 
|  WEB_SERVERS | **Verde** | 31    |
| DHCP_SERVERS | **Naranja** | 41    |


### Configuracion de Vlans
 
### S1
```
enable
configure terminal
vlan 11
name ADMIN
vlan 21
name ESTUDIANTES
exit

interface fa0/2
switchport mode access
switchport access vlan 11
no shutdown
exit

interface fa0/1 
switchport mode access
switchport access vlan 21
no shutdown
exit

interface range fa0/3-4  
switchport mode trunk
switchport trunk allowed vlan all  
no shutdown
exit

end
wr
```

### S2
```
enable
configure terminal
vlan 31
name WEB_SERVERS
vlan 41
name DHCP_SERVERS
exit

interface fa0/2
switchport mode access
switchport access vlan 31
no shutdown
exit

interface fa0/3 
switchport mode access
switchport access vlan 41
no shutdown
exit

interface fa0/1
switchport mode trunk
switchport trunk allowed vlan all  
no shutdown
exit

end
wr
```
## Subnetting

## 192.168.37.0/24 Network Segmentation

| **Subnet**          | **Subnet Mask**       | **Usable IPs**            | **Hosts**         |
|---------------------|-----------------------|---------------------------|------------------------------|
| **192.168.37.0/26** | 255.255.255.192       | 192.168.37.1 - 192.168.37.62   | **62**      |
| **192.168.37.64/26**| 255.255.255.192       | 192.168.37.65 - 192.168.37.126 | **62**             |
| **192.168.37.128/26**|255.255.255.192       |192.168.37.129 - 192.168.37.190 | **62**             |
| **192.168.37.192/28**|255.255.255.240      |192.168.37.193 - 192.168.37.206 | **14**   |


## 192.168.100.0/24 Network Segmentation
| **Subnet**            | **Subnet Mask**       | **Usable IPs**            | **Hosts** |
|-----------------------|-----------------------|---------------------------|-----------|
| **192.168.100.0/25**  | 255.255.255.128       | 192.168.100.1 - 192.168.100.126   | **126**  |
| **192.168.100.128/25**| 255.255.255.128       | 192.168.100.129 - 192.168.100.254 | **126**  |

## 10.0.37.0/24 Network Segmentation
| **Subnet**        | **Subnet Mask**       | **Usable IPs**      | **Hosts** |
|--------------------|----------------------|---------------------|-----------|
| **10.0.37.0/30**  | 255.255.255.252      | 10.0.37.1 - 10.0.37.2    | **2**     |
| **10.0.37.4/30**  | 255.255.255.252      | 10.0.37.5 - 10.0.37.6    | **2**     | 
| **10.0.37.8/30**  | 255.255.255.252      | 10.0.37.9 - 10.0.37.10   | **2**     |
| **10.0.37.12/30** | 255.255.255.252      | 10.0.37.13 - 10.0.37.14  | **2**     |
| **10.0.37.16/30** | 255.255.255.252      | 10.0.37.17 - 10.0.37.18  | **2**     |
| **10.0.37.20/30** | 255.255.255.252      | 10.0.37.21 - 10.0.37.22  | **2**     |
| **10.0.37.24/30** | 255.255.255.252      | 10.0.37.25 - 10.0.37.26  | **2**     |



## Configuración de LACP entre edificios
### Se implementa EtherChannel con LACP en modo activo
### El Port-Channel se configura como trunk para transportar VLANs
---
```
enable
configure terminal
```

### Configuración en Switch 1

***Crear EtherChannel con LACP en los puertos físicos***
```
interface range fa0/1 - 4  
channel-group 1 mode active  # Habilita LACP en modo activo
exit

! Configurar Port-Channel como trunk
interface Port-channel1
 switchport trunk encapsulation dot1q  # Define el encapsulado (si es necesario)
 switchport mode trunk  # Configura el enlace como trunk
 switchport trunk allowed vlan 11,21  # Permite solo las VLANs necesarias
 no shutdown  # Asegura que la interfaz esté activa
exit
```


### Configuración en Switch 2

```
! Crear EtherChannel con LACP en los puertos físicos
interface range fa0/11 - 14  
 channel-group 1 mode active  # Habilita LACP en modo activo
exit

! Configurar Port-Channel como trunk
interface Port-channel1
 switchport trunk encapsulation dot1q  # Define el encapsulado (si es necesario)
 switchport mode trunk  # Configura el enlace como trunk
 switchport trunk allowed vlan 31,41  # Permite solo las VLANs necesarias
 no shutdown  # Asegura que la interfaz esté activa
exit

! --------------------------
! Guardar configuración
! --------------------------
end
wr

```

switch central (12)
```
enable
conf term
interface range fa0/1-4
channel-protocol lacp
exit

interface range fa0/11-14
channel-protocol lacp
exit

interface range fa0/21-24
channel-protocol lacp

end 

wr
```

## Configuracion Enutamiento (EIRGP)


#### Multilyer SW1

```
enable
configure terminal
ip routing
interface port-channel 1
no switchport
ip address 10.0.37.5 255.255.255.252
no shutdown
exit

interface fa0/11
no switchport
ip address 10.0.37.1 255.255.255.252
exit

router eigrp 100
network 10.0.37.0 0.0.0.3
network 10.0.37.4 0.0.0.3
no auto-summary
exit

end 
wr
```

#### Multilyer SW0

```
enable
configure terminal
ip routing
interface port-channel 1
no switchport
ip address 10.0.37.6 255.255.255.252
no shutdown
exit

interface port-channel 2
no switchport
ip address 10.0.37.25 255.255.255.252
no shutdown
exit

interface port-channel 3
no switchport
ip address 10.0.37.17 255.255.255.252
no shutdown
exit

interface gig0/1
no switchport
ip address 10.0.37.9 255.255.255.252
exit

interface gig0/2
no switchport
ip address 10.0.37.13 255.255.255.252
exit

router eigrp 100
network 10.0.37.4 0.0.0.3
network 10.0.37.8 0.0.0.3
network 10.0.37.12 0.0.0.3
network 10.0.37.16 0.0.0.3
network 10.0.37.24 0.0.0.3
no auto-summary
exit

end 
wr
```

#### Multilyer SW2

```
enable
configure terminal
ip routing
interface port-channel 1
no switchport
ip address 10.0.37.18 255.255.255.252
no shutdown
exit

interface fa0/11
no switchport
ip address 10.0.37.21 255.255.255.252
exit

router eigrp 100
network 10.0.37.16 0.0.0.3
network 10.0.37.20 0.0.0.3
no auto-summary
exit

end 
wr
```

#### Multilyer SW3

```
enable
configure terminal
ip routing
interface port-channel 1
no switchport
ip address 10.0.37.26 255.255.255.252
no shutdown
exit

interface fa0/11
no switchport
no shutdown
exit

interface vlan 31
ip address 192.168.100.1 255.255.255.128
no shutdown
exit

interface vlan 41
ip address 192.168.100.129 255.255.255.128
no shutdown
exit

router eigrp 100
network 10.0.37.24 0.0.0.3
network 192.168.100.0 0.0.0.127
network 192.168.100.128 0.0.0.127
no auto-summary
exit

end 
wr
```

#### Router0

```
enable
configure terminal

interface gig0/1
ip address 10.0.37.10 255.255.255.252
no shutdown
exit

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.21
encapsulation dot1Q 21
ip address 192.168.37.1 255.255.255.192
exit

interface gigabitEthernet 0/0.11
encapsulation dot1Q 11
ip address 192.168.37.193 255.255.255.240
exit

router eigrp 100
network 10.0.37.8 0.0.0.3
network 192.168.37.0 0.0.0.63
network 192.168.37.192 0.0.0.15
no auto-summary
exit

end 
wr
```

#### Router1
```
enable
configure terminal

interface gig0/1
ip address 10.0.37.14 255.255.255.252
no shutdown
exit

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.21
encapsulation dot1Q 21
ip address 192.168.37.2 255.255.255.192
exit

interface gigabitEthernet 0/0.11
encapsulation dot1Q 11
ip address 192.168.37.194 255.255.255.240
exit

router eigrp 100
network 10.0.37.12 0.0.0.3
network 192.168.37.0 0.0.0.63
network 192.168.37.192 0.0.0.15
no auto-summary
exit

end 
wr
```


## VRRP (HRRP en Packet Tracer - standby)


### Router0

```
enable
configure terminal

! Configuración para la VLAN ESTUDIANTES-21
interface GigabitEthernet0/0.21
standby 21 ip 192.168.37.3
standby 21 priority 110
standby 21 preempt
exit

! Configuración para la VLAN ADMIN-11
interface GigabitEthernet0/0.11
standby 11 ip 192.168.37.195
standby 11 priority 90
standby 11 preempt
exit

end
wr

show standby brief

```
### Router1

```
enable
configure terminal

! Configuración para la VLAN ESTUDIANTES-21
interface GigabitEthernet0/0.21
standby 21 ip 192.168.37.3
standby 21 priority 90
standby 21 preempt
exit

! Configuración para la VLAN ADMIN-11
interface GigabitEthernet0/0.11
standby 11 ip 192.168.37.195
standby 11 priority 110
standby 11 preempt
exit

end
wr

show standby brief

```
