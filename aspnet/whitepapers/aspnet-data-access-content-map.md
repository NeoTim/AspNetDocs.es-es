---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET acceso a los datos - Recursos recomendados ? Microsoft Docs
author: rick-anderson
description: En este tema se proporcionan vínculos a recursos de documentación sobre cómo obtener acceso a los datos en ASP.NET aplicaciones web, principalmente mediante Entity Framework y SQL Se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675788"
---
# <a name="aspnet-data-access---recommended-resources"></a>Acceso a los datos de ASP.NET - Recursos recomendados

> En este tema se proporcionan vínculos a recursos de documentación sobre cómo obtener acceso a los datos de ASP.NET aplicaciones web, principalmente mediante Entity Framework y SQL Server.
> 
> Si conoce una gran entrada de blog, un hilo [stackoverflow](http://stackoverflow.com) o cualquier otro vínculo que sea útil, [envíenos un correo electrónico](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con el enlace.
> 
> Última actualización 4/3/2014

El tema contiene las siguientes secciones:

- [Introducción al acceso a datos en ASP.NET](#gettingstarted)
- [Uso de Entity Framework](#ef)

    - [Uso primero de Entity Framework Code](#cf)
    - [Uso de migraciones de Entity Framework Code First](#efcfmigrations)
    - [Uso de Entity Framework Database First o Model First (el diseñador de EF)](#efdbf)
    - [Carga de datos relacionados en Entity Framework (carga diferida, carga diligente y carga explícita)](#efrelateddata)
    - [Optimización del rendimiento de Entity Framework](#optimizingef)
    - [Manejo de la simultaneidad en una aplicación de Entity Framework](#efconcurrency)
    - [Libros sobre Entity Framework](#efbooks)
    - [Recursos adicionales de Entity Framework](#otherefresources)
- [Enlace de datos en aplicaciones de formularios Web Forms ASP.NET](#wfdatabinding)

    - [Uso del enlace de modelos de formularios Web Forms](#wfmodelbinding)
    - [Uso de controles de origen de datos de formularios Web Forms](#wfdsc)
    - [Uso de controles enlazados a datos de formularios Web Forms y expresiones de enlace de datos](#wfdbc)
- [Trabajar con bases de datos de SQL Server](#sqlserver)

    - [Trabajar con bases de datos locales de SQL Server Express](#sslocaldb)
    - [Trabajar con bases de datos de SQL Server Express](#sse)
    - [Trabajar con Windows Azure SQL Database](#ssdb)
    - [Elegir entre SQL Server y Windows Azure SQL Database](#ssdbchoosing)
- [Trabajar con NoSQL Database Management Systems](#nosql)
- [Uso de consultas LINQ en aplicaciones ASP.NET](#linq)
- [Uso del andamiaje de datos dinámicos](#dd)
- [Protección del acceso a datos](#securing)
- [Optimización del rendimiento del acceso a los datos](#optimizingdataaccess)
- [Implementación de una base de datos](#deploying)
- [Acceso a datos a través de un servicio web](#webservice)
- [Recursos adicionales](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introducción al acceso a datos en ASP.NET

- Opciones de almacenamiento de datos (creación de aplicaciones en la nube del [mundo real con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) Capítulo de un libro electrónico sobre el desarrollo para la nube. Presenta las bases de datos NoSQL como una alternativa que muchos desarrolladores familiarizados con las bases de datos relacionales tienden a pasar por alto. Presenta directrices sobre qué pensar al elegir relacional o NoSQL, o al elegir una plataforma determinada.
- [ASP.NET Opciones de acceso a datos](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Una introducción a las opciones de acceso a datos para bases de datos relacionales para ASP.NET y instrucciones sobre cómo elegir plataformas y métodos de acceso adecuados para su escenario.
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si no ha trabajado con bases de datos relacionales, consulte esta página para obtener una introducción a la terminología y los conceptos de la base de datos relacional. Para obtener una introducción a SQL Server en particular, vea [Trabajar con bases](#sqlserver) de datos de SQL Server más adelante en este tema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Uso de Entity Framework

- [Enfoques](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) de desarrollo de Entity Framework (MSDN). Instrucciones sobre cómo elegir un enfoque de desarrollo de Entity Framework Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Uso primero de Entity Framework Code

Los siguientes tutoriales ofrecen aplicaciones de ejemplo descargables:

- [Introducción a EF 6 mediante MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Cubre una amplia gama de escenarios de Entity Framework Code First, incluidas las migraciones y las características de EF 6, como la resistencia de conexión, la interceptación de comandos y la asincrónica. Esta es una versión actualizada de la [serie EF 5 / MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie anterior incluye un tutorial sobre el repositorio y patrones de unidad de trabajo que no se incluye en la nueva serie.
- [Introducción a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Cubre una gama más estrecha de escenarios de Entity Framework Code First, pero realiza un trabajo más completo de introducción de características MVC.
- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Utiliza Code First en una aplicación de formularios Web Forms.
- [Introducción a ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Una introducción a los formularios Web Forms con cierta cobertura de Code First. Utiliza el enlace de modelos.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utiliza Code First en una aplicación MVC 3 de comercio electrónico que también implementa la pertenencia y la autorización. La versión MVC y el sistema de pertenencia ASP.NET (autenticación y autorización) utilizados aquí están obsoletos; Para obtener información más actualizada sobre ASP.NET [https://asp.net/identity](https://asp.net/identity)membresía, véase .

Otros recursos:

- [Entity Framework: código primero](https://msdn.microsoft.com/data/jj200620)en una base de datos existente. Msdn. Vídeo y tutorial que muestra cómo usar Code First con una base de datos existente.
- [Centro de desarrollo](https://msdn.microsoft.com/data/ef)de datos - Entity Framework . Msdn. Para obtener una guía de la documentación de Entity Framework que ha creado y mantenido el equipo de Entity Framework, vea el vínculo [Introducción.](https://msdn.microsoft.com/data/ee712907)

Consulte también [Libros sobre Entity Framework](#efbooks) y recursos adicionales de Entity [Framework](#otherefresources) más adelante en este tema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Uso de migraciones de Entity Framework Code First

La mayoría de los tutoriales de Code First enumerados anteriormente cubren las migraciones. Consulte también los siguientes recursos.

- [Implementación web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Serie de tutoriales de 2 partes que muestra cómo usar migraciones de Code First para implementar una base de datos.
- [Implemente una aplicación Secure ASP.NET MVC 5 con Pertenencia, OAuth y SQL Database en un sitio web de Windows Azure.](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) Microsoft Azure). Cómo usar las migraciones para implementar datos de pertenencia y de aplicación en Azure.
- [Información general sobre la implementación web para Visual Studio y ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consulte la sección Configuración de la **implementación** de base de datos en Visual Studio para obtener una explicación de cómo se integran las migraciones de Code First en las características de implementación web de Visual Studio.
- Centro de desarrollo de [datos: migraciones](https://msdn.microsoft.com/data/jj591621) de Code First (MSDN). Documentación de migraciones del equipo de Entity Framework.
- [Migraciones Serie Screencast](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF). Tres vídeos sobre temas avanzados en Migraciones de Code First.
- [Migraciones de Code First Con ASP.NET sitios de páginas web](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting). Muestra cómo usar migraciones de Code First con un sitio de páginas Web de ASP.NET colocando el contexto de datos en un proyecto de biblioteca de clases de Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Uso de Entity Framework Database First o Model First (el diseñador de EF)

- [Introducción a Entity Framework 6 Base](../mvc/overview/getting-started/database-first-development/setting-up-database.md)de datos primero mediante MVC 5 . Ejecute un script en el Explorador de servidores para crear una base de datos y, a continuación, use el diseñador de Entity Framework para crear el modelo de datos. Muestra cómo crear páginas web CRUD simples y, para otras funciones de control de datos, puede seguir uno de los tutoriales de Code First, ya que todos los flujos de trabajo de EF usan la misma API de DbContext.

Los siguientes recursos son más antiguos. Son útiles si desea usar la versión 4.0 de Entity Framework y desea usar un control de origen de datos para el enlace de datos en una aplicación de formularios Web Forms.

- [Introducción a Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Muestra cómo utilizar el **EntityDataSource** control.
- [Continuar con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(muestra cómo usar el **ObjectDataSource** Control. Incluye un tutorial sobre el control de simultaneidad, un tutorial sobre el rendimiento de EF y un tutorial sobre las novedades de EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Manejo de datos relacionados en Entity Framework (carga diferida, carga diligente y carga explícita)

- [Lectura de datos relacionados con Entity Framework en una aplicación MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, aplicación de ejemplo MVC. Los métodos que se muestran se aplican también al enlace de modelos de formularios Web Forms y al flujo de trabajo de Database First.
- Centro de desarrollo de datos: carga de [entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentación del equipo de Entity Framework sobre la carga de datos relacionados.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimización del rendimiento de Entity Framework

- [Escenarios avanzados](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)de Entity Framework para una aplicación de ASP.NET . Muestra cómo ejecutar sus propias instrucciones SQL o llamar a sus propios procedimientos almacenados, cómo deshabilitar la detección de cambios y cómo deshabilitar la validación al guardar los cambios.
- [Consideraciones](https://msdn.microsoft.com/data/hh949853) de rendimiento para Entity Framework 5 (MSDN).
- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar el rendimiento con Entity Framework en una aplicación web ASP.NET.](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md) Se aplica a Entity Framework 4.0.
- Consulte también [Optimización del acceso a datos ASP.NET](#optimizingdataaccess) más adelante en este tema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Manejo de la simultaneidad en una aplicación de Entity Framework

- [Controlar la simultaneidad con Entity Framework en una aplicación MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, API de DbContext, mediante una aplicación de ejemplo MVC.
- Centro de desarrollo de datos: patrones de [simultaneidad optimista (MSDN).](https://msdn.microsoft.com/data/jj592904) Documentación de simultaneidad del equipo de Entity Framework.
- [Controlar la simultaneidad con Entity Framework en una aplicación web de ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4.0. Database First, ObjectContext API, mediante una aplicación de ejemplo de formularios Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libros sobre Entity Framework

- [Programming Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) de Julie Lerman y Rowan Miller.
- [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) de Julie Lerman y Rowan Miller.

Ambos libros están actualizados con las técnicas recomendadas actuales. Proporcionan una introducción más completa pero fácil de seguir a Entity Framework que cualquier cosa disponible en Internet. Otro libro, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) de Julie Lerman, es más grande y más completo, pero es más antiguo y muchas de las técnicas que cubre ya no son la forma recomendada de usar Entity Framework. Consulte también la lista de libros recomendados por el equipo de Entity Framework en [Data Developer Center - Libros](https://msdn.microsoft.com/data/aa937716) en el sitio DE MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Otros recursos de Entity Framework

- Blog del equipo de [Entity Framework (ADO.NET).](https://blogs.msdn.com/b/adonet/) Uno de los mejores recursos para la información más actualizada y los anuncios de nuevas mejoras. Para otros blogs relacionados con EF, vea Blogroll en [Introducción a Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consulte la columna **Puntos** de datos, que suele tratartemas de temas relacionados con Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Enlace de datos en aplicaciones de formularios Web Forms ASP.NET

- [ASP.NET Opciones](https://msdn.microsoft.com/library/jj822927.aspx) de acceso a<a id="wfmodelbinding"></a>datos de formularios Web Forms (MSDN).

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Uso del enlace de modelos de formularios Web Forms

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales con EF Code First.
- Web [Forms Modelo Enlace Parte 1: Selección de datos](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). En estas entradas de blog anteriores, la propiedad que actualmente se denomina ItemType se denomina ModelType, pero de lo contrario la información que contienen es válida.
- [Web Forms Modelo Deenlace Parte 2: Filtrado de datos](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- Web [Forms Model Binding Part 3: Actualización y validación](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [ASP.NET 4.5 Enlace de modelos Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Elemento de enlace](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) de modelo 1 - Selección de datos (vídeo).
- [Encuadernación de modelos Parte 2 - Filtrado](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introducción a ASP.NET formularios Web Forms 4.5 - Mostrar elementos](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)de datos y detalles .

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Uso de controles de origen de datos de formularios Web Forms

- [Controles](https://msdn.microsoft.com/library/ms247258.aspx) de servidor Web de origen de datos (MSDN).
- [Anunciando la versión del proveedor de datos dinámicos y el control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desarrollo web de Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Uso de controles enlazados a datos de formularios Web Forms y expresiones de enlace de datos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales que usa EF Code First.
- [Introducción a ASP.NET formularios Web Forms 4.5 - Mostrar elementos](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)de datos y detalles .
- Controles de [datos fuertemente tipados](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Controles de datos fuertemente tipados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4.5 Controles](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) de datos con tipo seguro de formularios Web Forms (vídeo).
- [Controles de servidor Web enlazados a](https://msdn.microsoft.com/library/ms228214.aspx) datos (MSDN).
- [Información general sobre expresiones de enlace de](https://msdn.microsoft.com/library/ms178366.aspx) datos (MSDN). Esta página solo cubre **Eval** y **Bind;** no se ha actualizado para incluir **Item** y **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabajar con bases de datos de SQL Server

- [Características](https://msdn.microsoft.com/library/hh230827.aspx) de base de datos de SQL Server (MSDN). Para obtener una introducción general a una amplia variedad de temas de SQL Server, vea las entradas de esta en el TD.T.T.
- [SQL Server Editions](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un resumen de las ediciones de SQL Server disponibles, con vínculos a más información sobre cada una.)
- [Cadenas de conexión de SQL Server para ASP.NET aplicaciones web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Uso de SQL Server Compact para aplicaciones web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Ejemplos de productos](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)de base de datos . Ejemplo de bases de datos AdventureWorks.
- [Instalación de bases de datos](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)de ejemplo . Además de los métodos que se muestran aquí, también\_puede descargar uno de los archivos .mdf de ejemplo a la carpeta App Data de un proyecto web, convertir la base de datos a LocalDB y crear una cadena de conexión LocalDB. Para obtener información sobre cómo hacerlo, consulte [Cómo: Actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consulte también las secciones siguientes sobre cómo trabajar con SQL Server Express y LocalDB y elegir entre SQL Server y SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabajar con bases de datos locales de SQL Server Express

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). La introducción oficial de MSDN a LocalDB.
- [Cadenas de conexión de SQL Server para ASP.NET aplicaciones web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Cómo: Actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Cómo migrar un archivo .mdf de una versión anterior de SQL Server Express a LocalDB. También tiene que pasar por este proceso si descarga una de las bases de datos de ejemplo de [SQL Server 2012.](https://go.microsoft.com/fwlink/?linkid=117483)
- [Presentamos LocalDB, un SQL Express mejorado](https://go.microsoft.com/fwlink/?LinkId=234375) (blog de SQL Server Express). Tiene más antecedentes sobre por qué se creó LocalDB que se incluye en MSDN.
- [LocalDB: ¿Dónde está mi base de datos?](https://go.microsoft.com/fwlink/?LinkId=234376) (blog de SQL Server Express). Información sobre dónde se crean los archivos de base de datos LocalDB.
- [Uso de LocalDB con IIS completo, Parte 1: Perfil](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) de usuario (blog de SQL Server Express). LocalDB no está diseñado para trabajar con IIS. Esta serie de entradas de blog explica los problemas y algunas soluciones.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabajar con bases de datos de SQL Server Express

- [Cadenas de conexión de SQL Server para ASP.NET aplicaciones web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si usa la configuración de cadena de conexión AttachDBFileName con SQL Server Express, consulte especialmente la sección Instancia de usuario de esta página.
- [Cómo tomar posesión de su SQL Server Express 2008 local](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog de SQL Server Express). Un problema común es no poder trabajar con bases de datos de SQL Server Express porque no es administrador en la instancia de SQL Server Express. De forma predeterminada, solo la persona que instaló SQL Server Express es un administrador. En este blog se explica cómo convertirse en administrador de SQL Server Express si es administrador en el equipo.
- [¿Puede mi aplicación web ASP.NET usar una base de datos de SQL Server Express en producción?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabajar con Windows Azure SQL Database

- [Implemente una aplicación Secure ASP.NET MVC con Membership, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sitio de Microsoft Azure).
- [Bases de datos SQL](https://docs.microsoft.com/azure/sql-database/) (sitio de Microsoft Azure). Tutoriales de introducción y guías de instrucciones.
- [Base](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) de datos SQL de Windows Azure (MSDN). El nodo de nivel superior de la tabla de contenido de SQL Database en MSDN.
- [Windows Azure SQL Database TechNet Wiki Articles Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sitio de Microsoft TechNet).
- Bloque de aplicación de control de [errores transitorios](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un marco de trabajo que le permite controlar errores de red transitorios y errores de conexión que resultan de la limitación. Disponible en un paquete NuGet: [Biblioteca empresarial 5.0 - Bloque](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling)de aplicación de control transitorio de errores .
- [Introducción a SQL Database y Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de formación de Windows Azure](https://www.microsoft.com/download/details.aspx?id=8396) (Centro de descarga de Microsoft). Incluye laboratorios prácticos para SQL Database.
- [Foro de la comunidad](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads)de Windows Azure SQL Database .
- [Mover a Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capítulo de un escenario integral de extremo a extremo por parte del equipo de microsoft Patterns and Practices. Explica por qué es posible que desee migrar y cómo migrar de SQL Server a SQL Database.
- [Migración de bases](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) de datos de SQL Server a Windows Azure SQL Database (MSDN).
- [Asistente para migración de SQL Database](http://sqlazuremw.codeplex.com/). Una herramienta de código abierto para migrar bases de datos a y desde SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Elegir entre SQL Server y Windows Azure SQL Database

- [Compare SQL Server con Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sitio de Microsoft TechNet).
- [Migración de datos a Windows Azure SQL Database: herramientas y técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Incluye secciones que comparan SQL Server con SQL Database y proporcionan instrucciones sobre cuándo migrar de SQL Server a SQL Database.
- [Guía de entrega de Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sitio de Microsoft TechNet).
- Limitaciones de características de [SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- Almacenamiento de tablas de [Windows Azure y Windows Azure SQL Database: comparado y contrastado](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Para una aplicación que implementa en Windows Azure, El almacenamiento de tablas de Windows Azure podría ser una alternativa a Windows Azure SQL Database. Este tema le ayuda a decidir entre estas alternativas.
- [Base](https://msdn.microsoft.com/library/windowsazure/ee336279) de datos SQL de Windows Azure (MSDN).
- [Instrucciones y limitaciones (SQL Database de Microsoft Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabajar con NoSQL Database Management Systems

- [Servicios de datos](https://www.windowsazure.com/develop/net/data/) de Windows Azure (sitio de Microsoft Azure). Consulte la guía de características de [Table Service](https://docs.microsoft.com/azure/) y la sección Big **Data** de la página.
- ASP.NET aplicación de varios [niveles mediante tablas de almacenamiento, colas y blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio de Microsoft Azure). Tutorial de extremo a extremo con aplicación de ejemplo descargable que usa tablas NoSQL de almacenamiento de Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Uso de consultas LINQ en aplicaciones ASP.NET

- [ASP.NET Opciones de acceso a datos](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Incluye una introducción a LINQ.
- [LINQ Training Videos](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [ASP.NET subproceso Foro con vínculos a recursos LINQ dinámicos.](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Uso del andamiaje dinámico de datos

- Plantillas de proyecto de [datos dinámicos](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Orientación sobre cuándo usar proyectos de datos dinámicos.
- [ASP.NET datos dinámicos](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protección del acceso a datos

- [Proteger el acceso a datos en ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Consideraciones de seguridad (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Cómo: Proteger cadenas](https://msdn.microsoft.com/library/ms178372.aspx) de conexión al usar controles de origen de datos (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimización del rendimiento del acceso a los datos

- [Descripción general ASP.NET rendimiento](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET almacenamiento en caché](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Mejorar el rendimiento ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Hay una advertencia de "Contenido retirado" en la parte superior de esta página, pero la mayor parte de la información sigue siendo relevante y no hay ningún recurso actualizado comparable.
- [Mejorar el rendimiento de SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). El mismo comentario que el enlace anterior.

Consulte también [Optimización](#optimizingef) del rendimiento de Entity Framework anteriormente en este tema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implementación de una base de datos

- [ASP.NET Web Deployment - Recursos recomendados](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acceso a datos a través de un servicio web

- [Acceso a datos a través](https://msdn.microsoft.com/library/ms178359.aspx#webservice) de un servicio web (MSDN). Instrucciones sobre cuándo usar la API web frente a WCF.
- [Introducción a ASP.NET API web.](../web-api/index.md)
- [Servicios de datos de WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.NET preguntas frecuentes sobre el acceso a datos](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Tutoriales de formularios Web Forms - Datos](../web-forms/overview/data-access/index.md). La mayoría de estos tutoriales son relativamente antiguos; Asegúrese de leer ASP.NET Opciones de [acceso a datos](https://msdn.microsoft.com/library/ms178359.aspx) y Opciones de almacenamiento de datos (Creación de aplicaciones en la nube del mundo real con Windows [Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) en primer lugar para que no llegue demasiado lejos en un método de acceso a datos que no sea adecuado para su escenario.
- [ASP.NET mapa de contenido MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Tutoriales de páginas Web - Datos](../web-pages/overview/data/index.md).
- [Acceso a datos en Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Proporciona una lista de vínculos similares a este mapa de contenido, pero con un foco en Visual Studio en lugar de ASP.NET.
