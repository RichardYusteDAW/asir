# COMANDOS CISCO

## 1. Edición de líneas de la CLI 💻
| Tecla                       | Acción                                                                                                                       |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `Tabulación`                | Completa una entrada de nombre de comando parcial.                                                                           |
| `Retroceso`                 | Borra el carácter a la izquierda del cursor.                                                                                 |
| `Ctrl-D`                    | Borra el carácter donde está el cursor.                                                                                      |
| `Ctrl-K`                    | Borra todos los caracteres desde el cursor hasta el final de la línea de comandos.                                           |
| `Esc D`                     | Borra todos los caracteres desde el cursor hasta el final de la palabra.                                                     |
| `Ctrl-U` `Ctrl-X`           | Borra todos los caracteres desde el cursor hasta el comienzo de la línea de comandos.                                        |
| `Ctrl-W`                    | Borra la palabra a la izquierda del cursor.                                                                                  |
| `Ctrl-A`                    | Desplaza el cursor hacia el principio de la línea.                                                                           |
| `Flecha izquierda` `Ctrl-B` | Desplaza el cursor un carácter hacia la izquierda.                                                                           |
| `Esc B`                     | Desplaza el cursor una palabra hacia la izquierda.                                                                           |
| `Esc F`                     | Desplaza el cursor una palabra hacia la derecha.                                                                             |
| `Flecha derecha` `Ctrl-F`   | Desplaza el cursor un carácter hacia la derecha.                                                                             |
| `Ctrl-E`                    | Desplaza el cursor hasta el final de la línea de comandos.                                                                   |
| `Flecha arriba` `Ctrl-P`    | Vuelve a introducir el comando que se encuentra en el búfer del historial, a partir de los comandos más recientes.           |
| `Ctrl-R` `Ctrl-I` `Ctrl-L`  | Vuelve a mostrar la petición de entrada del sistema y la línea de comando después de que se recibe un mensaje de la consola. |
---
<br>


## 2. Paginación “-----More-----” 📜
| Tecla                       | Acción                                                                                                                       |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `Tecla Entrar`              | Muestra la siguiente línea.                                                                                                  |
| `Barra espaciadora`         | Muestra la siguiente pantalla.                                                                                               |
| `Cualquier tecla`           | Termina la cadena que se muestra y vuelve al modo EXEC con privilegios.                                                      |
---
<br>

## 3. Teclas de interrupción 🛑
| Tecla                       | Acción                                                                                                                       |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `Ctrl-C`                    | Termina el modo de configuración y regresa al modo EXEC. También puede interrumpir procesos en curso.                        |
| `Ctrl-Z`                    | Sale del modo de configuración actual y regresa al modo EXEC privilegiado.                                                   |
| `Ctrl-Shift-6`              | Secuencia de pausa multiuso. Se la utiliza para interrumpir búsquedas DNS, traceroutes, pings.                               |
---
<br>

## 4. Principales 📝
```bash
enable                              # EXEC con privilegios.
disable                             # EXEC del usuario.
configure terminal                  # Configuración global.
line
    line console 0                  # Modo subconfiguración
    line vty 0 15                   # Líneas de terminal virtual (SSH, Telnet)
exit                                # Salir de la configuración global o de la subconfiguración.
end ó CNTRL+Z                       # Ir al EXEC privilegiado.
hostname                            # Cambiar el nombre al dispositivo.
no hostname                         # Niega o elimina el comando al que acompaña.
?                                   # Ayuda ofreciendo las posibilidades disponibles.
banner motd # el mensaje del día #  # Crea un mensaje de aviso

logging synchronous                 # Evita que los mensajes de consola interrumpan la entrada de comandos.
```
---
<br>

## 5. Password 🔑
```bash
enable password cisco               # Password EXEC privilegios sin encriptar.
enable secret cisco                 # Password EXEC privilegios encriptada.
password cisco                      # Password EXEC usuario o líneas de terminal virtual.
login                               # Habilita el acceso al usuario o a las líneas de comando virtual.
service password-encryption         # Cifra las contraseñas.
show                                # Muestra las contraseñas
    show startup-config             # Muestra la configuración de inicio.
    show running-config             # Muestra la configuración en ejecución.
```
---
<br>

