---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Usar dependencias de caché de SQL (VB) | Microsoft Docs
author: rick-anderson
description: La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expiren después de un período de tiempo especificado. Pero este sencillo enfoque significa que los datos almacenados en caché maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 175169772ba04376e1bd1cb143b13a200652a9bf
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044797"
---
# <a name="using-sql-cache-dependencies-vb"></a>Usar dependencias de caché de SQL (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) o [Descargar PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expiren después de un período de tiempo especificado. Pero este sencillo enfoque significa que los datos almacenados en caché no mantienen ninguna asociación con su origen de datos subyacente, lo que da lugar a datos obsoletos que se retienen demasiado largos o datos actuales que han expirado demasiado pronto. Un mejor enfoque consiste en usar la clase SqlCacheDependency para que los datos permanezcan en la memoria caché hasta que los datos subyacentes se hayan modificado en la base de datos SQL. En este tutorial se muestra cómo realizar las siguientes acciones.

## <a name="introduction"></a>Introducción

Las técnicas de almacenamiento en caché examinadas en los [datos de almacenamiento en caché con los datos ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) y [caching en los tutoriales de arquitectura](caching-data-in-the-architecture-vb.md) usaban una expiración basada en el tiempo para expulsar los datos de la memoria caché después de un período especificado. Este enfoque es la manera más sencilla de equilibrar las ventajas de rendimiento del almacenamiento en caché frente a la obsolescencia de los datos. Al seleccionar un tiempo de expiración de *x* segundos, un desarrollador de páginas conviene disfrutar de las ventajas de rendimiento del almacenamiento en caché durante solo *x* segundos, pero puede resultar fácil que los datos nunca queden obsoletos más que un máximo de *x* segundos. Por supuesto, para los datos estáticos, *x* puede extenderse hasta la duración de la aplicación Web, como se examinó en el tutorial de [Inicio de datos de almacenamiento en caché al iniciar la aplicación](caching-data-at-application-startup-vb.md) .

Al almacenar en caché los datos de la base de datos, a menudo se elige una expiración basada en el tiempo para facilitar su uso, pero con frecuencia es una solución inadecuada. Idealmente, los datos de la base de datos permanecerán en caché hasta que se hayan modificado los datos subyacentes en la base de datos. solo entonces se expulsará la memoria caché. Este enfoque maximiza las ventajas de rendimiento del almacenamiento en caché y minimiza la duración de los datos obsoletos. Sin embargo, para disfrutar de estas ventajas, debe existir algún sistema que sepa cuándo se han modificado los datos de la base de datos subyacente y expulsa los elementos correspondientes de la memoria caché. Antes de ASP.NET 2,0, los desarrolladores de páginas eran responsables de implementar este sistema.

