## VOLUMEN NOMBRADO
Un volumen nombrado (named volume) es un tipo de volumen gestionado por Docker que se almacena en una ubicación específica del sistema de archivos del host y se identifica mediante un nombre único. Los volúmenes nombrados no requieren que especifiques una ruta del sistema de archivos del host, y en su lugar, Docker se encarga de la gestión y el almacenamiento del volumen.


### Crear volumen
```
docker volume create <nombre volumen>
```

### Crear el volumen nombrado: vol-postgres
docker volume create vol-postgres

## MOUNTPOINT
Un mountpoint se refiere al lugar en el sistema de archivos donde un dispositivo de almacenamiento se une (o monta) al sistema de archivos. Es el punto donde los archivos y directorios almacenados en ese dispositivo de almacenamiento son accesibles para el sistema operativo y las aplicaciones.

Por ejemplo, en Windows las unidades de almacenamiento (como `C:`, `D:`, etc.) actúan como puntos de montaje principales para discos duros, unidades flash, unidades ópticas y otros dispositivos de almacenamiento.

Cuando creas un volumen nombrado, Docker asigna un punto de montaje específico en el sistema de archivos del host para ese volumen.

### Estructura del Punto de Montaje:
- /var/lib/docker/volumes/: Es la ubicación base donde Docker almacena todos los volúmenes en el sistema de archivos del host.
- nombreVolumen/: Es el nombre del volumen nombrado que has creado. Docker crea un directorio con este nombre dentro de /var/lib/docker/volumes/ para almacenar los datos del volumen.
- _data: Es el subdirectorio dentro de vol-postgres/ donde se almacenan los datos reales del volumen. El nombre _data es una convención utilizada por Docker para indicar el directorio donde se encuentran los datos del volumen.

### ¿Cómo acceder a ese Mountpoint?
En el contexto de WSL (Windows Subsystem for Linux), wsl$ se refiere al nombre de un recurso compartido de red especial que representa la raíz del sistema de archivos de Windows desde WSL. Cuando accedes a \\wsl$ desde el Explorador de archivos de Windows, puedes ver y acceder a los archivos del sistema de archivos de la distribución de Linux en WSL.
\\wsl.localhost\docker-desktop-data\data\docker\volumes

### Crear un contenedor vinculado a un volumen nombrado
```
docker run -d --name <nombre contenedor> -v <nombre volumen>:<ruta contenedor> <nombre imagen>
```
ó
```
docker run -d --name <nombre contenedor> --mount type=volume,src=<nombre >,dst=<mount-path>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje. Para volúmenes con nombre, este es el nombre del volumen. Para volúmenes anónimos, este campo se omite.


### Crear la red net-drupal de tipo bridge
docker network create --driver bridge net-drupal

### Crear un servidor postgres vinculado a la red net-drupal, completar la ruta del contenedor
```
docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal --network net-drupal postgres
```
docker run -d --name server-postgres --network net-drupal -v drupal-database:/var/lib/postgresql/data -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal postgres:latest
_No es necesario exponer el puerto, debido a que nos vamos a conectar desde la misma red de docker_

### Crear un cliente postgres vinculado a la red drupal a partir de la imagen dpage/pgadmin4, completar el correo
```
docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=<correo> --network net-drupal dpage/pgadmin4
```
docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=admin@drupal.com --network net-drupal dpage/pgadmin4

### Usar el cliente postgres para conectarse al servidor postgres, para la conexión usar el nombre del servidor en lugar de la dirección IP.

### Crear los volúmenes necesarios para drupal, esto se puede encontrar en la documentación
docker volume create drupal-files
docker volume create drupal-database
docker volume create drupal-config
docker volume create drupal-sites


### Crear el contenedor server-drupal vinculado a la red, usar la imagen drupal, y vincularlo a los volúmenes nombrados
```
docker run -d --name server-drupal --publish published=9700,target=80 -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> --network net-drupal drupal
```
docker run -d --name server-drupal --network net-drupal -p 8080:80 -v drupal-files:/var/www/html/sites/default/files -v drupal-config:/var/www/html/sites/default -v drupal-sites:/var/www/html/sites -e DRUPAL_DATABASE_HOST=server-postgres -e RUPAL_DATABASE_NAME=db_drupal -e DRUPAL_DATABASE_USER=user_drupal -e DRUPAL_DATABASE_PASSWORD=12345 drupal:latest

### Ingrese al server-drupal y siga el paso a paso para la instalación.
# COMPLETAR CON UNA CAPTURA DE PANTALLA DEL PASO 4
<img width="809" height="869" alt="image" src="https://github.com/user-attachments/assets/97253e67-4d08-458c-8619-993fbfedf963" />


_La instalación puede tomar varios minutos, mientras espera realice un diagrama de los contenedores que ha creado en este apartado._

<img width="599" height="577" alt="image" src="https://github.com/user-attachments/assets/2082092c-1080-4fe5-9c96-76ddb08d686e" />


### Eliminar un volumen específico
```
docker volume rm <nombre volumen>
```
docker volume rm drupal-files
docker volume rm drupal-config
docker volume rm drupal-sites
docker volume rm drupal-database

**Considerar**
Datos Persistentes: Asegúrate de que el volumen no contiene datos críticos antes de eliminarlo, ya que esta operación no se puede deshacer.
Contenedores Activos: No puedes eliminar un volumen que está actualmente en uso por un contenedor activo. Debes detener y/o eliminar el contenedor primero.
