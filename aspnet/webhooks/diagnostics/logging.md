---
uid: webhooks/diagnostics/logging
title: ASP.NET el registro de WebHooks ( WebHooks) Microsoft Docs
author: rick-anderson
description: Cómo iniciar sesión en ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 350732acbd3a73bddb8f8b20dcd50c225d89be82
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675342"
---
# <a name="aspnet-webhooks-logging"></a>registro de ASP.NET WebHooks

Microsoft ASP.NET WebHooks usa el registro como una forma de informar de problemas y problemas. De forma predeterminada, los registros se escriben mediante [System.Diagnostics.Trace,](https://msdn.microsoft.com/library/system.diagnostics.trace) donde se pueden registrar mediante [agentes de escucha](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) de seguimiento como cualquier otra secuencia de registro.

Al implementar la aplicación web como una aplicación web de Azure, los registros se seleccionan automáticamente y se pueden administrar junto con cualquier otro registro [System.Diagnostics.Trace.](https://msdn.microsoft.com/library/system.diagnostics.trace) Para obtener más información, consulte Habilitar el registro de [diagnósticos para aplicaciones web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Además, los registros se pueden obtener directamente desde dentro de Visual Studio como se describe en Solucionar problemas de [una aplicación web en Azure App Service mediante Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirección de registros

En lugar de escribir registros en [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es posible proporcionar una implementación de registro alternativa que pueda iniciar sesión directamente en un administrador de registros como [Log4Net](http://logging.apache.org/log4net/) y [NLog](http://nlog-project.org/). Simplemente proporcione una implementación de [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) y regístrelo con un motor de inyección de dependencias de su elección y será recogido por Microsoft ASP.NET WebHooks. Consulte [Inyección](https://www.asp.net/web-api/overview/advanced/dependency-injection) de dependencias en ASP.NET Web API 2 para obtener más información.
