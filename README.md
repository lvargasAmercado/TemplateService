# GroundZero

Descripción general del proyecto:
El archivo Docker Compose proporcionado configura un entorno de varios contenedores para una aplicación web, que incluye un frontend, backend, base de datos, Nginx, PHPMyAdmin y Portainer. Esta plantilla puede servir como base para otros proyectos que requieran una arquitectura similar.

Configuración de los servicios:

Frontend: El servicio de frontend se construye desde el directorio groundzero-front, expuesto en el puerto 8080, y depende de los servicios backend y base de datos.
Backend: El servicio de backend se construye desde el directorio groundzero_Back, expuesto en el puerto 8000, y depende del servicio de base de datos.
Base de datos (db): Utiliza la imagen más reciente de MySQL, expone el puerto 3306 y persiste datos en el volumen mysql-data.
Nginx: Utiliza la imagen más reciente de Nginx, expone el puerto 9090 y depende del servicio de frontend.
PHPMyAdmin: Utiliza la imagen de phpMyAdmin, expuesto en el puerto 8081, y se conecta al servicio de base de datos.
Portainer: Utiliza la imagen de Portainer, expuesto en el puerto 9000 y gestiona contenedores Docker.

Consideraciones adicionales:

Los servicios de frontend y backend se construyen desde directorios específicos, y el servicio de base de datos utiliza una imagen de MySQL con variables de entorno definidas.
Nginx, PHPMyAdmin y Portainer también se incluyen para mejorar la experiencia de desarrollo y gestión.

Posibles casos de uso:
Esta plantilla de Docker Compose puede utilizarse como punto de partida para proyectos que requieran una arquitectura de microservicios similar, facilitando el desarrollo y despliegue de aplicaciones web con componentes separados de frontend, backend y base de datos.

Para una personalización adicional o casos de uso específicos, pueden ser necesarios ajustes y configuraciones adicionales basados en los requisitos del proyecto.




# README - Implementación de Sistema con Docker Compose

Este proyecto utiliza Docker Compose para desplegar un sistema con los siguientes servicios:

- **Frontend** (puerto 8081)
- **Backend** (puerto 8001)
- **Base de Datos MySQL** (puerto 3307)
- **Nginx** (puerto 9091)
- **phpMyAdmin** (puerto 8082)

## Requisitos Previos

Asegúrate de tener instalados los siguientes componentes en tu sistema:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Estructura del Proyecto

```bash
.
├── trazav2_front      # Código del frontend
├── trazav2_Back       # Código del backend
├── mysql-data         # Volumen para los datos de la base de datos MySQL
├── nginx/nginx.conf   # Archivo de configuración para Nginx
└── docker-compose.yml # Archivo Docker Compose
```

## Configuración del Sistema

El archivo `docker-compose.yml` está configurado para gestionar los servicios de frontend, backend, base de datos MySQL, Nginx y phpMyAdmin. A continuación se detallan las configuraciones y cómo levantar los servicios.

### Servicios

1. **Frontend**

   - Ubicación del código: `./trazav2_front`
   - Mapeo de puertos: `8081:8080`
   - Depende de: `backend`, `db`
2. **Backend**

   - Ubicación del código: `./trazav2_Back`
   - Mapeo de puertos: `8001:8000`
   - Depende de: `db`
3. **Base de Datos MySQL**

   - Imagen: `mysql:latest`
   - Mapeo de puertos: `3307:3306`
   - Volumen de datos: `./mysql-data:/var/lib/mysql`
   - Variables de entorno:
     - `MYSQL_DATABASE=traza_db`
     - `MYSQL_ROOT_PASSWORD=root`
   - Reinicio automático: `always`
4. **Nginx**

   - Imagen: `nginx:latest`
   - Mapeo de puertos: `9091:80`
   - Archivo de configuración: `./nginx/nginx.conf:/etc/nginx/nginx.conf:ro`
   - Depende de: `frontend`
5. **phpMyAdmin**

   - Imagen: `phpmyadmin/phpmyadmin`
   - Mapeo de puertos: `8082:80`
   - Variables de entorno:
     - `PMA_HOST=db`
     - `MYSQL_ROOT_PASSWORD=root`
   - Reinicio automático: `always`

## Instrucciones de Uso

### 1. Clonar el Repositorio

Clona el repositorio donde está ubicado tu código del frontend, backend, y el archivo `docker-compose.yml`.

```bash
git clone <URL_DEL_REPOSITORIO>
cd <DIRECTORIO_DEL_PROYECTO>
```

### 2. Construir y Levantar los Servicios

Para construir y levantar los contenedores, ejecuta el siguiente comando:

```bash
docker-compose up --build
```

Esto descargará las imágenes necesarias, construirá los servicios definidos y los levantará.

### 3. Acceso a los Servicios

- **Frontend**: [http://localhost:8081](http://localhost:8081)
- **Backend**: [http://localhost:8001](http://localhost:8001)
- **phpMyAdmin**: [http://localhost:8082](http://localhost:8082)
  - Nombre de host MySQL: `db`
  - Contraseña root: `root`

### 4. Detener los Servicios

Para detener y eliminar los contenedores creados, usa el siguiente comando:

```bash
docker-compose down
```

## Volúmenes de Datos

- Los datos de la base de datos MySQL se almacenan de manera persistente en el directorio `./mysql-data`. Si necesitas eliminar los datos por completo, asegúrate de eliminar este directorio.

## Configuración de Nginx

El archivo `nginx.conf` debe estar ubicado en la ruta `./nginx/nginx.conf`. Asegúrate de que esté correctamente configurado para manejar las solicitudes del frontend y backend. Puedes modificarlo según tus necesidades.

---

Si tienes algún problema, revisa los logs de los contenedores con el siguiente comando:

```bash
docker-compose logs
```
