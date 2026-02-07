# Trabajo Docker – Aplicación To-Do List

## 1. Descripción del proyecto

Este proyecto consiste en desplegar una aplicación de gestión de tareas (To-Do List) utilizando **Docker Compose**.
El código de la aplicación ya venía proporcionado, por lo que el objetivo del trabajo ha sido crear la infraestructura necesaria para ejecutarla mediante contenedores.

La aplicación está formada por tres servicios:

* **Frontend**: archivos HTML, CSS y JavaScript servidos con Nginx.
* **Backend**: API REST desarrollada en Node.js.
* **Base de datos**: PostgreSQL para almacenar las tareas.

Cada servicio se ejecuta en un contenedor independiente y todos se comunican mediante una red interna de Docker.

---

## 2. Archivos añadidos

### docker-compose.yml

Es el archivo principal del proyecto.
Se encarga de:

* Construir las imágenes del frontend y del backend.
* Descargar la imagen oficial de PostgreSQL.
* Crear un volumen para que los datos de la base de datos no se pierdan.
* Crear una red interna para que los contenedores se comuniquen por nombre.
* Cargar las variables de entorno desde el archivo .env.

### frontend/Dockerfile

Define cómo se construye el contenedor del frontend:

* Utiliza la imagen de **nginx**.
* Copia los archivos estáticos del frontend a la carpeta que Nginx sirve por defecto.
* Expone el puerto 80 del contenedor.

### backend/Dockerfile

Define el contenedor del backend:

* Usa la imagen de **Node.js**.
* Instala las dependencias del proyecto.
* Copia el código del backend.
* Ejecuta el servidor con `npm start`.

### backend/init.sql

Script SQL que crea la tabla necesaria para la aplicación:

* Se ejecuta automáticamente la primera vez que se crea la base de datos.
* Garantiza que la aplicación funcione desde el primer arranque.

### .env

Contiene las variables de entorno con los datos de conexión a PostgreSQL.
Este archivo no se sube a Git por motivos de seguridad.

### .gitignore

Evita que se suban al repositorio:

* El archivo .env
* Dependencias locales
* Datos generados por Docker

---

## 3. Proceso de resolución

1. **Análisis del enunciado**
   Se identificó que no era necesario modificar el código de la aplicación, solo crear los contenedores y conectarlos correctamente para conseguir que la aplicación funcionase.

2. **Creación de Dockerfiles**
   Se crearon Dockerfile simples para frontend y backend.

3. **Configuración de Docker Compose**
   Se definieron los tres servicios, el volumen y la red personalizada.

4. **Uso de variables de entorno**
   La conexión del backend a PostgreSQL se configuró mediante un archivo .env.

5. **Inicialización de la base de datos**
   Se añadió el archivo init.sql para crear la tabla tasks sin tocar el código original.

---

## 4. Problemas encontrados y soluciones

### El backend no encontraba la base de datos

**Error:** `ENOTFOUND db`
**Solución:** hacer coincidir el nombre del servicio de PostgreSQL con el que usa el código (db).

---

### Fallo de autenticación

**Error:** `password authentication failed for user`
**Solución:** recrear el volumen con:
```
docker compose down -v
```
para que PostgreSQL se generara con las credenciales correctas.

---

### La tabla no existía

**Error:** `relation "tasks" does not exist`
**Solución:** añadir el script init.sql para crear la tabla automáticamente.

---

### Faltaba la columna “completed”

**Error:** `column "completed" does not exist`
**Solución:** actualizar el script SQL incluyendo:

* columna completed BOOLEAN
* valor por defecto false

---

## 5. Cómo ejecutar el proyecto

1. Crear el archivo .env con:

DB_USER=todouser
DB_PASSWORD=todopass
DB_NAME=tododb
DB_HOST=db
DB_PORT=5432

2. Ejecutar:
```
docker compose up -d --build
```
3. Abrir en el navegador:

[http://localhost:8080](http://localhost:8080)

---

## 6. Comandos útiles

Ver contenedores:
```
docker compose ps
```
Ver logs:
```
docker compose logs
```
Parar todo:
```
docker compose down
```
Borrar también los datos:
```
docker compose down -v
```
---

## 7. Estructura del proyecto

* docker-compose.yml
* frontend/

  * Dockerfile
  * archivos estáticos
* backend/

  * Dockerfile
  * init.sql
  * código del servidor
* .env
* README.md

---

## 8. Conclusiones

Con este trabajo he aprendido:

* Cómo desplegar una aplicación completa con Docker Compose.
* A comunicar contenedores mediante redes internas.
* A usar volúmenes para persistir datos.
* A inicializar una base de datos sin modificar el código original.
* A depurar errores reales de conexión entre servicios.

La verdad es que este trabajo me ha servido para entender cómo funciona Docker más allá de los ejemplos simples. Me he dado cuenta de que hay muchos detalles importantes: las redes internas, las variables de entorno, el orden de arranque de los servicios...

He tenido bastantes problemas durante el proceso, especialmente con la conexión entre el back y la base de datos, pero ir resolviendo todos los problemas leyendo los logs ha sido como un puzzle. Me ha ayudado a entender mejor qué estaba haciendo y por qué.

En general, me quedo con la sensación de haber aprendido bastante más de lo que esperaba y de haberle perdido el miedo a Docker, que al principio parecía tan intimidante.
