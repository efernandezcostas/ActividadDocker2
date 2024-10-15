### 1. Descarga la imagen "alpine" SIN ARRANCARLA y comprueba que está en tu equipo

La imagen se puede descargar con el comando: <code>docker pull alpine</code>.  
Para comprobar que la imagen se ha descargado correctamente utilizamos <code>docker images</code>, obteniendo:   
~~~
alpine        latest    91ef0af61f39   5 weeks ago     7.8MB
~~~

### 2. Crea un contenedor sin ponerle nombre. ¿está arrancado? Obtén el nombre

Para crear el contenedor se utiliza: <code>docker create alpine</code>. Si quisiesemos ponerle nombre deberiamos añadir *--name* en los argumentos, por ejemplo: <code>docker create --name=Alpine alpine</code>  
El contenedor no está arrancado porque utilicé el comando *create* en vez del comando *run*.  
Para comprobar que se creó correctamente y obtener su nombre utilizamos <code>docker ps -a</code>, obteniendo:    
~~~
4888d77c9d4b   alpine        "/bin/sh"   6 minutes ago   Created                             distracted_napier
~~~
