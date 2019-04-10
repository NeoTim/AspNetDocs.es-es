---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Insertar un nuevo registro desde el pie de página de GridView (VB) | Microsoft Docs
author: rick-anderson
description: Mientras el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, en este tutorial se muestra cómo aumentar el control GridView para incluir un...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 251cd769672f1610ac7c51772882b0c166184372
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59397440"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Insertar un nuevo registro desde el pie de página de GridView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) o [descargar PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Mientras el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, en este tutorial se muestra cómo aumentar el control GridView para incluir una interfaz de inserción.


## <a name="introduction"></a>Introducción

Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, los controles GridView, DetailsView y FormView Web incluyen capacidades de modificación de datos integrados. Cuando se usa con controles de origen de datos declarativo, el estos tres controles Web pueden configurarse de forma rápida y sencilla para modificar datos y en escenarios sin necesidad de escribir una sola línea de código. Desafortunadamente, los controles DetailsView y FormView ofrecen integrados Insertar, editar y eliminar capacidades. GridView sólo ofrece la edición y eliminación de soporte técnico. Sin embargo, con un poco codo grasa, podemos aumentar el control GridView para incluir una interfaz de inserción.

Agregar funciones de inserción en el control GridView, somos responsables de decidir cómo los registros nuevos se agregará, creación de la interfaz de inserción y escribir el código para insertar el nuevo registro. En este tutorial que se examinará la adición de la interfaz de inserción al pie de página de GridView s filas (consulte la figura 1). La celda de pie de página para cada columna incluye el elemento de interfaz de usuario de recopilación de datos correspondiente (un cuadro de texto para el nombre del producto s, DropDownList para el proveedor y así sucesivamente). También necesitamos una columna para agregar botón que, al hacer clic en, se producen un postback e insertar un nuevo registro en el `Products` utilizando los valores proporcionados en la fila de pie de tabla.


