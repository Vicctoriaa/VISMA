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

![Imagen1](https://github.com/Vicctoriaa/VISMA/assets/153718557/f14d5c9a-b3a1-46be-9efe-731832da937f)

Una vez Instalado, nos pedirá poner una contraseña de administrador para la Máquina, se la indicamos y acabará de iniciar. Una vez acabe de iniciar, nos logueamos y nos aparecerá lo siguiente:

![Imagen2](https://github.com/Vicctoriaa/VISMA/assets/153718557/eeb2f9da-7db9-45f9-af32-d21aa1e04daa)

## Instalando servicios:

Para poder instalar los servicios que necesitamos lo que haremos sera dirigirnos a manage > add roles and Features y desde ahí podremos instalar lo que necesitamos que en nuestro caso serian 3 servicos necesarios, active directory, dns y dhcp.
Solamente hemos de seleccionarlo en el apartado server roles, tal y como se muestra en la imagen.

![Imagen3](https://github.com/Vicctoriaa/VISMA/assets/153718557/4807130e-5fc4-42c1-a545-f9359b4bcdaf)

Aceptaremos todo y en el apartado de confirmación, confirmaremos pulsando “install”para así se instale lo escogido anteriormente.
Una vez instalado nos saldrá de esta manera, ya podremos cerrarlo.

Arriba a la derecha habrá una bandera el cual son las notificaciones, pulsaremos en ella y nos deberá salir un apartado el cual será el active directory, si pulsamos en él, nos llevará a la siguiente pestaña (lo que se muestra en imagen), crearemos nuestro propio dominio con el nombre que queramos.

![Imagen4](https://github.com/Vicctoriaa/VISMA/assets/153718557/72127c28-d569-4ba2-ad77-8c599e10cc41)

Seguido nos pedirá una constraseña para nuestro dominio, tendremos que tener activado (en la misma pestaña) "Domain Name System (DNS) server" y "Global Catalog (GC)". En el apartado opción adicional, le pondremos el nombre de nuestro proyecto. 
Una vez aceptemos todo nos pedirá reiniciar la máquina, la reiniciamos.


## Change Hostname: 

Para cambiar el nombre pc haremos click derecho en el símbolo de Windows de nuestra barra de tareas y nos dirigiremos a "System".
Nos saldrá una ventana con datos del sistema, deberemos darle a "Change settings".

![Imagen5](https://github.com/Vicctoriaa/VISMA/assets/153718557/ff524d02-dc1e-42c7-aa53-11f15baf89b3)

Nos saltara otra ventana el cual para poder cambiarle el nombre deberemos pulsar en "Change", escribiremos el nombre que queramos ponerle a nuestro pc, cuando aceptemos nos pedirá reiniciar para que se pueda cambiar correctamente, no es obligatorio reiniciar pero seria conveniente.

## Creación de usuarios AD:

Comenzaremos a crear los usuarios del active directory, para ello iremos a Tools > Active directory Users and Computers, nos saldrá una ventana tal y como se muestra en la imagen el cual haremos click derecho > users > new y crearemos los usuarios que necesitemos, con una contraseña sergura. Nosotros hemos creado el usuario de Ismail y Victoria como usuarios sin privilegios. 

![Imagen6](https://github.com/Vicctoriaa/VISMA/assets/153718557/7cae5d92-0d95-4080-b22e-7cd707a257c2)

Una hayamos creado los usuarios necesarios en nuestro active directory, pasaremos al siguiente paso.

## Configuración del DHCP:

Para configurar el DHCP será importante tenerlo instalado como comentamos en el primer punto. Comenzaremos yendo a la bandera veremos la notificación del DHCP, cuando le demos a "complete DHCP configuration" se nos abrira una ventana, en autorizacion pondremos el nombre que queramos,en nuestro caso pusimos VISMA\Administrator, aceptamos y instalamos. 

![Imagen7](https://github.com/Vicctoriaa/VISMA/assets/153718557/396c0986-f835-45b3-809c-3b15b961b9f5)

Seguido iremos a tolos > DHCP, entraremos abriremos las carpetas hasta entrar a iPv4, añadiremos una nueva ip comenzando por la ip 192.168.1.40 hasta la 192.168.1.200, con la máscara 255.255.255.0 pondremos 90 días, como router pondremos la ip 192.168.1.1 como name Domain pondremos el que pusimos al crear nuestro dominio visma.local  
 
![Imagen8](https://github.com/Vicctoriaa/VISMA/assets/153718557/5da1339a-0eac-4d1f-acee-d647b3c23dd4)

## Installing hmailserver:

Para que todo vaya correctamente deberemos instalar una característica llamada ".NET Framework 3.5".

![Imagen9](https://github.com/Vicctoriaa/VISMA/assets/153718557/95aed4f6-5ff2-4430-8f36-7c2ff5be02df)

Una vez este instalada, iremos a nuestro navegador y instalaremos el hmailserver (url), cuando ya lo hayamos instalado, lo iniciaremos, nos pedirá una contraseña pondremos la que nosotros queramos , una vez dentro añadiremos un dominio, pondremos el dominio que creamos anteriormente en nuestro AD, pondremos usuarios lo que creamos anteriormente yendo a nuestro dominio Accounts > Add, pondremos el nombre que queramos pero es recomendable poner el de los usuarios que creamos en nuestro AD.

![Imagen10](https://github.com/Vicctoriaa/VISMA/assets/153718557/451d6269-360f-4c71-bc3a-75f783716346)

## Creando y configurando los clientes del AD DC:

Empezaremos creando las máquinas de cada usuario, omitimos la instalación desatendida para poder escoger la versión de Windows, ya que para poder ser clientes del AD, debemos contar con la versión profesional de Windows (Windows pro). La instalamos con las carateristicas que queramos, en nuestro caso al tener un ordenador estable le pusimos las siguientes características:

![Imagen11](https://github.com/Vicctoriaa/VISMA/assets/153718557/51369ba7-0917-45a0-95fb-05df81c6502c)

Una vez tengamos la máquina la clonaremos identicamente para asi poder hacer la máquina del otro usuario. Una vez cloanda comenzaremos a instalarla, en nuestro caso le hemos puesto al nombre de equipo LocalAdmin, cuando acabemos de instalarla. Iniciar la terminal/cmd, pondremos ipconfig (comando el cual nos sirve para saber que ip tiene ) encontraremos que los 2 tienen la misma ip, si revisamos los dispositivos a los cuales el Windows server ha dado ip, solo nos aparece 1:

![Imagen12](https://github.com/Vicctoriaa/VISMA/assets/153718557/cd44ee9b-da67-41b0-bf3d-dcdafd760b02)

En lo que nos deberíamos fijar es en el “Unique ID”, este es la dirección MAC y es que al clonar las máquinas también se ha clonado la dirección MAC. Por lo que, para resolver este problema, simplemente debemos Apagar una máquina, entrar en configuración > red > Avanzado. Y aquí pedir otra MAC. Y ya está, se han cambiado solo las últimas 3 duplas de la dirección MAC, ya que las 3 primeras, hacen referencia al fabricante (Virtual box).
Una vez tenemos esto, ya podemos inicar la máquina y ver que tenemos una dirección ip distinta y además nos aparece en “Adresses Leases” en el Win Server.

![Imagen13](https://github.com/Vicctoriaa/VISMA/assets/153718557/3e8b3313-bfa7-4d5c-933c-3aad8fc88dd0)

## Añadir ordenadores al AD:

Para añadir ordenadores al AD, es decir, añadirles nuestro ddominio, deberemos dirijirnos a la confirguación de nuestra máquina, dentro de cuentas > Obtener acceso a trabajo o escuela, seleccionaremos conectar como nosotros no tendremos que poner un correo eléctronico sino un dominio, le daremos abajo, donde dice "UNir este dispositivo a un dominio local AD", pondremos nuestro dominio visma.local.

![Imagen14](https://github.com/Vicctoriaa/VISMA/assets/153718557/7cb03d7e-24f9-4252-b0a3-67814f73ad61)

Pondremos el usuario que queramos poner en ese ordenador, es decir, si estamos poniendo el dominio en la máquina Isma, pondremos le usuario ismail, seguido el tipo de cuenta la dejaremos con administrador, y reiniciaremos, lo mismo haremos en con la otra máquina, Victoria.

![Imagen15](https://github.com/Vicctoriaa/VISMA/assets/153718557/d97b13b6-1319-4b90-a63f-8f32fc283e08)

Lo siguiente será quitar el antivirus de nuestras máquinas. Para quitar el de nyestro serividor abriremos la PowerShell y pondremos el siguiente comando:

`Unistall-WindowsFeature -Name Windows-Defender`

![Imagen16](https://github.com/Vicctoriaa/VISMA/assets/153718557/ca11180b-26f9-40da-9eba-9883763a192f)

Para quitar el antivirus de nuestras máquinas usuarios nos dirigiremos a configuración > Actualización y seguridad > Seguridad win > Protección contra virus y amenazas > Admin de la configuración, lo desactivamos todo. Luego seguido para confirmarlo haremos win + r ponemos gpdit.msc, nos dirigimos a la carpeta, plantilla admin > antivirus win defender > veremos si esta desactivado al completo, en caso de que no lo desactivamos manualmente.

![Imagen17](https://github.com/Vicctoriaa/VISMA/assets/153718557/644d3cad-6c42-49c4-8546-7ffe1e1d38ae)




