# Docker
Docker es una plataforma de código abierto que permite a los desarrolladores automatizar el despliegue de aplicaciones dentro de contenedores de software.

## 1. Web Site 🌐
[https://www.docker.com](https://www.docker.com/)

---
<br>


## 2. Instalación ⚙️
- `apt-transport-https`: Permite la descarga de paquetes desde un repositorio HTTPS.
- `ca-certificates`: Certificados de autoridad.
- `wget`: Herramienta de descarga de archivos.
- `gnupg2`: Herramienta de cifrado.
- `software-properties-common`: Herramienta de gestión de paquetes.
```bash
apt update
apt install apt-transport-https ca-certificates wget gnupg2 software-properties-common

# Descargar la clave GPG de Docker y añadirla al sistema
wget -qO - https://download.docker.com/linux/debian/gpg | apt-key add -

# Añadir el repositorio de Docker (es lo mismo que añadirlo manualmente al archivo /etc/apt/sources.list)
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

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
docker contaner run IMAGE                   # Ejecuta un contenedor (docker run).
docker container exec -it ID /bin/bash      # Ejecuta un comando en un contenedor en ejecución (docker exec).
docker container stop ID                    # Detiene un contenedor en ejecución (docker stop).
docker container start ID                   # Inicia un contenedor detenido (docker start).
docker container restart ID                 # Reinicia un contenedor en ejecución (docker restart).
docker container attach ID                  # Conecta la terminal a un contenedor en ejecución (docker attach).
docker container pause ID                   # Pausa un contenedor en ejecución (docker pause).
docker container unpause ID                 # Reanuda un contenedor pausado (docker unpause).

# Imágenes
docker image build -t NAME .                # Construye una imagen de Docker (docker build).
docker image pull NAME                      # Descarga una imagen de Docker (docker pull).
docker image push NAME                      # Sube una imagen a Docker Hub (docker push).
docker image tag NAME NEW_NAME              # Cambia el nombre de una imagen (docker tag).
docker imgage load -i FILE                  # Carga una imagen desde un archivo (docker load).
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
COPY index.html .                         # Copia el archivo index.html del sistema anfitrión en el WORKDIR del contenedor.
EXPOSE 80                                 # Expone el puerto 80 del contenedor.
SHELL ["/bin/bash", "-c"]                 # Shell que se utilizará para ejecutar los comandos del Dockerfile.
VOLUME /var/www/html                      # /var/www/html es una ruta dentro del contenedor que persistirá los datos aunque el contenedor se elimine.
```
```bash
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
```bash
docker run -it --name "contenedor" -p 80:8080 -v /var/www/html:/var/www/html -e "VARIABLE=valor" --rm "imagen" /bin/bash
```
---
<br><br><br>

## *[volver al índice](../README.md)*