**Arquitectura de la Red DPT**

La red de **DOÑA PANCH TECH** se organiza en dos sedes (o zonas) interconectadas mediante un enlace punto a punto entre un switch capa 3 (SWM1) y un router (R1). Cada zona tiene su propia segmentación por VLANs, y el enrutamiento entre ellas se resuelve con dos métodos distintos: una switch capa 3 en el lado izquierdo y router-on-a-stick en el lado derecho. 

**Zona 1: Switch capa 3 (SWM1)**
- **SWM1** actúa como punto central de esta zona y realiza el enrutamiento entre
VLANs mediante interfaces SVI.

- **SW3** es el switch de acceso al que se conectan FIN1 (Finanzas) y ACA1
(Académicos).

- **SW** conecta el servidor SRV1 a través de la LAN Datacenter.
La conexión SWM1 ↔ R1 funciona como un enlace P2P.

**Zona 2: Router-on-a-stick (R1)**
- **R1** conecta con **SW4** mediante un único enlace troncal (802.1Q), sobre el cual se
configuran las subinterfaces, una por VLAN.

- **SW4** y **SW5** se interconectan con dos enlaces redundantes, que pueden agruparse en
un EtherChannel para dar más ancho de banda y tolerancia a fallos.

- Los equipos finales **DOC1** (Docentes), **EST1** (Estudiantes) y **GES1** (Gestión) se
conectan a los switches de acceso.

**Topologia de la red**

<img width="1597" height="537" alt="Topologia DPT" src="https://github.com/user-attachments/assets/5b977faf-5aa9-4210-b433-dac9214a73a1" />

**1.1 Dispositivos de la Red**
Los dispositivos necesarios para la implementación de la red:
| Cantidad | Dispositivo | Nombre | Función |
|:------:|:--------:|-------------|--------|
| 1 | Router | R1 | Router-on-a-stick para la Zona 2 y enlace P2P hacia SWM1 |
| 1 | Switch Capa 3 | SWM1 | Enrutamiento inter-VLAN con SVI en la Zona 1 |
| 4 | Switch Capa 2 | SW2, SW3, SW4, SW5 | Acceso de usuarios, servidor y troncales |

**1.2 Dispositivos finales**
| Cantidad | Dispositivo | Nombre | VLAN/LAN |
|:--------:|-------------|--------|----------|
| 1 | Servidor | SRV1 | LAN Datacenter |
| 1 | PC | FIN1 | Finanzas |
| 1 | PC | ACA1 | Académicos |
| 1 | PC | DOC1 | Docentes |
| 1 | PC | EST1 | Estudiantes |
| 1 | PC | GES1 | Gestión |
