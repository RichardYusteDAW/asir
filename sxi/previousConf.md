# Configuración previa de las máquinas

## Actualizaciones previas 🔄
```bash
apt-get update                                    # Actualiza la lista de paquetes
apt-get install vim gpm net-tools openssh-server  # Instala vim(editor), gpm(ratón), net-tools(ifconfig), openssh-server(servidor ssh)
dpkg-reconfigure locales                          # Configurar idioma
poweroff || shutdown now || shutdown -h || halt   # 4 comandos para apagar el sistema
```
---
<br>


## Anfitrión 🖥️
```bash
su lliurex                                        # Cambiar a usuario lliurex (pass = Ro0t@19-20)

sudo rm /etc/resolv.conf                          # Eliminar el enlace simbólico del archivo que contiene la configuración de DNS.
sudo nano /etc/resolv.conf                        # Crear el archivo resolv.conf de nuevo.
	nameserver 172.27.111.5
	nameserver 172.27.111.6

sudo nano /etc/hostname                           # Nombre del equipo.
	pc02

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
	pc02
```
---
<br>

## Servidores 🖥️🖥️🖥️
- `enp0s3` es la interfaz de red que se conecta a la red interna.
- `allow-hotplug` es para que la interfaz se active automáticamente.
- `auto` es para que la interfaz se active automáticamente.
- `iface` es para configurar la interfaz.
  - `inet static` es para configurar la interfaz en modo estático.
- `address` es para asignar la dirección IP.
- `netmask` es para asignar la máscara de red.
- `gateway` es para asignar la puerta de enlace.
- `dns-nameservers` es para asignar los servidores DNS.
---
<br>


### Servidor DMZ 🔒
```bash
nano /etc/network/interfaces                      # Archivo de configuración de red.
	allow-hotplug enp0s3                          # Eliminar esta línea.
	
	auto enp0s3
	iface enp0s3 inet static                      
	address 10.10.2.254
	netmask 255.255.255.0
	gateway 10.10.2.1

sudo rm /etc/resolv.conf                          # Eliminar el enlace simbólico del archivo que contiene la configuración de DNS.
sudo nano /etc/resolv.conf                        # Crear el archivo resolv.conf de nuevo.
	nameserver 172.27.111.5
	nameserver 172.27.111.6

sudo nano /etc/hostname                           # Nombre del equipo.
	dmz02srv

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
	127.0.0.1 dmz02srv.dom02.net dmz02srv localhost
```
---
<br>

### Servidor interno 💻
- Para Windows:
  - Poner en red interna
  - Darle IP y DNS
```bash
nano /etc/network/interfaces
	allow-hotplug enp0s3                          # Eliminar esta línea.
	
	auto enp0s3
	iface enp0s3 inet static
	address 192.168.2.254
	netmask 255.255.255.0
	gateway 192.168.2.1

sudo rm /etc/resolv.conf                          # Eliminar el enlace simbólico del archivo que contiene la configuración de DNS.
sudo nano /etc/resolv.conf                        # Crear el archivo resolv.conf de nuevo.
	nameserver 172.27.111.5
	nameserver 172.27.111.6

sudo nano /etc/hostname                           # Nombre del equipo.
	int02srv

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
	127.0.0.1 int02srv.dom02.net int02srv localhost
```
---
<br>

### Enrutador Cortafuegos 🔥
```bash
nano /etc/network/interfaces
allow-hotplug enp0s3                              # Eliminar esta línea.

auto enp0s3
iface enp0s3 inet static
address 192.168.2.1
netmask 255.255.255.0

auto enp0s8
iface enp0s8 inet static
address 10.10.2.1
netmask 255.255.255.0
	
auto enp0s9
iface enp0s9 inet static
address 192.168.21.25
netmask 255.255.255.0
gateway 192.168.21.254

sudo rm /etc/resolv.conf                          # Eliminar el enlace simbólico del archivo que contiene la configuración de DNS.
sudo nano /etc/resolv.conf
	nameserver 172.27.111.5
	nameserver 172.27.111.6

sudo nano /etc/hostname                           # Nombre del equipo.
	fw02router

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
	127.0.0.1 fw02router.dom02.net fw02router localhost

sudo nano /etc/rc.local                           # rc.local es un script que se ejecuta al inicio del sistema.
	#! /bin/bash
	echo 1 > /proc/sys/net/ipv4/ip_forward        # Habilitar el reenvío de paquetes (Para forwarding tiene que ser 1)
	iptables -t nat -A POSTROUTING -o enp0s9 -j MASQUERADE # Configurar NAT

sudo chmod +x /etc/rc.local                       # Dar permisos de ejecución al archivo rc.local.
```
---
<br>
	
## Comandos para comprobar que la configuración está OK 🛠️
```bash
hostname                                          # Muestra el nombre del equipo.
hostname --fqdn                                   # Muestra el nombre del equipo y el dominio (Fully Qualified Domain Name).
dnsdomainname                                     # Muestra el dominio.
iptables -nL -t nat                               # Muestra las reglas NAT.
iptables -t nat -D POSTROUTING 1                  # Elimina la regla NAT.

# Introducir rutas estáticas para que el anfitrión se pueda comunicar con el resto de máquinas
sudo ip route add 192.168.2.0/24 via 192.168.21.25
sudo ip route add 10.10.2.0/24 via 192.168.21.25

# Rutas estáticas permanentes (post-up para añadir y post-down para eliminar las rutas cuando se active o desactive la interfaz)
post-up ip r a 10.10.2.0/24 via 192.168.21.25
post-up ip r a 192.168.2.0/24 via 192.168.21.25

post-down ip route del 10.10.2.0/24 via 192.168.21.25
post-down ip r d 192.168.2.0/24 via 192.168.21.25

# Actualizar configuracion de red
systemctl restart networking.service
```
---

<br><br><br>

## *[volver al índice](../README.md)*