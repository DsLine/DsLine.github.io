---
layout: entrevistas
title: Entrevistas CSharp
description: >
  Este post fue realizado con preguntas que me realizaron en algunas entrevistas y otras que les realizaron a algunos amigos. El objetivo de este post es refrescar algunos conocimientos que vamos perdiendo y por otra parte si es que nunca escuchaste algún de estos conceptos que los puedas aprender e implementar en tu trabajo.
  Tuve la suerte de tener varias entrevista y poder realizar yo algunas entrevistas, creo que lo más importante al margen del puesto a aspirar es poder llevarte algún aprendizaje nuevo, tanto del lado del entrevistado como del entrevistador.
  Espero que les sirva. 
author: author1
noindex: true
---

### Entrevistas CCSharp

### Soporta CCSharp herencia múltiple?
No. Una clase solo puede heredar de una sola clase padre. Sin embargo, puede implementar múltiples interfaces.

### Cuál es la clase base de la cual heredan todas las demás en .NET?
Object: Admite todas las clases de la jerarquía de clases de .NET Framework y proporciona servicios de bajo nivel a las clases derivadas. Se trata de la clase base fundamental de todas las clases de .NET Framework; es la raíz de la jerarquía de tipos.

### Qué colección de clases permite acceder a los elementos usando una clave única?
ListDictionary: Si. Implementación de IDictionary a través de una lista de nodos. Útil si el número de elementos es menor a 10. Cada elemento de la colección es un elemento de tipo DictionaryEntry y consiste en un par clave/valor.
Stack: No. Es una colección de objetos LIFO(último en entrar, primero en salir) o pila de objetos. Acepta valores nulos o repetidos. 
Hashtable: Si. Representa una colección de pares clave/valor que se organizan basados en el hashcode de la clave. Cada elemento de la colección es un elemento de tipo DictionaryEntry. 
ArrayList: No. Implementación de la interfaz IList. Conviene usar como alternativa una lista tipada List<T> ya que mejora la performance.
StringCollection: No. Colección de strings. Acepta valores nulos y duplicados. 

### Cuando se ejecuta el bloque finally en un try-catch?
Se ejecuta siempre. Independientemente de si se lanzó una excepción o el código terminó con su ejecución normal.

### Se pueden declarar múltiples catch para un único bloque try?
Si, es posible usar más de una clausula catch para un bloque try. En este caso el orden de los catch es importante. Siempre se deben capturar las excepciones más específicas primero. En caso contrario, se produce un error de compilación.



### Qué palabra clave previene a una clase ser heredable?
Sealed.
Ej: La clase B hereda de A, pero ninguna clase puede heredar de B. 
class A {}
sealed class B : A {}

También se puede utilizar el modificador sealed en un método o property que sobreescribe un método o propiedad virtual de una clase base. 

### Cuándo se utiliza una clase abstracta?
Una clase abstracta no se instancia nunca. Es una clase que únicamente se utiliza como clase base de otras clases. 
Una clase abstracta no puede marcarse como sealed porque ambos modificadores tienen significados opuestos. 
Todas las clases que heredan de una clase abstracta deben incluir la implementación de todos los métodos abstractos heredados.

Los métodos abstractos sólo se pueden declarar dentro de una clase abstracta e implícitamente son métodos virtuales. 
public abstract void MyMethod();

Una clase abstracta que implementa una interfaz debe mapear los métodos de la interfaz como métodos abstractos.
Ej:
	interface I
{
	void M();
}
abstract class C : I
{
	public abstract void M();
}

### Permite .Net implementar múltiples interfaces?
Si


### Cuál es la diferencia entre una interfaz y una clase abstracta.
Mediante las interfaces se puede especificar los métodos que debe implementar un componente sin especificar realmente cómo se implementa el método. Las clases abstractas permiten crear definiciones del comportamiento al mismo tiempo que proporcionan implementación común para las clases heredadas.

Una clase puede heredar de una única clase abstracta pero implementar múltiples interfaces. 

Ambas permiten implementar un comportamiento polimórfico en sus componentes.
El polimorfismo es la capacidad que tienen las clases de proporcionar diferentes implementaciones de métodos a los que se denomina con un mismo nombre. 
Por ejemplo, un componente puede utilizar un método de una interfaz que puede redefinirse en cada clase que implementa esta interfaz. (Polimorfismo de Interfaz).

Puesto que las clases abstractas también pueden proporcionar propiedades y métodos ya implementados. Se puede garantizar una cierta magnitud de funcionalidad idéntica con una clase abstracta, pero no con una interfaz, que simplemente se entiende como la definición de un contrato de comportamiento.

### Para qué se utiliza la palabra clave virtual en la declaración de un método o propiedad?
Se puede modificar un método, propiedad o declaración de evento con la palabra virtual. Esto le permite ser sobreescrito en una clase derivada. 
Ej:
public virtual double Area()
{
	return x*y;
}
En tiempo de ejecución, el objeto instanciado ejecuta el método virtual. Si dicha clase tiene una sobreescritura del método virtual padre, se ejecuta este bloque de código. Si por el contrario, el objeto instanciado no sobreescribe el método virtual de la clase padre, ese bloque de código es el que se ejecuta.
Para sobreescribir un método virtual en una clase hija se utiliza la palabra override.
Por defecto, todos los métodos son no-virtuales y por ende no pueden ser sobreescritos a menos que se identifiquen explícitamente como virtuales.

La palabra reservada virtual no se puede utilizar con los modificadores static, abstract, private u override.

