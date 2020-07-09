---
layout: post
title: RabbitMQ Multitenant DB
description: >
  Como manejar múltiples inquilinos en distintas bases de datos utilizando Virtual Host de RabbitMq.
author: Frabrizio contin
noindex: true
---

![Screenshot](../../assets/img/blog/RabbitMQmultitenant.png)
![Cover page example](/assets/img/blog/RabbitMQmultitenant.png){:data-width="960" data-height="540"}
![Full-width image](https://drive.google.com/file/d/1EQYYjJrgvQxyEvU-wjjr8aqwxO4AMpLf/view?usp=sharing){:.lead data-width="800" data-height="100"}

![Screenshot](/assets/img/RabbitMQmultitenant.png)
![Adaptive Dark Mode Example](/assets/img/blog/dark-mode-ii.jpg){:data-width="1440" data-height="836"}

![Rabbit multi](/assets/img/blog/RabbitMQmultitenant.jpg){:data-width="1440" data-height="836"}

**Introducción**:
La idea de esta post es presentar una forma de conectar distintos inquilinos a sus propias bases de datos (Llamemos por el momento inquilinos a un grupo empresario, cliente o empresas que quieren tener sus propias base de datos, con sus colas en RabbitMq separadas de otros inquilinos para resguardar su seguridad).
Por el momento solo nos enfocaremos en la conexión de peticiones desde un sitio (microservicio1) para el “inquilino 1”  a otro sitio (microservicio2) para el mismo inquilino 

**NOTA**:  Para poder diferenciar usuarios del inquilino 1 o inquilino 2 podríamos enviar un token a través de la cola de RabbitMq, pero no vamos a  hacer énfasis en este post sobre como diferenciar a cada usuario. Prometo en próximos post profundizar sobre este tema.
{:.message}


**Empecemos...**
Bien lo primero para mí, es no reinventar la rueda, es por eso que les comparto un proyecto espectacular en el cual también pueden encontrar mucha información para aquellos amantes de microservicios.


~~~yml
layout: post
title: RabbitMq multitenant DB
image: /assets/img/blog/RabbitMQmultitenant.png
color: '#949667'
~~~
