# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)

PS C:\Users\Italo>  docker run -d --name nginx-container -p 80:80 -v "$(pwd)/nginx/html:/usr/share/nginx/html" nginx:alpine

¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost:80 se mostrará la página "403 Forbidden" o "404 Not Found" porque la carpeta html en el host está vacía. Nginx no encuentra un archivo index.html para servir.

¿Qué pasa con el archivo index.html del contenedor?
El archivo index.html original del contenedor queda oculto por el volumen mapeado. Al mapear la carpeta vacía del host sobre /usr/share/nginx/html, se reemplaza completamente el contenido del contenedor, perdiendo la página por defecto de Nginx.

### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost:80 se mostrará el template HTML descargado funcionando completamente, con todos sus estilos CSS, imágenes y estructura. Nginx servirá automáticamente el archivo index.html del template y todos los recursos asociados (CSS, JS, imágenes) desde la carpeta mapeada.

### Eliminar el contenedor
PS C:\Users\Italo> docker rm -f nginx-container

### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
Al recrear el contenedor con el mismo mapeo de volumen, el template HTML se mantiene intacto y funcionando, porque los archivos están almacenados en la carpeta del host, no dentro del contenedor. El nuevo contenedor Nginx montará los mismos archivos y servirá el template inmediatamente sin necesidad de reconfigurar nada.