### Qué diferencias hay entre overriding y overloading?
Overriding: Sobreescribir un método de una clase padre en una clase hija. Para poder ser sobreescrito el método debe estar explícitamente declarado como virtual en la clase base y declarado como override en la clase hija. Si la clase hija no tiene una sobreescritura del método, al momento de ejecutarse se tomará el bloque de código definido en la clase base.

Overloading: Sobrecarga de un método. En una misma clase un método tiene el mismo nombre pero puede ser definido varias veces con distintos parámetros. Cada definición del método es una sobrecarga.

### Qué es un delegado?
Un delegado es una variable de tipo de referencia que contiene la referencia a un método. La referencia se puede cambiar en tiempo de ejecución.
Los delegados se utilizan especialmente para implementar eventos y los métodos de devolución de llamada. Todos los delegados se derivan implícitamente de la clase System.Delegate .
Declarando delegados
La declaración del delegado determina los métodos a los que el delegado puede hacer referencia. Un delegado puede referirse a un método, que tiene la misma firma que la del delegado.
Por ejemplo, considere un delegado -
public delegate int MyDelegate (string s);


El delegado anterior se puede usar para hacer referencia a cualquier método que tenga un único parámetro de cadena y devuelve una variable de tipo int .
La sintaxis para la declaración del delegado es -
delegate <return type> <delegate-name> <parameter list>


Instanciando Delegados
Una vez que se declara un tipo de delegado, se debe crear un objeto delegado con la palabra clave nueva y asociarlo con un método en particular. Al crear un delegado, el argumento pasado a la nueva expresión se escribe de forma similar a una llamada a un método, pero sin los argumentos para el método. Por ejemplo -
public delegate void printString(string s);
...
printString ps1 = new printString(WriteToScreen);
printString ps2 = new printString(WriteToFile);


El siguiente ejemplo demuestra declaración, creación de instancias y uso de un delegado que se puede usar para hacer referencia a métodos que toman un parámetro entero y devuelve un valor entero.
 Demo en vivo
using System;

delegate int NumberChanger(int n);
namespace DelegateAppl {
   
   class TestDelegate {
      static int num = 10;
      
      public static int AddNum(int p) {
         num += p;
         return num;
      }
      public static int MultNum(int q) {
         num *= q;
         return num;
      }
      public static int getNum() {
         return num;
      }
      static void Main(string[] args) {
         //create delegate instances
         NumberChanger nc1 = new NumberChanger(AddNum);
         NumberChanger nc2 = new NumberChanger(MultNum);
         
         //calling the methods using the delegate objects
         nc1(25);
         Console.WriteLine("Value of Num: {0}", getNum());
         nc2(5);
         Console.WriteLine("Value of Num: {0}", getNum());
         Console.ReadKey();
      }
   }
}
Cuando se compila y ejecuta el código anterior, produce el siguiente resultado:
Value of Num: 35
Value of Num: 175

### Qué namespaces se requieren para crear una aplicación localizada?
System.Globalization

### Qué diferencia hay entre un delegado y un evento?
Una declaración de delegado define un TIPO que encapsula un método con un determinado conjunto de argumentos y un tipo de retorno. 
Para métodos estáticos, el objeto delegado encapsula el método al que se debe llamar. Para métodos de instancia, el objeto delegado encapsula tanto una instancia como su método de instancia. Es importante destacar que el delegado no necesita conocer la clase del objeto que encapsula, simplemente basta con que la firma del método corresponda a la del delegado.
Los delegados se pueden pasar por parámetros en los métodos.
Los delegados que son del mismo tipo se pueden componer mediante el operador “+”. Un delegado compuesto llama a los dos delegados de los que se compone. El operador “-” se puede utilizar para quitar un delegado componente de un delegado compuesto.

Un evento es un mensaje o notificación que envía un objeto cuando ocurre una acción. Esta acción puede estar causada por la interacción con el usuario o por otra lógica del programa.
Los eventos se declaran mediante delegados. El delegado encapsula un método de modo que se pueda llamar de forma anónima. Un evento es el modo que tiene una clase de permitir a los clientes proporcionar delegados a los métodos para llamarlos cuando se produce el evento. Cuando ocurre el evento, se llama a los delegados que proporcionan los clientes para el evento.

El delegado permite vincular el evento que se dispara con el método que se ejecuta en cada cliente que lo escucha.

### Qué es una clase parcial. Qué ventajas tiene?
Una única clase que está separada en varios archivos. Todas las partes se combinan cuando la aplicación se compila.
Se utiliza la palabra clave Partial. Todas las clases parciales tienen que tener el mismo nivel de accesibilidad (público, privado, etc.).

Ventajas:
	-En proyectos grandes, ayuda a organizar el código de clases muy grandes.
	-Al trabajar con código autogenerado (Windows forms, Web services wrapper, etc) Visual Studio crea clases parciales. De esta manera se puede añadir código propio a esas clases sin tener que modificar los archivos generados por el visual studio.


### A que objeto .Net está asociada la palabra clave “int”?
-System.Int16
#### -System.Int32
-System.Int64
-System.Int128

### Cómo se declara correctamente un arreglo de enteros de dos dimensiones?
-int [,] myArray;
#### -int[][] myArray;
-int[2] myArray;
-System.Array[2] myArray;
-System.Array[,] myArray;

### Si un método está marcado como protected internal, quién puede acceder a él?
Protected: Accesible dentro de la misma clase y por instancias de clases derivadas (que hereden de la clase base que contiene el método marcado como protected)
Internal: El acceso está limitado a archivos dentro del mismo assembly.

