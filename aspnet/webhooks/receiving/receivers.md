---
uid: webhooks/receiving/receivers
title: Receptores de ASP.NET WebHooks ( WebHooks) Microsoft Docs
author: rick-anderson
description: receptores ASP.NET WebHooks
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 60f46141b59fc3888a6480d8201160420469d1a7
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675160"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="1f4f1-103">receptores ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="1f4f1-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="1f4f1-104">La recepción de WebHooks depende de quién sea el remitente.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="1f4f1-105">A veces hay pasos adicionales que registran un WebHook verificando que el suscriptor está realmente escuchando.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="1f4f1-106">Algunos WebHooks proporcionan un modelo de inserción a extracción donde la solicitud HTTP POST solo contiene una referencia a la información del evento que, a continuación, se va a recuperar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="1f4f1-107">A menudo, el modelo de seguridad varía bastante.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="1f4f1-108">El propósito de Microsoft ASP.NET WebHooks es hacer que sea más simple y más coherente conectar la API sin pasar mucho tiempo averiguando cómo manejar cualquier variante particular de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="1f4f1-109">Un receptor de WebHook es responsable de aceptar y verificar WebHooks de un remitente determinado.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="1f4f1-110">Un receptor WebHook puede admitir cualquier número de WebHooks, cada uno con su propia configuración.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="1f4f1-111">Por ejemplo, el receptor de GitHub WebHook puede aceptar WebHooks desde cualquier número de repositorios de GitHub.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="1f4f1-112">URI de receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="1f4f1-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="1f4f1-113">Al instalar Microsoft ASP.NET WebHooks se obtiene un controlador WebHook general que acepta solicitudes de WebHook de un número abierto de servicios.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="1f4f1-114">Cuando llega una solicitud, selecciona el receptor adecuado que ha instalado para controlar un remitente de WebHook determinado.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="1f4f1-115">El URI de este controlador es el URI de WebHook que se registra con el servicio y tiene el formato:</span><span class="sxs-lookup"><span data-stu-id="1f4f1-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="1f4f1-116">Por motivos de seguridad, muchos receptores de WebHook requieren que el URI sea un URI *https* y, en algunos casos, también debe contener un parámetro de consulta adicional que se usa para exigir que solo la entidad deseada pueda enviar WebHooks al URI anterior.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="1f4f1-117">El `<receiver>` componente es el nombre del `github` `slack`receptor, por ejemplo o .</span><span class="sxs-lookup"><span data-stu-id="1f4f1-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="1f4f1-118">El *identificador* es un identificador opcional que se puede utilizar para identificar una configuración de receptor de WebHook determinada.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="1f4f1-119">Esto se puede utilizar para registrar N WebHooks con un receptor determinado.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="1f4f1-120">Por ejemplo, los tres URI siguientes se pueden utilizar para registrarse en tres WebHooks independientes:</span><span class="sxs-lookup"><span data-stu-id="1f4f1-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="1f4f1-121">Instalación de un receptor WebHook</span><span class="sxs-lookup"><span data-stu-id="1f4f1-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="1f4f1-122">Para recibir WebHooks mediante Microsoft ASP.NET WebHooks, primero debe instalar el paquete Nuget para el proveedor de WebHook o los proveedores de los que desea recibir WebHooks.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="1f4f1-123">Los paquetes Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="1f4f1-124">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="1f4f1-124">For example</span></span>

<span data-ttu-id="1f4f1-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para recibir WebHooks desde GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para recibir WebHooks generados por ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="1f4f1-126">Fuera de la caja puedes encontrar soporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="1f4f1-127">Configuración de un receptor WebHook</span><span class="sxs-lookup"><span data-stu-id="1f4f1-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="1f4f1-128">Los receptores de WebHook se configuran a través de la interfaz [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) y se pueden registrar implementaciones concretas de esa interfaz mediante cualquier modelo de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="1f4f1-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="1f4f1-129">La implementación predeterminada usa la configuración de la aplicación, que se puede establecer en el archivo Web.config o, si se usa Azure Web Apps, se puede establecer a través de [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1f4f1-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configuración de aplicaciones web](_static/AzureAppSettings.png)

<span data-ttu-id="1f4f1-131">El formato de las teclas de configuración de la aplicación es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="1f4f1-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="1f4f1-132">El valor es una lista separada por comas de valores que coinciden con *los* valores de la dirección de correo electrónico para los que se han registrado WebHooks, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1f4f1-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="1f4f1-133">Inicialización de un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="1f4f1-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="1f4f1-134">Los receptores de WebHook se inicializan registrándolos, normalmente en la clase estática *WebApiConfig,* por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1f4f1-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
