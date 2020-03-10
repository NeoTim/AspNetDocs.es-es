---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de la seguridad en una aplicación Signalr,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431191"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="35f19-104">Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="35f19-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="35f19-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="35f19-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="35f19-106">En este tema se describe cómo aplicar la autorización en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="35f19-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="35f19-107">Para obtener información general sobre la integración de la seguridad en una aplicación de Signalr, consulte [Introducción a la seguridad](index.md).</span><span class="sxs-lookup"><span data-stu-id="35f19-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="35f19-108">Exigir autorización</span><span class="sxs-lookup"><span data-stu-id="35f19-108">Enforce authorization</span></span>

<span data-ttu-id="35f19-109">Para aplicar reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , debe invalidar el método `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="35f19-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="35f19-110">No se puede usar el atributo `Authorize` con conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="35f19-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="35f19-111">Signalr Framework llama al método `AuthorizeRequest` antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="35f19-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="35f19-112">No se llama al método `AuthorizeRequest` desde el cliente; en su lugar, se autentica al usuario a través del mecanismo de autenticación estándar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="35f19-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="35f19-113">En el ejemplo siguiente se muestra cómo limitar las solicitudes a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="35f19-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="35f19-114">Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest. por ejemplo, comprobar si un usuario pertenece a un rol determinado.</span><span class="sxs-lookup"><span data-stu-id="35f19-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
