---
title: "Cómo crear una clave SSH para un servidor propio"
date: 2022-11-12T12:13:33-06:00
draft: false
---
Llevo ya tiempo trabajando en un servidor propio al cual me conectaba con un nombre de usuario y una contraseña, pero el otro día descubrí cómo generar una clave SSH para evitar usar la contraseña.
Esta necesidad nació de la forma en la que uso Neovim, mi editor de texto favorito para programar.

En Neovim suelo comparar mi versión local con la versión del servidor, esto no lo podía hacer en servidores donde no tenía una clave SSH, porque por el momento Neovim o VIM no permiten añadir la contraseña
a la hora de intentar abrir un archivo de un servidor que requiera de ella (al menos que yo sepa). Por lo tanto, para poder seguir utilizando mi editor de texto favorito, me puse a investigar cómo generar
una clave SSH para un servidor propio.

En este _post_ no me voy a centrar en cómo generar claves para servidores de la nube porque normalmente estos servicios incluyen documentación para ello. En este post me voy a centrar solamente a generar
una clave para un servidor que sea de nuestra propiedad. Esto lo hice en el sistema operativo Ubuntu 22.04.1 lts.

Lo primero que debemos realizar es crear la clave en nuestro equipo local:
```
ssh-keygen -t rsa -f ~/.ssh/[NOMBRE_DE_LA_CLAVE]
```
El parámetro `-t` se usa para indicarle que tipo de encriptado queremos utilizar para generar la clave. Por otro lado, el parámetro `-f` se utiliza para indicar dónde queremos guardar dicha clave y con
qué nombre queremos guardarla (donde pone `[NOMBRE_DE_LA_CLAVE]` deberíamos poner un nombre descriptivo que nos indique para qué utilizamos dicha clave o para que servidor es).

Una vez pulsado el botón _enter_ nos da la opción de crear una contraseña, esta contraseña permite tener una segunda capa de seguridad, esto hace que si alguien utiliza tu ordenador le va a pedir una
contraseña si intenta conectarse al servidor. En caso de no querer añadir una clave, simplemente pulsamos _enter_ sin escribir absolutamente nada. 

El siguiente paso es confirmar la contraseña, si no hemos utilizado ninguna simplemente pulsamos _enter_. En caso contrario, habrá que proporcionar la misma contraseña que habíamos puesto.

Una vez realizado estos pasos veremos que tendremos dos archivos nuevos en la ruta que le hemos proporcionado en el comando con el nombre que le hayamos puesto (en mi caso en el directorio `.ssh` que se
encuentra en mi directorio principal). Uno de los archivos tiene la extensión `.pub` y el otro no tiene extensión. El que tiene el nombre de extensión es la llave pública y el que no lo tiene es la privada.

Ahora lo único que nos queda es copiar esta llave en nuestro servidor, para ello existe un comando que nos facilitará mucho el trabajo:

```
ssh-copy-id -i $HOME/.ssh/[NOMBRE_DE_LA_CLAVE].pub [NOMBRE_USUARIO_DEL_SERVIDOR]@[HOST_DEL_SERVIDOR]
```

Como ven estamos copiando la llave pública que acabamos de generar en nuestro servidor. Obviamente, donde pone `[NOMBRE_USUARIO_DEL_SERVIDOR]` va el nombre de usuario que utilizamos para conectarnos vía ssh
y donde pone `[HOST_DEL_SERVIDOR]` es la IP o el dominio que hace referencia al servidor. A la hora de ejecutar este comando nos va a pedir la contraseña, ya que si no cualquiera podría saltarse el paso
de seguridad de la contraseña creando una llave ssh, pero os prometo que va a ser la última vez que os va a pedir que introduzcáis la contraseña.

Una vez terminado este paso, ya nos podremos conectar a nuestro servidor sin que nos pregunte por una contraseña:
```
ssh [NOMBRE_USUARIO_DEL_SERVIDOR]@[HOST_DEL_SERVIDOR]
```
