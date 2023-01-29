# Práctica 4.3 GBD Despliegue de una arquitectura EFS-EC2-MultiAZ
En primer lugar creamos un grupo de seguridad que lo llamaremos SGweb en el que abriremos el puerto 80 para el servicio http y otro grupo de seguridad llamado SGefs en el que abriremos el puerto 2049 para el NFS:
<img src="grupos%20seguridad.png">

En el grupo de seguridad SGefs solo podrán acceder los equipos que estén dentro del grupo de seguridad SGweb.
<hr>
Ahora lanzamos una instancia EC2 con sistema operativo amazon linux con par de claves vockey y con el grupo de seguridad SGweb, con subnet en la zona us-east-1a y que nos asigne una ip pública. 
<img src="ec2%20conf%20red.png">

Por último en datos de usuario al final del todo, copiaremos los siguientes comandos para inrtalar caracteristicas a la maquina:
<img src="datos%20usuario.png">
Y lanzamos la instancia.
<hr>
Creamos otra EC2 igual a la anterior pero con la subnet en la zona us-east-1b
<img src="ec2_2%20conf%20red.png">
y los datos de usuario, igual a la maquina anterior:
<img src="datos%20usuario.png">
Y lanzamos la instancia.

<hr>
Nos vamos al grupo de seguridad SGweb y abrimos el puerto ssh para poder conectarnos a la instancia via ssh 
<img src="ssh%20sgweb.png">
<hr>
Creamos un sistema de ficheros EFS el cual nos cobrará amazon por usarlo pero es una cantidad muy pequeña ya que no habra mucha información dentro de el:
<img src="crear%20efs.png">
 cuando tengamos el sistema de ficheros creado, entramos a editar la red y en las zonas que hemos usado en las maquinas EC2 que han sido us-east-1a y 1b, le cambiamos el grupo de seguridad al que creamos anteriormente, SGefs.
 <img src="grupo%20seguridad%20zonas.png">
 <hr>
 Nos conectamos a las instancias EC2 creadas:
 <img src="conectar1.png">
 <img src="conecctar2.png">
 <hr>
 Cando nos inicie el command prompt de la primera EC2 creamos una carpeta dentro de /var/www/html llamada efs-mount:
 <img src="mkdir%20efs-mount.png">
Y montaremos el sistema de ficheros dentro de la carpeta que hemos creado con el siguiente comando:
<img src="sudo%20mount.png">
Cambiando el Id por el de nuestro sistema EFS.
<hr>
Entramos en la carpeta efs-mount y realizamos un wget para descargarnos el .zip de la pagina web desde internet.
<img src="wget.png">
Cuando tengamos la pagina descargada descomprimimos el fichero descargado.
<hr>
Para que al buscar la ip en internet nos salga directamente la pagina web, debemos cambiar la ruta dentro del fichero /etc/httpd/confd/hhtpd.conf
<img src="sudo%20nano.png">
Cambiamos la ruta que hay en Documentroot por la de la carpeta efs-mount:
<img src="documntroot.png">
Y reiniciamos el servicio http:
<img src="restart%20httpd.png">
Y al introducir la ip en el navegador nos saldrá directamente la pagina web:
<img src="pagina%20peliculas.png">

<hr>

<b>TODOS LOS PASOS REALIZAMOS DESDE EL COMMAND PROMPT, SE REALIZARAN DE IGUAL MANERA EN LA SEGUNDA EC2</b>