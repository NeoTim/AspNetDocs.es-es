---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Crear un modelo con validaciones de reglas de negocios Microsoft Docs
author: rick-anderson
description: El paso 3 muestra cómo crear un modelo que podemos usar para consultar y actualizar la base de datos de nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 1a316e9051cf56cd4f1546336b334ace991c05b3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541642"
---
# <a name="build-a-model-with-business-rule-validations"></a>Crear un modelo con validaciones de regla de negocios

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 3 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 3 muestra cómo crear un modelo que podemos usar para consultar y actualizar la base de datos de nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner Paso 3: Construyendo el modelo

En un marco de modelo-vista-controlador el término "modelo" hace referencia a los objetos que representan los datos de la aplicación, así como la lógica de dominio correspondiente que integra la validación y las reglas de negocio con ella. El modelo es en muchos sentidos el "corazón" de una aplicación basada en MVC, y como veremos más adelante impulsa fundamentalmente el comportamiento de la misma.

El marco de ASP.NET MVC admite el uso de cualquier tecnología de acceso a datos y los desarrolladores pueden elegir entre una variedad de opciones de datos de .NET enriquecidas para implementar sus modelos, incluidos: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o simplemente ADO.NET DataReaders o DataSets sin procesar.

Para nuestra aplicación NerdDinner vamos a usar LINQ to SQLLINQ to SQL para crear un modelo simple que se corresponda bastante estrechamente con nuestro diseño de base de datos y agregue algunas reglas de negocio y lógica de validación personalizadas. A continuación, implementaremos una clase de repositorio que ayuda a abstraer la implementación de persistencia de datos del resto de la aplicación y nos permite probarla unitariamente.

### <a name="linq-to-sql"></a>LINQ a SQL

LINQ to SQLLINQ to SQL es un ORM (asignador relacional de objetos) que se incluye como parte de .NET 3.5.

LINQ to SQLLINQ to SQL proporciona una manera sencilla de asignar tablas de base de datos a clases .NET con las que podemos codificar. Para nuestra aplicación NerdDinner lo usaremos para asignar las tablas Dinners y RSVP dentro de nuestra base de datos a clases Dinner y RSVP. Las columnas de las tablas Dinners y RSVP corresponderán a las propiedades de las clases Dinner y RSVP. Cada objeto Dinner y RSVP representará una fila independiente dentro de las tablas Dinners o RSVP de la base de datos.

LINQ to SQLLINQ to SQL nos permite evitar tener que construir manualmente instrucciones SQL para recuperar y actualizar objetos Dinner y RSVP con datos de base de datos. En su lugar, definiremos las clases Dinner y RSVP, cómo se asignan a/desde la base de datos y las relaciones entre ellas. LINQ to SQLLINQ to SQL se encargará de generar la lógica de ejecución de SQL adecuada para usar en tiempo de ejecución cuando interactuemos y los usemos.

Podemos usar la compatibilidad con lenguaje LINQ dentro de VB y C- para escribir consultas expresivas que recuperan objetos Dinner y RSVP de la base de datos. Esto minimiza la cantidad de código de datos que necesitamos escribir y nos permite crear aplicaciones realmente limpias.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Agregar LINQ to SQL Classes a nuestro proyecto

Comenzaremos haciendo clic con el botón derecho en la carpeta "Modelos" dentro de nuestro proyecto y seleccionaremos el comando de menú **Agregar&gt;nuevo elemento:**

![](build-a-model-with-business-rule-validations/_static/image1.png)

Aparecerá el cuadro de diálogo "Agregar nuevo elemento". Filtraremos por la categoría "Datos" y seleccionaremos la plantilla "LINQ to SQL Classes" dentro de ella:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Nombraremos el elemento "NerdDinner" y haremos clic en el botón "Añadir". Visual Studio agregará un archivo NerdDinner.dbml en nuestro directorio .Models y, a continuación, abrirá el diseñador relacional de objetos LINQ to SQLLINQ to SQL:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Creación de clases de modelo de datos con LINQ to SQLLINQ to SQL

