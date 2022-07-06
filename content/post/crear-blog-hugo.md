---
title: "Crea tu blog con HUGO y GitHub Pages"
date: 2022-07-04T18:24:13-05:00
draft: false
---

Hoy en día vivimos rodeados de información, hay tanta que a veces resulta difícil encontrar buena información. Por ello considero muy importante que cuando encontramos buen contenido lo guardemos para poder consultarlo más tarde, para ello creé este blog.

Otra de las razones por las que decidí crear este blog fue porque creo que es importante tener una presencia en Internet y más en nuestro sector. No hace falta volverse loco ni perder mucho tiempo ni dinero, con frameworks como HUGO generar un blog nunca fue tan fácil.

## HUGO Framework
HUGO es un framework muy ligero que nos permite crear contenido estático mediante el uso del lenguaje MarkDown. Si eres desarrollador seguro que te suena este lenguaje, ya que se usa para crear la documentación en servicios como GitHub.

Hacer un blog con este framework es absurdamente sencillo. Podría explicarlo paso a paso, pero la verdad es que yo seguí [este vídeo](https://www.youtube.com/watch?v=LIFvgrRxdt4&t=560s&ab_channel=RyanSchachte) de __Ryan Schachte__ y siento que está muy bien explicado. Pienso que cuando algo está bien hecho y no puedes hacerlo o explicarlo mejor, es más lógico compartir ese contenido directamente, así le das visibilidad a otro compañero y te ahorras horas de trabajo innecesario.

Pero antes de despedirme me gustaría hacer un resumen con los comandos más útiles y que vamos a estar utilizando para trabajar con HUGO.

## Comandos
1. __Para crear un post__: `hugo new [RUTA]/[NOMBRE_ARCHIVO].md`
2. __Para correr el servidor en local__: `hugo server`
3. __Para publicar el contenido__: 
    - `hugo -t [NOMBRE_DEL_TEMPLATE]`
    - Ir al directorio que contiene el submódulo y subir los cambios a GitHub