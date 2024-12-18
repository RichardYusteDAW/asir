# Virtual Box
Es un software de virtualización para arquitecturas x86/amd64.  
Permite la creación y ejecución de máquinas virtuales en las que se pueden instalar sistemas operativos como si fueran equipos físicos.

## 1. Web Site 🌐
[Virtual Box](https://www.virtualbox.org/)

---
<br>

## 2. Instalación 🛠️
### 2.1. ¿Qúe son las Guest Additions? 🤔
- Las Guest Additions son un conjunto de controladores de dispositivos y aplicaciones del sistema que se instalan en las máquinas virtuales para mejorar su rendimiento y proporcionar una integración más estrecha entre el sistema anfitrión y el sistema invitado.

### 2.2. ¿Qué es el Extension Pack? 🤔
- El Extension Pack es un paquete de software que añade nuevas funcionalidades a Virtual Box:
  - Soporte para dispositivos USB 2.0 y 3.0.
  - Soporte para RDP (Remote Desktop Protocol).
  - Soporte para discos duros virtuales con cifrado.
  - Soporte para arranque PXE.
  - Soporte para controladores de red Intel PXE.
  - Soporte para controladores de red AMD PCNet.
  - Soporte para controladores de red virtuales.

```bash
apt update
apt upgrade
apt install build-essential
apt install linux-headers-$(uname -r)
/media/USER/vBox/LinuxVBox.run
```

<br><br><br>

## *[volver al índice](../README.md)*