LINQ to SQLLINQ to SQL nos permite crear rápidamente clases de modelo de datos a partir del esquema de base de datos existente. Para ello, abriremos la base de datos NerdDinner en el Explorador de servidores y seleccionaremos las tablas que queremos modelar en ella:

![](build-a-model-with-business-rule-validations/_static/image4.png)

A continuación, podemos arrastrar las tablas a la superficie del diseñador LINQ to SQLLINQ to SQL. Al hacerlo LINQ to SQLLINQ to SQL creará automáticamente clases Dinner y RSVP utilizando el esquema de las tablas (con propiedades de clase que se asignan a las columnas de la tabla de base de datos):

![](build-a-model-with-business-rule-validations/_static/image5.png)

De forma predeterminada, el diseñador LINQ to SQLLINQ to SQL "pluraliza" automáticamente los nombres de tabla y columna cuando crea clases basadas en un esquema de base de datos. Por ejemplo: la tabla "Cenas" de nuestro ejemplo anterior dio como resultado una clase "Cena". Esta nomenclatura de clase ayuda a que nuestros modelos sean coherentes con las convenciones de nomenclatura de .NET y, por lo general, me parece que hacer que el diseñador corrija esto sea conveniente (especialmente cuando se agregan muchas tablas). Si no le gusta el nombre de una clase o propiedad que genera el diseñador, sin embargo, siempre puede reemplazarlo y cambiarlo a cualquier nombre que desee. Puede hacerlo editando el nombre de entidad/propiedad en línea dentro del diseñador o modificándolo a través de la cuadrícula de propiedades.

De forma predeterminada, el diseñador LINQ to SQLLINQ to SQL también inspecciona las relaciones de clave principal y clave externa de las tablas y, en función de ellas, crea automáticamente las "asociaciones de relación" predeterminadas entre las diferentes clases de modelo que crea. Por ejemplo, cuando arrastramos las tablas Dinners y RSVP al diseñador LINQ to SQLLINQ to SQL, se dedujo una asociación de relación entre los dos basándose en el hecho de que la tabla RSVP tenía una clave externa a la tabla Dinners (esto se indica mediante la flecha del diseñador):

![](build-a-model-with-business-rule-validations/_static/image6.png)

La asociación anterior hará que LINQ to SQLLINQ to SQL agregue una propiedad "Dinner" fuertemente tipada a la clase RSVP que los desarrolladores pueden usar para tener acceso a la cena asociada a un RSVP determinado. También hará que la clase Dinner tenga una propiedad de colección "RSVPs" que permite a los desarrolladores recuperar y actualizar objetos RSVP asociados a una cena determinada.

A continuación puede ver un ejemplo de intellisense dentro de Visual Studio cuando creamos un nuevo objeto RSVP y lo agregamos a una colección RSVPs de la cena. Observe cómo LINQ to SQLLINQ to SQL agrega automáticamente una colección "RSVPs" en el objeto Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Al agregar el objeto RSVP a la colección RSVPs de la cena, le decimos a LINQ to SQLLINQ to SQL que asocie una relación de clave externa entre la fila Dinner y la fila RSVP de nuestra base de datos:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si no le gusta cómo el diseñador ha modelado o nombrado una asociación de tabla, puede invalidarla. Simplemente haga clic en la flecha de asociación dentro del diseñador y acceda a sus propiedades a través de la cuadrícula de propiedades para cambiarle el nombre, eliminarla o modificarla. Para nuestra aplicación NerdDinner, sin embargo, las reglas de asociación predeterminadas funcionan bien para las clases de modelo de datos que estamos creando y solo podemos usar el comportamiento predeterminado.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext Clase

Visual Studio creará automáticamente clases .NET que representan los modelos y las relaciones de base de datos definidas mediante el diseñador LINQ to SQLLINQ to SQL. También se genera una clase LINQ to SQL DataContext para cada archivo de diseñador LINQ to SQLLINQ to SQL agregado a la solución. Dado que hemos denominado nuestro elemento de clase LINQ to SQLLINQ to SQL "NerdDinner", la clase DataContext creada se denominará "NerdDinnerDataContext". Esta clase NerdDinnerDataContext es la forma principal en que interactuaremos con la base de datos.

