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
# <a name="aspnet-webhooks-overview"></a>Visión general de ASP.NET WebHooks

WebHooks es un patrón HTTP ligero que proporciona un modelo de pub/sub simple para conectar las API web y los servicios SaaS. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento que permite que el receptor actúe en consecuencia.

Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios, incluyendo [Dropbox,](http://dropbox.com/) [GitHub,](https://www.github.com/) [Bitbucket,](https://bitbucket.org/) [MailChimp,](http://www.mailchimp.com/) [PayPal,](http://www.paypal.com/) [Slack,](http://www.slack.com) [Stripe,](http://www.stripe.com) [Trello](http://www.trello.com/)y muchos más. Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), o se ha confirmado un cambio de código en GitHub, o se ha iniciado un pago en [PayPal](http://www.paypal.com/)o se ha creado una tarjeta en [Trello](http://www.trello.com/). ¡Las posibilidades son infinitas!

Microsoft ASP.NET WebHooks facilita el envío y la recepción de WebHooks como parte de la aplicación ASP.NET:

* En el lado de recepción, proporciona un modelo común para recibir y procesar WebHooks desde cualquier número de proveedores de WebHook. Viene de la caja con soporte para [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) y [Zendesk,](https://www.zendesk.com/) pero es fácil agregar soporte para más.

* En el lado de envío proporciona soporte para administrar y almacenar suscripciones, así como para enviar notificaciones de eventos al conjunto correcto de suscriptores. Esto le permite definir su propio conjunto de eventos a los que los suscriptores pueden suscribirse y notificarles cuando suceden las cosas.

Las dos partes se pueden utilizar juntas o separadas dependiendo de su escenario. Si solo necesita recibir WebHooks de otros servicios, puede usar solo la parte del receptor; si solo desea exponer WebHooks para que otros lo consuman, entonces puede hacer eso.

El código tiene como destino ASP.NET Web API 2 y ASP.NET MVC 5 y está disponible como [OSS en GitHub.](https://github.com/aspnet/WebHooks)

## <a name="webhooks-overview"></a>Descripción general de WebHooks

WebHooks es un patrón que significa que varía cómo se utiliza de servicio a servicio, pero la idea básica es la misma. Puede pensar en WebHooks como un modelo simple de pub/sub donde un usuario puede suscribirse a eventos que ocurren en otro lugar. Las notificaciones de eventos se propagan como solicitudes HTTP POST que contienen información sobre el propio evento.

Normalmente, la solicitud HTTP POST contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook, incluida información sobre el evento que hace que WebHook se desencadene. Por ejemplo, un cuerpo de solicitud POST de WebHook de [GitHub](https://www.github.com/) tiene este aspecto como resultado de un nuevo problema que se abre en un repositorio determinado:

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

Para asegurarse de que el WebHook es de hecho del remitente previsto, la solicitud POST se protege de alguna manera y, a continuación, verifica da la seguridad del receptor. Por ejemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) incluye un encabezado HTTP *X-Hub-Signature* con un hash del cuerpo de la solicitud que comprueba la implementación del receptor para que no tenga que preocuparse por él.

El flujo de WebHook generalmente va algo como esto:

* El remitente de WebHook expone eventos a los que un cliente puede suscribirse. Los eventos describen cambios observables en el sistema, por ejemplo, que se ha insertado un nuevo elemento de datos, que se ha completado un proceso o algo más.

* El receptor WebHook se suscribe mediante el registro de un WebHook que consta de cuatro cosas:

     1. Uri para donde se debe publicar la notificación de evento en forma de una solicitud HTTP POST;

     2. Un conjunto de filtros que describen los eventos concretos para los que se debe desencadenar WebHook;

     3. Una clave secreta que se utiliza para firmar la solicitud HTTP POST;

     4. Datos adicionales que se incluirán en la solicitud HTTP POST. Esto puede ser, por ejemplo, campos de encabezado HTTP adicionales o propiedades incluidas en el cuerpo de la solicitud HTTP POST.

* Una vez que se produce un evento, se encuentran los registros de WebHook coincidentes y se envían solicitudes HTTP POST. Normalmente, la generación de las solicitudes HTTP POST se vuelve a intentar varias veces si por alguna razón el destinatario no responde o la solicitud HTTP POST da como resultado una respuesta de error.

## <a name="webhooks-processing-pipeline"></a>Canalización de procesamiento de WebHooks

La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene este aspecto:

![ASP.NET canalización de procesamiento de WebHooks](_static/WebHookReceivers.png)

Los dos conceptos clave aquí son *Receptores* y *Manejadores:*

* *Los receptores* son responsables de controlar el sabor particular de WebHook de un remitente determinado y de hacer cumplir las comprobaciones de seguridad para asegurarse de que la solicitud de WebHook es efectivamente del remitente previsto.

* *Los controladores* suelen ser donde el código de usuario se ejecuta procesando el WebHook determinado.

En los nodos siguientes, estos conceptos se describen con más detalle.
