# Documentación Proyecto 2 - Redes 2

## Objetivo del Diseño de ISP 2

El propósito de la implementación para ISP 2 es diseñar una red jerárquica eficiente y segmentada, capaz de conectar múltiples áreas funcionales mediante el uso de VLANs, subredes optimizadas y protocolos de enrutamiento dinámico. Esta red debe permitir la comunicación segura y eficiente entre departamentos como Facturación y Ventas, utilizando infraestructura física robusta con redundancia (mediante LACP) y escalabilidad.

La red está compuesta por routers, switches de acceso y multilayer, enlaces punto a punto para interconexión entre dispositivos de capa 3, y segmentación por VLAN para asegurar control de tráfico y seguridad lógica. El protocolo EIGRP se utilizó para facilitar la propagación dinámica de rutas, lo cual asegura que cualquier cambio en la red sea rápidamente adaptado sin necesidad de modificar rutas estáticas.

En resumen, ISP 2 busca proporcionar una red funcional, escalable y administrable que permita el crecimiento y la integración eficiente de servicios de datos internos y externos.


## Implementación de Red Segmentada con VLANs y Enrutamiento EIGRP


| Name | Hosts Needed | Hosts Available | Unused Hosts | Network Address   | Slash | Mask              | Usable Range                     | Broadcast         |
|------|---------------|------------------|---------------|--------------------|--------|--------------------|----------------------------------|-------------------|
| 5    | 30            | 30               | 0             | 192.168.21.0       | /27    | 255.255.255.224    | 192.168.21.1 - 192.168.21.30     | 192.168.21.31     |
| 6    | 30            | 30               | 0             | 192.168.21.32      | /27    | 255.255.255.224    | 192.168.21.33 - 192.168.21.62    | 192.168.21.63     |
| 7    | 30            | 30               | 0             | 192.168.21.64      | /27    | 255.255.255.224    | 192.168.21.65 - 192.168.21.94    | 192.168.21.95     |
| 8    | 30            | 30               | 0             | 192.168.21.96      | /27    | 255.255.255.224    | 192.168.21.97 - 192.168.21.126   | 192.168.21.127    |
| 1    | 2             | 2                | 0             | 192.168.21.128     | /30    | 255.255.255.252    | 192.168.21.129 - 192.168.21.130  | 192.168.21.131    |
| 2    | 2             | 2                | 0             | 192.168.21.132     | /30    | 255.255.255.252    | 192.168.21.133 - 192.168.21.134  | 192.168.21.135    |
| 3    | 2             | 2                | 0             | 192.168.21.136     | /30    | 255.255.255.252    | 192.168.21.137 - 192.168.21.138  | 192.168.21.139    |
| 4    | 2             | 2                | 0             | 192.168.21.140     | /30    | 255.255.255.252    | 192.168.21.141 - 192.168.21.142  | 192.168.21.143    |


### R-MSW0_ISP2

```
enable
configure terminal

interface Fa0/1
no switchport
ip address 172.1.0.18 255.255.255.252
no shutdown
exit

router eigrp 10
network 192.168.21.64 0.0.0.31
network 192.168.21.132 0.0.0.3
network 192.168.21.136 0.0.0.3
network 172.1.0.16 0.0.0.3
no auto-summary
exit

end
wr
```



router eigrp 10
no network 192.168.21.0
network 192.168.21.136 0.0.0.3
network 192.168.21.140 0.0.0.3
no auto-summary
exit


router eigrp 10
no network 192.168.21.0
network 192.168.21.32 0.0.0.31
network 192.168.21.128 0.0.0.3
network 192.168.21.132 0.0.0.3
no auto-summary
exit


router eigrp 10
no network 192.168.21.0
network 192.168.21.128 0.0.0.3
network 192.168.21.0 0.0.0.31
no auto-summary
exit



### Tecnologías Implementadas

- **VLANs:**  
  - VLAN 20: Facturación  
  - VLAN 22: Ventas  
  - Se configuraron en switches de acceso con puertos en modo access.

  - **EIGRP:**  
  - Protocolo de enrutamiento interno implementado en todos los dispositivos de capa 3.  
  - Permite el intercambio automático de rutas entre redes.

  ### Consideraciones

- Se evitó el uso de trunk entre router y switch ya que se manejó una sola VLAN por enlace.
- Se usaron enlaces punto a punto para conectar routers y switches mediante subredes /30.
- Se establecieron gateways correctos en dispositivos finales para permitir comunicación inter-VLAN.


