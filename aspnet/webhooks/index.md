---
uid: webhooks/index
title: Descripción general de ASP.NET WebHooks ( WebHooks) Microsoft Docs
author: rick-anderson
description: Una introducción a ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa5fa190386ec803a6801de2d815c948677fe1f5
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675441"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="0a064-103">Visión general de ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="0a064-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="0a064-104">WebHooks es un patrón HTTP ligero que proporciona un modelo de pub/sub simple para conectar las API web y los servicios SaaS.</span><span class="sxs-lookup"><span data-stu-id="0a064-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="0a064-105">Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados.</span><span class="sxs-lookup"><span data-stu-id="0a064-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="0a064-106">La solicitud POST contiene información sobre el evento que permite que el receptor actúe en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="0a064-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="0a064-107">Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios, incluyendo [Dropbox,](http://dropbox.com/) [GitHub,](https://www.github.com/) [Bitbucket,](https://bitbucket.org/) [MailChimp,](http://www.mailchimp.com/) [PayPal,](http://www.paypal.com/) [Slack,](http://www.slack.com) [Stripe,](http://www.stripe.com) [Trello](http://www.trello.com/)y muchos más.</span><span class="sxs-lookup"><span data-stu-id="0a064-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="0a064-108">Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), o se ha confirmado un cambio de código en GitHub, o se ha iniciado un pago en [PayPal](http://www.paypal.com/)o se ha creado una tarjeta en [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="0a064-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="0a064-109">¡Las posibilidades son infinitas!</span><span class="sxs-lookup"><span data-stu-id="0a064-109">The possibilities are endless!</span></span>

<span data-ttu-id="0a064-110">Microsoft ASP.NET WebHooks facilita el envío y la recepción de WebHooks como parte de la aplicación ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="0a064-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="0a064-111">En el lado de recepción, proporciona un modelo común para recibir y procesar WebHooks desde cualquier número de proveedores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="0a064-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="0a064-112">Viene de la caja con soporte para [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) y [Zendesk,](https://www.zendesk.com/) pero es fácil agregar soporte para más.</span><span class="sxs-lookup"><span data-stu-id="0a064-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="0a064-113">En el lado de envío proporciona soporte para administrar y almacenar suscripciones, así como para enviar notificaciones de eventos al conjunto correcto de suscriptores.</span><span class="sxs-lookup"><span data-stu-id="0a064-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="0a064-114">Esto le permite definir su propio conjunto de eventos a los que los suscriptores pueden suscribirse y notificarles cuando suceden las cosas.</span><span class="sxs-lookup"><span data-stu-id="0a064-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="0a064-115">Las dos partes se pueden utilizar juntas o separadas dependiendo de su escenario.</span><span class="sxs-lookup"><span data-stu-id="0a064-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="0a064-116">Si solo necesita recibir WebHooks de otros servicios, puede usar solo la parte del receptor; si solo desea exponer WebHooks para que otros lo consuman, entonces puede hacer eso.</span><span class="sxs-lookup"><span data-stu-id="0a064-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="0a064-117">El código tiene como destino ASP.NET Web API 2 y ASP.NET MVC 5 y está disponible como [OSS en GitHub.](https://github.com/aspnet/WebHooks)</span><span class="sxs-lookup"><span data-stu-id="0a064-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="0a064-118">Descripción general de WebHooks</span><span class="sxs-lookup"><span data-stu-id="0a064-118">WebHooks Overview</span></span>

<span data-ttu-id="0a064-119">WebHooks es un patrón que significa que varía cómo se utiliza de servicio a servicio, pero la idea básica es la misma.</span><span class="sxs-lookup"><span data-stu-id="0a064-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="0a064-120">Puede pensar en WebHooks como un modelo simple de pub/sub donde un usuario puede suscribirse a eventos que ocurren en otro lugar.</span><span class="sxs-lookup"><span data-stu-id="0a064-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="0a064-121">Las notificaciones de eventos se propagan como solicitudes HTTP POST que contienen información sobre el propio evento.</span><span class="sxs-lookup"><span data-stu-id="0a064-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="0a064-122">Normalmente, la solicitud HTTP POST contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook, incluida información sobre el evento que hace que WebHook se desencadene.</span><span class="sxs-lookup"><span data-stu-id="0a064-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="0a064-123">Por ejemplo, un cuerpo de solicitud POST de WebHook de [GitHub](https://www.github.com/) tiene este aspecto como resultado de un nuevo problema que se abre en un repositorio determinado:</span><span class="sxs-lookup"><span data-stu-id="0a064-123">For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="0a064-124">Para asegurarse de que el WebHook es de hecho del remitente previsto, la solicitud POST se protege de alguna manera y, a continuación, verifica da la seguridad del receptor.</span><span class="sxs-lookup"><span data-stu-id="0a064-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="0a064-125">Por ejemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) incluye un encabezado HTTP *X-Hub-Signature* con un hash del cuerpo de la solicitud que comprueba la implementación del receptor para que no tenga que preocuparse por él.</span><span class="sxs-lookup"><span data-stu-id="0a064-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="0a064-126">El flujo de WebHook generalmente va algo como esto:</span><span class="sxs-lookup"><span data-stu-id="0a064-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="0a064-127">El remitente de WebHook expone eventos a los que un cliente puede suscribirse.</span><span class="sxs-lookup"><span data-stu-id="0a064-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="0a064-128">Los eventos describen cambios observables en el sistema, por ejemplo, que se ha insertado un nuevo elemento de datos, que se ha completado un proceso o algo más.</span><span class="sxs-lookup"><span data-stu-id="0a064-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="0a064-129">El receptor WebHook se suscribe mediante el registro de un WebHook que consta de cuatro cosas:</span><span class="sxs-lookup"><span data-stu-id="0a064-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="0a064-130">Uri para donde se debe publicar la notificación de evento en forma de una solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="0a064-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="0a064-131">Un conjunto de filtros que describen los eventos concretos para los que se debe desencadenar WebHook;</span><span class="sxs-lookup"><span data-stu-id="0a064-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="0a064-132">Una clave secreta que se utiliza para firmar la solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="0a064-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="0a064-133">Datos adicionales que se incluirán en la solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0a064-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="0a064-134">Esto puede ser, por ejemplo, campos de encabezado HTTP adicionales o propiedades incluidas en el cuerpo de la solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0a064-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="0a064-135">Una vez que se produce un evento, se encuentran los registros de WebHook coincidentes y se envían solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0a064-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="0a064-136">Normalmente, la generación de las solicitudes HTTP POST se vuelve a intentar varias veces si por alguna razón el destinatario no responde o la solicitud HTTP POST da como resultado una respuesta de error.</span><span class="sxs-lookup"><span data-stu-id="0a064-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="0a064-137">Canalización de procesamiento de WebHooks</span><span class="sxs-lookup"><span data-stu-id="0a064-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="0a064-138">La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="0a064-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET canalización de procesamiento de WebHooks](_static/WebHookReceivers.png)

<span data-ttu-id="0a064-140">Los dos conceptos clave aquí son *Receptores* y *Manejadores:*</span><span class="sxs-lookup"><span data-stu-id="0a064-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="0a064-141">*Los receptores* son responsables de controlar el sabor particular de WebHook de un remitente determinado y de hacer cumplir las comprobaciones de seguridad para asegurarse de que la solicitud de WebHook es efectivamente del remitente previsto.</span><span class="sxs-lookup"><span data-stu-id="0a064-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="0a064-142">*Los controladores* suelen ser donde el código de usuario se ejecuta procesando el WebHook determinado.</span><span class="sxs-lookup"><span data-stu-id="0a064-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="0a064-143">En los nodos siguientes, estos conceptos se describen con más detalle.</span><span class="sxs-lookup"><span data-stu-id="0a064-143">In the following nodes these concepts are described in more details.</span></span>
