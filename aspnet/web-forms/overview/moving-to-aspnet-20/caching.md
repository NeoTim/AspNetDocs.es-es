---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Almacenamiento en caché de la página de almacenamiento en caché de Microsoft Docs
author: rick-anderson
description: Una comprensión del almacenamiento en caché es importante para una aplicación de ASP.NET de buen rendimiento. ASP.NET 1.x ofrecía tres opciones diferentes para el almacenamiento en caché; almacenamiento en caché de salida,...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: a199a9c0352dfb054e8d4e5e67652db9bd38851c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542942"
---
# <a name="caching"></a>Almacenamiento en memoria caché

por [Microsoft](https://github.com/microsoft)

> Una comprensión del almacenamiento en caché es importante para una aplicación de ASP.NET de buen rendimiento. ASP.NET 1.x ofrecía tres opciones diferentes para el almacenamiento en caché; almacenamiento en caché de resultados, almacenamiento en caché de fragmentos y la API de caché.

Una comprensión del almacenamiento en caché es importante para una aplicación de ASP.NET de buen rendimiento. ASP.NET 1.x ofrecía tres opciones diferentes para el almacenamiento en caché; almacenamiento en caché de resultados, almacenamiento en caché de fragmentos y la API de caché. ASP.NET 2.0 ofrece estos tres métodos, pero añade algunas características adicionales significativas. Hay varias dependencias de caché nuevas y los desarrolladores ahora tienen la opción de crear dependencias de caché personalizadas también. La configuración del almacenamiento en caché también se ha mejorado significativamente en ASP.NET 2.0.

## <a name="new-features"></a>Nuevas características

## <a name="cache-profiles"></a>Perfiles de caché

Los perfiles de caché permiten a los desarrolladores definir valores de caché específicos que, a continuación, se pueden aplicar a páginas individuales. Por ejemplo, si tiene algunas páginas que deben caducar de la memoria caché después de 12 horas, puede crear fácilmente un perfil de caché que se pueda aplicar a esas páginas. Para agregar un nuevo perfil &lt;de&gt; caché, utilice la sección outputCacheSettings del archivo de configuración. Por ejemplo, a continuación se muestra la configuración de un perfil de caché denominado *twoday* que configura una duración de caché de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar este perfil de caché a una página determinada, utilice el atributo CacheProfile de la directiva de OutputCache como se muestra a continuación:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependencias de caché personalizadas

ASP.NET desarrolladores de 1.x criticó las dependencias de caché personalizadas. En ASP.NET 1.x, la clase CacheDependency se selló, lo que impedía a los desarrolladores derivar sus propias clases de ella. En ASP.NET 2.0, esa limitación se elimina y los desarrolladores pueden desarrollar sus propias dependencias de caché personalizadas. La clase CacheDependency permite la creación de una dependencia de caché personalizada basada en archivos, directorios o claves de caché.

Por ejemplo, el código siguiente crea una nueva dependencia de caché personalizada basada en un archivo denominado stuff.xml ubicado en la raíz de la aplicación web:

[!code-csharp[Main](caching/samples/sample3.cs)]

En este escenario, cuando cambia el archivo stuff.xml, se invalida el elemento almacenado en caché.

También es posible crear una dependencia de caché personalizada mediante claves de caché. Con este método, la eliminación de la clave de caché invalidará los datos almacenados en caché. Esto se ilustra en el ejemplo siguiente:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar el elemento que se insertó anteriormente, simplemente quite el elemento que se insertó en la memoria caché para que actúe como clave de caché.

[!code-csharp[Main](caching/samples/sample5.cs)]

Tenga en cuenta que la clave del elemento que actúa como clave de caché debe ser la misma que el valor agregado a la matriz de claves de caché.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dependencias de caché SQL basadas en sondeos (también denominadas dependencias basadas en tablas)

SQL Server 7 y 2000 usan el modelo basado en sondeo para las dependencias de caché SQL. El modelo basado en sondeo sesión utiliza un desencadenador en una tabla de base de datos que se desencadena cuando cambian los datos de la tabla. Ese desencadenador actualiza un campo **changeId** en la tabla de notificaciones que ASP.NET comprueba periódicamente. Si se ha actualizado el campo **changeId,** ASP.NET sabe que los datos han cambiado e invalida los datos almacenados en caché.

> [!NOTE]
> SQL Server 2005 también puede usar el modelo basado en sondeo, pero como el modelo basado en sondeo no es el modelo más eficaz, es recomendable usar un modelo basado en consultas (discutido más adelante) con SQL Server 2005.

Para que una dependencia de caché SQL que usa el modelo basado en sondeo funcione correctamente, las tablas deben tener habilitadas las notificaciones. Esto se puede lograr mediante programación mediante la clase\_SqlCacheDependencyAdmin o mediante la utilidad aspnet regsql.exe.

La siguiente línea de comandos registra la tabla Products en la base de datos Northwind ubicada en una instancia de SQL Server denominada *dbase* para la dependencia de caché SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

La siguiente es una explicación de los modificadores de línea de comandos utilizados en el comando anterior:

| **Interruptor de línea de comandos** | **Propósito** |
| --- | --- |
| -S *server* | Especifica el nombre del servidor. |
| -ed | Especifica que la base de datos debe estar habilitada para la dependencia de caché SQL. |
| -d *\_Nombre* de la base de datos | Especifica el nombre de base de datos que se debe habilitar para la dependencia de caché SQL. |
| -E | Especifica que aspnet\_regsql debe usar la autenticación de Windows al conectarse a la base de datos. |
| -et | Especifica que estamos habilitando una tabla de base de datos para la dependencia de caché SQL. |
| -t *\_nombre* de la tabla | Especifica el nombre de la tabla de base de datos que se habilitará para la dependencia de caché SQL. |

> [!NOTE]
> Hay otros modificadores disponibles\_para aspnet regsql.exe. Para obtener una lista\_completa, ejecute aspnet regsql.exe -? desde una línea de comandos.

Cuando se ejecuta este comando, se realizan los siguientes cambios en la base de datos de SQL Server:

- Se agrega una tabla **SqlCacheTablesForChangeNotification de AspNet.\_** Esta tabla contiene una fila para cada tabla de la base de datos para la que se ha habilitado una dependencia de caché SQL.
- Los siguientes procedimientos almacenados se crean dentro de la base de datos:

| AspNet\_SqlCachePollingStoredProcedure | Consulta la tabla\_SqlCacheTablesForChangeNotification de AspNet y devuelve todas las tablas habilitadas para la dependencia de caché SQL y el valor de changeId para cada tabla. Este proceso almacenado se utiliza para el sondeo para determinar si los datos han cambiado. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Devuelve todas las tablas habilitadas para la dependencia\_de caché SQL consultando la tabla SqlCacheTablesForChangeNotification de AspNet y devuelve todas las tablas habilitadas para la dependencia de caché SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabla para la dependencia de caché SQL agregando la entrada necesaria en la tabla de notificación y agrega el desencadenador. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Anula el registro de una tabla para la dependencia de caché SQL quitando la entrada de la tabla de notificaciones y quita el desencadenador. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Actualiza la tabla de notificaciones incrementando el changeId de la tabla modificada. ASP.NET utiliza este valor para determinar si los datos han cambiado. Como se indica a continuación, este proceso almacenado es ejecutado por el desencadenador creado cuando se habilita la tabla. |

- Se crea un desencadenador de SQL Server denominado ** _\_nombre_\_de tabla AspNet\_SqlCacheNotification\_Trigger** para la tabla. Este desencadenador ejecuta AspNet\_SqlCacheUpdateChangeIdStoredProcedure cuando se realiza un INSERT, UPDATE o DELETE en la tabla.
- Un rol de SQL Server denominado **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** se agrega a la base de datos.

El rol **de ASPnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server tiene permisos EXEC para AspNet\_SqlCachePollingStoredProcedure. Para que el modelo de sondeo funcione correctamente, debe agregar\_la\_cuenta de proceso al rol Aspnet ChangeNotification ReceiveNotificationsOnlyAccess. La herramienta\_aspnet regsql.exe no hará esto por usted.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configuración de dependencias de caché SQL basadas en sondeos

Hay varios pasos necesarios para configurar las dependencias de caché SQL basadas en sondeos. El primer paso es habilitar la base de datos y la tabla como se ha explicado anteriormente. Una vez que ese paso se ha completado, el resto de la configuración es la siguiente:

- Configuración del archivo de configuración de ASP.NET.
- Configuración de SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configuración del archivo de configuración de ASP.NET

Además de agregar una cadena de conexión como se &lt;describe&gt; en &lt;un módulo&gt; anterior, también debe configurar un elemento de caché con un elemento sqlCacheDependency como se muestra a continuación:

[!code-xml[Main](caching/samples/sample7.xml)]

Esta configuración habilita una dependencia de caché SQL en la base de datos *pubs.* Tenga en cuenta que &lt;el atributo&gt; pollTime del elemento sqlCacheDependency tiene un valor predeterminado de 60000 milisegundos o 1 minuto. (Este valor no puede ser inferior a 500 milisegundos.) En este ejemplo, el &lt;elemento add&gt; agrega una nueva base de datos e invalida pollTime, estableciéndola en 9000000 milisegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configuración de SqlCacheDependency

El siguiente paso es configurar SqlCacheDependency. La forma más fácil de lograrlo es especificar el valor del atributo SqlDependency en la directiva de Outcache de la siguiente manera:

[!code-aspx[Main](caching/samples/sample8.aspx)]

En la directiva anterior , OutputCache, se configura una dependencia de caché SQL para la tabla *authors* en la base de datos *pubs.* Se pueden configurar varias dependencias separándolas con un punto y coma como este:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Otro método de configuración de SqlCacheDependency es hacerlo mediante programación. El código siguiente crea una nueva dependencia de caché SQL en la tabla *authors* de la base de datos *pubs.*

[!code-csharp[Main](caching/samples/sample10.cs)]

Una de las ventajas de definir mediante programación la dependencia de caché SQL es que puede controlar las excepciones que puedan producirse. Por ejemplo, si intenta definir una dependencia de caché SQL para una base de datos que no se ha habilitado para la notificación, se producirá una excepción **DatabaseNotEnabledForNotificationException.** En ese caso, puede intentar habilitar la base de datos para las notificaciones llamando al método **SqlCacheDependencyAdmin.EnableNotifications** y pasándole el nombre de la base de datos.

Del mismo modo, si intenta definir una dependencia de caché SQL para una tabla que no se ha habilitado para la notificación, se producirá una **excepción TableNotEnabledForNotificationException.** A continuación, puede llamar al método **SqlCacheDependencyAdmin.EnableTableForNotifications** pasándole el nombre de la base de datos y el nombre de la tabla.

En el ejemplo de código siguiente se muestra cómo configurar correctamente el control de excepciones al configurar una dependencia de caché SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Más información:[https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependencias de caché SQL basadas en consultas (solo SQL Server 2005)

Cuando se usa SQL Server 2005 para la dependencia de caché SQL, el modelo basado en sondeo no es necesario. Cuando se usa con SQL Server 2005, las dependencias de caché SQL se comunican directamente a través de conexiones SQL a la instancia de SQL Server (no es necesaria ninguna configuración adicional) mediante notificaciones de consulta de SQL Server 2005.

La forma más sencilla de habilitar la notificación basada en consultas es hacerlo mediante declaración estableciendo el atributo **SqlCacheDependency** del objeto de origen de datos en **CommandNotification** y estableciendo el atributo **EnableCaching** en **true**. Con este método, no se requiere código. Si cambia el resultado de un comando ejecutado en el origen de datos, invalidará los datos de caché.

En el ejemplo siguiente se configura un control de origen de datos para la dependencia de caché SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

En este caso, si la consulta especificada en **SelectCommand** devuelve un resultado diferente al que tenía originalmente, los resultados que se almacenan en caché se invalidan.

También puede especificar que todos los orígenes de datos estén habilitados para las dependencias **CommandNotification**de caché SQL estableciendo el atributo **SqlDependency** de la directiva **.** El ejemplo siguiente ilustra esto.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obtener más información sobre las notificaciones de consulta en SQL Server 2005, vea los Libros en pantalla de SQL Server.

Otro método para configurar una dependencia de caché SQL basada en consultas es hacerlo mediante programación mediante la clase SqlCacheDependency. En el ejemplo de código siguiente se muestra cómo se logra esto.

[!code-csharp[Main](caching/samples/sample14.cs)]

Más información:[https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sustitución posterior a la caché

El almacenamiento en caché de una página puede aumentar considerablemente el rendimiento de una aplicación web. Sin embargo, en algunos casos necesita que la mayor parte de la página se almacene en caché y que algunos fragmentos de la página sean dinámicos. Por ejemplo, si crea una página de noticias que es totalmente estática para períodos de tiempo establecidos, puede establecer que toda la página se almacene en caché. Si desea incluir un banner de anuncio giratorio que haya cambiado en cada solicitud de página, la parte de la página que contiene el anuncio debe ser dinámica. Para permitirle almacenar en caché una página pero sustituir un contenido dinámicamente, puede utilizar ASP.NET sustitución posterior a la caché. Con la sustitución posterior a la caché, toda la página se almacena en caché con partes específicas marcadas como exentas del almacenamiento en caché. En el ejemplo de los banners de anuncios, el control AdRotator permite aprovechar la sustitución posterior a la caché para que los anuncios creados dinámicamente para cada usuario y para cada actualización de página.

Hay tres maneras de implementar la sustitución posterior a la caché:

- Declarativamente, mediante el control Sustitución.
- Mediante programación, mediante la API de control de sustitución.
- Implícitamente, mediante el Control AdRotator.

### <a name="substitution-control"></a>Control de sustitución

El ASP.NET el control Substitution especifica una sección de una página almacenada en caché que se crea dinámicamente en lugar de almacenarse en caché. Coloque un control Substitution en la ubicación de la página donde desea que aparezca el contenido dinámico. En tiempo de ejecución, el control Substitution llama a un método que especifique con la propiedad MethodName. El método debe devolver una cadena, que, a continuación, reemplaza el contenido de la Substitution control. El método debe ser un método estático en el control Page o UserControl que lo contiene. El uso del control de sustitución hace que la capacidad de caché del lado cliente se cambie a la capacidad de caché del servidor, de modo que la página no se almacene en caché en el cliente. Esto garantiza que las solicitudes futuras a la página vuelvan a llamar al método para generar contenido dinámico.

### <a name="substitution-api"></a>API de sustitución

Para crear contenido dinámico para una página almacenada en caché mediante programación, puede llamar al método [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) en el código de página, pasándole el nombre de un método como parámetro. El método que controla la creación del contenido dinámico toma un único parámetro [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) y devuelve una cadena. La cadena de retorno es el contenido que se sustituirá en la ubicación especificada. Una ventaja de llamar a la WriteSubstitution método en lugar de usar el Substitution control mediante declaración es que puede llamar a un método de cualquier objeto arbitrario en lugar de llamar a un método estático de la Page o el UserControl objeto.

Llamar al método WriteSubstitution hace que la capacidad de caché del lado cliente se cambie a la capacidad de caché del servidor, de modo que la página no se almacene en caché en el cliente. Esto garantiza que las solicitudes futuras a la página vuelvan a llamar al método para generar contenido dinámico.

### <a name="adrotator-control"></a>AdRotator Control

El control de servidor AdRotator implementa la compatibilidad con la sustitución posterior a la memoria caché internamente. Si coloca un control AdRotator en su página, representará anuncios únicos en cada solicitud, independientemente de si la página principal se almacena en caché. Como resultado, una página que incluye un control AdRotator solo se almacena en caché en el lado del servidor.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy (Clase)

La clase ControlCachePolicy permite el control mediante programación del almacenamiento en caché de fragmentos mediante controles de usuario. ASP.NET incrusta controles de usuario en una instancia [de BasePartialCachingControl.](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) La clase BasePartialCachingControl representa un control de usuario que tiene habilitado el almacenamiento en caché de resultados.

Al tener acceso a la [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) propiedad de un [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) control, siempre recibirá un válido ControlCachePolicy objeto. Sin embargo, si tiene acceso a la [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) propiedad de un [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) control, recibirá un válido ControlCachePolicy objeto solo si el control de usuario ya está ajustado por un BasePartialCachingControl control. Si no se ajusta, el ControlCachePolicy objeto devuelto por la propiedad producirá excepciones cuando se intenta manipular porque no tiene asociado BasePartialCachingControl. Para determinar si una instancia de UserControl admite el almacenamiento en caché sin generar excepciones, inspeccione la propiedad [SupportsCaching.](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)

El uso de la clase ControlCachePolicy es una de varias maneras de habilitar el almacenamiento en caché de resultados. En la lista siguiente se describen los métodos que puede utilizar para habilitar el almacenamiento en caché de resultados:

- Utilice la directiva [de OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) para habilitar el almacenamiento en caché de resultados en escenarios declarativos.
- Utilice el atributo [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) para habilitar el almacenamiento en caché de un control de usuario en un archivo de código subyacente.
- Utilice la controlCachePolicy clase para especificar la configuración de caché en escenarios de programación en los que se trabaja con BasePartialCachingControl instancias que se han habilitado para caché mediante uno de los métodos anteriores y cargado dinámicamente mediante el [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) método.

Una instancia de ControlCachePolicy solo se puede manipular correctamente entre las etapas Init y PreRender del ciclo de vida del control. Si modifica un ControlCachePolicy objeto después de la PreRender fase, ASP.NET produce una excepción porque los cambios realizados después de representar el control no puede afectar realmente a la configuración de caché (un control se almacena en caché durante el Render fase). Por último, una instancia de control de usuario (y, por lo tanto, su objeto ControlCachePolicy) solo está disponible para la manipulación mediante programación cuando se representa realmente.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Cambios en la &lt;configuración&gt; de almacenamiento en caché: el elemento de almacenamiento en caché

Hay varios cambios en la configuración de almacenamiento en caché en ASP.NET 2.0. El &lt;elemento&gt; de almacenamiento en caché es nuevo en ASP.NET 2.0 y le permite realizar cambios de configuración de almacenamiento en caché en el archivo de configuración. Los siguientes atributos están disponibles.

| **Elemento** | **Descripción** |
| --- | --- |
| **Caché** | Elemento opcional. Define la configuración global de la caché de aplicaciones. |
| **Outputcache** | Elemento opcional. Especifica la configuración de caché de salida de toda la aplicación. |
| **outputCacheSettings** | Elemento opcional. Especifica la configuración de la caché de salida que se puede aplicar a las páginas de la aplicación. |
| **sqlCacheDependency** | Elemento opcional. Configura las dependencias de caché de SQL para una aplicación ASP.NET. |

### <a name="the-ltcachegt-element"></a>El &lt;&gt; elemento cache

Los siguientes atributos &lt;están&gt; disponibles en el elemento cache:

| **Atributo** | **Descripción** |
| --- | --- |
| **disableMemoryCollection** | Atributo **booleano** opcional. Obtiene o establece un valor que indica si la colección de memoria caché que se produce cuando el equipo está bajo presión de memoria está deshabilitada. |
| **disableExpiration** | Atributo **booleano** opcional. Obtiene o establece un valor que indica si la expiración de la memoria caché está deshabilitada. Cuando se deshabilita, los elementos almacenados en caché no caducan y no se produce la eliminación en segundo plano de los elementos de caché caducados. |
| **privateBytesLimit** | Atributo **Int64** opcional. Obtiene o establece un valor que indica el tamaño máximo de los bytes privados de una aplicación antes de que la memoria caché comience a vaciar los elementos caducados e intentar recuperar memoria. Este límite incluye tanto la memoria utilizada por la memoria caché como la sobrecarga de memoria normal de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo empezar a recuperar memoria. |
| **percentagePhysicalMemoryUsedLimit** | Atributo **Int32** opcional. Obtiene o establece un valor que indica el porcentaje máximo de memoria física de un equipo que puede consumir una aplicación antes de que la memoria caché comience a vaciar elementos caducados e intentar recuperar memoria Este uso de memoria incluye tanto la memoria utilizada por la memoria caché como el uso normal de memoria de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo empezar a recuperar memoria. |
| **privateBytesPollTime** | Atributo **TimeSpan** opcional. Obtiene o establece un valor que indica el intervalo de tiempo entre el sondeo para el uso de memoria de bytes privados de la aplicación. |

### <a name="the-ltoutputcachegt-element"></a>El &lt;elemento&gt; outputCache

Los siguientes atributos &lt;están&gt; disponibles para el elemento outputCache.

|       <strong>Atributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descripción</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Atributo <strong>booleano</strong> opcional. Habilita/deshabilita la caché de resultados de la página. Si está deshabilitada, no se almacena en caché ninguna página independientemente de la configuración mediante programación o declarativa. El valor predeterminado es <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Atributo <strong>booleano</strong> opcional. Habilita/deshabilita la caché de fragmentos de aplicación. Si está deshabilitada, no se almacena en caché ninguna página, independientemente de la directiva [OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) o el perfil de almacenamiento en caché utilizado. Incluye un encabezado de control de caché que indica que los servidores proxy ascendentes, así como los clientes del explorador, no deben intentar almacenar en caché la salida de la página. El valor predeterminado es <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Atributo <strong>booleano</strong> opcional. Obtiene o establece un valor que indica si el módulo de caché de salida envía el encabezado <strong>cache-control:private</strong> de forma predeterminada. El valor predeterminado es <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Atributo <strong>booleano</strong> opcional. Habilita/deshabilita el envío<strong>de \<un http " Vary: /strong><em>" encabezado en la respuesta. Con la configuración predeterminada de false,</em>se envía un encabezado " *Vary: \*" para las <strong>páginas almacenadas en caché de salida. Cuando se envía el encabezado Vary, permite que se almacenen en caché diferentes versiones en función de lo que se especifica en el encabezado Vary. Por ejemplo, <em>Vary:User-Agents</em> almacenará diferentes versiones de una página en función del agente de usuario que emite la solicitud. El valor predeterminado es **false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>El &lt;elemento&gt; outputCacheSettings

El &lt;elemento&gt; outputCacheSettings permite la creación de perfiles de caché como se ha descrito anteriormente. El único elemento &lt;secundario para&gt; el &lt;elemento outputCacheSettings es el elemento outputCacheProfiles&gt; para configurar perfiles de caché.

### <a name="the-ltsqlcachedependencygt-element"></a>El &lt;elemento&gt; sqlCacheDependency

Los siguientes atributos &lt;están disponibles para el elemento sqlCacheDependency.&gt;

| **Atributo** | **Descripción** |
| --- | --- |
| **Habilitado** | **Atributo booleano** requerido. Indica si se están sondeando o no los cambios. |
| **pollTime** | Atributo **Int32** opcional. Establece la frecuencia con la que SqlCacheDependency sondea la tabla de base de datos en busca de cambios. Este valor corresponde al número de milisegundos entre sondeos sucesivos. No se puede establecer en menos de 500 milisegundos. El valor predeterminado es 1 minuto. |

### <a name="more-information"></a>Más información

Hay información adicional que debe tener en cuenta con respecto a la configuración de caché.

- Si no se establece el límite de bytes privados del proceso de trabajo, la memoria caché utilizará uno de los siguientes límites: 

    - x86 2GB: 800MB o 60% de RAM física, lo que sea menos
    - x86 3GB: 1800MB o 60% de RAM física, lo que sea menos
    - x64: 1 terabyte o 60% de RAM física, lo que sea menos
- Si se establecen el &lt;límite de bytes&gt; privados del proceso de trabajo y la memoria caché privateBytesLimit/, la memoria caché usará el mínimo de los dos.
- Al igual que en 1.x, desbloqueadoremos las entradas de caché y llamamos a GC. Recoger por dos razones: 

    - Estamos muy cerca del límite de bytes privados
    - La memoria disponible es cercana o inferior al 10%
- Puede deshabilitar eficazmente el recorte y la &lt;memoria caché para&gt; condiciones de memoria disponibles bajas estableciendo el porcentaje de cachéPhysicalMemoryUseLimit/ en 100.
- A diferencia de 1.x, 2.0 suspenderá el recorte y recogerá las llamadas si el último GC. Collect no redujo los bytes privados ni el tamaño de los montones administrados en más del 1% del límite de memoria (caché).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dependencias de caché personalizadas

1. Crear un sitio web nuevo.
2. Agregue un nuevo archivo XML denominado cache.xml y guárdelo en la raíz de la aplicación web.
3. Agregue el código siguiente\_al método Page Load en el código subyacente de default.aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Agregue lo siguiente a la parte superior de default.aspx en la vista de origen: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Examine Default.aspx. ¿Qué dice el tiempo?
6. Actualice el explorador. ¿Qué dice el tiempo?
7. Abra cache.xml y agregue el siguiente código: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Guarde cache.xml.
9. Actualice el explorador. ¿Qué dice el tiempo?
10. Explicar por qué se actualizó el tiempo en lugar de mostrar los valores almacenados anteriormente en caché:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Lab 2: Uso de dependencias de caché basadas en sondeos

Este laboratorio utiliza el proyecto que creó en el módulo anterior que permite la edición de datos en la base de datos Northwind a través de un Control GridView y DetailsView.

1. Abra el proyecto en Visual Studio 2005.
2. Ejecute la\_utilidad regsql de aspnet en la base de datos Northwind para habilitar la base de datos y la tabla Products. Utilice el siguiente comando desde un símbolo del sistema de Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Agregue lo siguiente al archivo web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Agregue un nuevo formulario web denominado showdata.aspx.
5. Agregue la siguiente directiva de outputcache a la página showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Agregue el código siguiente\_a la carga de página de showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Agregue un nuevo control SqlDataSource a showdata.aspx y configúrelo para usar la conexión de base de datos Northwind. Haga clic en Siguiente.
8. Seleccione las casillas ProductName y ProductID y haga clic en Next (Siguiente).
9. Haga clic en Finish.
10. Agregue un nuevo control GridView a la página showdata.aspx.
11. Elija SqlDataSource1 en la lista desplegable.
12. Guarde y examine showdata.aspx. Anote la hora mostrada.
