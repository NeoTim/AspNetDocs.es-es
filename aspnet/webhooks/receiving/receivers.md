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
# <a name="aspnet-webhooks-receivers"></a>receptores ASP.NET WebHooks

La recepción de WebHooks depende de quién sea el remitente. A veces hay pasos adicionales que registran un WebHook verificando que el suscriptor está realmente escuchando. Algunos WebHooks proporcionan un modelo de inserción a extracción donde la solicitud HTTP POST solo contiene una referencia a la información del evento que, a continuación, se va a recuperar de forma independiente. A menudo, el modelo de seguridad varía bastante.

El propósito de Microsoft ASP.NET WebHooks es hacer que sea más simple y más coherente conectar la API sin pasar mucho tiempo averiguando cómo manejar cualquier variante particular de WebHooks.

Un receptor de WebHook es responsable de aceptar y verificar WebHooks de un remitente determinado. Un receptor WebHook puede admitir cualquier número de WebHooks, cada uno con su propia configuración. Por ejemplo, el receptor de GitHub WebHook puede aceptar WebHooks desde cualquier número de repositorios de GitHub.

## <a name="webhook-receiver-uris"></a>URI de receptor de WebHook

Al instalar Microsoft ASP.NET WebHooks se obtiene un controlador WebHook general que acepta solicitudes de WebHook de un número abierto de servicios. Cuando llega una solicitud, selecciona el receptor adecuado que ha instalado para controlar un remitente de WebHook determinado.

El URI de este controlador es el URI de WebHook que se registra con el servicio y tiene el formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de seguridad, muchos receptores de WebHook requieren que el URI sea un URI *https* y, en algunos casos, también debe contener un parámetro de consulta adicional que se usa para exigir que solo la entidad deseada pueda enviar WebHooks al URI anterior.

El `<receiver>` componente es el nombre del `github` `slack`receptor, por ejemplo o .

El *identificador* es un identificador opcional que se puede utilizar para identificar una configuración de receptor de WebHook determinada. Esto se puede utilizar para registrar N WebHooks con un receptor determinado. Por ejemplo, los tres URI siguientes se pueden utilizar para registrarse en tres WebHooks independientes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalación de un receptor WebHook

Para recibir WebHooks mediante Microsoft ASP.NET WebHooks, primero debe instalar el paquete Nuget para el proveedor de WebHook o los proveedores de los que desea recibir WebHooks. Los paquetes Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido. Por ejemplo

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para recibir WebHooks desde GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para recibir WebHooks generados por ASP.NET WebHooks.

Fuera de la caja puedes encontrar soporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.

## <a name="configuring-a-webhook-receiver"></a>Configuración de un receptor WebHook

Los receptores de WebHook se configuran a través de la interfaz [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) y se pueden registrar implementaciones concretas de esa interfaz mediante cualquier modelo de inserción de dependencias. La implementación predeterminada usa la configuración de la aplicación, que se puede establecer en el archivo Web.config o, si se usa Azure Web Apps, se puede establecer a través de [Azure Portal](https://portal.azure.com/).

![Configuración de aplicaciones web](_static/AzureAppSettings.png)

El formato de las teclas de configuración de la aplicación es el siguiente:

```
MS_WebHookReceiverSecret_<receiver>
```

El valor es una lista separada por comas de valores que coinciden con *los* valores de la dirección de correo electrónico para los que se han registrado WebHooks, por ejemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicialización de un receptor de WebHook

Los receptores de WebHook se inicializan registrándolos, normalmente en la clase estática *WebApiConfig,* por ejemplo:

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
