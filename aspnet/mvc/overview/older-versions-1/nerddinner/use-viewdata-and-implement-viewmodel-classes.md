---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usar ViewData e Implementar clases de ViewModel ? Microsoft Docs
author: rick-anderson
description: El paso 6 muestra cómo habilitar la compatibilidad con escenarios de edición de formularios más enriquecidos y también se describen dos enfoques que se pueden usar para pasar datos de controladores a vistas:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 7fa2af2a55d12bbe11b29dff594823a1e5ea0152
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541109"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Usar ViewData e implementar clases ViewModel

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 6 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> En el paso 6 se muestra cómo habilitar la compatibilidad con escenarios de edición de formularios más enriquecidos y también se describen dos enfoques que se pueden usar para pasar datos de controladores a vistas: ViewData y ViewModel.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner Paso 6: ViewData y ViewModel

Hemos cubierto una serie de escenarios de publicación de formularios y hemos explicado cómo implementar la compatibilidad con la creación, actualización y eliminación (CRUD). Ahora llevaremos nuestra implementación de DinnersController más allá y habilitaremos la compatibilidad con escenarios de edición de formularios más enriquecidos. Al hacer esto, discutiremos dos enfoques que se pueden usar para pasar datos de controladores a vistas: ViewData y ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Pasar datos de controladores a plantillas de vista

Una de las características definitorias del patrón MVC es la estricta "separación de preocupaciones" que ayuda a aplicar entre los diferentes componentes de una aplicación. Modelos, controladores y vistas cada uno tiene roles y responsabilidades bien definidos, y se comunican entre sí de maneras bien definidas. Esto ayuda a promover la capacidad de prueba y la reutilización del código.

Cuando una clase Controller decide representar una respuesta HTML a un cliente, es responsable de pasar explícitamente a la plantilla de vista todos los datos necesarios para representar la respuesta. Las plantillas de vista nunca deben realizar ninguna lógica de aplicación o recuperación de datos y, en su lugar, deben limitarse a tener solo código de representación que el controlador le haya pasado fuera del modelo o los datos que le ha pasado.

En este momento, los datos del modelo que pasa nuestra clase DinnersController a nuestras plantillas de vista son simples y directos: una lista de objetos Dinner en el caso de Index(), y un único objeto Dinner en el caso de Details(), Edit(), Create() y Delete(). A medida que agregamos más capacidades de interfaz de usuario a nuestra aplicación, a menudo vamos a necesitar pasar algo más que estos datos para representar respuestas HTML dentro de nuestras plantillas de vista. Por ejemplo, es posible que deseemos cambiar el campo "País" dentro de nuestro Editar y crear vistas de ser un cuadro de texto HTML a una lista desplegable. En lugar de codificar de forma rígida la lista desplegable de nombres de países en la plantilla de vista, es posible que deseemos generarla a partir de una lista de países admitidos que rellenamos dinámicamente. Necesitaremos una manera de pasar tanto el objeto Dinner *como* la lista de países admitidos de nuestro controlador a nuestras plantillas de vista.

Veamos dos maneras en que podemos lograr esto.

### <a name="using-the-viewdata-dictionary"></a>Uso del diccionario ViewData

La clase base Controller expone una propiedad de diccionario "ViewData" que se puede usar para pasar elementos de datos adicionales de controladores a vistas.

Por ejemplo, para admitir el escenario en el que queremos cambiar el cuadro de texto "País" dentro de nuestra vista Editar de ser un cuadro de texto HTML a una lista desplegable, podemos actualizar nuestro método de acción Edit() para pasar (además de un objeto Dinner) un objeto SelectList que se puede utilizar como modelo de una lista desplegable de países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

El constructor de selectList anterior está aceptando una lista de condados con los que rellenar la lista desplegable, así como el valor seleccionado actualmente.

A continuación, podemos actualizar nuestra plantilla de vista Edit.aspx para utilizar el método auxiliar Html.DropDownList() en lugar del método auxiliar Html.TextBox() que usamos anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

El método auxiliar Html.DropDownList() anterior toma dos parámetros. El primero es el nombre del elemento de formulario HTML que se va a generar. El segundo es el modelo "SelectList" que pasamos a través del diccionario ViewData. Estamos usando la palabra clave "as" de C- para convertir el tipo dentro del diccionario como SelectList.

Y ahora cuando ejecutemos nuestra aplicación y accedamos a la URL */Dinners/Edit/1* dentro de nuestro navegador veremos que nuestra interfaz de usuario de edición se ha actualizado para mostrar una lista desplegable de países en lugar de un cuadro de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Dado que también representamos la plantilla de vista Editar desde el método de edición HTTP-POST (en escenarios cuando se producen errores), queremos asegurarnos de que también actualizamos este método para agregar SelectList a ViewData cuando la plantilla de vista se representa en escenarios de error:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Y ahora nuestro escenario de edición DinnersController admite un DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Uso de un patrón ViewModel

