## Proxmox Backup Cloud

En este tutorial, vamos a configurar de manera sencilla el servicio de copias de seguridad en nuestro proveedor de almacenamiento personal en la nube, gdrive, mega, Dropbox, onedrive… añadiéndolo como un datastore más, usando rclone en el que guardaremos nuestras copias de seguridad de manera segura y sin hacer uso de ningún script.
##
### Preparación:

Nos conectaremos por nuestro cliente ssh preferido o desde Shell del propio proxmox y creamos un directorio nuevo en la carpeta /mnt. Le podemos poner el nombre que queramos que nos ayude a identificarlo, por ejemplo si vamos a usar google drive podemos llamarlo gdrive. Por lo que lo haríamos asi:

```
mkdir /mnt/gdrive
```

Ahora vamos a añadir ese directorio a nuestro datastore.

Lo haremos asi:

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen1.png)



A continuación indicamos el nombre gdrive, el directorio que hemos creado y en contenido elegimos Archivo VZDump.

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen2.png)


Le damos a Agregar y como vemos nos añade el nuevo directorio a nuestro datastore

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen3.png)

##
### Usando rclone para el montaje en nuestra nube:

Aunque el directorio llamado gdrive este en nuestro datastore, evidentemente aún no esta montado en la nube, para ello vamos a usar [rclone](https://rclone.org).

Un detalle a tener en cuenta es que rclone a cambiado la manera en que autoriza el [servicio que vincula a nuestra nube](https://rclone.org/remote_setup/). Para ello tenemos que conectarnos por nuestro cliente ssh y lanzar el siguiente código cambiando el usuario por el de proxmox que posiblemente sea root si no lo hemos cambiado y la dirección ip que corresponda de proxmox:

```
ssh -L localhost:53682:localhost:53682 root@ip_proxmox
```
Ahora instalamos rclone:

```
apt-get update;
```

```
apt-get install rclone;
```

Seguimos los pasos. Podemos hacer uso de la [guía](https://rclone.org/docs/) de rclone de nuestra nube.

Importante: Cuando lleguemos a este punto diremos si “y” nos dará una dirección tipo localhost que al crear el vinculo con proxmox podremos abrirlo y autorizarnos.
```
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.
y) Yes
n) No
y/n> y
```
Una vez tengamos configurado rclone sólo falta montarlo. Podemos crear en nuestra nube personal una carpeta que se llame PBC.

Para montar rclone en proxmox y vincularlo con esa carpeta, lo harémos asi:

```
rclone mount gdrive:/PBC /mnt/gdrive --allow-other --allow-non-empty
```
- gdrive:/PBC es la carpeta de nuestra nube.
- /mnt/gdrive es el directiro de proxmox que añadimos a nuestro datastore.

##
### Automatizar el montaje de rclone

Si queremos que rclone se inicie en cada arranque de proxmox, podemos usarlo con un crontab para hacerlo, lo hacemos asi:

```
crontab -e
```
A continuación nos preguntará, en primer lugar, que tipo de editor queremos utilizar para editar el fichero. Yo indico la opción 1, que es la que corresponde a Nano, ya que es el editor por consola que me parece más simple e intuitivo de utilizar. 
Una vez dentro del editor solo falta añadir esta línea tal y como se muestra en la imagen:

```
@reboot rclone mount gdrive:/PBC /mnt/gdrive --allow-other -—allow-non-empty
```
![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen4.png)

Para terminar pulsamos la combinación de teclas control + X
Indicamos “Y” + intro y con esto ya tenemos montado rclone en nuestro proxmox con inicio automático y vinculado a nuestra nube.

Ahora solo nos falta comprobar si hacemos un backup y seleccionamos como disco de destino nuestro gdrive.

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen5.png)

Comprobarémos como al terminar la copia de seguridad esta estará justamente donde queríamos, en nuestra nube.

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen6.png)

##
### A tener en cuenta.

las copias de seguridad que hace proxmox a diferencia de proxmox backup server, no son increméntales. Por lo que en función del espacio que tengamos en nuestra nube, la cantidad de copias que hagamos y el tamaño de las mismas, podremos quedarnos sin espacio pronto.
Para evitar eso podemos añadir un sistema de purga de copias en función de los parámetros que querémos.
Por ejemplo que nos guarde sólo las 5 ultimas copias como se muestra en la imagen.

![This is an image](https://github.com/proxmology/manuales/blob/main/Proxmox%20Backup%20Cloud/imagen7.png)






##
Un tutorial de Proxmology. 
