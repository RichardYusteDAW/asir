# Ngrok

## 1. ¿Qué es ngrok?
Ngrok es una herramienta que permite exponer un servidor web local a internet.  
Es muy útil para probar aplicaciones web en desarrollo en dispositivos móviles, compartir un sitio web en desarrollo con un cliente, entre otros.

---
<br>

## 2. Instalación
- Registrarse en: [https://ngrok.com](https://ngrok.com)

- Generar un token de autenticación:
  - Identify & Access -> Authtokens -> Copiar el token generado.

- Descargar el archivo **zip** de la página de descargas.

- Descomprimir el archivo zip.

- Ejecutar el archivo **ngrok.exe**. (Es un ejecutabe sin dependencias, por lo que no requiere instalación).

- Añadir token al archivo yml de configuración:
`ngrok config add-authtoken 2k73hQIiBpaoYV8rz5AD1b0bnSW_3pZoBh1H6BGWTwPm7yXc4`

- Arrancar el servicio:
`ngrok http http://localhost:8080`

---
<br>

## 3. Creación de un archivo de configuración
```bash
# Configuración por defecto
nano ~//AppData/Local/ngrok/ngrok.yml

# Configuración personalizada
nano ~/.ngrok2/ngrok.yml
```
```yml
version: "2"
authtoken: 2k73hQIiBpaoYV8rz5AD1b0bnSW_3pZoBh1H6BGWTwPm7yXc4
tunnels:
  angular:
    addr: 4200
    proto: http
  node:
    addr: 8080
    proto: http
```
```bash
# Ejecutar ngrok con la configuración por defecto
ngrok start --all

# Ejectuar ngrok con la configuración personalizada
ngrok start --config ~/.ngrok2/ngrok.yml --all

# Ejectuar solo los túneles de angular y node
ngrok --config "C:\Users\Student\.ngrok2\ngrok.yml" start angular node
```
---
<br>

## 4. Evitar la página de advertencia de ngrok con Angular 19
```bash
# Creamos un interceptor para modificar la respuesta de ngrok
ng generate interceptor ngrok
```
```typescript
// src/app/interceptors/ngrok.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';

export const ngrokInterceptor: HttpInterceptorFn = (req, next) => {
  const clonedReq = req.clone({
    setHeaders: {
      'ngrok-skip-browser-warning': 'true'
    }
  });
  return next(clonedReq); // Devolvemos la petición modificada con el header personalizado
};
```
```typescript
// src/app/app.config.ts
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { ngrokInterceptor } from './ngrok.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes),
    provideHttpClient(withInterceptors([ngrokInterceptor])) // Añadimos el interceptor a la configuración del cliente http para que se ejecute en todas las peticiones
  ]
};
```
```bash
# Arrancamos el servidor de Angular y permitimos las peticiones desde el dominio de ngrok
ng serve ---allowed-hosts 5f82-88-17-32-110.ngrok-free.app

# Si lo preferimos podemos permitir todas las peticiones
ng serve ---allowed-hosts all
```
---
<br><br><br>

## *[volver al índice](../README.md)*