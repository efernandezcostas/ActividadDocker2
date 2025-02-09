### 1. Descarga la imagen "alpine" SIN ARRANCARLA y comprueba que está en tu equipo

La imagen se puede descargar con el comando: <code>docker pull alpine</code>.  
Para comprobar que la imagen se ha descargado correctamente utilizamos <code>docker images</code>, obteniendo:   
~~~
alpine        latest    91ef0af61f39   5 weeks ago     7.8MB
~~~

### 2. Crea un contenedor sin ponerle nombre. ¿está arrancado? Obtén el nombre

Para crear el contenedor se utiliza: <code>docker create alpine</code>. Si quisiesemos ponerle nombre deberiamos añadir *--name* en los argumentos, por ejemplo: <code>docker create --name Alpine alpine</code>  
El contenedor no está arrancado porque utilicé el comando *create* en vez del comando *run*.  
Para comprobar que se creó correctamente y obtener su nombre utilizamos <code>docker ps -a</code>, obteniendo:    
~~~
4888d77c9d4b   alpine        "/bin/sh"   6 minutes ago   Created                             distracted_napier
~~~

### 3. Crea un contenedor con el nombre 'dam_alp1'. ¿Como puedes acceder a él?

Podemos crear el contenedor y ejecutarlo inmediatamente con el comando: <code>docker run -it --name dam_alp1 alpine /bin/sh</code>   
Si no lo ejecutamos directamente podemos utilizar luego el comando <code>docker exec -it dam_alp1 /bin/sh</code>   
- ***-it***: Hace que sea interactivo   
- ***/bin/sh*** Se utiliza porque no tiene bash, en caso contrario se utilizaría */bin/bash*
~~~
enrique@damenrique:~$ docker exec -it dam_alp1 /bin/sh
/ # ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
~~~

### 4. Comprueba que ip tiene y si puedes hacer un ping a google.com

La IP la comprobamos igual que en la terminal del ordenador común, con el comando: <code>ip a</code>   
~~~
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
26: eth0@if27: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
~~~

Para hacer ping a google utilizamos el comando <code>ping google.com</code>
~~~
/ # ping google.com
PING google.com (142.250.201.78): 56 data bytes
64 bytes from 142.250.201.78: seq=0 ttl=109 time=17.258 ms
64 bytes from 142.250.201.78: seq=1 ttl=109 time=17.562 ms
64 bytes from 142.250.201.78: seq=2 ttl=109 time=17.454 ms
64 bytes from 142.250.201.78: seq=3 ttl=109 time=17.519 ms
^C
--- google.com ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 17.258/17.448/17.562 ms
~~~

### 5. Crea un contenedor con el nombre 'dam_alp2'. ¿Puedes hacer ping entre los contenedores?

Creamos el contenedor dam_alp2 y hacemos ping a la ip del contenedor dam_alp1 con el mismo comando pero utilizando su ip <code>ping 172.17.0.3</code>, y vemos que sí se puede hacer ping entre ellos.   
~~~
enrique@damenrique:~$ docker start dam_alp2 
dam_alp2
enrique@damenrique:~$ docker exec -it dam_alp2 /bin/sh
/ # ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.082 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.193 ms
64 bytes from 172.17.0.3: seq=2 ttl=64 time=0.120 ms
^C
--- 172.17.0.3 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.082/0.131/0.193 ms
~~~

### 6. Sal del terminal, ¿que ocurrió con el contenedor?

Al salir de la terminal e intentar hacer ping al contenedor desde el equipo vemos que permite hacer ping pero no llegan los paquetes.
~~~
dam@A09equipo21:~$ ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
^C
--- 172.17.0.3 ping statistics ---
6 packets transmitted, 0 received, 100% packet loss, time 5126ms
~~~

Una vez dentro de docker utilizo el comando <code>docker ps -a</code> y los contenedores aparecen todavía en ejecución   
~~~
enrique@damenrique:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND     CREATED          STATUS                    PORTS     NAMES
025713cc8081   alpine        "/bin/sh"   7 minutes ago    Up 4 minutes                        dam_alp2
f71d2ccbf20c   alpine        "/bin/sh"   33 minutes ago   Up 31 minutes                       dam_alp1
4888d77c9d4b   alpine        "/bin/sh"   54 minutes ago   Created                             distracted_napier
fcdfbf24ca42   hello-world   "/hello"    24 hours ago     Exited (0) 24 hours ago             pensive_shannon
~~~

### 7. ¿Cuanta memoria en el disco duro ocupaste?

Podemos comprobar cuanto espacio están ocupando las imágenes y los contenedores con el comando <code>docker system df</code>
~~~
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          2         2         7.811MB   0B (0%)
Containers      6         2         205B      0B (0%)
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
~~~

### 8. ¿Cuanta RAM ocupan los contenedores? ¿Hay algún comando docker para saber esto?

Sí, existe el comando <code>docker stats</code> que es como el comando <code>top</code> en Linux, lo que muestra información de los procesos en ejecución.
~~~
CONTAINER ID   NAME       CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
025713cc8081   dam_alp2   0.00%     512KiB / 10.24GiB   0.00%     1.01kB / 0B       0B / 0B     1
f71d2ccbf20c   dam_alp1   0.00%     528KiB / 10.24GiB   0.00%     10.2kB / 9.47kB   0B / 0B     1
~~~
