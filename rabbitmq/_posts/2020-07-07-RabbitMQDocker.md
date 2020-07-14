---
layout: post
title: RabbitMQ en Docker
description: >
  Como agregar RabbitMQ a un contenedor en Docker 
author: author1
noindex: true
---

### RabbitMQ en Docker

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
{:.message}

Para verificar que el contenedor esté corriendo correctamente, ingresen a http://localhost:15672/ con usuario y contraseña guest/ guest. 

**NOTA**: Este usuario es cargado por defecto, pero dicho usuario no puede utilizarse en entornos productivos, ya que está restringido para conexiones externas.
Si desea cambiar el nombre de usuario y la contraseña predeterminados, puede hacerlo con las variables ambientales RABBITMQ_DEFAULT_USERy RABBITMQ_DEFAULT_PASS
{:.message}

Para verificar en docker que este corriendo la imagen, podemos abrir una consola de Windows Powershell  y ejecutar el siguiente comando
~~~csharp
docker ps
~~~
![Screenshot](/assets/img/RabbitMQEnDockerWPS.png)


### Pruebas en NetCore 3.1
Comencemos abriendo tu IDE preferido (En mi caso voy a utilizar visual studio 2019), crear una nueva solución en blanco, junto a dos proyectos de consola en NetCore 3.1 uno para enviar mensajes “DsLine.RabbitMqEnDocker.Enviar” y otro para recibir mensajes “DsLine.RabbitMqEnDocker.Recibir”.
Luego instalar la librería de RabbitMQ.Client  desde nuget en ambos proyectos (Enviar y Recibir).

Versión resumida CLI.
{:.message}

~~~csharp
dotnet new console --name Enviar
mv Send/Program.cs Send/Enviar.cs
dotnet new console --name Recibir
mv Receive/Program.cs Receive/Recibir.cs

cd Enviar
dotnet add package RabbitMQ.Client
dotnet restore
cd ../Recibir
dotnet add package RabbitMQ.Client
dotnet restore
~~~

Dentro de nuestro proyecto "Enviar"
Agregar el siguiente código.

~~~csharp
using System;
using RabbitMQ.Client;
using System.Text;

class ​Send
{
   ​public static void Main()
   ​{
       ​var factory = new ConnectionFactory() { HostName = "localhost" };
       ​using(var connection = factory.CreateConnection())
       ​using(var channel = connection.CreateModel())
       ​{
           ​channel.QueueDeclare(queue: "hello",
                                ​durable: false,
                                ​exclusive: false,
                                ​autoDelete: false,
                                ​arguments: null);

           ​string message = "Hello World!";
           ​var body = Encoding.UTF8.GetBytes(message);

           ​channel.BasicPublish(exchange: "",
                                ​routingKey: "hello",
                                ​basicProperties: null,
                                ​body: body);
           ​Console.WriteLine(" [x] Sent {0}", message);
       ​}

       ​Console.WriteLine(" Press [enter] to exit.");
       ​Console.ReadLine();
   ​}
}
~~~


Dentro de nuestro proyecto "Recibir"
Agregar el siguiente código.

~~~csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System;
using System.Text;

class Receive
{
    public static void Main()
    {
        var factory = new ConnectionFactory() { HostName = "localhost" };
        using(var connection = factory.CreateConnection())
        using(var channel = connection.CreateModel())
        {
            channel.QueueDeclare(queue: "hello",
                                 durable: false,
                                 exclusive: false,
                                 autoDelete: false,
                                 arguments: null);

            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) =>
            {
                var body = ea.Body.ToArray();
                var message = Encoding.UTF8.GetString(body);
                Console.WriteLine(" [x] Received {0}", message);
            };
            channel.BasicConsume(queue: "hello",
                                 autoAck: true,
                                 consumer: consumer);

            Console.WriteLine(" Press [enter] to exit.");
            Console.ReadLine();
        }
    }
}
~~~

![Screenshot](/assets/img/RabbitMQEnDockerPruebas.png)