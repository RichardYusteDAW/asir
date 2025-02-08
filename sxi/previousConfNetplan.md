# Configuración previa de las máquinas con Netplan
[Configuración netplan](https://www.ochobitshacenunbyte.com/2021/04/26/netplan-configurar-la-red-en-ubuntu-20-04/)

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
```yaml
su lliurex                                        # Cambiar a usuario lliurex (pass = Ro0t@19-20)

sudo rm /etc/resolv.conf                          # Eliminar el enlace simbólico del archivo que contiene la configuración de DNS.

sudo nano /etc/netplan/00-installer-config.yaml   # Crear el archivo de configuración de red.
network:
    version: 2                                    # Versión de netplan (2 en Ubuntu 20.04)
    renderer: networkd                            # Renderizador de red
    ethernets:                                    # Configuración de las interfaces de red
        enp0s3:                                   # Nombre de la interfaz
            addresses: [192.168.2.103/24]         # Dirección IP y máscara de red
            gateway4: [192.168.2.1]               # Puerta de enlace
            nameservers:                          # Servidores DNS
                addresses: [172.27.111.5, 172.27.111.6]
```
```bash
sudo netplan apply                                # Aplicar la configuración de red

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
```yaml
nano /etc/netplan/00-installer-config.yaml        # Archivo de configuración de red.
network:
    version: 2                                    # Versión de netplan (2 en Ubuntu 20.04)
    renderer: networkd                            # Renderizador de red
    ethernets:                                    # Configuración de las interfaces de red
        enp0s3:                                   # Nombre de la interfaz
            dhcp4: no                             # Desactivar DHCP
            addresses: [10.10.2.254/24]           # Dirección IP y máscara de red
            gateway4: [10.10.2.1]                 # Puerta de enlace
            nameservers:                          # Servidores DNS
                    addresses: [172.27.111.5, 172.27.111.6]

sudo netplan apply                                # Aplicar la configuración de red

sudo nano /etc/hostname                           # Nombre del equipo.
    dmz02srv

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
   dmz02srv.dom02.net dmz02srv localhost
```
---
<br>

### Servidor interno 💻
- Para Windows:
    - Poner en red interna
    - Darle IP y DNS
```yaml
nano /etc/netplan/00-installer-config.yaml        # Archivo de configuración de red.
network:
        version: 2                                # Versión de netplan (2 en Ubuntu 20.04)
        renderer: networkd                        # Renderizador de red.
        ethernets:                                # Configuración de las interfaces de red.
            enp0s3:                               # Nombre de la interfaz.
                dhcp4: no                         # Desactivar DHCP.
                addresses: [192.168.2.104/24]     # Dirección IP y máscara de red.
                gateway4: [192.168.2.1]           # Puerta de enlace.
                nameservers:                      # Servidores DNS.
                    addresses: [172.27.111.5, 172.27.111.6]
```
```bash
sudo netplan apply                                # Aplicar la configuración de red.

sudo nano /etc/hostname                           # Nombre del equipo.
        int02srv

sudo nano /etc/hosts                              # Nombre del equipo y dominio.
        127.0.0.1 int02srv.dom02.net int02srv localhost
```
---
<br>

### Enrutador Cortafuegos 🔥
```yaml
nano /etc/netplan/00-installer-config.yaml        # Archivo de configuración de red.
network:
    version: 2                                    # Versión de netplan (2 en Ubuntu 20.04)
    renderer: networkd                            # Renderizador de red.
    ethernets:                                    # Configuración de las interfaces de red.
        enp0s3:                                   # Nombre de la interfaz.
            dhcp4: no                             # Desactivar DHCP.
            addresses: [192.168.2.1/24]           # Dirección IP y máscara de red.
            gateway4: [192.168.2.254]             # Puerta de enlace.
            nameservers:                          # Servidores DNS.
                addresses: [172.27.111.5, 172.27.111.6]
```
```bash
sudo netplan apply                                # Aplicar la configuración de red.

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

### Comandos para comprobar que la configuración está OK 🛠️
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
```
<br><br><br>

## *[volver al índice](../README.md)*