Nuestra clase NerdDinnerDataContext expone dos propiedades - "Dinners" y "RSVPs" - que representan las dos tablas que modelamos dentro de la base de datos. Podemos usar C- para escribir consultas LINQ en esas propiedades para consultar y recuperar objetos Dinner y RSVP de la base de datos.

En el código siguiente se muestra cómo crear instancias de un objeto NerdDinnerDataContext y realizar una consulta LINQ en él para obtener una secuencia de Dinners que se producen en el futuro. Visual Studio proporciona intellisense completo al escribir la consulta LINQ y los objetos devueltos desde ella están fuertemente tipados y también admiten intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Además de permitirnos consultar objetos Dinner y RSVP, un NerdDinnerDataContext también realiza un seguimiento automático de los cambios que posteriormente realizamos en los objetos Dinner y RSVP que recuperamos a través de él. Podemos usar esta funcionalidad para guardar fácilmente los cambios en la base de datos, sin tener que escribir ningún código de actualización SQL explícito.

Por ejemplo, el código siguiente muestra cómo usar una consulta LINQ para recuperar un único objeto Dinner de la base de datos, actualizar dos de las propiedades Dinner y, a continuación, guardar los cambios en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

El objeto NerdDinnerDataContext del código anterior realizaba un seguimiento automático de los cambios de propiedad realizados en el objeto Dinner que recuperamos de él. Cuando llamamos al método "SubmitChanges()", ejecutará una instrucción "UPDATE" de SQL adecuada en la base de datos para conservar los valores actualizados.

### <a name="creating-a-dinnerrepository-class"></a>Creación de una clase DinnerRepository

Para aplicaciones pequeñas, a veces está bien hacer que los controladores trabajen directamente en una clase LINQ to SQL DataContext e incrustar consultas LINQ en los controladores. Sin embargo, a medida que las aplicaciones se hacen más grandes, este enfoque se vuelve engorroso para mantener y probar. También puede llevarnos a duplicar las mismas consultas LINQ en varios lugares.

Un enfoque que puede hacer que las aplicaciones sean más fáciles de mantener y probar es usar un patrón de "repositorio". Una clase de repositorio ayuda a encapsular la lógica de consulta y persistencia de datos y abstrae los detalles de implementación de la persistencia de datos de la aplicación. Además de hacer que el código de la aplicación sea más limpio, el uso de un patrón de repositorio puede facilitar el cambio de las implementaciones de almacenamiento de datos en el futuro, y puede ayudar a facilitar las pruebas unitarias de una aplicación sin necesidad de una base de datos real.

Para nuestra aplicación NerdDinner definiremos una clase DinnerRepository con la siguiente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: Más adelante en este capítulo extraeremos una interfaz IDinnerRepository de esta clase y habilitaremos la inserción de dependencias con ella en nuestros controladores. Para empezar, sin embargo, vamos a empezar simple y simplemente trabajar directamente con la clase DinnerRepository.*

Para implementar esta clase, haga clic con el botón derecho en nuestra carpeta "Modelos" y elegiremos el comando de menú **Agregar&gt;nuevo elemento.** Dentro del cuadro de diálogo "Agregar nuevo elemento" seleccionaremos la plantilla "Clase" y nombraremos el archivo "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

A continuación, podemos implementar nuestra clase DinnerRepository utilizando el siguiente código:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, actualizar, insertar y eliminar mediante la clase DinnerRepository

Ahora que hemos creado nuestra clase DinnerRepository, echemos un vistazo a algunos ejemplos de código que muestran tareas comunes que podemos hacer con ella:

#### <a name="querying-examples"></a>Consultar ejemplos

El código siguiente recupera una sola cena con el valor DinnerID:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

El código siguiente recupera todas las próximas cenas y bucles sobre ellos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Insertar y actualizar ejemplos

El código siguiente muestra cómo agregar dos cenas nuevas. Las adiciones/modificaciones al repositorio no se confirman en la base de datos hasta que se llama al método "Save()" en ella. LINQ to SQLLINQ to SQL ajusta automáticamente todos los cambios en una transacción de base de datos, por lo que todos los cambios se producen o ninguno de ellos cuando se guarda el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

