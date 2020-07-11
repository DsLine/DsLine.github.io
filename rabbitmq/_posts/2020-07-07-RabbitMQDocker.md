---
layout: post
title: RabbitMQ en Docker
description: >
  Como agregar RabbitMQ a un contenedor en Docker 
author: author1
noindex: true
---

### Introducción.

En este post veremos como instalar RabbitMQ en Docker y también vamos a realizar unas pruebas desde un proyecto en NetCore 3.1 para aprender a escribir y leer mensajes de una cola. 

**NOTA**:  Si todavía no tiene instalado Docker en su sistema operativo, les dejo un link para su instalación.
https://docs.docker.com/docker-for-windows/install/
https://docs.docker.com/engine/install/ubuntu/
{:.message}

### Instalación de RabbitMQ en Docker.

Por el momento solo vamos a subir la imagen de RabbitMQ por ende vamos a usar Docker cli ejecutando el siguiente comando en la consola.

 docker run -d -p 15672:15672 -p 5672:5672 rabbitmq:3-management

-d Nos permite correr el contendor en segundo plano.
-p Especificamos como publicar los puertos en el host (En nuestro caso tanto interna como externamente vamos a utilizar los mismos puertos)
rabbitmq:3-management  Por ultimo especificamos la imagen base que queremos utilizar y luego de “:” la versión de la misma “3-management”

Para verificar que el contenedor esté corriendo correctamente, ingresen a http://localhost:15672/ con usuario y contraseña guest/ guest. 

**NOTA**: Este usuario es cargado por defecto, pero dicho usuario no puede utilizarse en entornos productivos, ya que está restringido para conexiones externas.
Si desea cambiar el nombre de usuario y la contraseña predeterminados, puede hacerlo con las variables ambientales RABBITMQ_DEFAULT_USERy RABBITMQ_DEFAULT_PASS
{:.message}