## Propuesta de Gasto Estimado para la Implementación de ISP 2

A continuación se detalla el costo aproximado del equipo necesario para la implementación de la red:

| Dispositivo             | Cantidad | Precio Unitario (USD) | Subtotal (USD) |
|--------------------------|----------|------------------------|----------------|
| Routers ISR 4321         | 3        | $1,000                  | $3,000         |
| Switches 2960-24TT       | 3        | $400                    | $1,200         |
| Switches Multilayer 3560 | 3        | $600                    | $1,800         |
| Access Point WRT300N     | 1        | $80                     | $80            |
| PCs (PC-PT)              | 4        | $500                    | $2,000         |
| Laptop (Laptop-PT)       | 1        | $600                    | $600           |
| Cables, racks y extras   | -        | -                       | $500           |

---

### Total Estimado: **$9,180 USD**

### Notas:
- Los precios son estimados y pueden variar según el proveedor.
- No se incluyen costos de licencias, servidores adicionales ni servicios de instalación.
- La infraestructura está diseñada para soportar crecimiento futuro con mínima inversión adicional.




## Configuración ISP-3

Subredes

| #   | Hosts | Subred           | Máscara         | Primer Host     | Último Host      | Broadcast        |
|-----|-------|------------------|-----------------|-----------------|-----------------|------------------|
| 1   | 30    | 192.168.31.0 /27 | 255.255.255.224 | 192.168.31.1    | 192.168.31.30   | 192.168.31.31    |
| 2   | 30    | 192.168.31.32 /27 | 255.255.255.224 | 192.168.31.33   | 192.168.31.62   | 192.168.31.63    |
| 3   | 30    | 192.168.31.64 /27 | 255.255.255.224 | 192.168.31.65   | 192.168.31.94   | 192.168.31.95    |
| 4   | 2     | 192.168.31.96 /30 | 255.255.255.252 | 192.168.31.97   | 192.168.31.98   | 192.168.31.99    |
| 5   | 2     | 192.168.31.100/30 | 255.255.255.252 | 192.168.31.101  | 192.168.31.102  | 192.168.31.103   |
| 6   | 2     | 192.168.31.104/30 | 255.255.255.252 | 192.168.31.105  | 192.168.31.106  | 192.168.31.107   |
| 7   | 2     | 192.168.31.108/30 | 255.255.255.252 | 192.168.31.109  | 192.168.31.110  | 192.168.31.111   |
| 8   | 2     | 192.168.31.112/30 | 255.255.255.252 | 192.168.31.113  | 192.168.31.114  | 192.168.31.115   |

### VTP y VLANS

### Switch0_ISP3
```
enable
configure terminal

! vtp server
vtp version 2
vtp domain g37_isp3
vtp mode server
vtp password g37_isp3

!vlans
vlan 30
name Soporte
vlan 35
name Seguridad

!trunk
interface range fa0/1-4
switchport mode trunk
switchport trunk allowed vlan all
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```

### Switch1_ISP3
```
enable
configure terminal
vtp version 2
vtp domain g37_isp3
vtp mode client
vtp password g37_isp3

!trunk
interface range fa0/1-2
switchport mode trunk
switchport trunk allowed vlan all
exit

!access
interface range Fa0/3-5
switchport mode access
switchport access vlan 30
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr
```

### Switch2_ISP3
```
enable
configure terminal
vtp version 2
vtp domain g37_isp3
vtp mode client
vtp password g37_isp3

!trunk
interface range fa0/1-2
switchport mode trunk
switchport trunk allowed vlan all
exit

!access
interface range Fa0/3-4
switchport mode access
switchport access vlan 35
exit

! spanning-tree
spanning-tree mode rapid-pvst

end
wr
```

### Switch4_ISP3
```
enable
configure terminal

!vlans
vlan 31
name Server_DNS

!trunk
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan all
exit

!access
interface Fa0/3
switchport mode access
switchport access vlan 31
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```


### Router1_ISP3

