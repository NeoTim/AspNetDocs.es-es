---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles enlazados a datos (Data Bound Controls) Microsoft Docs
author: rick-anderson
description: La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte fundamental de la interacción con...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 941e2ed15b3da28991e7b06cbab570eb1b5b8899
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543085"
---
# <a name="data-bound-controls"></a>Controles enlazados a datos

por [Microsoft](https://github.com/microsoft)

> La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte fundamental de la interacción con los datos en aplicaciones web dinámicas. ASP.NET 2.0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y sintaxis declarativa.

La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte fundamental de la interacción con los datos en aplicaciones web dinámicas. ASP.NET 2.0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y sintaxis declarativa.

El BaseDataBoundControl actúa como la clase base para el DataBoundControl clase y el HierarchicalDataBoundControl clase. En este módulo, discutiremos las siguientes clases que derivan de DataBoundControl:

- AdRotator
- Controles de lista
- GridView
- Formview
- Detailsview

También discutiremos las siguientes clases que derivan de la clase HierarchicalDataBoundControl:

- TreeView
- Menú
- SiteMapPath

## <a name="databoundcontrol-class"></a>Clase DataBoundControl

La clase DataBoundControl es una clase abstracta (marcada MustInherit en VB) utilizada para interactuar con datos tabulares o de estilo de lista. Los siguientes controles son algunos de los controles que derivan de DataBoundControl.

## <a name="adrotator"></a>AdRotator

El control AdRotator permite mostrar un banner gráfico en una página Web que está vinculada a una dirección URL específica. El gráfico que se muestra se gira mediante las propiedades del control. La frecuencia de un anuncio determinado que se muestra en una página se puede configurar mediante la propiedad Impresiones y los anuncios se pueden filtrar mediante el filtrado de **palabras** clave.

Los controles AdRotator utilizan un archivo XML o una tabla en una base de datos para los datos. Los siguientes atributos se utilizan en archivos XML para configurar el control AdRotator.

### <a name="imageurl"></a>ImageUrl
La dirección URL de una imagen que se va a mostrar para el anuncio.

### <a name="navigateurl"></a>NavigateUrl
La URL a la que se debe llevar el usuario cuando se hace clic en el anuncio. Esto debe estar codificado en URL.

### <a name="alternatetext"></a>AlternateText
Texto alternativo que se muestra en una información sobre herramientas y que los lectores de pantalla leen. También se muestra cuando la imagen especificada por ImageUrl no está disponible.

### <a name="keyword"></a>Palabra clave
Define una palabra clave que se puede utilizar al usar el filtrado de palabras clave. Si se especifica, solo se mostrarán los anuncios con una palabra clave que coincida con el filtro de palabras clave.

### <a name="impressions"></a>Impresiones
Un número de ponderación que determina la frecuencia con la que es probable que aparezca un anuncio en particular. Es relativo a la impresión de otros anuncios en el mismo archivo. El valor máximo de las impresiones colectivas para todos los anuncios de un archivo XML es 2.048.000.000 1.

### <a name="height"></a>Alto
La altura del anuncio en píxeles.

### <a name="width"></a>Ancho
El ancho del anuncio en píxeles.

> [!NOTE]
> Los atributos Height y Width reemplazan el alto y el ancho del propio control AdRotator.

Un archivo XML típico podría tener el siguiente aspecto:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

En el ejemplo anterior, es el doble de probable que aparezca el anuncio del sitio Web de ASP.NET debido al valor del atributo Impressions.

Para mostrar anuncios desde el archivo XML anterior, agregue un control AdRotator a una página y establezca la propiedad **AdvertisementFile** para que apunte al archivo XML como se muestra a continuación:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si decide utilizar una tabla de base de datos como origen de datos para el control AdRotator, primero deberá configurar una base de datos con el siguiente esquema:

| **Nombre de la columna** | **Tipo de datos** | **Descripción** |
| --- | --- | --- |
| id | int | Clave principal. Esta columna puede tener cualquier nombre. |
| ImageUrl | nvarchar(*longitud*) | La URL relativa o absoluta de la imagen que se va a mostrar para el anuncio. |
| NavigateUrl | nvarchar(*longitud*) | La URL de destino del anuncio. Si no proporciona un valor, el anuncio no es un hipervínculo. |
| AlternateText | nvarchar(*longitud*) | El texto que se muestra si no se encuentra la imagen. En algunos navegadores, el texto se muestra como información sobre herramientas. El texto alternativo también se utiliza para la accesibilidad para que los usuarios que no pueden ver el gráfico puedan escuchar su descripción leída en voz alta. |
| Palabra clave | nvarchar(*longitud*) | Una categoría para el anuncio en el que la página puede filtrar. |
| Impresiones | int(4) | Número que indica la probabilidad de que se muestre el anuncio. Cuanto mayor sea el número, más a menudo se mostrará el anuncio. El total de todos los valores de impresiones en el archivo XML no puede superar 2.048.000.000 - 1. |
| Ancho | int(4) | El ancho de la imagen en píxeles. |
| Alto | int(4) | La altura de la imagen en píxeles. |

En los casos en los que ya tiene una base de datos con un esquema diferente, puede usar las propiedades **AlternateTextField**, **ImageUrlField**y **NavigateUrlField** para asignar los atributos AdRotator a la base de datos existente. Para mostrar los datos de la base de datos en el control AdRotator, agregue un control de origen de datos a la página, configure la cadena de conexión para que el control de origen de datos apunte a la base de datos y establezca la propiedad **DataSourceID** del control AdRotator en el identificador del control de origen de datos. En los casos en los que tenga la necesidad de configurar anuncios de AdRotator mediante programación, utilice el evento AdCreated. El evento AdCreated toma dos parámetros; uno un objeto y el otro una instancia de AdCreatedEventArgs. AdCreatedEventArgs es una referencia al anuncio que se está creando.

En el siguiente fragmento de código se establece ImageUrl, NavigateUrl y AlternateText para un anuncio mediante programación:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Los controles de lista incluyen ListBox, DropDownList, CheckBoxList, RadioButtonList y BulletedList. Cada uno de estos controles puede estar enlazado a datos a un origen de datos. Utilizan un campo en el origen de datos como texto para mostrar y, opcionalmente, pueden usar un segundo campo como valor de un elemento. Los elementos también se pueden agregar estáticamente en tiempo de diseño y puede mezclar elementos estáticos y elementos dinámicos agregados desde un origen de datos.

Para enlazar datos a un control de lista, agregue un control de origen de datos a la página. Especifique un comando SELECT para el control de origen de datos y, a continuación, establezca la propiedad DataSourceID del control de lista en el identificador del control de origen de datos. Utilice las propiedades **DataTextField** y **DataValueField** para definir el texto para mostrar y el valor del control. Además, puede utilizar la propiedad **DataTextFormatString** para controlar la apariencia del texto para mostrar de la siguiente manera:

| **Expression** | **Descripción** |
| --- | --- |
| Precio:{0:C} | Para datos numéricos/decimales. Muestra el literal "Precio:" seguido de números en formato de moneda. El formato de moneda depende de la configuración de referencia cultural especificada en el atributo de referencia cultural en la directiva **Page** o en el archivo Web.config. |
| {0:D4} | Para datos enteros. No se puede utilizar con números decimales. Los enteros se muestran en un campo con acolchado cero que tiene cuatro caracteres de ancho. |
| {0:N2}% | Para datos numéricos. Muestra el número con una precisión de 2 decimales seguida del literal "%". |
| {0:000.0} | Para datos numéricos/decimales. Los números se redondean a un decimal. Los números de menos de tres dígitos se rellenan con ceros. |
| {0:D} | Para datos de fecha y hora. Muestra el formato de fecha larga ("jueves, 06 de agosto de 1996"). El formato de fecha depende de la configuración de la referencia cultural de la página o del archivo Web.config. |
| {0:d} | Para datos de fecha y hora. Muestra el formato de fecha corta ("12/31/99"). |
| {0:yy-MM-dd} | Para datos de fecha y hora. Muestra la fecha en formato numérico año-mes-día (96-08-06) |

## <a name="gridview"></a>GridView

El control GridView permite la visualización y edición de datos tabulares mediante un enfoque declarativo y es el sucesor del control DataGrid. Las siguientes características están disponibles en el control GridView.

- Enlace a controles de origen de datos, como SqlDataSource.
- Capacidades de clasificación integradas.
- Capacidades integradas de actualización y eliminación.
- Capacidades de paginación integradas.
- Capacidades de selección de filas integradas.
- Acceso mediante programación al modelo de objetos GridView para establecer propiedades dinámicamente, controlar eventos, etc.
- Varios campos clave.
- Varios campos de datos para las columnas de hipervínculo.
- Apariencia personalizable a través de temas y estilos.

**Campos de columna**

Cada columna de la GridView control se representa mediante un DataControlField objeto. De forma predeterminada, la propiedad AutoGenerateColumns se establece en **true**, lo que crea un objeto AutoGeneratedField para cada campo del origen de datos. A continuación, cada campo se representa como una columna en el control GridView en el orden en que cada campo aparece en el origen de datos. También puede controlar manualmente qué campos de columna aparecen en el **control GridView** estableciendo la propiedad **AutoGenerateColumns** en **false** y, a continuación, definiendo su propia colección de campos de columna. Diferentes tipos de campos de columna determinan el comportamiento de las columnas del control.

En la tabla siguiente se enumeran los diferentes tipos de campos de columna que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos. Este es el tipo de columna predeterminado de la GridView control. |
| ButtonField | Muestra un botón de comando para cada elemento en el GridView control. Esto le permite crear una columna de controles de botón personalizados, como el agregar o el quitar botón. |
| CheckBoxField | Muestra una casilla de verificación para cada elemento en el GridView control. Este tipo de campo de columna se utiliza normalmente para mostrar campos con un valor booleano. |
| CommandField | Muestra los botones de comando predefinidos para realizar operaciones de selección, edición o eliminación. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como hipervínculo. Este tipo de campo de columna le permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen para cada elemento en el GridView control. |
| TemplateField | Muestra contenido definido por el usuario para cada elemento en el control GridView según una plantilla especificada. Este tipo de campo de columna le permite crear un campo de columna personalizado. |

Para definir una colección de campos de columna mediante declaración, primero agregue etiquetas ** &lt;de&gt; ** apertura y cierre de columnas entre las etiquetas de apertura y cierre del control GridView. A continuación, enumere los campos de columna que desea incluir entre las etiquetas ** &lt;Columnas&gt; ** de apertura y cierre. Las columnas especificadas se agregan a la colección Columns en el orden indicado. El **Columns** colección almacena todos los campos de columna en el control y le permite administrar mediante programación los campos de columna en el GridView control.

Los campos de columna declarados explícitamente se pueden mostrar en combinación con los campos de columna generados automáticamente. Cuando se utilizan ambos, los campos de columna declarados explícitamente se representan primero, seguidos de los campos de columna generados automáticamente.

## <a name="binding-to-data"></a>Enlace a datos

El control GridView se puede enlazar a un control de origen de datos (como **SqlDataSource**, **ObjectDataSource**, etc.), así como a cualquier origen de datos que implemente la interfaz System.Collections.IEnumerable (como System.Data.DataView, System.Collections.ArrayList o System.Collections.Hashtable). Utilice uno de los métodos siguientes para enlazar el control GridView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca el DataSourceID propiedad de la GridView control en el valor de identificador del control de origen de datos. El control GridView se enlaza automáticamente al control de origen de datos especificado y puede aprovechar las capacidades del control de origen de datos para realizar la ordenación, actualización, eliminación y funcionalidad de paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el System.Collections.IEnumerable interfaz, establezca mediante programación el DataSource propiedad de la GridView control en el origen de datos y, a continuación, llame a la DataBind método. Cuando se usa este método, el control GridView no proporciona funcionalidad integrada de ordenación, actualización, eliminación y paginación. Debe proporcionar esta funcionalidad usted mismo.

## <a name="data-operations"></a>Operaciones de datos

El control GridView proporciona muchas capacidades integradas que permiten al usuario ordenar, actualizar, eliminar, seleccionar y paginar los elementos del control. Cuando el control GridView está enlazado a un control de origen de datos, el control GridView puede aprovechar las capacidades del control de origen de datos y proporcionar funcionalidad automática de ordenación, actualización y eliminación.

> [!NOTE]
> El control GridView puede proporcionar compatibilidad para ordenar, actualizar y eliminar con otros tipos de orígenes de datos; sin embargo, deberá proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.

Ordenación permite al usuario ordenar los elementos en el control GridView con respecto a una columna específica haciendo clic en el encabezado de la columna. Para habilitar la ordenación, establezca la propiedad AllowSorting en **true**.

Las funciones de actualización automática, eliminación y selección se habilitan cuando se hace clic en un botón de un campo de columna **ButtonField** o **TemplateField,** con un nombre de comando de "Editar", "Eliminar" y "Seleccionar", respectivamente. El control GridView puede agregar automáticamente un campo de columna **CommandField** con un botón Editar, Eliminar o Seleccionar si la propiedad AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton está establecida en **true**, respectivamente.

> [!NOTE]
> La inserción de registros en el origen de datos no es compatible directamente con el control GridView. Sin embargo, es posible insertar registros mediante el control GridView junto con el control DetailsView o FormView.

En lugar de mostrar todos los registros en el origen de datos al mismo tiempo, el control GridView puede dividir automáticamente los registros en páginas. Para habilitar la paginación, establezca la propiedad AllowPaging en **true**.

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia de la GridView control estableciendo las propiedades de estilo para las diferentes partes del control. En la tabla siguiente se enumeran las diferentes propiedades de estilo.

| **Propiedad Style** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el GridView control. Cuando se establece esta propiedad, las filas de datos se muestran alternando entre el RowStyle configuración y el **AlternatingRowStyle** configuración. |
| EditRowStyle | La configuración de estilo para la fila que se está editando en el GridView control. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el control GridView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página de la GridView control. |
| HeaderStyle | La configuración de estilo para la fila de encabezado de la GridView control. |
| PagerStyle | La configuración de estilo para la fila de paginación de la GridView control. |
| RowStyle | La configuración de estilo para las filas de datos en el GridView control. Cuando el **AlternatingRowStyle** propiedad también se establece, las filas de datos se muestran alternando entre el **RowStyle** configuración y **el AlternatingRowStyle** configuración. |
| SelectedRowStyle | La configuración de estilo para la fila seleccionada en el GridView control. |

También puede mostrar u ocultar diferentes partes del control. En la tabla siguiente se enumeran las propiedades que controlan qué partes se muestran u ocultan.

| **Propiedad** | **Descripción** |
| --- | --- |
| ShowFooter | Muestra u oculta la sección de pie de página de la GridView control. |
| ShowHeader | Muestra u oculta la sección de encabezado de la GridView control. |

### <a name="events"></a>Events

El control GridView proporciona varios eventos con los que puede programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el control GridView.

| **Evento** | **Descripción** |
| --- | --- |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones del localizador, pero después de la GridView control controla la operación de paginación. Este evento se utiliza normalmente cuando necesita realizar una tarea después de que el usuario navega a una página diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones del localizador, pero antes de que el control GridView controla la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |
| RowCancelingEditar | Se produce cuando se hace clic en el botón Cancelar de una fila, pero antes de que el control GridView salga del modo de edición. Este evento se utiliza a menudo para detener la operación de cancelación. |
| RowCommand | Se produce cuando se hace clic en un botón en el control GridView. Este evento se utiliza a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| RowCreated | Se produce cuando se crea una nueva fila en el control GridView. Este evento se utiliza a menudo para modificar el contenido de una fila cuando se crea la fila. |
| RowDataBound | Se produce cuando una fila de datos está enlazada a datos en el control GridView. Este evento se utiliza a menudo para modificar el contenido de una fila cuando la fila está enlazada a datos. |
| RowDeleted | Se produce cuando se hace clic en el botón Eliminar de una fila, pero después de que el control GridView elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| RowDeleting | Se produce cuando se hace clic en el botón Eliminar de una fila, pero antes de que el control GridView elimine el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| RowEditing | Se produce cuando se hace clic en el botón Editar de una fila, pero antes de que el control GridView entre en modo de edición. Este evento se utiliza a menudo para cancelar la operación de edición. |
| RowUpdated | Se produce cuando se hace clic en el botón Actualizar de una fila, pero después de que el control GridView actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| RowUpdating | Se produce cuando se hace clic en el botón Actualizar de una fila, pero antes de que el control GridView actualice la fila. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| SelectedIndexChanged | Se produce cuando se hace clic en el botón Seleccionar de una fila, pero después de que el control GridView controla la operación de selección. Este evento se utiliza a menudo para realizar una tarea después de seleccionar una fila en el control. |
| SelectedIndexChanging | Se produce cuando se hace clic en el botón Seleccionar de una fila, pero antes de que el control GridView controle la operación de selección. Este evento se utiliza a menudo para cancelar la operación de selección. |
| Ordenados | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero después de que el control GridView controla la operación de ordenación. Este evento se utiliza normalmente para realizar una tarea después de que el usuario hace clic en un hipervínculo para ordenar una columna. |
| Ordenación | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero antes de que el control GridView controla la operación de ordenación. Este evento se utiliza a menudo para cancelar la operación de ordenación o para realizar una rutina de ordenación personalizada. |

## <a name="formview"></a>Formview

El FormView control se utiliza para mostrar un único registro de un origen de datos. Es similar al control DetailsView, excepto que muestra plantillas definidas por el usuario en lugar de campos de fila. La creación de sus propias plantillas le ofrece una mayor flexibilidad para controlar cómo se muestran los datos. El control FormView admite las siguientes características:

- Enlace a controles de origen de datos, como SqlDataSource y ObjectDataSource.
- Capacidades de inserción integradas.
- Capacidades integradas de actualización y eliminación.
- Capacidades de paginación integradas.
- Acceso mediante programación al modelo de objetos FormView para establecer propiedades dinámicamente, controlar eventos, etc.
- Apariencia personalizable a través de plantillas, temas y estilos definidos por el usuario.

## <a name="templates"></a>Plantillas

Para que el formView control para mostrar contenido, debe crear plantillas para las diferentes partes del control. La mayoría de las plantillas son opcionales; sin embargo, debe crear una plantilla para el modo en el que está configurado el control. Por ejemplo, un FormView control que admite la inserción de registros debe tener una plantilla de elemento de inserción definida. En la tabla siguiente se enumeran las diferentes plantillas que puede crear.

| **Tipo de plantilla** | **Descripción** |
| --- | --- |
| EditItemTemplate | Define el contenido de la fila de datos cuando el FormView control está en modo de edición. Esta plantilla normalmente contiene controles de entrada y botones de comando con los que el usuario puede editar un registro existente. |
| EmptyDataTemplate | Define el contenido de la fila de datos vacía que se muestra cuando el FormView control está enlazado a un origen de datos que no contiene ningún registro. Esta plantilla normalmente contiene contenido para alertar al usuario de que el origen de datos no contiene ningún registro. |
| FooterTemplate | Define el contenido de la fila de pie de página. Esta plantilla normalmente contiene cualquier contenido adicional que desee mostrar en la fila de pie de página. Como alternativa, simplemente puede especificar el texto que se mostrará en la fila de pie de página estableciendo el FooterText propiedad. |
| HeaderTemplate | Define el contenido de la fila de encabezado. Esta plantilla normalmente contiene cualquier contenido adicional que desee mostrar en la fila de encabezado. Como alternativa, simplemente puede especificar el texto que se mostrará en la fila de encabezado estableciendo la propiedad HeaderText. |
| ItemTemplate | Define el contenido de la fila de datos cuando el FormView control está en modo de solo lectura. Esta plantilla normalmente contiene contenido para mostrar los valores de un registro existente. |
| InsertItemTemplate | Define el contenido de la fila de datos cuando el FormView control está en modo de inserción. Esta plantilla normalmente contiene controles de entrada y botones de comando con los que el usuario puede agregar un nuevo registro. |
| PagerTemplate | Define el contenido de la fila del localizador que se muestra cuando la característica de paginación está habilitada (cuando la propiedad AllowPaging está establecida en **true**). Esta plantilla normalmente contiene controles con los que el usuario puede navegar a otro registro. |

Los controles de entrada de la plantilla de elemento de edición y la plantilla de elemento de inserción se pueden enlazar a los campos de un origen de datos mediante una expresión de enlace bidireccional. Esto permite que el FormView control extraiga automáticamente los valores del control de entrada para una operación de actualización o inserción. Las expresiones de enlace bidireccionales también permiten que los controles de entrada de una plantilla de elemento de edición muestren automáticamente los valores de campo originales.

### <a name="binding-to-data"></a>Enlace a datos

El FormView control se puede enlazar a un control de origen de datos (como **SqlDataSource**, AccessDataSource, **ObjectDataSource,** etc.), o a cualquier origen de datos que implementa el System.Collections.IEnumerable interfaz (como System.Data.DataView, System.Collections.ArrayList, y System.Collections.Hashtable). Utilice uno de los métodos siguientes para enlazar el formView control al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca el DataSourceID propiedad de la FormView control en el identificador valor del control de origen de datos. El FormView control se enlaza automáticamente al control de origen de datos especificado y puede aprovechar las capacidades del control de origen de datos para realizar la inserción, actualización, eliminación y funcionalidad de paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** interfaz, establezca mediante programación el DataSource propiedad de la FormView control en el origen de datos y, a continuación, llame a la DataBind método. Cuando se usa este método, el FormView control no proporciona integrada inserción, actualización, eliminación y funcionalidad de paginación. Debe proporcionar esta funcionalidad mediante el evento adecuado.

## <a name="data-operations"></a>Operaciones de datos

El FormView control proporciona muchas capacidades integradas que permiten al usuario actualizar, eliminar, insertar y paginar a través de los elementos en el control. Cuando el FormView control está enlazado a un control de origen de datos, el FormView control puede aprovechar las capacidades del control de origen de datos y proporcionar actualización automática, eliminación, inserción y funcionalidad de paginación. El FormView control puede proporcionar compatibilidad para las operaciones de actualización, eliminación, inserción y paginación con otros tipos de orígenes de datos; sin embargo, debe proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.

Dado que el FormView control utiliza plantillas, no proporciona una manera de generar automáticamente botones de comando para realizar operaciones de actualización, eliminación o inserción. Debe incluir manualmente estos botones de comando en la plantilla adecuada. El FormView control reconoce ciertos botones que tienen su **CommandName** propiedades establecidas en valores específicos. En la tabla siguiente se enumeran los botones de comando que reconoce el control FormView.

| **Botón** | **Valor Commandname** | **Descripción** |
| --- | --- | --- |
| Cancelar | "Cancelar" | Se utiliza en la actualización o inserción de operaciones para cancelar la operación y descartar los valores introducidos por el usuario. El FormView control, a continuación, vuelve al modo especificado por el DefaultMode propiedad. |
| Eliminar | “Eliminar” | Se utiliza en la eliminación de operaciones para eliminar el registro mostrado del origen de datos. Genera los eventos ItemDeleting y ItemDeleted. |
| Editar | "Editar" | Se utiliza en las operaciones de actualización para poner el FormView control en modo de edición. El contenido especificado en el **EditItemTemplate** propiedad se muestra para la fila de datos. |
| Insertar | "Insertar" | Se utiliza en la inserción de operaciones para intentar insertar un nuevo registro en el origen de datos utilizando los valores proporcionados por el usuario. Genera el ItemInserting y ItemInserted eventos. |
| Nuevo | "Nuevo" | Se utiliza en las operaciones de inserción para poner el FormView control en modo de inserción. El contenido especificado en el **InsertItemTemplate** propiedad se muestra para la fila de datos. |
| Página | "Página" | Se utiliza en operaciones de paginación para representar un botón de la fila del localizador que realiza la paginación. Para especificar la operación de paginación, establezca la propiedad **CommandArgument** del botón en "Next", "Prev", "First", "Last" o el índice de la página a la que se va a navegar. Genera el PageIndexChanging y PageIndexChanged eventos. |
| Actualizar | "Actualización" | Se utiliza en las operaciones de actualización para intentar actualizar el registro mostrado en el origen de datos con los valores proporcionados por el usuario. Genera los eventos ItemUpdating y ItemUpdated. |

A diferencia del botón Eliminar (que elimina el registro mostrado inmediatamente), cuando se hace clic en el botón Editar o Nuevo, el control FormView entra en modo de edición o inserción respectivamente. En modo de edición, el contenido contenido en el **EditItemTemplate** propiedad se muestra para el elemento de datos actual. Normalmente, la plantilla de elemento de edición se define de forma que el botón Editar se reemplace por un botón Actualizar y Cancelar. Los controles de entrada que son adecuados para el tipo de datos del campo (como un control TextBox o CheckBox) también se muestran normalmente con el valor de un campo para que el usuario lo modifique. Al hacer clic en el botón Actualizar, se actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios.

Del mismo modo, el contenido contenido en el **InsertItemTemplate** propiedad se muestra para el elemento de datos cuando el control está en modo de inserción. La plantilla de elemento de inserción se define normalmente de modo que el botón Nuevo se reemplaza con un botón Insertar y cancelar, y se muestran controles de entrada vacíos para que el usuario escriba los valores para el nuevo registro. Al hacer clic en el botón Insertar se inserta el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios.

El FormView control proporciona una característica de paginación, que permite al usuario navegar a otros registros en el origen de datos. Cuando está habilitado, se muestra una fila de buscapersonas en el FormView control que contiene los controles de navegación de página. Para habilitar la paginación, establezca la propiedad **AllowPaging** en **true**. Puede personalizar la fila del localizador estableciendo las propiedades de los objetos contenidos en el PagerStyle y el PagerSettings propiedad. En lugar de usar la interfaz de usuario de fila de paginación integrada, puede crear su propia interfaz de usuario mediante el uso de la **PagerTemplate** propiedad.

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia de la FormView control estableciendo las propiedades de estilo para las diferentes partes del control. En la tabla siguiente se enumeran las diferentes propiedades de estilo.

| **Propiedad Style** | **Descripción** |
| --- | --- |
| EditRowStyle | La configuración de estilo para la fila de datos cuando el FormView control está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el FormView control cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página de la FormView control. |
| HeaderStyle | La configuración de estilo para la fila de encabezado de la FormView control. |
| InsertRowStyle | La configuración de estilo para la fila de datos cuando el FormView control está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila del buscapersonas que se muestra en el FormView control cuando se habilita la característica de paginación. |
| RowStyle | La configuración de estilo para la fila de datos cuando el FormView control está en modo de solo lectura. |

## <a name="events"></a>Events

El FormView control proporciona varios eventos que puede programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el FormView control.

| **Evento** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón dentro de un FormView control. Este evento se utiliza a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| ItemCreated | Se produce después de todos formViewRow objetos se crean en el FormView control. Este evento se utiliza a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón Eliminar (un botón con su propiedad **CommandName** establecida en "Eliminar"), pero después de que el control FormView elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de que el FormView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón Insertar (un botón con su propiedad **CommandName** establecida en "Insertar"), pero después de que el control FormView inserta el registro. Este evento se utiliza a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserción | Se produce cuando se hace clic en un botón Insertar, pero antes de que el FormView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón Actualizar (un botón con su propiedad **CommandName** establecida en "Update"), pero después de que el control FormView actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de que el FormView control actualiza el registro. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el FormView control cambia de modo (para editar, insertar o modo de solo lectura). Este evento se utiliza a menudo para realizar una tarea cuando el FormView control cambia de modo. |
| ModeChanging | Se produce antes de que el control FormView cambie de modo (para editar, insertar o en modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones del localizador, pero después de la FormView control controla la operación de paginación. Este evento se utiliza normalmente cuando necesita realizar una tarea después de que el usuario navega a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones del localizador, pero antes de que el FormView control controla la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="detailsview"></a>Detailsview

El DetailsView control se utiliza para mostrar un único registro de un origen de datos en una tabla, donde cada campo del registro se muestra en una fila de la tabla. Se puede usar en combinación con un control GridView para escenarios de detalle maestro. El control DetailsView admite las siguientes características:

- Enlace a controles de origen de datos, como SqlDataSource.
- Capacidades de inserción integradas.
- Capacidades integradas de actualización y eliminación.
- Capacidades de paginación integradas.
- Acceso mediante programación al modelo de objetos DetailsView para establecer propiedades dinámicamente, controlar eventos, etc.
- Apariencia personalizable a través de temas y estilos.

## <a name="row-fields"></a>Campos de fila

Cada fila de datos en el DetailsView control se crea mediante la declaración de un control de campo. Diferentes tipos de campos de fila determinan el comportamiento de las filas del control. Los controles de campo derivan de DataControlField. En la tabla siguiente se enumeran los diferentes tipos de campos de fila que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo de un origen de datos como texto. |
| ButtonField | Muestra un botón de comando en el DetailsView control. Esto le permite mostrar una fila con un control de botón personalizado, como un agregar o un quitar botón. |
| CheckBoxField | Muestra una casilla de verificación en el DetailsView control. Este tipo de campo de fila se utiliza normalmente para mostrar campos con un valor booleano. |
| CommandField | Muestra los botones de comando integrados para realizar operaciones de edición, inserción o eliminación en el control DetailsView. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como hipervínculo. Este tipo de campo de fila le permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen en el DetailsView control. |
| TemplateField | Muestra contenido definido por el usuario para una fila en el DetailsView control según una plantilla especificada. Este tipo de campo de fila le permite crear un campo de fila personalizado. |

De forma predeterminada, la propiedad AutoGenerateRows se establece en **true**, que genera automáticamente un objeto de campo de fila enlazado para cada campo de un tipo enlazable en el origen de datos. Los tipos enlazables válidos son String, DateTime, Decimal, Guid y el conjunto de tipos primitivos. A continuación, cada campo se muestra en una fila como texto, en el orden en que cada campo aparece en el origen de datos.

La generación automática de las filas proporciona una forma rápida y sencilla de mostrar todos los campos del registro. Sin embargo, para hacer uso de las capacidades avanzadas del control DetailsView debe declarar explícitamente los campos de fila para incluir en el DetailsView control. Para declarar los campos de fila, establezca primero la propiedad **AutoGenerateRows** en **false**. A continuación, agregue etiquetas ** &lt;Fields&gt; ** de apertura y cierre entre las etiquetas de apertura y cierre del control DetailsView. Por último, enumere los campos de fila que desea incluir entre las etiquetas ** &lt;de apertura&gt; ** y cierre de campos. Los campos de fila especificados se agregan a la colección Fields en el orden indicado. El **Fields** colección le permite administrar mediante programación los campos de fila en el DetailsView control.

> [!NOTE]
> Los campos de fila generados automáticamente no se agregan a la colección Fields.

## <a name="binding-to-data"></a>Enlace a datos

El DetailsView control se puede enlazar a un control de origen de datos, como **SqlDataSource** o AccessDataSource, o a cualquier origen de datos que implementa el System.Collections.IEnumerable interfaz, como System.Data.DataView, System.Collections.ArrayList y System.Collections.Hashtable.

Utilice uno de los métodos siguientes para enlazar el control DetailsView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control DetailsView en el valor de identificador del control de origen de datos. El DetailsView control se enlaza automáticamente al control de origen de datos especificado. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** interfaz, establezca mediante programación el DataSource propiedad de la DetailsView control en el origen de datos y, a continuación, llame a la DataBind método.

## <a name="security"></a>Seguridad

Este control se puede utilizar para mostrar la entrada del usuario, que puede incluir script de cliente malintencionado. Compruebe cualquier información que se envíe desde un cliente para el script ejecutable, instrucciones SQL u otro código antes de mostrarla en la aplicación. ASP.NET proporciona una característica de validación de solicitudes de entrada para bloquear el script y HTML en la entrada del usuario.

## <a name="data-operations"></a>Operaciones de datos

El DetailsView control proporciona capacidades integradas que permiten al usuario actualizar, eliminar, insertar y paginar a través de los elementos en el control. Cuando el DetailsView control está enlazado a un control de origen de datos, el DetailsView control puede aprovechar las capacidades del control de origen de datos y proporcionar actualización automática, eliminación, inserción y funcionalidad de paginación.

El DetailsView control puede proporcionar compatibilidad para las operaciones de actualización, eliminación, inserción y paginación con otros tipos de orígenes de datos; sin embargo, debe proporcionar la implementación para estas operaciones en un controlador de eventos adecuado.

El DetailsView control puede agregar automáticamente un **CommandField** campo de fila con un Editar, Eliminar, o Nuevo botón estableciendo el AutoGenerateEditButton, AutoGenerateDeleteButton, o AutoGenerateInsertButton propiedades **true**, respectivamente. A diferencia del botón Eliminar (que elimina el registro seleccionado inmediatamente), cuando se hace clic en el botón Editar o Nuevo, el control DetailsView entra en modo de edición o inserción, respectivamente. En el modo de edición, el botón Editar se sustituye por un botón Actualizar y Cancelar. Los controles de entrada que son adecuados para el tipo de datos del campo (como un control TextBox o CheckBox) se muestran con el valor de un campo para que el usuario lo modifique. Al hacer clic en el botón Actualizar, se actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios. Del mismo modo, en el modo de inserción, el botón Nuevo se reemplaza con un botón Insertar y cancelar, y se muestran controles de entrada vacíos para que el usuario introduzca los valores del nuevo registro.

El DetailsView control proporciona una característica de paginación, que permite al usuario navegar a otros registros en el origen de datos. Cuando está habilitado, los controles de navegación de página se muestran en una fila de buscapersonas. Para habilitar la paginación, establezca la propiedad AllowPaging en **true**. La fila del buscapersonas se puede personalizar mediante el PagerStyle y PagerSettings propiedades.

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia de la DetailsView control estableciendo las propiedades de estilo para las diferentes partes del control. En la tabla siguiente se enumeran las diferentes propiedades de estilo.

| **Propiedad Style** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el DetailsView control. Cuando se establece esta propiedad, las filas de datos se muestran alternando entre el RowStyle configuración y el **AlternatingRowStyle** configuración. |
| CommandRowStyle | La configuración de estilo de la fila que contiene los botones de comando integrados en el DetailsView control. |
| EditRowStyle | La configuración de estilo para las filas de datos cuando el DetailsView control está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el DetailsView control cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página de la DetailsView control. |
| HeaderStyle | La configuración de estilo para la fila de encabezado de la DetailsView control. |
| InsertRowStyle | La configuración de estilo para las filas de datos cuando el DetailsView control está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila del buscapersonas del control DetailsView. |
| RowStyle | La configuración de estilo para las filas de datos en el DetailsView control. Cuando el **AlternatingRowStyle** propiedad también se establece, las filas de datos se muestran alternando entre el **RowStyle** configuración y **el AlternatingRowStyle** configuración. |
| FieldHeaderStyle | La configuración de estilo para la columna de encabezado de la DetailsView control. |

## <a name="events"></a>Events

El DetailsView control proporciona varios eventos que puede programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el DetailsView control. El DetailsView control también hereda estos eventos de sus clases base: DataBinding, DataBound, Disposed, Init, Load, PreRender, y Render.

| **Evento** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón en el DetailsView control. |
| ItemCreated | Se produce después de todos DetailsViewRow objetos se crean en el DetailsView control. Este evento se utiliza a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón Eliminar, pero después de la DetailsView control elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de que el Control DetailsView elimine el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón Insertar, pero después de la DetailsView control inserta el registro. Este evento se utiliza a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserción | Se produce cuando se hace clic en un botón Insertar, pero antes de que el DetailsView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón Actualizar, pero después de la DetailsView control actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de que el Control DetailsView actualice el registro. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el Control DetailsView cambia de modo (editar, insertar o modo de solo lectura). Este evento se utiliza a menudo para realizar una tarea cuando el DetailsView control cambia de modo. |
| ModeChanging | Se produce antes de que el control DetailsView cambie de modo (editar, insertar o modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones del localizador, pero después de la DetailsView control controla la operación de paginación. Este evento se utiliza normalmente cuando necesita realizar una tarea después de que el usuario navega a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones del localizador, pero antes de que el Control DetailsView controla la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="the-menu-control"></a>El control de menú

El control Menú de ASP.NET 2.0 está diseñado para ser un sistema de navegación completo. Se puede enlazar datos fácilmente a orígenes de datos jerárquicos como SiteMapDataSource.

Una estructura de controles Menu se puede definir mediante declaración o dinámicamente y consta de un único nodo raíz y cualquier número de subnodos. El código siguiente define mediante declaración un menú para el Menu control.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

En el ejemplo anterior, el Home.aspx nodo es el nodo raíz. Todos los demás nodos están anidados dentro del nodo raíz en varios niveles.

Hay dos tipos de menús que el Menu control puede representar; menús estáticos y menús dinámicos. Los menús estáticos constan de elementos de menú que siempre están visibles. Los menús dinámicos constan de elementos de menú que solo son visibles cuando el usuario pasa el ratón sobre ellos. Los clientes a menudo pueden confundir los menús estáticos con menús definidos mediante declaración y menús dinámicos con menús enlazados a datos en tiempo de ejecución. De hecho, los menús dinámicos y estáticos no están relacionados con el método de población. Los términos *estático* y *dinámico* se refieren únicamente a si el menú se muestra estáticamente de forma predeterminada o solo se muestra cuando el usuario realiza alguna acción.

El **StaticDisplayLevels** propiedad se utiliza para configurar cuántos niveles del menú son estáticos y, por lo tanto, se muestrade de forma predeterminada. En el ejemplo anterior, establecer la propiedad **StaticDisplayLevels** en un valor de 2 haría que el menú mostrara estáticamente el nodo Inicio, el nodo Música y el nodo Movies. Todos los demás nodos se mostrarían dinámicamente cuando el usuario pasa el cursor sobre el nodo primario.

La propiedad **MaximumDynamicDisplayLevels** configura el número máximo de niveles dinámicos que el menú es capaz de mostrar. Se descartan los menús dinámicos en un nivel superior al valor especificado por la propiedad **MaximumDynamicDisplayLevels.**

> [!NOTE]
> Es casi seguro que puede encontrar situaciones en las que los menús no parecen representarse debido a la propiedad MaximumDynamicDisplayLevels. En esos casos, asegúrese de que la propiedad está suficientemente establecida para permitir la visualización de los menús de los clientes.

## <a name="data-binding-the-menu-control"></a>Enlace de datos del control de menú

El Menu control se puede enlazar a cualquier origen de datos jerárquico como el SiteMapDataSource o el XMLDataSource. SiteMapDataSource es el método más utilizado para el enlace de datos a un Menu control porque se alimenta del Web.sitemap archivo y su esquema proporciona una API conocida para el Menu control. La siguiente lista muestra un simple archivo Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Observe que solo hay un elemento siteMapNode raíz, en este caso, el elemento Home. Se pueden configurar varios atributos para cada siteMapNode. Los atributos más utilizados son:

- **url** Especifica la dirección URL que se mostrará cuando un usuario haga clic en el elemento de menú. Si este atributo no está presente, el nodo simplemente volverá a publicar cuando se haga clic en él.
- **título** Especifica el texto que se muestra en el elemento de menú.
- **descripción** Se utiliza como documentación para el nodo. También se muestra como información sobre herramientas cuando el ratón se coloca sobre el nodo.
- **siteMapFile** Permite sitemaps anidados. Este atributo debe apuntar a un archivo de sitemap de ASP.NET bien formado.
- **roles** Permite controlar la apariencia de un nodo mediante ASP.NET recorte de seguridad.

Tenga en cuenta que, aunque todos estos atributos son opcionales, el comportamiento del menú puede no ser el esperado si no se especifican. Por ejemplo, si se especifica el atributo *url* pero el atributo *description* no lo es, el nodo no será visible y no habrá forma de navegar a la dirección URL especificada.

## <a name="controlling-a-menus-operation"></a>Control de una operación de menús

Hay varias propiedades que afectan al funcionamiento de un ASP.NET Menu control; el **Orientation** propiedad, el **DisappearAfter** propiedad, el **StaticItemFormatString** propiedad y el **StaticPopoutImageUrl** propiedad son solo algunos de estos.

- La **Orientación** se puede establecer en *horizontal* o *vertical* y controla si los elementos de menú estáticos se distribuyen horizontalmente en una fila o verticalmente y se apilan unos sobre otros. Esta propiedad no afecta a los menús dinámicos.
- La propiedad **DisappearAfter** configura cuánto tiempo debe permanecer visible un menú dinámico después de que el mouse se haya alejado de él. El valor se especifica en milisegundos y el valor predeterminado es 500. Establecer esta propiedad en un valor de -1 hará que el menú nunca desaparezca automáticamente. En ese caso, el menú solo desaparecerá cuando el usuario haga clic fuera del menú.
- La propiedad **StaticItemFormatString** facilita el mantenimiento de la verborrea coherente en el sistema de menús. Al especificar esta *{0}* propiedad, debe especificarse en lugar de la descripción que aparece en el origen de datos. Por ejemplo, para que el elemento de menú del ejercicio 1 diga Visitar {0} nuestra página de productos, etc., especificaría Visitar nuestra página para StaticItemFormatString. En tiempo de ejecución, {0} ASP.NET reemplazará cualquier aparición de con la descripción correcta para el elemento de menú.
- El **StaticPopoutImageUrl** propiedad especifica la imagen que se utiliza para indicar que un nodo de menú determinado tiene nodos secundarios a los que se puede tener acceso pasando el cursor sobre él. Los menús dinámicos seguirán utilizando la imagen predeterminada.

## <a name="templated-menu-controls"></a>Controles de menú con plantillas

El Menu control es un control con plantilla y permite dos ItemTemplates diferentes; el StaticItemTemplate y el DynamicItemTemplate. Con estas plantillas, puede agregar fácilmente controles de servidor o controles de usuario a los menús.

Para editar las plantillas en Visual Studio .NET, haga clic en el botón Etiqueta inteligente del menú y elija Editar plantillas. A continuación, puede elegir entre editar el StaticItemTemplate o el DynamicItemTemplate.

Los controles agregados a la StaticItemTemplate aparecerán en el menú estático cuando se carga la página. Los controles agregados a DynamicItemTemplate aparecerán en todos los menús emergentes.

## <a name="menu-events"></a>Eventos del menú

El control Menu tiene dos eventos que son únicos; el **MenuItemClicked** y el **MenuItemDatabound** eventos.

El MenuItemClicked evento se produce cuando se hace clic en un elemento de menú. El MenuItemDatabound evento se produce cuando un elemento de menú está enlazado a datos. El **MenuEventArgs** que se pasa al controlador de eventos proporciona acceso al elemento de menú a través de la Item propiedad.

## <a name="controlling-a-menus-appearance"></a>Controlar la apariencia de un menú

También puede afectar a la apariencia de un control de menú mediante uno o varios de los muchos estilos disponibles para dar formato a los menús. Entre ellos se encuentran **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**y **DynamicHoverStyle**. Estas propiedades se configuran mediante una cadena de estilo HTML estándar. Por ejemplo, lo siguiente afectará al estilo de los menús dinámicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si está utilizando cualquiera de los estilos &lt;Hover, deberá agregar un elemento head&gt; a la página con el elemento *runat* establecido en *server*.

Los controles de menú también admiten el uso de temas ASP.NET 2.0.

## <a name="the-treeview-control"></a>El control TreeView

El TreeView control muestra datos en una estructura de árbol. Al igual que con el Menu control, se pueden enlazar fácilmente a datos enlazados a cualquier origen de datos jerárquico como el SiteMapDataSource.

La primera pregunta que es probable que los clientes pregunten sobre el control TreeView en ASP.NET 2.0 es si está relacionado o no con el WebControl de TreeView IE que estaba disponible para ASP.NET 1.x. No lo es. El control TreeView de ASP.NET 2.0 se escribió desde cero y ofrece una mejora significativa con respecto al IE TreeView WebControl que estaba disponible anteriormente.

No voy a entrar en detalle sobre cómo enlazar un TreeView control a un mapa del sitio, ya que se realiza exactamente de la misma manera que el Menu control. Sin embargo, el TreeView control tiene algunas diferencias distintas en la forma en que funciona.

De forma predeterminada, un TreeView control aparece completamente expandido. Para cambiar el nivel de expansión al cargar la carga inicial, modifique la propiedad **ExpandDepth** del control. Esto es especialmente importante en los casos en que TreeView está enlazado a datos al expandir nodos concretos.

## <a name="databinding-the-treeview-control"></a>DataBinding el TreeView control

A diferencia del control Menu, TreeView se presta bien para controlar grandes cantidades de datos. Por lo tanto, además de enlace de datos a un SiteMapDataSource o XMLDataSource, el TreeView es a menudo datos enlazados a un DataSet u otros datos relacionales. En los casos en que el TreeView control está enlazado a grandes cantidades de datos, es mejor enlazar solo a los datos que son realmente visibles en el control. A continuación, puede enlazar datos a datos adicionales a medida que se expanden los nodos TreeView.

En estos casos, la propiedad **PopulateOnDemand** de TreeView debe establecerse en *true*. A continuación, deberá proporcionar una implementación para el **TreeNodePopulate** método.

## <a name="data-binding-without-postback"></a>Enlace de datos sin devolución posterior

Tenga en cuenta que al expandir un nodo en el ejemplo anterior por primera vez, la página se devuelve y se actualiza. Eso no es un problema en este ejemplo, pero puede imaginar que podría estar en un entorno de producción con una gran cantidad de datos. Un escenario mejor sería uno en el que el TreeView todavía rellenaría dinámicamente sus nodos, pero sin una publicación de nuevo en el servidor.

Al establecer el **PopulateNodesFromClient** y el **PopulateOnDemand** propiedades true, el ASP.NET TreeView control rellenará dinámicamente nodos sin una devolución de mensajes. Cuando se expande el nodo primario, se realiza una solicitud XMLHttp desde el cliente y se desencadena el evento OnTreeNodePopulate. El servidor responde con una isla de datos XML que, a continuación, se utiliza para enlazar datos a los nodos secundarios.

ASP.NET crea dinámicamente el código de cliente que implementa esta funcionalidad. Las &lt;&gt; etiquetas de script que contienen el script se generan apuntando a un archivo AXD. Por ejemplo, la lista siguiente muestra los vínculos de script para el código de script que genera la solicitud XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si examina el archivo AXD anterior en su navegador y lo abre, verá el código que implementa la solicitud XMLHttp. Este método impide que los clientes modifiquen el archivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlar el funcionamiento del control TreeView

El TreeView control tiene varias propiedades que afectan al funcionamiento del control. Las propiedades más obvias son **ShowCheckBoxes**, **ShowExpandCollapse**y **ShowLines**.

El **ShowCheckBoxes** propiedad afecta a si los nodos muestran o no una casilla de verificación cuando se representan. Los valores válidos para esta propiedad son **None**, **Root**, **Parent**, **Leaf**y **All**. Afectan al control TreeView de la siguiente manera:

| **Valor de la propiedad** | **Efecto** |
| --- | --- |
| None | Las casillas de verificación no se muestran en ningún nodo. Esta es la configuración predeterminada. |
| Root | Solo se muestra una casilla de verificación en el nodo raíz. |
| Parent | Solo se muestra una casilla de verificación en los nodos que tienen nodos secundarios. Esos nodos secundarios pueden ser nodos primarios o nodos hoja. |
| Hoja | Solo se muestra una casilla de verificación en los nodos que no tienen nodos secundarios. |
| All | Se muestra una casilla de verificación en todos los nodos. |

Cuando se utilizan casillas de verificación, el **CheckedNodes** propiedad devolverá una colección de TreeView nodos que se comprueban en la devolución de datos.

La propiedad **ShowExpandCollapse** controla el aspecto de la imagen expand/collapse junto a los nodos raíz y primarios. Si esta propiedad se establece en **false**, TreeView nodos se representan como hipervínculos y se expanden o contrae haciendo clic en el vínculo.

La propiedad **ShowLines** controla si se muestran o no líneas que conectan nodos primarios a nodos secundarios. Cuando **es false** (valor predeterminado), no se muestra ninguna línea. Cuando **true**, el TreeView control usará imágenes de líneas en la carpeta especificada por el **LineImagesFolder** propiedad.

Para personalizar la apariencia de las líneas TreeView, Visual Studio .NET 2005 incluye una herramienta Diseñador de líneas. Puede acceder a esta herramienta mediante el botón Etiqueta inteligente del control TreeView como se indica a continuación.

![](data-bound-controls/_static/image1.jpg)

**Figura 1**

Al seleccionar la opción de menú **Personalizar imágenes** de línea, se iniciará la herramienta Diseñador de líneas, lo que le permitirá configurar la apariencia de las líneas TreeView.

## <a name="treeview-events"></a>Eventos TreeView

El treeView control tiene los siguientes eventos únicos:

- SelectedNodeChanged se produce cuando se selecciona un nodo en función de la **SelectAction** propiedad.
- TreeNodeCheckChanged se produce cuando se cambia el estado de las casillas de verificación de un nodo.
- TreeNodeExpanded se produce cuando se expande un nodo en función de la **SelectAction** propiedad.
- TreeNodeCollapsed se produce cuando se contrae un nodo.
- TreeNodeDataBound se produce cuando un nodo está enlazado a datos.
- TreeNodePopulate se produce cuando se rellena un nodo.

La propiedad **SelectAction** permite configurar qué evento se desencadena cuando se selecciona un nodo. La propiedad SelectAction proporciona las siguientes acciones:

- TreeNodeSelectAction.Expand eleva TreeNodeExpanded cuando se selecciona el nodo.
- TreeNodeSelectAction.None No provoca ningún evento cuando se selecciona el nodo.
- TreeNodeSelectAction.Select provoca el evento SelectedNodeChanged cuando se selecciona el nodo.
- TreeNodeSelectAction.SelectExpand provoca el evento SelectedNodeChanged y el evento TreeNodeExpanded cuando se selecciona el nodo.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

El TreeView control proporciona muchas propiedades para controlar la apariencia del control con estilos. Están disponibles las propiedades siguientes:

| **Nombre de la propiedad** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla el estilo de los nodos cuando el ratón se coloca sobre ellos. |
| LeafNodeStyle | Controla el estilo de los nodos hoja. |
| NodeStyle | Controla el estilo de todos los nodos. Estilos de nodo específicos (como LeafNodeStyle) invalidan este estilo. |
| ParentNodeStyle | Controla el estilo de todos los nodos primarios. |
| RootNodeStyle | Controla el estilo del nodo raíz. |
| SelectedNodeStyle | Controla el estilo del nodo seleccionado. |

Cada una de estas propiedades es de solo lectura. Sin embargo, cada uno de volverá un **TreeNodeStyle** objeto y las propiedades de ese objeto se pueden modificar mediante el formato *de propiedad-subpropiedad.* Por ejemplo, para establecer la propiedad **ForeColor** de **SelectedNodeStyle**, se utilizaría la sintaxis siguiente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Observe que la etiqueta anterior no está cerrada. Esto se debe a que al usar la sintaxis declarativa que se muestra aquí, también incluiría los nodos TreeViews en el código HTML.

Las propiedades de estilo también se pueden especificar en el código mediante el formato *property.subproperty.* Por ejemplo, para establecer la propiedad **ForeColor** de **RootNodeStyle** en el código, utilice la sintaxis siguiente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obtener una lista completa de las diferentes propiedades de estilo, consulte la documentación de MSDN en el TreeNodeStyle objeto.

## <a name="the-sitemappath-control"></a>El control SiteMapPath

El SiteMapPath control proporciona un control de navegación de mioja de pan para ASP.NET desarrolladores. Al igual que los otros controles de navegación, se pueden enlazar fácilmente a datos enlazados a orígenes de datos jerárquicos como SiteMapDataSource o XmlDataSource.

Un SiteMapPath control se compone de SiteMapNodeItem objetos. Hay tres tipos de nodos; el nodo raíz, los nodos primarios y el nodo actual. El nodo raíz es el nodo en la parte superior de la estructura jerárquica. El nodo actual representa la página actual. Todos los demás nodos son nodos primarios.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlar el funcionamiento del control SiteMapPath

Las propiedades que controlan el funcionamiento del control SiteMapPath son las siguientes:

| **Propiedad** | **Descripción de la propiedad** |
| --- | --- |
| ParentLevelsDisplayed | Controla cuántos nodos primarios se muestran. El valor predeterminado es -1, que no impone ninguna restricción al número de nodos primarios que se muestran. |
| PathDirection | Controla la dirección de SiteMapPath. Los valores válidos son RootToCurrent (valor predeterminado) y CurrentToRoot. |
| PathSeparator | Una cadena que controla el carácter que separa los nodos de un SiteMapPath control. El valor predeterminado es :. |
| RenderCurrentNodeAsLink | Valor booleano que controla si el nodo actual se representa como un vínculo. El valor predeterminado es False. |
| SkipLinkText | Ayuda con la accesibilidad cuando los lectores de pantalla ven la página. Esta propiedad permite a los lectores de pantalla omitir el SiteMapPath control. Para deshabilitar esta característica, establezca la propiedad en String.Empty. |

## <a name="templated-sitemappath-controls"></a>Controles SiteMapPath con plantillas

SiteMapControl es un control con plantilla y, como tal, puede definir diferentes plantillas para su uso en la visualización del control. Para editar plantillas en un control SiteMapPath, haga clic en el botón Etiqueta inteligente del control y elija Editar plantillas en el menú. Esto muestra el menú SiteMapTasks como se muestra a continuación, donde puede elegir entre las diferentes plantillas disponibles.

![](data-bound-controls/_static/image2.jpg)

**Figura 2**

La plantilla **NodeTemplate** hace referencia a cualquier nodo de SiteMapPath. Si el nodo es un nodo raíz o el nodo actual y se configura **RootNodeTemplate** o **CurrentNodeTemplate,** nosetemplate.

## <a name="sitemappath-events"></a>Eventos SiteMapPath

El SiteMapPath control tiene dos eventos que no se derivan de la Control clase; el evento **ItemCreated** y el evento **ItemDataBound.** El evento ItemCreated se genera cuando se crea un elemento SiteMapPath. ItemDataBound se produce cuando el DataBind se llama al método durante el enlace de datos de un SiteMapPath nodo. Un **SiteMapNodeItemEventArgs** objeto proporciona acceso a la específica SiteMapNodeItem a través de la Item propiedad.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

Los siguientes estilos están disponibles para dar formato a un SiteMapPath control.

| **Nombre de la propiedad** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla el estilo del texto del nodo actual. |
| RootNodeStyle | Controla el estilo del texto del nodo raíz. |
| NodeStyle | Controla el estilo del texto para todos los nodos suponiendo que un CurrentNodeStyle o RootNodeStyle no se aplica. |

El NodeStyle propiedad se reemplaza por el CurrentNodeStyle o el RootNodeStyle. Cada una de estas propiedades es de solo lectura y devuelve un **Style** objeto. Para afectar a la apariencia de un nodo mediante una de estas propiedades, deberá establecer las propiedades del objeto Style que se devuelve. Por ejemplo, el código siguiente cambia la propiedad forecolor del nodo actual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propiedad también se puede aplicar mediante programación de la siguiente manera:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si se aplica una plantilla, el estilo no se aplicará.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Lab 1: Configuración de un control de menú ASP.NET

1. Crear un sitio web nuevo.
2. Agregue un archivo de mapa del sitio seleccionando Archivo, Nuevo, Archivo y elija Mapa del sitio en la lista de plantillas de archivo.
3. Abra el mapa del sitio (Web.sitemap de forma predeterminada) y modifíquelo para que se parezca a la lista siguiente. Las páginas a las que está enlazando en el archivo de mapa del sitio no existen realmente, pero eso no será un problema para este ejercicio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra el formulario Web predeterminado en la vista Diseño.
5. En la sección Navegación del Cuadro de herramientas, agregue un nuevo control Menu a la página.
6. En la sección Datos del cuadro de herramientas, agregue un nuevo SiteMapDataSource. SiteMapDataSource usará automáticamente el archivo Web.sitemap en su sitio. (El archivo Web.sitemap *debe* estar en la carpeta raíz del sitio.)
7. Haga clic en el control Menú y, a continuación, haga clic en el botón Etiqueta inteligente para mostrar el cuadro de diálogo Tareas de menú.
8. En el menú desplegable Elegir origen de datos, seleccione SiteMapDataSource1.
9. Haga clic en el vínculo Autoformato y elija un formato para el menú.
10. En el panel Propiedades, establezca la propiedad **StaticDisplayLevels** en 2. El control Menu ahora debe mostrar el nodo Inicio, Productos y Servicios en el Diseñador.
11. Navegue por la página en su navegador para utilizar el menú. (Debido a que las páginas que ha agregado al mapa del sitio no existen realmente, verá un error cuando intente buscarlas.)

Experimente con el cambio de la StaticDisplayLevels y el MaximumDynamicDisplayLevels propiedades y ver cómo afectan a cómo se representa el menú.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Lab 2: Enlace dinámico de un control TreeView

En este ejercicio se supone que tiene SQL ServerSQL Server ejecutándose localmente y que la base de datos Northwind está presente en la instancia de SQL ServerSQL Server . Si no se cumplen estas condiciones, cambie la cadena de conexión en el ejemplo. Tenga en cuenta que es posible que también deba especificar la autenticación de SQL Server en lugar de una conexión de confianza.

1. Crear un sitio web nuevo.
2. Cambie a la vista Código para Default.aspx y reemplace todo el código por el código que se muestra a continuación. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Guarde la página como treeview.aspx.
4. Examine la página.
5. Cuando se muestre la página por primera vez, vea el origen de la página en el explorador. Tenga en cuenta que solo los nodos visibles se enviaron al cliente.
6. Haga clic en el signo más situado junto a cualquier nodo.
7. Vuelva a ver el origen en la página. Observe que los nodos recién mostrados están ahora presentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3: Details View and Editing Data Using a GridView and DetailsView

1. Crear un sitio web nuevo.
2. Agregue un nuevo web.config al sitio Web.
3. Agregue una cadena de conexión al archivo web.config como se muestra a continuación: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Es posible que deba cambiar la cadena de conexión en función de su entorno.
4. Guarde y cierre el archivo web.config.
5. Abra Default.aspx y agregue un nuevo control SqlDataSource.
6. Cambie el identificador del control SqlDataSource a **Products**.
7. En el menú **Tareas de SqlDataSource** , haga clic en **Configurar origen de datos**.
8. Seleccione **Northwind** en el menú desplegable de conexión y haga clic en Siguiente.
9. Seleccione **Productos** en la lista desplegable **Nombre** y marque las casillas **ProductID**, **ProductName**, **UnitPrice**y **UnitsInStock** como se muestra a continuación. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Haga clic en **Next**.
11. Haga clic en **Finalizar**
12. Cambie a la vista Origen y examine el código que se generó. Observe el **SelectCommand**, **DeleteCommand**, **InsertCommand**y **UpdateCommand** que se agregaron al control SqlDataSource . Observe también los parámetros que se agregaron.
13. Cambie a la vista Diseño y agregue un nuevo control GridView a la página.
14. Seleccione **Productos** en el menú desplegable Elegir origen de **datos.**
15. Marque **la paginación del permiso** y habilite la **selección** tal y como se muestra abajo. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Haga clic en el vínculo **Editar columnas** y asegúrese de que la opción Generar **automáticamente campos** está activada.
17. Haga clic en **Aceptar**.
18. Con el control GridView seleccionado, haga clic en el botón situado junto a la **DataKeyNames** propiedad en el panel Propiedades.
19. Seleccione **ProductID** en la lista Campos **&gt;** de datos **disponibles** y haga clic en el botón para agregarlo.
20. Haga clic en Aceptar.
21. Agregue un nuevo control SqlDataSource a la página.
22. Cambie el identificador del control SqlDataSource a **Details**.
23. En el menú Tareas de SqlDataSource , elija **Configurar origen de datos**.
24. Elija **Northwind** en el menú desplegable y haga clic en **Next (Siguiente).**
25. Seleccione <strong>Productos</strong> en el menú <strong> \<desplegable <strong>Nombre</strong> y marque la casilla /strong>* en el cuadro de lista <strong>Columnas.</strong>
26. Haga clic en el botón **WHERE.**
27. Seleccione **ProductID** en la lista desplegable **Columna.**
28. Seleccione **=** en el menú desplegable Operador.
29. Seleccione **Control** en el menú desplegable **Origen.**
30. Seleccione **GridView1** en el panel desplegable **ID** de control.
31. Haga clic en el botón **Agregar** para agregar la cláusula WHERE.
32. Haga clic en **Aceptar**.
33. Haga clic en el botón **Avanzadas** y active la casilla **Generar instrucciones INSERT, UPDATE y DELETE.**
34. Haga clic en **Aceptar**.
35. Haga clic en **Siguiente** y haga clic en **Finalizar**.
36. Agregue un control DetailsView a la página.
37. En el menú desplegable **Choose Data Source (Elegir origen** de datos), elija Details **(Detalles).**
38. Marque la casilla **de verificación de** la edición del permiso como se muestra abajo. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Guarde la página y examine Default.aspx.
40. Haga clic en el vínculo **Seleccionar** junto a diferentes registros para ver la actualización de DetailsView automáticamente.
41. Haga clic en **el** editar vínculo en el DetailsView control.
42. Realice un cambio en el registro y haga clic en **Actualizar**.
