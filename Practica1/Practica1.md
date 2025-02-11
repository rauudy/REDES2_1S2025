# MANUAL TECICO


#### Comandos VTP

----------------------------------------------

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

--------------------------------------------------

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
switchport access vlan 41
switchport access vlan 51
switchport access vlan 61
exit

enable 
configure terminal
interface fa0/3
switchport mode access
switchport access vlan 21
switchport access vlan 41
switchport access vlan 51
switchport access vlan 61
exit

enable 
configure terminal
interface fa0/3
switchport mode access
switchport access vlan 31
switchport access vlan 41
switchport access vlan 51
switchport access vlan 61
exit