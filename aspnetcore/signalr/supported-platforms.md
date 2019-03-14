---
title: Plataformas compatibles con ASP.NET Core SignalR
author: bradygaster
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: e4e84baf0120036b473eac256107b46a4accfe37
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57053392"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Plataformas compatibles con ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor

SignalR para ASP.NET Core es compatible con cualquier plataforma de servidor ASP.NET Core admite.

## <a name="javascript-client"></a>Cliente de JavaScript

El [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores y los siguientes exploradores:

| Explorador                         | Versión |
| ------------------------------- | ------- |
| Microsoft Edge                  | actuales |
| Mozilla Firefox                 | actuales |
| Google Chrome; incluye Android | actuales |
| Safari; incluye iOS            | actuales |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Cliente .NET

El [cliente.NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma compatible con ASP.NET Core. Por ejemplo, [los desarrolladores de Xamarin pueden usar SignalR](https://github.com/aspnet/Announcements/issues/305) para la creación de aplicaciones de Android con Xamarin.Android 8.4.0.1 y versiones posteriores y en aplicaciones de iOS con Xamarin.iOS 11.14.0.4 y versiones posteriores.

Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8.0 o posterior en Windows Server 2012 o posterior. Otros transportes se admiten en todas las plataformas.

## <a name="java-client"></a>Cliente de Java

El [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) admite Java 8 y versiones posteriores.

## <a name="unsupported-clients"></a>Clientes no compatibles

Los clientes siguientes están disponibles, pero son experimentales o no oficiales. Que no se admiten actualmente y nunca pueden ser.

* [Cliente de C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Cliente de SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
