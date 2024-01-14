# Instalar  GPU nvidia y actualizar el controlador a la versión 510
#

En este tutorial vamos usar una tarjeta grafica nvidia e instalar el controlador 510.108.
<br>
Esta versión del controlador nos permitirá actualizar nuestros servidores de contenido como Plex, emby etc… y no tener la limitación que teníamos antes ya que estábamos usando el controlador 440. La ventaja de este nuevo controlador es que puede usarse para GPU como para VGPU. Aunque más adelante se explicara como se instala para una VGPU (Virtual GPU) de momento vamos a ver como se instala en una grafica física.
<br>
El merito es del usuario: usuario: [pdbear · he/him](https://github.com/pdbear/syno_nvidia_gpu_driver) y del equpo de desarrollo de kkk.

Las tarjetas compatibles son las mismas que podriamos usar en los DVA.
<br>
Como característica especial:
<br>
- El controlador se actualiza a la versión: 510.108.03
- Funciona en la mayoia de modelos DSM cuya plataformas sean x86_64 con la versión del núcleo 4.4.302+. (7.2)

### Preparación:

Añadimos este repositorio de origenes de paquetes:

https://spk7.imnks.com/


![This is an image](imagenes/nvidia1.png)

<br>

 ### Y nos instalamos el paquete NVIDIA GPU Driver 

<br>

![This is an image](imagenes/nvidia13.png)

<br>

Cuando nos lo pida, aceptamos los términos y empezamos la instalación.

<br>

![This is an image](imagenes/nvidia14.png)

<br>

En este paso importante elegir GPU.

<br>

![This is an image](imagenes/nvidia15.png)

<br>

Aquí lo dejamos en blanco como esta, ya que esta opción es para las VGPU que no es nuestro caso ahora y seleccionamos siguiente.

<br>

![This is an image](imagenes/nvidia16.png)

<br>

Y finalizamos la instalación.

<br>

![This is an image](imagenes/nvidia17.png)

#

Una vez instalado nos conectamos por SSH y no logueamos como root

```
sudo -i
```
Una vez como root copiamos y pegamos esto y ejecutamos:
```
vgpuDaemon fix
```
<br>

![This is an image](imagenes/nvidia18.png)

#

¡Ahora importante!! vamos al centro de paquetes y nos dirigimos a los que tenemos instalados y detenemos el controlador de nvidia una vez detenido lo volvemos a ejecutar.

<br>

![This is an image](imagenes/nvidia19.png)

<br>

![This is an image](imagenes/nvidia20.png)