ASP.NET 2,0 proporciona una [ `SqlCacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) y la infraestructura necesaria para determinar cuándo se ha producido un cambio en la base de datos, de modo que se puedan desalojar los elementos almacenados en caché correspondientes. Hay dos técnicas para determinar cuándo han cambiado los datos subyacentes: notificación y sondeo. Después de analizar las diferencias entre notificación y sondeo, crearemos la infraestructura necesaria para admitir el sondeo y, después, exploraremos cómo usar la `SqlCacheDependency` clase en escenarios declarativos y mediante programación.

## <a name="understanding-notification-and-polling"></a>Descripción de las notificaciones y el sondeo

Existen dos técnicas que se pueden utilizar para determinar cuándo se han modificado los datos de una base de datos: notificación y sondeo. Con la notificación, la base de datos alerta automáticamente al tiempo de ejecución de ASP.NET cuando se han cambiado los resultados de una consulta determinada desde la última vez que se ejecutó la consulta, momento en el que se expulsan los elementos almacenados en caché asociados a la consulta. Con el sondeo, el servidor de base de datos mantiene información acerca de Cuándo se han actualizado por última vez las tablas concretas. El tiempo de ejecución de ASP.NET sondea periódicamente la base de datos para comprobar qué tablas han cambiado desde que se introdujeron en la memoria caché. Las tablas cuyos datos se han modificado tienen sus elementos de caché asociados expulsados.

La opción de notificación requiere menos configuración que el sondeo y es más granular, ya que realiza un seguimiento de los cambios en el nivel de consulta en lugar de en el nivel de tabla. Desafortunadamente, las notificaciones solo están disponibles en las ediciones completas de Microsoft SQL Server 2005 (es decir, las ediciones que no son Express). Sin embargo, la opción de sondeo se puede usar para todas las versiones de Microsoft SQL Server de 7,0 a 2005. Dado que estos tutoriales usan la edición Express de SQL Server 2005, nos centraremos en la configuración y el uso de la opción de sondeo. Consulte la sección lecturas adicionales al final de este tutorial para obtener más recursos sobre las capacidades de notificación de SQL Server 2005 s.

Con el sondeo, la base de datos debe configurarse para incluir una tabla denominada `AspNet_SqlCacheTablesForChangeNotification` que tenga tres columnas: `tableName` , `notificationCreated` y `changeId` . Esta tabla contiene una fila para cada tabla que tiene datos que pueden ser necesarios para usarse en una dependencia de caché de SQL en la aplicación Web. La `tableName` columna especifica el nombre de la tabla, mientras que `notificationCreated` indica la fecha y la hora en que se agregó la fila a la tabla. La `changeId` columna es de tipo `int` y tiene un valor inicial de 0. Su valor se incrementa con cada modificación de la tabla.

Además de la `AspNet_SqlCacheTablesForChangeNotification` tabla, la base de datos también necesita incluir desencadenadores en cada una de las tablas que pueden aparecer en una dependencia de caché de SQL. Estos desencadenadores se ejecutan cada vez que se inserta, actualiza o elimina una fila y se incrementa el valor de la tabla s `changeId` en `AspNet_SqlCacheTablesForChangeNotification` .

El tiempo de ejecución de ASP.NET realiza un seguimiento de la actual `changeId` para una tabla al almacenar datos en caché mediante un `SqlCacheDependency` objeto. La base de datos se comprueba periódicamente y se `SqlCacheDependency` expulsa cualquier objeto cuyo `changeId` valor difiere del valor de la base de datos, ya que un `changeId` valor diferente indica que se ha producido un cambio en la tabla desde que se almacenaron en caché los datos.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Paso 1: explorar el `aspnet_regsql.exe` programa de la línea de comandos

Con el enfoque de sondeo, la base de datos debe configurarse para que contenga la infraestructura descrita anteriormente: una tabla predefinida ( `AspNet_SqlCacheTablesForChangeNotification` ), una serie de procedimientos almacenados y desencadenadores en cada una de las tablas que se pueden usar en las dependencias de caché de SQL en la aplicación Web. Estas tablas, procedimientos almacenados y desencadenadores se pueden crear a través del programa de línea `aspnet_regsql.exe` de comandos, que se encuentra en la `$WINDOWS$\Microsoft.NET\Framework\version` carpeta. Para crear la `AspNet_SqlCacheTablesForChangeNotification` tabla y los procedimientos almacenados asociados, ejecute lo siguiente desde la línea de comandos:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Para ejecutar estos comandos, el inicio de sesión de base de datos especificado debe estar en los [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) roles y. Para examinar el T-SQL enviado a la base de datos mediante el `aspnet_regsql.exe` programa de línea de comandos, consulte [esta entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).

Por ejemplo, para agregar la infraestructura de sondeo a una base de datos de Microsoft SQL Server denominada `pubs` en un servidor de base de datos denominado `ScottsServer` con la autenticación de Windows, navegue hasta el directorio adecuado y, en la línea de comandos, escriba:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Una vez agregada la infraestructura de nivel de base de datos, es necesario agregar los desencadenadores a las tablas que se usarán en las dependencias de caché de SQL. `aspnet_regsql.exe`Vuelva a usar el programa de línea de comandos, pero especifique el nombre de la tabla mediante el `-t` modificador y, en lugar de usar el `-ed` modificador `-et` , como se indica a continuación:

[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Para agregar los desencadenadores a las `authors` `titles` tablas y de la `pubs` base de datos en `ScottsServer` , use:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

En este tutorial, agregue los desencadenadores a `Products` las `Categories` tablas, y `Suppliers` . Veremos la sintaxis de línea de comandos concreta en el paso 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Paso 2: hacer referencia a una base de datos de Microsoft SQL Server 2005 Express Edition en`App_Data`

El `aspnet_regsql.exe` programa de línea de comandos requiere la base de datos y el nombre del servidor para poder agregar la infraestructura de sondeo necesaria. Pero ¿cuál es el nombre del servidor y la base de datos de una base de datos Microsoft SQL Server Express 2005 que resida en la `App_Data` carpeta? En lugar de tener que detectar cuáles son los nombres de los servidores y la base de datos, encontramos que el enfoque más sencillo consiste en adjuntar la base de datos a la `localhost\SQLExpress` instancia de base de datos y cambiar el nombre de los datos mediante [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Si tiene una de las versiones completas de SQL Server 2005 instaladas en el equipo, es probable que ya tenga SQL Server Management Studio instalado en el equipo. Si solo tiene la edición Express, puede descargar el [Microsoft SQL Server gratuito Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Empiece por cerrar Visual Studio. A continuación, abra SQL Server Management Studio y elija conectarse al `localhost\SQLExpress` servidor mediante la autenticación de Windows.

![Adjuntar al servidor de localhost\SQLExpress](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figura 1**: conexión al `localhost\SQLExpress` servidor

Después de conectarse al servidor, Management Studio mostrará el servidor y tendrá subcarpetas para las bases de datos, la seguridad, etc. Haga clic con el botón derecho en la carpeta bases de datos y elija la opción adjuntar. Se abrirá el cuadro de diálogo Adjuntar bases de datos (consulte la figura 2). Haga clic en el botón Agregar y seleccione la `NORTHWND.MDF` carpeta de la base de datos en la carpeta de la aplicación Web `App_Data` .

[![Adjunte el archivo NORTHWND. Base de datos MDF de la carpeta App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figura 2**: Adjunte la `NORTHWND.MDF` base de datos de la `App_Data` carpeta ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image2.png))

Esto agregará la base de datos a la carpeta de bases de datos. El nombre de la base de datos puede ser la ruta de acceso completa al archivo de base de datos o la ruta de acceso completa antepuesto con un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar tener que escribir este nombre de base de datos largo cuando se usa la \_ herramienta de línea de comandos aspnetregsql.exe, cambie el nombre de la base de datos a un nombre más descriptivo; para ello, haga clic con el botón secundario en base de datos recién adjuntada y elija cambiar nombre. He cambiado el nombre de mi base de datos a tutoriales de datos.

![Cambiar el nombre de la base de datos adjunta a un nombre más descriptivo](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figura 3**: cambiar el nombre de la base de datos adjunta a un nombre más descriptivo

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Paso 3: agregar la infraestructura de sondeo a la base de datos Northwind

Ahora que hemos adjuntado la `NORTHWND.MDF` base de datos de la `App_Data` carpeta, se ha vuelto a preparar para agregar la infraestructura de sondeo. Suponiendo que ha cambiado el nombre de la base de datos a los tutoriales de datos, ejecute los cuatro comandos siguientes:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Después de ejecutar estos cuatro comandos, haga clic con el botón derecho en el nombre de la base de datos en Management Studio, vaya al submenú tareas y elija Desasociar. Después, cierre Management Studio y vuelva a abrir Visual Studio.

Una vez que Visual Studio se ha reabierto, profundice en la base de datos a través de la Explorador de servidores. Tenga en cuenta la nueva tabla ( `AspNet_SqlCacheTablesForChangeNotification` ), los nuevos procedimientos almacenados y los desencadenadores en `Products` las `Categories` tablas, y `Suppliers` .

![La base de datos ahora incluye la infraestructura de sondeo necesaria](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figura 4**: la base de datos ahora incluye la infraestructura de sondeo necesaria

## <a name="step-4-configuring-the-polling-service"></a>Paso 4: configurar el servicio de sondeo

Después de crear las tablas, desencadenadores y procedimientos almacenados necesarios en la base de datos, el último paso es configurar el servicio de sondeo, que se realiza mediante `Web.config` la especificación de las bases de datos que se van a usar y la frecuencia de sondeo en milisegundos. El marcado siguiente sondea la base de datos Northwind una vez por segundo.

[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

El `name` valor del `<add>` elemento (NorthwindDB) asocia un nombre legible con una base de datos determinada. Al trabajar con dependencias de caché de SQL, es necesario hacer referencia al nombre de la base de datos que se define aquí, así como a la tabla en la que se basan los datos almacenados en caché. Veremos cómo usar la `SqlCacheDependency` clase para asociar mediante programación las dependencias de caché de SQL con los datos almacenados en caché en el paso 6.

Una vez que se ha establecido una dependencia de caché de SQL, el sistema de sondeo se conectará a las bases de datos definidas en los `<databases>` elementos cada `pollTime` milisegundo y ejecutará el `AspNet_SqlCachePollingStoredProcedure` procedimiento almacenado. Este procedimiento almacenado, que se volvió a agregar en el paso 3 mediante la `aspnet_regsql.exe` herramienta de línea de comandos, devuelve los `tableName` `changeId` valores y para cada registro en `AspNet_SqlCacheTablesForChangeNotification` . Las dependencias de caché de SQL obsoletas se expulsan de la memoria caché.

La `pollTime` configuración presenta un equilibrio entre el rendimiento y la obsolescencia de los datos. Un `pollTime` valor pequeño aumenta el número de solicitudes a la base de datos, pero expulsa más rápidamente los datos obsoletos de la memoria caché. Un `pollTime` valor mayor reduce el número de solicitudes de base de datos, pero aumenta el retraso que transcurre entre el momento en que cambia el servidor back-end y el momento en que se expulsan los elementos de caché relacionados. Afortunadamente, la solicitud de base de datos está ejecutando un procedimiento almacenado simple que devuelve solo unas pocas filas de una tabla simple y ligera. Pero experimente con distintos `pollTime` valores para encontrar un equilibrio ideal entre el acceso a la base de datos y la obsolescencia de los datos de la aplicación. El valor más pequeño `pollTime` permitido es 500.

> [!NOTE]
> En el ejemplo anterior se proporciona un `pollTime` valor único en el `<sqlCacheDependency>` elemento, pero opcionalmente se puede especificar el `pollTime` valor en el `<add>` elemento. Esto es útil si tiene varias bases de datos especificadas y desea personalizar la frecuencia de sondeo por base de datos.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Paso 5: trabajar de forma declarativa con las dependencias de caché de SQL

En los pasos 1 a 4, analizamos cómo configurar la infraestructura de base de datos necesaria y configurar el sistema de sondeo. Con esta infraestructura implementada, ahora se pueden agregar elementos a la memoria caché de datos con una dependencia de caché de SQL asociada mediante técnicas de programación o declarativas. En este paso, examinaremos cómo trabajar mediante declaración con las dependencias de caché de SQL. En el paso 6 veremos el enfoque de programación.

Los [datos de almacenamiento en caché con el](caching-data-with-the-objectdatasource-vb.md) tutorial de ObjectDataSource exploraron las capacidades de almacenamiento en caché declarativo de ObjectDataSource. Simplemente estableciendo la `EnableCaching` propiedad en `True` y la `CacheDuration` propiedad en un intervalo de tiempo, ObjectDataSource almacenará automáticamente en caché los datos devueltos de su objeto subyacente para el intervalo especificado. ObjectDataSource también puede usar una o varias dependencias de caché de SQL.

Para mostrar mediante declaración las dependencias de caché de SQL, abra la `SqlCacheDependencies.aspx` página en la `Caching` carpeta y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca GridView s `ID` en `ProductsDeclarative` y, desde su etiqueta inteligente, elija enlazarlo a un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative` .

[![Cree un nuevo ObjectDataSource denominado ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figura 5**: creación de un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image4.png))

Configure ObjectDataSource para usar la `ProductsBLL` clase y establezca la lista desplegable de la pestaña seleccionar en `GetProducts()` . En la pestaña actualizar, elija la `UpdateProduct` sobrecarga con tres parámetros de entrada: `productName` , `unitPrice` y `productID` . Establezca las listas desplegables en (ninguno) en las pestañas insertar y eliminar.

[![Usar la sobrecarga UpdateProduct con tres parámetros de entrada](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figura 6**: uso de la sobrecarga UpdateProduct con tres parámetros de entrada ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image6.png))

[![Establecer la lista desplegable en (ninguno) para las pestañas insertar y eliminar](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figura 7**: establezca la lista desplegable en (ninguno) para las pestañas insertar y eliminar ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image8.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Quite todos los campos, pero, y `ProductName` `CategoryName` `UnitPrice` , y dé formato a estos campos como considere oportuno. En la etiqueta inteligente de GridView s, active las casillas habilitar paginación, habilitar ordenación y habilitar edición. Visual Studio establecerá la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}` . Para que la característica de edición de GridView funcione correctamente, quite esta propiedad completamente de la sintaxis declarativa o vuelva a establecerla en su valor predeterminado, `{0}` .

Por último, agregue un control Web Label sobre GridView y establezca su `ID` propiedad en `ODSEvents` y su `EnableViewState` propiedad en `False` . Después de realizar estos cambios, el marcado declarativo de la página debe ser similar al siguiente. Tenga en cuenta que he realizado varias personalizaciones estéticas en los campos de GridView que no son necesarios para mostrar la funcionalidad de dependencia de caché de SQL.

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el `Selecting` evento ObjectDataSource s y agregue el código siguiente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Recuerde que el evento ObjectDataSource s `Selecting` solo se activa cuando se recuperan datos de su objeto subyacente. Si ObjectDataSource obtiene acceso a los datos desde su propia memoria caché, este evento no se desencadena.

Ahora, visite esta página a través de un explorador. Puesto que todavía estamos implementando cualquier almacenamiento en caché, cada vez que se pagina, se ordena o se edita la cuadrícula, la página debe mostrar el texto "se ha desencadenado el evento, como se muestra en la figura 8.

[![El evento ObjectDataSource s Select se activa cada vez que se pagina, edita o ordena el control GridView.](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figura 8**: el evento ObjectDataSource s `Selecting` se activa cada vez que se pagina, se edita o se ordena GridView ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image10.png))

Como vimos en los [datos de almacenamiento en caché con el](caching-data-with-the-objectdatasource-vb.md) tutorial de ObjectDataSource, el establecimiento de la `EnableCaching` propiedad en `True` hace que ObjectDataSource almacene sus datos en la memoria caché durante la duración especificada por su `CacheDuration` propiedad. ObjectDataSource también tiene una [ `SqlCacheDependency` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que agrega una o más dependencias de caché de SQL a los datos almacenados en caché mediante el patrón:

[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Donde *DatabaseName* es el nombre de la base de datos tal y como se especifica en el `name` atributo del `<add>` elemento en `Web.config` , y *TableName* es el nombre de la tabla de base de datos. Por ejemplo, para crear un ObjectDataSource que almacene en memoria caché los datos de forma indefinida según una dependencia de caché de SQL en la tabla Northwind s `Products` , establezca la propiedad ObjectDataSource s `EnableCaching` en `True` y su `SqlCacheDependency` propiedad en NorthwindDB: Products.

> [!NOTE]
> Puede utilizar una dependencia de caché de SQL *y* una expiración basada en el tiempo estableciendo `EnableCaching` en `True` , `CacheDuration` en el intervalo de tiempo y `SqlCacheDependency` en los nombres de base de datos y tabla. ObjectDataSource expulsará sus datos cuando se alcance la expiración basada en el tiempo o cuando el sistema de sondeo tenga en cuenta que los datos de la base de datos subyacente han cambiado, lo que suceda primero.

GridView en `SqlCacheDependencies.aspx` muestra los datos de dos tablas: `Products` y `Categories` (el campo Product s `CategoryName` se recupera mediante un `JOIN` on `Categories` ). Por lo tanto, queremos especificar dos dependencias de caché de SQL: NorthwindDB: Products; NorthwindDB: categorías.

[![Configuración de ObjectDataSource para admitir el almacenamiento en caché mediante dependencias de caché de SQL en productos y categorías](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figura 9**: configuración de ObjectDataSource para admitir el almacenamiento en caché mediante dependencias de caché de SQL en `Products` y `Categories` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image12.png))

Después de configurar ObjectDataSource para admitir el almacenamiento en caché, vuelva a visitar la página a través de un explorador. De nuevo, el texto ' seleccionar evento desencadenado debe aparecer en la primera visita de la página, pero debe desaparecer al paginar, ordenar o hacer clic en los botones editar o cancelar. Esto se debe a que, una vez que los datos se cargan en la memoria caché de ObjectDataSource, permanece allí hasta que `Products` `Categories` se modifican las tablas o o se actualizan los datos a través de GridView.

Después de la paginación a través de la cuadrícula y de anotar la falta de texto que indica que se ha desencadenado el evento, abra una nueva ventana del explorador y vaya al tutorial de conceptos básicos de la sección edición, inserción y eliminación ( `~/EditInsertDelete/Basics.aspx` ). Actualice el nombre o el precio de un producto. A continuación, desde hasta la primera ventana del explorador, vea una página de datos diferente, ordene la cuadrícula o haga clic en el botón de edición de una fila. Esta vez, el evento de selección debe volver a aparecer, ya que se han modificado los datos de la base de datos subyacente (vea la figura 10). Si el texto no aparece, espere unos instantes e inténtelo de nuevo. Recuerde que el servicio de sondeo está comprobando los cambios en la `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre el momento en que se actualizan los datos subyacentes y el momento en que se expulsan los datos almacenados en caché.

[![La modificación de la tabla Products expulsa los datos del producto almacenados en caché](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figura 10**: modificación de la tabla de productos expulsa los datos de producto almacenados en caché ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Paso 6: trabajar mediante programación con la `SqlCacheDependency` clase

Los [datos de almacenamiento en caché en el](caching-data-in-the-architecture-vb.md) tutorial de arquitectura examinaron las ventajas de usar una capa de almacenamiento en caché independiente en la arquitectura en lugar de acoplar estrechamente el almacenamiento en caché con ObjectDataSource. En ese tutorial, creamos una `ProductsCL` clase para demostrar cómo trabajar con la memoria caché de datos mediante programación. Para usar las dependencias de caché de SQL en la capa de almacenamiento en caché, use la `SqlCacheDependency` clase.

Con el sistema de sondeo, un `SqlCacheDependency` objeto debe estar asociado a una base de datos y un par de tablas determinados. El código siguiente, por ejemplo, crea un `SqlCacheDependency` objeto basado en la tabla de base de datos Northwind `Products` :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Los dos parámetros de entrada del `SqlCacheDependency` constructor s son los nombres de base de datos y tabla, respectivamente. Al igual que con la `SqlCacheDependency` propiedad ObjectDataSource s, el nombre de la base de datos utilizado es el mismo que el valor especificado en el `name` atributo del `<add>` elemento en `Web.config` . El nombre de la tabla es el nombre real de la tabla de la base de datos.

Para asociar un `SqlCacheDependency` objeto a un elemento agregado a la memoria caché de datos, utilice una de las `Insert` sobrecargas del método que acepta una dependencia. En el código siguiente se agrega un *valor* a la memoria caché de datos durante una duración indefinida, pero se asocia a `SqlCacheDependency` en la `Products` tabla. En Resumen, el *valor* permanece en la memoria caché hasta que se expulsa debido a restricciones de memoria o porque el sistema de sondeo ha detectado que la `Products` tabla ha cambiado desde que se almacenó en caché.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La clase de nivel de almacenamiento en caché `ProductsCL` almacena actualmente los datos de la `Products` tabla mediante una expiración basada en el tiempo de 60 segundos. Permita actualizar esta clase para que use las dependencias de caché de SQL en su lugar. El `ProductsCL` método Class s `AddCacheItem` , que es responsable de agregar los datos a la memoria caché, actualmente contiene el código siguiente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Actualice este código para usar un `SqlCacheDependency` objeto en lugar de la `MasterCacheKeyArray` dependencia de caché:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Para probar esta funcionalidad, agregue un control GridView a la página debajo del control `ProductsDeclarative` GridView existente. Establezca este nuevo GridView s `ID` en `ProductsProgrammatic` y, a través de su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `ProductsDataSourceProgrammatic` . Configure ObjectDataSource para usar la `ProductsCL` clase, estableciendo las listas desplegables de las pestañas seleccionar y actualizar en `GetProducts` y `UpdateProduct` , respectivamente.

[![Configurar ObjectDataSource para usar la clase ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figura 11**: configuración de ObjectDataSource para usar la `ProductsCL` clase ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image16.png))

[![Seleccione el método GetProducts en la lista desplegable seleccionar s.](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figura 12**: seleccione el `GetProducts` método en la lista desplegable Seleccionar pestaña s ([haga clic para ver la imagen a tamaño completo](using-sql-cache-dependencies-vb/_static/image18.png))

[![Elija el método UpdateProduct en la lista desplegable actualizar pestaña s.](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figura 13**: elija el método UpdateProduct en la lista desplegable actualizar pestaña s ([haga clic para ver la imagen a tamaño completo](using-sql-cache-dependencies-vb/_static/image20.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Al igual que con la primera GridView agregada a esta página, quite todos los campos, `ProductName` `CategoryName` , y `UnitPrice` , y dé formato a estos campos como considere oportuno. En la etiqueta inteligente de GridView s, active las casillas habilitar paginación, habilitar ordenación y habilitar edición. Como con `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio establecerá la `ProductsDataSourceProgrammatic` propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}` . Para que la característica de edición de GridView funcione correctamente, vuelva a establecer esta propiedad en `{0}` (o quite la asignación de propiedad de la sintaxis declarativa por completo).

Después de completar estas tareas, el marcado declarativo de GridView y ObjectDataSource resultante debe ser similar al siguiente:

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Para probar la dependencia de caché de SQL en la capa de almacenamiento en caché, establezca un punto de interrupción en el `ProductCL` método Class s `AddCacheItem` y, a continuación, inicie la depuración. La primera vez que visita `SqlCacheDependencies.aspx` , se debe alcanzar el punto de interrupción, ya que los datos se solicitan por primera vez y se colocan en la memoria caché. A continuación, desplácese a otra página de GridView u ordene una de las columnas. Esto hace que GridView reconsulte sus datos, pero los datos se deben encontrar en la memoria caché, ya que la tabla de la base de datos `Products` no se ha modificado. Si los datos no se encuentran repetidamente en la memoria caché, asegúrese de que hay suficiente memoria disponible en el equipo e inténtelo de nuevo.

Después de paginar a través de algunas páginas de GridView, abra una segunda ventana del explorador y vaya al tutorial de conceptos básicos de la sección edición, inserción y eliminación ( `~/EditInsertDelete/Basics.aspx` ). Actualice un registro de la tabla productos y, a continuación, desde la primera ventana del explorador, vea una nueva página o haga clic en uno de los encabezados de ordenación.

En este escenario, verá una de estas dos cosas: se alcanzará el punto de interrupción, lo que indica que los datos almacenados en caché se expulsón debido al cambio en la base de datos. o bien, no se alcanzará el punto de interrupción, lo que significa que `SqlCacheDependencies.aspx` ahora muestra los datos obsoletos. Si no se alcanza el punto de interrupción, es probable que el servicio de sondeo todavía no se haya desencadenado desde que se cambiaron los datos. Recuerde que el servicio de sondeo está comprobando los cambios en la `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre el momento en que se actualizan los datos subyacentes y el momento en que se expulsan los datos almacenados en caché.

> [!NOTE]
> Es más probable que este retraso aparezca al editar uno de los productos a través de GridView en `SqlCacheDependencies.aspx` . En los [datos de almacenamiento en caché del tutorial de arquitectura](caching-data-in-the-architecture-vb.md) se ha agregado la `MasterCacheKeyArray` dependencia de caché para asegurarse de que los datos que se editan mediante el `ProductsCL` método Class s `UpdateProduct` se expulsón de la memoria caché. Sin embargo, reemplazamos esta dependencia de caché al modificar el `AddCacheItem` método anteriormente en este paso y, por tanto, la `ProductsCL` clase continuará mostrando los datos almacenados en caché hasta que el sistema de sondeo apunte el cambio a la `Products` tabla. Veremos cómo volver a introducir la `MasterCacheKeyArray` dependencia de caché en el paso 7.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Paso 7: asociar varias dependencias a un elemento almacenado en caché

Recuerde que la `MasterCacheKeyArray` dependencia de caché se usa para asegurarse de que *todos los* datos relacionados con el producto se expulsan de la memoria caché cuando se actualiza un único elemento asociado en él. Por ejemplo, el `GetProductsByCategoryID(categoryID)` método almacena en caché `ProductsDataTables` las instancias para cada valor *CategoryID* único. Si se expulsa uno de estos objetos, la `MasterCacheKeyArray` dependencia de caché garantiza que los demás también se quitan. Sin esta dependencia de caché, cuando se modifican los datos en caché, existe la posibilidad de que otros datos de productos almacenados en caché no estén actualizados. Por lo tanto, es importante que mantengamos la `MasterCacheKeyArray` dependencia de caché al usar las dependencias de caché de SQL. Sin embargo, el método de almacenamiento en caché `Insert` de datos solo permite un único objeto de dependencia.

Además, al trabajar con dependencias de caché de SQL, es posible que necesitemos asociar varias tablas de base de datos como dependencias. Por ejemplo, la `ProductsDataTable` almacenada en la memoria caché de la `ProductsCL` clase contiene los nombres de categoría y proveedor de cada producto, pero el `AddCacheItem` método solo utiliza una dependencia en `Products` . En esta situación, si el usuario actualiza el nombre de una categoría o proveedor, los datos del producto almacenados en caché permanecerán en la memoria caché y no estarán actualizados. Por lo tanto, deseamos que los datos del producto almacenados en la memoria caché dependan no solo de la `Products` tabla, sino también de las `Categories` `Suppliers` tablas y.

La [ `AggregateCacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) proporciona un medio para asociar varias dependencias a un elemento de la memoria caché. Empiece por crear una `AggregateCacheDependency` instancia de. A continuación, agregue el conjunto de dependencias mediante `AggregateCacheDependency` el `Add` método s. Al insertar el elemento en la memoria caché de datos después, pase la `AggregateCacheDependency` instancia. Cuando cambie *cualquiera* de las `AggregateCacheDependency` dependencias de las instancias, se expulsará el elemento almacenado en caché.

A continuación se muestra el código actualizado para el `ProductsCL` método Class s `AddCacheItem` . El método crea la `MasterCacheKeyArray` dependencia de caché junto con `SqlCacheDependency` los objetos para las `Products` `Categories` tablas, y `Suppliers` . Todos ellos se combinan en un `AggregateCacheDependency` objeto denominado `aggregateDependencies` , que se pasa al `Insert` método.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Pruebe este nuevo código. Ahora, los cambios en las `Products` `Categories` tablas, o `Suppliers` hacen que se expulsen los datos almacenados en caché. Además, el `ProductsCL` método Class s `UpdateProduct` , al que se llama al editar un producto a través de GridView, expulsa la `MasterCacheKeyArray` dependencia de caché, lo que hace que el almacenamiento en caché `ProductsDataTable` se expulse y que los datos se vuelvan a recuperar en la solicitud siguiente.

> [!NOTE]
> Las dependencias de caché de SQL también se pueden usar con el [almacenamiento en caché de resultados](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para obtener una demostración de esta funcionalidad, vea: [usar el almacenamiento en caché de resultados de ASP.net con SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Resumen

Al almacenar en caché los datos de la base de datos, los datos se conservarán idealmente en la memoria caché hasta que se modifiquen en la base de datos. Con ASP.NET 2,0, se pueden crear y usar dependencias de caché de SQL en escenarios declarativos y de programación. Uno de los desafíos de este enfoque es detectar cuándo se han modificado los datos. Las versiones completas de Microsoft SQL Server 2005 proporcionan funciones de notificación que pueden avisar a una aplicación cuando ha cambiado el resultado de una consulta. En el caso de la edición Express de SQL Server 2005 y versiones anteriores de SQL Server, se debe usar en su lugar un sistema de sondeo. Afortunadamente, la configuración de la infraestructura de sondeo necesaria es bastante sencilla.

¡ Feliz programación!

## <a name="further-reading"></a>Más información

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Uso de notificaciones de consulta en Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Crear una notificación de consulta](https://msdn.microsoft.com/library/ms188669.aspx)
- [Almacenar en caché en ASP.NET con la `SqlCacheDependency` clase](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Herramienta de registro de SQL Server ASP.NET ( `aspnet_regsql.exe` )](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Información general de `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET) .

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Marko Rangel, Teresa Murphy y Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, suéltelo en una línea en [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-at-application-startup-vb.md)