## 6. Modificación de la configuración ⚙️
```bash
# Guardar la configuración en ejecución:
copy running-config startup-config  # Guarda los cambios realizados en la configuración en ejecución, en el archivo de configuración de inicio.

# Eliminar la configuración en ejecución (3 formas):
reload                              # 1ª Recarga IOS y da la opción de guardar cambios o NO guardar cambios de configuración en ejecución.
copy startup-config running-config  # 2ª Restablece la configuración en ejecución a los valores de la configuración de inicio almacenada.
                                    # 3ª Apagando el dispositivo directamente.

# Eliminar la configuración de inicio:
erase startup-config                # Elimina la configuración de inicio.
dir                                 # Permite visualizar las memorias del sistema.
    dir flash                       # Permite visualizar la memoria FLASH.
    dir nvram                       # Permite visualizar la memoria NVRAM.
```
---
<br>

## 7. Configuración de la interfaz virtual del switch o del router 🌐
```bash
interface vlan 1                       # Configura una SVI (Interfaz Virtual de Switch).
interface Serial 0/0/0                 # Configura una Interfaz Serial del router.
interface loopback 0                   # Configura una Interfaz de software del router.
ip address 192.168.1.10 255.255.255.0  # Asigna una IPv4 y una máscara de subred a la interfaz.

ipv6 address 2001:0DB8:ACAD:1::/64     # Asigna una IPv6 y un prefix-length.
ipv6 address FE80::1 eui-64            # Asigna una IPv6 de unidifusión global mediante el proceso EUI-64.

ipv6 address FE80::1 link-local        # Asigna una IPv6 de link local.
ipv6 enable                            # Se usa para crear de forma automática una dirección link-local de IPv6 (se haya asignado una dirección de unidifusión global de IPv6 o no)

shutdown                               # Deshabilita la interfaz virtual (no shutdown la habilita).
ip default-gateway 192.168.1.1         # Para configurar un gateway predeterminado en un Switch.
ipv6 unicast-routing                   # Habilita a un router para que comience a formar parte del grupo de multidifusión para todos los nodos (FF02::2)

show interface F0/1                    # Muestra detalles de la interfaz, incluida la dirección MAC.
show mac address-table                 # Muestra la tabla de direcciones MAC del Switch.
clear mac-address-table dynamic        # Borra las entradas dinámicas de la tabla MAC.

no ip domain-lookup                    # Desactiva la búsqueda de DNS.
```
---
<br>

## 8. Interfaces y tablas de routing 🔗
```bash
show version                               # Muestra información sobre la versión del software Cisco IOS.
show ip route                              # Permite visualizar la tabla de routing IPv4
show ipv6 route                            # Permite visualizar la tabla de routing IPv6
show ip interface brief                    # Permite visualizar el estado (up o down) de la interfaz virtual.
show ip interface Serial 0/0/0             # Muestra las estadísticas IPv4 de todas las interfaces de un router.
show interfaces                            # Muestra las estadísticas de todas las interfaces de un dispositivo.
show running-config interface Serial 0/0/0 # Muestra los comandos configurados actualmente en la interfaz especificada
# Parámetros de filtrado:
    section <regex>                        # Muestra la sección completa.
    include <regex>                        # Resultados que coinciden con la expresión de filtrado.
    exclude <regex>                        # Excluye resultados que coinciden con la expresión de filtrado.
    begin   <regex>                        # Resultados que coinciden con la expresión de filtrado
router ?                                   # Muestra qué protocolos de routing admite IOS
clear ip route *                           # Borra las tablas de routing
show cdp neighbors                         # Descubre dispositivos Cisco vecinos de capa 2
show cdp neighbors detail                  # Muestra detalles de los dispositivos vecinos, útil para diagnóstico de configuración.
no cdp run                                 # Desactiva CPD globalmente
no cdp enable                              # Desactiva CPD en una interfaz concreta
```
---
<br><br><br>

## *[volver al índice](../README.md)*