---
layout: post
disqus: true
title:  "Iniciando en ruby"
date:   2018-04-11 16:56:50 -0300
categories: ruby
---

Hablando con un amigo que quiere iniciarse en el mundo de ruby, me pregunto que es lo primero que debería hacer para iniciar en ruby, la respuesta `instalar RVM`

> Pero ¿Que es RVM? "RVM es una herramienta de línea de comandos que le permite instalar, administrar y trabajar fácilmente con múltiples entornos ruby, desde intérpretes hasta conjuntos de gemas." by [RVM](https://rvm.io/)

En otras palabras si estas trabajando con muchos proyectos en ruby, cada proyecto usa una version diferente o igual de ruby, cada proyecto tiene sus propias gemas o comparten gemas pero estas gemas son versiones diferentes....mas o menos se van dando cuenta del problema de tener que administrar todos los ambientes?? Solución RVM, versiones de ruby separadas con sus propios conjunto de gems(gemsets)

No voy a hacer una guía para [instalar RVM](https://rvm.io/rvm/install), ya hay muchas guia de eso, pero si les voy a mostrar algunas características que pueden usar en sus proyectos.

- Instalar cualquier version de ruby `rvm install [version de ruby]`
- Ver version de ruby instaladas `rvm list`
- Cambiar de version de ruby `rvm use [version de ruby]`
- Crear un set de gems para un version de ruby `rvm gemset create [nombre del gemset]`
- Ver lista de gemsets para la version de ruby actual `rvm gemset list`
- Cambiar de gemset `rvm gemset use [nombre del gemset]`
- Copiar un gemset `rvm gemset copy [version de ruby de origen]@[nombre del gemset  de origen] [version de ruby de destino]@[nombre del gemset de destino]`
- Si quieren linkear una version de ruby con un gemset especifico `rvm use --ruby-version [version de ruby]@[nombre del gemset]`, esto de linkear sirve para que cuando entres a una carpeta especifica se cambia automáticamente la version de rubi y el gemset

![alt text](https://media.giphy.com/media/406il0a0dPvkjRbWBu/giphy.gif "RVM example")