```
enable
configure terminal

interface GigabitEthernet0/0
ip address 192.168.31.109 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
no shutdown
exit

! Configuración para la VLAN Soporte-30
interface GigabitEthernet0/1.30
encapsulation dot1Q 30
ip address 192.168.31.33 255.255.255.224
standby 30 ip 192.168.31.35
standby 30 priority 110
standby 30 preempt
exit

! Configuración para la VLAN Seguridad-35
interface GigabitEthernet0/1.35
encapsulation dot1Q 35
ip address 192.168.31.65 255.255.255.224
standby 35 ip 192.168.31.67
standby 35 priority 90
standby 35 preempt
exit

!ospf
router ospf 1
network 192.168.31.32 0.0.0.31 area 1
network 192.168.31.64 0.0.0.31 area 1
network 192.168.31.108 0.0.0.3 area 1

end
wr

show standby brief

```
### Router2_ISP3

```
enable
configure terminal

interface GigabitEthernet0/0
ip address 192.168.31.113 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
no shutdown
exit

! Configuración para la VLAN Soporte-30
interface GigabitEthernet0/1.30
encapsulation dot1Q 30
ip address 192.168.31.34 255.255.255.224
standby 30 ip 192.168.31.35
standby 30 priority 90
standby 30 preempt
exit

! Configuración para la VLAN Seguridad-35
interface GigabitEthernet0/1.35
encapsulation dot1Q 35
ip address 192.168.31.66 255.255.255.224
standby 35 ip 192.168.31.67
standby 35 priority 110
standby 35 preempt
exit

!ospf
router ospf 1
network 192.168.31.32 0.0.0.31 area 1
network 192.168.31.64 0.0.0.31 area 1
network 192.168.31.112 0.0.0.3 area 1

end
wr

show standby brief

```


### R-MSW2_ISP3
````
enable
configure terminal
ip routing

interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode active
no switchport
no shutdown
exit

interface port-channel 1
no switchport
ip address 192.168.31.101 255.255.255.252
exit

interface GigabitEthernet0/1
no switchport
ip address 192.168.31.105 255.255.255.252
no shutdown
exit

interface Fa0/4
no switchport
ip address 192.168.31.110 255.255.255.252
no shutdown
exit

interface Fa0/5
no switchport
ip address 192.168.31.114 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.31.100 0.0.0.3 area 1
network 192.168.31.104 0.0.0.3 area 1
network 192.168.31.108 0.0.0.3 area 1
network 192.168.31.112 0.0.0.3 area 1

end
wr

````


### R-MSW1_ISP3
````
enable
configure terminal
ip routing

interface range Fa0/4-6
channel-protocol lacp
channel-group 1 mode active
no shutdown
exit

interface port-channel 1
no switchport
ip address 192.168.31.97 255.255.255.252
exit

interface GigabitEthernet0/1
no switchport
ip address 192.168.31.106 255.255.255.252
no shutdown
exit


!vlan
vlan 31
name Server_DNS

interface vlan 31
ip address 192.168.31.1 255.255.255.252
exit

router ospf 1
network 192.168.31.96 0.0.0.3 area 1
network 192.168.31.104 0.0.0.3 area 1
network 192.168.31.0 0.0.0.31 area 1
end
wr

````


### R-MSW0_ISP3
````
enable
configure terminal
ip routing

interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode passive
no switchport
no shutdown
exit

interface range Fa0/4-6
channel-protocol lacp
channel-group 2 mode passive
no switchport
no shutdown
exit

interface port-channel 1
no switchport
ip address 192.168.31.102 255.255.255.252
exit

interface port-channel 2
no switchport
ip address 192.168.31.98 255.255.255.252
exit

interface Fa0/7
no switchport
ip address 172.1.0.22 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.31.96 0.0.0.3 area 1
network 192.168.31.100 0.0.0.3 area 1
network 172.1.0.20 0.0.0.3 area 1
exit

end
wr

````




## Configuracion DNS_ISP3
---

1. ***Asignar direccion IP estatica al Servidor***:
    * IP: 192.168.31.2
    * Subnet mask: 255.255.255.224
    * Default Gateway: Dirección IP del router o switch multilayer que conecta al servidor.
![imagen](img/dns/ip_server_dns.png)


2. ***Activar el servicio HTTP***:
    * En services seleccionar HTTP y asegurarse que el servicio esta encendido

![imagen](img/dns/server_dns_activar_http.png)

3. ***Editar el contenido de la pagina web y darle save***:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Grupo 37</title>
</head>
<body>
  <h1>Bienvenidos al Proyecto 2</h1>
  <p>Integrantes del Grupo 37:</p>
  <ul>
    <li> Alejandro Rene Caballeros Gonzales - 201903549</li>
    <li> Christtopher Jose Chitay Coutino - 201113851</li>
    <li> Raudy David Cabrera Contreras - 201901973</li>
  </ul>
