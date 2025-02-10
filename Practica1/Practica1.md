# MANUAL TECICO


#### Comandos VTP


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

+Configurar puertos trunk (ej: interfaces conectadas a otros switches):

interface GigabitEthernet0/1   # Reemplaza con la interfaz correcta  
switchport mode trunk  
switchport trunk allowed vlan all  
exit 

interface range Fa0/1-9
switchport mode trunk
switchport trunk allowed vlan all
exit


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

