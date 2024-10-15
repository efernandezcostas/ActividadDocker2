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
