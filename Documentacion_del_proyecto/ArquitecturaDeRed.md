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
