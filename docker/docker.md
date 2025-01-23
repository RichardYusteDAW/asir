# Docker
Docker es una plataforma de código abierto que permite a los desarrolladores automatizar el despliegue de aplicaciones dentro de contenedores de software.

## 1. Web Site 🌐
[https://www.docker.com](https://www.docker.com/)

---
<br>


## 2. Instalación ⚙️
- `apt-transport-https`: Habilita la descarga segura de paquetes usando HTTPS en el gestor de paquetes apt..
- `ca-certificates`:  Lista de certificados de autoridades de certificación confiables.
- `wget`: Herramienta de descarga de archivos desde la web.
- `gnupg2`: Utilidad de cifrado para la seguridad de las comunicaciones y verificación de paquetes.
- `software-properties-common`: Herramienta de gestión de repositorios de software (añadir, eliminar, etc.).
```bash
apt update
apt install apt-transport-https ca-certificates wget gnupg2 software-properties-common

# Descargar la clave GPG de Docker y añadirla al sistema
wget -qO - https://download.docker.com/linux/debian/gpg | apt-key add -

# Añadir el repositorio de Docker (es lo mismo que añadirlo manualmente al archivo /etc/apt/sources.list)
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

# Volvemos a actualizar la lista de paquetes ya que hemos añadido un nuevo repositorio
apt update

apt install docker-ce
```
---
<br>


## 3. Comandos de Servicio 🛠️
```bash
systemctl start docker.service              # Inicia el servicio de Docker.
systemctl stop docker.service               # Detiene el servicio de Docker.
systemctl restart docker.service            # Reinicia el servicio de Docker.
systemctl status docker.service             # Muestra el estado del servicio de Docker.
```
---
<br>


## 4. Comandos de Docker 🐳
```bash
# Información
docker --version                            # Muestra la versión de Docker.
docker info                                 # Muestra información sobre Docker.

# Ver
docker image ls                             # Muestra las imágenes de Docker (docker images).
docker container ls                         # Muestra los contenedores en ejecución (docker ps).
docker container ls -a                      # Muestra todos los contenedores (docker ps -a).
docker container logs ID                    # Muestra los logs de un contenedor (docker logs).
docker container inspect ID                 # Muestra información detallada de un contenedor (docker inspect).

# Eliminar
docker image rm ID                          # Elimina una imagen (docker rmi).
docker image prune                          # Elimina todas las imágenes no utilizadas (docker rmi $(docker images -q)).
docker container rm ID                      # Elimina un contenedor (docker rm).
docker container prune                      # Elimina todos los contenedores detenidos (docker rm $(docker ps -aq)).

# Ejecución
docker container run IMAGE                  # Ejecuta un contenedor (docker run).
docker container exec -it ID /bin/bash      # Ejecuta un comando en un contenedor en ejecución (docker exec).
docker container stop ID                    # Detiene un contenedor en ejecución (docker stop).
docker container start ID                   # Inicia un contenedor detenido (docker start).
docker container restart ID                 # Reinicia un contenedor en ejecución (docker restart).
docker container attach ID                  # Conecta la terminal a un contenedor en ejecución (docker attach).
docker container pause ID                   # Pausa un contenedor en ejecución (docker pause).
docker container unpause ID                 # Reanuda un contenedor pausado (docker unpause).
docker exec -it ID /bin/bash                # Ejecuta un comando en un contenedor en ejecución (docker exec).

# Imágenes
docker image build -t NAME .                # Construye una imagen de Docker (docker build).
docker image pull NAME                      # Descarga una imagen de Docker (docker pull).
docker image push NAME                      # Sube una imagen a Docker Hub (docker push).
docker image tag NAME NEW_NAME              # Cambia el nombre de una imagen (docker tag).
docker image load -i FILE                   # Carga una imagen desde un archivo (docker load).
docker image save -o FILE NAME              # Guarda una imagen en un archivo (docker save).
```
---
<br>


## 5. Crear una imagen 🖼️
### 5.1. A partir de un Dockerfile 
```bash
nano Dockerfile
```
```Dockerfile
FROM debian:10                            # Utiliza la imagen de Debian 10 para construir la nueva imagen.
WORKDIR /var/www/html                     # Directorio de trabajo dentro del contenedor.
RUN apt update && apt install -y apache2  # Actualiza la lista de paquetes e instala Apache al construir la imagen.

CMD ["apache2ctl", "-D", "FOREGROUND"]    # Ejecuta Apache al iniciar el contenedor.
COPY index.html .                         # Copia el archivo index.html desde el directorio de construcción al WORKDIR del contenedor.
EXPOSE 80                                 # Expone el puerto 80 del contenedor.
SHELL ["/bin/bash", "-c"]                 # Shell que se utilizará para ejecutar los comandos del Dockerfile.
VOLUME /var/www/html                      # /var/www/html es una ruta dentro del contenedor que persistirá los datos aunque el contenedor se elimine.
```
```bash
# -t: Tag de la imagen.
# -f: ruta del Dockerfile si no está en la ubicación por defecto.
# .: ruta del contexto de construcción.
docker build -t IMAGEN:1.0 .
```
- `ARG`: Argumento.
- `CMD`: Comando a ejecutar al iniciar el contenedor.
- `COPY`: Copia archivos del sistema anfitrión al contenedor.
- `ENTRYPOINT`: Comando a ejecutar al iniciar el contenedor. Si se combina con CMD, CMD se convierte en los argumentos de ENTRYPOINT.
- `ENV`: Variables de entorno.
- `EXPOSE`: Puerto a exponer.
- `FROM`: Imagen que se utilizará como base para construir la nueva imagen.
- `HEALTHCHECK`: Comprobación de salud.
- `LABEL`: Etiquetapara identificar la imagen.
- `MAINTAINER`: Mantenedor es la persona que mantiene la imagen.
- `ONBUILD`: Acción a realizar cuando se use la imagen como base.
- `RUN`: Comando a ejecutar al construir la imagen.
- `SHELL`: Shell que se utilizará.
- `STOPSIGNAL`: Señal de parada.
- `USER`: Usuario que se utilizará dentro del contenedor.
- `VOLUME`: Volumen que se crea fuera del contenedor para persistir los datos aunque el contenedor se elimine.
- `WORKDIR`: Directorio de trabajo dentro del contenedor.
<br><br>

### 5.2. A partir de un contenedor
```bash
docker commit -a "autor" -m "comentario" CONTENEDOR IMAGEN
```
---
<br>


## 6. Ejecutar un contenedor 🚀
- `it`: Modo interactivo + TTY (ocupa la terminal).
- `d`: Modo demonio (no ocupa la terminal).
- `name`: Nombre del contenedor.
- `p`: Mapeo de puertos(host:contenedor).
- `v`: Mapeo de volúmenes(host:contenedor).
- `e`: Variables de entorno.
- `rm`: Elimina el contenedor al detenerlo.
- `w`: Directorio de trabajo.
- `pull=never`: No descarga la imagen si no la tiene.
- `restart=always`: Reinicia el contenedor al iniciar el sistema.
```bash
docker run -it --name "contenedor" -p 80:8080 -v /var/www/html:/var/www/html -e "VARIABLE=valor" -w "/var/www/html" --rm "imagen" /bin/bash
```
---
<br>


## 7. Docker Compose 🐳
```yml
services:
  web:    # ----------------------------------------------- Nombre del servicio que queramos (contenedor).
    image: httpd   Si utilizamos la opción "build" no utilizaremos la opción "image".
    pull_policy: never    # ------------------------------- No descarga la imagen si no la tiene.
    build:    # ------------------------------------------- Construye la imagen.
      context: /rutaDelContextoDeConstruccion
      dockerfile: /rutaDelDockerfile
      args:    # ------------------------------------------ Argumentos (Si hay un argumento ARGS en el Dockerfile lo machaca.)
        ARGUMENTO: "Valor del argumento"  
    container_name: nombre_del_contendor_que_queramos
    hostname: nombre_del_host
    environment:    # ------------------------------------- Variables de entorno.
      VARIABLEDEENTORNO: "Valor de la variable de entorno" 
    ports:    # ------------------------------------------- Puertos (host:contenedor).
      - "8080:80"                      
    volumes:    # ----------------------------------------- Volúmenes (host:contenedor).
      - "./html/:/usr/share/html"           
    command: ["echo", "Hola"]    # ------------------------ Igual al CMD de Dockerfile.
    restart: always    # ---------------------------------- "no" por defecto; "on-failure" si algo falla; "unless-stopped" solo si se detiene. 
    tty: true    # ---------------------------------------- Modo interactivo (TTY).
    stdin_open: true   # ---------------------------------- Modo interactivo (STDIN).
    networks:
      NOMBREDELARED:                                       
        aliases:
          - web    # -------------------------------------- Alias de la red (Esto simula un DNS pero debemos configurar las redes (networks)).
    depends_on:
      - mongodb
      - bitcoind

# Sin esta configuración no funcionará "aliases":
networks:
  NOMBREDELARED:
    name: web_net          # Esta red aparecerá con el comando "docker network ls".
    driver: bridge         # Nos dará una IP de nuestra red.
    ipam:                  # IP Network Manager
      driver: default      # Las IP nos las proporcionará Docker automaticamente.
```

```bash
# Información
docker-compose version     # Muestra la versión de Docker Compose.
docker-compose config      # Valida y muestra la configuración del archivo docker-compose.yml.
docker-compose convert     # Convierte el archivo docker-compose a formato canónico de la plataforma.

# Ejecución
docker-compose up          # Inicia los servicios definidos en docker-compose.yml (-d para segundo plano, --build cada vez que se actualiza el Dockerfile).
docker-compose down        # Detiene y elimina los contenedores, además de las redes y volúmenes asociados.
docker-compose start       # Arranca servicios detenidos.
docker-compose stop        # Detiene servicios sin eliminar los contenedores.
docker-compose restart     # Reinicia todos los servicios.
docker-compose pause       # Pausa los servicios sin detenerlos completamente.
docker-compose unpause     # Reanuda los servicios pausados.
docker-compose kill        # Fuerza la detención de los servicios.
docker-compose create      # Crea contenedores para los servicios sin iniciarlos.
docker-compose rm          # Elimina contenedores detenidos.
docker-compose run         # Ejecuta un comando puntual en un servicio.
docker-compose exec        # Ejecuta un comando en un contenedor en ejecución.
docker-compose cp          # Copia archivos/folders entre un contenedor y el sistema local.

# Imágenes
docker-compose images      # Lista las imágenes usadas por los contenedores.
docker-compose build       # Reconstruye las imágenes especificadas en el archivo docker-compose.yml.
docker-compose pull        # Descarga las imágenes definidas en el archivo docker-compose.yml.
docker-compose push        # Sube las imágenes a un registro de Docker.

# Diagnóstico y Monitoreo
docker-compose ps          # Muestra los contenedores en ejecución basados en el archivo docker-compose.yml.
docker-compose top         # Muestra los procesos en ejecución en los contenedores.
docker-compose events      # Recibe eventos en tiempo real de los contenedores.
docker-compose logs        # Muestra los logs de los contenedores.
docker-compose port        # Muestra los puertos mapeados de un contenedor (opcional: especifica "--protocol").
docker-compose ls          # Lista proyectos de Docker Compose en ejecución.
```
<br><br><br>

## *[volver al índice](../README.md)*