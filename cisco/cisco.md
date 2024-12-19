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


## 2. Paginación “-----More-----” 📑
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
no cdp run                                 # Desactiva CDP globalmente
no cdp enable                              # Desactiva CDP en una interfaz concreta
```
---
<br>


## 9. Historial de comandos 📜
```bash
CNTRL+P (↑) y CNTRL+N (↓)                  # Recorren el historial de comandos
terminal history size 100                  # Tamaño del historial
show history                               # Muestra el historial (10 comandos por defecto)

```
---
<br>


## 10. Comandos de Windows 🖥️
```bash
netstat                                    # Lista los protocolos, ip, puertos y estado
netstat -n                                 # Ip y puertos en formato numérico
route print o netstat -r                   # Muestra la tabla de routing de un host de Windows
ping 192.168.1.10                          # El comando ping o solicitud eco nos indica si hay respuesta por parte del dispositivo a la que se le solicita.
traceroute (tracert) 192.168.1.10          # Es una utilidad que genera una lista de saltos que se alcanzaron correctamente a lo largo de la ruta.
show ip arp                                # Para visualizar la tabla ARP en el router
arp -a                                     # Para visualizar la tabla ARP en Windows.
arp -d                                     # Para eliminar la tabla ARP en Windows.	

ipconfig                                   # Muestra los ajustes de configuración IP en una PC con Windows.
ipconfig /all                              # Para identificar la dirección MAC de un adaptador Ethernet.
ipconfig /release                          # Libera la IP.
ipconfig /renew                            # Solicita una nueva IP.
ipconfig /displaydns                       # Muestra todas las entradas DNS en caché.
nslookup                                   # Permite que el usuario consulte de forma manual los servidores de nombres para resolver un nombre de host dado.
```
---
<br>


## 11. Enrutamiento dinamico 🔄
```bash
router rip                                 # Habilita RIPv1.
version 2                                  # Para habilitar RIPv2.
no auto-summary                            # Anula la sumarización automática (solo RIPv2).
no version                                 # Revierte de  RIPv2 a RIPv1.
network 192.168.1.0                        # Anuncia las redes (con clase) para RIPv1.
show ip protocols                          # Muestra los parámetros del protocolo de routing IPv4.
passive-interface g0/0                     # Para evitar que las actualizaciones de routing se transmitan.
no passive-interface g0/0                  # Vuelve a dar de alta la interfaz.
passive-interface default                  # Convierte todas las interfaces en pasivas.
default-information originate              # Propaga de la ruta estática predeterminada en actualizaciones RIP.
redistribute static                        # Propaga las redes estáticas en actualizaciones RIP.

debug                                      # Muestra menajes de comunicación de estado (procesos, protocolos, mecanismos y eventos).
debug ip rip                               # Rutas aprendidas por rip.
no debug ip rip o undebug debug ip rip     # Detiene la recopilación de información de depuración para RIP.
undebug all                                # Detener el comando debug.
debug ip icmp                              # Supervisa el estado de mensajes ICMP.

terminal monitor                           # Muestra los mensajes de registro en una terminal (consola virtual).
terminal no monitor                        # Los desactiva.
```
---
<br>


## 12. Seguridad 🔒
```bash
service password-encryption                                                         # Encripta las contraseñas.
security passwords min-length 8                                                     # Mímino 8 caracteres.
login block-for 120 attempts 3 within 60                                            # Bloquea durante 120s si hay 3 intentos en 60s.
exec-timeout 10                                                                     # Desconecta a los usuarios inactivos después de 10 min.

