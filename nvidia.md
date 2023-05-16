## Utilizar tarjeta nvidia en DS918+,DS920+. DDS923+...

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
###  
