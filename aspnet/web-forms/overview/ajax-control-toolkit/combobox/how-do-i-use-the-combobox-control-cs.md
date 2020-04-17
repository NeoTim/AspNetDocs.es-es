---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: ¿Cómo uso el control ComboBox? (C-) Microsoft Docs
author: rick-anderson
description: ComboBox es un ASP.NET control AJAX que combina la flexibilidad de un cuadro de texto con una lista de opciones entre las que los usuarios pueden elegir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2adb9663cb7bdc38f28a127f7932f45a3447d1de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543810"
---
# <a name="how-do-i-use-the-combobox-control-c"></a>¿Cómo uso el control ComboBox? (C#)

por [Microsoft](https://github.com/microsoft)

> ComboBox es un ASP.NET control AJAX que combina la flexibilidad de un cuadro de texto con una lista de opciones entre las que los usuarios pueden elegir.

El objetivo de este tutorial es explicar el control ComboBox de AJAX Control Toolkit. El ComboBox funciona como una combinación entre un estándar ASP.NET DropDownList control y un TextBox control. Puede seleccionar de una lista de elementos preexistentes o introducir un nuevo elemento.

El ComboBox es similar al extensor de control AutoCompletar, pero los controles se usan en diferentes escenarios. El extensor Autocompletar consulta un servicio web para obtener entradas coincidentes. El Control ComboBox, en cambio, se inicializa con un conjunto de elementos. El uso del extensor Autocompletar tiene sentido cuando se trabaja con un gran conjunto de datos (millones de piezas de automóviles) mientras se utiliza el control ComboBox tiene sentido cuando se trabaja con un pequeño conjunto de datos (decenas de piezas de coche).

## <a name="selecting-from-a-static-list-of-items"></a>Selección de una lista estática de elementos

Vamos a empezar con un ejemplo simple de uso de la ComboBox control. Imagine que desea mostrar una lista estática de elementos en una lista desplegable. Sin embargo, desea dejar abierta la posibilidad de que la lista no esté completa. Desea permitir que un usuario escriba un valor personalizado en la lista.

Crearemos una nueva página de formularios Web Forms ASP.NET y usaremos el control ComboBox en la página. Agregue la nueva página ASP.NET al proyecto y cambie a la vista Diseño.

Si desea utilizar el ComboBox control en la página, a continuación, debe agregar un ScriptManager control a la página. Arrastre el control ScriptManager desde debajo de la pestaña Extensiones de AJAX a la superficie de Designer. Debe agregar el control ScriptManager en la parte superior de la página; puede agregarlo inmediatamente debajo de &lt;&gt; la etiqueta de formulario del lado del servidor de apertura.

A continuación, arrastre el control ComboBox a la página. Puede encontrar el control ComboBox en el cuadro de herramientas con los otros controles y extensores de control del Kit de herramientas de control de AJAX (consulte la figura 1).

[![Formulario simple para crear una tarjeta de visita](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: Selección del control ComboBox en el cuadro de herramientas (Haga clic para ver la[imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image2.png)

Usaremos el control ComboBox para mostrar una lista estática de opciones. El usuario puede seleccionar un nivel particular de picante para sus alimentos de una lista de tres opciones: Suave, Medio y Caliente (consulte la Figura 2).

[![Selección de una lista estática de elementos](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: Selección de una lista estática de elementos[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image4.png)

Hay dos maneras de agregar estas opciones al control ComboBox. En primer lugar, seleccione la opción de tarea Editar opciones al pasar el ratón sobre el control en la vista Diseño y abra el Editor de elementos (consulte la figura 3).

[![Edición de elementos ComboBox](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: Edición de elementos ComboBox[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image6.png)

La segunda opción es agregar la lista de &lt;elementos entre&gt; las etiquetas asp:ComboBox de apertura y cierre en la vista Origen. La página del listado 1 contiene el ComboBox actualizado que tiene la lista de elementos.

**Listado 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Al abrir la página en el listado 1, puede seleccionar una de las opciones preexistentes de ComboBox. En otras palabras, el ComboBox funciona igual que un DropDownList control.

Sin embargo, también tiene la opción de introducir una nueva opción (por ejemplo, Super Spicy) que no esté en la lista existente. Por lo tanto, el ComboBox también funciona como un TextBox control.

Independientemente de si elige un elemento preexistente o especifica un elemento personalizado, al enviar el formulario, su elección aparece en el control de etiqueta. Al enviar el formulario, el\_controlador btnSubmit Click ejecuta y actualiza la etiqueta (consulte la figura 4).

[![Visualización del elemento seleccionado](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: Visualización del elemento seleccionado[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image8.png)

El ComboBox admite las mismas propiedades que el DropDownList control para recuperar el elemento seleccionado después de enviar un formulario:

- SelectedItem.Text: muestra el valor de la propiedad Text del elemento seleccionado.
- SelectedItem.Value: muestra el valor de la propiedad Value del elemento seleccionado o muestra el texto escrito en el cuadro combinado.
- SelectedValue: igual que SelectedItem.Value, excepto que esta propiedad le permite especificar el elemento seleccionado predeterminado (inicial).

Si escribe una opción personalizada en el ComboBox, la opción personalizada se asigna a la SelectedItem.Text y SelectedItem.Value propiedades.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selección de la lista de elementos de la base de datos

Puede recuperar la lista de elementos que el ComboBox muestra de una base de datos. Por ejemplo, puede enlazar el ComboBox a un SqlDataSource control, un ObjectDataSource control, un LinqDataSource, o un EntityDataSource.

Imagine que desea mostrar una lista de películas en un ComboBox. Desea recuperar la lista de películas de la tabla de base de datos Movies. Siga estos pasos:

1. Crear una página denominada Movies.aspx
2. Agregue un control ScriptManager a la página arrastrando scriptManager desde la pestaña Extensiones de AJAX del cuadro de herramientas a la página.
3. Agregue un control ComboBox a la página arrastrando comboBox a la página.
4. En la vista Diseño, coloque el mouse sobre el control ComboBox y seleccione la opción de tarea **Elegir origen** de datos (consulte la figura 5). Se inicia el Asistente para configuración del origen de datos.
5. En el paso Elegir un &lt;origen de&gt; **datos,** seleccione la opción Nuevo origen de datos.
6. En el paso Elegir un tipo de origen de **datos,** seleccione Base de datos.
7. En el paso **Elegir** la conexión de datos, seleccione la base de datos (por ejemplo, MoviesDB.mdf).
8. En el paso **Guardar la cadena** de conexión en el archivo de configuración de la aplicación, seleccione la opción para guardar la cadena de conexión.
9. En el paso **Configurar la instrucción Select,** seleccione la tabla de base de datos Movies y seleccione todas las columnas.
10. En el paso Consulta de **prueba,** haga clic en el botón Finalizar.
11. En el paso Elegir origen de **datos,** seleccione la columna Título del campo que se va a mostrar y la columna Id del campo de datos (consulte la figura).
12. Haga clic en el botón Aceptar para cerrar el asistente.

[![Elegir un origen de datos](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: Elegir un origen de datos[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image10.png)

[![Elegir el texto de datos y los campos de valor](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: Elegir el texto de datos y los campos de valor[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image12.png)

Después de completar los pasos anteriores, el ComboBox está enlazado a un SqlDataSource control que representa las películas de la Movies tabla de base de datos. El origen de la página se parece al listado 2 (limpié un poco el formato).

**Listado 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Observe que el ComboBox control tiene un DataSourceID propiedad que apunta a la SqlDataSource control. Al abrir la página en un explorador, se muestra la lista de películas de la base de datos (consulte la figura 7). Puede elegir una película de la lista o introducir una nueva escribiendo la película en el ComboBox.

[![Visualización de una lista de películas](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: Visualización de una lista de películas[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image14.png)

## <a name="setting-the-dropdownstyle"></a>Establecer el DropDownStyle

Puede usar el ComboBox DropDownStyle propiedad para cambiar el comportamiento de la ComboBox. Esta propiedad acepta valores posibles:

- DropDown - (valor predeterminado) El ComboBox muestra una lista desplegable al hacer clic en la flecha y puede introducir un valor personalizado.
- Simple: el ComboBox muestra una lista desplegable automáticamente y puede introducir un valor personalizado.
- DropDownList - El ComboBox funciona igual que un DropDownList control.

La diferencia entre DropDown y Simple es cuando se muestra la lista de elementos. En el caso de Simple, la lista se muestra inmediatamente cuando se mueve el foco a ComboBox. En el caso de DropDown, debe hacer clic en la flecha para ver la lista de elementos.

El DropDownList valor hace que el ComboBox control funcione igual que un estándar DropDownList control. Sin embargo, hay una diferencia importante aquí. Las versiones anteriores de Internet Explorer muestran un control DropDownList con un índice z infinito, por lo que el control aparecerá delante de cualquier control colocado delante de él. Dado que ComboBox representa &lt;&gt; una etiqueta div &lt;&gt; HTML en lugar de una etiqueta de selección HTML, ComboBox respeta correctamente el orden z.

## <a name="setting-the-autocompletemode"></a>Establecer el AutoCompleteMode

Utilice la propiedad AutoCompleteMode de ComboBox para especificar lo que sucede cuando alguien escribe texto en el ComboBox. Esta propiedad acepta los siguientes valores posibles:

- Ninguno - (valor predeterminado) El ComboBox no proporciona ningún comportamiento de autocompletar.
- Sugerir: el Cuadro combinado muestra la lista y resalta el elemento coincidente de la lista (consulte la figura 8).
- Anexar: el ComboBox no muestra la lista y anexa el elemento coincidente de la lista a lo que ha escrito (consulte la figura 9).
- SuggestAppend: el ComboBox muestra la lista y anexa el elemento coincidente de la lista a lo que ha escrito (consulte la figura 10).

[![El ComboBox hace una sugerencia](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: El ComboBox hace una sugerencia[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image16.png)

[![ComboBox anexa texto coincidente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: ComboBox anexa texto coincidente[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image18.png)

[![El ComboBox sugiere y añade](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: El ComboBox sugiere y añade[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-combobox-control-cs/_static/image20.png)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a usar el ComboBox control para mostrar un conjunto fijo de elementos. Hemos enlazado el control ComboBox a un conjunto estático de elementos y a una tabla de base de datos. Por último, aprendió a modificar el comportamiento de comboBox estableciendo su DropDownStyle y AutoCompleteMode propiedades.

> [!div class="step-by-step"]
> [Siguiente](how-do-i-use-the-combobox-control-vb.md)
