---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Creación de una capa de acceso a datos (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que con objetos DataSet con tipo, para acceder a la información en una base de datos.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d8afd13fc693c828850bec53664a4db7d91dede
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59420606"
---
# <a name="creating-a-data-access-layer-c"></a>Crear una capa de acceso a datos (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que con objetos DataSet con tipo, para acceder a la información en una base de datos.


## <a name="introduction"></a>Introducción

Como desarrollador web, nuestras vidas giran en torno a trabajar con datos. Vamos a crear las bases de datos para almacenar los datos, el código para recuperar y modificar la base de datos y páginas web para recopilar y resumirlos. Este es el primer tutorial de una serie larga que explorará técnicas para implementar estos patrones comunes en ASP.NET 2.0. Comenzaremos con la creación de un [arquitectura de software](http://en.wikipedia.org/wiki/Software_architecture) consta de una capa de acceso de datos (DAL) mediante los DataSets con tipo, una capa de lógica empresarial (BLL) que aplica las reglas de negocios personalizada y una capa de presentación que se compone de ASP.NET que las páginas compartir un diseño de página común. Una vez que se ha diseñado este marco de trabajo de back-end, pasaremos a informes, que muestra cómo mostrar, resumir, recopilar y validar los datos desde una aplicación web. Estos tutoriales están pensados para ser conciso y se proporcionan instrucciones paso a paso con una gran cantidad de capturas de pantalla que le guiarán a través del proceso visualmente. Cada tutorial está disponible en versiones de Visual Basic y C# e incluye la descarga de código completo usado. (Este primer tutorial, es bastante largo, pero el resto se presentan en fragmentos ofrece mucho más).

Estos tutoriales vamos a usar una versión de Microsoft SQL Server 2005 Express Edition de la base de datos de Northwind que se coloca en el **aplicación\_datos** directory. Además del archivo de base de datos, el **aplicación\_datos** carpeta también contiene los scripts SQL para crear la base de datos, en caso de que desea usar una versión diferente de la base de datos. Se puede también estas secuencias de comandos [descargar directamente desde Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), si lo prefiere. Si usa una versión de SQL Server diferente de la base de datos Northwind, deberá actualizar el **NORTHWNDConnectionString** en la aplicación **Web.config** archivo. La aplicación web compilada con Visual Studio 2005 Professional Edition como un proyecto de sitio Web basado en el sistema de archivos. Sin embargo, todos los tutoriales funcionará igual de bien con la versión gratuita de Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que seguido por la creación de la capa de lógica empresarial (BLL) en el segundo tutorial y trabaja en el diseño de página y la navegación en la tercera. Los tutoriales después de que el tercero se basa en la base se colocan en las tres primeras. Tenemos mucho que cubren en este primer tutorial, así que ponen en marcha Visual Studio y vamos a empezar a trabajar.

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Paso 1: Crear un proyecto Web y conectarse a la base de datos

Para poder crear nuestro nivel de acceso de datos (DAL), primero es necesario crear un sitio web y configurar la base de datos. Empiece por crear un nuevo sitio de web ASP.NET basada en el sistema de archivos. Para ello, vaya al menú archivo y elija Nuevo sitio Web, mostrar el cuadro de diálogo nuevo sitio Web. Elija la plantilla de sitio Web de ASP.NET, Establece la lista desplegable de ubicación en el sistema de archivos, elija una carpeta para colocar el sitio web y establecer el lenguaje C#.


[![Crear un sitio Web de New File System-Based](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Figura 1**: Crear un sitio Web de New File System-Based ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image3.png))


Esto creará un nuevo sitio web con un **Default.aspx** página ASP.NET y un **aplicación\_datos** carpeta.

Con el sitio web creado, el siguiente paso es agregar una referencia a la base de datos en el Explorador de servidores de Visual Studio. Mediante la adición de una base de datos en el Explorador de servidores puede agregar tablas, procedimientos almacenados, vistas y así sucesivamente todo desde dentro de Visual Studio. También puede ver los datos de la tabla o crear sus propias consultas a mano o gráficamente mediante el generador de consultas. Además, cuando se compilación los conjuntos de datos con tipo para la capa DAL necesitaremos para dirigir Visual Studio a la base de datos desde el que se debe construir los conjuntos de datos con tipo. Aunque podemos proporcionamos esta información de conexión en ese momento, Visual Studio rellena automáticamente una lista desplegable de las bases de datos ya está registrado en el Explorador de servidores.

Los pasos para agregar la base de datos Northwind en el Explorador de servidores dependen de si desea utilizar la base de datos de SQL Server 2005 Express Edition en el **aplicación\_datos** carpeta o si tiene un Microsoft SQL Server 2000 o 2005 configuración del servidor de bases de datos que desea usar en su lugar.

## <a name="using-a-database-in-theappdatafolder"></a>Uso de una base de datos en theApp\_DataFolder

Si no tiene un servidor SQL Server 2000 o 2005 servidor de base de datos para conectarse a, o simplemente desea evitar tener que agregar la base de datos a un servidor de base de datos, puede usar la versión de SQL Server 2005 Express Edition de la base de datos de Northwind que se encuentra en el sitio Web descargado e's **aplicación\_datos** carpeta (**NORTHWND. MDF**).

Una base de datos se coloca en el **aplicación\_datos** carpeta se agrega automáticamente en el Explorador de servidores. Suponiendo que tiene SQL Server 2005 Express Edition instalada en su equipo, verá un nodo denominado NORTHWND. MDF en el Explorador de servidores, que se puede expandir y explorar sus tablas, vistas, procedimientos almacenados y así sucesivamente (consulte la figura 2).

El **aplicación\_datos** carpeta también puede contener Microsoft Access **.mdb** archivos, lo que, como sus homólogos de SQL Server, se agregan automáticamente para el Explorador de servidores. Si no desea utilizar cualquiera de las opciones de SQL Server, siempre puede [descargar una versión de Microsoft Access del archivo de base de datos Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) y colocar en el **aplicación\_datos** directory. Tenga en cuenta, sin embargo, que no son las bases de datos de Access como llena de características que SQL Server y no están diseñados para usarse en escenarios de sitio web. Además, un par de los tutoriales de 35 + usará ciertas características de nivel de base de datos que no son compatibles con el acceso.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conectarse a la base de datos en un servidor de base de datos de Microsoft SQL Server 2000 o 2005

Como alternativa, puede conectarse a una base de datos Northwind instalada en un servidor de base de datos. Si el servidor de base de datos aún no tiene instalada la base de datos Northwind, primero debe agregarlo al servidor de base de datos, ejecute el script de instalación incluido en la descarga de este tutorial o por [descargando la versión de SQL Server 2000 de Northwind script de instalación y](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) directamente desde el sitio web de Microsoft.

Una vez que tenga instalada la base de datos, vaya al explorador de servidores en Visual Studio, haga doble clic en el nodo Conexiones de datos y elija Agregar conexión. Si no ve el Explorador de servidores para que vaya a la vista / Explorador de servidores o Ctrl + Alt + S posicionamiento. Se abrirá el cuadro de diálogo Agregar conexión, donde puede especificar el servidor al que conectarse, la información de autenticación y el nombre de la base de datos. Una vez que ha configurado la información de conexión de base de datos y hace clic en el botón Aceptar, se agregará la base de datos como un nodo bajo el nodo Conexiones de datos. Puede expandir el nodo de base de datos para explorar sus tablas, vistas, procedimientos almacenados y así sucesivamente.


![Agregar una conexión a la base de datos del servidor de la base de datos Northwind](creating-a-data-access-layer-cs/_static/image4.png)

**Figura 2**: Agregar una conexión a la base de datos del servidor de la base de datos Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Paso 2: Creación de la capa de acceso a datos

Cuando se trabaja con datos de una opción es insertar la lógica específica de datos directamente en la capa de presentación (en una aplicación web, la capa de presentación constituyen las páginas ASP.NET). Esto puede adoptar la forma de escribir código de ADO.NET en la parte del código de la página ASP.NET o mediante el control SqlDataSource desde la parte del marcado. En cualquier caso, este enfoque acopla estrechamente la lógica de acceso a datos con la capa de presentación. Sin embargo, el enfoque recomendado es separar la lógica de acceso a datos de la capa de presentación. Esta capa independiente se conoce como la capa de acceso a datos, la capa DAL para abreviar y normalmente se implementa como un proyecto de biblioteca de clases independiente. Las ventajas de esta arquitectura por capas están bien documentadas (consulte la sección "Más lecturas" al final de este tutorial para obtener información sobre estas ventajas) y es el enfoque que tomamos en esta serie.

Todo el código que es específico del origen de datos subyacente como la creación de una conexión a la base de datos, emitir **seleccione**, **insertar**, **actualización**, y  **ELIMINAR** comandos etc. deben encontrarse en la capa DAL. La capa de presentación no debe contener todas las referencias a dicho código de acceso de datos, pero en su lugar debe realizar llamadas en la capa DAL para las solicitudes de todos los datos. Niveles de acceso a datos normalmente contienen métodos para tener acceso a la base de datos subyacente. Por ejemplo, tiene la base de datos Northwind, **productos** y **categorías** las tablas que registran los productos de venta y las categorías a las que pertenecen. En nuestro DAL tenemos métodos, como:

- **GetCategories(),** que devolverá información sobre todas las categorías
- **GetProducts()**, que devolverá información sobre todos los productos
- **GetProductsByCategoryID (*categoryID*)**, que devolverá todos los productos que pertenecen a una categoría especificada
- **GetProductByProductID (*productID*)**, que devolverá información sobre un producto determinado

Estos métodos, cuando se invoca, conectarse a la base de datos, ejecute la consulta adecuada y devolver los resultados. ¿Cómo se devuelven estos resultados es importante. Estos métodos podrían devolver simplemente un conjunto de datos o DataReader rellenada por la consulta de base de datos, pero lo ideal es que se deben devolver estos resultados con *objetos fuertemente tipados*. Un objeto fuertemente tipado es uno cuyo esquema se define de forma rígida en tiempo de compilación, mientras que lo contrario, un objeto fuertemente tipado, es uno cuyo esquema no se conoce hasta el tiempo de ejecución.

Por ejemplo, DataReader y el conjunto de datos (de forma predeterminada) son objetos fuertemente tipado dado que su esquema se define mediante las columnas devueltas por la consulta de base de datos utilizada para rellenarlos. Para tener acceso a una columna concreta de una DataTable débilmente tipadas debemos usar la sintaxis siguiente: <strong><em>DataTable</em>.Rows[<em>index</em>]["<em>columnName</em>"]</strong>. La DataTable flexible escribiendo en este ejemplo se mostró por el hecho de que necesitamos para tener acceso al nombre de columna mediante una cadena o el índice ordinal. Un objeto DataTable fuertemente tipado, por otro lado, tendrá cada uno de sus columnas que se implementan como propiedades, lo que genera código similar al siguiente: <strong><em>DataTable</em>.Rows[<em>index</em>].*columnName</strong>*.

Para devolver objetos fuertemente tipados, los desarrolladores pueden crear sus propios objetos de negocios personalizada o usar conjuntos de datos con tipo. Un objeto de negocios se implementa con el desarrollador como representa una clase cuyas propiedades suelen reflejan las columnas de la tabla de base de datos subyacente del objeto comercial. Un conjunto de datos con tipo es una clase generada automáticamente por Visual Studio basado en un esquema de base de datos y cuyos miembros son fuertemente tipada de acuerdo con este esquema. El conjunto de datos con tipo propio consta de las clases que extienden las clases de DataRow, DataTable y DataSet de ADO.NET. Además de tablas de datos fuertemente tipados, los conjuntos de datos con tipo ahora también incluyen los TableAdapters, que son clases con métodos para rellenar tablas de datos del conjunto de datos y propagar las modificaciones de las tablas de datos a la base de datos.

> [!NOTE]
> Para obtener más información sobre las ventajas y desventajas de utilizar DataSets con tipo frente a objetos de negocios personalizados, consulte [diseñar componentes de capa de datos y pasar datos a través de niveles](https://msdn.microsoft.com/library/ms978496.aspx).


Vamos a usar conjuntos de datos fuertemente tipados para la arquitectura de estos tutoriales. Figura 3 ilustra el flujo de trabajo entre los diferentes niveles de una aplicación que utilice conjuntos de datos con tipo.


[![ACódigo de acceso a datos es relegado a la capa DAL de ll](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Figura 3**: Código de acceso de todos los datos es relegado a la capa DAL ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Creación de un DataSet con tipo y el adaptador de tabla

Para empezar a crear nuestro DAL, comenzamos agregando un DataSet con tipo para nuestro proyecto. Para ello, haga doble clic en el nodo del proyecto en el Explorador de soluciones y elija Agregar un nuevo elemento. Seleccione la opción de conjunto de datos de la lista de plantillas y asígnele el nombre **Northwind.xsd**.


[![CElija Agregar un nuevo conjunto de datos a su proyecto](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Figura 4**: Optar por agregar un nuevo conjunto de datos en el proyecto ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image10.png))


Después de hacer clic en Agregar, cuando se le pida que agregue el conjunto de datos a la **aplicación\_código** carpeta, elija Sí. A continuación, se mostrará el diseñador para el conjunto de datos con tipo y se iniciará el Asistente para configuración de TableAdapter, lo que permite agregar su primera TableAdapter al conjunto de datos con tipo.

Un conjunto de datos con tipo actúa como una colección fuertemente tipada de datos. se compone de instancias de DataTable fuertemente tipado, cada uno de los cuales a su vez se compone de las instancias de DataRow fuertemente tipado. Se creará una tabla de datos fuertemente tipado para cada una de las tablas de base de datos subyacentes que tenemos que trabajar en esta serie de tutoriales. Comencemos con la creación de un objeto DataTable para el **productos** tabla.

Tenga en cuenta que DataTables fuertemente tipada no incluyen ninguna información sobre cómo tener acceso a datos de su tabla de base de datos subyacente. Para recuperar los datos para rellenar la tabla de datos, se usa una clase de TableAdapter, que funciona como la capa de acceso a datos. Para nuestro **productos** DataTable, TableAdapter contendrá los métodos **GetProducts()**, **GetProductByCategoryID (*categoryID*)**, etc. que se podrá invocar desde la capa de presentación. Rol de la DataTable es servir como la usa para pasar datos entre las capas de objetos fuertemente tipados.

El Asistente para configuración de TableAdapter comienza con la que le pide que seleccione a qué base de datos para trabajar con. La lista desplegable muestra esas bases de datos en el Explorador de servidores. Si no agregó la base de datos Northwind en el Explorador de servidores, haga clic en el botón nueva conexión en este momento para hacerlo.


[![CElija la base de datos Northwind en la lista desplegable](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Figura 5**: Elija la base de datos Northwind en la lista desplegable ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image13.png))


Después de seleccionar la base de datos y hacer clic en siguiente, se le preguntará si desea guardar la cadena de conexión en el **Web.config** archivo. Al guardar la cadena de conexión que se evitará tener duro codificadas en las clases TableAdapter, lo que simplifica las cosas si cambia la información de cadena de conexión en el futuro. Si decide para guardar la cadena de conexión en el archivo de configuración se coloca en el **&lt;connectionStrings&gt;** sección, que puede ser [opcionalmente cifrados](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) mejorado seguridad o modificados más adelante a través de la nueva página de propiedades de ASP.NET 2.0 dentro de la herramienta de administración de GUI de IIS, que es más adecuado para los administradores.


[![Sguardar la cadena de conexión al archivo Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Figura 6**: Guardar la cadena de conexión a **Web.config** ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image16.png))


A continuación, se debe definir el esquema para la primera DataTable fuertemente tipadas y proporcionan el primer método para nuestro TableAdapter que se utilizará al rellenar el conjunto de datos fuertemente tipados. Estos dos pasos se llevan a cabo al mismo tiempo mediante la creación de una consulta que devuelve las columnas de la tabla que queremos reflejados en nuestra tabla de datos. Al final del asistente, se asignará a un nombre de método para esta consulta. Una vez que se ha logrado, este método se puede invocar desde nuestro nivel de presentación. El método ejecutará la consulta definida y rellenar un DataTable fuertemente tipado.

Para empezar a definir la consulta SQL que debemos indicamos cómo queremos TableAdapter para emitir la consulta. Podemos usar una instrucción de SQL ad hoc, cree un nuevo procedimiento almacenado o usar un procedimiento almacenado existente. Estos tutoriales, vamos a usar instrucciones SQL ad hoc. Consulte [Brian Noyes](http://briannoyes.net/)del artículo, [crear una capa de acceso a datos con el Diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obtener un ejemplo del uso de procedimientos almacenados.


[![Qlos datos mediante una instrucción de SQL Ad-Hoc ulta](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Figura 7**: Consultar los datos mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image19.png))


En este momento además podemos escribir en la consulta SQL a mano. Al crear el primer método en TableAdapter normalmente desea que la consulta devuelva las columnas que deben expresarse en la correspondiente DataTable. Podemos lograrlo mediante la creación de una consulta que devuelve todas las columnas y todas las filas de la **productos** tabla:


[![EEscriba el SQL Query en el cuadro de texto](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Figura 8**: Escriba la consulta SQL en el Textbox ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image22.png))


Como alternativa, use el generador de consultas y construir gráficamente la consulta, como se muestra en la figura 9.


[![Crear la consulta gráficamente, mediante el Editor de consultas](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Figura 9**: Crear la consulta gráficamente, mediante el Editor de consultas ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image25.png))


Después de crear la consulta, pero antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. En los proyectos de sitio Web, "generar Insert, Update y Delete instrucciones" es la única opción seleccionada de forma predeterminada; avanzada Si ejecuta a este asistente desde una biblioteca de clases o un proyecto de Windows también se seleccionará la opción "Usar simultaneidad optimista". Deje la opción "Usar simultaneidad optimista" desactivada por ahora. Examinaremos la simultaneidad optimista en tutoriales futuros.


[![Selegir solo las generar Insert, Update y Delete instrucciones opción](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Figura 10**: Seleccione solo las generar Insert, Update y Delete instrucciones opción ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image28.png))


Después de comprobar las opciones avanzadas, haga clic en siguiente para ir a la pantalla final. Aquí se nos pide que seleccione los métodos van a agregar a TableAdapter. Hay dos patrones para rellenar los datos:

- **Rellenar un DataTable** con este enfoque se crea un método que toma un objeto DataTable como parámetro y lo rellena basándose en los resultados de la consulta. La clase DataAdapter de ADO.NET, por ejemplo, implementa este patrón con su **Fill()** método.
- **Devolver un DataTable** con este enfoque el método crea y rellena el DataTable para usted y lo devuelve como valor devuelvan de los métodos.

Puede tener el TableAdapter implementar uno o ambos de estos patrones. También puede cambiar el nombre de los métodos que se proporcionan aquí. Vamos a deje ambas casillas de verificación está activados, aunque vamos a usar solo el último modelo a lo largo de estos tutoriales. Además, vamos a cambiar el nombre de la genérica **GetData** método **GetProducts**.

Si está activada, la casilla de verificación final, "GenerateDBDirectMethods", crea **Insert()**, **Update()**, y **Delete()** métodos del TableAdapter. Si deja esta opción desactivada, todas las actualizaciones se deben realizarse a través de sole del TableAdapter **Update()** método, que toma en el conjunto de datos con tipo, un objeto DataTable, una sola fila de datos o una matriz de objetos DataRow. (Si ha desactivado el "generar Insert, Update y Delete instrucciones" opción de las propiedades avanzadas en la figura 9, esta casilla de verificación configuración no tiene ningún efecto.) Vamos a dejar activada esta casilla.


[![Ccambiar el nombre del método GetData a GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Figura 11**: Cambiar el nombre del método de **GetData** a **GetProducts** ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image31.png))


Complete el asistente, haga clic en Finalizar. Después de cerrarse el Asistente nos estamos devuelto en el Diseñador de DataSet que muestra la DataTable que acabamos de crear. Puede ver la lista de columnas en el **productos** DataTable (**ProductID**, **ProductName**, y así sucesivamente), así como los métodos de la  **ProductsTableAdapter** (**Fill()** y **GetProducts()**).


[![Tse han agregado al conjunto de datos con tipo, productos de DataTable y ProductsTableAdapter](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Figura 12**: El **productos** DataTable y **ProductsTableAdapter** se han agregado al conjunto de datos con tipo ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image34.png))


En este momento tenemos un conjunto de datos con tipo con un único DataTable (**Northwind.Products**) y una clase fuertemente tipada de DataAdapter (**NorthwindTableAdapters.ProductsTableAdapter**) con un  **GetProducts()** método. Estos objetos pueden utilizarse para tener acceso a una lista de todos los productos de código similar al siguiente:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Este código no era necesario escribir un poco de código de acceso específico de datos. No se tuvo que crear instancias de las clases ADO.NET, no tuve que hacer referencia a las cadenas de conexión, las consultas de SQL o procedimientos almacenados. En su lugar, lo TableAdapter proporciona el código de acceso a datos de bajo nivel para nosotros.

Cada objeto que se usa en este ejemplo también está fuertemente tipados, lo que permite a Visual Studio para proporcionar IntelliSense y comprobación de tipos en tiempo de compilación. Y lo mejor de DataTables devueltas por el TableAdapter se puede enlazar a datos ASP.NET controles Web, como GridView, DetailsView, DropDownList, CheckBoxList y otros varios. El ejemplo siguiente muestra el enlace el DataTable devuelto por la **GetProducts()** método en un control GridView en sólo una escasa tres líneas de código dentro de la **página\_carga** controlador de eventos.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![TLista de productos de él se muestra en un control GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Figura 13**: Se muestra la lista de productos en un control GridView ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image37.png))


Aunque este ejemplo escribimos tres líneas de código en nuestra página ASP.NET es necesario **página\_carga** controlador de eventos, en el futuro tutoriales examinaremos cómo usar ObjectDataSource para recuperar los datos de forma declarativa la DAL. Con el origen ObjectDataSource, no tendrá que escribir ningún código y obtendrá también soporte de paginación y ordenación!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Paso 3: Agregar parámetros de métodos para la capa de acceso a datos

En este momento nuestros **ProductsTableAdapter** clase tiene, pero un método, **GetProducts()**, que devuelve todos los productos de la base de datos. Aunque es definitivamente útil poder trabajar con todos los productos, hay veces cuando desearemos para recuperar información sobre un producto específico o todos los productos que pertenecen a una categoría determinada. Para agregar esta funcionalidad a la capa de acceso a datos podemos agregar métodos con parámetros al TableAdapter.

Vamos a agregar el **GetProductsByCategoryID (*categoryID*)** método. Para agregar un nuevo método a la capa DAL, vuelva al diseñador de DataSet, haga clic en el **ProductsTableAdapter** sección y seleccionar Add Query.


![Haga doble clic en el TableAdapter y elija Agregar consulta](creating-a-data-access-layer-cs/_static/image38.png)

**Figura 14**: Haga doble clic en el TableAdapter y elija Agregar consulta


En primer lugar, se nos pide sobre si desea tener acceso a la base de datos mediante una instrucción de SQL ad hoc o un procedimiento almacenado nuevo o existente. Vamos a optar por usar una instrucción de SQL ad hoc a intentarlo. A continuación, se nos pide qué tipo de consulta SQL le gustaría utilizar. Puesto que queremos devolver todos los productos que pertenecen a una categoría especificada, que queremos escribir un **seleccione** instrucción que devuelve filas.


[![CElija crear un Seleccionar instrucción que devuelve filas](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Figura 15**: Optar por crear un **seleccione** que devuelve filas del informe de ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image41.png))


El siguiente paso es definir la consulta SQL utilizada para tener acceso a los datos. Puesto que queremos devolver solo los productos que pertenecen a una categoría determinada, usar el mismo <strong>seleccione</strong> instrucción desde <strong>GetProducts()</strong>, pero agregue el siguiente <strong>donde</strong> cláusula: <strong>DONDE CategoryID = @CategoryID</strong> . El <strong>@CategoryID</strong> parámetro indica el Asistente de TableAdapter para que el método que estamos creando requerirá un parámetro de entrada del tipo correspondiente (es decir, un entero que acepta valores NULL).


[![EEscriba una consulta para devolver sólo los productos en una categoría especificada](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Figura 16**: Escriba una consulta para devolver sólo los productos en una categoría especificada ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image44.png))


En el paso final podemos elegir que los patrones para usar, así como personalizar los nombres de los métodos generados de acceso a datos. Para la trama de relleno, vamos a cambiar el nombre a <strong>FillByCategoryID</strong> y devolver un DataTable devolver patrón (el <strong>obtener*X</strong>*  métodos), vamos a usar  <strong>GetProductsByCategoryID</strong>.


[![CElija los nombres de los métodos de TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Figura 17**: Elija los nombres de los métodos de TableAdapter ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image47.png))


Después de completar al asistente, el Diseñador de DataSet incluye los nuevos métodos de TableAdapter.


![Los productos ahora pueden ser consultadas por categoría](creating-a-data-access-layer-cs/_static/image48.png)

**Figura 18**: Los productos ahora pueden ser consultadas por categoría


Dedique un momento para agregar un **GetProductByProductID (*productID*)** método utilizando la misma técnica.

Estas consultas con parámetros se pueden probar directamente desde el Diseñador de DataSet. Haga doble clic en el método en TableAdapter y elija la vista previa de datos. A continuación, escriba los valores para los parámetros y haga clic en vista previa.


[![Tse muestran aquellos productos pertenecientes a la categoría Bebidas](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Figura 19**: Se muestran la pertenencia de esos productos a la categoría bebidas ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image51.png))


Con el **GetProductsByCategoryID (*categoryID*)** método nuestro DAL, ahora podemos crear una página ASP.NET que muestra solo los productos en una categoría especificada. El ejemplo siguiente muestra todos los productos que se encuentran en la categoría Bebidas, que tienen un **CategoryID** de 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Tse muestra los productos de la categoría Bebidas manguera](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Figura 20**: Se muestran los productos de la categoría bebidas ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Paso 4: Insertar, actualizar y eliminar datos

Hay dos patrones usados para insertar, actualizar y eliminar datos. El primer patrón, que llamaré el patrón de base de datos directa, implica la creación de métodos que, cuando se invoca, problema una **insertar**, **actualización**, o **eliminar** comando a la base de datos que actúa sobre un registro de base de datos única. Estos métodos se pasan normalmente en una serie de valores escalares (enteros, cadenas, booleanos, fechas y horas etc.) que corresponden a los valores para insertar, actualizar o eliminar. Por ejemplo, con este patrón para el **productos** tabla tardaría el método delete en un parámetro de entero que indica el **ProductID** del registro que desea eliminar, mientras que el método insert tardaría un la cadena para la **ProductName**, un decimal para la **UnitPrice**, un entero para el **UnitsOnStock**, y así sucesivamente.


[![EACH Insert, Update y solicitud de eliminación se envía a la base de datos inmediatamente](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Figura 21**: Cada Insert, Update y Delete de solicitud se envían a la base de datos inmediatamente ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image57.png))


El otro patrón, que me referiré a como el lote de actualizaciones de patrón, consiste en actualizar un DataSet, DataTable o colección de filas de datos en una llamada al método completo. Con este patrón de un desarrollador elimina, inserta y modifica las filas de datos en una tabla de datos y, a continuación, pasa esos objetos DataRow o DataTable a un método de actualización. Este método, a continuación, enumera las DataRows pasados, determina si ha ha modificado, agregados o eliminado (a través de la DataRow [propiedad RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valor) y emite la solicitud de base de datos adecuada para cada registro.


[![Ase invoca ll que cambios se sincronizan con la base de datos cuando el método de actualización](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Figura 22**: Todos los cambios se sincronizan con la base de datos cuando se invoca el método de actualización ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter usa el patrón de actualización por lotes de forma predeterminada, pero también admite el patrón directo de la base de datos. Puesto que hemos seleccionado la opción "generar Insert, Update y Delete instrucciones" desde las propiedades avanzadas al crear nuestra TableAdapter, el **ProductsTableAdapter** contiene un **Update()** método, que implementa el patrón de actualización por lotes. En concreto, lo TableAdapter contiene un **Update()** método que se puede pasar el conjunto de datos con tipo, una tabla de datos fuertemente tipados o uno o más filas de datos. Si deja la casilla de verificación "GenerateDBDirectMethods" activada cuando creando primero el TableAdapter en el patrón directo de la base de datos también se implementará a través de **Insert()**, **Update()**, y **Delete()**  métodos.

Ambos patrones de modificación de datos utiliza el TableAdapter **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades para emitir sus **insertar** , **Actualización**, y **eliminar** comandos para la base de datos. Puede inspeccionar y modificar el **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades haciendo clic en el TableAdapter en el Diseñador de DataSet y, a continuación, va en la ventana Propiedades. (Asegúrese de que ha seleccionado el TableAdapter y que la **ProductsTableAdapter** objeto es el seleccionado en la lista desplegable en la ventana Propiedades.)


[![TTableAdapter tiene InsertCommand, UpdateCommand y DeleteCommand propiedades](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Figura 23**: Tiene el TableAdapter **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image63.png))


Para examinar o modificar cualquiera de estas propiedades de comando de base de datos, haga clic en el **CommandText** subpropiedades, que se abrirá el generador de consultas.


[![Configurar la INSERCIÓN, actualización y las instrucciones DELETE en el generador de consultas](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Figura 24**: Configurar la **insertar**, **actualización**, y **eliminar** instrucciones en el generador de consultas ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image66.png))


El ejemplo de código siguiente muestra cómo usar el patrón de actualización por lotes para doblar el precio de todos los productos que no se han suspendido y que tiene 25 unidades en existencias o menos:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

El código siguiente muestra cómo usar el patrón directo de base de datos para eliminar un determinado producto mediante programación, a continuación, actualizar uno y, a continuación, agregue una nueva:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Creación personalizada Insertar, actualizar y eliminar métodos

El **Insert()**, **Update()**, y **Delete()** métodos creados por el método directo de la base de datos pueden ser un poco complicados, especialmente para las tablas con muchas columnas. Buscar en el ejemplo de código anterior, sin ayuda de IntelliSense no es especialmente claro lo que **productos** columna de tabla se asigna a cada parámetro de entrada a la **Update()** y **Insert()**  métodos. Puede haber ocasiones cuando solo queremos actualizar una sola columna o dos, o desea una personalizada **Insert()** método que, quizás, devolver el valor del registro recién insertado **identidad** (incremento automático) campo.

Para crear este tipo de método personalizado, vuelva al diseñador de DataSet. Haga doble clic en el TableAdapter y elija Agregar consulta devolver para el Asistente de TableAdapter. Podemos indicar el tipo de consulta para crear en la segunda pantalla. Vamos a crear un método que agrega un nuevo producto y, a continuación, devuelve el valor del registro recién agregado **ProductID**. Por lo tanto, optar por crear un **insertar** consulta.


[![Ccrear un método para agregar una nueva fila a la tabla Products](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Figura 25**: Cree un método para agregar una fila nueva a la **productos** tabla ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image69.png))


En la siguiente pantalla del **InsertCommand**del **CommandText** aparece. Aumentar esta consulta agregando **seleccionar ámbito\_IDENTITY()** al final de la consulta que devolverá el último valor identity insertado en un **identidad** columna en el mismo ámbito. (Consulte la [documentación técnica](https://msdn.microsoft.com/library/ms190315.aspx) para obtener más información acerca de **ámbito\_IDENTITY()** y por qué lo más conveniente [utilizar ámbito\_IDENTITY() en lugar de @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Asegúrese de que finalice la **insertar** instrucción con un punto y coma antes de agregar el **seleccione** instrucción.


[![Augment la consulta para devolver el valor SCOPE_IDENTITY ()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Figura 26**: Aumentar la consulta para devolver el **ámbito\_IDENTITY()** valor ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image72.png))


Por último, nombre del nuevo método **InsertProduct**.


[![Sel nombre del método nuevo a InsertProduct et](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Figura 27**: Establece el nombre del nuevo método en **InsertProduct** ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image75.png))


Cuando vuelva al diseñador de DataSet que verá el **ProductsTableAdapter** contiene un método nuevo, **InsertProduct**. Si este nuevo método no tiene un parámetro para cada columna de la **productos** tabla, lo más probable es que haya olvidado de finalizar la **insertar** instrucción con un punto y coma. Configurar la **InsertProduct** método y asegúrese de que tiene un punto y coma de delimitación el **insertar** y **seleccione** instrucciones.

De forma predeterminada, inserte el problema no sea una consulta métodos, lo que significa que devuelven el número de filas afectadas. Sin embargo, deseamos la **InsertProduct** método para devolver el valor devuelto por la consulta, no el número de filas afectadas. Para ello, ajuste el **InsertProduct** del método **ExecuteMode** propiedad **escalares**.


[![Cla propiedad ExecuteMode para escalar ambiar](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Figura 28**: Cambiar el **ExecuteMode** propiedad **escalares** ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image78.png))


El código siguiente muestra esta nueva **InsertProduct** método en acción:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Paso 5: Finalización de la capa de acceso a datos

Tenga en cuenta que el **ProductsTableAdapters** clase devuelve el **CategoryID** y **SupplierID** los valores de la **productos** tabla, pero no incluye el **CategoryName** columna desde la **categorías** tabla o la **CompanyName** columna desde la **proveedores**de tabla, aunque es probable que son las columnas que queremos mostrar cuando se muestre información del producto. Podemos aumentamos el método del TableAdapter inicial, **GetProducts()**, para incluir tanto la **CategoryName** y **CompanyName** valores de columna, que actualizarán el DataTable fuertemente tipado para incluir estas nuevas columnas.

Esto puede suponer un problema, sin embargo, como los métodos del TableAdapter para insertar, actualizar, y eliminar datos se basan en este método inicial. Afortunadamente, los métodos generados automáticamente para insertar, actualizar y eliminar no están afectadas por las subconsultas en la **seleccione** cláusula. Para agregar nuestras consultas que se aseguran **categorías** y **proveedores** como subconsultas, en lugar de **UNIR** s, se evitará tener que rehacer esos métodos para modificar los datos. Haga doble clic en el **GetProducts()** método en el **ProductsTableAdapter** y elija Configurar. A continuación, ajustar el **seleccione** cláusula para que quede como:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Ula instrucción SELECT para el método GetProducts() ctualización](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Figura 29**: Actualización de la **seleccione** instrucción para el **GetProducts()** método ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image81.png))


Después de actualizar el **GetProducts()** método que se usará esta nueva consulta la tabla de datos incluirá dos nuevas columnas: **CategoryName** y **NombreProveedor**.


![La tabla de datos de productos tiene dos nuevas columnas](creating-a-data-access-layer-cs/_static/image82.png)

**Figura 30**: El **productos** DataTable tiene dos nuevas columnas


Dedique unos minutos a actualizar el **seleccione** cláusula en la **GetProductsByCategoryID (*categoryID*)** método así.

Si actualiza el **GetProducts()** **seleccione** mediante **UNIR** sintaxis el Diseñador de DataSet no pueden generar automáticamente los métodos para insertar, actualizar y eliminar base de datos con el patrón directo de la base de datos. En su lugar, tendrá que crearlas manualmente mucho como hicimos con el **InsertProduct** método anteriormente en este tutorial. Además, manualmente tendrá que proporcionar el **InsertCommand**, **UpdateCommand**, y **DeleteCommand** los valores de propiedad si desea usar el lote de actualización de patrón.

## <a name="adding-the-remaining-tableadapters"></a>Adición de los TableAdapters restantes

Hasta ahora, sólo hemos analizado trabajar con un solo TableAdapter para una tabla de base de datos única. Sin embargo, la base de datos Northwind contiene varias tablas relacionadas que se necesitará para trabajar con en nuestra aplicación web. Un conjunto de datos con tipo puede contener varios relacionados con DataTables. Por lo tanto, para completar nuestro DAL, necesitamos agregar DataTable para las demás tablas que usaremos en estos tutoriales. Para agregar un nuevo TableAdapter a un conjunto de datos con tipo, abra el Diseñador de DataSet, haga doble clic en el diseñador y elija Agregar / TableAdapter. Esto creará una nueva DataTable y TableAdapter y le guiará a través del asistente que hemos visto anteriormente en este tutorial.

Tardar unos minutos en crear los siguientes métodos con las siguientes consultas y los TableAdapters. Tenga en cuenta que las consultas en el **ProductsTableAdapter** incluyen las subconsultas para obtener los nombres de categoría y el proveedor de cada producto. Además, si ha seguido a lo largo de, ya ha agregado el **ProductsTableAdapter** la clase **GetProducts()** y **GetProductsByCategoryID (*categoryID* )** métodos.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![Tél DataSet diseñador después de los cuatro TableAdapters se han agregado](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Figura 31**: El conjunto de datos diseñador después de los cuatro TableAdapters se han agregado ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Agregar código personalizado a la capa DAL

Los TableAdapters y las DataTables que agregó al conjunto de datos con tipo se expresan como un archivo de definición de esquemas XML (**Northwind.xsd**). Puede ver esta información del esquema con el botón secundario en el **Northwind.xsd** de archivos en el Explorador de soluciones y elija Ver código.


[![Tque el archivo de definición de esquemas XML (XSD) del conjunto de datos Northwinds escrito](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Figura 32**: El archivo de definición de esquemas XML (XSD) del conjunto de datos Northwinds escrito ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image88.png))


Esta información del esquema se traduce en código C# o Visual Basic en tiempo de diseño cuando se compila o en tiempo de ejecución (si es necesario), momento en que puede ir a través de él con el depurador. Para ver el código generado automáticamente, vaya a la vista de clases y la exploración en profundidad hacia abajo a las clases TableAdapter o conjunto de datos con tipo. Si no ve la vista de clases en la pantalla, vaya al menú Ver y seleccionarlo desde allí o presione Ctrl + Mayús + C. Desde la vista de clases puede ver las propiedades, métodos y eventos de las clases de conjunto de datos con tipo y un TableAdapter. Para ver el código para un método concreto, haga doble clic en el nombre del método en la vista de clases o haga doble clic en él y elija Ir a definición.


![Inspeccionar el código generado automáticamente por seleccionar ir a definición de la vista de clases](creating-a-data-access-layer-cs/_static/image89.png)

**Figura 33**: Inspeccionar el código generado automáticamente por seleccionar ir a definición de la vista de clases


Aunque el código generado automáticamente puede ser un gran ahorro de tiempo, el código es muy genérico, a menudo y debe personalizarse para satisfacer las necesidades exclusivas de una aplicación. El riesgo de extender el código generado automáticamente, sin embargo, es que la herramienta que generó el código podría decidir es el momento "regenerar" y sobrescribir las personalizaciones. Con el nuevo concepto de clase parcial de .NET 2.0, es fácil dividir una clase en varios archivos. Esto nos permite agregar nuestra propia métodos, propiedades y eventos para las clases generadas automáticamente sin tener que preocuparse acerca de Visual Studio sobrescribir las personalizaciones de nuestro.

Para demostrar cómo personalizar la capa DAL, vamos a agregar un **GetProducts()** método a la **SuppliersRow** clase. El **SuppliersRow** clase representa un único registro en el **proveedores** tabla; cada proveedor de proveedor puede cero a muchos productos, por lo que **GetProducts()** devolverá aquellas productos del proveedor especificado. Para lograr esto crear un nuevo archivo de clase en el **aplicación\_código** carpeta denominada **SuppliersRow.cs** y agregue el código siguiente:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Esta clase parcial indica al compilador que, cuando generar el **Northwind.SuppliersRow** clase para que incluya el **GetProducts()** que acabamos de definir el método. Si compila el proyecto y, a continuación, volver a la vista de clases verá **GetProducts()** ahora aparece como un método de **Northwind.SuppliersRow**.


![El método GetProducts() es ahora parte de la clase Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Figura 34**: El **GetProducts()** método forma ahora parte de la **Northwind.SuppliersRow** clase


El **GetProducts()** método puede utilizarse para enumerar el conjunto de productos para un determinado proveedor, como se muestra en el código siguiente:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Estos datos también se pueden mostrar en cualquiera de ASP. Controles Web de datos de la red. La página siguiente, utiliza un control GridView con dos campos:

- BoundField que muestra el nombre de cada proveedor, y
- TemplateField que contiene un control BulletedList que está enlazado a los resultados devueltos por la **GetProducts()** método para cada proveedor.

Examinaremos cómo se muestran estos informes de maestro y detalles en tutoriales futuros. Por ahora, en este ejemplo está diseñado para ilustrar mediante el método personalizado agregado a la **Northwind.SuppliersRow** clase.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![TNombre de la compañía del proveedor aparece en la columna izquierda, sus productos en la derecha](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Figura 35**: Nombre de la compañía del proveedor aparece en la columna izquierda, sus productos en la derecha ([haga clic aquí para ver imagen en tamaño completo](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Resumen

Al compilar una aplicación web de creación del DAL debe ser uno de los primeros pasos, antes de empezar a crear la capa de presentación. Con Visual Studio, la creación de un DAL basado en conjuntos de datos con tipo es una tarea que se puede realizar en 10-15 minutos sin necesidad de escribir una línea de código. Los tutoriales más adelante creará sobre este DAL. En el [siguiente tutorial](creating-a-business-logic-layer-cs.md) nos definiré un número de reglas de negocios y vea cómo implementarlos en una capa de lógica de negocios independientes.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Creación de un DAL con los TableAdapters tipados fuertemente y DataTables de VS 2005 y ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Diseño de componentes de nivel de datos y pasar datos a través de niveles](https://msdn.microsoft.com/library/ms978496.aspx)
- [Crear una capa de acceso a datos con el Diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Cifrar información de configuración en ASP.NET 2.0 aplicaciones](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Información general sobre TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Trabajar con un conjunto de datos con tipo](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Mediante el acceso de datos fuertemente tipados en Visual Studio 2005 y ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Cómo ampliar los métodos de TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperación de datos escalares de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>En los temas contenidos en este Tutorial en vídeo

- [Niveles de acceso a datos en aplicaciones de ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Cómo enlazar manualmente un conjunto de datos a un control Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Cómo trabajar con conjuntos de datos y los filtros de una aplicación ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez y Carlos Santos. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](creating-a-business-logic-layer-cs.md)
