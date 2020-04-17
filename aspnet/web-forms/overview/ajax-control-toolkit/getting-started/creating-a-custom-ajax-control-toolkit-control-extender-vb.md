---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Creación de un extensor de control de kit de herramientas de control de AJAX personalizado (VB) Microsoft Docs
author: rick-anderson
description: Los extensores personalizados le permiten personalizar y ampliar las capacidades de ASP.NET controles sin tener que crear nuevas clases.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: adce8e23ccf0a3364ee85534e98f8ea67ae4718d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543683"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Crear un extensor de control personalizado de AJAX Control Toolkit (VB)

por [Microsoft](https://github.com/microsoft)

> Los extensores personalizados le permiten personalizar y ampliar las capacidades de ASP.NET controles sin tener que crear nuevas clases.

En este tutorial, aprenderá a crear un extensor de control de AJAX Control Toolkit personalizado. Creamos un nuevo extensor simple, pero útil, que cambia el estado de un Button de deshabilitado a habilitado al escribir texto en un cuadro de texto. Después de leer este tutorial, usted será capaz de extender el ASP.NET AJAX Toolkit con sus propios extensores de control.

Puede crear extensores de control personalizados mediante Visual Studio o Visual Web Developer (asegúrese de que tiene la versión más reciente de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Descripción general del extensor DisabledButton

Nuestro nuevo extensor de control se llama disabledButton extender. Este extensor tendrá tres propiedades:

- TargetControlID: el cuadro de texto que extiende el control.
- TargetButtonIID: el botón que está deshabilitado o habilitado.
- DisabledText: el texto que se muestra inicialmente en el botón. Al empezar a escribir, el botón muestra el valor de la Button Text propiedad.

Enlazar el DisabledButton extensor a un TextBox y Button control. Antes de escribir cualquier texto, el botón está deshabilitado y el cuadro de texto y el botón se ven así:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

( Haga clic para ver la[imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))

Después de empezar a escribir texto, el botón está habilitado y el cuadro de texto y el botón se ven así:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

( Haga clic para ver la[imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))

Para crear nuestro extensor de control, necesitamos crear los siguientes tres archivos:

- DisabledButtonExtender.vb: este archivo es la clase de control del lado servidor que administrará la creación del extensor y le permitirá establecer las propiedades en tiempo de diseño. También define las propiedades que se pueden establecer en el extensor. Estas propiedades son accesibles a través del código y en tiempo de diseño y coinciden con las propiedades definidas en el archivo DisableButtonBehavior.js.
- DisabledButtonBehavior.js: este archivo es donde agregará toda la lógica de script de cliente.
- DisabledButtonDesigner.vb: esta clase habilita la funcionalidad en tiempo de diseño. Necesita esta clase si desea que el extensor de control funcione correctamente con Visual Studio/Visual Web Developer Designer.

Por lo tanto, un extensor de control consta de un control del lado servidor, un comportamiento del lado cliente y una clase de diseñador del lado servidor. Aprenderá a crear estos tres archivos en las secciones siguientes.

## <a name="creating-the-custom-extender-website-and-project"></a>Creación del sitio web y el proyecto de Custom Extender

El primer paso es crear un proyecto de biblioteca de clases y un sitio web en Visual Studio/Visual Web Developer. Crearemos el extensor personalizado en el proyecto de biblioteca de clases y probaremos el extensor personalizado en el sitio web.

Vamos a empezar con el sitio web. Siga estos pasos para crear el sitio web:

1. Seleccione la opción de menú **Archivo, Nuevo sitio web**.
2. Seleccione la ASP.NET plantilla **de sitio web.**
3. Asigne al nuevo sitio web el nombre *Website1*.
4. Haga clic en el botón **Aceptar** .

A continuación, necesitamos crear el proyecto de biblioteca de clases que contendrá el código para el extensor de control:

1. Seleccione la opción de menú **Archivo, Agregar, Nuevo proyecto**.
2. Seleccione la plantilla **Biblioteca de clases.**
3. Asigne un nombre a la nueva biblioteca de clases con el nombre **CustomExtenders**.
4. Haga clic en el botón **Aceptar** .

Después de completar estos pasos, la ventana del Explorador de soluciones debe tener el aspecto de la figura 1.

[![Solución con proyecto de biblioteca de clases y sitio web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: Solución con el sitio web y el proyecto de biblioteca de clases[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png)

A continuación, debe agregar todas las referencias de ensamblado necesarias al proyecto de biblioteca de clases:

1. Haga clic con el botón derecho en el proyecto CustomExtenders y seleccione la opción de menú **Agregar referencia**.
2. Seleccione la pestaña .NET.
3. Agregue referencias a los siguientes ensamblados:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Seleccione la pestaña Examinar.
5. Agregue una referencia al ensamblado AjaxControlToolkit.dll. Este ensamblado se encuentra en la carpeta donde descarclaó el Kit de herramientas de control de AJAX.

Puede comprobar que ha agregado todas las referencias correctas haciendo clic con el botón derecho en el proyecto, seleccionando Propiedades y haciendo clic en la pestaña Referencias (consulte la figura 2).

[![Carpeta de referencias con referencias necesarias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: Carpeta Referencias con referencias necesarias[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png)

## <a name="creating-the-custom-control-extender"></a>Creación del extensor de control personalizado

Ahora que tenemos nuestra biblioteca de clases, podemos empezar a construir nuestro control extensor. Vamos a empezar con los huesos desnudos de una clase de control extensor personalizado (consulte el listado 1).

**Listado 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Hay varias cosas que se observan acerca de la clase de extensor de control en el listado 1. En primer lugar, observe que la clase hereda de la base ExtenderControlBase clase. Todos los controles extensores de AJAX Control Toolkit derivan de esta clase base. Por ejemplo, la clase base incluye la propiedad TargetID que es una propiedad necesaria de cada extensor de control.

A continuación, observe que la clase incluye los dos atributos siguientes relacionados con el script de cliente:

- WebResource: hace que un archivo se incluya como un recurso incrustado en un ensamblado.
- ClientScriptResource: hace que un recurso de script se recupere de un ensamblado.

El atributo WebResource se utiliza para incrustar el archivo JavaScript MyControlBehavior.js en el ensamblado cuando se compila el extensor personalizado. El atributo ClientScriptResource se utiliza para recuperar el script MyControlBehavior.js del ensamblado cuando se usa el extensor personalizado en una página web.

Para que los atributos WebResource y ClientScriptResource funcionen, debe compilar el archivo JavaScript como un recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *Recurso incrustado* a la propiedad Acción de **compilación.**

Observe que el extensor de control también incluye un atributo TargetControlType. Este atributo se utiliza para especificar el tipo de control que se extiende por el extensor de control. En el caso del listado 1, el extensor de control se utiliza para extender un cuadro de texto.

Por último, observe que el extensor personalizado incluye una propiedad denominada MyProperty. La propiedad se marca con el atributo ExtenderControlProperty. Los métodos GetPropertyValue() y SetPropertyValue() se utilizan para pasar el valor de propiedad del extensor de control del lado servidor al comportamiento del lado cliente.

Vamos a seguir adelante e implementar el código para nuestro extensor DisabledButton. El código de este extensor se puede encontrar en el listado 2.

**Listado 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

El disabledButton extensor en el listado 2 tiene dos propiedades denominadas TargetButtonID y DisabledText. El IDReferenceProperty aplicado a la TargetButtonID propiedad le impide asignar cualquier cosa que no sea el identificador de un Button control a esta propiedad.

Los atributos WebResource y ClientScriptResource asocian un comportamiento del lado cliente ubicado en un archivo denominado DisabledButtonBehavior.js a este extensor. Hablamos de este archivo JavaScript en la siguiente sección.

## <a name="creating-the-custom-extender-behavior"></a>Creación del comportamiento del extensor personalizado

El componente del lado cliente de un extensor de control se denomina comportamiento. La lógica real para deshabilitar y habilitar el Button está contenida en el DisabledButton comportamiento. El código JavaScript para el comportamiento se incluye en el listado 3.

**Listado 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

El archivo JavaScript del listado 3 contiene una clase del lado cliente denominada DisabledButtonBehavior. Esta clase, al igual que su gemelo del lado servidor, incluye dos\_propiedades denominadas\_TargetButtonID y\_DisabledText\_a las que puede tener acceso mediante get TargetButtonID/set TargetButtonID y get DisabledText/set DisabledText.

El método initialize() asocia un controlador de eventos keyup con el elemento de destino para el comportamiento. Cada vez que escribe una letra en el cuadro de texto asociado a este comportamiento, se ejecuta el controlador keyup. El controlador keyup habilita o deshabilita el Button dependiendo de si el cuadro de texto asociado con el comportamiento contiene cualquier texto.

Recuerde que debe compilar el archivo JavaScript en el listado 3 como un recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *Recurso incrustado* a la propiedad Acción de **compilación** (consulte la figura 3). Esta opción está disponible en Visual Studio y Visual Web Developer.

[![Agregar un archivo JavaScript como un recurso incrustado](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: Adición de un archivo JavaScript como un recurso incrustado[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png)

## <a name="creating-the-custom-extender-designer"></a>Creación del diseñador de extensores personalizados

Hay una última clase que necesitamos crear para completar nuestro extensor. Necesitamos crear la clase de diseñador en el listado 4. Esta clase es necesaria para que el extensor se comporte correctamente con Visual Studio/Visual Web Developer Designer.

**Listado 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Asociar el diseñador en el listado 4 con el DisabledButton extensor con el Designer atributo. Debe aplicar el atributo Designer a la clase DisabledButtonExtender de la siguiente manera:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Uso del extensor personalizado

Ahora que hemos terminado de crear el extensor de control DisabledButton, es el momento de usarlo en nuestro sitio web de ASP.NET. En primer lugar, necesitamos agregar el extensor personalizado a la caja de herramientas. Siga estos pasos:

1. Abra una página de ASP.NET haciendo doble clic en la página en la ventana Explorador de soluciones.
2. Haga clic con el botón derecho en la caja de herramientas y seleccione la opción de menú **Elegir elementos**.
3. En el cuadro de diálogo Elegir elementos del cuadro de herramientas, vaya al ensamblado CustomExtenders.dll.
4. Haga clic en el botón **Aceptar** para cerrar el cuadro de diálogo.

Después de completar estos pasos, el extensor de control DisabledButton debe aparecer en el cuadro de herramientas (consulte la figura 4).

[![DisabledButton en la caja de herramientas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton en la caja de herramientas[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png)

A continuación, necesitamos crear una nueva página de ASP.NET. Siga estos pasos:

1. Cree una nueva página de ASP.NET denominada ShowDisabledButton.aspx.
2. Arrastre un ScriptManager a la página.
3. Arrastre un TextBox control a la página.
4. Arrastre un Button control a la página.
5. En la ventana Propiedades, cambie la propiedad ID de botón al valor <em>btnSave</em> y la propiedad Text al valor *\*Save*.

Hemos creado una página con un control de cuadro de texto y botón de ASP.NET estándar.

A continuación, necesitamos extender el control TextBox con el extensor DisabledButton:

1. Seleccione la opción de tarea **Agregar extensor** para abrir el cuadro de diálogo Asistente para extensores (consulte la figura 5). Observe que el cuadro de diálogo incluye nuestro extensor DisabledButton personalizado.
2. Seleccione el extensor DisabledButton y haga clic en el botón **Aceptar.**

[![El cuadro de diálogo Asistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: El cuadro de diálogo Asistente de extensor[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png)

Por último, podemos establecer las propiedades del extensor DisabledButton. Puede modificar las propiedades del extensor DisabledButton modificando las propiedades del control TextBox:

1. Seleccione el cuadro de texto en el diseñador.
2. En la ventana Propiedades, expanda el nodo Extensores (consulte la figura 6).
3. Asigne el valor *Save* a la propiedad DisabledText y el valor *btnSave* a la propiedad TargetButtonID.

[![Configuración de las propiedades del extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: Configuración de las propiedades del extensor[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png)

Al ejecutar la página (pulsando F5), el Button control se deshabilita inicialmente. Tan pronto como comience a escribir texto en el cuadro de texto, el Button control está habilitado (consulte la figura 7).

[![El extensor DisabledButton en acción](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: El extensor DisabledButton en acción[(Haga clic para ver la imagen de tamaño completo)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png)

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo se puede ampliar el kit de herramientas de control de AJAX con controles extensores personalizados. En este tutorial, hemos creado un extensor de control DisabledButton simple. Implementamos este extensor mediante la creación de un DisabledButtonExtender clase, un DisabledButtonBehavior JavaScript comportamiento y un DisabledButtonDesigner clase. Siga un conjunto similar de pasos cada vez que cree un extensor de control personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
