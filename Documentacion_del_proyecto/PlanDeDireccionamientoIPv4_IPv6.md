**Plan de Direccionamiento IPv4 e IPv6**

**2.1 Consideraciones de diseño:**

Para el direccionamiento IPv4 se utiliza como bloque base la red privada **172.16.0.0/15**, que dispone de 131 072 direcciones.  

Las subredes se asignaron mediante VLSM, ordenándolas de mayor a menor número de hosts para evitar desperdicio de direcciones y fragmentación del espacio.

**2.3 Cálculo de máscaras (VLSM)**
| Nombre | Hosts requeridos | Tamaño del bloque | Máscara |
|--------|:----------------:|:-----------------:|:-------:|
| Estudiantes | 1500 | 2048 | /21 |
| Docentes | 164 | 256 | /24 |
| Académicos | 80 | 128 | /25 |
| Finanzas | 64 | 128 | /25 |
| Datacenter | 44 | 64 | /26 |
| Gestión | 29 | 32 | /27 |
| Voz | 16 | 32 | /27 |
| Blackhull x2 | 10 | 16 | /28 |
| LAN P2P | 2 | 4 | /30 |

**2.4 Tabla de direccionamiento IPv4**
| VLAN | Nombre | Red IPv4 | Rango útil | Gateway | Broadcast |
|:----:|--------|----------|------------|---------|-----------|
| 10 | Estudiantes | 172.16.0.0/21 | 172.16.0.1 - 172.16.7.254 | 172.16.0.1 | 172.16.7.255 |
| 20 | Docentes | 172.16.8.0/24 | 172.16.8.1 - 172.16.8.254 | 172.16.8.1 | 172.16.8.255 |
| 30 | Académicos | 172.16.9.0/25 | 172.16.9.1 - 172.16.9.126 | 172.16.9.1 | 172.16.9.127 |
| 40 | Finanzas | 172.16.9.128/25 | 172.16.9.129 - 172.16.9.254 | 172.16.9.129 | 172.16.9.255 |
| 50 | Datacenter | 172.16.10.0/26 | 172.16.10.1 - 172.16.10.62 | 172.16.10.1 | 172.16.10.63 |
| 60 | Gestión | 172.16.10.64/27 | 172.16.10.65 - 172.16.10.94 | 172.16.10.65 | 172.16.10.95 |
| 70 | Voz | 172.16.10.96/27 | 172.16.10.97 - 172.16.10.126 | 172.16.10.97 | 172.16.10.127 |
| 80 | Blackhull-R1 | 172.16.10.128/28 | 172.16.10.129 - 172.16.10.142 | Sin gateway | 172.16.10.143 |
| 90 | Blackhull-SWM1 | 172.16.10.144/28 | 172.16.10.145 - 172.16.10.158 | Sin gateway | 172.16.10.159 |
| 100 | P2P | 172.16.10.160/30 | 172.16.10.161 - 172.16.10.162 | N/A | 172.16.10.163 |

Para IPv6 se utiliza como bloque base la red **2001:25:16::/48** se asigna un prefijo /64 por cada VLAN y enlace, con el identificador de subred incrementándose de forma consecutiva. Como puerta de enlace se utiliza la primera dirección útil de cada subred (.1 en IPv4 y ::1 en IPv6). 

**2.5 Direccionamiento IPv6**
| VLAN | Nombre | Red IPv6 | Primera IP |
|:----:|--------|----------|------------|
| 10 | Estudiantes | 2001:25:16:0::/64 | 2001:25:16:0::1 |
| 20 | Docentes | 2001:25:16:1::/64 | 2001:25:16:1::1 |
| 30 | Académicos | 2001:25:16:2::/64 | 2001:25:16:2::1 |
| 40 | Finanzas | 2001:25:16:3::/64 | 2001:25:16:3::1 |
| 50 | Datacenter | 2001:25:16:4::/64 | 2001:25:16:4::1 |
| 60 | Gestión | 2001:25:16:5::/64 | 2001:25:16:5::1 |
| 70 | Voz | 2001:25:16:6::/64 | 2001:25:16:6::1 |
| 80 | Blackhull-R1 | 2001:25:16:7::/64 | 2001:25:16:7::1 |
| 90 | Blackhull-SWM1 | 2001:25:16:8::/64 | 2001:25:16:8::1 |
| N/A | P2P | 2001:25:16:9::/64 | 2001:25:16:9::1 |

**2.6 Distribución por zona**
| Zona | Dispositivo de enrutamiento | VLAN/LAN asignadas |
|------|-----------------------------|--------------------|
| Zona 1 | SWM1 (switch capa 3, SVI) | Académicos (30), Finanzas (40), Datacenter (50), Blackhole-SWM1 (90) |
| Zona 2 | R1 (router-on-a-stick) | Estudiantes (10), Docentes (20), Gestión (60), Voz (70), Blackhole-R1 (80) |
| Enlace entre zonas | SWM1 ↔ R1 | P2P (100) |

**2.7 Tabla de conexiones**
| Dispositivo A | Interfaz A | Dispositivo B | Interfaz B | Tipo de enlace |
|---------------|:----------:|---------------|:----------:|----------------|
| SRV1 | E0 | SW2 | G0/1 | Acceso (VLAN 50 Datacenter) |
| SW2 | G0/0 | SWM1 | G0/2 | Troncal 802.1Q |
| SWM1 | G0/0 | SW3 | G0/0 | Troncal 802.1Q |
| SW3 | G0/1 | FIN1 | E0 | Acceso (VLAN 40 Finanzas) |
| SW3 | G0/2 | ACA1 | E0 | Acceso (VLAN 30 Académicos) |
| SWM1 | G0/1 | R1 | G0/0 | P2P enrutado (172.16.10.160/30) |
| R1 | G0/1 | SW4 | G0/0 | Troncal 802.1Q (router-on-a-stick) |
| SW4 | G1/0 | DOC1 | E0 | Acceso (VLAN 20 Docentes) |
| SW4 | G0/1 | SW5 | G0/1 | Troncal / EtherChannel |
| SW4 | G0/2 | SW5 | G0/2 | Troncal / EtherChannel |
| SW5 | G2/0 | EST1 | E0 | Acceso (VLAN 10 Estudiantes) |
| SW5 | G3/0 | GES1 | E0 | Acceso (VLAN 60 Gestión) |