# En los puertos                                                        
Switchport port-security mac-address D6-6D-6D-F5-1B-BC                              # Direcciones MAC seguras estáticas.
switchport port-security maximum 3                                                  # Direcciones MAC seguras dinámicas.
switchport port-security mac-address sticky D6-6D-6D-F5-1B-BC                       # Direcciones MAC seguras dinámicas persistentes.
switchport port-security violation {protect | restrict | shutdown | shutdown vlan3} # Cambia la seguridad de los puertos.
show port-security interface fa 0/0                                                 # Muestra la configuración de seguridad de los puertos.
show port-security address # Muestra todas las direcciones MAC seguras configuradas en todas las interfaces del switch o en una interfaz especificada con la información de vencimiento.
```
---
<br>


## 13. SSH 📡
```bash
show ip ssh                                        # Para verificar que el switch admita SSH.
show ssh                                           # Para revisar las conexiones SSH.
ip domain-name iesconselleria.com                  # Nombre de dominio ip.
ip ssh version 2                                   # Para configurar la versión 2 de SSH.
crypto key generate rsa general-keys modulus 1024  # Para que el router cifre el tráfico.1024 son los bits, el tamaño de la clave. Cuanto más grande más segura (360 a 2048 bits).
crypto key zeroize rsa                             # Elimina el par de claves RSA.
username richard                                   # Nombre de usuario.
secret 1234                                        # Contraseña.
login local && transport input ssh                 # Habilita las sesiones SSH entrantes..
```
** Algunos comandos debug, como `debug all` y `debug ip packet`, generan una importante cantidad de resultados y usan una gran porción de recursos del sistema. El router estaría tan ocupado mostrando mensajes de depuración que no tendría suficiente potencia de procesamiento para realizar las funciones de red, o incluso escuchar los comandos para desactivar la depuración. Por este motivo, no se recomienda y se debe evitar utilizar estas opciones de comando.

---
<br>


## 14. ACLs 🚧
```bash
# Estándar (0-99, 1300-1999):
access-list 1 permit 192.168.1.13                            # Permite tráfico desde 192.168.1.13.
access-list 1 permit host 192.168.1.13                       # Permite tráfico solo desde la dirección exacta 192.168.1.13.
access-list 1 remark Esto es un comentario                   # Añade un comentario a la ACL.
ip access-group 1 in                                         # Aplica la ACL en la interfaz entrante.

# Extendida (100-199, 2000-2699):
access-list 100 deny tcp 192.168.1.13 172.16.0.14 eq telnet  # Niega el tráfico Telnet de 192.168.1.13 a 172.16.0.14.
access-list 100 permit tcp any any established               # Permite tráfico TCP establecido desde cualquier origen a cualquier destino.
access-list 100 permit icmp any any                          # Permite todo el tráfico ICMP.
ip access-group 100 out                                      # Aplica la ACL en la interfaz saliente.

# Nombradas:
ip access-list extended ASIR
    deny tcp 192.168.1.13 172.16.0.14 eq telnet
    25 deny tcp 192.168.1.13 172.16.0.14 range 20 21
    no 10                                                    # Elimina la regla número 10.
ip access-group 100 out                                      # Aplica la ACL extendida en la interfaz saliente.

# VTY:
line VTY 0 4
login local
transport input ssh
access-class 1 in                                            # Aplica la ACL en las conexiones VTY entrantes.

show access-lists                                            # Muestra las ACLs configuradas y sus coincidencias.
clear access-list counters 1                                 # Reinicia los contadores de coincidencias de la ACL 1.

```
---
<br>


## 15. Spanning Tree Protocol (STP) 🌉
```bash
bridge priority 32.768                                       # Cambia la prioridad del puente raíz.
show spanning tree                                           # Muestra la ID del raíz, la ID del puente y el estado de los puertos.
show spanning tree summary                                   # Muestra un resumen del estado de los puertos.
show spanning tree root                                      # Muestra el estado y la configuración del puente raíz.
show spanning tree detail                                    # Muestra información detallada sobre los puertos.
show spanning tree interface                                 # Muestra la config y el estado de la interfaz de STP.
show spanning tree blockedports                              # Muestra los puertos bloqueados.
```
---
<br>


## 16. VLANs 🏷️
```bash
vlan 3
name ASIR
switchport access vlan 3                                    # Asigna una VLAN a un puerto.

