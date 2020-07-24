---
layout: post
title: Entrevistas SQL Junior
description: >
  Este post fue realizado con preguntas que me realizaron en algunas entrevistas y otras que les realizaron a algunos amigos. El objetivo de este post es refrescar algunos conocimientos que vamos perdiendo y por otra parte si es que nunca escuchaste algún de estos conceptos que los puedas aprender e implementar en tu trabajo.
  Tuve la suerte de tener varias entrevista y poder realizar yo algunas entrevistas, creo que lo más importante al margen del puesto a aspirar es poder llevarte algún aprendizaje nuevo, tanto del lado del entrevistado como del entrevistador.
  Espero que les sirva.
author: author1
noindex: true
---

### Entrevistas SQL Junior

### Instrucciones SQL que permiten manipular datos en una BD.

#### Select: 
Seleccionar datos de una tabla o un conjunto de tablas
SELECT column_name,column_name
FROM table_name;

El simbolo * trae todas las columnas de la tabla.
SELECT * FROM table_name;

La palabra clave distinct permite traer sólo una vez los valores repetidos en cada columna.
SELECT DISTINCT column_name,column_name
FROM table_name;

La clausula SELECT TOP permite indicar específicamente el número de registros a traer desde la base de datos. (Mejora de performance en registros con gran cantidad de datos)
SELECT TOP number|percent column_name(s)
FROM table_name;


#### Where:
Se filtran registros de la tabla según la condición.
SELECT * FROM Customers
WHERE Country='Mexico';

Los operadores AND y OR permiten anidar condiciones.
SELECT * FROM Customers
WHERE Country='Germany'
AND City='Berlin';

El operador LIKE permite buscar un patrón específico en una columna.
Ej: Todos los clientes cuyas ciudades arrancan con la letra s.
SELECT * FROM Customers
WHERE City LIKE 's%';



#### Wildcard
Representa
%
_
0 o muchos caracteres.
1 solo carácter.
[charlist]
Un rango específico de caracteres
[!charlist]
Excluye un rango específico de caracteres.

Ej: Todos los clientes cuyas ciudades arranquen con cualquier carácter seguidos por erlin.
SELECT * FROM Customers
WHERE City LIKE '_erlin';

Ej: Ciudades que comienzan con ‘b’, ‘s’ o ‘p’. 
SELECT * FROM Customers
WHERE City LIKE '[bsp]%';

Ej: Ciudades que comienzan con ‘a’, ‘b’ o ‘c’.
SELECT * FROM Customers
WHERE City LIKE '[a-c]%';

Ej: Ciudades que no comienzan con ‘b’, ‘s’ o ‘p’.
SELECT * FROM Customers
WHERE City LIKE '[!bsp]%';

El operador IN permite especificar múltiples valores en una clausula where.
SELECT * FROM Customers
WHERE City IN ('Paris','London');

El operador BETWEEN se utiliza para seleccionar valores dentro de un rango.
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;



#### Order by:
Se ordenan los registros en orden ascendente ASC (por defecto)  o descendente DESC.
SELECT * FROM Customers
ORDER BY Country DESC;

#### SQL Join:
Una clausula SQL Join permite combinar filas de dos o más tablas, las cuales están relacionadas a partir de un campo en común.

SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID=Customers.CustomerID;

#### Insert into:
Se insertan nuevos registros en una tabla.
Si no se especifican las columnas, se insertan los valores por orden de las columnas.
INSERT INTO table_name
VALUES (value1,value2,value3,...);

Si se especifican los nombres de columnas se insertan los valores en la columna correspondiente.
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');

#### UPDATE:
Se actualizan registros de una tabla en base a una condición.
UPDATE Customers
SET ContactName='Alfred Schmidt', City='Hamburg'
WHERE CustomerName='Alfreds Futterkiste';

#### DELETE:
Se borran registros de una tabla.
Si no se especifica la condición se borran todos los registros de la tabla.
DELETE FROM table_name
WHERE some_column=some_value;



### 2.Diferencias entre cláusulas Having y Where.

Una cláusula Having es similar a una cláusula Where, pero solo se aplica a los grupos en su totalidad (es decir, a las filas que se obtienen como resultado del agrupamiento previo realizado a partir de un Group by), a diferencia de la cláusula Where, que se aplica a filas individuales.

Una consulta puede contener tanto una cláusula Where como una cláusula Having.
En tal caso:
La cláusula Where se aplica primero a las filas individuales de las tablas. Sólo se agrupan las filas que cumplen las condiciones del where.
La cláusula Having se aplica a continuación a las filas del conjunto de resultados. Sólo aparecen en el resultado de la consulta los grupos que cumplen las condiciones Having. Sólo se puede aplicar una cláusula Having a las columnas que también aparecen en la cláusula Group By o en una función de agregado.

Ej:
SELECT titles.pub_id, AVG(titles.price)
FROM titles INNER JOIN publishers
   ON titles.pub_id = publishers.pub_id
