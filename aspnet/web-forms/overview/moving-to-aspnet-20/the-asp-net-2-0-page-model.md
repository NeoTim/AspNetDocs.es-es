---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modelo de página ASP.NET 2.0 ? Microsoft Docs
author: rick-anderson
description: En ASP.NET 1.x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente podría implementarse utilizando el Attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 6c2435a06d04209db21fb8e075f68ff0b7a9ef7e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542864"
---
# <a name="the-aspnet-20-page-model"></a>El modelo de página ASP.NET 2.0

por [Microsoft](https://github.com/microsoft)

> En ASP.NET 1.x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente se podría implementar mediante el atributo Src @Page o el atributo CodeBehind de la directiva. En ASP.NET 2.0, los desarrolladores todavía tienen la opción entre el código en línea y el código subyacente, pero ha habido mejoras significativas en el modelo de código subyacente.

En ASP.NET 1.x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente se podría implementar mediante el atributo Src @Page o el atributo CodeBehind de la directiva. En ASP.NET 2.0, los desarrolladores todavía tienen la opción entre el código en línea y el código subyacente, pero ha habido mejoras significativas en el modelo de código subyacente.

## <a name="improvements-in-the-code-behind-model"></a>Mejoras en el modelo de código subyacente

Con el fin de comprender completamente los cambios en el modelo de código subyacente en ASP.NET 2.0, lo mejor es revisar rápidamente el modelo tal como existía en ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>El modelo de código subyacente en ASP.NET 1.x

En ASP.NET 1.x, el modelo de código subyacente consistía en un archivo ASPX (el formulario WebForm) y un archivo de código subyacente que contenía código de programación. Los dos archivos se @Page conectaron mediante la directiva en el archivo ASPX. Cada control de la página ASPX tenía una declaración correspondiente en el archivo de código subyacente como una variable de instancia. El archivo de código subyacente también contenía código para el enlace de eventos y el código generado necesario para el diseñador de Visual Studio. Este modelo funcionó bastante bien, pero debido a que cada ASP.NET elemento de la página ASPX requería el código correspondiente en el archivo de código subyacente, no había una verdadera separación de código y contenido. Por ejemplo, si un diseñador agrega un nuevo control de servidor a un archivo ASPX fuera del IDE de Visual Studio, la aplicación se interrumpiría debido a la ausencia de una declaración para ese control en el archivo de código subyacente.

## <a name="the-code-behind-model-in-aspnet-20"></a>El modelo de código subyacente en ASP.NET 2.0

ASP.NET 2.0 mejora considerablemente este modelo. En ASP.NET 2.0, el código subyacente se implementa mediante las *nuevas clases parciales proporcionadas* en ASP.NET 2.0. La clase de código subyacente en ASP.NET 2.0 se define como una clase parcial, lo que significa que contiene solo una parte de la definición de clase. La parte restante de la definición de clase se genera dinámicamente por ASP.NET 2.0 mediante la página ASPX en tiempo de ejecución o cuando se precompila el sitio Web. El vínculo entre el archivo de código subyacente y la página ASPX todavía se establece mediante la directiva de página. Sin embargo, en lugar de un atributo CodeBehind o Src, ASP.NET 2.0 ahora usa el atributo CodeFile. El atributo Inherits también se utiliza para especificar el nombre de clase de la página.

Una directiva típica de la página podría tener este aspecto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definición de clase típica en un archivo de código subyacente ASP.NET 2.0 podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> Los únicos lenguajes administrados que admiten clases parciales son los únicos lenguajes administrados que admiten actualmente clases parciales. Por lo tanto, los desarrolladores que usan J no podrán usar el modelo de código subyacente en ASP.NET 2.0.

El nuevo modelo mejora el modelo de código subyacente porque los desarrolladores ahora tendrán archivos de código que contienen solo el código que han creado. También proporciona una verdadera separación de código y contenido porque no hay declaraciones de variables de instancia en el archivo de código subyacente.

> [!NOTE]
> Dado que la clase parcial de la página ASPX es donde se lleva a cabo el enlace de eventos, los desarrolladores de Visual Basic pueden realizar un ligero aumento del rendimiento mediante el uso de la Handles palabra clave en el código subyacente para enlazar eventos. No tiene ninguna palabra clave equivalente.

## <a name="new--page-directive-attributes"></a>Nuevos atributos de directiva de página

ASP.NET 2.0 agrega muchos atributos nuevos a la directiva de página. Los siguientes atributos son nuevos en ASP.NET 2.0.

## <a name="async"></a>Async

El atributo Async permite configurar la página para que se ejecute de forma asincrónica. Cubra bien las páginas asincrónicas más adelante en este módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Se ha especificado el tiempo de espera de las páginas asincrónicas. El valor predeterminado es 45 segundos.

## <a name="codefile"></a>CodeFile

El atributo CodeFile es el reemplazo del atributo CodeBehind en Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

El atributo CodeFileBaseClass se utiliza en los casos en los que desea que varias páginas deriven de una sola clase base. Debido a la implementación de clases parciales en ASP.NET, sin este atributo, una clase base que usa campos comunes compartidos para hacer referencia a controles declarados en una página ASPX no funcionaría correctamente porque el motor de compilación ASP.NET creará automáticamente nuevos miembros basados en controles de la página. Por lo tanto, si desea una clase base común para dos o más páginas en ASP.NET, deberá definir la clase base en el atributo CodeFileBaseClass y, a continuación, derivar cada clase de páginas de esa clase base. El atributo CodeFile también es necesario cuando se utiliza este atributo.

## <a name="compilationmode"></a>CompilationMode

Este atributo le permite establecer la propiedad CompilationMode de la página ASPX. La propiedad CompilationMode es una enumeración que contiene los valores **Always**, **Auto**y **Never**. El valor predeterminado es **Siempre**. El ajuste **Automático** impedirá que ASP.NET compilen dinámicamente la página si es posible. La exclusión de páginas de la compilación dinámica aumenta el rendimiento. Sin embargo, si una página que se excluye contiene ese código que se debe compilar, se producirá un error cuando se examine la página.

## <a name="enableeventvalidation"></a>EnableEventValidation

Este atributo especifica si se validan o no los eventos de devolución de datos y devolución de llamada. Cuando esto está habilitado, se comprueban los argumentos para devolver eventos o devolver eventos para asegurarse de que se originaron desde el control de servidor que los representó originalmente.

## <a name="enabletheming"></a>EnableTheming

Este atributo especifica si se utilizan o no ASP.NET temas en una página. El valor predeterminado es **false**. ASP.NET temas se tratan en [el Módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Este atributo especifica si se deben agregar pragmas de línea durante la compilación. Las pragmas de línea son opciones utilizadas por los depuradores para marcar secciones específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Este atributo especifica si JavaScript se inserta o no en la página para mantener la posición de desplazamiento entre las devueltas. Este atributo es **false** de forma predeterminada.

Cuando este atributo es **true** &lt;,&gt; ASP.NET agregará un bloque de script en la devolución de datos que tenga este aspecto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Tenga en cuenta que el src para este bloque de script es WebResource.axd. Este recurso no es una ruta de acceso física. Cuando se solicita este script, ASP.NET crea dinámicamente el script.

### <a name="masterpagefile"></a>MasterPageFile

Este atributo especifica el archivo de página maestra para la página actual. La ruta de acceso puede ser relativa o absoluta. Las páginas maestras se tratan en [el Módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Este atributo le permite invalidar las propiedades de apariencia de la interfaz de usuario definidas por un tema ASP.NET 2.0. Los temas se tratan en [el Módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Especifica el tema de la página. Si no se especifica un valor para el atributo StyleSheetTheme, el atributo Theme reemplaza todos los estilos aplicados a los controles de la página.

## <a name="title"></a>Título

Establece el título de la página. El valor especificado aquí aparecerá &lt;&gt; en el elemento title de la página representada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Establece el valor de la enumeración ViewStateEncryptionMode. Los valores disponibles son **Always**, **Auto**y **Never**. El valor predeterminado es **Automático**. Cuando este atributo se establece en un valor de **Auto**, viewstate se cifra es un control que lo solicita mediante una llamada a la **RegisterRequiresViewStateEncryption** método.

## <a name="setting-public-property-values-via-the--page-directive"></a>Establecer los valores de propiedad pública a través de la Directiva de página

Otra nueva capacidad de la directiva de página en ASP.NET 2.0 es la capacidad de establecer el valor inicial de las propiedades públicas de una clase base. Supongamos, por ejemplo, que tiene una propiedad pública denominada **SomeText** en la clase base y desea que se inicialice en **Hello** cuando se cargue una página. Esto se puede lograr simplemente estableciendo el valor en la directiva de la página de la siguiente manera:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

El atributo **SomeText** de la directiva de página establece el valor inicial de la propiedad SomeText de la clase base en *Hello!*. El siguiente vídeo es un tutorial de establecer el valor inicial de una propiedad pública en una clase base mediante la directiva de página.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Abrir vídeo a pantalla completa](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nuevas propiedades públicas de la clase Page

Las siguientes propiedades públicas son nuevas en ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Devuelve la ruta de acceso relativa a la aplicación a la página o control. Por ejemplo, para una http://app/folder/page.aspxpágina ubicada en , la propiedad devuelve /folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Devuelve la ruta de acceso del directorio virtual relativo a la página o control. Por ejemplo, para http://app/folder/page.aspxuna página ubicada en , la propiedad devuelve ./folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtiene o establece el tiempo de espera utilizado para el control de páginas asincrónica. (Las páginas asincrónicas se tratarán más adelante en este módulo.)

## <a name="clientquerystring"></a>ClientQueryString

Propiedad de solo lectura que devuelve la parte de la cadena de consulta de la dirección URL solicitada. Este valor está codificado en URL. Puede usar el método UrlDecode de la clase HttpServerUtility para descodificarlo.

## <a name="clientscript"></a>ClientScript

Esta propiedad devuelve un clientScriptManager objeto que se puede utilizar para administrar ASP.NET emisión de script del lado cliente. (La clase ClientScriptManager se trata más adelante en este módulo.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Esta propiedad controla si la validación de eventos está habilitada para eventos de devolución de datos y devolución de llamada. Cuando está habilitado, los argumentos para los eventos de devolución de datos o devolución de llamada se comprueban para asegurarse de que se originaron desde el control de servidor que los representó originalmente.

## <a name="enabletheming"></a>EnableTheming

Esta propiedad obtiene o establece un valor booleano que especifica si un tema de ASP.NET 2.0 se aplica a la página.

## <a name="form"></a>Form

Esta propiedad devuelve el formulario HTML de la página ASPX como un objeto HtmlForm.

## <a name="header"></a>Encabezado

Esta propiedad devuelve una referencia a un HtmlHead objeto que contiene el encabezado de página. Puede utilizar el objeto HtmlHead devuelto para obtener/establecer hojas de estilo, etiquetas Meta, etc.

## <a name="idseparator"></a>IdSeparator

Esta propiedad de solo lectura obtiene el carácter que se usa para separar los identificadores de control cuando ASP.NET está creando un identificador único para los controles de una página. No debe usarse directamente desde el código.

## <a name="isasync"></a>IsAsync

Esta propiedad permite páginas asincrónicas. Las páginas asincrónicas se describen más adelante en este módulo.

## <a name="iscallback"></a>IsCallback

Esta propiedad de solo lectura devuelve **true** si la página es el resultado de una devolución de llamada. Las devueltas de llamada se discuten más adelante en este módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Esta propiedad de solo lectura devuelve **true** si la página forma parte de una devolución de datos entre páginas. Las devueltas entre páginas se tratan más adelante en este módulo.

## <a name="items"></a>Elementos

Devuelve una referencia a una instancia de IDictionary que contiene todos los objetos almacenados en el contexto de páginas. Puede agregar elementos a este objeto IDictionary y estarán disponibles durante toda la duración del contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Esta propiedad controla si ASP.NET emite JavaScript que mantiene la posición de desplazamiento de las páginas en el explorador después de que se produce una devolución de datos. (Los detalles de esta propiedad se discutieron anteriormente en este módulo.)

## <a name="master"></a>Master

Esta propiedad de solo lectura devuelve una referencia a la instancia de MasterPage para una página a la que se ha aplicado una página maestra.

## <a name="masterpagefile"></a>MasterPageFile

Obtiene o establece el nombre de archivo de la página maestra para la página. Esta propiedad solo se puede establecer en el método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propiedad obtiene o establece la longitud máxima para el estado de las páginas en bytes. Si la propiedad se establece en un número positivo, el estado de vista de páginas se dividirá en varios campos ocultos para que no supere el número de bytes especificado. Si la propiedad es un número negativo, el estado de vista no se dividirá en fragmentos.

## <a name="pageadapter"></a>PageAdapter

Devuelve una referencia al objeto PageAdapter que modifica la página para el explorador solicitante.

## <a name="previouspage"></a>PreviousPage

Devuelve una referencia a la página anterior en casos de un Server.Transfer o una devolución de datos entre páginas.

## <a name="skinid"></a>SkinID

Especifica el aspecto ASP.NET 2.0 que se aplicará a la página.

## <a name="stylesheettheme"></a>StyleSheetTheme

Esta propiedad obtiene o establece la hoja de estilos que se aplica a una página.

## <a name="templatecontrol"></a>TemplateControl

Devuelve una referencia al control contenedor de la página.

## <a name="theme"></a>Tema

Obtiene o establece el nombre del tema ASP.NET 2.0 aplicado a la página. Este valor debe establecerse antes de la PreInit método.

## <a name="title"></a>Título

Esta propiedad obtiene o establece el título de la página tal como se obtiene del encabezado de páginas.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtiene o establece el ViewStateEncryptionMode de la página. Vea una explicación detallada de esta propiedad anteriormente en este módulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuevas propiedades protegidas de la clase Page

A continuación se muestran las nuevas propiedades protegidas de la clase Page en ASP.NET 2.0.

## <a name="adapter"></a>Adapter (Adaptador)

Devuelve una referencia a la ControlAdapter que representa la página en el dispositivo que lo solicitó.

## <a name="asyncmode"></a>AsyncMode

Esta propiedad indica si la página se procesa de forma asincrónica. Está diseñado para su uso por el tiempo de ejecución y no directamente en el código.

## <a name="clientidseparator"></a>ClientIDSeparator

Esta propiedad devuelve el carácter utilizado como separador al crear identificadores de cliente únicos para los controles. Está diseñado para su uso por el tiempo de ejecución y no directamente en el código.

## <a name="pagestatepersister"></a>PageStatePersister

Esta propiedad devuelve el pageStatePersister objeto para la página. Esta propiedad se utiliza principalmente por ASP.NET desarrolladores de controles.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Esta propiedad devuelve un sufijo único que se anexa a la ruta de acceso del archivo para almacenar en caché los exploradores. El valor \_ \_predeterminado es ufps y un número de 6 dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Nuevos métodos públicos para la clase Page

Los siguientes métodos públicos son nuevos en la clase Page en ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Este método registra delegados de controlador de eventos para la ejecución asincrónica de páginas. Las páginas asincrónicas se describen más adelante en este módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Aplica las propiedades de una hoja de estilos de páginas a la página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Este método es una tarea asincrónica.

### <a name="getvalidators"></a>GetValidators

Devuelve una colección de validadores para el grupo de validación especificado o el grupo de validación predeterminado si no se especifica ninguno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Este método registra una nueva tarea asincrónica. Las páginas asincrónicas se tratan más adelante en este módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Este método indica a ASP.NET que el estado del control de páginas debe conservarse.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Este método indica a ASP.NET que el estado de vista de páginas requiere cifrado.

## <a name="resolveclienturl"></a>ResolveClientUrl

Devuelve una dirección URL relativa que se puede utilizar para las solicitudes de cliente de imágenes, etc.

## <a name="setfocus"></a>SetFocus

Este método establecerá el foco en el control que se especifica cuando se carga inicialmente la página.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Este método anulará el registro del control que se le pasa ya que ya no requiere persistencia de estado de control.

## <a name="changes-to-the-page-lifecycle"></a>Cambios en el ciclo de vida de la página

El ciclo de vida de la página en ASP.NET 2.0 no ha cambiado drásticamente, pero hay algunos métodos nuevos que debe tener en cuenta. El ciclo de vida de la página ASP.NET 2.0 se describe a continuación.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Nuevo en ASP.NET 2.0)

El evento PreInit es la etapa más temprana del ciclo de vida a la que un desarrollador puede acceder. La adición de este evento permite cambiar mediante programación ASP.NET temas 2.0, páginas maestras, propiedades de acceso para un perfil de ASP.NET 2.0, etc. Si se encuentra en un estado de devolución de datos, es importante tener en cuenta que Viewstate aún no se ha aplicado a los controles en este punto del ciclo de vida. Por lo tanto, si un desarrollador cambia una propiedad de un control en esta etapa, es probable que se sobrescriba más adelante en el ciclo de vida de las páginas.

## <a name="init"></a>Init

El evento Init no ha cambiado de ASP.NET 1.x. Aquí es donde desea leer o inicializar las propiedades de los controles en la página. En esta etapa, las páginas maestras, temas, etc. ya se aplican a la página.

## <a name="initcomplete-new-in-20"></a>InitComplete (Nuevo en 2.0)

Se llama al evento InitComplete al final de la fase de inicialización de las páginas. En este punto del ciclo de vida, puede tener acceso a los controles de la página, pero su estado aún no se ha rellenado.

## <a name="preload-new-in-20"></a>Precarga (nuevo en 2.0)

Este evento se llama después de que se\_hayan aplicado todos los datos de devolución de datos y justo antes de la carga de página.

## <a name="load"></a>Cargar

El evento Load no ha cambiado de ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nuevo en 2.0)

El evento LoadComplete es el último evento de la fase de carga de páginas. En esta etapa, todos los datos de devolución de datos y estado de vista se han aplicado a la página.

## <a name="prerender"></a>PreRender

Si desea que viewstate se mantenga correctamente para los controles que se agregan a la página dinámicamente, el evento PreRender es la última oportunidad para agregarlos.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nuevo en 2.0)

En la fase PreRenderComplete, todos los controles se han agregado a la página y la página está lista para representarse. El evento PreRenderComplete es el último evento generado antes de guardar el estado de vista de páginas.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Nuevo en 2.0)

Se llama al evento SaveStateComplete inmediatamente después de guardar todo el estado de vista de página y el estado de control. Este es el último evento antes de que la página se represente realmente en el explorador.

## <a name="render"></a>Representación

El render método no ha cambiado desde ASP.NET 1.x. Aquí es donde el HtmlTextWriter se inicializa y la página se representa en el explorador.

## <a name="cross-page-postback-in-aspnet-20"></a>Devolución de correos entre páginas en ASP.NET 2.0

En ASP.NET 1.x, las devueltas debían publicarse en la misma página. No se permitían devueltas entre páginas. ASP.NET 2.0 agrega la capacidad de publicar de nuevo a una página diferente a través de la Interfaz IButtonControl. Cualquier control que implemente la nueva interfaz IButtonControl (Button, LinkButton e ImageButton además de controles personalizados de terceros) puede aprovechar esta nueva funcionalidad mediante el uso del atributo PostBackUrl. El código siguiente muestra un Button control que devuelve a una segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Cuando se devuelve la página, la página que inicia la devolución de datos es accesible a través de la PreviousPage propiedad en la segunda página. Esta funcionalidad se implementa a través de la nueva función del lado cliente de WebForm\_DoPostBackWithOptions que ASP.NET 2.0 se representa en la página cuando un control vuelve a una página diferente. Esta función de JavaScript es proporcionada por el nuevo webResource.axd controlador que emite script al cliente.

El siguiente vídeo es un tutorial de una devolución de datos entre páginas.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Abrir vídeo a pantalla completa](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Más detalles sobre las devueltas entre páginas

### <a name="viewstate"></a>Viewstate

Es posible que ya se haya preguntado sobre lo que sucede con el estado de vista desde la primera página en un escenario de devolución de datos entre páginas. Después de todo, cualquier control que no implemente IPostBackDataHandler conservará su estado a través de viewstate, por lo que para tener acceso a las propiedades de ese control en la segunda página de una devolución de datos entre páginas, debe tener acceso al estado de vista de la página. ASP.NET 2.0 se encarga de este escenario mediante \_ \_un nuevo campo oculto en la segunda página denominado PREVIOUSPAGE. El \_ \_campo de formulario PREVIOUSPAGE contiene el estado de vista de la primera página para que pueda tener acceso a las propiedades de todos los controles de la segunda página.

### <a name="circumventing-findcontrol"></a>Eludir FindControl

En el tutorial de vídeo de una devolución de datos entre páginas, usé el FindControl método para obtener una referencia a la TextBox control en la primera página. Ese método funciona bien para ese propósito, pero FindControl es caro y requiere escribir código adicional. Afortunadamente, ASP.NET 2.0 proporciona una alternativa a FindControl para este propósito que funcionará en muchos escenarios. La directiva PreviousPageType permite tener una referencia fuertemente tipada a la página anterior mediante el atributo TypeName o VirtualPath. El atributo TypeName permite especificar el tipo de la página anterior, mientras que el atributo VirtualPath permite hacer referencia a la página anterior mediante una ruta de acceso virtual. Después de establecer la directiva PreviousPageType, debe exponer los controles, etc. a los que desea permitir el acceso mediante propiedades públicas.

## <a name="lab-1-cross-page-postback"></a>Lab 1 Devolución de Correos entre páginas

En este laboratorio, creará una aplicación que usa la nueva funcionalidad de devolución de datos entre páginas de ASP.NET 2.0.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web ASP.NET.
2. Agregue un nuevo formulario Webform denominado page2.aspx.
3. Abra el Default.aspx en la vista Diseño y agregue un Button control y un TextBox control. 

    1. Asigne al control Button un identificador de **SubmitButton** y el control TextBox un identificador de **UserName**.
    2. Establezca la propiedad PostBackUrl de Button en page2.aspx.
4. Abra page2.aspx en la vista Origen.
5. Agregue una directiva de Tipo de Página Anterior como se muestra a continuación:
6. Agregue el código siguiente\_a la carga de página de page2.aspx código subyacente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compile el proyecto haciendo clic en Compilar en el menú Generar.
8. Agregue el código siguiente al código subyacente de Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Cambie la\_carga de página en page2.aspx a lo siguiente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile el proyecto.
11. Ejecute el proyecto.
12. Introduzca su nombre en el cuadro de texto y haga clic en el botón.
13. ¿Cuál es el resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas asincrónicas en ASP.NET 2.0

Muchos problemas de contención en ASP.NET se deben a la latencia de llamadas externas (como llamadas a servicios web o bases de datos), latencia de E/S de archivos, etc. Cuando se realiza una solicitud en una aplicación ASP.NET, ASP.NET usa uno de sus subprocesos de trabajo para atender esa solicitud. Esa solicitud es propietaria de ese subproceso hasta que se completa la solicitud y se ha enviado la respuesta. ASP.NET 2.0 busca resolver problemas de latencia con estos tipos de problemas agregando la capacidad de ejecutar páginas de forma asincrónica. Esto significa que un subproceso de trabajo puede iniciar la solicitud y, a continuación, entregar la ejecución adicional a otro subproceso, volviendo así al grupo de subprocesos disponible rápidamente. Cuando se ha completado la E/S del archivo, la llamada a la base de datos, etc., se obtiene un nuevo subproceso del grupo de subprocesos para finalizar la solicitud.

El primer paso para hacer que una página se ejecute de forma asincrónica es establecer el atributo **Async** de la directiva de página de la siguiente manera:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Este atributo indica a ASP.NET implementar el IHttpAsyncHandler para la página.

El siguiente paso es llamar al método AddOnPreRenderCompleteAsync en un punto del ciclo de vida de la página anterior a PreRender. (Este método se llama\_normalmente en carga de página.) El método AddOnPreRenderCompleteAsync toma dos parámetros; un BeginEventHandler y un EndEventHandler. El BeginEventHandler devuelve un IAsyncResult que, a continuación, se pasa como un parámetro a la EndEventHandler.

El siguiente vídeo es un tutorial de una solicitud de página asincrónica.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Abrir vídeo a pantalla completa](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Una página asincrónica no se representa en el explorador hasta que EndEventHandler se ha completado. Sin duda, pero algunos desarrolladores pensarán que las solicitudes asincrónicas son similares a las devoluciones de llamada asincrónicas. Es importante darse cuenta de que no lo son. La ventaja de las solicitudes asincrónicas es que el primer subproceso de trabajo se puede devolver al grupo de subprocesos para atender nuevas solicitudes, lo que reduce la contención debido a que está enlazado a E/S, etc.

## <a name="script-callbacks-in-aspnet-20"></a>Devoluciones de llamada de script en ASP.NET 2.0

Los desarrolladores web siempre han buscado maneras de evitar el parpadeo asociado a una devolución de llamada. En ASP.NET 1.x, SmartNavigation era el método más común para evitar el parpadeo, pero SmartNavigation causó problemas para algunos desarrolladores debido a la complejidad de su implementación en el cliente. ASP.NET 2.0 soluciona este problema con las devoluciones de llamada de script. Las devoluciones de llamada de script utilizan XMLHttp para realizar solicitudes en el servidor Web a través de JavaScript. La solicitud XMLHttp devuelve datos XML que, a continuación, se pueden manipular a través del DOM del explorador. El nuevo controlador WebResource.axd oculta al usuario el código XMLHttp.

Hay varios pasos que son necesarios para configurar una devolución de llamada del script en ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Paso 1: Implementar la interfaz ICallbackEventHandler

Para que ASP.NET reconozca la página como participante en una devolución de llamada de script, debe implementar la interfaz ICallbackEventHandler. Puede hacer esto en el archivo de código subyacente de la siguiente manera:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

También puede hacer esto mediante la directiva implementos de la siguiente manera:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente, se usaría la directiva implementos cuando se usa código de ASP.NET en línea.

## <a name="step-2--call-getcallbackeventreference"></a>Paso 2 : Llame a GetCallbackEventReference

Como se mencionó anteriormente, la llamada XMLHttp se encapsula en el WebResource.axd controlador. Cuando se representa la página, ASP.NET agregará\_una llamada a WebForm DoCallback, un script de cliente proporcionado por WebResource.axd. La función WebForm\_DoCallback reemplaza la \_ \_función doPostBack para una devolución de llamada. Recuerde \_ \_que doPostBack envía el formulario mediante programación en la página. En un escenario de devolución de llamada, \_ \_desea evitar una devolución de datos, por lo que doPostBack no será suficiente.

> [!NOTE]
> \_\_doPostBack todavía se representa en la página en un escenario de devolución de llamada de script de cliente. Sin embargo, no se utiliza para la devolución de llamada.

Los argumentos para\_la función del lado cliente DeCallback de WebForm se proporcionan a\_través de la función del lado servidor GetCallbackEventReference que normalmente se llamaría en carga de página. Una llamada típica a GetCallbackEventReference podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> En este caso, cm es una instancia de ClientScriptManager. La clase ClientScriptManager se tratará más adelante en este módulo.

Hay varias versiones sobrecargadas de GetCallbackEventReference. En este caso, los argumentos son los siguientes:

`this`

Una referencia al control donde se llama a GetCallbackEventReference. En este caso, es la propia página.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argumento de cadena que se pasará del código del lado cliente al evento del lado servidor. En este caso, Im pasar el valor de una lista desplegable denominada ddlCompany.

`ShowCompanyName`

El nombre de la función del lado cliente que aceptará el valor devuelto (como cadena) del evento de devolución de llamada del lado servidor. Esta función solo se llamará cuando la devolución de llamada del lado del servidor se realice correctamente. Por lo tanto, en aras de la robustez, generalmente se recomienda utilizar la versión sobrecargada de GetCallbackEventReference que toma un argumento de cadena adicional que especifica el nombre de una función del lado cliente para ejecutaren en caso de error.

`null`

Cadena que representa una función del lado cliente que inició antes de la devolución de llamada al servidor. En este caso, no existe tal script, por lo que el argumento es null.

`true`

Un valor booleano que especifica si se debe llevar a cabo la devolución de llamada de forma asincrónica.

La llamada a\_WebForm DoCallback en el cliente pasará estos argumentos. Por lo tanto, cuando esta página se representa en el cliente, ese código tendrá ese aspecto:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Observe que la firma de la función en el cliente es un poco diferente. La función del lado cliente pasa 5 cadenas y un valor booleano. La cadena adicional (que es null en el ejemplo anterior) contiene la función del lado cliente que controlará los errores de la devolución de llamada del lado servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Paso 3 : Enganche el evento de control del lado del cliente

Observe que el valor devuelto de GetCallbackEventReference anterior se asignó a una variable de cadena. Esa cadena se utiliza para enlazar un evento del lado cliente para el control que inicia la devolución de llamada. En este ejemplo, la devolución de llamada se inicia mediante una lista desplegable en la página, por lo que quiero enlazar el evento *OnChange.*

Para enlazar el evento del lado cliente, simplemente agregue un controlador al marcado del lado cliente de la siguiente manera:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Recuerde que *cbRef* es el valor devuelto de la llamada a GetCallbackEventReference. Contiene la llamada a\_WebForm DoCallback que se mostró anteriormente.

## <a name="step-4--register-the-client-side-script"></a>Paso 4: Registre el script del lado cliente

Recuerde que la llamada a GetCallbackEventReference especificó que se ejecutaría un script del lado cliente denominado **ShowCompanyName** cuando la devolución de llamada del lado servidor se realizara correctamente. Ese script debe agregarse a la página mediante una instancia de ClientScriptManager. (La clase ClientScriptManager se tratará más adelante en este módulo.) Lo haces así:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Paso 5 : Llame a los métodos de la interfaz ICallbackEventHandler

El ICallbackEventHandler contiene dos métodos que debe implementar en el código. Son **RaiseCallbackEvent** y **GetCallbackEvent**.

**RaiseCallbackEvent** toma una cadena como argumento y no devuelve nada. El argumento de cadena se pasa de\_la llamada del lado cliente a WebForm DoCallback. En este caso, ese valor es el atributo *value* de la lista desplegable denominado ddlCompany. El código del lado servidor debe colocarse en el método RaiseCallbackEvent. Por ejemplo, si la devolución de llamada realiza una WebRequest en un recurso externo, ese código debe colocarse en RaiseCallbackEvent.

**GetCallbackEvent** es responsable de procesar la devolución de la devolución de llamada al cliente. No toma ningún argumento y devuelve una cadena. La cadena que devuelve se pasará como argumento a la función del lado cliente, en este caso *ShowCompanyName*.

Una vez que haya completado los pasos anteriores, está listo para realizar una devolución de llamada de script en ASP.NET 2.0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Abrir vídeo a pantalla completa](the-asp-net-2-0-page-model/_static/callback1.wmv)