El enfoque de diccionario ViewData tiene la ventaja de ser bastante rápido y fácil de implementar. Sin embargo, a algunos desarrolladores no les gusta usar diccionarios basados en cadenas, ya que los errores tipográficos pueden dar lugar a errores que no se detectarán en tiempo de compilación. El diccionario ViewData sin tipo también requiere el uso del operador "as" o la conversión cuando se usa un lenguaje fuertemente tipado como C- en una plantilla de vista.

Un enfoque alternativo que podríamos usar es uno que a menudo se conoce como el patrón "ViewModel". Cuando se usa este patrón, creamos clases fuertemente tipadas que están optimizadas para nuestros escenarios de vista específicos y que exponen propiedades para los valores dinámicos o el contenido que necesitan nuestras plantillas de vista. Nuestras clases de controlador pueden rellenar y pasar estas clases optimizadas para vista a nuestra plantilla de vista para usarlas. Esto habilita la seguridad de tipos, la comprobación en tiempo de compilación y el editor intellisense dentro de las plantillas de vista.

Por ejemplo, para habilitar escenarios de edición de formularios de cena podemos crear una clase "DinnerFormViewModel" como la siguiente que expone dos propiedades fuertemente tipadas: un objeto Dinner y el modelo SelectList necesario para rellenar la lista desplegable de países:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

A continuación, podemos actualizar nuestro método de acción Edit() para crear DinnerFormViewModel utilizando el objeto Dinner que recuperamos de nuestro repositorio y, a continuación, pasarlo a nuestra plantilla de vista:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

A continuación, actualizaremos nuestra plantilla de vista para que espere un objeto "DinnerFormViewModel" en lugar de un objeto "Dinner" cambiando el atributo "inherits" en la parte superior de la página edit.aspx de esa forma:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Una vez hecho esto, el intellisense de la propiedad "Modelo" dentro de nuestra plantilla de vista se actualizará para reflejar el modelo de objetos del tipo DinnerFormViewModel que le estamos pasando:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

A continuación, podemos actualizar nuestro código de vista para trabajar fuera de él. Observe a continuación cómo no estamos cambiando los nombres de los elementos de entrada que estamos creando (los elementos del formulario seguirán siendo denominados "Título", "País") – pero estamos actualizando los métodos auxiliares HTML para recuperar los valores mediante la clase DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

También actualizaremos nuestro método Edit post para usar la clase DinnerFormViewModel al representar errores:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

También podemos actualizar nuestros métodos de acción Create() para reutilizar exactamente la misma clase *DinnerFormViewModel* para habilitar los países DropDownList dentro de ellos también. A continuación se muestra la implementación HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

A continuación se muestra la implementación del método HTTP-POST Create:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Y ahora nuestras pantallas Editar y Crear admiten listas desplegables para elegir el país.

### <a name="custom-shaped-viewmodel-classes"></a>Clases ViewModel con forma personalizada

En el escenario anterior, nuestra clase DinnerFormViewModel expone directamente el objeto de modelo Dinner como una propiedad, junto con una propiedad de modelo SelectList compatible. Este enfoque funciona bien para escenarios donde la interfaz de usuario HTML que queremos crear dentro de nuestra plantilla de vista corresponde relativamente a nuestros objetos de modelo de dominio.

Para escenarios donde este no es el caso, una opción que puede usar es crear una clase ViewModel con forma personalizada cuyo modelo de objetos está más optimizado para el consumo de la vista y que podría tener un aspecto completamente diferente del objeto de modelo de dominio subyacente. Por ejemplo, podría exponer diferentes nombres de propiedad o propiedades de agregado recopiladas de varios objetos de modelo.

Las clases ViewModel con forma personalizada se pueden usar tanto para pasar datos de controladores a vistas para representar, como para ayudar a controlar los datos de formulario publicados en el método de acción de un controlador. Para este escenario posterior, es posible que el método de acción actualice un objeto ViewModel con los datos publicados en el formulario y, a continuación, use la instancia de ViewModel para asignar o recuperar un objeto de modelo de dominio real.

Las clases ViewModel con forma personalizada pueden proporcionar una gran flexibilidad y son algo para investigar cada vez que encuentre el código de representación dentro de las plantillas de vista o el código de publicación de formularios dentro de los métodos de acción que empiezan a complicarse demasiado. Esto suele ser una señal de que los modelos de dominio no se corresponden limpiamente con la interfaz de usuario que está generando y que una clase ViewModel con forma personalizada intermedia puede ayudar.

### <a name="next-step"></a>siguiente paso

Veamos ahora cómo podemos usar parciales y páginas maestras para reutilizar y compartir la interfaz de usuario en toda nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Siguiente](re-use-ui-using-master-pages-and-partials.md)
