---
layout: post
image: /images/articles/defaultbk.png
title: Vim como editor de código
resume: (Work in progress) Desde hace algún tiempo que estoy usando vim como editor de código este artículo describe los plugins que me sirvieron.
---

# (Work in progress)
Vim es un editor de texto muy liviano, normalmente corre en una consola, originalmente desarrollado para linux. Yo personalmente lo uso para editar todo lo que sea posible, este post busca compartir algunas de las configuraciones que he ido agregando a Vim para convertirlo en mi editor de texto preferido.

*Este publicación esta enfocada en la version linux de Vim, no se consideraron versiones adapatadas a otros sistemas operativos.*

Vim ofrece una gran variedad de [atajos](https://www.arsys.es/blog/soluciones/comandos-vim/) que pueden hacer que tu experiencia con la herramienta sea mucho más agradable. sin embargo este post busca cubrir el uso de plugins que faciliten la edición de código, en este caso particular me voy a enfocar en python como lenguaje de programación ya que actualmente es el que más utilizo.

## Intalación del plugin manager
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
