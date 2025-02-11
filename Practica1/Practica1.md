# MANUAL TECICO


#### Comandos VTP

----------------------------------------------Lado derecho

+En el switch SERVIDOR VTP:

enable
configure terminal
vtp domain g37
vtp mode server
vtp password cisco123
exit

show vtp status

+Crear las VLANs Derecha:

enable
configure terminal
vlan 41
name Primaria  
vlan 51  
name Basicos  
vlan 61  
name Diversificado 
exit

show vlan brief

-----------------------------------------------------


+Configurar puertos trunk (ej: interfaces conectadas a otros switches):

interface GigabitEthernet0/1   # Reemplaza con la interfaz correcta  
switchport mode trunk  
switchport trunk allowed vlan all  
exit 

interface range Fa0/1-9
switchport mode trunk
switchport trunk allowed vlan all
exit

----------------------------------------------------

-En los switches CLIENTES VTP:

enable
configure terminal
vtp domain g37
vtp mode client
vtp password cisco123
exit

-Configurar puertos trunk (igual que en el servidor):
 

interface range Fa0/1-2
switchport mode trunk
switchport trunk allowed vlan all
exit

-Asignación de VLANs a puertos:

configure terminal
interface fa0/3
switchport mode access
switchport access vlan 41
exit

interface fa0/3
switchport mode access
switchport access vlan 51
exit

interface fa0/3
switchport mode access
switchport access vlan 61
exit

--------------------------------------------------Lado izquierdo

+En el switch SERVIDOR SW5_G37 VTP:

enable
configure terminal
vtp domain g37
vtp mode server
vtp password cisco123
exit

show vtp status

- CONFIGURAR VLANS IZQUIERDA

enable
configure terminal
vlan 11
name Primaria  
vlan 21  
name Basicos  
vlan 31  
name Diversificado 
exit

show vlan brief

--------------------------------------------------

Configurar puertos en modo trunk del server

interface range Fa0/1-9
switchport mode trunk
switchport trunk allowed vlan all
exit


--------------------------------------------------

Configurar puertos en modo trunk del cliente

enable
configure terminal
interface range Fa0/1-2
switchport mode trunk
switchport trunk allowed vlan all
exit


enable
configure terminal
interface range Fa0/1-3
switchport mode trunk
switchport trunk allowed vlan all
exit

enable
configure terminal
interface range Fa0/1-4
switchport mode trunk
switchport trunk allowed vlan all
exit



enable 
show vlan brief
show vtp status

------------------------------------------------

Configurar puerto modo acceso del cliente

enable 
configure terminal
interface fa0/3
switchport mode access
switchport access vlan 11
exit

enable 
configure terminal
interface fa0/3
switchport mode access
switchport access vlan 21
exit

enable 
configure terminal
interface fa0/3
switchport mode access
switchport access vlan 31
exit


#### Comandos STP
---------------------Lado derecho
enable
configure terminal
spanning-tree mode pvst
exit
show spanning-tree

---------------------Lado izquierdo

enable
configure terminal
spanning-tree mode rapid-pvst
exit
show spanning-tree


#### Configuracion PC's

Mediante a la tabla excel se configuraron las ips de las pc


### Configuracion Router

-----------------------------------------Lado izquierdo Router0
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.11
encapsulation dot1Q 11
ip address 192.168.11.1 255.255.255.0
exit

interface gigabitEthernet 0/0.21
encapsulation dot1Q 21
ip address 192.168.21.1 255.255.255.0
exit

interface gigabitEthernet 0/0.31
encapsulation dot1Q 31
ip address 192.168.31.1 255.255.255.0
exit

show ip interface brief


-----------------------------------------Lado derecho Router3

enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.41
encapsulation dot1Q 41
ip address 192.168.41.1 255.255.255.0
exit

interface gigabitEthernet 0/0.51
encapsulation dot1Q 51
ip address 192.168.51.1 255.255.255.0
exit

interface gigabitEthernet 0/0.61
encapsulation dot1Q 61
ip address 192.168.61.1 255.255.255.0
exit

show ip interface brief

--------------------------------------