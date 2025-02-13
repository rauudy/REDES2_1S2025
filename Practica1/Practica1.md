# MANUAL TECICO

#### Comandos de Seguridad

---------------------------------------------Password a switch server
enable
configure terminal
enable secret redes2grupo37
exit
wr


---------------------------------------------Interfaces con host de basicos
enable
configure terminal
interface fa0/3 
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky
exit
write memory




#### Comandos de Seguridad DTP

enable
configure terminal
interface range Fa0/1 - 9
switchport mode trunk
switchport nonegotiate
exit
exit
write memory


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


### Configuracion Router Protocolos OSPF, RIP, EIGRP

-----------------------------------------Router0
enable
configure terminal

interface gigabitEthernet 0/1
ip address 10.0.11.1 255.255.255.0
no shutdown
exit

router ospf 1
network 192.168.11.0 0.0.0.255 area 0
network 192.168.21.0 0.0.0.255 area 0
network 192.168.31.0 0.0.0.255 area 0
network 10.0.11.0 0.0.0.255 area 0
exit



show ip route

-----------------------------------------Router1
enable
configure terminal

interface GigabitEthernet0/0
ip address 10.0.11.2 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 10.0.21.1 255.255.255.0
no shutdown
exit

router ospf 1
network 10.0.11.0 0.0.0.255 area 0
exit

router rip
version 2
network 10.0.21.0
no auto-summary
do write
exit

router ospf 1
redistribute rip subnets
exit

router rip
redistribute ospf 1 metric 5
exit


show ip route
-----------------------------------------Router2
enable
configure terminal

interface GigabitEthernet0/0
ip address 10.0.21.2 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 10.0.31.1 255.255.255.0
no shutdown
exit

router rip
version 2
network 10.0.21.0
no auto-summary
exit

router eigrp 100
network 10.0.31.0
no auto-summary
exit

router rip
redistribute eigrp 100 metric 5
exit

router eigrp 100
redistribute rip metric 10000 100 255 1 1500
exit
-----------------------------------------Router3

enable
configure terminal

interface GigabitEthernet0/1
ip address 10.0.31.2 255.255.255.0
no shutdown
exit

router eigrp 100
network 192.168.41.0
network 192.168.51.0
network 192.168.61.0
network 10.0.31.0
no auto-summary
exit

show ip interface brief

--------------------------------------


# Escenario de Prueba

Cuando se desconecta el cable o se pierde la conexion en el enlace activo/forwarding en el caso de Rapid PVST los paquetes siguieron transmitiendose sin perdida y sin interrupcion
por lo que no hubo "Request Timed Out" al desconectar el cable, lo que indica una convergencia inmediata como se presenta a continuacion:

### Comparación de Escenarios de Spanning-Tree

| Escenario | Protocolo Spanning-Tree | Red Primaria (Convergencia) | Red Básicos (Convergencia) | Red Diversificado (Convergencia) |
|-----------|-------------------------|-----------------------------|-----------------------------|-------------------------------|
| **1**     | PVST                    | Lento (~10-20 seg)         | Lento (~55-60 seg)         | Lento (~30-40 seg)          |
| **2**     | Rapid PVST               | Muy rápida (~0-1 seg)      | Muy rápida (~0-1 seg)      | 0seg       |

Con Rapid PVST cuando el enlace original se recuperó, solo se registró un "Request Timed Out", lo que indica que la red necesitó un breve momento para redirigir el tráfico de regreso a su ruta principal. Este tiempo de interrupción es mínimo y no representa una pérdida prolongada de paquetes, lo que confirma que la red se adaptó rápidamente a los cambios sin afectar significativamente la conectividad.

En comparación, PVST es notablemente más lento debido a que utiliza el protocolo STP tradicional, el cual opera con temporizadores fijos y puede tardar hasta 50 segundos en converger. Esto se debe a que PVST sigue una transición secuencial de estados:

1. Blocking (20 seg)
2. Listening (15 seg)
3. Learning (15 seg)
4. Forwarding

Los múltiples "Request Timed Out" observados durante la prueba con PVST indican que la red no cambia de ruta de forma inmediata, lo que puede afectar la disponibilidad en entornos críticos.

Por otro lado, Rapid PVST es más eficiente porque emplea BPDUs instantáneos y reemplaza el estado Blocking por Discarding, lo que optimiza la convergencia y permite una recuperación mucho más rápida. 


### Primaria

| Protocolo    | Comportamiento al Desconectar | Comportamiento al Reconectar | Convergencia |
|-------------|-----------------------------|-----------------------------|-------------|
| **PVST**    | 5 paquetes perdidos (~10-20 seg) | 5 paquetes perdidos (~10-20 seg) | Lento (~10-20 seg) |
| **Rapid PVST** | Comunicación inmediata | Pequeña interrupción (1 request timed out) | 0seg |

---

### Diversificado

| Protocolo    | Comportamiento al Desconectar | Comportamiento al Reconectar | Convergencia |
|-------------|-----------------------------|-----------------------------|-------------|
| **PVST**    | 5 paquetes perdidos (~30-40 seg) | 5 paquetes perdidos (~30-40 seg) | Lento (~30-40 seg) |
| **Rapid PVST** | Comunicación inmediata | Pequeña interrupción (1 request timed out) | 0seg |

---

### Básicos

| Protocolo    | Comportamiento al Desconectar | Comportamiento al Reconectar | Convergencia |
|-------------|-----------------------------|-----------------------------|-------------|
| **PVST**    | 5 paquetes perdidos (~55-60 seg) | 5 paquetes perdidos (~30-40 seg) | Lento (~55-60 seg) |
| **Rapid PVST** | Comunicación inmediata | Pequeña interrupción (1 request timed out ~5-10 seg) | 0seg |



### Propuesta Final

Después de realizar las pruebas de convergencia con PVST y Rapid PVST en las redes Primaria, Básicos y Diversificado, los resultados demuestran que Rapid PVST ofrece el menor tiempo de convergencia en todos los casos.

- Rapid PVST tiene una convergencia casi instantánea (~0-1 seg), mientras que PVST presenta tiempos de convergencia de hasta 60 segundos en algunos casos.
- Al recuperar un enlace, Rapid PVST solo genera un Request Timed Out, mientras que PVST presenta múltiples pérdidas de paquetes.
- PVST utiliza un método más lento basado en transiciones secuenciales de estado (Blocking → Listening → Learning → Forwarding), lo que aumenta el tiempo de respuesta ante fallos.
- Rapid PVST optimiza la convergencia con BPDUs instantáneos y el estado Discarding, evitando largas esperas.

Se elige Rapid PVST como el protocolo definitivo, ya que ofrece una recuperación de enlaces mucho más rápida, minimizando el impacto en la conectividad. Esto lo hace ideal para entornos donde la disponibilidad de la red es crítica.