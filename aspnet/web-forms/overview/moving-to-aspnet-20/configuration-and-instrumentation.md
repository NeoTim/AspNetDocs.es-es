---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuración e Instrumentación (Configuración e Instrumentación) Microsoft Docs
author: rick-anderson
description: Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 30ea2ffd3d055c5373a33615bc19a8f68fb568ab
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543059"
---
# <a name="configuration-and-instrumentation"></a>Configuración e instrumentación

por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

En este módulo, discutiremos ASP.NET la API de configuración en lo que se refiere a la lectura y escritura en ASP.NET archivos de configuración, y también trataremos ASP.NET instrumentación. También cubriremos las nuevas características disponibles en ASP.NET el trazado.

## <a name="aspnet-configuration-api"></a>API de configuración de ASP.NET

La API de configuración de ASP.NET le permite desarrollar, implementar y administrar datos de configuración de aplicaciones mediante una única interfaz de programación. Puede usar la API de configuración para desarrollar y modificar configuraciones completas de ASP.NET mediante programación sin editar directamente el XML en los archivos de configuración. Además, puede usar la API de configuración en aplicaciones de consola y scripts que desarrolle, en herramientas de administración basadas en web y en complementos de Microsoft Management Console (MMC).

Las dos herramientas de administración de configuración siguientes utilizan la API de configuración y se incluyen con la versión 2.0 de .NET Framework:

- El complemento MMC ASP.NET, que utiliza la API de configuración para simplificar las tareas administrativas, proporcionando una vista integrada de los datos de configuración local de todos los niveles de la jerarquía de configuración.
- La herramienta de administración de sitios web, que le permite administrar la configuración de aplicaciones locales y remotas, incluidos los sitios hospedados.

La API de configuración ASP.NET comprende un conjunto de objetos de administración de ASP.NET que puede usar para configurar aplicaciones y sitios web mediante programación. Los objetos de administración se implementan como una biblioteca de clases de .NET Framework. El modelo de programación de API de configuración ayuda a garantizar la coherencia y confiabilidad del código al aplicar tipos de datos en tiempo de compilación. Para facilitar la administración de las configuraciones de la aplicación, la API de configuración le permite ver los datos que se combinan desde todos los puntos de la jerarquía de configuración como una sola colección, en lugar de ver los datos como colecciones independientes de diferentes archivos de configuración. Además, la API de configuración le permite manipular configuraciones de aplicación completas sin editar directamente el XML en los archivos de configuración. Por último, la API simplifica las tareas de configuración al admitir herramientas administrativas, como la herramienta de administración de sitios web. La API de configuración simplifica la implementación al admitir la creación de archivos de configuración en un equipo y la ejecución de scripts de configuración en varios equipos.

> [!NOTE]
> La API de configuración no admite la creación de aplicaciones IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Trabajar con configuraciones de configuración local y remota

Un objeto Configuration representa la vista combinada de los valores de configuración que se aplican a una entidad física específica, como un equipo, o a una entidad lógica, como una aplicación o un sitio Web. La entidad lógica especificada puede existir en el equipo local o en un servidor remoto. Cuando no existe ningún archivo de configuración para una entidad especificada, el objeto Configuration representa los valores de configuración predeterminados definidos por el archivo Machine.config.

Puede obtener un objeto Configuration mediante uno de los métodos de configuración abiertos de las clases siguientes:

1. La clase ConfigurationManager, si la entidad es una aplicación cliente.
2. La clase WebConfigurationManager, si la entidad es una aplicación web.

Estos métodos devolverán un objeto Configuration, que a su vez proporciona los métodos y propiedades necesarios para controlar los archivos de configuración subyacentes. Puede acceder a estos archivos para leer los archivos o escribirlos.

### <a name="reading"></a>Lectura

Utilice el método GetSection o GetSectionGroup para leer la información de configuración. El usuario o proceso que lee debe tener permisos de lectura en todos los archivos de configuración de la jerarquía.

> [!NOTE]
> Si utiliza un método GetSection estático que toma un parámetro path, el parámetro path debe hacer referencia a la aplicación en la que se ejecuta el código. De lo contrario, se omite el parámetro y se devuelve la información de configuración de la aplicación que se está ejecutando actualmente.

### <a name="writing"></a>Escritura

Utilice uno de los métodos Save para escribir información de configuración. El usuario o proceso que escribe debe tener permisos de escritura en el archivo de configuración y el directorio en el nivel de jerarquía de configuración actual, así como permisos de lectura en todos los archivos de configuración de la jerarquía.

Para generar un archivo de configuración que represente los valores de configuración heredados para una entidad especificada, utilice uno de los siguientes métodos de configuración de guardado:

1. El método Save para crear un nuevo archivo de configuración.
2. El método SaveAs para generar un nuevo archivo de configuración en otra ubicación.

## <a name="configuration-classes-and-namespaces"></a>Clases de configuración y espacios de nombres

Muchas clases y métodos de configuración son similares entre sí. En la tabla siguiente se describen las clases de configuración y espacios de nombres más utilizados.

