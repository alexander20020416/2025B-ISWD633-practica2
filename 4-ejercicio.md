## Esquema para el ejercicio
![Imagen](esquema-4-ejercicio.PNG)

### Crear la red
```
C:\Users\ASUS TUF F15>docker network create net-wp -d bridge
5f2db5c1b77678deaa1403f224ae390e69babae72f460dacf440e569df9182c5
```
### Crear el contenedor mysql a partir de la imagen mysql:8, configurar las variables de entorno necesarias
```
C:\Users\ASUS TUF F15>docker run -d --name server-mysql --network net-wp -e MYSQL_ROOT_PASSWORD=secretpass -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wpuser -e MYSQL_PASSWORD=wppass mysql:8
Unable to find image 'mysql:8' locally
8: Pulling from library/mysql
46d5b31c795a: Pull complete
970f697f30e8: Pull complete
a49b4bec6f69: Pull complete
8389b884d3d6: Pull complete
d3dc946e9b73: Pull complete
f56ec30544fc: Pull complete
7a99a8dca35c: Pull complete
69718387e824: Pull complete
35a745903ff9: Pull complete
Digest: sha256:5367102acfefeaa47eb0eb57c8d4f8b96c8c14004859131eac9bbfaa62f81e34
Status: Downloaded newer image for mysql:8
8433138cf421d28071e877bbf24eb1c65967f2b7cc107498e47aa6b2c442704e
```
### Crear el contenedor wordpress a partir de la imagen: wordpress, configurar las variables de entorno necesarias
```
C:\Users\ASUS TUF F15>docker run -d --name server-wordpress --network net-wp -p 9300:80 -e WORDPRESS_DB_HOST=server-mysql -e WORDPRESS_DB_USER=wpuser -e WORDPRESS_DB_PASSWORD=wppass -e WORDPRESS_DB_NAME=wordpress wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
1dc412ee8328: Pull complete
8c7716127147: Pull complete
aff9b4e3d718: Pull complete
439737a78ff3: Pull complete
24403a1f6855: Pull complete
14ca90809f9f: Pull complete
e1cf44d6017a: Pull complete
004f06ab2f6c: Pull complete
a6a46d84ee78: Pull complete
2489d5e860a7: Pull complete
ad1401f3880c: Pull complete
1ad617015078: Pull complete
8d83c968ca9a: Pull complete
6571cfdbe5b2: Pull complete
4f4fb700ef54: Pull complete
089dfe755d3d: Pull complete
a139c2f3234a: Pull complete
dd53cf9bf4cf: Pull complete
4eed3454c20c: Pull complete
00ef78e422f0: Pull complete
0248257cbd51: Pull complete
fdfd46fc530c: Pull complete
749b92ea0995: Pull complete
fddb92e888a7: Pull complete
Digest: sha256:d34b44296d24686bf1a9c70c435b1640202e8e97d824aad9997fc63a61ac5cbb
Status: Downloaded newer image for wordpress:latest
2bd5b570d1df2223d916b882872f4fe332867613058cf0afb6179b5229a94dbe
```
De acuerdo con el trabajo realizado, en el esquema del ejercicio el puerto a es **(completar con el valor)**

Ingresar desde el navegador al wordpress y finalizar la configuración de instalación.
# COLOCAR UNA CAPTURA DE LA CONFIGURACIÓN
<img width="1920" height="912" alt="imagen" src="https://github.com/user-attachments/assets/4bd081da-e8ef-4d40-835e-52b54df3a030" />

Desde el panel de admin: cambiar el tema y crear una nueva publicación.
Ingresar a: http://localhost:9300/ 
recordar que a es el puerto que usó para el mapeo con wordpress
# COLOCAR UNA CAPTURA DEL SITO EN DONDE SEA VISIBLE LA PUBLICACIÓN.
<img width="1920" height="912" alt="imagen" src="https://github.com/user-attachments/assets/78078c8f-30ca-4cfa-a7b4-495accdb5119" />

### Eliminar el contenedor wordpress
```
C:\Users\ASUS TUF F15>docker rm -f server-wordpress
server-wordpress
```
### Crear nuevamente el contenedor wordpress
Ingresar a: http://localhost:9300/ 
recordar que a es el puerto que usó para el mapeo con wordpress

### ¿Qué ha sucedido, qué puede observar?
```
Ah perfecto Dennis, entonces NO se perdió todo. Lo que observas es diferente a lo que esperaba.
¿Qué ha sucedido, qué puede observar?
Al acceder nuevamente a http://localhost:9300/ después de eliminar y recrear el contenedor de WordPress, se puede observar que el sitio sigue funcionando y mantiene la configuración básica (título "Blog de Dennis" y la estructura).
Sin embargo, la publicación personalizada que se creó ("Mi primera publicación - Dennis Morales") ha desaparecido y solo aparece la publicación por defecto de WordPress "¡Hola mundo!".
```
