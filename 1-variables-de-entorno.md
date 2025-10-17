# Variables de Entorno
### ¿Qué son las variables de entorno?
```
Las variables de entorno son valores de configuración que se pasan a un contenedor en el momento de su creación. Funcionan como parámetros que modifican el comportamiento de las aplicaciones que corren dentro del contenedor, como usuarios, contraseñas, rutas de archivos o ajustes específicos del sistema. Estas variables permiten personalizar contenedores sin necesidad de modificar la imagen original.
```
### Para crear un contenedor con variables de entorno

```
docker run -d --name <nombre contenedor> -e <nombre variable1>=<valor1> -e <nombre variable2>=<valor2>
```

### Crear un contenedor a partir de la imagen de nginx:alpine con las siguientes variables de entorno: username y role. Para la variable de entorno rol asignar el valor admin.
```
C:\Users\ASUS TUF F15>docker run -d --name srv-nginx -e username=usuario -e role=admin nginx:alpine
30cd09bb00f24f75d7e754774e7c5b805fe316b54b2dc22a9a050c0bde860940
```
# CAPTURA CON LA COMPROBACIÓN DE LA CREACIÓN DE LAS VARIABLES DE ENTORNO DEL CONTENEDOR ANTERIOR
<img width="810" height="266" alt="imagen" src="https://github.com/user-attachments/assets/ac174f16-db75-4af5-b1a6-58ad1f4b4d76" />

### Crear un contenedor con la imagen de mysql, mapear todos los puertos
```
C:\Users\ASUS TUF F15>docker run -P -d --name srv-mysql mysql
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
930d9dafee77: Pull complete
806f49275cbf: Pull complete
e81ddb8dbdb4: Pull complete
d9301ae294fe: Pull complete
1620875f220d: Pull complete
872c71b4bef6: Pull complete
6457b1401253: Pull complete
619a14803e82: Pull complete
4c8e7ce47d40: Pull complete
e522a8b4b86f: Pull complete
Digest: sha256:91447968e66961302339ec4dc4d385f5e1a957d98e63c7d52ecf8b1de0907346
Status: Downloaded newer image for mysql:latest
bd31d54e8d50ed3a20889b33a45ddb55eb2c1bd41b8db0f6ee40ffa46c5ffbed
```
C:\Users\ASUS TUF F15>docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
30cd09bb00f2   nginx:alpine   "/docker-entrypoint.…"   17 minutes ago   Up 17 minutes   80/tcp    srv-nginx

### ¿El contenedor se está ejecutando?
```
No, el contenedor NO se está ejecutando. Se detuvo inmediatamente después de crearse 
```
### Identificar el problema
```
MySQL requiere configurar al menos una variable de entorno obligatoria para la contraseña del usuario root.
```
### Para crear un contenedor con variables de entorno especificadas
- Portabilidad: Las aplicaciones se vuelven más portátiles y pueden ser desplegadas en diferentes entornos (desarrollo, pruebas, producción) simplemente cambiando el archivo de variables de entorno.
- Centralización: Todas las configuraciones importantes se centralizan en un solo lugar, lo que facilita la gestión y auditoría de las configuraciones.
- Consistencia: Asegura que todos los miembros del equipo de desarrollo o los entornos de despliegue utilicen las mismas configuraciones.
- Evitar Exposición en el Código: Mantener variables sensibles como contraseñas, claves API, y tokens fuera del código fuente reduce el riesgo de exposición accidental a través del control de versiones.
- Control de Acceso: Los archivos de variables de entorno pueden ser gestionados con permisos específicos, limitando quién puede ver o modificar la configuración sensible.

### ¿Qué bases de datos existen en el contenedor creado?
```
C:\Users\ASUS TUF F15>docker rm srv-mysql
srv-mysql

C:\Users\ASUS TUF F15>docker run -P -d --name srv-mysql -e MYSQL_ROOT_PASSWORD=password123 mysql
226c5d5ec1504abaa9fc9b4cb7fa50abb06965ca3cda771989ca30cad07b6f94

C:\Users\ASUS TUF F15>docker exec -it srv-mysql mysql -uroot -ppassword123 -e "SHOW DATABASES;"
mysql: [Warning] Using a password on the command line interface can be insecure.
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```
