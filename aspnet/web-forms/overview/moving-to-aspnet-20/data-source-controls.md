---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controles de origen de datos ? Microsoft Docs
author: rick-anderson
description: El control DataGrid de ASP.NET 1.x marcó una gran mejora en el acceso a datos en aplicaciones web. Sin embargo, no era tan fácil de usar como podría haber sido....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 2b4302b509af57dc5d9db9de9ee824df767d0737
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540225"
---
# <a name="data-source-controls"></a>Controles de origen de datos

por [Microsoft](https://github.com/microsoft)

> El control DataGrid de ASP.NET 1.x marcó una gran mejora en el acceso a datos en aplicaciones web. Sin embargo, no era tan fácil de usar como podría haber sido. Todavía requería una cantidad considerable de código para obtener mucha funcionalidad útil de él. Tal es el modelo en todos los esfuerzos de acceso a datos en 1.x.

El control DataGrid de ASP.NET 1.x marcó una gran mejora en el acceso a datos en aplicaciones web. Sin embargo, no era tan fácil de usar como podría haber sido. Todavía requería una cantidad considerable de código para obtener mucha funcionalidad útil de él. Tal es el modelo en todos los esfuerzos de acceso a datos en 1.x.

ASP.NET 2.0 aborda esto con en parte con controles de origen de datos. Los controles de origen de datos de ASP.NET 2.0 proporcionan a los desarrolladores un modelo declarativo para recuperar datos, mostrar datos y editar datos. El propósito de los controles de origen de datos es proporcionar una representación coherente de los datos a los controles enlazados a datos independientemente del origen de esos datos. En el corazón de los controles de origen de datos en ASP.NET 2.0 se encuentra la clase abstracta DataSourceControl. La clase DataSourceControl proporciona una implementación base de la interfaz IDataSource y la interfaz IListSource, la última de las cuales permite asignar el control de origen de datos como DataSource de un control enlazado a datos (a través de la nueva propiedad DataSourceId que se describe más adelante) y exponer los datos que hay como una lista. Cada lista de datos de un control de origen de datos se expone como un DataSourceView objeto. El acceso a las instancias de DataSourceView lo proporciona la interfaz IDataSource. Por ejemplo, el GetViewNames método devuelve un ICollection que le permite enumerar el DataSourceViews asociado a un control de origen de datos determinado y el GetView método le permite tener acceso a una determinada DataSourceView instancia por nombre.

Los controles de origen de datos no tienen interfaz de usuario. Se implementan como controles de servidor para que puedan admitir la sintaxis declarativa y para que tengan acceso al estado de página si lo desea. Los controles de origen de datos no representan ningún marcado HTML en el cliente.

> [!NOTE]
> Como verá más adelante, también hay ventajas de almacenamiento en caché obtenidas mediante el uso de controles de origen de datos.

## <a name="storing-connection-strings"></a>Almacenamiento de cadenas de conexión

Antes de empezar a ver cómo configurar los controles de origen de datos, debemos cubrir una nueva capacidad en ASP.NET 2.0 con respecto a las cadenas de conexión. ASP.NET 2.0 introduce una nueva sección en el archivo de configuración que le permite almacenar fácilmente cadenas de conexión que se pueden leer dinámicamente en tiempo de ejecución. La &lt;sección&gt; connectionStrings facilita el almacenamiento de cadenas de conexión.

El fragmento de código siguiente agrega una nueva cadena de conexión.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Al igual &lt;que&gt; con la &lt;sección&gt; appSettings, &lt;la sección&gt; connectionStrings aparece fuera de la sección system.web del archivo de configuración.

Para usar esta cadena de conexión, puede usar la sintaxis siguiente al establecer el atributo ConnectionString de un control de servidor.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

La &lt;sección&gt; connectionStrings también se puede cifrar para que no se exponga información confidencial. Esa capacidad se cubrirá en un módulo posterior.

## <a name="caching-data-sources"></a>Almacenamiento en caché de orígenes de datos

Cada DataSourceControl proporciona cuatro propiedades para configurar el almacenamiento en caché; EnableCaching, CacheDuration, CacheExpirationPolicy y CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching es una propiedad booleana que determina si el almacenamiento en caché está habilitado o no para el control de origen de datos.

## <a name="cacheduration-property"></a>Propiedad CacheDuration

La propiedad CacheDuration establece el número de segundos que la memoria caché sigue siendo válida. Establecer esta propiedad en **0** hace que la memoria caché siga siendo válida hasta que se invalide explícitamente.

## <a name="cacheexpirationpolicy-property"></a>Propiedad CacheExpirationPolicy

La propiedad CacheExpirationPolicy se puede establecer en **Absolute** o **Sliding**. Establecerlo en Absoluto significa que la cantidad máxima de tiempo que los datos se almacenarán en caché es el número de segundos especificado por el CacheDuration propiedad. Al establecerlo en Deslizamiento, el tiempo de expiración se restablece cuando se realiza cada operación.

## <a name="cachekeydependency-property"></a>Propiedad CacheKeyDependency

Si se especifica un valor de cadena para la propiedad CacheKeyDependency, ASP.NET configurará una nueva dependencia de caché basada en esa cadena. Esto le permite invalidar explícitamente la memoria caché simplemente cambiando o quitando CacheKeyDependency.

**Importante:** Si la suplantación está habilitada y el acceso al origen de datos y/o al contenido de los datos se basa en la identidad del cliente, se recomienda deshabilitar el almacenamiento en caché estableciendo EnableCaching en False. Si el almacenamiento en caché está habilitado en este escenario y un usuario distinto del usuario que solicitó originalmente los datos emite una solicitud, no se aplica la autorización al origen de datos. Los datos simplemente se servirán desde la memoria caché.

## <a name="the-sqldatasource-control"></a>El control SqlDataSource

El control SqlDataSource permite a un desarrollador tener acceso a los datos almacenados en cualquier base de datos relacional que admita ADO.NET. Puede utilizar el proveedor System.Data.SqlClient para tener acceso a una base de datos de SQL Server, el proveedor System.Data.OleDb, el proveedor System.Data.Odbc o el proveedor System.Data.OracleClient para tener acceso a Oracle. Por lo tanto, SqlDataSource ciertamente no solo se usa para tener acceso a los datos de una base de datos de SQL Server.

Para usar SqlDataSource, simplemente proporcione un valor para la propiedad ConnectionString y especifique un comando SQL o un procedimiento almacenado. El control SqlDataSource se encarga de trabajar con la arquitectura de ADO.NET subyacente. Abre la conexión, consulta el origen de datos o ejecuta el procedimiento almacenado, devuelve los datos y, a continuación, cierra la conexión automáticamente.

> [!NOTE]
> Dado que la clase DataSourceControl cierra automáticamente la conexión, debe reducir el número de llamadas de cliente generadas por la pérdida de conexiones de base de datos.

El fragmento de código siguiente enlaza un DropDownList control a un SqlDataSource control mediante la cadena de conexión que se almacena en el archivo de configuración como se muestra anteriormente.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Como se muestra anteriormente, el DataSourceMode propiedad de la SqlDataSource especifica el modo para el origen de datos. En el ejemplo anterior, el DataSourceMode se establece en DataReader. En ese caso, el SqlDataSource devolverá un IDataReader objeto mediante un cursor de solo avance y de solo lectura. El tipo especificado de objeto que se devuelve está controlado por el proveedor que se usa. En este caso, estoy usando el proveedor System.Data.SqlClient como se especifica en la &lt;sección connectionStrings&gt; del archivo web.config. Por lo tanto, el objeto que se devuelve será de tipo SqlDataReader. Al especificar un DataSourceMode valor de DataSet, los datos se pueden almacenar en un DataSet en el servidor. Este modo le permite añadir características como clasificación, paginación, etc. Si hubiera estado enlazando datos sqlDataSource a un control GridView, habría elegido el modo DataSet. Sin embargo, en el caso de un DropDownList, el modo DataReader es la opción correcta.

> [!NOTE]
> Al almacenar en caché un SqlDataSource o un AccessDataSource, el DataSourceMode propiedad debe establecerse en DataSet. Se producirá una excepción si habilita el almacenamiento en caché con un DataSourceMode de DataReader.

## <a name="sqldatasource-properties"></a>Propiedades sqlDataSource

A continuación se muestran algunas de las propiedades del control SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valor booleano que especifica si se cancela un comando select si uno de los parámetros es null. De forma predeterminada es True.

### <a name="conflictdetection"></a>ConflictDetection

En una situación en la que varios usuarios pueden actualizar un origen de datos al mismo tiempo, el ConflictDetection propiedad determina el comportamiento de la SqlDataSource control. Esta propiedad se evalúa como uno de los valores de la ConflictOptions enumeración. Esos valores son **CompareAllValues** y **OverwriteChanges**. Si se establece en OverwriteChanges, la última persona que escriba datos en el origen de datos sobrescribe los cambios anteriores. Sin embargo, si el ConflictDetection propiedad está establecida en CompareAllValues, los parámetros se crean para las columnas devueltas por el SelectCommand y los parámetros también se crean para contener los valores originales en cada una de esas columnas que permiten el SqlDataSource para determinar si los valores han cambiado desde que se ejecutó SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Establece u obtiene la cadena SQL utilizada al eliminar filas de la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="deletecommandtype"></a>DeleteCommandType

Establece u obtiene el tipo de comando delete, ya sea una consulta SQL (Text) o un procedimiento almacenado (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Devuelve los parámetros que usa deleteCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Esta propiedad se utiliza para especificar el formato de los parámetros de valor original en los casos en que el ConflictDetection propiedad está establecida en CompareAllValues. El valor {0} predeterminado es lo que significa que los parámetros de valor original tomarán el mismo nombre que el parámetro original. En otras palabras, si el nombre del campo @EmployeeIDes EmployeeID, el parámetro de valor original sería .

### <a name="selectcommand"></a>SelectCommand

Establece u obtiene la cadena SQL que se usa para recuperar datos de la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="selectcommandtype"></a>SelectCommandType

Establece u obtiene el tipo de comando select, ya sea una consulta SQL (Text) o un procedimiento almacenado (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Devuelve los parámetros que usa SelectCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtiene o establece el nombre de un parámetro de procedimiento almacenado que se usa al ordenar los datos recuperados por el control de origen de datos. Válido solo cuando SelectCommandType está establecido en StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Cadena delimitada por punto y coma que especifica las bases de datos y tablas utilizadas en una dependencia de caché de SQL ServerSQL Server . (Las dependencias de caché SQL se tratarán en un módulo posterior.)

### <a name="updatecommand"></a>Updatecommand

Establece u obtiene la cadena SQL que se usa al actualizar datos en la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="updatecommandtype"></a>UpdateCommandType

Establece u obtiene el tipo de comando de actualización, ya sea una consulta SQL (Texto) o un procedimiento almacenado (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Devuelve los parámetros que usa UpdateCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

## <a name="the-accessdatasource-control"></a>El control AccessDataSource

El AccessDataSource control se deriva de la SqlDataSource clase y se utiliza para enlazar datos a una base de datos de Microsoft Access. El ConnectionString propiedad para el AccessDataSource control es una propiedad de solo lectura. En lugar de usar la propiedad ConnectionString, la propiedad DataFile se utiliza para apuntar a la base de datos de Access como se muestra a continuación.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource siempre establecerá el ProviderName de la base SqlDataSource en System.Data.OleDb y se conecta a la base de datos mediante el Microsoft.Jet.OLEDB.4.0 proveedor OLE DB. No puede usar el control AccessDataSource para conectarse a una base de datos de Access protegida con contraseña. Si tiene que conectarse a una base de datos protegida por contraseña, debe usar el Control SqlDataSource.

> [!NOTE]
> Las bases de datos de acceso almacenadas en el sitio Web deben colocarse en el directorio Datos de la aplicación.\_ ASP.NET no permite que se exploren los archivos de este directorio. Deberá conceder a la cuenta de proceso permisos de lectura y escritura al directorio De datos de la aplicación\_al usar las bases de datos de Access.

## <a name="the-xmldatasource-control"></a>El control XmlDataSource

El XmlDataSource se utiliza para enlazar datos XML a controles enlazados a datos. Puede enlazar a un archivo XML mediante el DataFile propiedad o puede enlazar a una cadena XML mediante el Data propiedad. El XmlDataSource expone atributos XML como campos enlazables. En los casos en los que necesite enlazar a valores que no están representados como atributos, deberá utilizar una transformación XSL. También puede utilizar expresiones XPath para filtrar datos XML.

Considere el siguiente archivo XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Observe que El XmlDataSource utiliza una propiedad XPath de *People/Person* para filtrar solo los &lt;nodos Person.&gt; El DropDownList, a continuación, enlaza datos a la LastName atributo mediante el DataTextField propiedad.

Aunque el xmlDataSource control se utiliza principalmente para enlazar datos a datos XML de solo lectura, es posible editar el archivo de datos XML. Tenga en cuenta que, en estos casos, la inserción automática, la actualización y la eliminación de información en el archivo XML no se producen automáticamente como lo hace con otros controles de origen de datos. En su lugar, tendrá que escribir código para editar manualmente los datos mediante los siguientes métodos del control XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera un objeto XmlDocument que contiene el código XML recuperado por XmlDataSource.

### <a name="save"></a>Save

Guarda el XmlDocument en memoria en el origen de datos.

Es importante tener en cuenta que el método Save solo funcionará cuando se cumplan las dos condiciones siguientes:

1. El XmlDataSource está utilizando el DataFile propiedad para enlazar a un archivo XML en lugar de la Data propiedad para enlazar a datos XML en memoria.
2. No se especifica ninguna transformación a través de la Transform o TransformFile propiedad.

Tenga en cuenta también que el Save método puede producir resultados inesperados cuando se llama por varios usuarios simultáneamente.

## <a name="the-objectdatasource-control"></a>El ObjectDataSource Control

Los controles de origen de datos que hemos cubierto hasta este punto son excelentes opciones para aplicaciones de dos niveles donde el control de origen de datos se comunica directamente con el almacén de datos. Sin embargo, muchas aplicaciones del mundo real son aplicaciones de varios niveles en las que un control de origen de datos puede necesitar comunicarse con un objeto de negocio que, a su vez, se comunica con la capa de datos. En estas situaciones, el origen ObjectDataSource rellena la factura muy bien. ObjectDataSource funciona junto con un objeto de origen. El ObjectDataSource control creará una instancia del objeto de origen, llamar al método especificado y eliminar la instancia de objeto todo dentro del ámbito de una sola solicitud, si el objeto tiene métodos de instancia en lugar de métodos estáticos (Shared en Visual Basic). Por lo tanto, el objeto debe ser sin estado. Es decir, el objeto debe adquirir y liberar todos los recursos necesarios en el intervalo de una sola solicitud. Puede controlar cómo se crea el objeto de origen controlando el evento ObjectCreating del control ObjectDataSource. Puede crear una instancia del objeto de origen y, a continuación, establecer la propiedad ObjectInstance de la clase ObjectDataSourceEventArgs en esa instancia. El ObjectDataSource control usará la instancia que se crea en el ObjectCreating eventos en lugar de crear una instancia por sí mismo.

Si el objeto de origen de un ObjectDataSource control expone métodos estáticos públicos (Shared en Visual Basic) que se pueden llamar para recuperar y modificar datos, un ObjectDataSource control llamará a esos métodos directamente. Si un ObjectDataSource control debe crear una instancia del objeto de origen para realizar llamadas a métodos, el objeto debe incluir un constructor público que no toma ningún parámetro. El ObjectDataSource control llamará a este constructor cuando crea una nueva instancia del objeto de origen.

Si el objeto de origen no contiene un constructor público sin parámetros, puede crear una instancia del objeto de origen que usará el ObjectDataSource control en el ObjectCreating eventos.

## <a name="specifying-object-methods"></a>Especificación de métodos de objeto

El objeto de origen de un ObjectDataSource control puede contener cualquier número de métodos que se utilizan para seleccionar, insertar, actualizar o eliminar datos. El objectDataSource control denomina a estos métodos en función del nombre del método, tal como se identifica mediante el uso de la SelectMethod, InsertMethod, UpdateMethod, o DeleteMethod propiedad de la ObjectDataSource control. El objeto de origen también puede incluir un método SelectCount opcional, que se identifica mediante el control ObjectDataSource mediante la propiedad SelectCountMethod, que devuelve el recuento del número total de objetos en el origen de datos. El ObjectDataSource control llamará a la SelectCount método después de un Select se ha llamado al método para recuperar el número total de registros en el origen de datos para su uso al paginar.

## <a name="lab-using-data-source-controls"></a>Laboratorio mediante controles de origen de datos

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ejercicio 1 - Mostrar datos con el Control SqlDataSource

El ejercicio siguiente usa el control SqlDataSource para conectarse a la base de datos Northwind. Se supone que tiene acceso a la base de datos Northwind en una instancia de SQL Server 2000.

1. Cree un nuevo sitio web de ASP.NET.
2. Agregue un nuevo archivo web.config.

    1. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
    2. Elija Web Configuration File (Archivo de configuración web) en la lista de plantillas y haga clic en Add (Agregar).
3. Edite &lt;la&gt; sección connectionStrings de la siguiente manera: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Cambie a la vista Código y agregue un atributo &lt;ConnectionString y&gt; un atributo SelectCommand al control asp:SqlDataSource de la siguiente manera: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. En la vista Diseño, agregue un nuevo control GridView.
6. En el elegir origen de datos menú desplegable en el GridView tareas menú, elija SqlDataSource1.
7. Haga clic con el botón derecho en Default.aspx y elija Ver en el explorador en el menú. Haga clic en Sí cuando se le pida que guarde.
8. El control GridView muestra los datos de la Products tabla.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ejercicio 2 - Edición de datos con el control SqlDataSource

En el siguiente ejercicio se muestra cómo enlazar datos un DropDownList control mediante la sintaxis declarativa y le permite editar los datos presentados en el DropDownList control.

1. En la vista Diseño, elimine el control GridView de Default.aspx. 

    **Importante:** Deje el control SqlDataSource en la página.
2. Agregue un control DropDownList a Default.aspx.
3. Cambie a la vista Origen.
4. Agregue un atributo DataSourceId, DataTextField y &lt;DataValueField al&gt; control asp:DropDownList de la siguiente manera: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Guarde Default.aspx y visualíselo en el explorador. Tenga en cuenta que El DropDownList contiene todos los productos de la base de datos Northwind.
6. Cierre el explorador.
7. En la vista Origen de Default.aspx, agregue un nuevo control TextBox debajo del control DropDownList. Cambie la propiedad ID de TextBox a txtProductName.
8. En el TextBox control, agregue un nuevo Button control. Cambie la propiedad ID de Button a btnUpdate y la propiedad Text a **Update Product Name**.
9. En la vista Origen de Default.aspx, agregue una propiedad UpdateCommand y dos nuevos UpdateParameters a la etiqueta SqlDataSource de la siguiente manera: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Tenga en cuenta que hay dos parámetros de actualización (ProductName y ProductID) agregados en este código. Estos parámetros se asignan a la Text propiedad de la txtProductName TextBox y el SelectedValue propiedad de la ddlProducts DropDownList.
10. Cambie a la vista Diseño y haga doble clic en el control Button para agregar un controlador de eventos.
11. Agregue el siguiente código al\_código btnUpdate Click: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Haga clic con el botón derecho en Default.aspx y elija verlo en el explorador. Haga clic en Sí cuando se le pida que guarde todos los cambios.
13. ASP.NET clases parciales 2.0 permiten la compilación en tiempo de ejecución. No es necesario compilar una aplicación para que los cambios de código surtan efecto.
14. Seleccione un producto de DropDownList.
15. Escriba un nuevo nombre para el producto seleccionado en el cuadro de texto y, a continuación, haga clic en el botón Actualizar.
16. El nombre del producto se actualiza en la base de datos.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ejercicio 3 Uso del control ObjectDataSource

Este ejercicio demostrará cómo utilizar el ObjectDataSource control y un objeto de origen para interactuar con la base de datos Northwind.

1. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
2. Seleccione Formulario web en la lista de plantillas. Cambie el nombre a object.aspx y haga clic en Agregar.
3. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
4. Seleccione Clase en la lista de plantillas. Cambie el nombre de la clase a NorthwindData.cs y haga clic en Agregar.
5. Haga clic en Sí cuando se le\_pida que agregue la clase a la carpeta Código de aplicación.
6. Agregue el código siguiente al archivo NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Agregue el código siguiente a la vista Origen de object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Guarde todos los archivos y examine object.aspx.
9. Interactúe con la interfaz viendo detalles, editando empleados, agregando empleados y eliminando empleados.
