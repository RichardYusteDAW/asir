# bash_it

## ¿qué es bash_it? 🤔
Bash-it es un framework para Bash que permite gestionar fácilmente alias, funciones, autocompletado, plugins y temas del prompt.

---
<br>

## Instalación 🚀
```bash
# Clonar el repositorio
git clone --depth=1 https://github.com/Bash-it/bash-it.git ~/.bash_it

# Entrar en el directorio
cd ~/.bash_it

# Instalar bash_it
~/.bash_it/install.sh --silent
```
---
<br>

## Añadir alias 🎭
- Haciendo un `echo` al archivo de alias personalizado.
```bash
# Añadir alias al archivo de alias personalizado
echo "alias lll='ls -lha --color=auto'" >> ~/.bash_it/custom/aliases.bash
```

- Editando el archivo de alias personalizado con `nano` o tu editor de texto favorito.
```bash
nano ~/.bash_it/custom/aliases.bash

# Aliases
alias lll='ls -lha --color=auto'

# Functions
catmd() {
  cat -- "$1" | command glow
}

jq() {
  command jq . "$@"
}
```
- Recargar bash_it para aplicar los cambios.
```bash
source ~/.bashrc
```
---
<br><br><br>

## *[volver al índice](../README.md)*