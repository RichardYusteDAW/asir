# Certbot

## ¿Qué es Certbot? 🤖🔐
Certbot es una herramienta de software libre que permite automatizar la obtención y renovación de certificados SSL/TLS de Let's Encrypt, facilitando la configuración de HTTPS en servidores web.

---
<br>


## Instalación 🛠️
- Para instalar Certbot en un contenedor Docker, puedes utilizar la imagen oficial de Certbot:
  ```bash
  docker pull certbot/certbot:latest
  ```

- Comprobar que la instalación se ha realizado correctamente ejecutando:
  ```bash
  docker run -it --rm certbot/certbot:latest --version
  ```
---
<br>


## Creación de directorios para Certbot 📁
- Creamos un directorio para Certbot:
  ```bash
  mkdir -p ~/certbot
  ```

- Creamos los directorios necesarios para almacenar los certificados (en `~/compiler/_certs`):
  - `letsencrypt`: Almacena los certificados SSL, las claves privadas y la configuración de renovación de Certbot.
  - `certbot-www`: Almacena temporalmente los archivos utilizados para verificar la propiedad del dominio mediante HTTP.
  - `certbot-lib`: Almacena datos internos y archivos de trabajo de Certbot.
  
   ```bash
   mkdir -p ~/compiler/_certs/{letsencrypt,certbot-www,certbot-lib}
   ```

  ```bash
  ~/
  │
  ├── certbot/                           # Scripts Docker de Certbot
  │   ├── run.sh                         # Solicitar certificados SSL
  │   └── renew.sh                       # Renovar certificados SSL
  │
  └── compiler/
      └── _certs/
          │
          ├── letsencrypt/               # Certificados SSL y configuración de Certbot
          │   ├── accounts/              # Datos de la cuenta de Let's Encrypt
          │   ├── archive/               # Versiones históricas de los certificados
          │   │   └── wallacoin.io/
          │   │       ├── cert1.pem      # Certificado del dominio
          │   │       ├── chain1.pem     # Certificados intermedios
          │   │       ├── fullchain1.pem # Certificado + cadena de confianza
          │   │       └── privkey1.pem   # Clave privada
          │   │
          │   ├── live/                  # Enlaces simbólicos a los certificados vigentes
          │   │   └── wallacoin.io/
          │   │       ├── cert.pem       # Certificado del dominio
          │   │       ├── chain.pem      # Certificados intermedios
          │   │       ├── fullchain.pem  # Certificado completo utilizado por Nginx y Node.js
          │   │       └── privkey.pem    # Clave privada utilizada por Nginx y Node.js
          │   │
          │   └── renewal/               # Configuración de renovación
          │       └── wallacoin.io.conf
          │
          ├── certbot-www/               # Verificación de propiedad del dominio
          │   └── .well-known/
          │       └── acme-challenge/    # Archivos temporales de validación HTTP
          │
          └── certbot-lib/               # Datos internos de trabajo de Certbot
  ```
---
<br>

## Creación de scripts de Certbot 📜
- Creamos el script `run.sh` para ejecutar Certbot:
  ```bash
  nano ~/certbot/run.sh
  chmod +x ~/certbot/run.sh
  ```

  ```bash
  #!/bin/bash

  set -e  # Exit if it fails
  
  BASE_DIR="$(cd "$(dirname "$0")/.." && pwd)"
  CONTAINER_NAME=certbot
  DOMAIN=wallacoin.io
  DOMAIN_WWW=www.wallacoin.io
  EMAIL=info@wallacoin.io
  
  echo "🔐 Requesting SSL certificate for $DOMAIN and $DOMAIN_WWW..."
  
  docker run --rm \
    --name "$CONTAINER_NAME" \
    -v "$BASE_DIR/compiler/_certs/letsencrypt":/etc/letsencrypt \
    -v "$BASE_DIR/compiler/_certs/certbot-www":/var/www/certbot \
    -v "$BASE_DIR/compiler/_certs/certbot-lib":/var/lib/letsencrypt \
    certbot/certbot:latest certonly \
    --webroot \
    -w /var/www/certbot \
    --cert-name "$DOMAIN" \
    -d "$DOMAIN" \
    -d "$DOMAIN_WWW" \
    --email "$EMAIL" \
    --agree-tos \
    --non-interactive
  
  echo "✅ Certbot finished successfully"
  ```

