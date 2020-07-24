---
layout: entrevistas
title: Entrevistas Programación
description: >
  Este post fue realizado con preguntas que me realizaron en algunas entrevistas y otras que les realizaron a algunos amigos. El objetivo de este post es refrescar algunos conocimientos que vamos perdiendo y por otra parte si es que nunca escuchaste algún de estos conceptos que los puedas aprender e implementar en tu trabajo.
  Tuve la suerte de tener varias entrevista y poder realizar yo algunas entrevistas, creo que lo más importante al margen del puesto a aspirar es poder llevarte algún aprendizaje nuevo, tanto del lado del entrevistado como del entrevistador.
  Espero que les sirva.
author: author1
noindex: true
---

### Entrevistas Programación

###  Qué es el polimorfismo?
El Polimorfismo permite proporcionar diferentes implementaciones de un mismo método. 
Se pude lograr mediante el uso de interfaces o la herencia de clases. Por ejemplo, una clase base puede definir e implementar métodos virtuales y sus clases derivadas sobreescribir estos métodos, lo que significa que proveen su propia implementación. En tiempo de ejecución, la clase cliente llama a un método de la clase base y se ejecuta el código correspondiente a la clase hija instanciada (Polimorfismo por herencia).

En CSharp los métodos y propiedades virtuales se pueden sobreescribir varias veces en las distintas clases derivadas. 
Ej:       
public class A
{
	public virtual void DoWork();
}
public class B : A
{
	public override void DoWork();
}
public class C : B
{
	public sealed override void DoWork();
}

Las clase A define un método virtual. La clase B lo sobreescribe. La clase C también lo sobreescribe y a su vez lo sella, de esta manera el método ya no se puede sobreescribir para ninguna clase derivada de C.

### Patrón Singleton:
Una clase que se instancia una sola vez. Se provee un punto de acceso global a dicha instancia.
Implementación sencilla 

using System;

public class Singleton
{
   private static Singleton instance;

   private Singleton() {}

   public static Singleton Instance
   {
      get 
      {
         if (instance == null)
         {
            instance = new Singleton();
         }
         return instance;
      }
   }
}


### Patrón Builder:
Como Patrón de diseño, el patrón builder (Constructor) es usado para permitir la creación de una variedad de objetos complejos desde un objeto fuente (Producto), el objeto fuente se compone de una variedad de partes que contribuyen individualmente a la creación de cada objeto complejo a través de un conjunto de llamadas secuenciales a una implementación específica que extienda la clase Abstract Builder. Así, cada implementación existente de Abstract Builder construirá un objeto complejo Producto de una forma diferente deseada.

El patrón builder es creacional.

A menudo, el patrón builder construye el patrón Composite, un patrón estructural.

Intención: Abstrae el proceso de creación de un objeto complejo, centralizando dicho proceso en un único punto, de tal forma que el mismo proceso de construcción pueda crear representaciones diferentes.[Wikipedia](https://es.wikipedia.org/wiki/Builder_(patr%C3%B3n_de_dise%C3%B1o))

### Patrón Facade:
Fachada (Facade) es un tipo de patrón de diseño estructural. Viene motivado por la necesidad de estructurar un entorno de programación y reducir su complejidad con la división en subsistemas, minimizando las comunicaciones y dependencias entre estos.
Se aplicará el patrón fachada cuando se necesite proporcionar una interfaz simple para un subsistema complejo, o cuando se quiera estructurar varios subsistemas en capas, ya que las fachadas serían el punto de entrada a cada nivel. Otro escenario proclive para su aplicación surge de la necesidad de desacoplar un sistema de sus clientes y de otros subsistemas, haciéndolo más independiente, portable y reutilizable (esto es, reduciendo dependencias entre los subsistemas y los clientes).[Wikipedia](https://es.wikipedia.org/wiki/Facade_(patr%C3%B3n_de_dise%C3%B1o))

### Qué es encapsulamiento?
El encapsulamiento consiste en agrupar variables, propiedades y métodos en una unidad, es decir, en una Clase. 

Puntos importantes: 
-La clase esconde los detalles internos de cómo el objeto realiza una acción.
-La clase especifica el nivel de acceso que tienen sus miembros (variables, propiedades o métodos) 
-Se puede modificar la implementación interna dentro de una clase sin tener que modificar el resto de las clases del sistema.

### Qué son las expresiones lambda?
Una expresión lambda es una función anónima que suele utilizarse para crear delegados en LINQ. Una función anónima es un método sin una declaración, es decir:  modificador de acceso, declaración de valor de retorno y nombre de función.
Como ventaja, es un atajo para escribir un método exactamente en el lugar donde va a ser utilizado, sin necesidad de declararlo y escribirlo como un método aparte en alguna clase.
La definición de una expresión lambda es:
	“Parámetros => Código a Ejecutar”

 ### ¿Que son los Web Services?
Un servicio web (en inglés, web service o web services) es una tecnología que utiliza un conjunto de protocolos y estándares que sirven para intercambiar datos entre aplicaciones. Distintas aplicaciones de software desarrolladas en lenguajes de programación diferentes, y ejecutadas sobre cualquier plataforma, pueden utilizar los servicios web para intercambiar datos en redes de ordenadores como Internet. La interoperabilidad se consigue mediante la adopción de estándares abiertos. Las organizaciones OASIS y W3C son los comités responsables de la arquitectura y reglamentación de los servicios Web. Para mejorar la interoperabilidad entre distintas implementaciones de servicios Web se ha creado el organismo WS-I, encargado de desarrollar diversos perfiles para definir de manera más exhaustiva estos estándares. Es una máquina que atiende las peticiones de los clientes web y les envía los recursos solicitados.

WCF (Windows comunication fundation) transmite por soap.
API REST (Transmite por protocolo http y sus estandares)

 ### ¿Cuales de las siguientes opciones no forman parte de POO?
 -Abstracción
-Polimorfismo
-Hilos
-Abstracto

