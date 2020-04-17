---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Mejorar el rendimiento con el almacenamiento en caché de salidas (C-) Microsoft Docs
author: rick-anderson
description: En este tutorial, aprenderá cómo puede mejorar drásticamente el rendimiento de las aplicaciones web ASP.NET MVC aprovechando el almacenamiento en caché de resultados. tú...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: a6dd43320117e235d12a13aa894302bbc767727c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542721"
---
# <a name="improving-performance-with-output-caching-c"></a>Mejorar el rendimiento con el almacenamiento en caché de resultados (C#)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá cómo puede mejorar drásticamente el rendimiento de las aplicaciones web ASP.NET MVC aprovechando el almacenamiento en caché de resultados. Aprenderá a almacenar en caché el resultado devuelto por una acción de controlador para que no sea necesario crear el mismo contenido cada vez que un nuevo usuario invoca la acción.

El objetivo de este tutorial es explicar cómo puede mejorar drásticamente el rendimiento de una aplicación ASP.NET MVC aprovechando la caché de resultados. La memoria caché de salida le permite almacenar en caché el contenido devuelto por una acción de controlador. De este modo, no es necesario generar el mismo contenido cada vez que se invoca la misma acción de controlador.

Imagine, por ejemplo, que la aplicación MVC de ASP.NET muestra una lista de registros de base de datos en una vista denominada index. Normalmente, cada vez que un usuario invoca la acción de controlador que devuelve la vista de índice, el conjunto de registros de base de datos debe recuperarse de la base de datos mediante la ejecución de una consulta de base de datos.

Si, por otro lado, aprovecha la memoria caché de resultados, puede evitar ejecutar una consulta de base de datos cada vez que un usuario invoca la misma acción de controlador. La vista se puede recuperar de la memoria caché en lugar de regenerarse desde la acción del controlador. El almacenamiento en caché le permite evitar realizar trabajos redundantes en el servidor.

## <a name="enabling-output-caching"></a>Habilitación del almacenamiento en caché de salida

Habilitar el almacenamiento en caché de resultados agregando un [OutputCache] atributo a una acción de controlador individual o una clase de controlador completa. Por ejemplo, el controlador del listado 1 expone una acción denominada Index(). La salida de la acción Index() se almacena en caché durante 10 segundos.

**Listado 1 – Controllers-HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