Las devoluciones de llamada de script en ASP.NET se admiten en cualquier explorador que admita la realización de llamadas XMLHttp. Eso incluye todos los navegadores modernos en uso hoy en día. Internet Explorer utiliza el objeto ActiveX XMLHttp, mientras que otros exploradores modernos (incluido el próximo IE 7) utilizan un objeto XMLHttp intrínseco. Para determinar mediante programación si un explorador admite devoluciones de llamada, puede usar el **Request.Browser.SupportCallback** propiedad. Esta propiedad devolverá **true** si el cliente solicitante admite devoluciones de llamada de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabajar con el script de cliente en ASP.NET 2.0

Los scripts de cliente en ASP.NET 2.0 se administran mediante el uso de la clase ClientScriptManager. La clase ClientScriptManager realiza un seguimiento de los scripts de cliente mediante un tipo y un nombre. Esto evita que el mismo script se inserte mediante programación en una página más de una vez.

> [!NOTE]
> Después de que un script se haya registrado correctamente en una página, cualquier intento posterior de registrar el mismo script simplemente dará como resultado que el script no se registre una segunda vez. No se agregan scripts duplicados y no se produce ninguna excepción. Para evitar cálculos innecesarios, hay métodos que puede usar para determinar si un script ya está registrado para que no intente registrarlo más de una vez.

