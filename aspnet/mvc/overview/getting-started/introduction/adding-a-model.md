---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Agregar un modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 453a55bd9f16c7b33525de18bc49493d22776f47
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045122"
---
# <a name="adding-a-model"></a>Agregar un modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

En esta sección, agregará algunas clases para administrar películas en una base de datos de. Estas clases serán la &quot; parte del modelo &quot; de la aplicación ASP.NET MVC.

Usará una .NET Framework tecnología de acceso a datos conocida como [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*. Code First permite crear objetos de modelo escribiendo clases simples. (También se conocen como clases POCO, a partir de &quot; objetos CLR antiguos). &quot; Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio. Si necesita crear primero la base de datos, puede seguir este tutorial para obtener información sobre el desarrollo de aplicaciones MVC y EF. Después, puede seguir el tutorial de [scaffolding Fizmakens ASP.net](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) , que cubre el primer enfoque de la base de datos.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el nombre de *clase* &quot; Movie &quot; .

Agregue las siguientes cinco propiedades a la `Movie` clase:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos la `Movie` clase para representar películas en una base de datos. Cada instancia de un `Movie` objeto se corresponderá con una fila de una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna de la tabla.

Nota: para poder usar System. Data. Entity y la clase relacionada, debe instalar el [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga el vínculo para obtener más instrucciones.

En el mismo archivo, agregue la siguiente `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

La `MovieDBContext` clase representa el contexto de la base de datos de Entity Framework Movie, que controla la captura, el almacenamiento y la actualización `Movie` de instancias de clase en una base de datos. `MovieDBContext`Deriva de la `DbContext` clase base proporcionada por el Entity Framework.

Para poder hacer referencia a `DbContext` y, debe `DbSet` Agregar la siguiente `using` instrucción en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Para ello, puede Agregar manualmente la instrucción using o puede mantener el mouse sobre las líneas onduladas rojas, hacer clic `Show potential fixes` y hacer clic en `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: `using` se han quitado varias instrucciones no utilizadas. Visual Studio mostrará las dependencias no usadas en gris. Para quitar las dependencias no utilizadas, mantenga el mouse sobre las dependencias grises, haga clic `Show potential fixes` y haga clic en **quitar las utilizaciones no utilizadas.**

![](adding-a-model/_static/image3.png)

Por último, hemos agregado un modelo (la M en MVC). En la siguiente sección, trabajará con la cadena de conexión de base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)
