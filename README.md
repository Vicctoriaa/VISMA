# Maquinas

Este es un script de instalación de BSPWM para kali o parrot linux, cabe aclarar que puede funcionar en otras distribuciones base debian sin embargo en las unicas que se mantiene estable y las cuales les daremos soporte será Kali linux y Parrot linux

## Maquina aragog
(AVISO: No ejecutes el script como ROOT o SUDO, el script te pedirá tu contraseña por cuenta propia, si lo ejecutas como root la instalación se detendrá)
(IMPORTANTE: la ip que utilizamos no sera la misma que la de tu red por eso mismo deberás estar atento a esos puntos, igualmente lo indicamos)

Una vez explicado cómo funciona el pivoting vamos a explicar cómo hemos sido capaces de comprometer un laboratorio con 6 máquinas (1 Windows y 5 linux) donde hay 4 redes internas configuradas. Aunque antes de ello debemos preparar el entorno:

Empezamos con la máquina Aragog, ya que es la que se encuentra en nuestra red. Primero debemos encontrarla y para ello necesiatremos convertirnos en root.
Si quieres aprender a vulnerarla sigue los pasos como se indican.

1.- RED

```
sudo nano /etc/network/interface
```
Nos tiene que quedar como se muestra la imagen.
![Captura de pantalla 2024-02-27 193951](https://github.com/Vicctoriaa/VISMA/assets/153718557/44a1151b-10f4-4293-9d0d-6758bdec6e69)

En caso de que queramos saber que hemos puesto en la interface podemos utilizar el siguiente comando:

```
cat /etc /network/interface
```

Reiniciamos la maquina, y nos dirigimos a nuestra maquina principal es importante tener la Aragog encendida para que nos encuentre la ip, para saber que ip tiene escanearemos la red poniendo el siguiente comando:
```
arp-scan -I ens33 –localnet 
```
Si solamenente queremos buscar la maquina, y estamos usando VmWare podremos añadirle el comando grep y entre comillas pondremos lo siguiente:
```
arp-scan -I ens33 –localnet | grep "VMware, Inc."
```
![segunda cap_arp scan](https://github.com/Vicctoriaa/VISMA/assets/153718557/0adfdaf6-8c45-4ebb-8f52-129be138e098)


2.-ATAQUE

Revisaremos si la maquina se encunetra activa o si hay algun firewall bloqueando las trazas ICMP, para ello debermos hacer un ping. 

```
ping -c 1 IP_DE_LA_ARAGOG
```
![tercera cap_ping](https://github.com/Vicctoriaa/VISMA/assets/153718557/a16ed55c-d8de-43e8-aeab-ed765bd07e2c)

Una vez le hemos mandado un ping, podemos revisar que la máquina esta encendida, también vemos que el ttl = 64 por lo que es una máquina Linux, si el ttl es “=” o menor a 64 quiere decir, que probablemente estamos ante una máquina Linux. 
Podemos observar también que ningún paquete a sido descartado, entonces ya sabemos que esta en el mismo segmento de red y esta preparada para ser vulnerada.

Una vez tengamos esa información, deberemos hacer un escaneo de puertos para ello utilizaremos la herramienta nmap, especializada en escanear puertos, pondremos parametros ya que queremos solamente cosas especificas. (si quieres saber sobre los parametros entra aqui:.....)
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

En caso de que queramos saber que hemos puesto en la interface podemos utilizar el siguiente comando:

```
cat /etc /network/interface
```

Reiniciamos la maquina, y nos dirigimos a nuestra maquina principal es importante tener la Aragog encendida para que nos encuentre la ip, para saber que ip tiene escanearemos la red poniendo el siguiente comando:
```
arp-scan -I ens33 –localnet 
```
Si solamenente queremos buscar la maquina, y estamos usando VmWare podremos añadirle el comando grep y entre comillas pondremos lo siguiente:
```
arp-scan -I ens33 –localnet | grep "VMware, Inc."
```
![segunda cap_arp scan](https://github.com/Vicctoriaa/VISMA/assets/153718557/0adfdaf6-8c45-4ebb-8f52-129be138e098)







#===============================MIS-REDES==================================#

Practicamente ZLCube en todos lados

https://www.youtube.com/@zlcube9936

https://www.twitter.com/zlcube

https://www.tiktok.com/@zlcube

https://www.twitch.tv/zlcube

https://www.instagram.com/zlcube

#=========================================================================#