Los métodos de ClientScriptManager deben ser familiares para todos los desarrolladores de ASP.NET actuales:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Este método agrega un script a la parte superior de la página representada. Esto es útil para agregar funciones que se llamarán explícitamente en el cliente.

Hay dos versiones sobrecargadas de este método. Tres de cuatro argumentos son comunes entre ellos. Son las siguientes:

`type (string)`

El argumento ***type*** identifica un tipo para el script. Por lo general, es una buena idea usar el tipo de página (esto. GetType()) para el tipo.

`key (string)`

El argumento ***key*** es una clave definida por el usuario para el script. Esto debe ser único para cada script. Si intenta agregar un script con la misma clave y tipo de un script ya agregado, no se agregará.

`script (string)`

El argumento ***script*** es una cadena que contiene el script real que se va a agregar. Se recomienda utilizar stringBuilder para crear el script y, a continuación, utilizar el método ToString() en StringBuilder para asignar el argumento ***de script.***

Si utiliza el RegisterClientScriptBlock sobrecargado que solo toma tres argumentos, &lt;debe&gt;incluir elementos de script (&lt;script&gt; y /script ) en el script.

Puede optar por usar la sobrecarga de RegisterClientScriptBlock que toma un cuarto argumento. El cuarto argumento es un valor booleano que especifica si ASP.NET debe agregar elementos de script automáticamente. Si este argumento es **true**, el script no debe incluir explícitamente los elementos de script.

