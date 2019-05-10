---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Diseño y menú categoría | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 3 aborda la adición de diseño y un menú de la categoría.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131030"
---
# <a name="part-3-layout-and-category-menu"></a>Parte 3: Diseño y menú Categoría

por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 3 aborda la adición de diseño y un menú de la categoría.

## <a id="_Toc260221669"></a>  Adición de algunos diseño y un menú categoría

En la página principal del sitio vamos a agregar un elemento div para la columna del lado izquierdo que contendrá el menú de la categoría de producto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Tenga en cuenta que se proporcionará la alineación deseada y otro formato de la clase CSS que agregamos a nuestro archivo Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

El menú de la categoría de producto se crearán dinámicamente en tiempo de ejecución consultando la base de datos de comercio de categorías de producto existente y crear los elementos de menú y correspondientes vínculos.

Para ello usaremos dos de ASP. Controles de datos eficaces de la red. El control de "Origen de datos de entidad" y el control "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos a cambiar a la "Vista de diseño" y usar las aplicaciones auxiliares para configurar los controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos a establecer la propiedad ID de EntityDataSource a EDS\_categoría\_menú y haga clic en "Configurar origen de datos".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Seleccione la conexión de CommerceEntities que se creó para nosotros cuando creamos el modelo de origen de datos de entidad para nuestra base de datos de comercio y haga clic en "Siguiente".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Seleccione el nombre del conjunto de entidades de "Categorías" y deje el resto de las opciones como valor predeterminado. Haga clic en "Finalizar".

Ahora vamos a establecer la propiedad ID de la instancia del control ListView que se coloca en nuestra página de ListView\_ProductsMenu y activar su aplicación auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Aunque podríamos usar las opciones de control para dar formato a la presentación del elemento de datos y formato, nuestra creación de menús sólo requerirá marcado simple por lo que se especificará el código en la vista del origen.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Tenga en cuenta la instrucción "Eval": &lt;% # Eval("CategoryName") %&gt;

La sintaxis ASP.NET &lt;% # %&gt; es una convención taquigrafía que indica el tiempo de ejecución para ejecutar todo lo que está dentro y mostrar los resultados "en línea".

La instrucción Eval("CategoryName") indica, a la entrada actual en la colección enlazada de elementos de datos, capturar el valor de los nombres de elemento de modelo de entidad "CategoryName". Se trata de una sintaxis concisa para una característica muy eficaz.

Permite ejecutar la aplicación ahora.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Tenga en cuenta que ahora se muestra el menú de la categoría de producto y cuando se mantiene el puntero sobre uno de los elementos de menú categoría apunta el vínculo de elemento de menú a una página que se deben implementar con aún podemos ver denominado ProductsList.aspx y que hemos creado un argumento de cadena de consulta dinámica que contiene el  Id. de categoría.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Siguiente](tailspin-spyworks-part-4.md)