switchport mode trunk                                       # Configura un puerto como troncal.                              
switchport mode dynamic desirable                           # Configura un puerto como troncal dinámico deseable.
switchport trunk encapsulation dot1q                        # Configura la encapsulación de troncal 802.1Q. (dot1q native vlan 3 para cambiar la VLAN nativa)
switchport trunk native vlan 3                              # Cambia la VLAN nativa
switchport trunk allowed vlan 3, 4, 5, 6                    # Permite las VLAN 3, 4, 5 y 6 en un enlace troncal.

mls qos trust [cos | device cisco-phone | dscp | ip-precedence] # Establece el estado confiable de una interfaz, y para indicar qué campos del paquete se usan para clasificar el tráfico
delete flash:vlan.dat                                       # Elimina el archivo vlan.dat

# En el router:
interface fa 0/1
no ip address
no shutdown
interface fa 0/1.10
encapsulation dot1q 10
ip address 192.168.1.10 255.255.255.0

show vlan                                                   # Lista detallada de los nº y nombres de las VLAN que están activas y puertos asociados a éstas.
show vlan brief                                             # Lista abreviada de las VLAN que están activas y puertos asociados.
show vlan id 3                                              # Muestra info sobre una VLAN específica.
show vlan name ASIR                                         # Muestra info sobre una VLAN específica.
show vlan summary                                           # Muestra el conteo de todas las VLAN configuradas.
show interfaces F0/18 switchport                            # Verifica que la VLAN de acceso para la interfaz F0/18 se haya restablecido a la VLAN 1.
show interfaces F0/18 trunk                                 # Resolver problemas de enlaces troncales
```
---
<br>


## 17. Switch 🔌
```bash
show boot                                                   # Para ver la configuración actual del archivo de arranque de IOS.
duplex full                                                 # Para especificar manualmente el modo dúplex.
speed 100                                                   # Para especificar manualmente la velocidad.
mdix auto                                                   # Habilita MDIX y la interfaz detecta automáticamente el tipo de conexión de cable requerido (directo o cruzado) y configura la conexión conforme a esa información.
duplex auto y speed auto                                    # Modo automático.
show controllers ethernet-controller phy | include Auto-MDIX  # Para examinar la configuración de auto-MDIX de una interfaz específica.
```
---
<br>


## 18. DHCP 🖧
```bash
ip dhcp excluded-address 192.168.1.1                        # Para excluir direcciones específicas.
ip dhcp excluded-address 192.168.1.1 192.168.1.10           # Para excluir un rango.
ip dhcp pool PACO-POOL                                      # Para crear el nombre del pool.
network 192.168.1.0 255.255.255.0                           # Define el conjunto de direcciones.
default-router 192.168.1.1                                  # Define el gateway predeterminado.
dns-server 8.8.8.8                                          # Define el servidor DNS.
domain-name paco.local                                      # Define el nombre de dominio.
lease {days [hours] [minutes] | infinite}                   # Duración de la concesión.
netbios-name-server 192.168.1.200                           # Define el servidor WINS con NetBIOS.
no service dhcp                                             # Deshabilita el servicio de DHCP.
show ip dhcp binding                                        # Muestra una lista de todas las vinculaciones de la dirección IPv4 con la dirección MAC que fueron proporcionadas por el servicio DHCPv4.
show ip dhcp server statistics                              # Verifica si el router recibe o envía los mensajes.
ip helper-address 192.168.100.150                           # Hace que el router actúe como agente de retransmisión DHCPv4.
ip address dhcp                                             # Para configurar el router como cliente DHCPv4.
show ip dhcp conflict                                       # Muestra todos los conflictos de direcciones que registran el servidor de DHCPv4.
debug ip packet 192                                         # Para mostrar solamente los mensajes DHCPv4.
debug ip dhcp server events                                 # Informa eventos del servidor, como asignaciones de direcciones y actualizaciones de bases de datos.
```
---
<br>


## 19. SLAAC 🔧
```bash
# SLAAC solamente:
no ipv6 nd managed-config-flag                              # Indicador M=0 (indicador de configuración de dirección administrada)
no ipv6 nd other-config-flag                                # Indicador O=0 (indicador de otra configuración)

