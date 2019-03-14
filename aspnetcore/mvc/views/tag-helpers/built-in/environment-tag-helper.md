---
title: Asistente de etiquetas de entorno en ASP.NET Core
author: pkellner
description: Asistente de etiquetas de entorno de ASP.NET Core definida con todas las propiedades
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028632"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Asistente de etiquetas de entorno en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya) y [Luke Latham](https://github.com/guardrex)

El asistente de etiquetas de entorno representa condicionalmente el contenido incluido en función del entorno de [hospedaje actual](xref:fundamentals/environments). El único atributo del asistente de etiquetas de entorno, `names`, es una lista de nombres de entorno separados por comas. Si alguno de los nombres de entorno proporcionados coincide con el entorno actual, se representa el contenido incluido.

Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Atributos del asistente de etiquetas de entorno

### <a name="names"></a>nombres

`names` acepta un solo nombre de entorno de hospedaje o una lista de nombres de entorno de hospedaje separados por comas que desencadenan la representación del contenido incluido.

Los valores de entorno se comparan con el valor actual devuelto por [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*). La comparación ignora el uso de mayúsculas y minúsculas.

En este ejemplo se usa un asistente de etiquetas de entorno. El contenido se representa si el entorno de hospedaje es de almacenamiento provisional o de producción:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>Atributos include y exclude

Los atributos `include` & `exclude` controlan la representación del contenido incluido en función de los nombres de entorno de hospedaje incluidos o excluidos.

### <a name="include"></a>include

La propiedad `include` exhibe un comportamiento similar al atributo `names`. Un entorno que se muestra en el valor de atributo `include` debe coincidir con el entorno de hospedaje de la aplicación ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) para representar el contenido de la etiqueta `<environment>`.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>exclude

A diferencia del atributo `include`, el contenido de la etiqueta `<environment>` se representa cuando el entorno de hospedaje no coincide con un entorno que se muestra en el valor de atributo `exclude`.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/environments>
