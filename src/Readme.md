## DOCUMENTACIÓN Tarea 03 - Docker 01 - Martín Barreiro

### Paso 0: Instalar Docker en nuestro S.O

Esta es la información de mi máquina (equipo anfitrión):

- S.O. y versión: Debian 12.15

- Cores disponibles: 12

- Memoria total (RAM): 15 Gi

- Espacio en disco: 916 GB totales (680 GB disponibles)



Antes de nada, tendremos que instalar Docker en nuestro Sistema Operativo, en este caso Debian

![Captura desde 2026-09-29 10-40-58.png](Capturas/Captura%20desde%202026-09-29%2010-40-58.png)

Una vez descargado, pondremos esta serie de comandos en el CMD: 


 sudo apt update

sudo apt install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL htps://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources <<EOF

![Captura desde 2026-09-29 10-45-57.png](Capturas/Captura%20desde%202026-09-29%2010-45-57.png)

Ahora, siguiendo con el tutorial, copiaremos este comando:

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

A mi me saltó un error, en el que tuve que editar el archivo con

sudo rm /etc/apt/sources.list.d/docker.sources

Para que me quedase así, y funcionara:
![Captura desde 2026-09-29 10-54-09.png](Capturas/Captura%20desde%202026-09-29%2010-54-09.png)

Hecho esto, comprobamos que Docker se está ejecutando:
![Captura desde 2026-09-29 11-00-43.png](Capturas/Captura%20desde%202026-09-29%2011-00-43.png)

![Captura desde 2026-09-29 11-03-00.png](Capturas/Captura%20desde%202026-09-29%2011-03-00.png)

## Paso 1: Imágenes y Contenedores

Descargamos la imagende Alpine sin arrancar:

![Captura desde 2026-09-29 11-08-28.png](Capturas/Captura%20desde%202026-09-29%2011-08-28.png)

## Paso 2: Operaciones con Imágenes

Con este comando, Docker crea el contenedor pero no lo arranca, se queda en estado Created.

Ese número tan grande es el ID que identifica el contenedor, que también le podemos ver el nombre con el segundo comando ejecutado:

![Captura desde 2026-09-29 11-19-34.png](Capturas/Captura%20desde%202026-09-29%2011-19-34.png)

## Paso 3: Operaciones con Contenedores

![Captura desde 2026-09-29 11-34-11.png](Capturas/Captura%20desde%202026-09-29%2011-34-11.png)

Estamos dentro de dam_alp1, para poder interactuar y escribir dentro del contenedor, he necesitado usar los parámetros -i y -t

-i: Activa el modo interactivo, que enlaza la entrada estándar al proceso del contenedor.

-t: Asigna una "segunda termina" (pseudoterminal) para poder trabajar desde la consola.   

Además, se lanza con /bin/sh porque las imágenes mínimas como Alpine no traen bash incluido.

## Paso 4: IP y Ping

Al ejecutar ip a dentro de dam_alp1, comprobamos que Docker le ha asignado la dirección IP 172.17.0.2:
![Captura desde 2026-09-29 11-44-58.png](Capturas/Captura%20desde%202026-09-29%2011-44-58.png)

Confirmamos que hace ping a google.com:
![Captura desde 2026-09-29 11-47-27.png](Capturas/Captura%20desde%202026-09-29%2011-47-27.png)

## Paso 5: Ping entre Contenedores

Abrimos otra ventana del CMD, dejando la primera dentro de dam_alp1, haremos ping con la IP de dam_alp1. Igual que como hicimos con google.com:

![Captura desde 2026-09-29 12-41-05.png](Capturas/Captura%20desde%202026-09-29%2012-41-05.png)

Ahora probamos con el nombre (dam_alp1):

![Captura desde 2026-09-29 12-46-04.png](Capturas/Captura%20desde%202026-09-29%2012-46-04.png)

El ping por nombre falla debido a que en la red bridge los contenedores solo se ven por dirección IP, pero no por nombre.

## Paso 6: Consumo Memoria

Ejecutamos docker stats:

![Captura desde 2026-09-29 12-55-25.png](Capturas/Captura%20desde%202026-09-29%2012-55-25.png)

## Paso 7: Acabar Procesos

Si intentas escribir exit mientras se está ejecutando docker stats no te dejará, ya que está esperando a que salgas de esa monitorización.

Para detenerlo haremos ctrl + c.

Ahora finalizaremos los procesos de los contenedores con exit, ambos dam_alp1 y dam_alp2.

![Captura desde 2026-09-29 13-05-03.png](Capturas/Captura%20desde%202026-09-29%2013-05-03.png)
![Captura desde 2026-09-29 13-05-25.png](Capturas/Captura%20desde%202026-09-29%2013-05-25.png)

Habiendo cerrado los procesos, volvemos a ejecutar docker stats:
![Captura desde 2026-09-29 13-09-01.png](Capturas/Captura%20desde%202026-09-29%2013-09-01.png)

## Paso 8: Disco Ocupado

![Captura desde 2026-09-29 13-15-00.png](Capturas/Captura%20desde%202026-09-29%2013-15-00.png)

Con el comando docker system df comprobamos el espacio total.   

Imágenes : Ocupan 170.4 MB (corresponde a las plantillas base descargadas, como Alpine y el hello world).   

Contenedores (Containers): Ocupan 98 B 