---
title: Páginas de ayuda de ASP.NET Core Web API con Swagger/Open API
author: rsuter
description: En este tutorial se proporciona una guía sobre cómo incorporar Swagger para generar documentación y páginas de ayuda para una aplicación de API web.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d7a6ed158dcb464bb80c83773ed7d455b25ce44b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049982"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a><span data-ttu-id="3c342-103">Páginas de ayuda de ASP.NET Core Web API con Swagger/Open API</span><span class="sxs-lookup"><span data-stu-id="3c342-103">ASP.NET Core web API help pages with Swagger / OpenAPI</span></span>

<span data-ttu-id="3c342-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="3c342-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="3c342-105">Cuando un desarrollador usa una API Web, puede que le resulte complicado comprender sus diversos métodos.</span><span class="sxs-lookup"><span data-stu-id="3c342-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="3c342-106">[Swagger](https://swagger.io/), también conocido como [OpenAPI](https://www.openapis.org/), resuelve el problema de generar páginas útiles de ayuda y documentación relativas a las API web.</span><span class="sxs-lookup"><span data-stu-id="3c342-106">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="3c342-107">Así, reporta ventajas como una documentación interactiva, la generación de SDK de cliente y la detectabilidad de API.</span><span class="sxs-lookup"><span data-stu-id="3c342-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="3c342-108">En este artículo, nos centraremos en las implementaciones de Swagger .NET [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) y [NSwag](https://github.com/RSuter/NSwag):</span><span class="sxs-lookup"><span data-stu-id="3c342-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="3c342-109">**Swashbuckle.AspNetCore** es un proyecto de código abierto para generar documentos de Swagger para las API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c342-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="3c342-110">**NSwag** es otro proyecto de código abierto para generar documentos de Swagger y que sirve para integrar la [interfaz de usuario de Swagger](https://swagger.io/swagger-ui/) o [ReDoc](https://github.com/Rebilly/ReDoc) en las API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c342-110">**NSwag** is another open source project for generating Swagger documents and integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core web APIs.</span></span> <span data-ttu-id="3c342-111">De manera opcional, NSwag ofrece métodos para generar código de cliente de C# y TypeScript para la API.</span><span class="sxs-lookup"><span data-stu-id="3c342-111">Additionally, NSwag offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--openapi"></a><span data-ttu-id="3c342-112">¿Qué es Swagger/OpenAPI?</span><span class="sxs-lookup"><span data-stu-id="3c342-112">What is Swagger / OpenAPI?</span></span>

<span data-ttu-id="3c342-113">Swagger es una especificación independiente del lenguaje que sirve para describir API de [REST](https://en.wikipedia.org/wiki/Representational_state_transfer).</span><span class="sxs-lookup"><span data-stu-id="3c342-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="3c342-114">El proyecto de Swagger se donó a la [iniciativa OpenAPI](https://www.openapis.org/), donde ahora se conoce como OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="3c342-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as OpenAPI.</span></span> <span data-ttu-id="3c342-115">Ambos nombres se usan indistintamente, aunque se prefiere OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="3c342-115">Both names are used interchangeably; however, OpenAPI is preferred.</span></span> <span data-ttu-id="3c342-116">Gracias a esta API, tanto los equipos como los personas podrán conocer las funciones de un servicio sin necesidad de obtener acceso directo a la implementación (código fuente, acceso a la red, documentación).</span><span class="sxs-lookup"><span data-stu-id="3c342-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="3c342-117">Uno de los objetivos consiste en reducir al mínimo la cantidad de trabajo necesario para conectar servicios que no están asociados.</span><span class="sxs-lookup"><span data-stu-id="3c342-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="3c342-118">Otro es reducir la cantidad de tiempo necesario para documentar un servicio con precisión.</span><span class="sxs-lookup"><span data-stu-id="3c342-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="3c342-119">Especificación de Swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="3c342-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="3c342-120">El elemento principal del flujo de Swagger es la especificación de Swagger, que es de forma predeterminada un documento llamado *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="3c342-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="3c342-121">Lo genera la cadena de herramientas de Swagger (o las implementaciones de terceros de dicha cadena) en función de su servicio.</span><span class="sxs-lookup"><span data-stu-id="3c342-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="3c342-122">Describe las funciones de su API y cómo tener acceso a ella con HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c342-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="3c342-123">Esta especificación también controla la interfaz de usuario de Swagger, y la cadena de herramientas la usa para permitir la detección y generación de código de cliente.</span><span class="sxs-lookup"><span data-stu-id="3c342-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="3c342-124">Este es un ejemplo de especificación de Swagger (se ha reducido por motivos de brevedad):</span><span class="sxs-lookup"><span data-stu-id="3c342-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a><span data-ttu-id="3c342-125">Interfaz de usuario de Swagger</span><span class="sxs-lookup"><span data-stu-id="3c342-125">Swagger UI</span></span>

<span data-ttu-id="3c342-126">La [interfaz de usuario de Swagger](https://swagger.io/swagger-ui/) es una interfaz de usuario basada en Internet que proporciona información sobre el servicio por medio de la especificación de Swagger generada.</span><span class="sxs-lookup"><span data-stu-id="3c342-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="3c342-127">Swashbuckle y NSwag incluyen una versión insertada de la interfaz de usuario de Swagger, de modo que se puede hospedar en una aplicación ASP.NET Core realizando una llamada de registro de middleware.</span><span class="sxs-lookup"><span data-stu-id="3c342-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="3c342-128">La interfaz de usuario web tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="3c342-128">The web UI looks like this:</span></span>

![Interfaz de usuario de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="3c342-130">Todos los métodos de acción públicos aplicados a los controladores se pueden probar desde la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3c342-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="3c342-131">Haga clic en un nombre de método para expandir la sección.</span><span class="sxs-lookup"><span data-stu-id="3c342-131">Click a method name to expand the section.</span></span> <span data-ttu-id="3c342-132">Agregue todos los parámetros necesarios y haga clic en **Try it out!** (¡Pruébelo!).</span><span class="sxs-lookup"><span data-stu-id="3c342-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Ejemplo de prueba de acción GET de Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="3c342-134">La versión de interfaz de usuario de Swagger usada para las capturas de pantalla es la versión 2.</span><span class="sxs-lookup"><span data-stu-id="3c342-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="3c342-135">Para obtener un ejemplo de la versión 3, vea el [ejemplo de Petstore](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="3c342-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c342-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3c342-136">Next steps</span></span>

* <span data-ttu-id="3c342-137">[Get started with Swashbuckle](xref:tutorials/get-started-with-swashbuckle) (Introducción a Swashbuckle)</span><span class="sxs-lookup"><span data-stu-id="3c342-137">[Get started with Swashbuckle](xref:tutorials/get-started-with-swashbuckle)</span></span>
* <span data-ttu-id="3c342-138">[Get started with NSwag](xref:tutorials/get-started-with-nswag) (Introducción a NSwag)</span><span class="sxs-lookup"><span data-stu-id="3c342-138">[Get started with NSwag](xref:tutorials/get-started-with-nswag)</span></span>