# DHCP sin información de estado
Ipv6 dhcp server PACO-POOL                                  # Crea un pool de direcciones IPv6.
ipv6 nd other-config-flag                                   # Indicador O=1 (indicador de otra configuración)

ipv6 address autoconfig                                     # Habilita la configuración automática del direccionamiento IPv6 mediante SLAAC (para que el router actúe como cliente de DHCP)

# DHCP con información de estado (como DHCPv4)
address prefix 2001:0DB8:ACAD:0001::/64 lifetime infinite   # Indica el conjunto de direcciones que debe asignar el servidor y el tiempo de concesión en segundos.
ipv6 dhcp server PACO-POOL                                  # Crea un pool de direcciones IPv6.
ipv6 nd managed-config-flag                                 # indicador M=1 (indicador de configuración de dirección administrada)

# DHCPv6
ipv6 address dhcp                                           # Habilita al router para que funcione como cliente DHCPv6.
show ipv6 dhcp pool                                         # Verifica el nombre del pool de DHCPv6 y sus parámetros.
show ipv6 dhcp binding                                      # Muestra la vinculación automática entre la dirección link-local del cliente y la dirección asignada por el servidor.
debug ipv6 dhcp detail                                      # Muestra los mensajes DHCPv6 intercambiados entre el cliente y el servidor.
```
---
<br>


## 20. NAT 🌐
```bash
ip nat inside source static 192.168.1.1 209.165.201.5       # Traducción estática

ip nat pool PACO-POOL 209.165.201.226 209.165.201.240 netmask 255.255.255.0 # Define el ámbito de direcciones
access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 pool PACO-POOL                  # Traducción dinámica

ip nat inside source list 1 interface fa 0/1 overload       # Traducción estática PAT
ip nat inside source list 1 pool PACO-POOL overload         # Traducción dinámica PAT

ip nat inside source static tcp 192.168.10.254 80 209.165.201.5 8080 # Reenvío de puertos

ip nat inside                                               # Interna (en la interfaz correspondiente)
ip nat outside                                              # Externa (en la interfaz correspondiente)

show ip nat translations verbose                            # Muestra las traducciones NAT activas
ip nat translation timeout 120                              # Reconfigura los contadores (seg) sino caduca en 24h.
show ip nat statistics                                      # Muestra información sobre la cantidad total de traducciones activas, los parámetros de configuración NAT, la cantidad de direcciones en el conjunto y la cantidad de direcciones que se asignaron.
clear ip nat statistics                                     # Borra las estadísticas de todas las traducciones anteriores.

debug ip nat detailed                                       # Muestra información sobre cada paquete que traduce el router
```
---
<br>


## 21. CDP & LLDP 📡
```bash
cdp run                                                     # Habilita el CDP globalmente para todas las interfaces
cdp enable                                                  # Habilita el CDP en una interfaz concreta
show cdp neighbors detail                                   # Verifica el estado de CDP y muestra una lista de sus componentes adyacentes
show cdp interface                                          # Muestra las interfaces que están habilitadas en CDP

