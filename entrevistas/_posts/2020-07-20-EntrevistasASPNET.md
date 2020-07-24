---
layout: entrevistas
title: Entrevistas ASP.NET
description: >
  Este post fue realizado con preguntas que me realizaron en algunas entrevistas y otras que les realizaron a algunos amigos. El objetivo de este post es refrescar algunos conocimientos que vamos perdiendo y por otra parte si es que nunca escuchaste algún de estos conceptos que los puedas aprender e implementar en tu trabajo.
  Tuve la suerte de tener varias entrevista y poder realizar yo algunas entrevistas, creo que lo más importante al margen del puesto a aspirar es poder llevarte algún aprendizaje nuevo, tanto del lado del entrevistado como del entrevistador.
  Espero que les sirva.
author: author1
noindex: true
---

### RabbitMQ en Docker

### ¿Qué es el Web.config?
Web.config es el archivo principal de opciones de configuración para una aplicación web en ASP.NET. El archivo es un documento XML que define información de configuración concerniente a la aplicación web. El archivo web.config contiene información que controla la carga de módulos, configuraciones de seguridad, configuraciones del estado de la sesión, opciones de compilación y el lenguaje de la aplicación. Los archivos web.config pueden contener también objetos específicos tales como cadenas de conexión a la base de datos.
<!-- [Wikipedia](https://es.wikipedia.org/wiki/Web.config) -->

### ¿Qué es el Global.asax?
El global.asax es el archivo de aplicación para todo proyecto ASP.NET, se encuentra en la raíz del proyecto. En él se declaran todos los eventos a nivel de aplicación como el inicio y fin de la misma, las peticiones web, etc... Es la evolución del antiguo global.asa existente en entornos de ASP clásico.

Se compila cada vez que inicia la aplicación. Cualquier modificación del archivo es detectada y origina el reinicio de la misma.[tuprogramacion](http://www.tuprogramacion.com/programacion/que-es-el-global-asax-y-como-funciona/)

### ¿Cual es el concepto de Posback?
El postaback implica la comunicacion del cliente con el servidor cuando se lanza un evento de un control de asp.net
en si mismo el postback no mantiene la informacion de los controles, es justamente el viewstate el que lo hace
cuando se invoca una pagina mediente una accion de postback en un evento de un control, este ejecuta todo el ciclo de vida de esa pagina, incluido el Page_Load, es por eso que existe el IsPostBack

pudiendo hacer

private void Page_Load(...){

   if(!IsPostBack){

      //aqui trabjas con la carga inicial de los controles

   }

}

Cuando una pagna se carga la primera vez el IsPosBack sera false, solo cuando se invoca a la pagina ante un evento de un control es que el IsPostBack sera true, ya que como comente este implica una llamada al servidor ante un evento de un control.
 <!-- [MicrosoftSocial](https://social.msdn.microsoft.com/Forums/es-ES/6654992b-a112-496d-a623-e0af352a5bbf/acerca-de-postback?forum=netfxwebes) -->

### ¿Que es StateView?
es  donde la pagina conserva los valores de los controles

### ¿Qué son los modos de estado de sesión en ASP.NET?
El estado de la sesión ASP.NET admite varias opciones de almacenamiento diferentes para los datos de la sesión. Cada opción se identifica mediante un valor en la enumeración SessionStateMode . La siguiente lista describe los modos de estado de sesión disponibles:

*Modo InProc, que almacena el estado de la sesión en la memoria del servidor web. Este es el valor predeterminado.

*Modo StateServer, que almacena el estado de la sesión en un proceso separado llamado servicio de estado ASP.NET. Esto garantiza que se conserva el estado de la sesión si se reinicia la aplicación web y también hace que el estado de la sesión esté disponible para varios servidores web en una granja de servidores web.

*El modo SQLServer almacena el estado de la sesión en una base de datos de SQL Server. Esto garantiza que se conserva el estado de la sesión si se reinicia la aplicación web y también hace que el estado de la sesión esté disponible para varios servidores web en una granja de servidores web.

*Modo personalizado, que le permite especificar un proveedor de almacenamiento personalizado.

*Modo apagado, que deshabilita el estado de la sesión.

