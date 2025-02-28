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