Utilice el método IsClientScriptBlockRegistered para determinar si ya se ha registrado un script. Esto le permite evitar un intento de volver a registrar un script que ya se ha registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nuevo en 2.0)

La etiqueta RegisterClientScriptInclude crea un bloque de script que se vincula a un archivo de script externo. Tiene dos sobrecargas. Uno toma una clave y una URL. El segundo agrega un tercer argumento que especifica el tipo.

Por ejemplo, el código siguiente genera un bloque de script que se vincula a jsfunctions.js en la raíz de la carpeta de scripts de la aplicación:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Este código genera el siguiente código en la página representada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> El bloque de script se representa en la parte inferior de la página.

Utilice el método IsClientScriptIncludeRegistered para determinar si ya se ha registrado un script. Esto le permite evitar un intento de volver a registrar un script.

## <a name="registerstartupscript"></a>RegisterStartupScript

El RegisterStartupScript método toma los mismos argumentos que el RegisterClientScriptBlock método. Un script registrado con RegisterStartupScript se ejecuta después de que se cargue la página, pero antes del evento del lado cliente OnLoad. En 1.X, los scripts registrados con RegisterStartupScript&gt; se colocaron justo antes de la etiqueta &lt;&gt; /form de cierre, &lt;mientras que los scripts registrados con RegisterClientScriptBlock se colocaron inmediatamente después de la etiqueta de formulario de apertura. En ASP.NET 2.0, ambos se &lt;colocan&gt; inmediatamente antes de la etiqueta /form de cierre.

> [!NOTE]
> Si registra una función con RegisterStartupScript, esa función no se ejecutará hasta que la llame explícitamente en código del lado cliente.

Utilice el método IsStartupScriptRegistered para determinar si ya se ha registrado un script y evitar un intento de volver a registrar un script.

## <a name="other-clientscriptmanager-methods"></a>Otros métodos ClientScriptManager

Estos son algunos de los otros métodos útiles de la clase ClientScriptManager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Consulte las devoluciones de llamada de script anteriormente en este módulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtiene una referencia de JavaScript (javascript:&lt;call&gt;) que se puede utilizar para volver a publicar desde un evento del lado cliente.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtiene una cadena que se puede usar para iniciar una publicación desde el cliente.                                    |
|      <strong>GetWebResourceUrl</strong>       | Devuelve una dirección URL a un recurso incrustado en un ensamblado. Debe utilizarse junto con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra un recurso web con la página. Estos son recursos incrustados en un ensamblado y controlados por el nuevo WebResource.axd controlador.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo de formulario oculto con la página.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra el código del lado cliente que se ejecuta cuando se envía el formulario HTML.                                   |
