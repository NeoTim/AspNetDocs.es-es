---
title: Versión de compatibilidad para ASP.NET Core MVC
author: rick-anderson
description: Descubra cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048022"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Versión de compatibilidad para ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El método <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permite a una aplicación participar o no en los cambios de comportamiento importantes incorporados en ASP.NET Core MVC 2.1 o una versión posterior. Estos cambios de comportamiento importantes suelen estar relacionados con cómo se comporta el subsistema de MVC y cómo el tiempo de ejecución llama al **código**. Si la aplicación participa, obtendrá el comportamiento más reciente y a largo plazo de ASP.NET Core.

El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Le recomendamos que pruebe la aplicación con la versión más reciente (`CompatibilityVersion.Version_2_2`). Prevemos que la mayoría de las aplicaciones no tendrán cambios de comportamiento importantes usando la versión más reciente.

Las aplicaciones que llaman a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` están protegidas frente a los cambios de comportamiento importantes incorporados en ASP.NET Core 2.1 MVC y versiones 2.x posteriores. Esta protección:

* No es aplicable a todos los cambios de 2.1 y versiones posteriores, sino que tiene como destino los cambios importantes de comportamiento en tiempo de ejecución de ASP.NET Core en el subsistema de MVC.
* No se extiende a la siguiente versión principal.

La compatibilidad predeterminada de las aplicaciones ASP.NET Core 2.1 y versiones 2.x posteriores que **no** llaman a `SetCompatibilityVersion` es la compatibilidad 2.0. Es decir, no llamar a `SetCompatibilityVersion` es igual que llamar a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2, salvo en los siguientes comportamientos:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

En el caso de las aplicaciones que encuentran cambios de comportamiento importantes, si se usan los modificadores de compatibilidad adecuados:

* Se podrá usar la versión más reciente y descartar cambios de comportamiento importantes específicos.
* Se dispondrá de tiempo para actualizar la aplicación para que funcione con los cambios más recientes.

En la documentación de <xref:Microsoft.AspNetCore.Mvc.MvcOptions> se incluye una completa explicación de los cambios y por qué son una mejora para la mayoría de los usuarios.

Próximamente habrá una [versión ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Los comportamientos anteriores admitidos por los modificadores de compatibilidad se quitarán en esta versión 3.0. Estamos convencidos de que estos son cambios positivos que beneficiarán a prácticamente todos los usuarios. Al presentarlos ahora, la mayoría de las aplicaciones podrán empezar a sacar partido de ellos ya y los demás tendrán tiempo suficiente para actualizar sus aplicaciones.
