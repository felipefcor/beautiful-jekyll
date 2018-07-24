---
layout: post
title: Cómo funciona la terminal (SHELL)
tags:shell terminal linux comandos 
---
## [**Aprende Javascript con MentoringJS - Pretraining Step 4**](http://MentoringJS.com)
### ¿Qué es la shell?
La _shell_ es un programa que recibe comandos del teclado para darle órdenes al sistema operativo de cómo actuar. Antiguamente, esta era la única manera que un usuario tenía de interactuar en un sistema [Unix](http://www.unix.org/what_is_unix/history_timeline.html).
En la actualidad existen multitud de _interfaces_ gráficos para los usuarios.

En los actuales sistemas [Linux](https://www.debian.org/releases/stable/armel/ch01s02.html.es) un programa llamado Bash actúa como la Shell. De todos modos, se pueden instalar otros prograsmas _shells_, como por ejemplo _ksch_, _tcsh_ y _zsh_.

### ¿Qué es la terminal?

Es un programa llamado **terminal emulador**. Es un programa que abre una ventana y te permite interactuar con la _shell_.  En Linux hay multitud de terminales que se pueden usar, tales como Gnome-terminal, _konsole_, _xterm_, _rxvt_, _kvt_, _nxterm_, y _eterm_.

### Comandos útiles
+ #### Navegación
El sistema de ficheros en Linux tiene una estructura jerárquica. Para moverse por esa estructura hay tres comandos básicos esenciales:
```
ls
```
Este comando lista los archivos que hay en un directorio
```
pwd
```
Este comadndo permite saber en todo momento en qué directorio te encuentras. Cuando uno se mueve a través de diferentes directorios, es esencial saber dónde está en un momento dado.
```
cd
```
Este comando permite moverse entre directorios, tanto a uno inferior como a uno superior.

  Como iremos viendo, todos los comandos permiten el uso de _parámetros_.
  Por ejemplo, en el comando **ls**, dos parámetros útiles serían:
  ```
    ls -la

  ```
El parámetro -l muestra información de los archivos listados, tal como propietario, fecha de creación o modificación,etc.

  El parámetro -a muestra los archivos ocultos.

+ #### Tratamiento de ficheros

  ```
  less archivo
  ```
  El comando less te permite ver el contenido de un archivo.
  ```
  file archivo
  ```
  El comando file te permite saber qué tipo de archivo es.


+ #### Manipulando archivos

  ```
  cp
  ```
  Este comando permite copiar archivos o directorios.
  ```
  mv
  ```
  Este comando permite mover o renombrar archivos y directorios.
  ```
  rm
  ```
  Este comando permite eliminar archios y directorios.
  ```
  mkdir
  ```
  Este comando permite crear directorios.

+ #### Trabajando con comandos

  Los comandos pueden ser de cuatro tipos:

    * Un programa ejecutable
    * Un comando creado dentro del _shell_
    * Una función _shell_
    * Un alias

  Para identificar comandos se puede utilizar los comandos:
  ```
  type
  ```
  ```
  which
  ```

  Para saber más de los comandos se pueden utilizar los siguientes comandos:

  ```
  cualquier comando --help
  ```
  ```
  man cualquier comando
  ```

+ #### Permisos

  Los permisos son una parte muy importante en eltratamiento de ficheros o directorios. Los comandossiguientes son los más importantes para realizar tareason permisos.

  ```
  chmod
  ```
  Este comando permite cambiar los permisos de ejecución,lectura y escritura de archivos y directorios.
  ```
  chown
  ```
  Este comando permite cambiar el propietario de un archivoo directorio.
  ```
  chgroup
    ```
    Este comando permite cambiar el grupo de un archivo o directorio.


Existen muchos más comandos y trabajos que se pueden llevar a cabo en sistemas Linux. Para ampliar toda esta información se puede consultar la [siguiente web.](http://linuxcommand.org/lc3_learning_the_shell.php)

---