En las versiones Beta de ASP.NET MVC, el [http://www.MySite.com/](http://www.mysite.com/)almacenamiento en caché de resultados no funciona para una dirección URL como . En su lugar, debe [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)introducir una dirección URL como . 

En el listado 1, la salida de la acción Index() se almacena en caché durante 10 segundos. Si lo prefiere, puede especificar una duración de caché mucho más larga. Por ejemplo, si desea almacenar en caché la salida de una acción de controlador durante un día, \* puede \* especificar una duración de caché de 86400 segundos (60 segundos 60 minutos 24 horas).

No hay ninguna garantía de que el contenido se almacenará en caché durante el tiempo que especifique. Cuando los recursos de memoria se vuelven bajos, la memoria caché comienza a expulsar contenido automáticamente.

El controlador de inicio en el listado 1 devuelve la vista de índice en el listado 2. No hay nada especial en este punto de vista. La vista de índice simplemente muestra la hora actual (consulte la figura 1).

**Listado 2 – Vistas-Home-Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – Vista de índice en caché**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Si invoca la acción Index() varias veces introduciendo la URL /Home/Index en la barra de direcciones de su navegador y pulsando el botón Actualizar/Recargar en el explorador repetidamente, el tiempo mostrado por la vista de índice no cambiará durante 10 segundos. Se muestra la misma hora porque la vista se almacena en caché.

Es importante comprender que la misma vista se almacena en caché para todos los que visitan la aplicación. Cualquier persona que invoque la acción Index() obtendrá la misma versión almacenada en caché de la vista Index. Esto significa que la cantidad de trabajo que el servidor web debe realizar para servir a la vista de índice se reduce drásticamente.

La vista en el listado 2 resulta estar haciendo algo muy simple. La vista solo muestra la hora actual. Sin embargo, podría almacenar en caché fácilmente una vista que muestra un conjunto de registros de base de datos. En ese caso, no es necesario recuperar el conjunto de registros de base de datos de la base de datos cada vez que se invoca la acción del controlador que devuelve la vista. El almacenamiento en caché puede reducir la cantidad de trabajo que deben realizar tanto el servidor web como el servidor de bases de datos.

No use la &lt;directiva de la&gt; página %- OutputCache % en una vista MVC. Esta directiva está sangrando desde el mundo de formularios Web Forms y no se debe usar en una aplicación ASP.NET MVC.

## <a name="where-content-is-cached"></a>Donde el contenido se almacena en caché

De forma predeterminada, cuando se utiliza el atributo [OutputCache], el contenido se almacena en caché en tres ubicaciones: el servidor web, los servidores proxy y el explorador web. Puede controlar exactamente dónde se almacena en caché el contenido modificando la propiedad Location del atributo [OutputCache].

Puede establecer la propiedad Location en cualquiera de los siguientes valores:

> · Cualquier
> 
> · Cliente
> 
> · Downstream
> 
> · Servidor
> 
> · Ninguno
> 
> · ServerAndClient

De forma predeterminada, la propiedad Location tiene el valor Any. Sin embargo, hay situaciones en las que es posible que desee almacenar en caché solo en el explorador o solo en el servidor. Por ejemplo, si va a almacenar en caché la información personalizada para cada usuario, no debe almacenar en caché la información en el servidor. Si está mostrando información diferente a diferentes usuarios, debe almacenar en caché la información solo en el cliente.

Por ejemplo, el controlador del listado 3 expone una acción denominada GetName() que devuelve el nombre de usuario actual. Si Jack inicia sesión en el sitio web e invoca la acción GetName(), la acción devuelve la cadena "Hi Jack". Si, posteriormente, Jill inicia sesión en el sitio web e invoca la acción GetName(), también obtendrá la cadena "Hi Jack". La cadena se almacena en caché en el servidor web para todos los usuarios después de que Jack invoca inicialmente la acción del controlador.

**Listado 3 – Controllers-BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Lo más probable es que el controlador en el listado 3 no funciona de la manera que desea. No querrás mostrar el mensaje "Hola Jack" a Jill.

Nunca debe almacenar en caché el contenido personalizado en la memoria caché del servidor. Sin embargo, es posible que desee almacenar en caché el contenido personalizado en la caché del explorador para mejorar el rendimiento. Si almacena en caché el contenido en el explorador y un usuario invoca la misma acción de controlador varias veces, el contenido se puede recuperar de la caché del explorador en lugar del servidor.

El controlador modificado en el listado 4 almacena en caché la salida de la acción GetName(). Sin embargo, el contenido se almacena en caché solo en el explorador y no en el servidor. De este modo, cuando varios usuarios invocan el método GetName(), cada persona obtiene su propio nombre de usuario y no el nombre de usuario de otra persona.

**Listado 4 – Controllers-UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Observe que el atributo [OutputCache] del listado 4 incluye una propiedad Location establecida en el valor OutputCacheLocation.Client. El atributo [OutputCache] también incluye una propiedad NoStore. La propiedad NoStore se utiliza para informar a los servidores proxy y al explorador de que no deben almacenar una copia permanente del contenido almacenado en caché.

## <a name="varying-the-output-cache"></a>Variación de la caché de salida

En algunas situaciones, es posible que desee diferentes versiones almacenadas en caché del mismo contenido. Imagine, por ejemplo, que está creando una página maestra/detalle. La página maestra muestra una lista de títulos de películas. Al hacer clic en un título, obtendrá los detalles de la película seleccionada.

Si almacena en caché la página de detalles, los detalles de la misma película se mostrarán independientemente de la película en la que haga clic. La primera película seleccionada por el primer usuario se mostrará a todos los usuarios futuros.

Puede solucionar este problema aprovechando la propiedad VaryByParam del atributo [OutputCache]. Esta propiedad le permite crear diferentes versiones almacenadas en caché del mismo contenido cuando varía un parámetro de formulario o un parámetro de cadena de consulta.

Por ejemplo, el controlador del listado 5 expone dos acciones denominadas Master() y Details(). La acción Master() devuelve una lista de títulos de películas y la acción Details() devuelve los detalles de la película seleccionada.

**Listado 5 – Controllers-MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

La acción Master() incluye una propiedad VaryByParam con el valor "none". Cuando se invoca la acción Master(), se devuelve la misma versión almacenada en caché de la vista maestra. Se omiten los parámetros de formulario o los parámetros de cadena de consulta (consulte la figura 2).

**Figura 2 – La vista /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – La vista /Movies/Details**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

La acción Details() incluye una propiedad VaryByParam con el valor "Id". Cuando se pasan diferentes valores del parámetro Id a la acción del controlador, se generan diferentes versiones almacenadas en caché de la vista Detalles.

Es importante comprender que el uso de la VaryByParam propiedad da como resultado más almacenamiento en caché y no menos. Se crea una versión almacenada en caché diferente de la vista Detalles para cada versión diferente del parámetro Id.

Puede establecer la propiedad VaryByParam en los siguientes valores:

> \*• Cree una versión almacenada en caché diferente siempre que un parámetro de cadena de formulario o consulta varíe.
> 
> ninguno : Nunca cree diferentes versiones almacenadas en caché
> 
> Lista de parámetros de punto y coma: cree diferentes versiones almacenadas en caché siempre que cualquiera de los parámetros de forma o cadena de consulta de la lista varíe

## <a name="creating-a-cache-profile"></a>Creación de un perfil de caché

Como alternativa a la configuración de propiedades de caché de resultados mediante la modificación de las propiedades del atributo [OutputCache], puede crear un perfil de caché en el archivo de configuración web (web.config). La creación de un perfil de caché en el archivo de configuración web ofrece un par de ventajas importantes.

En primer lugar, al configurar el almacenamiento en caché de resultados en el archivo de configuración web, puede controlar cómo las acciones del controlador almacenan en caché el contenido en una ubicación central. Puede crear un perfil de caché y aplicar el perfil a varios controladores o acciones de controlador.

En segundo lugar, puede modificar el archivo de configuración web sin volver a compilar la aplicación. Si necesita deshabilitar el almacenamiento en caché de una aplicación que ya se ha implementado en producción, simplemente puede modificar los perfiles de caché definidos en el archivo de configuración web. Cualquier cambio en el archivo de configuración web se detectará automáticamente y se aplicará.

Por ejemplo, &lt;la&gt; sección de configuración web de almacenamiento en caché del listado 6 define un perfil de caché denominado Cache1Hour. La &lt;&gt; sección de almacenamiento &lt;en caché&gt; debe aparecer dentro de la sección system.web de un archivo de configuración web.

**Listado 6 – Sección de almacenamiento en caché para web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

El controlador del listado 7 ilustra cómo puede aplicar el perfil Cache1Hour a una acción de controlador con el atributo [OutputCache].

**Listado 7 – Controllers-ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Si invoca la acción Index() expuesta por el controlador en el listado 7, se devolverá la misma hora durante 1 hora.

## <a name="summary"></a>Resumen

El almacenamiento en caché de resultados proporciona un método muy fácil de mejorar drásticamente el rendimiento de las aplicaciones MVC de ASP.NET. En este tutorial, aprendió a usar el atributo [OutputCache] para almacenar en caché la salida de las acciones del controlador. También aprendió a modificar las propiedades del atributo [OutputCache], como las propiedades Duration y VaryByParam, para modificar cómo se almacena en caché el contenido. Por último, aprendió a definir perfiles de caché en el archivo de configuración web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-cs.md)
> [Siguiente](adding-dynamic-content-to-a-cached-page-cs.md)
