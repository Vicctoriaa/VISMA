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

(IMAGEN)

Una hayamos creado los usuarios necesarios en nuestro active directory, pasaremos al siguiente paso.

## Configuración del DHCP:

Para configurar el DHCP será importante tenerlo instalado como comentamos en el primer punto. Comenzaremos yendo a la bandera veremos la notificación del DHCP, cuando le demos a "complete DHCP configuration" se nos abrira una ventana, en autorizacion pondremos el nombre que queramos,en nuestro caso pusimos VISMA\Administrator, aceptamos y instalamos. 

(Imagen)

Seguido iremos a tolos > DHCP, entraremos abriremos las carpetas hasta entrar a iPv4, añadiremos una nueva ip comenzando por la ip 192.168.1.40 hasta la 192.168.1.200, con la máscara 255.255.255.0 pondremos 90 días, como router pondremos la ip 192.168.1.1 como name Domain pondremos el que pusimos al crear nuestro dominio visma.local  

(imagenes)

## Installing hmailserver:

Para que todo vaya correctamente deberemos instalar una característica llamada ".NET Framework 3.5". Una vez este instalada, iremos a nuestro navegador y instalaremos el hmailserver (url), cuando ya lo hayamos instalado lo iniciaremos, nos p