</body>
</html>
```
![imagen](img/dns/server_dns_editarIndex.png)

### Configurar el Servidor DNS (ServerDHCP o ServerDNS)

1. Ir Services > DNS y encender el servicio ON

2. En el campo de configuracion de dominio agregar:
    * Name: www.Proyecto2_Grupo37.com
    * Address: Dirección IP del ServerWeb, por ejemplo 192.168.31.2
    * Presionar Add.

![imagen](img/dns/server_dns_dominio.png)



### Actualizar los Pool del Server DHCP en la propiedad DNS

Irse al Servidor DHCP y actuacilar la el dns

![imagen](img/dns/server_dns_UpdatePools.png)

### Actualizar DNS de Wirless Router 1

![imagen](img/dns/server_dns_UpdateWR1.png)

### Actualizar DNS de Wirless Router 0

![imagen](img/dns/server_dns_UpdateWR0.png)

### Actualizar DNS de todas las PC

![imagen](img/dns/server_dns_UpdateDHCP_pcs.png)

### Prueba DNS

Ingresar a PC > Desktop > Web Browser

Y poner esta URL
```
http://www.Proyecto2_Grupo37.com
```
Darle "Go"

![imagen](img/dns/server_dns_prueba.png)


## Configuracion ISP 1

### Subneteo

| #   | Hosts | Subred            | Máscara         | Primer Host      | Último Host       | Broadcast         |
|-----|-------|-------------------|-----------------|------------------|-------------------|-------------------|
| 1   | 30    | 192.168.11.0 /27  | 255.255.255.224 | 192.168.11.1     | 192.168.11.30    | 192.168.11.31     |
| 2   | 30    | 192.168.11.32 /27 | 255.255.255.224 | 192.168.11.33    | 192.168.11.62    | 192.168.11.63     |
| 3   | 30    | 192.168.11.64 /27 | 255.255.255.224 | 192.168.11.65    | 192.168.11.94    | 192.168.11.95     |
| 4   | 30    | 192.168.11.96 /27 | 255.255.255.224 | 192.168.11.97    | 192.168.11.126   | 192.168.11.127    |
| 5   | 2     | 192.168.11.128/30 | 255.255.255.252 | 192.168.11.129   | 192.168.11.130   | 192.168.11.131    |
| 6   | 2     | 192.168.11.132/30 | 255.255.255.252 | 192.168.11.133   | 192.168.11.134   | 192.168.11.135    |
| 7   | 2     | 192.168.11.136/30 | 255.255.255.252 | 192.168.11.137   | 192.168.11.138   | 192.168.11.139    |
| 8   | 2     | 192.168.11.140/30 | 255.255.255.252 | 192.168.11.141   | 192.168.11.142   | 192.168.11.143    |

### Switch1_ISP1
```
enable
configure terminal

!vlans
vlan 10
name Administracion

!trunk
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan all
exit

!access
interface range Fa0/2-3
switchport mode access
switchport access vlan 10
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```

### Switch2_ISP1
```
enable
configure terminal

!vlans
vlan 15
name Atencion_al_cliente

!trunk
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan all
exit

!access
interface Fa0/2
switchport mode access
switchport access vlan 15
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```

### Switch3_ISP1
```
enable
configure terminal

!vlans
vlan 11
name Server_DCHP

!trunk
interface fa0/1
switchport mode access
switchport access vlan 11
exit

!access
interface Fa0/2
switchport mode access
switchport access vlan 11
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```

### Switch4_ISP1
```
enable
configure terminal

!vlans
vlan 15
name Atencion_al_cliente

!trunk
interface fa0/1
switchport mode access
switchport access vlan 15
exit

!access
interface range Fa0/2-3
switchport mode access
switchport access vlan 15
exit

! spanning-tree
spanning-tree mode rapid-pvst 

end
wr

