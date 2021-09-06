---
layout: post
image: /images/articles/defaultbk.png
title: Vim como editor de código
resume: VIM es un editor de texto básico, con 2 grandes características: es muy liviano y además se encuentra disponible en (casi) todas las distribuciones linux de la actualidad. Este post contiene un par de configuraciones para convertir esta herramienta básica en un gran editor de código.
---
![VIM Welcome Page](/images/aticles/vim/vim_welcomepage.png)

## (Work in progress)

VIM es una de esas herramientas que causan grandes divisiones entre las personas, no hay terminos medios, se quiere o se odia, es muy parecida a riquelme.
VIM quiere decir "Vi Improved" que significa Vi mejorado, esto no dice mucho, sin mebargo vi quiere decir visual ya que es la parte visual de un editor de texto. ex es el editor de texto que se ejecuta bajo el motor de Vi, Ex proviene de em el cual proviene del editor más básico que se puede encontrar en una computadora actual, el viejo y desconocido por multitudes "ed".
<!--una buena manera de iniciar-->

<!--Agregar mas historia de VIM-->
Vim es un editor de texto muy liviano, normalmente corre en una consola, originalmente desarrollado para linux. Yo personalmente lo uso para editar todo lo que sea posible, este post busca compartir algunas de las configuraciones que he ido agregando a Vim para convertirlo en mi editor de texto preferido.

*Este publicación esta enfocada en la version linux de Vim, no se consideraron versiones adapatadas a otros sistemas operativos.*



Cuando empecé a utilizar vim como editor de código mientras abandonaba el mundo de los IDE's, era lógico extrañar algunas características que hacían todo más sencillo, en este caso al momento de desembarcar en *Vim*(o su predecesor [Vi](https://en.wikipedia.org/wiki/Vi)) todo parece hacerse más complicado, en este caso asumo que el lector a conoce los aspectos básicos del funcionamiento de *Vim*, de no ser así quizá debería iniciar leyendo estos articulos:

* Tutorial 1: Enlace acá
* Tutorial 2: Enlace acá

*Vim* ofrece una gran variedad de [atajos](https://www.arsys.es/blog/soluciones/comandos-vim/) que pueden hacer que tu experiencia con la herramienta sea mucho más agradable. sin embargo este post busca cubrir el uso de plugins que faciliten la edición de código, en este caso particular me voy a enfocar en python como lenguaje de programación ya que actualmente es el que más utilizo.

## Los básicos
Primero cubriremos los requerimientos fundamentales para la edición de código.

### Número de línea
Más que básico, fundamental!, por defecto *Vim* no ofrece el número de línea visible, esto hace sea más dificil de leer o incluso de buscar un error. para este caso lo más sencillo es salir del modo editor y escribir:

```
:set number
```
Esto permitirá ver lo número de línea del archivo que se está editando actualmente y perdera su efecto en cuanto se cierre el archivo, es decir que es totalmente temporal. Si desea que este cambio sea permanente debe agregar el comando anterior al archivo:

```
~/.vimrc
```

Si no encuentra este archivo en su directorio 'home' puede crearlo y agregar esta línea. Una vez editado este archivo, para ver los cambios aplicados inicie un nuevo editor de *Vim*, es posible que este cambio no se vea reflejado en los editores que estaban abiertos desde antes de la edición del archivo, en este caso reinicie su editor para ver los cambios.

     
### Espacios del Tab = 4
Por defecto *Vim* asigna 8 espacios cada vez apretamos la tecla 'Tab', esta médida se puede modificar según sea necesario, en mi caso voy a usar Tab=4 porque es la cantidad de espacios que requiere python para la identación, para esto vamos a agregar 3 comandos adicionales al archivo *vimrc*:

```
set tabstop=4

set shiftwidth=4

set expandtab
```
### Detector de llaves '{}'
```
set showmatch
```

### Resaltar sintaxis
```
syntax on
```

## Instalación del plugin manager

Vim utiliza por defecto un plugin manager pero vamos a instalar **vundle** para simplificar la instalación y actualización de paquetes. en la terminal escriba:
```
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
Esto va a clonar el repositorio *bundle* en su equipo.
El siguiente paso es modificar el archivo de confiración de Vim (vimrc):

  - busque el archivo *vimrc* por lo general se encuentra en su home folder.
  ```
  vim ~/.vimrc
  ```
  
  y agregue el siguiente código:
  

  ```  
  filetype off                  " required

  " set the runtime path to include Vundle and initialize
  set rtp+=~/.vim/bundle/Vundle.vim
  call vundle#begin()

  " let Vundle manage Vundle, required
  Plugin 'gmarik/Vundle.vim'

  " All of your Plugins must be added before the following line
  call vundle#end()
  filetype plugin indent on
  ```
  
  Finalmente guarde los cambios y cierre el archivo.

  - Para instalar el plugin de Vundle dentro de vim, inicie vim desde su terminal. Luego escriba `:PluginInstall` y presione Enter.

## Plugins 

https://www.ionos.mx/digitalguide/servidores/herramientas/editores-linux-como-editar-codigo-con-vim/
