---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Maestro y detalles de filtrado a través de dos páginas (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores de la base de datos. Cada fila del proveedor en el control GridView contendrá una p...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 451f9f2698780650c32e453b78b11f6babed88de
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59405292"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrado de maestro y detalles en dos páginas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) o [descargar PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores de la base de datos. Cada fila del proveedor en el control GridView contendrá un vínculo Ver productos que, al hacer clic en, llevará al usuario a una página independiente enumeran los productos para el proveedor seleccionado.


## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores, vimos cómo [mostrar informes de maestro y detalles en una única página web mediante DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md) a [mostrar los registros de "maestros" y un control GridView o DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) para mostrar el " Detalles". Otro patrón común que se usa para los informes de maestro y detalles es que los registros maestros en una página web y los detalles que se muestra en otro. Un sitio Web del foro, como [los foros de ASP.NET](https://forums.asp.net/), es un buen ejemplo de este patrón en la práctica. Los foros de ASP.NET están formados por una variedad de foros de introducción, formularios Web Forms, controles de presentación de datos y así sucesivamente. Cada foro está formado por muchos subprocesos y cada subproceso se compone de un número de publicaciones. En la página principal de foros de ASP.NET, se enumeran los foros. Al hacer clic en un foro de inmediato a un `ShowForum.aspx` página, que enumera los subprocesos del foro. Del mismo modo, en un subproceso le llevará al `ShowPost.aspx`, que muestra las entradas para el subproceso que se hizo clic.

En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores de la base de datos. Cada fila del proveedor en el control GridView contendrá un vínculo Ver productos que, al hacer clic en, llevará al usuario a una página independiente enumeran los productos para el proveedor seleccionado.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Paso 1: Agregar`SupplierListMaster.aspx`y`ProductsForSupplierDetails.aspx`páginas a la`Filtering`carpeta

Al definir el diseño de página en el tercer tutorial hemos agregado una serie de páginas de "inicio" en el `BasicReporting`, `Filtering`, y `CustomFormatting` carpetas. Sin embargo, no agregamos una página de inicio para este tutorial en ese momento, así que Tómese un momento para agregar dos nuevas páginas a la `Filtering` carpeta: `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` mostrará una lista de los registros de "master" (los proveedores) mientras `ProductsForSupplierDetails.aspx` mostrará los productos para el proveedor seleccionado.

Al crear estas dos páginas nuevas estar seguro asociarlos con el `Site.master` página maestra.


![Agregue las páginas de ProductsForSupplierDetails.aspx y SupplierListMaster.aspx a la carpeta de filtrado](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figura 1**: Agregar el `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` páginas a la `Filtering` carpeta


Además, al agregar nuevas páginas al proyecto, asegúrese de actualizar el archivo de mapa del sitio, `Web.sitemap`, según corresponda. En este tutorial basta con agregar el `SupplierListMaster.aspx` página al mapa del sitio mediante el siguiente contenido XML como elemento secundario de los informes de filtrado `<siteMapNode>` elemento:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Puede ayudar a automatizar el proceso de actualizar el archivo de mapa del sitio al agregar el nuevo ASP.NET páginas mediante [K. Scott Allen](http://odetocode.com/Blogs/scott/)de Visual Studio gratis [macros de mapa del sitio](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Paso 2: Mostrar en la lista de proveedores`SupplierListMaster.aspx`

Con el `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` páginas creadas, el siguiente paso es crear el control GridView de proveedores en `SupplierListMaster.aspx`. Agregar un control GridView a la página y enlazarlo a un nuevo origen ObjectDataSource. Debe usar este origen ObjectDataSource el `SuppliersBLL` la clase `GetSuppliers()` método para devolver todos los proveedores.


[![Soptar por la clase SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figura 2**: Seleccione el `SuppliersBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Cconfigurar el origen ObjectDataSource para usar el método GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figura 3**: Configurar el origen ObjectDataSource que se usarán el `GetSuppliers()` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Es necesario incluir un vínculo titulado ver productos en cada fila GridView que, al hacer clic, lleva al usuario a `ProductsForSupplierDetails.aspx` pasando la fila seleccionada `SupplierID` valor a través de la cadena de consulta. Por ejemplo, si el usuario hace clic en el vínculo Ver productos para el proveedor Comercial Tasmania (que tiene un `SupplierID` valor de 4), se enviarán a `ProductsForSupplierDetails.aspx?SupplierID=4`.

Para ello, agregue un [campo HYPERLINK](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) en el control GridView, que agrega un hipervínculo para cada fila de GridView. Iniciar, haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView. A continuación, seleccione el campo HYPERLINK en la lista en la parte superior izquierda y haga clic en Agregar para incluir el campo HYPERLINK en la lista de campos de GridView.


[![Aun campo en el control GridView Hyperlink dd](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figura 4**: Agregar un campo HYPERLINK en GridView ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image10.png))


El campo HYPERLINK puede configurarse para usar el mismo texto o valores en cada fila GridView el vínculo dirección URL, o puede basar estos valores en los valores de datos enlazados a cada fila en particular. Para especificar una variable static valor en todas las filas, usar el campo de Hyperlink `Text` o `NavigateUrl` propiedades. Puesto que deseamos que el texto del vínculo a ser el mismo para todas las filas, establezca el campo de Hyperlink `Text` propiedad para ver los productos.


[![Set propiedad de texto el campo del Hyperlink para ver los productos](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figura 5**: Establezca el campo de Hyperlink `Text` propiedad para ver los productos ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Para establecer el texto o los valores de dirección URL debe basarse en los datos subyacentes que se enlaza a la fila GridView, especifique el texto de los datos de campos o valores de dirección URL deben extraerse de en el `DataTextField` o `DataNavigateUrlFields` propiedades. `DataTextField` solo puede establecerse en un único campo de datos; `DataNavigateUrlFields`, sin embargo, se puede establecer en una lista delimitada por comas de campos de datos. Con frecuencia es necesario basar el texto o la dirección URL en una combinación del valor del campo de datos de la fila actual y algún marcado estático. En este tutorial, por ejemplo, queremos que la dirección URL de vínculos el campo del Hyperlink sea `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, donde *`supplierID`* es la fila de cada GridView `SupplierID` valor. Observe que necesitamos estáticos y controladas por datos aquí los valores: el `ProductsForSupplierDetails.aspx?SupplierID=` parte de la dirección URL del vínculo es estático, mientras que el *`supplierID`* parte está controlada por datos como su valor es propio de cada fila `SupplierID` valor.

Para indicar una combinación de valores estáticos y controladas por datos, utilice el `DataTextFormatString` y `DataNavigateUrlFormatString` propiedades. En estas propiedades, escriba el marcado estático según sea necesario y, a continuación, utilice el marcador `{0}` donde desea que el valor del campo especificado en el `DataTextField` o `DataNavigateUrlFields` propiedades aparezcan. Si el `DataNavigateUrlFields` propiedad tiene uso especificado de varios campos `{0}` en la que desea que el primer valor del campo insertado, `{1}` para el segundo valor de campo y así sucesivamente.

Si se aplica a nuestro tutorial, debe establecer el `DataNavigateUrlFields` propiedad `SupplierID`, ya que es el campo de datos cuyo valor es necesario personalizar según cada fila, y el `DataNavigateUrlFormatString` propiedad a `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Cel campo HYPERLINK debe incluir la correcta vínculo URL basado tras el SupplierID onfigurar](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figura 6**: Configurar el campo HYPERLINK para incluir la adecuada vínculo URL basándose en la `SupplierID` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Después de agregar el campo HYPERLINK, no dude en Personalizar y volver a ordenar los campos de GridView. El marcado siguiente muestra el control GridView después de que he hecho algunas pequeñas personalizaciones de nivel de campo.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Dedique un momento para ver el `SupplierListMaster.aspx` página a través de un explorador. Como se muestra en la figura 7, la página actualmente muestran todos los proveedores con un vínculo Ver productos. Al hacer clic en la vista de los productos vínculo le llevará a `ProductsForSupplierDetails.aspx`, pasando a lo largo del proveedor `SupplierID` en la cadena de consulta.


[![EACH proveedor fila contiene un vínculo de productos de vista](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figura 7**: Cada fila del proveedor contiene un vínculo de productos de vista ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Paso 3: Lista de productos del proveedor en`ProductsForSupplierDetails.aspx`

En este momento la `SupplierListMaster.aspx` página está enviando a los usuarios `ProductsForSupplierDetails.aspx`, pasando el proveedor seleccionado `SupplierID` en la cadena de consulta. Paso final del tutorial es mostrar los productos en un control GridView en `ProductsForSupplierDetails.aspx` cuyo `SupplierID` es igual a la `SupplierID` pasan a través de la cadena de consulta. Para realizar este tutorial de inicio mediante la adición de un control GridView a la `ProductsForSupplierDetails.aspx` página mediante un control ObjectDataSource nuevo denominado `ProductsBySupplierDataSource` que invoca la `GetProductsBySupplierID(supplierID)` método desde el `ProductsBLL` clase.


[![Aun nuevo ProductsBySupplierDataSource denominado de ObjectDataSource dd](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figura 8**: Agregar un nuevo origen ObjectDataSource denominado `ProductsBySupplierDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Soptar por la clase ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figura 9**: Seleccione el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Hla invocación de ObjectDataSource el método GetProductsBySupplierID(supplierID) ave](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figura 10**: Que el ObjectDataSource invoque la `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image28.png))


El último paso del Asistente para configurar origen de datos nos pide que proporcione el origen de la `GetProductsBySupplierID(supplierID)` del método *`supplierID`* parámetro. Para usar el valor de cadena de consulta, establezca el origen de parámetro en la cadena de consulta y escriba el nombre del valor de cadena de consulta para usar en el cuadro de texto QueryStringField (`SupplierID`).


[![Polver a llenar el valor del parámetro del valor de cadena de consulta SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figura 11**: Rellenar el *`supplierID`* valor del parámetro de la `SupplierID` el valor de cadena de consulta ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Así de simple. Figura 12 se muestra el `ProductsForSupplierDetails.aspx` página cuando visita al hacer clic en el vínculo Comercial Tasmania desde `SupplierListMaster.aspx`.


[![Tse muestran, productos suministrados por Tokyo Traders](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figura 12**: Se muestran los productos suministrados por Tokyo Traders ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Mostrar información de proveedor en`ProductsForSupplierDetails.aspx`

Como se muestra en la figura 12, el `ProductsForSupplierDetails.aspx` página simplemente enumera los productos suministrados por el `SupplierID` especificado en la cadena de consulta. Alguien se envía directamente a esta página, sin embargo, no sabría que figura 12 muestra productos de Tokio Traders. Para solucionar este problema podemos mostrar información del proveedor en esta página también.

Empiece agregando un FormView por encima de los productos de GridView. Crear un nuevo control ObjectDataSource denominado `SuppliersDataSource` que invoca la `SuppliersBLL` la clase `GetSupplierBySupplierID(supplierID)` método.


[![Soptar por la clase SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figura 13**: Seleccione el `SuppliersBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Hla invocación de ObjectDataSource el método GetSupplierBySupplierID(supplierID) ave](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figura 14**: Que el ObjectDataSource invoque la `GetSupplierBySupplierID(supplierID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Igual que con el `ProductsBySupplierDataSource`, tiene la *`supplierID`* parámetro asignado el valor de la `SupplierID` el valor de cadena de consulta.


[![Polver a llenar el valor del parámetro del valor de cadena de consulta SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figura 15**: Rellenar el *`supplierID`* valor del parámetro de la `SupplierID` el valor de cadena de consulta ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Al enlazar FormView a ObjectDataSource en la vista Diseño, Visual Studio creará automáticamente la FormView `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate` con controles Web Label y TextBox para cada uno de los campos de datos devueltos por el ObjectDataSource. Puesto que queremos mostrar proveedores información si quiere, puede quitar el `InsertItemTemplate` y `EditItemTemplate`. A continuación, edite la plantilla ItemTemplate para que se muestre el nombre de la compañía del proveedor en un `<h3>` elemento y la dirección, ciudad, país y número de teléfono debajo del nombre de la empresa. Como alternativa, puede establecer manualmente el FormView `DataSourceID` y crear el `ItemTemplate` marcado, como hicimos en el "[mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" tutorial.

Después de estas modificaciones marcado declarativo de FormView debe ser similar al siguiente:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Figura 16 se muestra una captura de pantalla de la `ProductsForSupplierDetails.aspx` página después de que se ha incluido la información del proveedor detallada anteriormente.


[![TLista de productos incluye un resumen sobre el proveedor](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figura 16**: La lista de productos incluye un resumen sobre el proveedor ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Aplicar la última toca para el`ProductsForSupplierDetails.aspx`la interfaz de usuario

Para mejorar el usuario experiencia para este informe existe son un par de adiciones se debería hacer en el `ProductsForSupplierDetails.aspx` página. Actualmente la única manera de un usuario puede ir desde el `ProductsForSupplierDetails.aspx` página vuelva a la lista de proveedores es hacer clic en el botón Atrás del explorador. Vamos a agregar un control de hipervínculo para el `ProductsForSupplierDetails.aspx` página que el vínculo al `SupplierListMaster.aspx`, proporcionar otra forma para que el usuario que vuelva a la lista maestra.


[![Aun Control de hipervínculo para el usuario volver a tomar para SupplierListMaster.aspx dd](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figura 17**: Agregar un Control de hipervínculo para aprovechar el nuevo usuario a `SupplierListMaster.aspx` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Si el usuario hace clic en el vínculo Ver productos para un proveedor que no tiene ningún producto, la `ProductsBySupplierDataSource` ObjectDataSource en `ProductsForSupplierDetails.aspx` no devuelve ningún resultado. El control GridView enlazado a ObjectDataSource no representa ningún marcado resultante en un área en blanco en la página en el explorador del usuario. Podemos establecer la GridView para comunicarse con mayor claridad para el usuario que no hay productos asociados con el proveedor seleccionado `EmptyDataText` propiedad al mensaje que desea mostrar cuando se produzca esta situación. He establecido esta propiedad en "No hay productos suministrados por este proveedor"

De forma predeterminada, todos los proveedores en la base de datos Northwinds proporcionan al menos un producto. Sin embargo, para este tutorial manualmente modifiqué la `Products` para que el proveedor caracoles Nouveaux ya no está asociado con ningún producto de la tabla. Figura 18 se muestra la página de detalles para caracoles Nouveaux después de que se ha realizado este cambio.


[![Us se informaren de que el proveedor no ofrece ningún producto](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figura 18**: Los usuarios se les informa de que el proveedor no ofrece ningún producto ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Resumen

Mientras los informes de maestro y detalles pueden mostrar tanto los registros maestro como detalle en una sola página, en muchos sitios Web se dividen en dos páginas web. En este tutorial tratamos cómo implementar un informe de maestro y detalles haciendo que los proveedores enumerados en un control GridView en la página web "principal" y los productos asociados aparecen en la página "Detalles". Cada fila del proveedor en la página web principal contiene un vínculo a la página de detalles que se pasa a lo largo de la fila `SupplierID` valor. Estos vínculos específicas de la fila se pueden agregar fácilmente mediante el campo Hyperlink de GridView.

En la página de detalles recuperar esos productos para el proveedor especificado se logra invocando el `ProductsBLL` la clase `GetProductsBySupplierID(supplierID)` método. El *`supplierID`* se especificó el valor del parámetro mediante declaración utilizando la cadena de consulta como origen del parámetro. También hemos analizado cómo mostrar los detalles del proveedor en la página de detalles mediante un FormView.

Nuestro [siguiente tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) es la última en los informes de maestro y detalles. Echemos un vistazo cómo mostrar una lista de productos en un GridView donde cada fila tiene un botón de selección. Al hacer clic en el botón de selección mostrará detalles de ese producto en un control DetailsView en la misma página.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Siguiente](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