```

### R-MSW0_ISP1
````
enable
configure terminal
ip routing

interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode passive
no switchport
no shutdown
exit

interface range Fa0/4-6
channel-protocol lacp
channel-group 2 mode passive
no switchport
no shutdown
exit

interface port-channel 1
ip address 192.168.11.129 255.255.255.252
exit

interface port-channel 2
ip address 192.168.11.137 255.255.255.252
exit

interface Fa0/7
no switchport
ip address 192.168.11.133 255.255.255.252
no shutdown
exit

interface Fa0/8
no switchport
ip address 192.168.11.141 255.255.255.252
no shutdown
exit

interface Fa0/9
no switchport
ip address 172.1.0.14 255.255.255.252
no shutdown
exit

router eigrp 100
network 192.168.11.128 0.0.0.3
network 192.168.11.132 0.0.0.3
network 192.168.11.136 0.0.0.3
network 192.168.11.140 0.0.0.3
network 172.1.0.12 0.0.0.3
no auto-summary
exit

end
wr

````


### R-MSW1_ISP1
````
enable
configure terminal
ip routing

interface range Fa0/1-3
channel-protocol lacp
channel-group 1 mode active
no switchport
no shutdown
exit

interface port-channel 1
ip address 192.168.11.130 255.255.255.252
exit


!vlan
vlan 10
name Administracion

interface vlan 10
ip address 192.168.11.33 255.255.255.224
exit


router eigrp 100
network 192.168.11.32 0.0.0.31
network 192.168.11.128 0.0.0.3
no auto-summary
exit

end
wr

````

### R-MSW2_ISP1
````
enable
configure terminal
ip routing

interface range Fa0/4-6
channel-protocol lacp
channel-group 1 mode active
no switchport
no shutdown
exit

interface port-channel 1
ip address 192.168.11.138 255.255.255.252
exit

!vlan
vlan 15
name Atencion_al_cliente

interface vlan 15
ip address 192.168.11.65 255.255.255.224
exit


router eigrp 100
network 192.168.11.64 0.0.0.31
network 192.168.11.136 0.0.0.3
no auto-summary
exit

end
wr

````

### Router1_ISP1

```
enable
configure terminal

interface GigabitEthernet0/0
ip address 192.168.11.142 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.11.97 255.255.255.224
no shutdown
exit

!EIGRP
router eigrp 100
network 192.168.11.96 0.0.0.31
network 192.168.11.140 0.0.0.3
no auto-summary
exit

end
wr

show standby brief

```
### Router2_ISP1

```
enable
configure terminal

interface GigabitEthernet0/0
ip address 192.168.11.134 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.11.1 255.255.255.224
no shutdown
exit

!EIGRP
router eigrp 100
network 192.168.11.0 0.0.0.31
network 192.168.11.132 0.0.0.3
no auto-summary
exit

end
wr

show standby brief

```


## Configuración BGP

### Subneteo de red 172.1.0.0 /16

|   Name | Network Address   | Mask            | Usable Range            | Broadcast   |
|-------:|:------------------|:----------------|:------------------------|:------------|
|      1 | 172.1.0.0 /30     | 255.255.255.252 | 172.1.0.1 - 172.1.0.2   | 172.1.0.3   |
|      2 | 172.1.0.4 /30     | 255.255.255.252 | 172.1.0.5 - 172.1.0.6   | 172.1.0.7   |
|      3 | 172.1.0.8 /30     | 255.255.255.252 | 172.1.0.9 - 172.1.0.10  | 172.1.0.11  |
|      4 | 172.1.0.12 /30    | 255.255.255.252 | 172.1.0.13 - 172.1.0.14 | 172.1.0.15  |
|      5 | 172.1.0.16 /30    | 255.255.255.252 | 172.1.0.17 - 172.1.0.18 | 172.1.0.19  |
|      6 | 172.1.0.20 /30   | 255.255.255.252 | 172.1.0.21 - 172.1.0.22 | 172.1.0.23  |


### RR1
```
enable
configure terminal
ip routing

interface GigabitEthernet1/1/3
no switchport
ip address 172.1.0.1 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/1/4
no switchport
ip address 172.1.0.9 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/0/1
no switchport
ip address 172.1.0.13 255.255.255.252
no shutdown
exit

router bgp 65001
neighbor 172.1.0.10 remote-as 65002
neighbor 172.1.0.3 remote-as 65003
network 172.1.0.0 mask 255.255.255.252
network 172.1.0.8 mask 255.255.255.252
network 172.1.0.12 mask 255.255.255.252
redistribute eigrp 100
exit

router eigrp 100
redistribute bgp 65001 metric 100000 100 255 1 1500
network 172.1.0.12 0.0.0.3
no auto-summary
exit

end
wr

```

### RR2
```
enable
configure terminal
ip routing

