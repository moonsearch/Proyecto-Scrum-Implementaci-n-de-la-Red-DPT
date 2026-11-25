**Configuración de SSH**

Los seis equipos de red (SWM1, SW, SW3, SW4, SW5 y R1) tienen SSH habilitado con la misma configuración base. Cada uno tiene un nombre de host y el dominio donapanch.local. El acceso se controla con un usuario local llamado admin, con privilegio 15 y contraseña cifrada, de modo que quien se autentica entra directamente al modo de máxima administración. Además, el modo privilegiado está protegido con enable secret. 

La administración está organizada en dos grupos. Los equipos del lado de **SWM1** se administran por la VLAN 50 (Datacenter): SWM1 responde en 172.16.10.1, SW en 172.16.10.3 y SW3 en 172.16.10.4. Los equipos del lado de **R1** se administran por la VLAN 60 (Gestión): SW4 responde en 172.16.10.66 y SW5 en 172.16.10.67. El router R1 se alcanza por la IP de su enlace con SWM1 (172.16.10.162) o por cualquiera de sus subinterfaces, por ejemplo 172.16.10.65 en la VLAN de Gestión. 