[![TFila de pie de página proporciona una interfaz para agregar nuevos productos](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: La fila de pie de página proporciona una interfaz para agregar nuevos productos ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Paso 1: Muestra información del producto en un control GridView

Antes de que se refieren a nosotros mismos con la creación de la interfaz de inserción en el pie de página de GridView s, permiten centrarnos primero de s acerca de cómo agregar un control GridView a la página que se enumera los productos en la base de datos. Comience abriendo la `InsertThroughFooter.aspx` página en el `EnhancedGridView` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, configurando la s GridView `ID` propiedad `Products`. A continuación, use la etiqueta inteligente de GridView s para enlazarla a un nuevo origen ObjectDataSource denominado `ProductsDataSource`.


[![Ccrear un nuevo ProductsDataSource denominado de ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figura 2**: Crear un nuevo origen ObjectDataSource denominado `ProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase s `GetProducts()` método para recuperar información del producto. Para este tutorial, permiten s centrarse estrictamente agregar funciones de inserción y no preocuparse acerca de la edición y eliminación. Por lo tanto, asegúrese de que se establece la lista desplegable en la pestaña Insertar en `AddProduct()` y que las listas desplegables en las fichas de UPDATE y DELETE se establecen en (ninguno).


[![Mel método AddProduct al método ObjectDataSource s Insert() AP](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figura 3**: Mapa del `AddProduct` método al origen ObjectDataSource s `Insert()` método ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Set las listas desplegables de fichas DELETE y UPDATE (ninguna)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figura 4**: Establezca la actualización y listas desplegables de las pestañas eliminar (ninguno) ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Después de completar al Asistente para configurar origen de datos de origen ObjectDataSource s, Visual Studio agregará automáticamente los campos en el control GridView para cada uno de los campos de datos correspondiente. Por ahora, deje todos los campos agregados por Visual Studio. Más adelante en este tutorial, se deberá volver y quitar algunos de los campos cuyos valores don t deben especificarse cuando se agrega un nuevo registro.

Dado que hay cerca de 80 productos en la base de datos, un usuario tendrá que desplazarse hasta la parte inferior de la página web con el fin de agregar un nuevo registro. Por lo tanto, permiten s habilita la paginación hacer la interfaz de inserción más visible y accesible. Para activar la paginación, simplemente marque la casilla de verificación Habilitar la paginación de la etiqueta inteligente de s GridView.

En este momento, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![All que campos de datos de productos se muestran en un control GridView paginado](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Figura 5**: Se muestran todos los campos de datos de producto en un control GridView paginado ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Paso 2: Agregar una fila de pie de página

Junto con su encabezado y las filas de datos, el control GridView incluye una fila de pie de página. Las filas de encabezado y pie de página se muestran según los valores de las operaciones de asignación GridView [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) y [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) propiedades. Para mostrar la fila de pie de página, basta con establecer la `ShowFooter` propiedad `True`. Como se muestra en la figura 6, establecer el `ShowFooter` propiedad `True` agrega una fila de pie de página a la cuadrícula.


[![TMostrar o la fila de pie de página, establezca ShowFooter en True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Figura 6**: Para mostrar la fila de pie de página, establezca `ShowFooter` a `True` ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Tenga en cuenta que la fila de pie de página tiene un color de fondo de color rojo oscuro. Esto ocurre con el tema DataWebControls se crean y se aplican a todas las páginas en el [mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial. En concreto, el `GridView.skin` archivo configura el `FooterStyle` propiedad tal que usa el `FooterStyle` clase CSS. El `FooterStyle` clase se define en `Styles.css` como sigue:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Se ve al explorar el uso de la fila de pie de página de GridView s en tutoriales anteriores. Si es necesario, vuelva a consultar el [mostrar información de resumen en el pie de página de GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial para ponerse al día.


Después de establecer el `ShowFooter` propiedad `True`, dedique un momento para ver la salida en un explorador. Actualmente la t de fila de pie de página contiene cualquier texto o controles Web. En el paso 3 se modificará el pie de página para cada campo de GridView para que incluya la interfaz adecuada de inserción.


[![TFila de pie de página vacía es muestran encima de la paginación controles de interfaz](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figura 7**: La fila de pie de página vacía es muestran encima de la paginación controles de interfaz ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Paso 3: Personalización de la fila de pie de página

En el [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial hemos visto cómo personalizar en gran medida la presentación de una determinada columna de GridView, uso de TemplateFields (en lugar de BoundFields o CheckBoxFields); en [ Personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) analizamos el uso de TemplateFields para personalizar la interfaz de edición en un control GridView. Recuerde que TemplateField se compone de una serie de plantillas que define la combinación de marcado, los controles Web y sintaxis de enlace de datos había usado con ciertos tipos de filas. El `ItemTemplate`, por ejemplo, especifica la plantilla utilizada para las filas de solo lectura, mientras que el `EditItemTemplate` define la plantilla para la fila editable.

Junto con el `ItemTemplate` y `EditItemTemplate`, TemplateField también incluye un `FooterTemplate` que especifica el contenido de la fila de pie de página. Por lo tanto, podemos agregar los controles Web necesarios para cada campo s insertar interfaz en el `FooterTemplate`. Para empezar, convertir todos los campos en el control GridView en TemplateFields. Esto puede hacerse el vínculo Editar columnas en el control GridView s etiquetas inteligentes, al seleccionar cada campo en la esquina inferior izquierda y haga clic en la función Convert este campo en un vínculo TemplateField.


![Convierte cada campo en TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figura 8**: Convierte cada campo en TemplateField


Al hacer clic en la función Convert este campo en TemplateField convierte el tipo de campo actual en un TemplateField equivalente. Por ejemplo, se reemplaza cada BoundField por TemplateField con un `ItemTemplate` que contiene una etiqueta que muestra el campo de datos correspondiente y un `EditItemTemplate` que muestra el campo de datos en un cuadro de texto. El `ProductName` BoundField se ha convertido en el siguiente marcado TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Del mismo modo, el `Discontinued` CampoCasillaVerificación se ha convertido en TemplateField cuya `ItemTemplate` y `EditItemTemplate` contiene un control de casilla de verificación Web (con el `ItemTemplate` s casilla deshabilitada). Solo lectura `ProductID` BoundField se ha convertido en TemplateField con un control de etiqueta tanto en el `ItemTemplate` y `EditItemTemplate`. En resumen, convertir un GridView existente en TemplateField es una manera rápida y sencilla para cambiar a TemplateField más personalizable sin perder la funcionalidad de campo s existente.

Desde el control GridView se está trabajando edición de soporte técnico de t, no dude en quitar el `EditItemTemplate` desde cada TemplateField, dejando sólo los `ItemTemplate`. Una vez hecho esto, el marcado declarativo de GridView s debe ser similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Ahora que cada campo del control GridView se ha convertido en TemplateField, podemos escribir la interfaz adecuada de inserción en cada campo s `FooterTemplate`. Algunos de los campos no tendrá una interfaz de inserción (`ProductID`, por ejemplo); otros usuarios varían en los controles Web que se usa para recopilar la información del producto s nuevo.

Para crear la interfaz de edición, elija el vínculo Editar plantillas de la etiqueta inteligente de s GridView. A continuación, en la lista desplegable, seleccione el campo apropiado s `FooterTemplate` y arrastre el control adecuado desde el cuadro de herramientas hasta el diseñador.


[![Ala interfaz Inserting adecuada para cada campo s FooterTemplate dd](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figura 9**: Agregar la interfaz adecuada de inserción para cada campo s `FooterTemplate` ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


La lista con viñetas siguiente enumera los campos de GridView, especificar la interfaz de inserción para agregar:

- `ProductID` Ninguno.
- `ProductName` Agregue un cuadro de texto y establezca su `ID` a `NewProductName`. Agregue un control RequiredFieldValidator para asegurarse de que el usuario escribe un valor para el nuevo nombre de producto s.
- `SupplierID` Ninguno.
- `CategoryID` Ninguno.
- `QuantityPerUnit` Agregar un cuadro de texto, establecer su `ID` a `NewQuantityPerUnit`.
- `UnitPrice` Agregar un cuadro de texto denominado `NewUnitPrice` y un control CompareValidator que garantiza que el valor especificado es un valor de moneda mayor o igual que cero.
- `UnitsInStock` Use un cuadro de texto cuyo `ID` está establecido en `NewUnitsInStock`. Incluir un CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual que cero.
- `UnitsOnOrder` Use un cuadro de texto cuyo `ID` está establecido en `NewUnitsOnOrder`. Incluir un CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual que cero.
- `ReorderLevel` Use un cuadro de texto cuyo `ID` está establecido en `NewReorderLevel`. Incluir un CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual que cero.
- `Discontinued` Agregar un control CheckBox, establecer su `ID` a `NewDiscontinued`.
- `CategoryName` Agregue un DropDownList y establezca su `ID` a `NewCategoryID`. Enlazarlo a un nuevo origen ObjectDataSource denominado `CategoriesDataSource` y configúrelo para utilizar el `CategoriesBLL` clase s `GetCategories()` método. Tiene la s DropDownList `ListItem` mostrar s el `CategoryName` datos de campo, utilizando el `CategoryID` campo de datos como sus valores.
- `SupplierName` Agregue un DropDownList y establezca su `ID` a `NewSupplierID`. Enlazarlo a un nuevo origen ObjectDataSource denominado `SuppliersDataSource` y configúrelo para utilizar el `SuppliersBLL` clase s `GetSuppliers()` método. Tiene la s DropDownList `ListItem` mostrar s el `CompanyName` datos de campo, utilizando el `SupplierID` campo de datos como sus valores.

Para cada uno de los controles de validación, borre el `ForeColor` propiedad para que el `FooterStyle` color de primer plano blanco s de la clase CSS que se usará en lugar del predeterminado en rojo. Use también el `ErrorMessage` propiedad para obtener una descripción detallada, pero establece el `Text` propiedad en un asterisco. Para evitar que el texto del control s validación causando la interfaz de inserción para que se ajusten a dos líneas, establezca el `FooterStyle` s `Wrap` propiedad en false para cada uno de los `FooterTemplate` que utilizan un control de validación. Por último, agregue un control ValidationSummary bajo el control GridView y establezca su `ShowMessageBox` propiedad `True` y su `ShowSummary` propiedad `False`.

Al agregar un nuevo producto, se debe proporcionar el `CategoryID` y `SupplierID`. Esta información se captura a través de las listas desplegables en las celdas de pie de página para el `CategoryName` y `SupplierName` campos. He optado por usar estos campos en contraposición a la `CategoryID` y `SupplierID` TemplateFields porque en la cuadrícula s filas de datos del usuario es probablemente más interesado en ver los nombres de categoría y proveedor en lugar de los valores de identificador. Puesto que la `CategoryID` y `SupplierID` ahora se capturan los valores en el `CategoryName` y `SupplierName` interfaces Insertar s de campo, podemos quitar el `CategoryID` y `SupplierID` TemplateFields en el control GridView.

De forma similar, el `ProductID` no se utiliza cuando se agrega un nuevo producto, por lo que el `ProductID` TemplateField también se puede quitar. Sin embargo, permitan s abandonar el `ProductID` campo en la cuadrícula. Además de los cuadros de texto, listas desplegables, casillas y los controles de validación que componen la interfaz de inserción, necesitaremos también una Agregar botón que, al hacer clic, lleva a cabo la lógica para agregar el nuevo producto a la base de datos. En el paso 4, se debe incluir un botón Add en la interfaz de inserción en el `ProductID` TemplateField s `FooterTemplate`.

No dude en mejorar la apariencia de los distintos campos de GridView. Por ejemplo, desea dar formato a la `UnitPrice` valores como valores de moneda, Alinear a la derecha el `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` campos y actualizar el `HeaderText` valores para el TemplateFields.

Después de crear el montón de la inserción de las interfaces en el `FooterTemplate` s, quitando el `SupplierID`, y `CategoryID` TemplateFields y mejorar la estética de la cuadrícula a través de formato y la alineación del TemplateFields, la s GridView declarativa marcado debe tener un aspecto similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Cuando se ve mediante un explorador, la fila de pie de página de GridView s ahora incluye completado insertar interfaz (consulte la figura 10). En este momento, la inserción t de interfaz son un medio para que el usuario indicar que s escribió los datos para el nuevo producto y desea insertar un nuevo registro en la base de datos. Además, se ve aún para abordar cómo los datos introducidos en el pie de página se traducirán en un nuevo registro en el `Products` base de datos. En el paso 4, veremos cómo incluir un botón Add a la interfaz de inserción y cómo ejecutar código en el postback cuando lo s que se hizo clic. Paso 5 muestra cómo insertar un nuevo registro con los datos en el pie de página.


[![Tpie de página de GridView proporciona una interfaz para agregar un nuevo registro](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figura 10**: El pie de página de GridView proporciona una interfaz para agregar un nuevo registro ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Paso 4: Incluir un botón Add en la interfaz de inserción

Es necesario incluir un botón Add en algún lugar en la interfaz de inserción porque la s de la fila de pie de página Insertar interfaz actualmente no tiene los medios para el usuario indicar que termine de escribir la nueva información de producto s. Esto se podría colocar en uno de los existentes `FooterTemplate` s o nos podríamos agregar una nueva columna a la cuadrícula para este propósito. Para este tutorial, s permiten colocar en el botón Agregar el `ProductID` TemplateField s `FooterTemplate`.

En el diseñador, haga clic en el vínculo Editar plantillas en la etiqueta inteligente de GridView s y, a continuación, elija el `ProductID` campo s `FooterTemplate` en la lista desplegable. Agregar un control de botón Web (o un LinkButton o ImageButton, si lo prefiere) a la plantilla, estableciendo su ID `AddProduct`, sus `CommandName` para Insert y su `Text` propiedad para agregar, como se muestra en la figura 11.


[![Place el botón Agregar en la s ProductID TemplateField FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figura 11**: Coloque el botón Agregar en el `ProductID` TemplateField s `FooterTemplate` ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Una vez que ha había incluido el botón Agregar, pruebe la página en un explorador. Tenga en cuenta que al hacer clic en el botón Agregar con datos no válidos en la interfaz de inserción, la devolución de datos se resumen de circuito y el control ValidationSummary indica que los datos no válidos (consulte la figura 12). Con los datos apropiados especificados, al hacer clic en el botón Agregar, se produce un postback. No hay ningún registro se agrega a la base de datos, sin embargo. Deberá escribir un fragmento de código para llevar a cabo la inserción.


[![TBotón Agregar s Postback es corto de circuito si no hay datos no válidos en la interfaz de inserción](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figura 12**: La s de botón Agregar Postback es corto de circuito si no hay datos no válidos en la interfaz de inserción ([haga clic aquí para ver imagen en tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Los controles de validación en la interfaz de inserción no se han asignado a un grupo de validación. Esto funciona bien siempre y cuando la interfaz de inserción es el único conjunto de controles de validación en la página. Si, sin embargo, hay otros controles de validación en la página (por ejemplo, los controles de validación en la interfaz de edición de cuadrícula s), los controles de validación en la inserción de la interfaz y agregar botón s `ValidationGroup` propiedades se deben asignar el mismo valor de fin asociar estos controles a un grupo de validación determinado. Consulte [Disecar los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obtener más información acerca de las particiones de los botones en una página y controles de validación en los grupos de validación.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Paso 5: Insertar un nuevo registro en el`Products`tabla

Al utilizar las características de edición integradas del control GridView, el control GridView controla automáticamente todo el trabajo necesario para realizar la actualización. En concreto, cuando se hace clic en el botón Actualizar copia los valores especificados de la interfaz de edición para los parámetros en las operaciones de asignación ObjectDataSource `UpdateParameters` colección e inicia la actualización invocando la s ObjectDataSource `Update()` método. Puesto que el control GridView no proporciona esta funcionalidad integrada para insertar, debemos implementar código que llama a la s ObjectDataSource `Insert()` método y copia los valores de la inserción de la interfaz para el s ObjectDataSource `InsertParameters` colección .

Esta lógica de inserción se debe ejecutar después de que se ha hecho clic el botón Agregar. Como se describe en el [agregar y responder a los botones en un control GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) tutorial, cada vez que un botón, LinkButton o ImageButton en un control GridView se hace clic en, la s GridView `RowCommand` desencadena el evento en el postback. Este evento se desencadena si el botón, LinkButton o ImageButton se ha agregado explícitamente como el botón Agregar en la fila de pie de página o si se agregó automáticamente por el control GridView (por ejemplo, el LinkButtons en la parte superior de cada columna cuando se selecciona la opción Enable Sorting, o LinkButtons en la interfaz de paginación cuando se selecciona Habilitar paginación).

Por lo tanto, para responder al usuario hacer clic en el botón Agregar, es necesario crear un controlador de eventos para el s GridView `RowCommand` eventos. Puesto que este evento se desencadena cada vez que *cualquier* se hace clic en Button, LinkButton o ImageButton en GridView, lo esencial que nos continuar solo con la lógica de inserción si s el `CommandName` propiedad pasa a las asignaciones de controlador de eventos para el `CommandName` valor del botón Agregar (Insertar). Además, debemos también solo continuamos si informan de los controles de validación de datos válidos. Para dar cabida a esto, cree un controlador de eventos para el `RowCommand` evento con el código siguiente:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Quizás se pregunte por qué el controlador de eventos molesta comprobando el `Page.IsValid` propiedad. ¿Después de todo, no se pueden suprimir la devolución de datos si se proporcionan los datos no válidos en la interfaz de inserción? Esta suposición es correcta, siempre que el usuario no ha deshabilitado JavaScript o ha tomado medidas para evitar la lógica de validación del lado cliente. En resumen, uno nunca debería basarse estrictamente en la validación del lado cliente; siempre debe realizarse una comprobación del lado servidor para validez antes de trabajar con los datos.


En el paso 1 se creó el `ProductsDataSource` ObjectDataSource tal que su `Insert()` método se asigna a la `ProductsBLL` clase s `AddProduct` método. Para insertar el nuevo registro en el `Products` tabla, que podemos llamar simplemente la s ObjectDataSource `Insert()` método:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Ahora que la `Insert()` ha invocado al método, lo único que queda es copiar los valores de la interfaz de inserción a los parámetros pasados a la `ProductsBLL` clase s `AddProduct` método. Como vimos en el [examinando los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial, esto puede realizarse a través de las operaciones de asignación ObjectDataSource `Inserting` eventos. En el `Inserting` eventos que necesitamos hacer referencia mediante programación a los controles de la `Products` pie de página de GridView s de fila y asignar los valores para el `e.InputParameters` colección. Si el usuario la omita un valor como dejando la `ReorderLevel` en blanco del cuadro de texto se debe especificar que el valor insertado en la base de datos debe ser `NULL`. Puesto que la `AddProducts` método acepta los tipos que aceptan valores NULL para los campos que aceptan valores NULL de la base de datos, basta con usar un tipo que acepta valores NULL y establezca su valor en `Nothing` en el caso de que se omita la entrada del usuario.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Con el `Inserting` completado un controlador de eventos, se pueden agregar nuevos registros a la `Products` tabla de base de datos a través de la fila de pie de página de GridView s. Siga adelante e intente agregar varios productos nuevos.

## <a name="enhancing-and-customizing-the-add-operation"></a>Mejorar y personalizar la operación de adición

Actualmente, al hacer clic en el botón Agregar agrega un nuevo registro a la tabla de base de datos pero no proporciona a ningún tipo de comentarios visuales que el registro se ha agregado correctamente. Idealmente, un cuadro de alerta de control o de cliente de Web de la etiqueta podría informar al usuario que se ha completado su inserción con éxito. Dejar como un ejercicio para el lector.

GridView usada en este tutorial no aplica ningún criterio de ordenación para los productos enumerados, ni tampoco permite al usuario final ordenar los datos. Por lo tanto, los registros se ordenan tal como están en la base de datos por su campo de clave principal. Puesto que cada nuevo registro tiene un `ProductID` valor mayor que la última de ellas, cada vez que se agrega un nuevo producto se ha agregado al final de la cuadrícula. Por lo tanto, es posible que desee enviar automáticamente al usuario a la última página del control GridView después de agregar un nuevo registro. Esto puede realizarse agregando la siguiente línea de código después de llamar a `ProductsDataSource.Insert()` en el `RowCommand` controlador de eventos para indicar que el usuario debe enviarse a la última página después de enlazar los datos en GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` es una variable booleana de nivel de página que inicialmente está asignada el valor `False`. En la s GridView `DataBound` controlador de eventos, si `SendUserToLastPage` es false, el `PageIndex` propiedad se actualiza para enviar al usuario a la última página.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

La razón por la `PageIndex` propiedad está establecida el `DataBound` controlador de eventos (en contraposición al `RowCommand` controlador de eventos) es porque cuando el `RowCommand` controlador de eventos se activa se ve aún para agregar el nuevo registro a la `Products` tabla de base de datos. Por lo tanto, en el `RowCommand` el último índice de la página del controlador de eventos (`PageCount - 1`) representa el último índice de página *antes* se ha agregado el nuevo producto. Para la mayoría de los productos que se va a agregar el último índice de página es el mismo después de agregar el nuevo producto. Pero cuando el producto se ha agregado como resultado en un nuevo índice de página último, si se actualiza correctamente el `PageIndex` en el `RowCommand` controlador de eventos, a continuación, se le dirigirá a la segunda a la última página (el último índice de página antes de agregar el nuevo producto) en lugar de la última página nueva i ndice. Puesto que la `DataBound` controlador de eventos se activa una vez que se ha agregado el nuevo producto y enlazar los datos a la cuadrícula, estableciendo el `PageIndex` existe propiedad sabemos, sacando el último índice de página correcto.

Finalmente, el control GridView usado en este tutorial es bastante amplia debido al número de campos que deben recopilarse para agregar un nuevo producto. Debido a este ancho, un diseño vertical de DetailsView s podría ser preferido. Las operaciones de asignación GridView ancho global podría reducirse mediante la recopilación de entradas menos. Tal vez no vamos t necesidad de recopilar la `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos cuando se agrega un nuevo producto, en cuyo caso se podrían quitar estos campos de GridView.

Para ajustar los datos recopilados, podemos usar uno de estos dos enfoques:

- Seguir usando el `AddProduct` método que espera valores para el `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos. En el `Inserting` controlador de eventos, proporcionar codificado de forma rígida, que se usará para estas entradas que se han quitado de la interfaz de insertar los valores predeterminados.
- Crear una nueva sobrecarga de la `AddProduct` método en el `ProductsBLL` clase que no acepta entradas para el `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos. A continuación, en la página ASP.NET, configure el origen ObjectDataSource para usar esta nueva sobrecarga.

Cualquiera de las opciones funcionará igual de bien. En más allá de los tutoriales se usa esta última opción, crear varias sobrecargas para el `ProductsBLL` clase s `UpdateProduct` método.

## <a name="summary"></a>Resumen

El control GridView carece de las capacidades integradas de inserción que se encuentra en el DetailsView y FormView, pero con un poco de esfuerzo de una interfaz de inserción se puede agregar a la fila de pie de página. Para mostrar la fila de pie de página en un control GridView simplemente establece su `ShowFooter` propiedad `True`. El contenido de la fila de pie de página se puede personalizar para cada campo mediante la conversión del campo en TemplateField y agregar la inserción de la interfaz para el `FooterTemplate`. Como hemos visto en este tutorial, el `FooterTemplate` puede contener botones, cuadros de texto, listas desplegables, casillas, controles de origen de datos para rellenar los controles de Web controlada por datos (por ejemplo, listas desplegables) y los controles de validación. Junto con los controles para recopilar la entrada del usuario s, se necesita un botón de agregar, LinkButton o ImageButton.

Cuando se presiona el botón Agregar, la s ObjectDataSource `Insert()` se invoca el método para iniciar el flujo de trabajo de inserción. El origen ObjectDataSource, a continuación, llamará al método de inserción configurado (el `ProductsBLL` clase s `AddProduct` método, en este tutorial). Debemos copiaremos los valores de inserción de interfaz al origen ObjectDataSource s GridView s `InsertParameters` colección antes de que se invoca el método de inserción. Esto puede realizarse mediante la referencia mediante programación a los controles Web de interfaz insertar en la s ObjectDataSource `Inserting` controlador de eventos.

En este tutorial se completa nuestro aspecto en las técnicas para mejorar el aspecto de s GridView. El siguiente conjunto de tutoriales examinará los datos de los controles Web y de cómo trabajar con datos binarios tales como imágenes, archivos PDF, documentos de Word y así sucesivamente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Bernadette Leigh. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-vb.md)