interface GigabitEthernet1/1/3
no switchport
ip address 172.1.0.10 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/1/4
no switchport
ip address 172.1.0.5 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/0/1
no switchport
ip address 172.1.0.17 255.255.255.252
no shutdown
exit

router bgp 65002
neighbor 172.1.0.9 remote-as 65001
neighbor 172.1.0.6 remote-as 65003
network 172.1.0.4 mask 255.255.255.252
network 172.1.0.8 mask 255.255.255.252
network 172.1.0.16 mask 255.255.255.252
redistribute eigrp 10
exit

router eigrp 10
redistribute bgp 65002 metric 100000 100 255 1 1500
network 172.1.0.16 0.0.0.3
no auto-summary
exit

end
wr
```
### RR3
```
enable
configure terminal
ip routing

interface GigabitEthernet1/1/3
no switchport
ip address 172.1.0.2 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/1/4
no switchport
ip address 172.1.0.6 255.255.255.252
no shutdown
exit

interface GigabitEthernet1/0/1
no switchport
ip address 172.1.0.21 255.255.255.252
no shutdown
exit

router bgp 65003
neighbor 172.1.0.1 remote-as 65001
neighbor 172.1.0.5 remote-as 65002
network 172.1.0.0 mask 255.255.255.252
network 172.1.0.4 mask 255.255.255.252
network 172.1.0.20 mask 255.255.255.252
redistribute ospf 1
exit

router ospf 1
redistribute bgp 65003 subnets
network 172.1.0.20 0.0.0.3 area 1
exit

end
wr
```


## Server DHCP

#### IP
```
192.168.11.2
```
![imagen](img/dhcp/ip_server_dhcp.png)

### Pools

![imagen](img/dhcp/server_dhcp_PoolAdmin.png)
![imagen](img/dhcp/server_dhcp_PoolEstudiantes.png)

### R-MSW1_ISP1
```
! Helper Address
enable
configure terminal

interface vlan 10
ip helper-address 192.168.11.2
exit

end
wr

```

### R-MSW2_ISP1
```
! Helper Address
enable
configure terminal

interface vlan 15
ip helper-address 192.168.11.2
exit

end
wr

```

### Router1_ISP1
```
! Helper Address
enable
configure terminal

interface GigabitEthernet0/1
ip helper-address 192.168.11.2
exit

end
wr

```

### Router1_ISP2
```
! Helper Address
enable
configure terminal

interface GigabitEthernet0/0/0
ip helper-address 192.168.11.2
exit

end
wr

```
### R-MSW1_ISP2
```
! Helper Address
enable
configure terminal

interface Fa0/1
ip helper-address 192.168.11.2
exit

end
wr

```

### R-MSW2_ISP2
```
! Helper Address
enable
configure terminal

interface Fa0/1
ip helper-address 192.168.11.2
exit

end
wr

```


### Router1_ISP3
```
! Helper Address
enable
configure terminal
interface GigabitEthernet0/1.30
ip helper-address 192.168.11.2
exit
interface GigabitEthernet0/1.35
ip helper-address 192.168.11.2
exit
end
wr

```

### Router2_ISP3
```
! Helper Address
enable
configure terminal
interface GigabitEthernet0/1.30
ip helper-address 192.168.11.2
exit
interface GigabitEthernet0/1.35
ip helper-address 192.168.11.2
exit
end
wr

```


### PC's
![imagen](img/dhcp/pc0_ip-dhcp.png)
![imagen](img/dhcp/laptop0_ip-dhcp.png)










## ACL

### R-MSW1_ISP1
```
enable
configure terminal

! Administracion-10
no access-list 100
access-list 100 permit ip any any

interface Vlan10
ip access-group 100 in
exit

end
wr

```

### R-MSW2_ISP1
```
enable
configure terminal

! Atencion_al_cliente-15_2
no access-list 100
!isp1
access-list 100 permit icmp 192.168.11.64 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.11.64 0.0.0.31 192.168.11.32 0.0.0.31
!isp2
access-list 100 deny ip 192.168.11.64 0.0.0.31 192.168.21.0 0.0.0.31
access-list 100 deny ip 192.168.11.64 0.0.0.31 192.168.21.64 0.0.0.31
!isp3
access-list 100 deny ip 192.168.11.64 0.0.0.31 192.168.31.32 0.0.0.31
access-list 100 deny ip 192.168.11.64 0.0.0.31 192.168.31.64 0.0.0.31

access-list 100 permit ip any any


interface vlan 15
ip access-group 100 in
exit