| **Clase de configuración o espacio de nombres** | **Descripción** |
| --- | --- |
| [Espacio de nombres System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Contiene las clases de configuración principales para todas las aplicaciones de .NET Framework. Las clases de controlador de sección se usan para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. Estos dos métodos no son estáticos. |
| Clase System.Configuration.Configuration | Representa un conjunto de datos de configuración para un equipo, aplicación, directorio web u otro recurso. Esta clase contiene métodos útiles, como GetSection y GetSectionGroup, para actualizar la configuración y obtener referencias a secciones y grupos de secciones. Esta clase se utiliza como un tipo de valor devuelto para los métodos que obtienen datos de configuración en tiempo de diseño, como los métodos de las clases WebConfigurationManager y ConfigurationManager. |
| Espacio de nombres System.Web.Configuration | Contiene las clases de controlador de sección para las secciones de configuración de ASP.NET definidas en [ASP.NET Configuración](https://msdn.microsoft.com/library/b5ysx397.aspx). Las clases de controlador de sección se usan para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. |
| Clase System.Web.Configuration.WebConfigurationManager | Proporciona métodos útiles para obtener referencias a los valores de configuración en tiempo de ejecución y en tiempo de diseño. Estos métodos utilizan la clase System.Configuration.Configuration como tipo de valor devuelto. Puede usar el método estático GetSection de esta clase o el método GetSection no estático de la clase System.Configuration.ConfigurationManager indistintamente. Para las configuraciones de aplicaciones web, se recomienda la clase System.Web.Configuration.WebConfigurationManager en lugar de la clase System.Configuration.ConfigurationManager. |
| [Espacio de nombres System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Proporciona una manera de personalizar y ampliar el proveedor de configuración. Esta es la clase base para todas las clases de proveedor en el sistema de configuración. |
| [Espacio de nombres System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contiene clases e interfaces para administrar y supervisar el estado de las aplicaciones web. Estrictamente hablando, este espacio de nombres no se considera parte de la API de configuración. Por ejemplo, el seguimiento y la activación de eventos se realiza mediante las clases de este espacio de nombres. |
| [Espacio de nombres System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Proporciona las clases necesarias para la instrumentación de aplicaciones para exponer su información de administración y eventos a través de Instrumental de administración de Windows (WMI) a los consumidores potenciales. ASP.NET supervisión de estado utiliza WMI para entregar eventos. Estrictamente hablando, este espacio de nombres no se considera parte de la API de configuración. |

## <a name="reading-from-aspnet-configuration-files"></a>Lectura de ASP.NET archivos de configuración

La clase WebConfigurationManager es la clase principal para leer de ASP.NET archivos de configuración. Hay esencialmente tres pasos para leer ASP.NET archivos de configuración:

1. Obtener un Configuration objeto mediante el OpenWebConfiguration método.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Lea la información deseada del archivo de configuración.

El objeto Configuration representa no representa un archivo de configuración determinado. En su lugar, representa una vista combinada de la configuración de un equipo, aplicación o sitio web. En el ejemplo de código siguiente se crea una instancia de un objeto Configuration que representa la configuración de una aplicación web denominada *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Tenga en cuenta que si la ruta de acceso /ProductInfo no existe, el código anterior devolverá la configuración predeterminada tal como se especifica en el archivo machine.config.

Una vez que tenga el objeto Configuration, puede usar el método GetSection o GetSectionGroup para profundizar en los valores de configuración. En el ejemplo siguiente se obtiene una referencia a la configuración de suplantación para la aplicación ProductInfo anterior:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Escribir en ASP.NET archivos de configuración

Al igual que al leer de los archivos de configuración, la clase WebConfigurationManager es el núcleo para escribir en Asp.NET archivos de configuración. También hay tres pasos para escribir en ASP.NET archivos de configuración.

1. Obtener un Configuration objeto mediante el OpenWebConfiguration método.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Escriba la información deseada desde el archivo de configuración mediante el método Save o SaveAs.

El código siguiente cambia el &lt;&gt; atributo **debug** del elemento de compilación a false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Cuando se ejecuta este código, **el** atributo debug del &lt;elemento de compilación&gt; se establecerá en false para el archivo web.config de la aplicación *webApp.*

## <a name="systemwebmanagement-namespace"></a>Espacio de nombres System.Web.Management

El espacio de nombres System.Web.Management proporciona las clases e interfaces para administrar y supervisar el estado de las aplicaciones ASP.NET.

El registro se realiza mediante la definición de una regla que asocia eventos con un proveedor. La regla define el tipo de eventos que se envían al proveedor. Los siguientes eventos base están disponibles para que los registre:

| **WebBaseEvent** | La clase de evento base para todos los eventos. Contiene las propiedades necesarias para todos los eventos, como el código de evento, el código de detalle del evento, la fecha y hora en que se generó el evento, el número de secuencia, el mensaje de evento y los detalles del evento. |
| --- | --- |
| **Evento WebManagementEvent** | La clase de evento base para eventos de administración, como la duración de la aplicación, la solicitud, el error y los eventos de auditoría. |
| **WebHeartbeatEvent** | El evento generado por la aplicación en intervalos regulares para capturar información útil del estado del tiempo de ejecución. |
| **WebAuditEvent** | La clase base para eventos de auditoría de seguridad, que se utilizan para marcar condiciones como error de autorización, error de descifrado, *etc.* |
| **WebRequestEvent** | La clase base para todos los eventos de solicitud informativa. |
| **WebBaseErrorEvent** | La clase base para todos los eventos que indican condiciones de error. |

Los tipos de proveedores disponibles le permiten enviar la salida de eventos al Visor de eventos, SQL Server, Instrumental de administración de Windows (WMI) y correo electrónico. Los proveedores preconfigurados y las asignaciones de eventos reducen la cantidad de trabajo necesario para registrar la salida del evento.

ASP.NET 2.0 usa el proveedor de registro de eventos listo para usar para registrar eventos basados en dominios de aplicación que se inician y detienen, así como para registrar las excepciones no controladas. Esto ayuda a cubrir algunos de los escenarios básicos. Por ejemplo, supongamos que la aplicación produce una excepción, pero el usuario no guarda el error y no puede reproducirlo. Con la regla de registro de eventos predeterminada, podría recopilar la información de excepción y pila para tener una mejor idea de qué tipo de error se produjo. Otro ejemplo se aplica si la aplicación está perdiendo el estado de sesión. En ese caso, puede buscar en el registro de eventos para determinar si el dominio de aplicación se está reciclando y por qué el dominio de aplicación se detuvo en primer lugar.

Además, el sistema de supervisión de la salud es extensible. Por ejemplo, puede definir eventos Web personalizados, activarlos dentro de la aplicación y, a continuación, definir una regla para enviar la información del evento a un proveedor como su correo electrónico. Esto le permite vincular fácilmente su instrumentación a los proveedores de monitoreo de salud. Como otro ejemplo, podría desencadenar un evento cada vez que se procesa un pedido y configurar una regla que envía cada evento a la base de datos de SQL Server. También puede desencadenar un evento cuando un usuario no puede iniciar sesión varias veces en una fila y configurar el evento para usar los proveedores basados en correo electrónico.

La configuración de los proveedores y eventos predeterminados se almacena en el archivo Web.config global. El archivo Web.config global almacena todas las configuraciones basadas en Web que se almacenaron en el archivo Machine.config en ASP.NET 1x. El archivo Web.config global se encuentra en el directorio siguiente:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

La &lt;sección&gt; healthMonitoring del archivo Web.config global proporciona la configuración predeterminada. Puede invalidar esta configuración o configurar sus &lt;propias opciones implementando la sección healthMonitoring&gt; en el archivo Web.config de la aplicación.

La &lt;sección&gt; healthMonitoring del archivo Web.config global contiene los siguientes elementos:

| **providers** | Contiene proveedores configurados para el Visor de eventos, WMI y SQL Server. |
| --- | --- |
| **eventMappings** | Contiene asignaciones para las diversas clases WebBase. Puede ampliar esta lista si genera su propia clase de evento. Generar su propia clase de eventos le proporciona una granularidad más fina sobre los proveedores a los que envía información. Por ejemplo, podría configurar excepciones no controladas para enviarlas a SQL ServerSQL Server, mientras envía sus propios eventos personalizados al correo electrónico. |
| **Reglas** | Vincula eventMappings al proveedor. |
| **Búfer** | Se utiliza con SQL ServerSQL Server y proveedores de correo electrónico para determinar la frecuencia con la que se vacían los eventos en el proveedor. |

A continuación se muestra un ejemplo de código del archivo Web.config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Cómo almacenar eventos en el Visor de eventos

Como se mencionó anteriormente, el proveedor para registrar eventos en el Visor de eventos está configurado automáticamente en el archivo Web.config global. De forma predeterminada, se registran todos los eventos basados en **WebBaseErrorEvent** y **WebFailureAuditEvent.** Puede agregar reglas adicionales para registrar información adicional en el registro de eventos. Por ejemplo, si desea registrar todos los eventos *(es decir,* cada evento basado en **WebBaseEvent**), puede agregar la siguiente regla al archivo Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Esta regla vincularía el mapa de eventos **Todos los eventos** al proveedor de registro de eventos. Tanto eventMapping como el proveedor se incluyen en el archivo Web.config global.

## <a name="how-to-store-events-to-sql-server"></a>Cómo almacenar eventos en SQL Server

Este método utiliza la base de datos **ASPNETDB,** que genera la herramienta aspnet\_regsql.exe. El proveedor predeterminado usa la cadena de conexión LocalSqlServer, que\_usa una base de datos basada en archivos en la carpeta de datos de la aplicación o la instancia local de SQLExpress de SQL Server. Tanto la cadena de conexión LocalSqlServer como SqlProvider se configuran en el archivo Web.config global.

La cadena de conexión LocalSqlServer del archivo Web.config global tiene este aspecto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si desea usar otra instancia de SQL Server, deberá\_usar la herramienta regsql.exe de Aspnet, que se encuentra en %windir%. \* carpeta. Use la\_herramienta Regsql.exe de Aspnet para generar una base de datos **ASPNETDB** personalizada en la instancia de SQL Server, agregue la cadena de conexión al archivo de configuración de aplicaciones y, a continuación, agregue un proveedor mediante la nueva cadena de conexión. Una vez creada la base de datos **ASPNETDB,** deberá establecer una regla para vincular un eventMapping a sqlProvider.

Si usa el valor predeterminado SqlProvider o configurar su propio proveedor, deberá agregar una regla que vincule al proveedor con un mapa de eventos. La siguiente regla vincula el nuevo proveedor que creó anteriormente al mapa de eventos **Todos los eventos.** Esta regla registrará todos los eventos basados en **WebBaseEvent** y los enviará a MySqlWebEventProvider que usará la cadena de conexión MYASPNETDB. El código siguiente agrega una regla para vincular el proveedor con un mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si solo desea enviar errores a SQL Server, puede agregar la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Cómo reenviar eventos a WMI

También puede reenviar los eventos a WMI. El proveedor WMI está configurado automáticamente en el archivo Web.config global de forma predeterminada.

En el ejemplo de código siguiente se agrega una regla para reenviar los eventos a WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Deberá agregar una regla para asociar un eventMapping al proveedor y también una aplicación de escucha WMI para escuchar los eventos. En el ejemplo de código siguiente se agrega una regla para vincular el proveedor WMI al mapa de eventos **Todos los eventos:**

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Cómo reenviar eventos al correo electrónico

También puede reenviar eventos al correo electrónico. Tenga cuidado con las reglas de eventos que asigna a su proveedor de correo electrónico, ya que puede enviarse involuntariamente una gran cantidad de información que puede ser más adecuada para SQL ServerSQL Server o el registro de eventos. Hay dos proveedores de correo electrónico; SimpleMailWebEventProvider y TemplatedMailWebEventProvider. Cada uno tiene los mismos atributos de configuración, con la excepción de los atributos "template" y "detailedTemplateErrors", los cuales solo están disponibles en TemplatedMailWebEventProvider.

> [!NOTE]
> Ninguno de estos proveedores de correo electrónico está configurado para usted. Deberá agregarlos al archivo Web.config.

La principal diferencia entre estos dos proveedores de correo electrónico es que SimpleMailWebEventProvider envía correos electrónicos en una plantilla genérica que no se puede modificar. El archivo Web.config de ejemplo agrega este proveedor de correo electrónico a la lista de proveedores configurados mediante la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

La siguiente regla también se agrega para vincular el proveedor de correo electrónico al mapa de eventos **Todos los eventos:**

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>seguimiento de ASP.NET 2.0

Hay tres mejoras principales en el seguimiento en ASP.NET 2.0.

1. Funcionalidad de rastreo integrada
2. Acceso mediante programación a mensajes de seguimiento
3. Seguimiento mejorado a nivel de aplicación

## <a name="integrated-tracing-functionality"></a>Funcionalidad de seguimiento integrado

Ahora puede enrutar los mensajes emitidos por la clase System.Diagnostics.Trace para ASP.NET salida de seguimiento y enrutar los mensajes emitidos por ASP.NET seguimiento a System.Diagnostics.Trace. También puede reenviar ASP.NET eventos de instrumentación a System.Diagnostics.Trace. Esta funcionalidad la proporciona el nuevo atributo **writeToDiagnosticsTrace** del &lt;elemento trace.&gt; Cuando este valor booleano es true, ASP.NET los mensajes de seguimiento se reenvían a la infraestructura de seguimiento System.Diagnostics para que los agentes de escucha registrados para mostrar los mensajes de seguimiento los usen.

## <a name="programmatic-access-to-trace-messages"></a>Acceso mediante programación a los mensajes de seguimiento

ASP.NET 2.0 permite el acceso mediante programación a todos los mensajes de seguimiento a través de la **TraceContextRecord** clase y el **TraceRecords** colección. La forma más eficaz de obtener acceso a los mensajes de seguimiento es registrar un delegado **TraceContextEventHandler** (también nuevo en ASP.NET 2.0) para controlar el nuevo evento **TraceFinished.** A continuación, puede recorrer los mensajes de seguimiento como desee.

El ejemplo de código siguiente ilustra esto:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

En el ejemplo anterior, recorro la colección TraceRecords y, a continuación, escribo cada mensaje en la secuencia Response.

## <a name="improved-application-level-tracing"></a>Seguimiento mejorado del nivel de aplicación

El seguimiento de nivel de aplicación se mejora mediante &lt;&gt; la introducción del nuevo atributo **mostRecent** del elemento trace. Este atributo especifica si se muestra la salida de seguimiento de nivel de aplicación más reciente y se descartan los datos de seguimiento más antiguos más allá de los límites indicados por requestLimit. Si es false, se muestran los datos de seguimiento para las solicitudes hasta que se alcanza el atributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Herramientas de línea de comandos ASP.NET

Hay varias herramientas de línea de comandos para ayudar en la configuración de ASP.NET. ASP.NET desarrolladores deben estar\_familiarizados con la herramienta aspnet regiis.exe. ASP.NET 2.0 proporciona otras tres herramientas de línea de comandos para ayudar en la configuración.

Están disponibles las siguientes herramientas de línea de comandos:

| **Herramienta** | **Uso** |
| --- | --- |
| **aspnet\_regiis.exe** | Permite el registro de ASP.NET con IIS. Hay dos versiones de estas herramientas que se encenden con ASP.NET 2.0, una para sistemas de 32 bits (en la carpeta Framework) y otra para sistemas de 64 bits (en la carpeta Framework64.) La versión de 64 bits no se instalará en un sistema operativo de 32 bits. |
| **aspnet\_regsql.exe** | La ASP.NET herramienta registro de SQL Server se usa para crear una base de datos de Microsoft SQL Server para que la usen los proveedores de SQL Server en ASP.NET o para agregar o quitar opciones de una base de datos existente. El archivo\_regsql.exe de Aspnet se encuentra en la carpeta [unidad:]-WINDOWS-Microsoft.NET-Framework-versionNumber del servidor Web. |
| **aspnet\_regbrowsers.exe** | La herramienta ASP.NET registro del explorador analiza y compila todas las definiciones del explorador de todo el sistema en un ensamblado e instala el ensamblado en la caché global de ensamblados. La herramienta utiliza los archivos de definición del navegador (. BROWSER) desde el subdirectorio Exploradores de .NET Framework. La herramienta se puede encontrar en el directorio %SystemRoot%- Microsoft.NET -Framework-version. |
| **aspnet\_compiler.exe** | La herramienta de compilación de ASP.NET le permite compilar una aplicación web ASP.NET, ya sea en su lugar o para su implementación en una ubicación de destino, como un servidor de producción. La compilación en el lugar ayuda al rendimiento de la aplicación porque los usuarios finales no encuentran un retraso en la primera solicitud a la aplicación mientras se compila la aplicación. |

Debido a\_que la herramienta aspnet regiis.exe no es nueva en ASP.NET 2.0, no lo discutiremos aquí.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>ASP.NET Herramienta de registro\_de SQL Server - aspnet regsql.exe

Puede establecer varios tipos de opciones mediante la herramienta ASP.NET registro de SQL ServerSQL Server . Puede especificar una conexión SQL, especificar qué servicios de aplicación ASP.NET usar SQL ServerSQL Server para administrar información, indicar qué base de datos o tabla se usa para la dependencia de caché SQL y agregar o quitar compatibilidad para usar SQL ServerSQL Server para almacenar procedimientos y estado de sesión.

Varios ASP.NET servicios de aplicaciones dependen de un proveedor para administrar el almacenamiento y la recuperación de datos de un origen de datos. Cada proveedor es específico del origen de datos. ASP.NET incluye un proveedor de SQL Server para las siguientes características de ASP.NET:

- Pertenencia (la clase [SqlMembershipProvider).](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)
- Administración de roles (la clase [SqlRoleProvider).](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)
- Profile (la clase [SqlProfileProvider).](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)
- Personalización de elementos web (la clase [SqlPersonalizationProvider).](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)
- Eventos Web (la clase [SqlWebEventProvider).](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)

Al instalar ASP.NET, el archivo Machine.config del servidor incluye elementos de configuración que especifican proveedores de SQL Server para cada una de las características de ASP.NET que dependen de un proveedor. Estos proveedores están configurados, de forma predeterminada, para conectarse a una instancia de usuario local de SQL Server Express 2005. Si cambia la cadena de conexión predeterminada utilizada por los proveedores, antes de poder usar cualquiera de las características de ASP.NET configuradas\_en la configuración del equipo, debe instalar la base de datos de SQL Server y los elementos de base de datos para la característica elegida mediante Aspnet regsql.exe. Si la base de datos que especifique con la herramienta de registro SQL aún no existe (aspnetdb será la base de datos predeterminada si no se especifica una en la línea de comandos), el usuario actual debe tener derechos para crear bases de datos en SQL ServerSQL Server, así como para crear elementos de esquema dentro de una base de datos.

### <a name="sql-cache-dependency"></a>Dependencia de memoria caché de SQL

Una característica avanzada del almacenamiento en caché de resultados de ASP.NET es la dependencia de caché SQL. La dependencia de caché SQL admite dos modos de operación diferentes: uno que usa una implementación ASP.NET de sondeo de tablas y un segundo modo que usa las características de notificación de consulta de SQL Server 2005. La herramienta de registro SQL se puede utilizar para configurar el modo de operación de sondeo de tablas.

### <a name="session-state"></a>Estado de sesión

De forma predeterminada, los valores de estado de sesión y la información se almacenan en la memoria dentro del proceso de ASP.NET. Como alternativa, puede almacenar datos de sesión en una base de datos de SQL Server, donde pueden ser compartidos por varios servidores web. Si la base de datos que especifique para el estado de sesión con la herramienta de registro SQL aún no existe, el usuario actual debe tener derechos para crear bases de datos en SQL ServerSQL Server, así como para crear elementos de esquema dentro de una base de datos. Si la base de datos existe, el usuario actual debe tener derechos para crear elementos de esquema en la base de datos existente.

Para instalar la base de datos de\_estado de sesión en SQL Server, ejecute la herramienta regsql.exe de Aspnet y proporcione la siguiente información con el comando:

- El nombre de la instancia de SQL Server, mediante la opción **-S.**
- Las credenciales de inicio de sesión de una cuenta que tiene permiso para crear una base de datos en un equipo que ejecuta SQL Server. Utilice la opción **-E** para utilizar el usuario que ha iniciado sesión actualmente o utilice la opción **-U** para especificar un ID de usuario junto con la opción **-P** para especificar una contraseña.
- La opción de línea de **comandos -ssadd** para agregar la base de datos de estado de sesión.

De forma predeterminada, no\_puede utilizar la herramienta regsql.exe de Aspnet para instalar la base de datos de estado de sesión en un equipo que ejecute SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>La herramienta de registro\_del navegador ASP.NET - aspnet regbrowsers.exe

En ASP.NET versión 1.1, el archivo Machine.config contenía una sección denominada &lt;browserCaps&gt;. Esta sección contenía una serie de entradas XML que definieron las configuraciones para varios exploradores en función de una expresión regular. Para ASP.NET versión 2.0, una nueva . EL archivo BROWSER define los parámetros de un navegador determinado mediante entradas XML. Puede agregar información en un nuevo explorador agregando un nuevo archivo . BROWSER a la carpeta que se encuentra en %SystemRoot%-Microsoft.NET-Framework-versión-CONFIG-Browsers en su sistema.

Dado que una aplicación no lee un archivo .config cada vez que requiere información del explorador, puede crear un nuevo archivo . BROWSER y ejecute\_Aspnet regbrowsers.exe para agregar los cambios necesarios al ensamblado. Esto permite que el servidor acceda a la nueva información del navegador inmediatamente para que no tenga que apagar ninguna de sus aplicaciones para recoger la información. Una aplicación puede tener acceso a las capacidades del explorador a través de la propiedad Browser de la HttpRequest actual.

Las siguientes opciones están disponibles al ejecutar aspnet\_regbrowser.exe:

| **Opción** | **Descripción** |
| --- | --- |
| **-?** | Muestra el\_texto de la Ayuda de Aspnet regbbrowsers.exe en la ventana de comandos. |
| **-i** | Crea el ensamblado de capacidades del explorador en tiempo de ejecución y lo instala en la caché global de ensamblados. |
| **-u** | Desinstala el ensamblado de capacidades del explorador en tiempo de ejecución de la caché global de ensamblados. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>The ASP.NET Compilation Tool\_- aspnet compiler.exe

La herramienta de compilación ASP.NET se puede utilizar de dos maneras generales: para la compilación en el lugar y la compilación para la implementación, donde se especifica un directorio de salida de destino.

### <a name="compiling-an-application-in-place"></a>[Compilación de una solicitud en el lugar](https://msdn.microsoft.com/library/ms229863.aspx)

La herramienta de compilación ASP.NET puede compilar una aplicación en su lugar, es decir, imita el comportamiento de realizar varias solicitudes a la aplicación, lo que provoca una compilación regular. Los usuarios de un sitio precompilado no experimentarán un retraso causado por la compilación de la página en la primera solicitud.

Al precompilar un sitio en su lugar, se aplican los siguientes elementos:

- El sitio conserva sus archivos y la estructura de directorios.
- Debe tener compiladores para todos los lenguajes de programación utilizados por el sitio en el servidor.
- Si se produce un error en la compilación de algún archivo, se produce un error en la compilación de todo el sitio.

También puede volver a compilar una aplicación en su lugar después de agregar nuevos archivos de origen a ella. La herramienta compila solo los archivos nuevos o modificados a menos que incluya la opción **-c.**

> [!NOTE]
> La compilación de una aplicación que contiene una aplicación anidada no compila la aplicación anidada. La aplicación anidada debe compilarse por separado.

### <a name="compiling-an-application-for-deployment"></a>[Compilación de una aplicación para la implementación](https://msdn.microsoft.com/library/ms229863.aspx)

Compile una aplicación para la implementación (compilación en una ubicación de destino) especificando el parámetro targetDir. El targetDir puede ser la ubicación final de la aplicación web o la aplicación compilada se puede implementar más. El uso de la opción **-u** compila la aplicación de tal manera que puede realizar cambios en determinados archivos de la aplicación compilada sin volver a compilarla. Aspnet\_compiler.exe hace una distinción entre los tipos de archivo estáticos y dinámicos y los controla de forma diferente al crear la aplicación resultante.

- Los tipos de archivo estáticos son aquellos que no tienen un compilador o proveedor de compilación asociado, como los archivos cuyo nombre tiene extensiones como .css, .gif, .htm, .html, .jpg, .js, etc. Estos archivos simplemente se copian en la ubicación de destino, con sus lugares relativos en la estructura de directorios conservados.
- Los tipos de archivo dinámicos son aquellos que tienen un compilador o proveedor de compilación asociado, incluidos los archivos con extensiones de nombre de archivo específicas de ASP.NET, como .asax, .ascx, .ashx, .aspx, .browser, .master, etc. La herramienta ASP.NET compilación genera ensamblados a partir de estos archivos. Si se omite la opción **-u,** la herramienta también crea archivos con la extensión de nombre de archivo . COMPILED que asignan los archivos de origen originales a su ensamblado. Para asegurarse de que se conserva la estructura de directorios del origen de la aplicación, la herramienta genera archivos de marcador de posición en las ubicaciones correspondientes de la aplicación de destino.

Debe utilizar la opción **-u** para indicar que se puede modificar el contenido de la aplicación compilada. De lo contrario, las modificaciones posteriores se omiten o provocan errores en tiempo de ejecución.

En la tabla siguiente se describe cómo la herramienta de compilación de ASP.NET controla diferentes tipos de archivo cuando se incluye la opción **-u.**

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .ascx, .aspx, .master | Estos archivos se dividen en código fuente y de marcado, que incluye &lt;tanto archivos de código&gt; subyacente como cualquier código que se incluye en elementos runat de script runat-"server". El código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash y los ensamblados se colocan en el directorio Bin. Cualquier código en línea, es decir, ** &lt; ** ** % ** código entre corchetes, se incluye con el marcado y no se compila. Los nuevos archivos con el mismo nombre que los archivos de origen se crean para contener el marcado y se colocan en los directorios de salida correspondientes. |
| .ashx, .asmx | Estos archivos no se compilan y se mueven a los directorios de salida tal cual y no se compilan. Si desea compilar el código del controlador, coloque el código\_en los archivos de código fuente en el directorio Código de aplicación. |
| .cs, .vb, .jsl, .cpp (sin incluir archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Estos archivos se compilan e incluyen como un recurso en los ensamblados que hacen referencia a ellos. Los archivos de origen no se copian en el directorio de salida. Si no se hace referencia a un archivo de código, no se compila. |
| Tipos de archivos personalizados | Estos archivos no se compilan. Estos archivos se copian en los directorios de salida correspondientes. |
| Archivos de código\_fuente en el subdirectorio App Code | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. |
| Archivos .resx y .resource\_en el subdirectorio App GlobalResources | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. No\_se crea ningún subdirectorio App GlobalResources en el directorio de salida principal y no se copia ningún archivo .resx o .resources ubicado en el directorio de origen en los directorios de salida. |
| Archivos .resx y .resource\_en el subdirectorio App LocalResources | Estos archivos no se compilan y se copian en los directorios de salida correspondientes. |
| .skin en el\_subdirectorio App Themes | Los archivos .skin y los archivos de tema estáticos no se compilan y se copian en los directorios de salida correspondientes. |
| .browser Web.config Tipos de archivo estáticos Ensamblados ya presentes en el directorio Bin | Estos archivos se copian tal como está en los directorios de salida. |

En la tabla siguiente se describe cómo la herramienta de compilación de ASP.NET controla diferentes tipos de archivo cuando se omite la opción **-u.**

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Estos archivos se dividen en código fuente y de marcado, que incluye &lt;tanto archivos de código&gt; subyacente como cualquier código que se incluye en elementos runat de script runat-"server". El código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash. Los ensamblados resultantes se colocan en el directorio Bin. Cualquier código en línea, es decir, ** &lt; ** ** % ** código entre corchetes, se incluye con el marcado y no se compila. El compilador crea nuevos archivos para contener el marcado con el mismo nombre que los archivos de origen. Estos archivos resultantes se colocan en el directorio Bin. El compilador también crea archivos con el mismo nombre que los archivos de origen pero con la extensión . COMPILED que contienen información de asignación. el. Los archivos COMPILED se colocan en los directorios de salida correspondientes a la ubicación original de los archivos de origen. |
| .ascx | Estos archivos se dividen en código fuente y de marcado. El código fuente se compila en ensamblados y se coloca en el directorio Bin, con nombres que se derivan de un algoritmo hash. No se generan archivos de marcado. |
| .cs, .vb, .jsl, .cpp (sin incluir archivos de código subyacente para los tipos de archivo enumerados anteriormente) | El código fuente al que hacen referencia los ensamblados generados a partir de archivos .ascx, .ashx o .aspx se compila en ensamblados y se coloca en el directorio Bin. No se copia ningún archivo de origen. |
| Tipos de archivos personalizados | Estos archivos se compilan como archivos dinámicos. Dependiendo del tipo de archivo en el que se basan, el compilador puede colocar archivos de asignación en los directorios de salida. |
| Archivos en\_el subdirectorio Código de aplicación | Los archivos de código fuente de este subdirectorio se compilan en ensamblados y se colocan en el directorio Bin. |
| Archivos en\_el subdirectorio App GlobalResources | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. No\_se crea ningún subdirectorio App GlobalResources en el directorio de salida principal. Si el archivo de configuración especifica appliesTo-"All", los archivos .resx y .resources se copian en los directorios de salida. No se copian si un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)hace referencia a ellos. |
| Archivos .resx y .resource\_en el subdirectorio App LocalResources | Estos archivos se compilan en ensamblados con nombres únicos y se colocan en el directorio Bin. No se copia ningún archivo .resx o .resource en los directorios de salida. |
| .skin en el\_subdirectorio App Themes | Los temas se compilan en ensamblados y se colocan en el directorio Bin. Los archivos Stub se crean para los archivos .skin y se colocan en el directorio de salida correspondiente. Los archivos estáticos (como .css) se copian en los directorios de salida. |
| .browser Web.config Tipos de archivo estáticos Ensamblados ya presentes en el directorio Bin | Estos archivos se copian tal como está en el directorio de salida. |

### <a name="fixed-assembly-names"></a>[Nombres de asambleas fijas](https://msdn.microsoft.com/library/ms229863.aspx##)

Algunos escenarios, como la implementación de una aplicación web mediante MSI Windows Installer, requieren el uso de nombres de archivo y contenido coherentes, así como estructuras de directorio coherentes para identificar ensamblados o valores de configuración para las actualizaciones. En esos casos, puede usar la opción **-fixednames** para especificar que la herramienta ASP.NET compilación debe compilar un ensamblado para cada archivo de origen en lugar de usar el lugar donde se compilan varias páginas en ensamblados. Esto puede dar lugar a un gran número de ensamblados, por lo que si le preocupa la escalabilidad, debe usar esta opción con precaución.

### <a name="strong-name-compilation"></a>[Compilación de nombre fuerte](https://msdn.microsoft.com/library/ms229863.aspx##)

Las opciones **-aptca**, **-delaysign**, **-keycontainer** y **-keyfile** \_se proporcionan para que pueda usar Aspnet compiler.exe para crear ensamblados con nombre seguro sin usar la herramienta de nombre seguro [(Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) por separado. Estas opciones corresponden, respectivamente, a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**y **AssemblyKeyFileAttribute**.

La discusión de estos atributos está fuera del alcance de este curso.

## <a name="labs"></a>Laboratorios

Cada uno de los siguientes laboratorios se basa en los laboratorios anteriores. Tendrás que hacerlos en orden.

## <a name="lab-1-using-the-configuration-api"></a>Lab 1: Uso de la API de configuración

1. Cree un nuevo sitio Web llamado *mod9lab*.
2. Agregue un nuevo archivo de configuración web al sitio.
3. Agregue lo siguiente al archivo web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Esto garantizará que tiene permiso para guardar los cambios en el archivo web.config.

1. Agregue un nuevo control Label a Default.aspx y cambie el identificador a **lblDebugStatus**.
2. Agregue un nuevo control Button a Default.aspx.
3. Cambie el identificador del control Button a **btnToggleDebug** y text to **Toggle Debug Status**.
4. Abra la vista de código para el archivo de código subyacente de Default.aspx y agregue una instrucción **using** para **System.Web.Configuration** de la siguiente manera:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Agregue dos variables privadas a\_la clase y un método Page Init como se muestra a continuación:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Agregue el siguiente\_código a Carga de página:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Guarde y examine default.aspx. Observe que el control Label muestra el estado de depuración actual.
2. Haga doble clic en el button control en el diseñador y agregue el código siguiente a la Click eventos para el Button control:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Guarde y examine default.aspx y haga clic en el botón.
2. Abra el archivo web.config después de hacer &lt;clic&gt; en cada botón y observe el atributo **debug** en la sección de compilación.

## <a name="lab-2-logging-application-restarts"></a>Lab 2: Reinicios de aplicaciones de registro

En este laboratorio, creará código que le permitirá alternar el registro de cierres de aplicaciones, inicios y recompilaciones en el Visor de eventos.

1. Agregue un DropDownList a default.aspx y cambie el identificador a ddlLogAppEvents.
2. Establezca la propiedad **AutoPostBack** para DropDownList en **true**.
3. Agregue tres elementos a la Items colección para el DropDownList. Haga el **texto** para el primer elemento *Seleccionar valor* y el valor -1. Haga que el **texto** y el **valor** del segundo elemento **sean verdaderos** y el **texto** y el **valor** del tercer elemento **sean false.**
4. Agregue una nueva etiqueta a default.aspx. Cambie el identificador a **lblLogAppEvents**.
5. Abra la vista de código subyacente para default.aspx y agregue una nueva declaración para una variable de tipo HealthMonitoringSection como se muestra a continuación:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Agregue el código siguiente al\_código existente en Page Init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Haga doble clic en DropDownList y agregue el código siguiente al evento SelectedIndexChanged:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Examine default.aspx.
2. Establezca la lista desplegable en **False**.
3. Borre el registro de la aplicación en el Visor de eventos.
4. Haga clic en el botón para cambiar el atributo Debug de la aplicación.
5. Actualice el registro de la aplicación en el Visor de eventos. 

    1. ¿Se registraron eventos?
    2. ¿Por qué o por qué no?
6. Establezca la lista desplegable en **True.**
7. Haga clic en el botón para alternar el atributo Debug de la aplicación.
8. Actualice el inicio de sesión de la aplicación en el Visor de eventos. 

    1. ¿Se registraron eventos?
    2. ¿Cuál fue la razón del cierre de la aplicación?
9. Experimente con activar y desactivar el registro y examine los cambios realizados en el archivo web.config.

## <a name="more-information"></a>Más información:

ASP.NET modelo de proveedor de 2.0 le permite crear sus propios proveedores no solo para la instrumentación de aplicaciones, sino también para muchos otros usos, como membresía, perfiles, etc. Para obtener información detallada sobre cómo escribir un proveedor personalizado para registrar eventos de aplicación en un archivo de texto, visite [este vínculo](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
