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

## VTP y VLANS

### Switch0 servidor
```
!vtp
enable
configure terminal
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

### Switch1 cliente Soporte y propagación de VLANs
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

### Switch2 cliente Seguridad y propagación de VLANs
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


### Router1

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
### Router2

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


### R-MSW2
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


### R-MSW1
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

interface GigabitEthernet0/2
no switchport
ip address 192.168.31.1 255.255.255.252
no shutdown
exit


router ospf 1
network 192.168.31.96 0.0.0.3 area 1
network 192.168.31.104 0.0.0.3 area 1
network 192.168.31.0 0.0.0.31 area 1
end
wr

````


### R-MSW0
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


router ospf 1
network 192.168.31.96 0.0.0.3 area 1
network 192.168.31.100 0.0.0.3 area 1

end
wr

````




## Configuracion DNS
---
<div align="justify">
El servicio DNS (Domain Name System) fue implementado para permitir la resolución de nombres dentro de la red, facilitando el acceso a servicios web mediante nombres de dominio en lugar de direcciones IP. Esta configuración mejora la usabilidad de la red y simula un entorno real en el que los usuarios acceden a recursos utilizando URLs.
</div>

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


## ISP 1