lldp run                                                    # Habilita el LLDP globalmente para todas las interfaces.
lldp transmit                                               # Permite que una interfaz emita LLDP
lldp receive                                                # Permite que una interfaz reciba LLDP
show lldp                                                   # Verifica que LLDP ya se haya habilitado en el dispositivo.
show lldp neighbors detail                                  # Detecta los componentes adyacentes al dispositivo
```
---
<br>


## 22. Reloj y NTP 🕒
```bash
clock set 20:36:00 dec 11 2015                            # Establece la hora y la fecha.
show clock detail                                         # Muestra la hora actual en el reloj.
show ntp status                                           # Para verificar que el router esté sincronizado con el servidor NTP.
show ntp associations                                     # Verifica que el reloj de los dispositivos esté sincronizado por medio de NTP.
ntp server  209.165.201.5                                 # Asigna servidor ntp
```
---
<br>


## 23. Syslog 📜
```bash
service timestamps log datetime msec                      # Obliga a los eventos registrados a que indiquen la fecha y hora
logging console                                           # Envía mensajes de registro a la consola para todos los niveles de gravedad.
logging buffered                                          # Almacena en búfer los mensajes de registro.
show logging                                              # Muestra la configuración predeterminada del servicio de registro en un router Cisco

# Server:
logging 192.168.1.1                                       # Asigna la dirección IP del servidor de registro.
logging trap 5                                            # Establece el nivel de gravedad de los mensajes que se envían al servidor de registro.
logging source-interface g 0/0                            # Especifica la interfaz de origen para los mensajes de registro.
interface lookback 0                                      # Crea una interfaz de software.
```
---
<br>

## 24. Mantenimiento de archivos 📁
```bash
show file systems                                         # Enumera todos los sistemas de archivos.
dir                                                       # Enumera el contenido de flash.
cd vram                                                   # Para ver el contenido de la memoria NVRAM.
file >> log                                               # Para realizar una copia de respaldo de las configuraciones con captura de texto (Tera Term).
file >> send file                                         # Para restaurar la copia de respaldo.
copy running-config tftp                                  # Para guardar la configuración en ejecución en un servidor TFTP.
copy startup-config tftp                                  # Para guardar la configuración de inicio en un servidor TFTP.
copy tftp running-config o copy tftp startup-config       # Para restaurar la copia de respaldo
copy run usbflash0:/                                      # Copia el archivo de configuración a la unidad de memoria flash USB
copy usbflash0:/R1-config run                             # Para restaurar la copia de respaldo.
copy flash0: tftp                                         # Para copiar la imagen del IOS en un servidor TFTP.
copy tftp: flash0                                         # Para copiar la imagen del IOS desde un servidor TFTP.
boot system flash c1841-ipbasek9-mz.124-12.bin            # Para que cargue la nueva imagen durante el inicio.
```
---
<br>

## 25. Instalación de licencias 📜
```bash
show license feature                                          # Para ver las licencias de paquetes de tecnología y las licencias de características admitidas en el router
show license udi                                              # Muestra el UDI (ID del producto (PID), el número de serie (SN) y la versión de hardware)
license install flash0:Seck9-C1900-SPE150_K9-FAB12340099.xml  # Instala la licencia 
show version                                                  # Para verificar que se instaló la licencia.
show license                                                  # Para mostrar información adicional sobre las licencias.
license accept end user agreement                             # Acepta el EULA.
license boot module c1900 technology-package datak9           # Activa una licencia de RTU license save flash0:R2_license_files: realiza copia de respaldo de la licencia.

# Desinstalación de la licencia
license boot module c1900 technology-package seck9 disable    # Desactiva la licencia
license clear seck9                                           # Elimina la licencia
no license boot module c1900 technology-package seck9 disable # Revierte la desactivación de la licencia

# Restablecer contraseña:
http://www.cisco.com/c/en/us/support/docs/routers/10000-series-routers/12818-61.html
rommon 1 > confreg 0x2142
rommon 2 > reset

Router# copy startup-config running-config
Router# configure terminal
Router(config)# enable secret cisco
Router(config)# config-register 0x2102
Router(config)# end
Router# copy running-config startup-config
Router# reload
```
<br><br><br>

## *[volver al índice](../README.md)*