WHERE publishers.state = 'CA'
GROUP BY titles.pub_id
HAVING AVG(price) > 10

Se combinan las filas de las tablas Titles y Publishers.
Se filtran todas aquellas filas cuyos publishers.state = ‘CA’.
Se agrupan las filas individuales por el identificador de publishers.
Se filtran los grupos de filas de publishers por aquellos cuyo precio promedio de Titles es mayor a 10.  



### Procedimientos almacenados.

Un procedimiento almacenado de SQL Server es un conjunto de una o varias instrucciones T-SQL almacenado físicamente en la base de datos. Su principal ventaja radica en que al ser ejecutado, en respuesta a una petición de usuario, es ejecutado directamente en el motor de bases de datos.
Otra ventaja importante es la reutilización de código: El código de cualquier operación de base de datos redundante resulta un candidato perfecto para la encapsulación de procedimientos.

El procedimiento almacenado permite:
Aceptar parámetros de entrada y devolver valores de salida al programa que realiza la llamada.
Contener instrucciones que realicen operaciones en la base de datos (incluso llamadas a otros procedimientos almacenados)
Devolver un valor de estado a un programa que realiza la llamada para indicar si la operación fue realizada correctamente o se produjeron errores.


Creación de un procedimiento almacenado mediante una query:

USE AdventureWorks2012;
GO
CREATE PROCEDURE HumanResources.uspGetEmployeesTest2 
    @LastName nvarchar(50), 
    @FirstName nvarchar(50) 
AS 

    SET NOCOUNT ON;
    SELECT FirstName, LastName, Department
    FROM HumanResources.vEmployeeDepartmentHistory
    WHERE FirstName = @FirstName AND LastName = @LastName
    AND EndDate IS NULL;
GO

Ejecución del procedimiento:

EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';
GO



### Diferencias entre Procedimientos almacenados y Trigger.

Un procedimiento almacenado es una rutina programada en SQL y que realiza cierta tarea o conjunto de tareas con una o más tablas para realizar una secuencia repetitiva de acciones. El SP se invoca recibiendo un conjunto de parámetros de inicio y salida. Es una forma eficiente de tratar ciclos repetitivos de operaciones, barridas de tablas registro a registro, cálculos y búsquedas de cierta complejidad, etc.

El trigger es una rutina almacenada VINCULADA A UNA SOLA TABLA, y que se ejecuta únicamente y en todos los casos en que se de un EVENTO DETERMINADO DE ESA TABLA. Los eventos básicos son Insert, Update y Delete. Se usa principalmente para realizar tareas que afectan a esa tabla en cada ocurrencia de ese evento.

Por ejemplo: Se quiere realizar una inserción de un valor en el campo A de una tabla automáticamente siempre que se inserte un valor 0 en el campo B.

CREATE TRIGGER TRG_01 ON TABLA1 BEFORE INSERT FOR EACH ROW
BEGIN
IF NEW.B=0 THEN SET NEW.A = (SELECT IDCAMPO FROM TABLA2 WHERE ID = NEW.B); 
END


Una diferencia entre ambos es que los procedimientos almacenados pueden tener o no parámetros de entrada, entrada-salida o salida, mientras que los únicos parámetros de un trigger son los campos del registro entrante y son de entrada.
Si bien el trigger puede invocar a otras tablas para realizar su trabajo, el trigger está siempre vinculado a una tabla y solamente una.
Otra diferencia es que un SP se crea para ser invocado en ciertas circunstancias. Un trigger se dispara siempre que se dé la circunstancia programada.




### Que es una transaccion en SQL?
Una transacción es un conjunto secuencial  de cambios a una base de datos. Las transacciones se emplean como mecanismo de petición para bases de datos de multiusuarios

Propiedades de una transacción
Las transacciones deben poseer las siguientes características para ser exitosas e integrales:
Atomicidad : Todas las sentencias dentro de la transacción deben ejecutarse sin errores. Si por algún motivo no se cumplen, la transacción aborta en el punto de ruptura y deshace todo los cambios realizados. Como quien dice “O todo o Nada!”.
Consistencia: Se debe asegurar que todos los cambios en la base de datos sean correctos y estén guardados.
Aislamiento: La transacción debe actuar independientemente de otra, dos transacciones no pueden estar visualizando la misma información a tiempo.
Durabilidad: La transacción debe asegurar que los cambios que hará persistirán aunque el sistema falle.
Control de una transacción
Para dirigir el flujo de una transacción con efectividad y mantener sus propiedades, existen las siguientes sentencias en SQL. La sintaxis y etiqueta dependen de cada gestor de bases de datos:
COMMIT : guarda los cambios.
ROLLBACK: deshace los cambios.
SAVEPOINT: crea un punto de restauración dentro de un conjunto de transacciones para luego deshacer los cambios si es necesario.
