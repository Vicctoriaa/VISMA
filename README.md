# VISMA 
Este es un script de instalación de BSPWM para kali o parrot linux, cabe aclarar que puede funcionar en otras distribuciones base debian sin embargo en las unicas que se mantiene estable y las cuales les daremos soporte será Kali linux y Parrot linux

> [!NOTE]
> Sigue los pasos tal y como indican, hay palabras con un numero que te llevan a una definición de la palabra, en caso de que haya alguna que no sepas debereas buscar información tu mismo/a. Este manual no lleva nada que sea demasiado complicado o que la información no aparezca por internet.

> [!IMPORTANT]
> la ip que utilizamos no sera la misma que la de tu red por eso mismo deberás estar atento a esos puntos. Igualmente indicamos cuando debes poner la ip de la máquina de la maerna siguiente `IP_DE_X`


El pivoting es una técnica utilizada en la ciberseguridad donde utilizamos una máquina como intermediaria para poder acceder a otra no visible, como esta imagen explica:

![pivoting](https://github.com/Vicctoriaa/VISMA/assets/153718557/833fbfe8-aac0-4f85-af1e-6f603126766e)


Una vez explicado cómo funciona el pivoting vamos a explicar cómo hemos sido capaces de comprometer un laboratorio con 6 máquinas (1 Windows y 5 linux) donde hay 4 redes internas configuradas. Aunque antes de ello debemos preparar el entorno.
### 1. [Aragog](https://github.com/Vicctoriaa/VISMA/blob/main/aragog.md)
Se trata de la primera máquina con 2 flags. Esta cuenta con una interfaz de red que trabajará en el mismo segmento de red que la máquina atacante. Consta con otra interfaz conectada a la VMNET 2 donde en ese mismo segmento de red se encuentra la segunda máquina del pack: Nagini.


      
