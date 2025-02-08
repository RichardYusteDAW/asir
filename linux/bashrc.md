# bashrc

## ¿Qué es? 🤔
El bashrc es un archivo de configuración de bash que se ejecuta cada vez que se inicia una nueva sesión de bash.

--- 
<br>


## Opciones de configuración ⚙️

### Prompt 🖥️
- `\u` - Nombre de usuario
- `\h` - Nombre del host
- `\w` - Ruta actual
- `\W` - Nombre de la carpeta actual
- `\d` - Fecha
- `\t` - Hora
```bash
export PS1="\u@\h:\w\$ "
```

### Colores 🌈
- `\033[01;31m` - Rojo
- `\033[01;32m` - Verde
- `\033[01;33m` - Amarillo
- `\033[01;34m` - Azul
- `\033[01;35m` - Magenta
- `\033[01;36m` - Cyan
- `\033[01;37m` - Blanco
- `\033[00m`    - Reset

```bash
export PS1="\[$rojo\]\u\[$reset\]@\[$verde\]\h\[$reset\]:\[$amarillo\]\w\[$reset\]\$ "
```

### Alias 🎭
```bash
alias ll="ls -l"
```

### Funciones 🛠️
```bash
function mkcd() {
    mkdir -p $1
    cd $1
}
```

### Variables de entorno 🌐
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```
---
<br>

## Ejecución 🚀
Para ejecutar el bashrc, se puede hacer de dos formas:
```bash
source ~/.bashrc
. ~/.bashrc
```
<br><br><br>

## *[volver al índice](../README.md)*