- Creamos el script `renew.sh` para comprobar y renovar los certificados SSL cuando sea necesario:
  ```bash
  nano ~/certbot/renew.sh
  chmod +x ~/certbot/renew.sh
  ```

  ```bash
  #!/bin/bash

  set -e  # Exit if it fails
  
  BASE_DIR="$(cd "$(dirname "$0")/.." && pwd)"
  CONTAINER_NAME=certbot
  DOMAIN=wallacoin.io
  
  CERT_PATH="$BASE_DIR/compiler/_certs/letsencrypt/live/$DOMAIN/fullchain.pem"
  
  # Read current certificate fingerprint
  BEFORE=""
  if [ -f "$CERT_PATH" ]; then
    BEFORE=$(openssl x509 -in "$CERT_PATH" -noout -fingerprint -sha256)
  fi
  
  echo "🔄 Checking SSL certificate renewal..."
  
  docker run --rm \
    --name "$CONTAINER_NAME" \
    -v "$BASE_DIR/compiler/_certs/letsencrypt":/etc/letsencrypt \
    -v "$BASE_DIR/compiler/_certs/certbot-www":/var/www/certbot \
    -v "$BASE_DIR/compiler/_certs/certbot-lib":/var/lib/letsencrypt \
    certbot/certbot:latest renew
  
  # Read certificate fingerprint after renewal
  AFTER=$(openssl x509 -in "$CERT_PATH" -noout -fingerprint -sha256)
  
  if [ "$BEFORE" != "$AFTER" ]; then
    echo "🔐 Certificate renewed. Restarting services..."
  
    docker restart wallacoin-backend
    docker restart wallacoin-frontend
  
    echo "✅ Services restarted successfully"
  else
    echo "✅ No renewal needed"
  fi
  ```
---
<br>


## Creación de un contenedor de prueba 🧪
- Creamos una configuración temporal de Nginx para probar los certificados SSL:
  ```bash
  cat > ~/compiler/frontend/nginx-certbot.conf <<'EOF'
  server {
      listen 80;
      server_name wallacoin.io www.wallacoin.io;

      # Validación de dominios mediante Let's Encrypt
      location ^~ /.well-known/acme-challenge/ {
          root /var/www/certbot;
          default_type text/plain;
          try_files $uri =404;
      }

      # Redirigir el resto de peticiones a HTTPS
      location / {
          return 301 https://$host$request_uri;
      }
  }
  EOF
  ```
- Arrancamos un contenedor temporal de Nginx para probar la configuración:
  ```bash
  docker run -d \
    --name subastaspro-certbot-http \
    -p 80:80 \
    -v "$HOME/compiler/frontend/nginx-certbot.conf":/etc/nginx/conf.d/default.conf:ro \
    -v "$HOME/compiler/_certs/certbot-www":/var/www/certbot:ro \
    nginx:stable-alpine
  ```
---
<br>


## Ejecución de los scripts 🚀
- Para ejecutar Certbot y obtener los certificados SSL:
  ```bash
  bash ~/certbot/run.sh
  ```

- Para comprobar y renovar los certificados SSL cuando sea necesario:
  ```bash
  bash ~/certbot/renew.sh
  ```
---
<br>


## Creación de tarea programada ⏰
- Creamos una tarea programada (cron job) para ejecutar el script de renovación automáticamente:
  ```bash
  crontab -e
  ```
- Añadimos la siguiente línea al final del archivo para ejecutar `renew.sh` todos los días a las 3:00 AM:
  ```bash
  0 3 * * * /bin/bash /home/ubuntu/certbot/renew.sh >> /home/ubuntu/certbot/renew.log 2>&1
  ```
- Comprobamos que la tarea programada se ha añadido correctamente:
  ```bash
  crontab -l
  ```
---
<br>


## Comprobación de renovación de certificados SSL ✅
- Ejecutamos el siguiente comando para simular la renovación de los certificados SSL y comprobar que todo funciona correctamente:
  ```bash
  docker run --rm \
  -v "$HOME/compiler/_certs/letsencrypt":/etc/letsencrypt \
  -v "$HOME/compiler/_certs/certbot-www":/var/www/certbot \
  -v "$HOME/compiler/_certs/certbot-lib":/var/lib/letsencrypt \
  certbot/certbot:latest renew --dry-run
  ```
---
<br><br><br>

## *[volver al índice](../README.md)*