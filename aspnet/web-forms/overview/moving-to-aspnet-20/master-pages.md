---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Páginas maestras ? Microsoft Docs
author: rick-anderson
description: Uno de los componentes clave para un sitio Web exitoso es una apariencia coherente. En ASP.NET 1.x, los desarrolladores usaban controles de usuario para replicar elem de página común...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: b493feb21d2e3d6429f0a23df5aab66e0c4c5b07
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543189"
---
# <a name="master-pages"></a>Páginas maestras

por [Microsoft](https://github.com/microsoft)

> Uno de los componentes clave para un sitio Web exitoso es una apariencia coherente. En ASP.NET 1.x, los desarrolladores usaban controles de usuario para replicar elementos de página comunes en una aplicación web. Aunque es a duda una solución viable, el uso de controles de usuario tiene algunos inconvenientes. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas de un sitio. Los controles de usuario tampoco se representan en la vista Diseño después de insertarse en una página.

Uno de los componentes clave para un sitio Web exitoso es una apariencia coherente. En ASP.NET 1.x, los desarrolladores usaban controles de usuario para replicar elementos de página comunes en una aplicación web. Aunque es a duda una solución viable, el uso de controles de usuario tiene algunos inconvenientes. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas de un sitio. Los controles de usuario tampoco se representan en la vista Diseño después de insertarse en una página.

ASP.NET 2.0 presenta las páginas maestras como una forma de mantener una apariencia coherente, y como verá pronto, las páginas maestras representan una mejora significativa sobre el método de control de usuario.

## <a name="why-master-pages"></a>¿Por qué Master Pages?

Es posible que se pregunte por qué se necesitaban páginas maestras en ASP.NET 2.0. Después de todo, los desarrolladores de sitios web ya están utilizando controles de usuario en ASP.NET 1.x para compartir áreas de contenido entre páginas. En realidad, hay varias razones por las que los controles de usuario son una solución menos que óptima para crear un diseño común.

Los controles de usuario no definen realmente el diseño de página. En su lugar, definen el diseño y la funcionalidad de una parte de una página. La distinción entre estos dos es importante porque hace que la capacidad de administración de una solución de control de usuario sea mucho más difícil. Por ejemplo, cuando desea cambiar la posición de un control de usuario en la página, debe editar la página real en la que aparece el control de usuario. Eso está bien si usted tiene sólo unas pocas páginas, pero en los sitios grandes, rápidamente se convierte en una pesadilla de administración del sitio!

Otro inconveniente de usar controles de usuario para definir un diseño común se basa en la arquitectura de ASP.NET sí misma. Si se cambia algún miembro público de un control de usuario, requiere que vuelva a compilar todas las páginas que usan el control de usuario. A su vez, ASP.NET volverá a JIT sus páginas cuando se acceda por primera vez. Esto, una vez más, produce una arquitectura no escalable y un problema de administración de sitios para sitios más grandes.

Ambos problemas (y muchos más) son bien abordados por las páginas maestras en ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Cómo funcionan las páginas maestras

Una página maestra es análoga a una plantilla para otras páginas. Los elementos de página que se deben compartir entre otras páginas (es decir, menús, bordes, etc.) se agregan a la página maestra. Cuando se agregan nuevas páginas al sitio, puede asociarlas a una página maestra. Una página asociada a una página maestra se denomina página de **contenido**. De forma predeterminada, una página de contenido toma la apariencia de la página maestra. Sin embargo, al crear una página maestra, puede definir partes de la página que la página de contenido puede reemplazar con su propio contenido. Estas partes se definen mediante un nuevo control introducido en ASP.NET 2.0; el control **ContentPlaceHolder.**

Una página maestra puede contener cualquier número de controles ContentPlaceHolder (o ninguno en absoluto.) En la página de contenido, el contenido de los controles ContentPlaceHolder aparece dentro de los controles **Content,** otro nuevo control en ASP.NET 2.0. De forma predeterminada, las páginas de contenido Los controles de contenido están vacíos para que pueda proporcionar su propio contenido. Si desea utilizar el contenido de la página maestra dentro de los controles de contenido, puede hacerlo como verá más adelante en este módulo. El control Content se asigna al control ContentPlaceHolder a través del atributo ContentPlaceHolderID del control Content. El código siguiente asigna un control Content a un control ContentPlaceHolder denominado mainBody en una página maestra.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> A menudo escuchará a las personas describir las páginas maestras como una clase base para otras páginas. Eso no es verdad. La relación entre las páginas maestras y las páginas de contenido no es de herencia.

**En la figura 1** se muestra una página maestra y una página de contenido asociada tal como aparecen en Visual Studio 2005. Puede ver el control ContentPlaceHolder en la página maestra y el control Content correspondiente en la página de contenido. Tenga en cuenta que el contenido de las páginas maestras que está fuera de ContentPlaceHolder está visible pero atenuado en la página de contenido. Solo el contenido dentro de ContentPlaceHolder se puede suplantar mediante la página de contenido. El resto del contenido que proviene de la página maestra es inmutable.

![Una página maestra y su página de contenido asociada](master-pages/_static/image1.jpg)

**Figura 1**: Una página maestra y su página de contenido asociada

## <a name="creating-a-master-page"></a>Creación de una página maestra

Para crear una nueva página maestra:

1. Abra Visual Studio 2005 y cree un nuevo sitio Web.
2. Haga clic en Archivo, Nuevo, Archivo.
3. Elija el archivo maestro del cuadro de diálogo Del nuevo elemento tal y como se muestra en **de la figura 2.**
4. Haga clic en Agregar.

![Creación de una nueva página maestra](master-pages/_static/image2.jpg)

**Figura 2**: Creación de una nueva página maestra

Observe que la extensión de archivo para una página maestra es *.master*. Esta es una de las formas en que una página maestra difiere de una página normal. La otra diferencia principal es @Page que, en lugar @Master de una directiva, la página maestra contiene una directiva. Cambie a la vista de origen para la página maestra que acaba de crear y revise el código.

Una nueva página maestra tendrá un control ContentPlaceHolder de forma predeterminada. En la mayoría de los casos, tiene más sentido crear los elementos de página comunes primero y, a continuación, insertar ContentPlaceHolder controles donde se desea contenido personalizado. En esos casos, los desarrolladores querrán eliminar el control ContentPlaceHolder predeterminado e insertar otros nuevos a medida que se desarrolla la página. ContentPlaceHolder controles no son redimensionables a pesar del hecho de que muestran identificadores de tamaño. El ContentPlaceHolder tamaños de control automáticamente en función del contenido que contiene con una excepción; si coloca un control ContentPlaceHolder dentro de un elemento de bloque como una celda de tabla, tendrá el tamaño según el tamaño del elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 Trabajar con páginas maestras

En este laboratorio, creará una nueva página maestra y definirá tres controles ContentPlaceHolder. A continuación, creará una nueva página de contenido y reemplazará el contenido de al menos uno de los controles ContentPlaceHolder.

1. Cree una página maestra e inserte controles ContentPlaceHolder. 

    1. Cree una nueva página maestra como se ha descrito anteriormente.
    2. Elimine el control ContentPlaceHolder predeterminado.
    3. Seleccione el control ContentPlaceHolder haciendo clic en el borde superior sombreado del control y, a continuación, elimínelo pulsando la tecla DEL del teclado.
    4. Inserte una nueva tabla usando el *encabezado y* la plantilla lateral como se muestra en la figura 3. Cambie el ancho y el alto al 90% cada uno para que toda la tabla sea visible en el diseñador.

![](master-pages/_static/image3.jpg)

**Figura 3**

1. Coloque el cursor en cada celda de la tabla y establezca la propiedad *valign* en *superior*.
2. Desde el cuadro de herramientas, inserte un Control ContentPlaceHolder en la celda superior de la tabla (la celda de encabezado.)
3. Al insertar este control ContentPlaceHolder, observará que el alto de fila ocupará casi toda la página como se muestra en la figura 4. No te preocupes por eso en este momento.

![El espacio vacío está en la misma celda que ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: El espacio vacío está en la misma celda que ContentPlaceHolder

1. Coloque un control ContentPlaceHolder en las otras dos celdas. Una vez que se han insertado los otros controles ContentPlaceHolder, el tamaño de las celdas de la tabla debe ser como cabría esperar. La página ahora debería parecerse a la página que se muestra en **la figura 5**.

![El maestro con todos los controles ContentPlaceHolder. Observe que la altura de celda para la celda de encabezado es ahora lo que debe ser](master-pages/_static/image2.gif)

**Figura 5**: El maestro con todos los controles ContentPlaceHolder. Observe que la altura de celda para la celda de encabezado es ahora lo que debe ser

1. Escriba algún texto de su elección en cada uno de los tres controles ContentPlaceHolder.
2. Guarde la página maestra como exercise1.master.
3. Cree un nuevo formulario Web Forms y asócielo a la página maestra exercise1.master.
4. Seleccione Archivo, Nuevo, Archivo en Visual Studio 2005.
5. Seleccione **Formulario web** en el cuadro de diálogo Agregar nuevo elemento.
6. Asegúrese de que la casilla Seleccionar página maestra esté marcada como se muestra en la figura 6.

![Adición de una nueva página de contenido](master-pages/_static/image3.gif)

**Figura 6**: Adición de una nueva página de contenido

1. Haga clic en Agregar.
2. Seleccione exercise1.master en el cuadro de diálogo Seleccionar una página maestra como se muestra en la figura 7.
3. Haga clic en Aceptar para agregar la nueva página de contenido.

La nueva página de contenido aparece en Visual Studio con un control Content para cada control ContentPlaceHolder de la página maestra. De forma predeterminada, los controles Content están vacíos para que pueda agregar su propio contenido. Si desea que utilicen el contenido del control ContentPlaceHolder de la página maestra, simplemente haga clic en el símbolo de etiqueta inteligente (la pequeña flecha negra en la esquina superior derecha del control) y elija *Default to Masters Content* en la etiqueta inteligente como se muestra en la figura **8.** Al hacerlo, el elemento de menú cambia a *Crear contenido personalizado*. Al hacer clic en él en ese momento, se quita el contenido de la página maestra, lo que le permite definir contenido personalizado para ese control de contenido determinado.

![Establecer un control de contenido en Default en el contenido de las páginas maestras](master-pages/_static/image4.gif)

**Figura 7**: Establecer un control de contenido en Default para el contenido de las páginas maestras

## <a name="connecting-master-page-and-content-pages"></a>Conexión de páginas maestras y páginas de contenido

La asociación entre una página maestra y una página de contenido se puede configurar de una de cuatro maneras diferentes:

- El atributo <strong>MasterPageFile</strong> de la @Page directiva
- Establecer el **Page.MasterPageFile** propiedad en el código.
- El elemento ** &lt;pages&gt; ** del archivo de configuración de aplicaciones (web.config en la carpeta raíz de la aplicación)
- El elemento ** &lt;pages&gt; ** de un archivo de configuración de subcarpetas (web.config en una subcarpeta)

## <a name="masterpagefile-attribute"></a>Atributo MasterPageFile

El atributo MasterPageFile facilita la aplicación de una página maestra a una página de ASP.NET determinada. También es el método utilizado para aplicar la página maestra al marcar la casilla **Seleccionar página maestra** como lo hizo en el Ejercicio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Establecer Page.MasterPageFile en código

Al establecer la propiedad MasterPageFile en el código, puede aplicar una página maestra determinada al contenido en tiempo de ejecución. Esto es útil en los casos en los que es posible que deba aplicar una página maestra específica basada en un rol de usuario o algún otro criterio. El MasterPageFile propiedad debe establecerse en el PreInit método. Si se establece después de la PreInit método, un InvalidOperationException se producirá. La página en la que se establece esta propiedad también debe tener un control Content como control de nivel superior para la página. De lo contrario, se producirá una excepción HttpException cuando se establezca la propiedad MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Uso &lt;de&gt; las páginas Element

Puede configurar una página maestra para las páginas estableciendo el atributo masterPageFile en el &lt;elemento pages&gt; del archivo web.config. Al usar este método, tenga en cuenta que los archivos web.config inferiores en la estructura de la aplicación pueden invalidar esta configuración. Cualquier atributo MasterPageFile @Page establecido en una directiva también invalidará esta configuración. El &lt;uso&gt; del elemento pages facilita la creación de *una* página maestra maestra que se puede invalidar si es necesario en determinadas carpetas o archivos.

## <a name="properties-in-master-pages"></a>Propiedades en páginas maestras

Una página maestra puede exponer propiedades simplemente haciendo que esas propiedades sean públicas dentro de la página maestra. Por ejemplo, el código siguiente define una propiedad denominada SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Para tener acceso a la propiedad SomeProperty desde la página Contenido, deberá usar la propiedad Master de la siguiente forma:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Anidar páginas maestras

Las páginas maestras son la solución perfecta para garantizar un aspecto común en una aplicación web grande. Sin embargo, no es raro que ciertas partes de un sitio grande compartan una interfaz común, mientras que otras partes comparten una interfaz diferente. Para abordar esa necesidad, varias páginas maestras son la solución perfecta. Sin embargo, eso todavía no aborda el hecho de que una aplicación grande puede tener ciertos componentes (como un menú, por ejemplo) que se comparten entre todas las páginas y otros componentes que se comparten solo entre ciertas secciones del sitio. Para ese tipo de situación, las páginas maestras anidadas llenan la necesidad muy bien. Como ha visto, una página maestra normal consta de una página maestra y una página de contenido. En una situación de página maestra anidada, hay dos páginas maestras; un maestro padre y un maestro de niños. La página maestra secundaria también es una página de contenido y su maestro es la página maestra principal.

Aquí está el código para una página maestra típica:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

En un escenario maestro anidado, este sería el maestro primario. Otra página maestra usaría esta página como su página maestra, y ese código tendría este aspecto:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Tenga en cuenta que en este escenario, el maestro secundario también es una página de contenido para el maestro primario. Todo el contenido del maestro secundario aparece dentro de un control Content que obtiene su contenido del control ContentPlaceHolder del elemento primario.

> [!NOTE]
> La compatibilidad con Designer no está disponible para las páginas maestras anidadas. Cuando se desarrolla mediante patrones anidados, deberá usar la vista de origen.

Este vídeo muestra un tutorial sobre el uso de páginas maestras anidadas.

![](master-pages/_static/image1.png)

[Abrir vídeo a pantalla completa](master-pages/_static/nested1.wmv)

![Selección de una página maestra](master-pages/_static/image4.jpg)

**Figura 8**: Selección de una página maestra
