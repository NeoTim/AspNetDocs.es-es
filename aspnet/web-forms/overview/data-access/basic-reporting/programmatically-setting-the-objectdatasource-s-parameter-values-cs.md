---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: Establecer mediante programación los valores de parámetro de ObjectDataSource (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 032b6665d3e99998dba870c8f7f2cdfec17737bf
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59383081"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>Configurar los valores de parámetro de ObjectDataSource mediante programación (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) o [descargar PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro mediante programación.


## <a name="introduction"></a>Introducción

Como hemos visto en el [tutorial anterior](declarative-parameters-cs.md), una serie de opciones está disponible para pasar de forma declarativa los valores de parámetro a los métodos de ObjectDataSource. Si el valor del parámetro está codificado de forma rígida, procede de un control Web en la página o en cualquier otro origen que es legible por un origen de datos `Parameter` objeto, por ejemplo, que se puede enlazar valor al parámetro de entrada sin tener que escribir una línea de código.

Puede haber ocasiones, sin embargo, cuando el valor del parámetro se trata de algún origen ya no tienen en cuenta uno del origen de datos integrados `Parameter` objetos. Si nuestro sitio admite las cuentas de usuario que podría desear establecer el parámetro en función de la sesión iniciada actualmente ID de usuario. del visitante O podemos necesitar personalizar el valor del parámetro antes de enviarlo a lo largo de método del objeto subyacente de ObjectDataSource.

Cada vez que el ObjectDataSource `Select` se invoca el método genera ObjectDataSource en primer lugar su [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). A continuación, se invoca el método del objeto subyacente de ObjectDataSource. Una vez que se haya completado el ObjectDataSource [eventos seleccionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) se activa (figura 1 muestra esta secuencia de eventos). Se pueden establecer los valores de parámetro que se pasó al método del objeto subyacente de ObjectDataSource o personalizados en un controlador de eventos para el `Selecting` eventos.


[![Tse invoca de ObjectDataSource seleccionado y seleccionar eventos Fire antes y del después de su objeto subyacente método](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Figura 1**: El ObjectDataSource `Selected` y `Selecting` eventos Fire antes y del después de su objeto subyacente método se invoca ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada `Month`, del tipo `int` y devuelve un `EmployeesDataTable` objeto rellenado con los empleados que tienen su aniversario contratación especificado `Month`. En nuestro ejemplo establecerá este parámetro mediante programación en función del mes actual, que muestra una lista de "Employee aniversarios este mes".

Comencemos.

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Paso 1: Agregar un método a`EmployeesTableAdapter`

En nuestro ejemplo primero que necesitamos agregar un medio para recuperar aquellos empleados cuyo `HireDate` se produjo en un mes especificado. Para proporcionar esta funcionalidad de acuerdo con nuestra arquitectura se debe crear primero un método en `EmployeesTableAdapter` que se asigna a la instrucción SQL adecuada. Para lograr esto, comience abriendo el conjunto de datos de Northwind con tipo. Haga doble clic en el `EmployeesTableAdapter` etiquetar y seleccionar Add Query.


[![Auna nueva consulta para el EmployeesTableAdapter dd](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Figura 2**: Agregar una nueva consulta a la `EmployeesTableAdapter` ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Elija esta opción para agregar una instrucción SQL que devuelve filas. Cuando llegue a la especificación de un `SELECT` el valor predeterminado de la pantalla de instrucción `SELECT` instrucción para el `EmployeesTableAdapter` ya se va a cargar. Basta con agregar en el `WHERE` cláusula: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) es una función de Transact-SQL que devuelve una parte de fecha en particular de un `datetime` tipo; en este caso usamos `DATEPART` para devolver el mes de la `HireDate` columna.


[![RVolve solo las filas donde la HireDate columna es menor o igual que el @HiredBeforeDate parámetro](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Figura 3**: Devolver solo las filas donde la `HireDate` columna es menor o igual que el `@HiredBeforeDate` parámetro ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Por último, cambie el `FillBy` y `GetDataBy` los nombres de método a `FillByHiredDateMonth` y `GetEmployeesByHiredDateMonth`, respectivamente.


[![CElija más adecuado método nombres que FillBy y GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Figura 4**: Elija más adecuado método nombres que `FillBy` y `GetDataBy` ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Haga clic en Finalizar para completar al asistente y volver a la superficie de diseño del conjunto de datos. El `EmployeesTableAdapter` debe incluir ahora un nuevo conjunto de métodos para tener acceso a los empleados contratados en un mes especificado.


[![Tque se muestran métodos nuevos en la superficie de diseño del conjunto de datos](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Figura 5**: Los nuevos métodos aparecen en la superficie de diseño del conjunto de datos ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Paso 2: Agregar el`GetEmployeesByHiredDateMonth(month)`método a la capa de lógica de negocios

Dado que nuestra aplicación arquitectura usa otra capa para la lógica de negocios y datos de acceso a la lógica, necesitamos agregar un método a nuestro BLL que las llamadas a la capa DAL para recuperar a los empleados contratadas antes que una fecha especificada. Abra el `EmployeesBLL.cs` archivo y agregue el siguiente método:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Al igual que con los otros métodos de esta clase, `GetEmployeesByHiredDateMonth(month)` simplemente llama a hacia abajo en la capa DAL y devuelve los resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Paso 3: Mostrar a los empleados cuyo aniversario de contratación es este mes

El último paso en este ejemplo es mostrar a los empleados cuyo aniversario de contratación es este mes. Empiece agregando un control GridView a la `ProgrammaticParams.aspx` página en el `BasicReporting` carpeta y agregue un nuevo origen ObjectDataSource como origen de datos. Configurar el origen ObjectDataSource para usar el `EmployeesBLL` clase con el `SelectMethod` establecido en `GetEmployeesByHiredDateMonth(month)`.


[![Ula clase EmployeesBLL se](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Figura 6**: Use la `EmployeesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![SElegir método de the GetEmployeesByHiredDateMonth(month)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Figura 7**: Select From el `GetEmployeesByHiredDateMonth(month)` método ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


La pantalla final nos pide que proporcione el `month` origen del valor del parámetro. Puesto que este valor también se establece mediante programación, deje el origen de los parámetros establecidos en el valor predeterminado None opción y haga clic en Finalizar.


[![Lejar el origen de parámetro establecido en None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Figura 8**: Deje el código fuente establecido en None ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Esto creará un `Parameter` objeto en el ObjectDataSource `SelectParameters` colección que no tiene un valor especificado.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Para establecer este valor mediante programación, es necesario crear un controlador de eventos para el origen de ObjectDataSource `Selecting` eventos. Para ello, vaya a la vista de diseño y haga doble clic en el origen ObjectDataSource. Como alternativa, seleccione el origen ObjectDataSource, vaya a la ventana Propiedades y haga clic en el icono de rayo. A continuación, ya sea haga doble clic en el cuadro de texto junto a la `Selecting` eventos o escriba el nombre, el controlador de eventos que desea usar.


![Haga clic en el icono de rayo en la ventana Propiedades para mostrar los eventos de un Control Web](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Figura 9**: Haga clic en el icono de rayo en la ventana Propiedades para mostrar los eventos de un Control Web


Ambos enfoques agregue un nuevo controlador de eventos para el origen de ObjectDataSource `Selecting` eventos a la clase de código subyacente de la página. En este controlador de eventos podemos leer y escribir en los valores de parámetro mediante `e.InputParameters[parameterName]`, donde *`parameterName`* es el valor de la `Name` atributo en el `<asp:Parameter>` etiqueta (el `InputParameters` colección también puede ser indizada ordinal, como en `e.InputParameters[index]`). Para establecer el `month` parámetro en el mes actual, agregue lo siguiente a la `Selecting` controlador de eventos:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Al visitar esta página a través de un explorador podemos ver que sólo un empleado ha sido contratado este mes (marzo) Laura Callahan, quién ha trabajado con la empresa desde 1994.


[![Taquellos empleados cuyo aniversarios este mes se muestran](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Figura 10**: Los empleados cuyo aniversarios este mes se muestran ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Resumen

Mientras que los valores de parámetros de ObjectDataSource normalmente pueden establecerse de forma declarativa, sin necesidad de una línea de código, es fácil establecer los valores de parámetro mediante programación. Todo lo que necesitamos hacer es crear un controlador de eventos para el origen de ObjectDataSource `Selecting` eventos, que se desencadena antes de método del objeto subyacente se invoca y establecer manualmente los valores para uno o varios parámetros a través de la `InputParameters` colección.

Este tutorial finaliza la sección informes básicos. El [siguiente tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) a partir de la sección de filtrado y escenarios de detalles principales, en el que se examinará técnicas para permitir que el visitante para filtrar los datos y explorar en profundidad desde un informe principal en un informe de detalles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-cs.md)
> [Siguiente](displaying-data-with-the-objectdatasource-vb.md)