El código siguiente recupera un objeto Dinner existente y modifica dos propiedades en él. Los cambios se confirman en la base de datos cuando se llama al método "Save()" en nuestro repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

El código siguiente recupera una cena y, a continuación, agrega un RSVP a ella. Lo hace mediante la colección RSVPs en el objeto Dinner que LINQ to SQLLINQ to SQL creó para nosotros (porque hay una relación de clave principal/clave externa entre los dos de la base de datos). Este cambio se conserva de nuevo a la base de datos como una nueva fila de tabla RSVP cuando se llama al método "Save()" en el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Eliminar ejemplo

El código siguiente recupera un objeto Dinner existente y, a continuación, lo marca para eliminarlo. Cuando se llama al método "Save()" en el repositorio, se confirmará la eliminación en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integración de validación y lógica de reglas de negocio con clases de modelos

La integración de la validación y la lógica de reglas de negocio es una parte clave de cualquier aplicación que funcione con datos.

#### <a name="schema-validation"></a>Validación de esquemas

Cuando las clases de modelo se definen mediante el diseñador LINQ to SQLLINQ to SQL, los tipos de datos de las propiedades de las clases de modelo de datos corresponden a los tipos de datos de la tabla de base de datos. Por ejemplo: si la columna "EventDate" de la tabla Dinners es un "datetime", la clase de modelo de datos creada por LINQ to SQLLINQ to SQL será de tipo "DateTime" (que es un tipo de datos .NET integrado). Esto significa que obtendrá errores de compilación si intenta asignarle un entero o booleano desde el código y generará un error automáticamente si intenta convertir implícitamente un tipo de cadena no válido en él en tiempo de ejecución.

LINQ to SQLLINQ to SQL también controlará automáticamente el escape de valores SQL cuando se usan cadenas, lo que le ayuda a protegerse contra ataques de inyección de SQL al usarlo.

#### <a name="validation-and-business-rule-logic"></a>Validación y lógica de reglas de negocio

La validación de esquemas es útil como primer paso, pero rara vez es suficiente. La mayoría de los escenarios del mundo real requieren la capacidad de especificar una lógica de validación más completa que pueda abarcar varias propiedades, ejecutar código y, a menudo, tener conocimiento del estado de un modelo (por ejemplo: se está creando /updated/deleted, o dentro de un estado específico del dominio como "archivado"). Hay una variedad de patrones y marcos diferentes que se pueden usar para definir y aplicar reglas de validación a las clases de modelo, y hay varios marcos basados en .NET por ahí que se pueden usar para ayudar con esto. Puede usar prácticamente cualquiera de ellos dentro de ASP.NET aplicaciones MVC.

Para los fines de nuestra aplicación NerdDinner, usaremos un patrón relativamente simple y directo donde exponemos una propiedad IsValid y un método GetRuleViolations() en nuestro objeto de modelo Dinner. La propiedad IsValid devolverá true o false en función de si las reglas de validación y de negocio son válidas. El método GetRuleViolations() devolverá una lista de errores de regla.

Implementaremos IsValid y GetRuleViolations() para nuestro modelo Dinner agregando una "clase parcial" a nuestro proyecto. Las clases parciales se pueden usar para agregar métodos/propiedades/eventos a las clases mantenidas por un diseñador de VS (como la clase Dinner generada por el diseñador LINQ to SQLLINQ to SQL) y ayudar a evitar que la herramienta se meta con nuestro código. Podemos agregar una nueva clase parcial a nuestro proyecto haciendo clic con el botón derecho en la carpeta Desmodelos y, a continuación, seleccionando el comando de menú "Agregar nuevo elemento". A continuación, podemos elegir la plantilla "Clase" dentro del cuadro de diálogo "Agregar nuevo elemento" y nombrarlo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Al hacer clic en el botón "Agregar" se agregará un archivo Dinner.cs a nuestro proyecto y se abrirá dentro del IDE. A continuación, podemos implementar un marco básico de aplicación de reglas/validación utilizando el código siguiente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algunas notas sobre el código anterior:

- La clase Dinner está precedida por una palabra clave "parcial", lo que significa que el código contenido en ella se combinará con la clase generada/mantenida por el diseñador LINQ to SQLLINQ to SQL y se compilará en una sola clase.
- La clase RuleViolation es una clase auxiliar que agregaremos al proyecto que nos permite proporcionar más detalles sobre una infracción de regla.
- El método Dinner.GetRuleViolations() hace que se evalúen nuestras reglas de validación y de negocio (las implementaremos en breve). A continuación, devuelve una secuencia de RuleViolation objetos que proporcionan más detalles sobre los errores de regla.
- El Dinner.IsValid propiedad proporciona una propiedad auxiliar conveniente que indica si el Dinner objeto tiene alguna RuleViolations activa. Puede ser comprobado proactivamente por un desarrollador que usa el objeto Dinner en cualquier momento (y no genera una excepción).
- El método parcial Dinner.OnValidate() es un gancho que LINQ to SQLLINQ to SQL proporciona que nos permite recibir una notificación cada vez que el objeto Dinner está a punto de persistirse en la base de datos. Nuestra implementación OnValidate() anterior garantiza que la cena no tenga RuleViolations antes de que se guarde. Si está en un estado no válido, genera una excepción, lo que hará que LINQ to SQLLINQ to SQL anule la transacción.

Este enfoque proporciona un marco simple en el que podemos integrar la validación y las reglas de negocio. Por ahora vamos a añadir las siguientes reglas a nuestro método GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando la característica "yield return" de C- para devolver una secuencia de cualquier RuleViolations. Las primeras seis comprobaciones de reglas anteriores simplemente aplican que las propiedades de cadena en nuestra cena no pueden ser nulas o vacías. La última regla es un poco más interesante, y llama a un método auxiliar PhoneValidator.IsValidNumber() que podemos agregar a nuestro proyecto para comprobar que el formato de número ContactPhone coincide con el país de la cena.

Podemos usar . Compatibilidad con expresiones regulares de NET para implementar este soporte de validación de teléfono. A continuación se muestra una sencilla implementación de PhoneValidator que podemos agregar a nuestro proyecto que nos permite agregar comprobaciones de patrones Regex específicas del país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Manejo de validación y violaciones de lógica empresarial

Ahora que hemos agregado el código de validación y regla de negocio anterior, cada vez que intentemos crear o actualizar una cena, nuestras reglas lógicas de validación se evaluarán y aplicarán.

Los desarrolladores pueden escribir código como el siguiente para determinar de forma proactiva si un objeto Dinner es válido y recuperar una lista de todas las infracciones en él sin generar excepciones:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si intentamos guardar una cena en un estado no válido, se producirá una excepción cuando llamemos al método Save() en DinnerRepository. Esto ocurre porque LINQ to SQLLINQ to SQL llama automáticamente a nuestro método parcial Dinner.OnValidate() antes de guardar los cambios de dinner y agregamos código a Dinner.OnValidate() para generar una excepción si existen infracciones de reglas en la cena. Podemos detectar esta excepción y recuperar de forma reactiva una lista de las infracciones que se van a corregir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Dado que nuestras reglas de validación y de negocio se implementan dentro de nuestra capa de modelo, y no dentro de la capa de interfaz de usuario, se aplicarán y usarán en todos los escenarios de nuestra aplicación. Más adelante podemos cambiar o agregar reglas de negocio y hacer que todo el código que funciona con nuestros objetos Dinner las honre.

Tener la flexibilidad para cambiar las reglas de negocio en un solo lugar, sin que estos cambios se alimenten en toda la aplicación y la lógica de la interfaz de usuario, es un signo de una aplicación bien escrita y una ventaja que un marco MVC ayuda a fomentar.

### <a name="next-step"></a>siguiente paso

Ahora tenemos un modelo que podemos usar para consultar y actualizar nuestra base de datos.

Ahora vamos a agregar algunos controladores y vistas al proyecto que podemos usar para crear una experiencia de interfaz de usuario HTML a su alrededor.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Siguiente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
