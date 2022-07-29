---
title: "Crear un entorno LAMP con Docker"
date: 2022-07-29T17:01:25-05:00
draft: false
---

Hasta hace unas semanas trabajàbamos usando LAMPP y sus variantes de XAMPP para Windows y MAMPP para macOS. Esta herramienta era un quebradero de cabeza cada vez que teníamos que instalar o configurar alguna dependencia, ya que en cada sistema operativo la instalación era diferente e incluso en cada versión de dicho SO las cosas cambiaban. Esto hacía que a unas personas les funcionara el proyecto en local y a otras no, o al menos no sin meter muchas horas de investigación para conseguir echarlo a andar.

Hacía ya tiempo que había escuchado Docker, pero la verdad nunca me había puesto a investigar sobre el tema. Me sonaba que utilizar Docker podía solucionar estos temas de compatibilidades entre distintos equipos y sistemas operativos, por lo que empecé a leer un libro sobre el tema que tenía guardado en mi zona pendiente de la estantería de libros y me resultó muy interesante.

## ¿Qué es Docker?
En resumidas cuentas, Docker es una "máquina virtual" a nivel de aplicaciones. La diferencia entre un contenedor de Docker y una máquina virtual es que una _virtual machine_ agarra recursos de un equipo y se queda completamente encapsulado, en cambio, un contenedor de Docker comparte el mismo SO del equipo en el que se está corriendo, permitiendo así que dos contenedores se comuniquen entre sí.

Lo más interesante de Docker es que funciona como GitHub u otros servicios de versionado de código, es decir, al igual que en GitHub puedes publicar tus repositorios para que otros puedan usar tu código, en Docker puedes descargarte configuraciones de contenedores, ahorrándote así horas de configuraciones. Hay imágenes completamente oficiales con los servicios más habituales como servidores configurados con apache, con node, con mysql, etc. Estas imágenes se pueden conseguir en [Docker Hub](https://hub.docker.com/) una vez nos hayamos registrado.

## Para qué usé Docker
En nuestro caso particular, quise usar docker para crear un entorno de desarrollo en nuestras máquinas locales para que este entorno fuera completamente exportable, independientemente del equipo y del SO. La idea era configurar el entorno una vez y olvidarnos de más configuraciones en el futuro. No voy a mentir, echarlo a andar me costó más de lo esperado, estuve dos-tres días batallando, pero al final se pudo.

## Entorno LAMPP con Docker
Para crear un entorno LAMPP (Linux, Apache, MySQL, PHP y PHPMyAdmin) con Docker utilizamos 3 imágenes:

1. __La imagen de php:7.4.30-apache__: es una imagen de un servidor de apache configurado con la versión 7.4.3 de PHP.
2. __La imagen de mariadb__: una imagen completamente configurada con mariadb para la base de datos.
3. __La imagen de PHPMyAdmin__: una imagen con la última versiòn de PHPMyAdmin.

La imagen de PHP fue la que más tuvimos que configurar, tuvimos que añadirle bastantes dependencias para poder usar ___Coposer___ y el motor de búsqueda que utilizamos en la empresa. Estas instalaciones de dependencias se hicieron utilizando un _Dockerfile_.

Después lo único que quedaba era entrelazar las tres imágenes, para ello las unimos usando un archivo especial llamado _docker-compose.yml_ que permite englobar las configuraciones necesarias para no tener que estar pasando toda la información por parámetros. De esta forma, también la configuración es más mantenible y replicable. En dicho fichero lo único que había que hacer era configurar contraseñas, puertos y la red en la que iban a correr las imágenes.

No fue al primer intento, pero finalmente conseguí conectar las tres imágenes, consiguiendo un entorno LAMPP completamente personalizado a nuestras necesidades, y lo más importante, replicable y exportable para todo el equipo.
