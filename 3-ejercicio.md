## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp

PS C:\Users\Italo> docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: .../ejercicio3/db

### ¿Qué contiene la carpeta db del host?
La carpeta db del host está vacía inicialmente.

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
PS C:\Users\Italo> docker run -d --name mysql --network net-wp -v "$(pwd)/ejercicio3/db:/var/lib/mysql" -e MYSQL_ROOT_PASSWORD=rootpassword123 -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=wppassword123 mysql:8

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
La carpeta db ahora contiene archivos de la base de datos MySQL (archivos .ibd, .frm, ibdata1, ib_logfile*, etc.) que se crearon automáticamente durante la inicialización de MySQL.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html

Ruta carpeta host: .../ejercicio3/www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)

PS C:\Users\Italo> docker run -d --name wordpress --network net-wp -p 80:80 -v "$(pwd)/ejercicio3/www:/var/www/html" -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=wppassword123 -e WORDPRESS_DB_NAME=wordpress wordpress:latest

### Personalizar la apariencia de wordpress y agregar una entrada

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?

Al eliminar y crearlo nuevamente el contenedor, el sitio WordPress se mantiene intacto con todas las personalizaciones y entradas; es decir, todo funciona inmediatamente sin necesidad de reconfigurar. Esto sucede porque los volúmenes mapeados almacenan toda la información en el host.

