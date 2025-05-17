# NANO

## ¿Qué es NANO? 📝 
NANO es un editor de texto de línea de comandos que se utiliza en sistemas Unix y Linux. Es conocido por su simplicidad y facilidad de uso, lo que lo convierte en una excelente opción para aquellos que son nuevos en la edición de texto en la terminal.

## Instalación ⚙️
- Suele estar preinstalado en la mayoría de las distribuciones de Linux.
```bash
sudo apt-get install nano
```
---
<br>


## Abrir NANO 📂
```bash
nano nombre_archivo     # Abre un archivo existente o crea uno nuevo
nano -l nombre_archivo  # Abre un archivo con números de línea
nano -v nombre_archivo  # Abre un archivo en modo de solo lectura
nano -c nombre_archivo  # Abre un archivo y muestra la posición del cursor
nano -B nombre_archivo  # Abre un archivo y crea una copia de seguridad
nano -m nombre_archivo  # Abre un archivo y habilita el modo de marcado
nano -E nombre_archivo  # Abre un archivo y habilita el modo de finalización de línea
nano -R nombre_archivo  # Abre un archivo y habilita el modo de lectura
```

---
<br>


## Comandos básicos de NANO ⌨️
| Acción                          | Comando o tecla                    | Notas                                                        |
|---------------------------------|------------------------------------|--------------------------------------------------------------|
| Alternar números de línea       | `Alt + Shift + #`                  | Activa/desactiva los números de línea en tiempo real         |
| Mover cursor a línea concreta   | `Ctrl + _` y luego número + Enter  | También puede usarse para ir a columna (ej: `10,20`)         |
| Guardar archivo                 | `Ctrl + O`                         | Luego pulsa Enter para confirmar el nombre                   |
| Salir                           | `Ctrl + X`                         | Si hay cambios, preguntará si quieres guardarlos             |
| Cortar línea                    | `Ctrl + K`                         | Elimina la línea y la guarda en el portapapeles interno      |
| Pegar línea                     | `Ctrl + U`                         | Pega la última línea cortada (también funciona con bloques)  |
| Copiar texto                    | `Alt + 6`                          | Copia la línea o selección activa                            |
| Cortar bloque                   | `Ctrl + K`                         | Tras haber seleccionado texto con `Ctrl + ^`                 |
| Pegar bloque                    | `Ctrl + U`                         | Igual que con una línea                                      |
| Deshacer                        | `Alt + U`                          | Deshace el último cambio                                     |
| Rehacer                         | `Alt + E`                          | Rehace el cambio deshecho                                    |
| Buscar texto                    | `Ctrl + W`                         | Luego escribe lo que quieres buscar                          |
| Repetir búsqueda                | `Ctrl + W` y luego `Alt + W`       | Busca el siguiente resultado                                 |
| Reemplazar texto                | `Ctrl + \`                         | Buscar y reemplazar                                          |
| Seleccionar texto               | `Ctrl + ^`                         | Luego mueve el cursor para seleccionar                       |
| Justificar párrafo              | `Ctrl + J`                         | Útil para texto largo o documentos                           |
| Mostrar ayuda integrada         | `Ctrl + G`                         | Lista de atajos en pantalla                                  |
---
<br><br><br>

## *[volver al índice](../README.md)*