Primero vamos a preparar el entorno de Active directory, para ello empezaremos con un Windows Server Datacenter que tendrá el rol de Domain Controller dentro del Active Directory, seguido de las 3 máquinas win 10 que harán de clientes de este servidor. El Windows Server aparte de tener el rol de DC, también ofrecerá otros servicios como:

+	Correo
+	Servidor DHCP
+	Servidor DNS

Así que después de haber enumerado todos los servicios que ofrecerá, toca crear la máquina. 

## Crear máquinas (DC):

En este caso utilizaremos un Windows Server 2016, pero la versión Datacenter. Y en nuestro caso le pondremos el nombre de máquina DC-Companny:

![Imagen1](https://github.com/Vicctoriaa/VISMA/assets/153718557/0546c1b1-835a-44d7-874b-c516ae2d148d)