end
wr

```

### Router1_ISP1
```
enable
configure terminal

! Atencion_al_cliente-15_1
no access-list 100
!isp1
access-list 100 permit icmp 192.168.11.96 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.11.96 0.0.0.31 192.168.11.32 0.0.0.31
!isp2
access-list 100 deny ip 192.168.11.96 0.0.0.31 192.168.21.0 0.0.0.31
access-list 100 deny ip 192.168.11.96 0.0.0.31 192.168.21.64 0.0.0.31
!isp3
access-list 100 deny ip 192.168.11.96 0.0.0.31 192.168.31.32 0.0.0.31
access-list 100 deny ip 192.168.11.96 0.0.0.31 192.168.31.64 0.0.0.31

access-list 100 permit ip any any

interface GigabitEthernet0/1
ip access-group 100 in
exit

end
wr

```



### Router1_ISP2
```
enable
configure terminal

! Facturacion-20_1
no access-list 100
!isp1
access-list 100 permit icmp 192.168.21.0 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.21.0 0.0.0.31 192.168.11.32 0.0.0.31
access-list 100 deny ip 192.168.21.0 0.0.0.31 192.168.11.64 0.0.0.31
access-list 100 deny ip 192.168.21.0 0.0.0.31 192.168.11.96 0.0.0.31
!isp2
!isp3
access-list 100 deny ip 192.168.21.0 0.0.0.31 192.168.31.32 0.0.0.31
access-list 100 deny ip 192.168.21.0 0.0.0.31 192.168.31.64 0.0.0.31


access-list 100 permit ip any any


interface GigabitEthernet0/1
ip access-group 100 in
exit

end
wr

```

### R-MSW1_ISP2
```
enable
configure terminal

! Ventas-22
no access-list 100
!isp1
access-list 100 permit icmp 192.168.21.32 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.21.32 0.0.0.31 192.168.11.32 0.0.0.31
!isp2
!isp3
access-list 100 deny ip 192.168.21.32 0.0.0.31 192.168.31.32 0.0.0.31
access-list 100 deny ip 192.168.21.32 0.0.0.31 192.168.31.64 0.0.0.31

access-list 100 permit ip any any

interface fa0/1
ip access-group 100 in
exit

end
wr

```

### R-MSW2_ISP2
```
enable
configure terminal

! Facturacion-20_2
no access-list 100
!isp1
access-list 100 permit icmp 192.168.21.64 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.21.64 0.0.0.31 192.168.11.32 0.0.0.31
access-list 100 deny ip 192.168.21.64 0.0.0.31 192.168.11.64 0.0.0.31
access-list 100 deny ip 192.168.21.64 0.0.0.31 192.168.11.96 0.0.0.31
!isp2
!isp3
access-list 100 deny ip 192.168.21.64 0.0.0.31 192.168.31.32 0.0.0.31
access-list 100 deny ip 192.168.21.64 0.0.0.31 192.168.31.64 0.0.0.31

interface fa0/1
ip access-group 100 in
exit

end
wr

```



### Router1_ISP3
```
enable
configure terminal

! Soporte-30
no access-list 100
access-list 100 permit icmp 192.168.31.32 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.31.32 0.0.0.31 192.168.11.32 0.0.0.31
access-list 100 permit ip any any


interface GigabitEthernet0/1.30
ip access-group 100 in
exit

! Seguridad-35
no access-list 101
access-list 101 permit icmp 192.168.31.64 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 101 deny ip 192.168.31.64 0.0.0.31 192.168.11.32 0.0.0.31
access-list 101 permit ip any any


interface GigabitEthernet0/1.35
ip access-group 101 in
exit



end
wr

```

### Router2_ISP3
```
enable
configure terminal

! Soporte-30
no access-list 100
access-list 100 permit icmp 192.168.31.32 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 100 deny ip 192.168.31.32 0.0.0.31 192.168.11.32 0.0.0.31
access-list 100 permit ip any any


interface GigabitEthernet0/1.30
ip access-group 100 in
exit

! Seguridad-35
no access-list 101
access-list 101 permit icmp 192.168.31.64 0.0.0.31 192.168.11.32 0.0.0.31 echo-reply
access-list 101 deny ip 192.168.31.64 0.0.0.31 192.168.11.32 0.0.0.31
access-list 101 permit ip any any


interface GigabitEthernet0/1.35
ip access-group 101 in
exit



end
wr

```