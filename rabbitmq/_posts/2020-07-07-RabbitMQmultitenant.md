---
layout: post
title: RabbitMQ Multitenant DB
description: >
  Como manejar múltiples inquilinos en distintas bases de datos utilizando Virtual Host de RabbitMq.
author: author1
noindex: true
---

![Screenshot](/assets/img/RabbitMQmultitenant.png)


**Introducción**:
La idea de esta post es presentar una forma de conectar distintos inquilinos a sus propias bases de datos (Llamemos por el momento inquilinos a un grupo empresario, cliente o empresas que quieren tener sus propias base de datos, con sus colas en RabbitMq separadas de otros inquilinos para resguardar su seguridad). Este es un problema que vengo viendo en varias empresas en las que trabaje y me pareció un tema recurrente al cual quería encontrar una posible solución en un laboratorio hogareño.
Por el momento solo nos enfocaremos en la conexión de peticiones desde un microservicio para hacer pedidos de productos (DsLine.Orders.Services.Api) para el “inquilino X”  a otro microservicios de stock para validar y actualizar el stock (DsLine.Stock.Services.Api) para el mismo inquilino.

**Comencemos**:
Para poder dividir nuestras colas y mantener la seguridad entre nuestros inquilinos RabbitMQ cuenta con “Host Virtuales” los cuales nos proporcionan una agrupación lógica y separación de recursos.
Para poder setear directorios virtuales para el inquilino1 (tenan1) y el inquilino2 (tenant2) dentro de RabbitMQ se agregó una carpeta con el nombre “rabbitmq” en donde se puede visualizar todas las configuraciones. Dicha carpeta es un volumen de docker el cual va a poder ejecutar cuando corramos el comando 
“Docker-compose” y crear estas configuraciones

**NOTA**:  En caso de querer ejecutar estas configuraciones a mano lo podemos hacer con los siguientes comandos.

~~~csharp
rabbitmqctl add_vhost tenant1
rabbitmqctl add_vhost tenant2
~~~

Luego  vamos a agregar dos usuarios uno de ellos para el inquilino1 (Username:usertenant1  Password:usertenant1) y otro para el inquilino2 (Username:usertenant2  Password:usertenant2) a través de los comandos 

~~~csharp
rabbitmqctl add_user  usertenant1 usertenant1
rabbitmqctl add_user  usertenant2 usertenant2
~~~

A continuación debemos asociar el  nuestro  “usertenant1“al host virtual “tenant1” y lo mismo para nuestro “usertenant2” para el host “tenant2”

~~~csharp
rabbitmqctl set_permissions -p tenant1 usertenant1 ".*" ".*" ".*"
rabbitmqctl set_permissions -p tenant2 usertenant2 ".*" ".*" ".*"
~~~

Todos los pasos que realizamos con anterior se puede llevar a cabo desde la interfaz web. Para poder acceder a ella debemos habilitar dicha interfaz y luego ir a  la ruta http://localhost:15672 

{:.message}


Lo primero desde mi punto de vista, es no reinventar la rueda, es por eso que partí desde un proyecto desarrollado por DevsMentors el cual vengo siguiendo desde hace un tiempo. Este proyecto no cuenta con multiples inquilinos, por lo que tuve que hacer un fork para poder adaptarlo a mis requerimientos.

Nota: Si estás viendo esto por microservicios, recomiendo ver los videos de DevsMentors.
https://devmentors.io/

En la solución encontraremos 2 proyectos Api en los cuales podemos visualizar que en sus archivos “appsettings.Development.json” se configuraron las distintas conexiones a las bases de datos de cada tenant en la sección “tenants” y por otra parte una lista de conexiones para RabbitMQ en la sección “ListRabbitMq”. Luego en el archivo StartUp.cs
Encontraremos la subscripción al evento “UpdateStockEvent” para ambos tenants (No es la mejor implementación, ya que al agregar muchos tenants se torna engorroso, pero por ahora nos sirve).

Para demostrar su funcionamiento vamos a llamar desde Postman al microservicio “DsLine.Orders.Services.Api”  simulando un nuevo pedido de items para el inquilino tenant1. Para poder verificar las trazas de logs entre los microservicios y el acceso a las cada base de datos para los distintos tenants utilizamos OpenTracing junto a JAEGER. En las siguientes imágenes podemos ver los llamados desde Postman y las trazas correspondientes para los distintos tenants.


**NOTA**:   Para poder diferenciar usuarios del inquilino 1 o inquilino 2 podríamos enviar un token a través de la cola de RabbitMq, pero no vamos a  hacer énfasis en este post sobre como diferenciar a cada usuario de forma más segura. Prometo en próximos post profundizar sobre este tema, por ahora nos bastará con un header con “Authorization:tenant1”.
{:.message}

**Llamado desde tenant1**

![Screenshot](/assets/img/PostmanTenant1.png)

![Screenshot](/assets/img/RegistroLogsTenant1.png)

**Llamado desde tenant2**

![Screenshot](/assets/img/PostmanTenant2.png)


![Screenshot](/assets/img/RegistrosLogsTenant2.png)

Podes encontrar este ejemplo en  [GitHub](https://github.com/DsLine/DsLine.RabbitMQMultitenantDB)
