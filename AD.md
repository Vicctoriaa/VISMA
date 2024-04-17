Primero vamos a preparar el entorno de Active directory, para ello empezaremos con un Windows Server Datacenter que tendrá el rol de Domain Controller dentro del Active Directory, seguido de las 3 máquinas win 10 que harán de clientes de este servidor. El Windows Server aparte de tener el rol de DC, también ofrecerá otros servicios como:

+	Correo
+	Servidor DHCP
+	Servidor DNS

Así que después de haber enumerado todos los servicios que ofrecerá, toca crear la máquina. 

## Crear máquinas (DC):

En este caso utilizaremos un Windows Server 2016, pero la versión Datacenter. Y en nuestro caso le pondremos el nombre de máquina DC-Companny.
+ Le Ponemos 4gb de RAM, 2 núcleos y habilitamos el modo EFI.
+ De disco, ponemos el espacio correspondiente (en nuestro caso 400gb).

Una vez tenemos esto, ya podemos iniciar la máquina virtual (si vamos a poner la MV en una red NAT o vamos a modificar la interfaz de red, antes de inicarla, modificarlo). Procedemos con la instalación normal, Al escoger la versión, debemos escoger la “Windows Server 2016 Datacenter Evaluation (Desktop Experience)”.

![Imagen1](https://github.com/Vicctoriaa/VISMA/assets/153718557/7d39d3f8-9302-48b1-b797-98db102d180c)
