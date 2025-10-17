# Reflexión y Aprendizajes

## Comparando mis conocimientos antes y después de las prácticas

### Antes de la práctica

Antes de realizar estos ejercicios, tenía una idea muy vaga de lo que era Docker. Sabía que existía y que servía para "algo relacionado con aplicaciones", pero no entendía realmente cómo funcionaba ni para qué me serviría en mi carrera profesional. No comprendía conceptos como contenedores, imágenes o redes, y mucho menos cómo todo esto se conectaba entre sí.

### Después de la práctica

Después de completar estos ejercicios, puedo decir que ahora entiendo Docker de una manera mucho más práctica y real. Los principales aprendizajes que obtuve fueron:

**1. Manejo de contenedores e imágenes**

Aprendí que las imágenes son como plantillas que sirven para crear contenedores, y que los contenedores son básicamente aplicaciones aisladas que corren en mi computadora sin afectar el resto del sistema. Ahora sé cómo descargar imágenes, crear contenedores, iniciarlos, detenerlos y eliminarlos cuando ya no los necesito.

**2. Mapeo de puertos**

Este fue uno de los conceptos que más me costó al principio, pero ahora lo entiendo claramente. El mapeo de puertos me permite acceder desde mi navegador a servicios que están corriendo dentro de un contenedor. Por ejemplo, cuando mapeé el puerto 9300 de mi computadora al puerto 80 del contenedor de WordPress, pude acceder al sitio desde mi navegador sin problemas.

**3. Variables de entorno**

Descubrí que las variables de entorno son fundamentales para configurar contenedores sin tener que entrar manualmente a modificar archivos. Cuando creé el contenedor de MySQL y WordPress, tuve que pasar variables como contraseñas y nombres de bases de datos. Esto me enseñó lo importante que es planificar estas configuraciones desde el inicio.

**4. Redes en Docker**

Este tema me pareció muy interesante. Aprendí que los contenedores no funcionan aislados del todo, sino que pueden comunicarse entre ellos a través de redes. Cuando conecté WordPress con MySQL usando la red net-wp, entendí cómo dos contenedores pueden "hablarse" entre sí sin necesidad de exponer puertos al exterior.

**5. Persistencia de datos**

El ejercicio de eliminar el contenedor de WordPress y volverlo a crear fue muy revelador. Me di cuenta de que los datos dentro de un contenedor son temporales si no se configuran volúmenes. Esto me hizo entender por qué es tan importante planificar bien dónde se van a guardar los datos importantes de una aplicación.

### Problemas que tuve y cómo los resolví

**Problema 1: Docker Desktop no iniciaba**

Al principio, cuando intentaba ejecutar comandos de Docker, me salía un error diciendo que no se podía conectar. El problema era que Docker Desktop no estaba corriendo. Lo resolví simplemente buscando Docker Desktop en el menú de Windows y esperando a que iniciara completamente antes de ejecutar comandos.

**Problema 2: Contenedor de MySQL no arrancaba**

Cuando intenté crear el contenedor de MySQL sin variables de entorno, el contenedor se detenía inmediatamente. Al revisar los logs con `docker logs`, me di cuenta de que necesitaba obligatoriamente configurar una contraseña. Esto me enseñó la importancia de leer los mensajes de error y los logs.

**Problema 3: No podía eliminar redes**

Cuando intenté eliminar las redes que había creado, Docker me daba error diciendo que había contenedores conectados. Aprendí que primero debo eliminar todos los contenedores que están usando esa red antes de poder eliminarla.

### Aplicación profesional

Estos conocimientos son muy valiosos para mi formación profesional porque:

- Ahora puedo montar ambientes de desarrollo sin necesidad de instalar todo directamente en mi computadora
- Entiendo cómo funcionan las aplicaciones modernas que usan contenedores
- Puedo trabajar en proyectos que requieran múltiples servicios (bases de datos, servidores web, etc.) de forma organizada
- Comprendo mejor cómo se despliegan aplicaciones en producción en empresas reales

---

## Gestión de datos confidenciales con Docker Secrets

### ¿Qué son los Docker Secrets?

Docker Secrets es un mecanismo diseñado para manejar información sensible de forma segura en Docker, especialmente cuando se trabaja con Docker Swarm (modo de orquestación de contenedores). Los secrets permiten almacenar datos confidenciales como contraseñas, tokens de API, claves SSH o certificados sin exponerlos en el código o en variables de entorno visibles.

### ¿Por qué son importantes?

Durante las prácticas, pasé contraseñas usando variables de entorno con el flag `-e`. Esto funciona bien para pruebas locales, pero tiene un problema de seguridad: cualquier persona que ejecute `docker inspect` en el contenedor puede ver esas variables de entorno en texto plano. En un ambiente de producción, esto es un riesgo enorme.

### ¿Cómo funcionan los Docker Secrets?

Los secrets funcionan de la siguiente manera:

1. **Se crean de forma segura:** Los datos confidenciales se almacenan de forma encriptada en el sistema de Docker Swarm.

2. **Solo se montan cuando se necesitan:** El secret se monta como un archivo temporal en `/run/secrets/` dentro del contenedor, nunca como variable de entorno visible.

3. **Acceso controlado:** Solo los servicios que explícitamente tienen permiso pueden acceder a un secret específico.

4. **Encriptación:** Los secrets se transmiten de forma encriptada entre los nodos del swarm y se almacenan encriptados en el disco.

### Ejemplo práctico

Si quisiera usar secrets en lugar de variables de entorno para el ejercicio de WordPress y MySQL, haría algo así:

**Crear el secret:**
```bash
echo "micontraseñasegura" | docker secret create mysql_password -
```

**Usar el secret en un servicio:**
```bash
docker service create \
  --name mysql \
  --secret mysql_password \
  -e MYSQL_ROOT_PASSWORD_FILE=/run/secrets/mysql_password \
  mysql:8
```

En este caso, MySQL lee la contraseña desde el archivo `/run/secrets/mysql_password` en lugar de una variable de entorno, lo cual es mucho más seguro.

### Diferencia con variables de entorno

| Variables de Entorno | Docker Secrets |
|---------------------|----------------|
| Visibles con `docker inspect` | No visibles en inspect |
| Se pasan en texto plano | Encriptados en tránsito y almacenamiento |
| Fáciles de usar en desarrollo | Requieren Docker Swarm |
| Menos seguras | Más seguras |
| Para cualquier contenedor | Solo para servicios en Swarm |

### Limitaciones

Los Docker Secrets tienen una limitación importante: **solo funcionan en modo Docker Swarm**. En los ejercicios que hice, usé Docker en modo standalone (un solo host), por lo que no pude usar secrets. Para ambientes de desarrollo local, las variables de entorno son suficientes, pero en producción con múltiples servidores, los secrets son la forma correcta de manejar información confidencial.

### Conclusión sobre secrets

Aprender sobre Docker Secrets me hizo entender que la seguridad no es algo que se agrega después, sino que debe planificarse desde el inicio. En mi carrera profesional, saber manejar datos confidenciales de forma correcta será fundamental para trabajar en proyectos reales donde la seguridad de la información es crítica.
