### Crear contenedor de Postgres sin que exponga los puertos. Usar la imagen: postgres:15-alpine3.21
```
C:\Users\ASUS TUF F15>docker run -d --name srv-postgres -e POSTGRES_PASSWORD=postgres123 postgres:15-alpine3.21
Unable to find image 'postgres:15-alpine3.21' locally
15-alpine3.21: Pulling from library/postgres
b0fbac26fcea: Pull complete
3e45eca4ff4a: Pull complete
ee415d75f298: Pull complete
8a294beff787: Pull complete
7313fa95eb89: Pull complete
66f87e0d43db: Pull complete
f637881d1138: Pull complete
c50fd9315cc2: Pull complete
8eb8ad54a0e1: Pull complete
8b4be2a0ba65: Pull complete
1dd8b1dbc901: Pull complete
Digest: sha256:267a966c6c554e0ac26264b9756697bf79a45aa7f0d3bd106550638f7dda04f1
Status: Downloaded newer image for postgres:15-alpine3.21
ab4911b0b002b394947bbfee2b5ce1147501fc21d3f7e83428d6e8f797e5eb70
```
### Crear un cliente de postgres. Usar la imagen: dpage/pgadmin4
```
C:\Users\ASUS TUF F15>docker run -d --name srv-pgadmin -p 8080:80 -e PGADMIN_DEFAULT_EMAIL=admin@admin.com -e PGADMIN_DEFAULT_PASSWORD=admin123 dpage/pgadmin4
Unable to find image 'dpage/pgadmin4:latest' locally
latest: Pulling from dpage/pgadmin4
e45ab8d926cc: Pull complete
dcc959905076: Pull complete
689d2bd3a211: Pull complete
0f9eea49a739: Pull complete
df70902a7a11: Pull complete
1f1caa93c083: Pull complete
dfbae50cdff7: Pull complete
34b139945354: Pull complete
0d80388e04f8: Pull complete
ab3060faa26a: Pull complete
09c161f799fc: Pull complete
03724893b2f4: Pull complete
868cc98db81b: Pull complete
43544896dddb: Pull complete
Digest: sha256:5d9624a93634d1c5e595619cc57b1d330758120d1baf445fa97300c0c1fc3c0a
Status: Downloaded newer image for dpage/pgadmin4:latest
f56f8dd3b93bdd34e6d3c268fe47c1a3e8f3cbd8fdf337273e8b265468966c10
```
La figura presenta el esquema creado en donde los puertos son:
- a: (completar con el valor)
- b: (completar con el valor)
- c: (completar con el valor)

![Imagen](esquema-2-ejercicio.PNG)

## Desde el cliente
### Acceder desde el cliente al servidor postgres creado.
<img width="1920" height="912" alt="imagen" src="https://github.com/user-attachments/assets/9e308887-45b5-4674-833f-5806f6620398" />

### Crear la base de datos info, y dentro de esa base la tabla personas, con id (serial) y nombre (varchar), agregar un par de registros en la tabla, obligatorio incluir su nombre.
```
C:\Users\ASUS TUF F15>docker exec -it srv-postgres psql -U postgres
psql (15.14)
Type "help" for help.
postgres=# CREATE DATABASE info;
CREATE DATABASE
postgres=# \c info
You are now connected to database "info" as user "postgres".
info=# CREATE TABLE personas (
info(#     id SERIAL PRIMARY KEY,
info(#     nombre VARCHAR(100)
info(# );
CREATE TABLE
info=# INSERT INTO personas (nombre) VALUES ('Dennis Morales');
INSERT 0 1
info=# INSERT INTO personas (nombre) VALUES ('Juan Pérez');
INSERT 0 1
info=# INSERT INTO personas (nombre) VALUES ('María García');
INSERT 0 1
info=# SELECT * FROM personas;
 id |     nombre
----+----------------
  1 | Dennis Morales
  2 | Juan Pérez
  3 | María García
(3 rows)

info=# \q
```
## Desde el servidor postgresl
### Acceder al servidor
```
C:\Users\ASUS TUF F15>docker exec -it srv-postgres psql -U postgres -d info
psql (15.14)
Type "help" for help.

info=#
```
# COMPLETAR
### Realizar un select *from personas
# AGREGAR UNA CAPTURA DE PANTALLA DEL RESULTADO
```
info=# SELECT * FROM personas;
 id |     nombre
----+----------------
  1 | Dennis Morales
  2 | Juan Pérez
  3 | María García
(3 rows)
```
