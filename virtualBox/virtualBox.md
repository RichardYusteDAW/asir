# Virtual Box
Es un software de virtualización para arquitecturas x86/amd64.  
Permite la creación y ejecución de máquinas virtuales en las que se pueden instalar sistemas operativos como si fueran equipos físicos.


## 1. Web Site 🌐
[Virtual Box](https://www.virtualbox.org/)

---
<br>

## 2. Instalación 🛠️
```bash
apt update                             # Actualiza la lista de paquetes.
apt upgrade                            # Actualiza los paquetes instalados.
apt install build-essential            # Instala las herramientas de compilación (gcc, make, ...).
apt install linux-headers-$(uname -r)  # Instala las cabeceras del kernel (necesarias para compilar módulos).
/media/USER/vBox/LinuxVBox.run         # Ejecuta el instalador de Virtual Box.

ps aux | grep -i vbox                  # Comprueba que los servicios de Virtual Box están en ejecución (-i: case-insensitive).
lsmod  | grep -i vbox                  # Comprueba que los módulos de Virtual Box están cargados.
```
---
<br>

## 3. Guest Additions 🖥️  
- Las Guest Additions son un conjunto de controladores de dispositivos y aplicaciones del sistema que se instalan en las máquinas virtuales para mejorar su rendimiento y proporcionar una integración más estrecha entre el sistema anfitrión y el sistema invitado.
  - **Máquina Virtual → Dispositivos → Insertar imagen de CD de las Guest Additions...**
---
<br>

## 4. Extension Pack 📦
- El Extension Pack es un paquete de software que añade nuevas funcionalidades a Virtual Box:
  - Soporte para dispositivos USB 2.0 y 3.0.
  - Soporte para RDP (Remote Desktop Protocol).
  - Soporte para discos duros virtuales con cifrado.
  - Soporte para arranque PXE.
  - Soporte para controladores de red Intel PXE.
  - Soporte para controladores de red AMD PCNet.
  - Soporte para controladores de red virtuales.
- Se descarga desde la web oficial de Virtual Box y se instala desde la interfaz gráfica de Virtual Box.
---
<br>

## 5. Cambiar UUID 🆔
```bash
cd 'C:\Program Files\Oracle\VirtualBox\'
VBoxManage.exe internalcommands sethduuid 'D:\NewVM\myDisk1.vdi'										#Crea UUID aleatorio
VBoxManage.exe internalcommands sethduuid 'D:\NewVM\myDisk1.vdi' 10000000-0000-0000-0000-000000000001	#Crea UUID elegido
```
---
<br><br><br>

## *[volver al índice](../README.md)*