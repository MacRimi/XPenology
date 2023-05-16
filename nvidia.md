# Utilizar tarjeta nvidia en DS918+,DS920+. DDS923+...

En este tutorial vamos a ver de que manera podemos usar una tarjeta grafica nvidia en un modelo no “compatible” con tarjeta nvidia de serie como los DVA 3221 y 3219.
Modelos confirmados:
-	DS918+
-	DS920+
-	DS923+
-	DS1520+
-	DS1621+
-	DS2422+

Los modelos XS no son compatibles.

##
### Preparación:

Añadimos este repositorio de origenes de paquetes:

https://spk7.imnks.com/


![This is an image](imagenes/nvidia1.png)


Y nos instalamos el paquete NVIDIA Runtime Library

![This is an image](imagenes/nvidia2.png)

##
### Parchamos el controlador:

Una vez instalado reiniciamos nuestro Nas. Cuando reinicie nos conectamos por SSH y no logueamos como root

```
sudo -i
```
Una vez como root copiamos y pegamos esto:
```
cd /var/packages/NVIDIARuntimeLibrary/conf && mv -f privilege.bak privilege
```

```
cd /var/packages/NVIDIARuntimeLibrary/scripts && ./start-stop-status start
```

Comprobamos que todo esta correcto con estos comandos:

```
nvidia-smi -pm 1
```

```
ls /dev/nvid*
```
Si todo esta correcto deberíamos ver algo así:

![This is an image](imagenes/nvidia3.png)

Ahora comprobamos que este instalado el controlador y la tarjeta grafica este reconocida

![This is an image](imagenes/nvidia4.png)


##


### Usar nuestro servidor multimedia con NVIDIA:

Hay que tener en cuenta que Synology lleva su ritmo a la hora de las actualizaciones y como hemos podido observar el controlador que nos instala el paquete NVIDIA Rumtime Library es la versión: NVIDIA-SMI 440.44
Eso puede generarnos un problema con nuestros servidores de medios que requieran versiones superiores, como caso de Plex, Jellyfin y posiblemete Emby.


#### Plex:

- Puesto que la versión de Plex Media Server v1.20.2 requiere como mínimo el controlador de nvidia 450.66, tenemos que instalarnos la versión de Plex modificada para no tener problemas con el controilador, de este repositorio de paquetes.

![This is an image](imagenes/nvidia5.png)


- También vamos a necesitar instalarnos este paquete de Synology puesto que lo necesitaremso para modificar el archivo Preferences.xml


![This is an image](imagenes/nvidia6.png)


- Abrimos el editor de texto recién instalaso y seleccionamos el archivo Preferences.xml ubicado en: /PlexMediaServer/AppData/Plex Media Server


![This is an image](imagenes/nvidia7.png)


- Una vez abierto el archivo Preferences.xml nos dirigimos al final de la línea y añadimos los siguiente:

```
HardwareDevicePath="/dev/nvidia0"
```


![This is an image](imagenes/nvidia8.png)


- Guardamos los cambios y cerramos el archivo. También deteneos Plex y lo volvemos a arrancar. Esto es importante para que aplique el cambio que hemos modificado y utilice nuestra tarjeta nvidia.


#### Emby:


En ajustes vamos a transcodificación y habilitamos la aceleración de hardware (si está disponible): 
En vanzado, cambiamos a NVDEC (recomendado) o CUVID según el decodificador de hardware preferido.


#### Jellifyn:


Puesto que el controlador de nvidia es el 440.44 solo podemos usar la versión 10.7.7 de Jellifyn. Las versiones superiores no son compatibles con este controlador y no podremos usar nuestra tarjeta nvidia.
Aceleración de hardware seleccionamos: Nvidia NVENC


### Banco de pruebas:


- Placa base: Asrock J4205  PCIe 2.0 x1
[modificada bios para arranque rápido]( https://xpenology.com/forum/topic/63876-j3455-xpenology-slow-boot-solution/page/2/)
- Adptador PCIe x1 a x16
- Trajeta grafica: nvidia P620
- Modelo DSM: DS918+
- Boot: tinycore-redpill M-shell 

Os paso unas capturas para que veáis las pruebas:


![This is an image](imagenes/nvidia9.png)



![This is an image](imagenes/nvidia10.png)



![This is an image](imagenes/nvidia11.png)


### Conclusion:

Con una grafica potente, podemos usar una placa base discreta de bajo consumo y hacer una buena actualización a nuestro Nas. 

Que lo disfrutéis!

