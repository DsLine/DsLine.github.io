---
layout: post
title: Atacando el BackEnd
description: >
  Como agregar RabbitMQ a un contenedor en Docker 
author: author1
noindex: true
---

### Atacando el BackEnd

### Compresión HTTP.

Es una funcionalidad que se puede integrar en servidores web y en clientes web para mejorar la velocidad de transferencia y ancho el ancho de banda. Los esquemas de compresión más común y que yo tuve la oportunidad de utilizar  son Gzip y Deflate (Por supuesto que existen muchos más)
Un tema importante es determinar el tamaño de los archivos a comprimir (aclaración importante cuando me refiero archivos estoy hablando de datos a transferir por medio de http), ya que no tiene sentido gastar recursos (más que nada CPU) en comprimir archivos muy pequeños que si o si se van a transferir de manera rápida. 

Para este caso realicé un pequeño proyecto en NetCore 3.1 para demostrar dicho funcionamiento. En el primero imagen se puede visualizar el primer llamado sin comprimir y el segundo ya comprimido.

Podes encontrar este ejemplo en  [GitHub](https://github.com/DsLine/DsLine.Performance)

~~~csharp
  public void ConfigureServices(IServiceCollection services)
        {

            services.AddControllers();
            //These lines
            services.AddResponseCompression(options =>
            {
                options.EnableForHttps = true;
                options.Providers.Add<GzipCompressionProvider>();
            });


        }


        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            //This line
            app.UseResponseCompression();

            app.UseHttpsRedirection();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
    }
~~~

**NOTA**: En esta primer imagen, podemos visualizar un resultado sin comprimir 
{:.message}

![Screenshot](/assets/img/DsLine.Performance.NotCompression.png)

**NOTA**: En esta segunda imagen, podemos ver el resultado al comprimir con GZip
{:.message}

![Screenshot](/assets/img/DsLine.Performance.Compression.png)


### Pretty view JSON.
Puede ser una tontería, pero siempre tener la precaución de sacar las vistas amigables de  nuestros JSON al pasar a un entorno que no sea de DESARROLLO. Ya que los espacios en blancos y fines de líneas agregan una carga mínima de datos que no son requeridos.
Por suerte para nosotros en las nuevas versiones de NetCore este valor está configurado por defecto para optimizar nuestras consultas.
Para poder mostrar este caso en el ejemplo anterior le agregué la siguiente linea para forzar una vista amigable en nuestros entornos de desarrollo


~~~csharp
   services.AddControllers()
            .AddJsonOptions(options => options.JsonSerializerOptions.WriteIndented = _environment.IsDevelopment());
~~~


![Screenshot](/assets/img/DsLine.Performance.CompressionWriteIndented.png)