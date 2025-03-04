# Proyecto 1

## GRUPO 37

| Nombre                                 | Carnet      |
|----------------------------------------|-------------|
| Alejandro René Caballeros González     | 201903549   |
| Raudy David Cabrera Contreras	         | 201901973   |
| Christtopher Jose Chitay Coutino       | 201113851   |


## Objetivos

- Realizar las configuraciones de switches multicapa y capa 2.
- Implementar los protocolos de capa 3: RIP, OSPF, EIGRP y BGP.
- Aplicar los conocimientos de redes MAN, LAN y WAN.
- Aplicar los conocimientos de LACP.y PAGP
- Implementar ACL’s.
- Familiarizarse con las configuraciones de DHCP y sus conceptos.

## Topologia
![imagen](img/)


## Configuraciones

### Switches Capa 2

Para determinar vtp primero hay que identificar quienes actuaran como capa 2

S = Server vtp 
C = Cliente vtp
R = router (switch actua como router no aplicará vtp)

![imagen](img/config-vtp.png)

### Switch servidor - S
```
enable 
configure terminal 
vtp domain g37 
vtp mode server 
vtp password cisco123 
end
wr
show vtp status
```

### Switches clientes - C
```
enable 
configure terminal 
vtp domain g37 
vtp mode client 
vtp password cisco123 
end
wr
show vtp status
```

### Switches servidor - S con STP (rapid-pvst)
```
enable
configure terminal
spanning-tree mode rapid-pvst
end
wr
show spanning-tree
```

### Switches clientes - C con STP (rapid-pvst)
```
enable
configure terminal
spanning-tree mode rapid-pvst
end
wr
show spanning-tree
```



### Configuracion de Vlans S1
enable
configure terminal
vlan 10
 name 37_Naranja_EdificioIZQ
vlan 20
 name 37_Verde_EdificioIZQ
exit


### Configuracion de puertos trunkales

enable
configure terminal
interface range fa0/1-4
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan all
end 
write memory

### Configuracion R4

configure terminal
vlan 30
name 37_Admin_Central
exit

interface GigabitEthernet1/0/3
switchport mode access
switchport access vlan 30
no shutdown
exit

interface range GigabitEthernet1/1/1 - 2  
switchport mode trunk
switchport trunk allowed vlan all  
no shutdown
exit


### Configuracion R3
enable
configure terminal
interface range GigabitEthernet1/0/1 - 2  
switchport mode trunk
switchport trunk allowed vlan all  
no shutdown
exit

interface range GigabitEthernet1/1/1 - 2
switchport mode trunk
switchport trunk allowed vlan all  
no shutdown
exit



## LACP Lado Izquierdo

### S1

enable
configure terminal
interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode active
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 1

### C1

enable
configure terminal
interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode passive
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 1


### C2

enable
configure terminal
interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode active
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 1

### R5

```
enable
configure terminal
interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode passive
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 1

enable
configure terminal
interface range Fa0/11-13
channel-protocol lacp
channel-group 2 mode active
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 2

```

### R2

```
enable
configure terminal
interface range GigabitEthernet1/0/1-3 
channel-protocol lacp
channel-group 1 mode passive
no shutdown
end 
wr

show etherchannel
show etherchannel port-channel
show interfaces port-channel 1
```




## PAGP Lado Derecho

# Switch R3
interface range GigabitEthernet1/0/1-3
 channel-group 1 mode desirable

# Switch R6
interface range FastEthernet0/11-13
 channel-group 1 mode auto

interface range FastEthernet0/1-3
 channel-group 3 mode desirable

# Switch C5
interface range FastEthernet0/1-3
 channel-group 3 mode auto

# Switch C6
interface range FastEthernet0/1-3
 channel-group 2 mode desirable

# Switch S2
interface range FastEthernet0/1-3
 channel-group 2 mode auto


## Verificacion de la configuracion 

# Ver el estado de los EtherChannels configurados
show etherchannel summary

# Ver detalles de una interfaz agregada a un canal
show interfaces port-channel <ID>

# Mostrar información sobre los vecinos PagP
show pagp neighbor

# Ver los puertos configurados en un EtherChannel
show etherchannel port-channel

# Revisar la configuración aplicada a las interfaces
show running-config | section interface

# IP Address Allocation Table

## 10.0.37.0/24 Network Segmentation

| **Subnet**        | **Subnet Mask**       | **Usable IPs**           | **Assigned Devices** |
|-------------------|----------------------|--------------------------|----------------------|
| **10.0.37.0/30**  | 255.255.255.252       | 10.0.37.1 - 10.0.37.2    | **R5 - Port-Channel1** |
| **10.0.37.4/30**  | 255.255.255.252       | 10.0.37.5 - 10.0.37.6    | **R2 - Gi1/1/2 → R1 - Gi1/1/2** |
| **10.0.37.8/30**  | 255.255.255.252       | 10.0.37.9 - 10.0.37.10   | **R2 - Gi1/1/4 → R3 - Gi1/1/3** |
| **10.0.37.12/30** | 255.255.255.252       | 10.0.37.13 - 10.0.37.14  | **R2 - Gi1/1/3 → R4 - Gi1/1/1** |
| **10.0.37.16/30** | 255.255.255.252       | 10.0.37.17 - 10.0.37.18  | **R4 - Gi1/1/2 → R3 - Gi1/1/1** |
| **10.0.37.20/30** | 255.255.255.252       | 10.0.37.21 - 10.0.37.22  | **R1 - Gi1/1/1 → R3 - Gi1/1/2** |
| **10.0.37.24/30** | 255.255.255.252       | 10.0.37.25 - 10.0.37.26  | **Server1 - Fa0 → R1 - Gi1/0/1** |
| **10.0.37.28/30**	| 255.255.255.252	    |10.0.37.29 - 10.0.37.30   | **Server2 - Fa0 → R1 - Gi1/0/2** |
| **10.0.37.32/30** | 255.255.255.252       | 10.0.37.33 - 10.0.37.34  | **R3 - Port-Channel1 → R6 - Port-Channel1** |

---

## 192.168.37.0/24 Network Segmentation

| **Subnet**         | **Subnet Mask**       | **Usable IPs**            | **Assigned Devices** |
|--------------------|----------------------|---------------------------|----------------------|
| **192.168.37.0/28**  | 255.255.255.240    | 192.168.37.1 - 192.168.37.14  | **R5 - Port-Channel1** |
| **192.168.37.16/28** | 255.255.255.240    | 192.168.37.17 - 192.168.37.30 | **R5 - FastEthernet0/4** |
| **192.168.37.32/28** | 255.255.255.240    | 192.168.37.33 - 192.168.37.46 | **R4 - GigabitEthernet1/0/3** |
| **192.168.37.48/28** | 255.255.255.240    | 192.168.37.49 - 192.168.37.62 | **R6 - FastEthernet0/4** |
| **192.168.37.64/28** | 255.255.255.240    | 192.168.37.65 - 192.168.37.78 | **R6 - Port-Channel3** |

