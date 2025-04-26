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
