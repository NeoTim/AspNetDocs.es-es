---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Crear el esquema de pertenencia en SQL Server (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial se inicia examinando técnicas para agregar el esquema necesario a la base de datos con el fin de usar el SqlMembershipProvider. A continuación, se proporciona la siguiente...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 8160c422d7a20b7c6954f960e73d5d59f7b90a5f
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044745"
---
# <a name="creating-the-membership-schema-in-sql-server-c"></a>Crear el esquema de pertenencia en SQL Server (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Este tutorial se inicia examinando técnicas para agregar el esquema necesario a la base de datos con el fin de usar el SqlMembershipProvider. A continuación, examinaremos las tablas clave del esquema y analizaremos su finalidad y su importancia. Este tutorial finaliza con un vistazo a cómo indicar a una aplicación de ASP.NET qué proveedor debe usar el marco de pertenencia.

## <a name="introduction"></a>Introducción

Los dos tutoriales anteriores examinaban el uso de la autenticación de formularios para identificar a los visitantes del sitio Web. El marco de autenticación de formularios facilita a los desarrolladores la sesión de un usuario en un sitio web y recordarlos a través del uso de vales de autenticación. La `FormsAuthentication` clase incluye métodos para generar el vale y agregarlo a las cookies del visitante. `FormsAuthenticationModule`Examina todas las solicitudes entrantes y, para aquellas con un vale de autenticación válido, crea y asocia un `GenericPrincipal` objeto y un `FormsIdentity` objeto con la solicitud actual. La autenticación de formularios es simplemente un mecanismo para conceder un vale de autenticación a un visitante cuando inicia sesión y, en solicitudes posteriores, analiza el vale para determinar la identidad del usuario. Para que una aplicación web admita cuentas de usuario, todavía es necesario implementar un almacén de usuario y agregar funcionalidad para validar las credenciales, registrar nuevos usuarios y la infinidad de otras tareas relacionadas con la cuenta de usuario.

Antes de ASP.NET 2,0, los desarrolladores estaban en el enlace para implementar todas estas tareas relacionadas con la cuenta de usuario. Afortunadamente, el equipo de ASP.NET reconoció este inconveniente e incorporó el marco de pertenencia con ASP.NET 2,0. El marco de pertenencia es un conjunto de clases de la .NET Framework que proporcionan una interfaz de programación para realizar las tareas principales relacionadas con las cuentas de usuario. Este marco de trabajo se basa en el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite a los desarrolladores conectar una implementación personalizada a una API normalizada.

Como se describe en el tutorial de compatibilidad con los <a id="Tutorial1"></a> [*conceptos básicos de seguridad y ASP.net*](../introduction/security-basics-and-asp-net-support-cs.md) , el .NET Framework se suministra con dos proveedores de pertenencia integrados: [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) y [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) . Como su nombre implica, `SqlMembershipProvider` utiliza una base de datos Microsoft SQL Server como almacén del usuario. Para usar este proveedor en una aplicación, es necesario indicar al proveedor qué base de datos se va a usar como almacén. Como puede imaginar, espera que `SqlMembershipProvider` la base de datos del almacén de usuarios tenga determinadas tablas, vistas y procedimientos almacenados de base de datos. Necesitamos agregar este esquema esperado a la base de datos seleccionada.

Este tutorial se inicia examinando técnicas para agregar el esquema necesario a la base de datos con el fin de usar `SqlMembershipProvider` . A continuación, examinaremos las tablas clave del esquema y analizaremos su finalidad y su importancia. Este tutorial finaliza con un vistazo a cómo indicar a una aplicación de ASP.NET qué proveedor debe usar el marco de pertenencia.

Comencemos.

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Paso 1: decidir dónde colocar el almacén del usuario

Normalmente, los datos de una aplicación ASP.NET se almacenan en varias tablas de una base de datos. Al implementar el esquema de la base de datos `SqlMembershipProvider` , se debe decidir si se va a colocar el esquema de pertenencia en la misma base de datos que los datos de la aplicación o en una base de datos alternativa.

Recomiendo buscar el esquema de pertenencia en la misma base de datos que los datos de la aplicación por los siguientes motivos:

- **Mantenimiento** ' una aplicación cuyos datos se encapsulan en una base de datos es más fácil de comprender, mantener e implementar que una aplicación que tiene dos bases de datos independientes.
- **Integridad relacional** ' al buscar las tablas relacionadas con la pertenencia en la misma base de datos que las tablas de aplicación, es posible establecer [restricciones Foreign Key](http://en.wikipedia.org/wiki/Foreign_key) entre las claves principales de las tablas relacionadas con la pertenencia y las tablas de aplicaciones relacionadas.

Desacoplar los datos del almacén de usuarios y de la aplicación en bases de datos independientes solo tiene sentido si tiene varias aplicaciones que utilizan bases de datos independientes, pero necesitan compartir un almacén de usuario común.

### <a name="creating-a-database"></a>Crear una base de datos

La aplicación que se ha creado desde el segundo tutorial todavía no necesita una base de datos. No obstante, necesitamos uno ahora para el almacén de usuario. Vamos a crear una y agregarle el esquema requerido por el `SqlMembershipProvider` proveedor (consulte el paso 2).

> [!NOTE]
> A lo largo de esta serie de tutoriales, usaremos una base de datos de [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) para almacenar las tablas de la aplicación y el `SqlMembershipProvider` esquema. Esta decisión se tomó por dos motivos: en primer lugar, debido a su coste gratuito: la edición Express es la versión más accesible de SQL Server 2005; en segundo lugar, SQL Server 2005 Express Edition bases de datos se pueden colocar directamente en la carpeta de la aplicación Web `App_Data` , lo que lo convierte en un cinch para empaquetar la base de datos y la aplicación web en un archivo zip y volver a implementarla sin ninguna instrucción de configuración especial ni opciones de configuración. Si prefiere seguir el uso de una versión de SQL Server no Express Edition, no dude en hacerlo. Los pasos son prácticamente idénticos. El `SqlMembershipProvider` esquema funcionará con cualquier versión de Microsoft SQL Server 2000 y versiones.

En el Explorador de soluciones, haga clic con el botón derecho en la `App_Data` carpeta y elija Agregar nuevo elemento. (Si no ve una `App_Data` carpeta en el proyecto, haga clic con el botón derecho en el proyecto en explorador de soluciones, seleccione Agregar carpeta ASP.net y elija `App_Data` ). En el cuadro de diálogo Agregar nuevo elemento, elija Agregar un nuevo SQL Database denominado `SecurityTutorials.mdf` . En este tutorial se agregará el `SqlMembershipProvider` esquema a esta base de datos; en los tutoriales siguientes, se crearán tablas adicionales para capturar los datos de la aplicación.

[![Agregue una nueva SQL Database denominada SecurityTutorials. MDF a la carpeta App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: agregar una nueva SQL Database base de datos con nombre `SecurityTutorials.mdf` a la `App_Data` carpeta ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))

Al agregar una base de datos a la `App_Data` carpeta, se incluye automáticamente en la vista Explorador de bases de datos. (En la versión de Visual Studio no Express Edition, el Explorador de bases de datos se denomina Explorador de servidores). Vaya a la Explorador de bases de datos y expanda la base de datos recién agregada `SecurityTutorials` . Si no ve el Explorador de bases de datos en la pantalla, vaya al menú Ver y elija Explorador de bases de datos, o presione Ctrl + Alt + S. Como se muestra en la figura 2, la `SecurityTutorials` base de datos está vacía, no contiene ninguna tabla, ninguna vista ni ningún procedimiento almacenado.

[![La base de datos SecurityTutorials está vacía actualmente](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: la `SecurityTutorials` base de datos está vacía actualmente ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Paso 2: agregar el `SqlMembershipProvider` esquema a la base de datos

`SqlMembershipProvider`Requiere la instalación de un conjunto determinado de tablas, vistas y procedimientos almacenados en la base de datos del almacén de usuarios. Estos objetos de base de datos necesarios se pueden agregar mediante la [ `aspnet_regsql.exe` herramienta](https://msdn.microsoft.com/library/ms229862.aspx). Este archivo se encuentra en la `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpeta.

> [!NOTE]
> La `aspnet_regsql.exe` herramienta ofrece la funcionalidad de línea de comandos y una interfaz de usuario gráfica. La interfaz gráfica es más fácil de usar y es lo que examinaremos en este tutorial. La interfaz de la línea de comandos es útil cuando la adición del `SqlMembershipProvider` esquema debe automatizarse, como en los scripts de compilación o en escenarios de pruebas automatizadas.

La `aspnet_regsql.exe` herramienta se usa para agregar o quitar *servicios de aplicación de ASP.net* en una base de datos de SQL Server especificada. Los servicios de aplicación de ASP.NET abarcan los esquemas para `SqlMembershipProvider` y `SqlRoleProvider` , junto con los esquemas de los proveedores basados en SQL para otros marcos de ASP.net 2,0. Necesitamos proporcionar dos bits de información a la `aspnet_regsql.exe` herramienta:

- Si desea agregar o quitar servicios de aplicación, y
- Base de datos de la que se va a agregar o quitar el esquema de servicios de aplicación.

En el caso de la base de datos que se va a usar, la `aspnet_regsql.exe` herramienta nos pide que proporcione el nombre del servidor en el que reside la base de datos, las credenciales de seguridad para conectarse a la base de datos y el nombre de la base de datos. Si usa la edición no Express de SQL Server, ya debe conocer esta información, ya que se trata de la misma información que debe proporcionar a través de una cadena de conexión al trabajar con la base de datos a través de una página web de ASP.NET. La determinación del nombre del servidor y de la base de datos al usar una base de datos de SQL Server 2005 Express Edition en la `App_Data` carpeta, sin embargo, es un poco más complicada.

En la siguiente sección se examina una forma sencilla de especificar el nombre del servidor y de la base de datos de una base de datos SQL Server 2005 Express Edition en la `App_Data` carpeta. Si no usa SQL Server 2005 Express Edition no dude en ir directamente a la sección Instalación de la Servicios de aplicación.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Determinar el nombre del servidor y de la base de datos de una base de datos SQL Server 2005 Express Edition en la `App_Data` carpeta

Para usar la `aspnet_regsql.exe` herramienta, es necesario conocer los nombres de servidor y base de datos. El nombre del servidor es `localhost\InstanceName` . Lo más probable es que *InstanceName* sea `SQLExpress` . Sin embargo, si ha instalado SQL Server 2005 Express Edition manualmente (es decir, si no lo instaló automáticamente al instalar Visual Studio), es posible que haya seleccionado un nombre de instancia diferente.

El nombre de la base de datos es un poco más complicado de determinar. Las bases de datos de la `App_Data` carpeta normalmente tienen un nombre de base de datos que incluye un [identificador único global](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) junto con la ruta de acceso al archivo de base de datos. Es necesario determinar este nombre de base de datos para agregar el esquema de servicios de aplicación mediante `aspnet_regsql.exe` .

La forma más fácil de determinar el nombre de la base de datos es examinarlo a través de SQL Server Management Studio. SQL Server Management Studio proporciona una interfaz gráfica para la administración de bases de datos de SQL Server 2005, pero no se incluye con la edición Express de SQL Server 2005. La buena noticia es que [puede descargar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) la edición gratuita Express de SQL Server Management Studio.

> [!NOTE]
> Si también tiene una versión de edición no Express de SQL Server 2005 instalada en el escritorio, es probable que se instale la versión completa de Management Studio. Puede usar la versión completa para determinar el nombre de la base de datos, siguiendo los mismos pasos que se describen a continuación para la edición Express.

Empiece por cerrar Visual Studio para asegurarse de que se cierran los bloqueos impuestos por Visual Studio en el archivo de base de datos. A continuación, inicie SQL Server Management Studio y conéctese a la `localhost\InstanceName` base de datos de SQL Server 2005 Express Edition. Como se indicó anteriormente, lo más probable es que el nombre de la instancia sea `SQLExpress` . En la opción de autenticación, seleccione autenticación de Windows.

[![Conexión a la instancia de SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: conexión a la instancia de SQL Server 2005 Express Edition ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))

Después de conectarse a la instancia de SQL Server 2005 Express Edition, Management Studio muestra las carpetas de las bases de datos, la configuración de seguridad, los objetos de servidor, etc. Si expande la pestaña bases de datos, verá que la `SecurityTutorials.mdf` base de datos *no* está registrada en la instancia de base de datos (es necesario adjuntar primero la base de datos).

Haga clic con el botón derecho en la carpeta bases de datos y elija adjuntar en el menú contextual. Se mostrará el cuadro de diálogo Adjuntar bases de datos. Desde aquí, haga clic en el botón Agregar, vaya a la `SecurityTutorials.mdf` base de datos y haga clic en Aceptar. La figura 4 muestra el cuadro de diálogo Adjuntar bases de datos después de que se haya `SecurityTutorials.mdf` seleccionado la base de datos. En la figura 5 se muestra el Explorador de objetos de Management Studio después de que la base de datos se haya adjuntado correctamente.

[![Adjunte la base de datos SecurityTutorials. MDF](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: Adjunte la base de datos `SecurityTutorials.mdf` ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))

[![La base de datos SecurityTutorials. MDF aparece en la carpeta bases de datos](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: la `SecurityTutorials.mdf` base de datos aparece en la carpeta bases de datos ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))

Como se muestra en la figura 5, la `SecurityTutorials.mdf` base de datos tiene un nombre en lugar de abstruse. Vamos a cambiarlo a un nombre más memorable y más fácil de escribir. Haga clic con el botón derecho en la base de datos, elija cambiar nombre en el menú contextual y cambie su nombre `SecurityTutorialsDatabase` . Esto no cambia el nombre de archivo, solo el nombre que usa la base de datos para identificarse como SQL Server.

[![Cambiar el nombre de la base de datos a SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: cambiar el nombre de la base de datos a `SecurityTutorialsDatabase` ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))

Llegados a este punto, conocemos los nombres de servidor y base de datos para el `SecurityTutorials.mdf` archivo de base de datos: `localhost\InstanceName` y `SecurityTutorialsDatabase` , respectivamente. Ahora estamos preparados para instalar los servicios de aplicación a través de la `aspnet_regsql.exe` herramienta.

### <a name="installing-the-application-services"></a>Instalación del Servicios de aplicación

Para iniciar la `aspnet_regsql.exe` herramienta, vaya al menú Inicio y elija Ejecutar. Escriba `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` en el cuadro de texto y haga clic en Aceptar. Como alternativa, puede usar el explorador de Windows para explorar en profundidad hasta la carpeta adecuada y hacer doble clic en el `aspnet_regsql.exe` archivo. Cualquiera de los enfoques tendrá el mismo resultado.

La ejecución de la `aspnet_regsql.exe` herramienta sin argumentos de línea de comandos inicia la interfaz gráfica de usuario del Asistente para la instalación de ASP.NET SQL Server. El asistente facilita la adición o la eliminación de los servicios de aplicación de ASP.NET en una base de datos especificada. La primera pantalla del asistente, que se muestra en la figura 7, describe el propósito de la herramienta.

[![Usar el Asistente para la instalación de ASP.NET SQL Server hace que agregue el esquema de pertenencia](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: uso del Asistente para la instalación de ASP.net SQL Server hace que agregue el esquema de pertenencia ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))

El segundo paso del asistente nos pregunta si queremos agregar los servicios de la aplicación o quitarlos. Como queremos agregar las tablas, las vistas y los procedimientos almacenados necesarios para el `SqlMembershipProvider` , elija la opción configurar SQL Server para servicios de aplicación. Más adelante, si desea quitar este esquema de la base de datos, vuelva a ejecutar este asistente, pero elija la opción quitar información de servicios de aplicación de una base de datos existente.

[![Elija la opción configurar SQL Server para Servicios de aplicación](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: elija la opción configurar SQL Server para servicios de aplicación ([haga clic para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))

En el tercer paso se solicita la información de la base de datos: el nombre del servidor, la información de autenticación y el nombre de la base de datos. Si ha estado siguiendo este tutorial y ha agregado la base de datos a, la ha `SecurityTutorials.mdf` `App_Data` adjuntado a `localhost\InstanceName` y le ha cambiado el nombre a `SecurityTutorialsDatabase` , use los valores siguientes:

- Servidor: `localhost\InstanceName`
- Autenticación de Windows.
- Base `SecurityTutorialsDatabase`

[![Escriba la información de la base de datos](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: escriba la información de la base de datos ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))

Después de escribir la información de la base de datos, haga clic en siguiente. En el paso final se resumen los pasos que se van a realizar. Haga clic en siguiente para instalar los servicios de aplicación y, a continuación, en finalizar para completar el asistente.

> [!NOTE]
> Si usó Management Studio para adjuntar la base de datos y cambiar el nombre del archivo de base de datos, asegúrese de desasociar la base de datos y cerrar Management Studio antes de volver a abrir Visual Studio. Para desasociar la `SecurityTutorialsDatabase` base de datos, haga clic con el botón derecho en el nombre de la base de datos y, en el menú tareas, elija Desasociar.

Una vez finalizado el asistente, vuelva a Visual Studio y navegue hasta el Explorador de bases de datos. Expanda la carpeta Tablas. Debería ver una serie de tablas cuyos nombres comienzan por el prefijo `aspnet_` . Del mismo modo, se pueden encontrar diversas vistas y procedimientos almacenados en las carpetas vistas y procedimientos almacenados. Estos objetos de base de datos componen el esquema de servicios de aplicación. Examinaremos los objetos de base de datos específicos de la pertenencia y el rol en el paso 3.

[![Se han agregado varias tablas, vistas y procedimientos almacenados a la base de datos](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: se han agregado varias tablas, vistas y procedimientos almacenados a la base de datos ([haga clic para ver la imagen de tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))

> [!NOTE]
> La `aspnet_regsql.exe` interfaz gráfica de usuario de la herramienta instala todo el esquema de servicios de aplicación. Sin embargo, cuando `aspnet_regsql.exe` se ejecuta desde la línea de comandos, puede especificar qué componentes de servicios de aplicación concretos se deben instalar (o quitar). Por lo tanto, si desea agregar solo las tablas, vistas y procedimientos almacenados necesarios para los `SqlMembershipProvider` proveedores de y `SqlRoleProvider` , ejecute `aspnet_regsql.exe` desde la línea de comandos. Como alternativa, puede ejecutar manualmente el subconjunto adecuado de comandos CREATE de T-SQL que usa `aspnet_regsql.exe` . Estos scripts se encuentran en la `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpeta con nombres como `InstallCommon.sql` , `InstallMembership.sql` , `InstallRoles.sql` , `InstallProfile.sql` ,, etc `InstallSqlState.sql` .

En este momento, hemos creado los objetos de base de datos que necesita `SqlMembershipProvider` . Sin embargo, todavía es necesario indicar al marco de pertenencia que debe usar (en comparación con, por `SqlMembershipProvider` ejemplo, `ActiveDirectoryMembershipProvider` ) y que `SqlMembershipProvider` debe usar la `SecurityTutorials` base de datos. Veremos cómo especificar qué proveedor usar y cómo personalizar la configuración del proveedor seleccionado en el paso 4. Pero en primer lugar, echemos un vistazo más profundo a los objetos de base de datos que se acaban de crear.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Paso 3: eche un vistazo a las tablas principales del esquema

Al trabajar con los marcos de pertenencia y roles en una aplicación ASP.NET, el proveedor encapsula los detalles de la implementación. En futuros tutoriales, se interscribirán con estos marcos de trabajo a través de las `Membership` clases y del .NET Framework `Roles` . Al usar estas API de alto nivel, no es necesario preocuparnos de los detalles de bajo nivel, como qué consultas se ejecutan o qué tablas modifica `SqlMembershipProvider` y `SqlRoleProvider` .

Dado esto, podríamos usar con confianza los marcos de pertenencia y roles sin haber explorado el esquema de la base de datos creado en el paso 2. Sin embargo, al crear las tablas para almacenar los datos de la aplicación, es posible que sea necesario crear entidades relacionadas con usuarios o roles. Ayuda a estar familiarizado con los `SqlMembershipProvider` `SqlRoleProvider` esquemas y al establecer restricciones de clave externa entre las tablas de datos de la aplicación y las tablas creadas en el paso 2. Además, en algunas circunstancias poco frecuentes, es posible que necesitemos interactuar con el usuario y los almacenes de roles directamente en el nivel de base de datos (en lugar de a través de las `Membership` `Roles` clases o).

### <a name="partitioning-the-user-store-into-applications"></a>Creación de particiones del almacén de usuarios en aplicaciones

Los marcos de pertenencia y roles están diseñados de forma que un único usuario y un almacén de roles pueden compartirse entre muchas aplicaciones diferentes. Una aplicación ASP.NET que use los marcos de pertenencia o roles debe especificar qué partición de aplicación se va a usar. En Resumen, varias aplicaciones Web pueden usar los mismos almacenes de usuarios y roles. En la figura 11 se describen los almacenes de usuarios y roles con particiones en tres aplicaciones: HRSite, CustomerSite y SalesSite. Cada una de estas tres aplicaciones web tiene sus propios usuarios y roles únicos, pero todos ellos almacenan físicamente la información de la cuenta de usuario y el rol en las mismas tablas de base de datos.

[![Las cuentas de usuario se pueden particionar entre varias aplicaciones](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: las cuentas de usuario se pueden particionar en varias aplicaciones ([haga clic para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))

La `aspnet_Applications` tabla es lo que define estas particiones. Cada aplicación que usa la base de datos para almacenar la información de la cuenta de usuario se representa mediante una fila en esta tabla. La `aspnet_Applications` tabla tiene cuatro columnas: `ApplicationId` , `ApplicationName` , `LoweredApplicationName` y `Description` . `ApplicationId` es de tipo [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) y es la clave principal de la tabla; `ApplicationName` proporciona un nombre descriptivo único para cada aplicación.

Las demás tablas relacionadas con la pertenencia y los roles se vinculan de nuevo al `ApplicationId` campo de `aspnet_Applications` . Por ejemplo, la `aspnet_Users` tabla, que contiene un registro para cada cuenta de usuario, tiene un `ApplicationId` campo de clave externa; Ditto en la `aspnet_Roles` tabla. El `ApplicationId` campo de estas tablas especifica la partición de aplicación a la que pertenece la cuenta de usuario o el rol.

### <a name="storing-user-account-information"></a>Almacenar información de cuentas de usuario

La información de la cuenta de usuario se aloja en dos tablas: `aspnet_Users` y `aspnet_Membership` . La `aspnet_Users` tabla contiene campos que contienen la información de la cuenta de usuario esencial. Las tres columnas más relevantes son:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` es la clave principal (y de tipo `uniqueidentifier` ). `UserName` es de tipo `nvarchar(256)` y, junto con la contraseña, crea las credenciales del usuario. (La contraseña de un usuario se almacena en la `aspnet_Membership` tabla) `ApplicationId` . vincula la cuenta de usuario a una aplicación determinada en `aspnet_Applications` . Hay una [ `UNIQUE` restricción](https://msdn.microsoft.com/library/ms191166.aspx) compuesta en las `UserName` columnas y `ApplicationId` . Esto garantiza que, en una aplicación determinada, cada nombre de usuario es único, pero permite que `UserName` se use el mismo en aplicaciones diferentes.

La `aspnet_Membership` tabla incluye información adicional de la cuenta de usuario, como la contraseña del usuario, la dirección de correo electrónico, la fecha y hora del último inicio de sesión, etc. Hay una correspondencia uno a uno entre los registros de las `aspnet_Users` `aspnet_Membership` tablas y. Esta relación está garantizada por el `UserId` campo en `aspnet_Membership` , que actúa como la clave principal de la tabla. Al igual que la `aspnet_Users` tabla, `aspnet_Membership` incluye un `ApplicationId` campo que vincula esta información a una partición de aplicación determinada.

### <a name="securing-passwords"></a>Protección de contraseñas

La información de contraseña se almacena en la `aspnet_Membership` tabla. `SqlMembershipProvider`Permite que las contraseñas se almacenen en la base de datos mediante una de las tres técnicas siguientes:

- **Clear** : la contraseña se almacena en la base de datos como texto sin formato. Desaconsejamos encarecidamente usar esta opción. Si la base de datos se ve comprometida, un pirata informático que encuentra una puerta trasera o un empleado descontento que tiene acceso a la base de datos, cada una de las credenciales de un solo usuario está ahí.
- Las contraseñas con **hash** se aplican a un algoritmo hash unidireccional y un valor Salt generado de forma aleatoria. Este valor con hash (junto con el valor Salt) se almacena en la base de datos.
- **Cifrado** : una versión cifrada de la contraseña se almacena en la base de datos.

La técnica de almacenamiento de contraseñas usada depende de la `SqlMembershipProvider` configuración especificada en `Web.config` . Veremos cómo personalizar la `SqlMembershipProvider` configuración en el paso 4. El comportamiento predeterminado es almacenar el hash de la contraseña.

Las columnas responsables de almacenar la contraseña son `Password` , `PasswordFormat` y `PasswordSalt` . `PasswordFormat` es un campo de tipo `int` cuyo valor indica la técnica usada para almacenar la contraseña: 0 para Clear; 1 para Hashed; 2 para cifrado. `PasswordSalt` se le asigna una cadena generada de forma aleatoria independientemente de la técnica de almacenamiento de contraseñas usada. el valor de `PasswordSalt` solo se usa cuando se calcula el hash de la contraseña. Por último, la `Password` columna contiene los datos de contraseña reales, la contraseña de texto sin formato, el hash de la contraseña o la contraseña cifrada.

En la tabla 1 se muestra el aspecto de estas tres columnas para las distintas técnicas de almacenamiento al almacenar la contraseña persecret. .

| **Técnica de almacenamiento &lt; \_ O3A \_ p/&gt;** | **Contraseña &lt; \_ O3A \_ p/&gt;** | **PasswordFormat &lt; \_ O3A \_ p/&gt;** | **PasswordSalt &lt; \_ O3A \_ p/&gt;** |
| --- | --- | --- | --- |
| Desactivar | Misecreto! | 0 | tTnkPlesqissc2y2SMEygA = = |
| Con hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Cifrados | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/AA/oqAXGLHJNBw = = |

**Tabla 1**: valores de ejemplo para los campos relacionados con la contraseña al almacenar la contraseña persecret.

> [!NOTE]
> La configuración del elemento determina el cifrado o algoritmo hash determinado que utiliza `SqlMembershipProvider` `<machineKey>` . Analizamos este elemento de configuración en el paso 3 del <a id="Tutorial3"></a> tutorial [*configuración de autenticación de formularios y temas avanzados*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .

### <a name="storing-roles-and-role-associations"></a>Almacenar roles y asociaciones de roles

El marco de trabajo de roles permite a los desarrolladores definir un conjunto de roles y especificar qué usuarios pertenecen a qué roles. Esta información se captura en la base de datos a través de dos tablas: `aspnet_Roles` y `aspnet_UsersInRoles` . Cada registro de la `aspnet_Roles` tabla representa un rol para una aplicación determinada. Al igual que la `aspnet_Users` tabla, la `aspnet_Roles` tabla tiene tres columnas relacionadas con nuestra discusión:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` es la clave principal (y de tipo `uniqueidentifier` ). `RoleName` es de tipo `nvarchar(256)`. Y `ApplicationId` vincula la cuenta de usuario a una aplicación determinada en `aspnet_Applications` . Hay una restricción compuesta `UNIQUE` en las `RoleName` columnas y `ApplicationId` , asegurándose de que en una aplicación determinada cada nombre de rol sea único.

La `aspnet_UsersInRoles` tabla actúa como una asignación entre usuarios y roles. Solo hay dos columnas, `UserId` y `RoleId` juntas forman una clave principal compuesta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Paso 4: especificar el proveedor y personalizar su configuración

Todos los marcos de trabajo que admiten el modelo de proveedor, como los marcos de pertenencia y roles, carecen de detalles de implementación y, en su lugar, delegan esa responsabilidad en una clase de proveedor. En el caso del marco de pertenencia, la `Membership` clase define la API para administrar cuentas de usuario, pero no interactúa directamente con ningún almacén de usuario. En su lugar, los `Membership` métodos de la clase entregarán la solicitud al proveedor configurado. se utilizará `SqlMembershipProvider` . Cuando se invoca uno de los métodos de la `Membership` clase, ¿cómo sabe el marco de pertenencia delegar la llamada a `SqlMembershipProvider` ?

La `Membership` clase tiene una [ `Providers` propiedad](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) que contiene una referencia a todas las clases de proveedor registradas disponibles para su uso por parte del marco de pertenencia. Cada proveedor registrado tiene un nombre y un tipo asociados. El nombre proporciona una manera fácil de hacer referencia a un proveedor determinado en la `Providers` colección, mientras que el tipo identifica la clase del proveedor. Además, cada proveedor registrado puede incluir valores de configuración. Entre los valores de configuración para el marco de pertenencia se incluyen `passwordFormat` y `requiresUniqueEmail` , entre muchos otros. Vea la tabla 2 para obtener una lista completa de las opciones de configuración que usa `SqlMembershipProvider` .

El `Providers` contenido de la propiedad se especifica a través de los valores de configuración de la aplicación Web. De forma predeterminada, todas las aplicaciones web tienen un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider` . Este proveedor de pertenencia predeterminado se registra en `machine.config` (ubicado en `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)` :

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Como se muestra en el marcado anterior, el [ `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx) define los valores de configuración para el marco de pertenencia mientras el [ `<providers>` elemento secundario](https://msdn.microsoft.com/library/6d4936ht.aspx) especifica los proveedores registrados. Los proveedores se pueden agregar o quitar mediante [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) los [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementos o; utilice el [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) elemento para quitar todos los proveedores actualmente registrados. Como se muestra en el marcado anterior, `machine.config` agrega un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider` .

Además de los `name` atributos y `type` , el `<add>` elemento contiene atributos que definen los valores de diversas opciones de configuración. En la tabla 2 se enumeran las `SqlMembershipProvider` Opciones de configuración específicas de disponibles, junto con una descripción de cada una.

> [!NOTE]
> Los valores predeterminados indicados en la tabla 2 hacen referencia a los valores predeterminados definidos en la `SqlMembershipProvider` clase. Tenga en cuenta que no todos los valores de configuración de `AspNetSqlMembershipProvider` corresponden a los valores predeterminados de la `SqlMembershipProvider` clase. Por ejemplo, si no se especifica en un proveedor de pertenencia, el `requiresUniqueEmail` valor predeterminado es true. Sin embargo, `AspNetSqlMembershipProvider` reemplaza este valor predeterminado especificando explícitamente un valor de `false` .

| **Establecer &lt; \_ O3A \_ p/&gt;** | **Descripción &lt; \_ O3A \_ p/&gt;** |
| --- | --- |
| `ApplicationName` | Recuerde que el marco de pertenencia permite que un único almacén de usuario se divida en varias aplicaciones. Este valor indica el nombre de la partición de aplicación que utiliza el proveedor de pertenencia. Si no se especifica explícitamente este valor, se establece, en tiempo de ejecución, en el valor de la ruta de acceso de la raíz virtual de la aplicación. |
| `commandTimeout` | Especifica el valor de tiempo de espera del comando SQL (en segundos). El valor predeterminado es 30. |
| `connectionStringName` | Nombre de la cadena de conexión en el `<connectionStrings>` elemento que se va a usar para conectarse a la base de datos del almacén de usuarios. Este valor es necesario. |
| `description` | Proporciona una descripción descriptiva del proveedor registrado. |
| `enablePasswordRetrieval` | Especifica si los usuarios pueden recuperar su contraseña olvidada. El valor predeterminado es `false`. |
| `enablePasswordReset` | Indica si los usuarios pueden restablecer su contraseña. Tiene como valor predeterminado `true`. |
| `maxInvalidPasswordAttempts` | Número máximo de intentos de inicio de sesión incorrectos que pueden producirse para un usuario determinado durante el especificado `passwordAttemptWindow` antes de que se bloquee el usuario. El valor predeterminado es 5. |
| `minRequiredNonalphanumericCharacters` | Número mínimo de caracteres no alfanuméricos que deben aparecer en la contraseña de un usuario. Este valor debe estar comprendido entre 0 y 128; el valor predeterminado es 1. |
| `minRequiredPasswordLength` | Número mínimo de caracteres necesarios en una contraseña. Este valor debe estar comprendido entre 0 y 128; el valor predeterminado es 7. |
| `name` | Nombre del proveedor registrado. Este valor es necesario. |
| `passwordAttemptWindow` | El número de minutos durante los que se realiza el seguimiento de los intentos de inicio de sesión incorrectos. Si un usuario proporciona las credenciales de inicio de sesión no válidas `maxInvalidPasswordAttempts` en esta ventana especificada, se bloquearán. El valor predeterminado es 10. |
| `PasswordFormat` | El formato de almacenamiento de contraseñas: `Clear` , `Hashed` o `Encrypted` . El valor predeterminado es `Hashed`. |
| `passwordStrengthRegularExpression` | Si se proporciona, esta expresión regular se usa para evaluar la seguridad de la contraseña seleccionada del usuario al crear una nueva cuenta o al cambiar su contraseña. El valor predeterminado es una cadena vacía. |
| `requiresQuestionAndAnswer` | Especifica si un usuario debe responder a su pregunta de seguridad al recuperar o restablecer su contraseña. El valor predeterminado es `true`. |
| `requiresUniqueEmail` | Indica si todas las cuentas de usuario de una partición de aplicación determinada deben tener una dirección de correo electrónico única. El valor predeterminado es `true`. |
| `type` | Especifica el tipo del proveedor. Este valor es necesario. |

**Tabla 2**: configuración de pertenencia y `SqlMembershipProvider` configuración

Además de `AspNetSqlMembershipProvider` , se pueden registrar otros proveedores de pertenencia en función de la aplicación mediante la adición de marcado similar al `Web.config` archivo.

> [!NOTE]
> El marco de trabajo de roles funciona de la misma manera: hay un proveedor de roles registrado predeterminado en `machine.config` y los proveedores registrados pueden personalizarse aplicación por aplicación en `Web.config` . Examinaremos en detalle el marco de trabajo de roles y su marcado de configuración en un tutorial futuro.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalización de la `SqlMembershipProvider` configuración

El valor predeterminado `SqlMembershipProvider` ( `AspNetSqlMembershipProvider` ) tiene el `connectionStringName` atributo establecido en `LocalSqlServer` . Al igual que el `AspNetSqlMembershipProvider` proveedor, el nombre `LocalSqlServer` de la cadena de conexión se define en `machine.config` .

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Como puede ver, esta cadena de conexión define una base de datos de SQL 2005 Express Edition ubicada en | DataDirectory | Aspnetdb. MDF '. La cadena | DataDirectory | se traduce en tiempo de ejecución para que apunte al `~/App_Data/` directorio, por lo que la ruta de acceso de la base de datos | DataDirectory | Aspnetdb. MDF "se convierte en `~/App_Data` / `aspnet.mdf` .

Si no se especifica ninguna información del proveedor de pertenencia en el archivo de la aplicación `Web.config` , la aplicación utiliza el proveedor de pertenencia registrado predeterminado, `AspNetSqlMembershipProvider` . Si la `~/App_Data/aspnet.mdf` base de datos no existe, el tiempo de ejecución de ASP.net lo creará automáticamente y agregará el esquema de servicios de aplicación. Sin embargo, no queremos usar la `aspnet.mdf` base de datos; en su lugar, queremos usar la `SecurityTutorials.mdf` base de datos que creamos en el paso 2. Esta modificación puede realizarse de una de estas dos maneras:

- <strong>Especifique un valor para el</strong> <strong>`LocalSqlServer`</strong> <strong>nombre de la cadena de conexión en</strong> <strong>`Web.config`</strong> <strong>.</strong> Al sobrescribir el `LocalSqlServer` valor de nombre de cadena de conexión en `Web.config` , se puede usar el proveedor de pertenencia registrado predeterminado ( `AspNetSqlMembershipProvider` ) y hacer que funcione correctamente con la `SecurityTutorials.mdf` base de datos. Este enfoque es correcto si está contenido con las opciones de configuración especificadas por `AspNetSqlMembershipProvider` . Para obtener más información sobre esta técnica, vea la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configurar ASP.net 2,0 Servicios de aplicación para usar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Agregar un nuevo proveedor registrado de tipo</strong> <strong>`SqlMembershipProvider`</strong> <strong>y configure su</strong> <strong>`connectionStringName`</strong> <strong>establecer para que apunte al</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de datos.</strong> Este enfoque es útil en escenarios en los que desea personalizar otras propiedades de configuración además de la cadena de conexión de base de datos. En mis proyectos, siempre utilizo este enfoque debido a su flexibilidad y legibilidad.

Antes de poder agregar un nuevo proveedor registrado que haga referencia a la `SecurityTutorials.mdf` base de datos, primero debemos agregar un valor de cadena de conexión adecuado en la `<connectionStrings>` sección de `Web.config` . El marcado siguiente agrega una nueva cadena de conexión denominada `SecurityTutorialsConnectionString` que hace referencia a la `SecurityTutorials.mdf` base de datos de SQL Server 2005 Express Edition de la `App_Data` carpeta.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Si utiliza un archivo de base de datos alternativo, actualice la cadena de conexión según sea necesario. Para obtener más información sobre cómo formar la cadena de conexión correcta, consulte [connectionStrings.com](http://www.connectionstrings.com/).

A continuación, agregue el siguiente marcado de configuración de pertenencia al `Web.config` archivo. Este marcado registra un nuevo proveedor denominado `SecurityTutorialsSqlMembershipProvider` .

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Además de registrar el `SecurityTutorialsSqlMembershipProvider` proveedor, el marcado anterior define `SecurityTutorialsSqlMembershipProvider` como el proveedor predeterminado (a través del `defaultProvider` atributo en el `<membership>` elemento). Recuerde que el marco de pertenencia puede tener varios proveedores registrados. Puesto que `AspNetSqlMembershipProvider` se registra como el primer proveedor de `machine.config` , actúa como proveedor predeterminado a menos que se indique lo contrario.

Actualmente, nuestra aplicación tiene dos proveedores registrados: `AspNetSqlMembershipProvider` y `SecurityTutorialsSqlMembershipProvider` . Sin embargo, antes de registrar el proveedor, podríamos `SecurityTutorialsSqlMembershipProvider` haber borrado todos los proveedores previamente registrados agregando un [ `<clear />` elemento](https://msdn.microsoft.com/library/t062y6yc.aspx) inmediatamente antes de nuestro `<add>` elemento. Esto borraría `AspNetSqlMembershipProvider` de la lista de proveedores registrados, lo que significa que el `SecurityTutorialsSqlMembershipProvider` sería el único proveedor de pertenencia registrado. Si usamos este enfoque, no sería necesario marcar `SecurityTutorialsSqlMembershipProvider` como el proveedor predeterminado, ya que sería el único proveedor de pertenencia registrado. Para obtener más información sobre el uso de `<clear />` , vea [usar `<clear />` al agregar proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Tenga en cuenta que la `SecurityTutorialsSqlMembershipProvider` `connectionStringName` configuración de hace referencia al nombre de la cadena de conexión recién agregada `SecurityTutorialsConnectionString` y que su configuración se ha `applicationName` establecido en un valor de SecurityTutorials. Además, la `requiresUniqueEmail` configuración se ha establecido en `true` . Todas las demás opciones de configuración son idénticas a los valores de `AspNetSqlMembershipProvider` . Si lo desea, puede realizar cualquier modificación de configuración. Por ejemplo, puede reforzar la seguridad de la contraseña al requerir dos caracteres no alfanuméricos en lugar de uno, o bien aumentar la longitud de la contraseña a ocho caracteres en lugar de a siete.

> [!NOTE]
> Recuerde que el marco de pertenencia permite que un único almacén de usuario se divida en varias aplicaciones. La configuración del proveedor de pertenencia `applicationName` indica qué aplicación utiliza el proveedor cuando se trabaja con el almacén del usuario. Es importante que establezca explícitamente un valor para la `applicationName` opción de configuración porque si `applicationName` no se establece explícitamente, se asigna a la ruta de acceso de la raíz virtual de la aplicación web en tiempo de ejecución. Esto funciona bien siempre y cuando la ruta de acceso de la raíz virtual de la aplicación no cambie, pero si la aplicación se mueve a una ruta de acceso diferente, la `applicationName` configuración también cambiará. Cuando esto sucede, el proveedor de pertenencia comenzará a trabajar con una partición de aplicación diferente de la que se usó anteriormente. Las cuentas de usuario creadas antes del traslado residirán en una partición de aplicación diferente y esos usuarios ya no podrán iniciar sesión en el sitio. Para obtener información más detallada sobre esta cuestión, vea [establecer siempre la `applicationName` propiedad al configurar la pertenencia a ASP.net 2,0 y otros proveedores](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Resumen

En este momento, tenemos una base de datos con los servicios de aplicación configurados ( `SecurityTutorials.mdf` ) y configuramos nuestra aplicación web para que el marco de pertenencia use el `SecurityTutorialsSqlMembershipProvider` proveedor que acabamos de registrar. Este proveedor registrado es de tipo `SqlMembershipProvider` y su `connectionStringName` conjunto se establece en la cadena de conexión adecuada ( `SecurityTutorialsConnectionString` ) y su `applicationName` valor se establece explícitamente.

Ahora estamos listos para usar el marco de pertenencia de nuestra aplicación. En el siguiente tutorial, examinaremos cómo crear nuevas cuentas de usuario. A continuación, se analizarán los usuarios de autenticación, la autorización basada en el usuario y el almacenamiento de información adicional relacionada con el usuario en la base de datos.

¡ Feliz programación!

### <a name="further-reading"></a>Más información

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Establecer siempre la `applicationName` propiedad al configurar la pertenencia a ASP.NET 2,0 y otros proveedores](https://weblogs.asp.net/scottgu/443634)
- [Configuración de ASP.NET 2,0 Servicios de aplicación para usar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Descargar SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen de la pertenencia, los roles y el perfil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>`Elemento para los proveedores de pertenencia](https://msdn.microsoft.com/library/whae3t94.aspx)
- [El `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>`Elemento para la pertenencia](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Usar `<clear />` al agregar proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabajar directamente con el `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vídeo de aprendizaje sobre temas incluidos en este tutorial

- [Descripción de las pertenencias a ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurar SQL para trabajar con esquemas de pertenencia](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Cambiar la configuración de la pertenencia en el esquema de pertenencia predeterminado](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/) .

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Alicja Maziarz. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, suéltelo en una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com) .

> [!div class="step-by-step"]
> [Siguiente](creating-user-accounts-cs.md)
