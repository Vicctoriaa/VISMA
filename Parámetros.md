1. “-sS”:
  - este parámetro nos permite hacer un sondeo Tcp pero no establecer conexión En vez de “SYN>SYN/ACK>ACK” hace un “SYN>SYN/ACK/RST” El rst es para cerrar conexión, como no se ha esablecido conexión normalmente no se guarda en los logs ni rastro en los firewalls. 
2.	“-p-“
  - Con esto le indicamos que nos escanee los 65535 puertos totales que hay, 
3. “—open” 
  -	para que nos muestre solo los puertos abiertos y que ignore los filtrados, 
4. “-T5” 
  - Este nos permite modificar la plantilla de temporizado del escaneo, existen 5 modos : paranoico (0), sigiloso (1), amable (2), normal (3), agresivo (4) y loco (5), Los 2 primeros reducen el sondeo tcp para que utilize menos ancho de banda. La 0 sondea 1 puerto a la vez, y espera 5 min antes de enviar cada sonda, el 1 y el 2 son similares, esperan 0.4 segundos entre sondas, el 3 (uso por defecto de nmap) hace sondeos en paralelo, el 4 y el 5 envian sondas cada 5 ms, lo único que les diferencian es el tiempo que esperan para recibir la respuesta de el sondeo TCP. 
•	“—min-rate 5000”
  -	Nos permite especificar la cantidad mínima de paquetes que se envían por segundo, por lo que he investigado el numero que menos falsos positivos y falsos negativos da se encuentra entre el 5000-5800. 
•	”-n” 
  -	Le indicamos que no queremos que nos haga la resolución DNS al host, básicamente pregunta a los servidores DNS cual es el dns del host (nslookup), lo que ocurre es que la resolución hace muy lento el escaneo por lo que lo desactivamos ya que si queremos hacer resolución dns lo podemos hacer manualmente. 
•	“-Pn”
  - En este caso le indicamos que no lance trazas icmp al host, ya que normalmente antes de un escaneo nmap envia trazas icmp para determinar si el host está activo o no, lo que ocurre es que ralentiza mucho el escaneo por lo que lo deshabilitamos ya que si queremos determinar si el host esta vivo tenemos muchísimos otros métodos incluso si las trazas icmp están bloqueadas por firewall físico o de sistema operativo.
•	“-oG”
  - Le indicamos que nos ponga el output en un archivo en formato grepeable para que después cuando lo pase por una función de mi zshrc me copie al portapapeles los puertos abiertos
•	“-vvv” 
  -	Es para ponerlo en verbose mode, básicamente que mientras está haciendo el escaneo nos muestre que puertos encuentra abiertos
•	“-sC”
  -	Nos permite detectar que servicios están corriendo en los puertos abiertos
•	“-sV”
  -	Nos permite detectar la versión de los servicios que estan ejecutándose en los puertos lo que nos permite encontrar vulnerabilidades 
•	“-oN”
  -	Es para exportarlo en formato Nmap por si después tenemos que volver a analizarlo.
