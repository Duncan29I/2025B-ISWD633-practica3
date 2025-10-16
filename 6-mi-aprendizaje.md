# COMPLETAR  
Comparando sus conocimientos antes de hacer la práctica con sus conocimientos después de hacer la tarea, explicar los principales aprendizajes logrados para beneficio de su formación profesional:

Antes de esta práctica completa, comprendía los conceptos básicos de Docker de forma aislada, pero carecía de experiencia en la orquestación de aplicaciones multi-contenedor. Sabía crear contenedores individuales pero no había diseñado arquitecturas complejas con redes personalizadas, volúmenes persistentes y comunicaciones entre servicios. Mi conocimiento de Drupal y PostgreSQL se limitaba a instalaciones tradicionales, sin experiencia en su containerización integrada.

Aprendí a crear y gestionar redes Docker personalizadas (net-drupal) que permiten la comunicación segura entre contenedores usando nombres de servicio como DNS. Comprobé cómo server-drupal puede conectarse a server-postgres usando simplemente el nombre del contenedor, eliminando la complejidad de manejar direcciones IP. Esto es fundamental para arquitecturas de microservicios en entornos de producción.

Si solucionó un problema presentado o utilizó otros comandos que no se mencionan al realizar la práctica también se debe documentar.
## Problema de Requisitos en Drupal: El error indica que falta el directorio de traducciones.
<img width="782" height="728" alt="image" src="https://github.com/user-attachments/assets/42140c75-30d7-471c-9ca0-9ffd585408b3" />

## Solución:
1. Crear el directorio faltante
   # Crear el directorio de traducciones en el volumen
    docker exec server-drupal mkdir -p /var/www/html/sites/default/files/translations
2. Corregir permisos:
   # Dar permisos correctos al directorio files
    docker exec server-drupal chown -R www-data:www-data /var/www/html/sites/default/files
    docker exec server-drupal chmod -R 755 /var/www/html/sites/default/files
3. Verificar que se crearon los directorios:
   # Verificar estructura de directorios
docker exec server-drupal ls -la /var/www/html/sites/default/files/

4. Volver a la instalación.
