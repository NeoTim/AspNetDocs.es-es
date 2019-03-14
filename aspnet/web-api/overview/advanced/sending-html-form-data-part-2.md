---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviar datos de formulario HTML en ASP.NET Web API: Carga de archivos y MIME de varias partes | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028982"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="11162-102">Enviar datos de formulario HTML en ASP.NET Web API: Carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="11162-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="11162-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11162-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="11162-104">Parte 2: Carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="11162-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="11162-105">Este tutorial muestra cómo cargar archivos a una API web.</span><span class="sxs-lookup"><span data-stu-id="11162-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="11162-106">También se describe cómo procesar los datos de varias partes MIME.</span><span class="sxs-lookup"><span data-stu-id="11162-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="11162-107">[Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="11162-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="11162-108">Este es un ejemplo de un formulario HTML para cargar un archivo:</span><span class="sxs-lookup"><span data-stu-id="11162-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="11162-109">Este formulario contiene un control de entrada de texto y un control de entrada de archivo.</span><span class="sxs-lookup"><span data-stu-id="11162-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="11162-110">Cuando un formulario contiene un control de entrada de archivo, el **enctype** atributo debe ser siempre &quot;multipart/datos de formulario&quot;, que especifica que el formulario se enviará como un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="11162-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="11162-111">El formato de un mensaje MIME de varias partes es más fácil comprender echando un vistazo a una solicitud de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="11162-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="11162-112">Este mensaje se divide en dos *partes*, uno para cada control de formulario.</span><span class="sxs-lookup"><span data-stu-id="11162-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="11162-113">Los límites de parte se indican mediante las líneas que comienzan con guiones.</span><span class="sxs-lookup"><span data-stu-id="11162-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="11162-114">El límite de la parte incluye un componente aleatorio (&quot;41184676334&quot;) para asegurarse de que la cadena delimitadora no accidentalmente aparecen dentro de una parte de mensaje.</span><span class="sxs-lookup"><span data-stu-id="11162-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="11162-115">Cada parte del mensaje contiene uno o varios encabezados, seguidos por el contenido del elemento.</span><span class="sxs-lookup"><span data-stu-id="11162-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="11162-116">El encabezado Content-Disposition incluye el nombre del control.</span><span class="sxs-lookup"><span data-stu-id="11162-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="11162-117">Para los archivos, también contiene el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="11162-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="11162-118">El encabezado Content-Type describe los datos en la parte.</span><span class="sxs-lookup"><span data-stu-id="11162-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="11162-119">Si se omite este encabezado, el valor predeterminado es texto/sin formato.</span><span class="sxs-lookup"><span data-stu-id="11162-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="11162-120">En el ejemplo anterior, el usuario ha cargado un archivo llamado GrandCanyon.jpg, con el tipo de contenido de imagen/jpeg; y el valor de la entrada de texto era &quot;vacaciones de verano&quot;.</span><span class="sxs-lookup"><span data-stu-id="11162-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="11162-121">Carga de archivos</span><span class="sxs-lookup"><span data-stu-id="11162-121">File Upload</span></span>

<span data-ttu-id="11162-122">Ahora Echemos un vistazo a un controlador de Web API que lee los archivos de un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="11162-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="11162-123">El controlador lee los archivos de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="11162-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="11162-124">Web API es compatible con las acciones asincrónicas mediante el [modelo de programación basado en tareas](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="11162-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="11162-125">En primer lugar, este es el código si tiene como destino .NET Framework 4.5, que es compatible con la **async** y **await** palabras clave.</span><span class="sxs-lookup"><span data-stu-id="11162-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="11162-126">Tenga en cuenta que la acción de controlador no toma ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="11162-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="11162-127">Esto es debido a que se procesa dentro de la acción, el cuerpo de solicitud sin invocar a un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="11162-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="11162-128">El **IsMultipartContent** método comprueba si la solicitud contiene un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="11162-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="11162-129">Si no es así, el controlador devuelve el código de estado HTTP 415 (tipo de medio no compatible).</span><span class="sxs-lookup"><span data-stu-id="11162-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="11162-130">El **MultipartFormDataStreamProvider** clase es un objeto auxiliar que asigna las secuencias de archivo para archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="11162-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="11162-131">Para leer el mensaje MIME de varias partes, llame a la **ReadAsMultipartAsync** método.</span><span class="sxs-lookup"><span data-stu-id="11162-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="11162-132">Este método extrae todas las partes del mensaje y los escribe en las secuencias proporcionadas por el **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="11162-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="11162-133">Cuando se completa el método, puede obtener información acerca de los archivos desde el **FileData** propiedad, que es una colección de **MultipartFileData** objetos.</span><span class="sxs-lookup"><span data-stu-id="11162-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="11162-134">**MultipartFileData.FileName** es el nombre de archivo local en el servidor, donde se guardó el archivo.</span><span class="sxs-lookup"><span data-stu-id="11162-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="11162-135">**MultipartFileData.Headers** contiene el encabezado de elemento (*no* el encabezado de solicitud).</span><span class="sxs-lookup"><span data-stu-id="11162-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="11162-136">Se puede usar para tener acceso al contenido\_encabezados Disposition y Content-Type.</span><span class="sxs-lookup"><span data-stu-id="11162-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="11162-137">Como sugiere su nombre, **ReadAsMultipartAsync** es un método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="11162-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="11162-138">Para realizar el trabajo después de completarse el método, use un [tarea de continuación](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o el **await** palabra clave (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="11162-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="11162-139">Esta es la versión de .NET Framework 4.0 del código anterior:</span><span class="sxs-lookup"><span data-stu-id="11162-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="11162-140">Leer datos de Control de formulario</span><span class="sxs-lookup"><span data-stu-id="11162-140">Reading Form Control Data</span></span>

<span data-ttu-id="11162-141">El formulario HTML que mostré anteriormente tenía un control de entrada de texto.</span><span class="sxs-lookup"><span data-stu-id="11162-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="11162-142">Puede obtener el valor del control desde el **FormData** propiedad de la **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="11162-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="11162-143">**FormData** es un **NameValueCollection** que contiene pares nombre/valor para los controles del formulario.</span><span class="sxs-lookup"><span data-stu-id="11162-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="11162-144">La colección puede contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="11162-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="11162-145">Considere este formulario:</span><span class="sxs-lookup"><span data-stu-id="11162-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="11162-146">El cuerpo de solicitud podría parecerse a esto:</span><span class="sxs-lookup"><span data-stu-id="11162-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="11162-147">En ese caso, el **FormData** colección contendría los siguientes pares de clave/valor:</span><span class="sxs-lookup"><span data-stu-id="11162-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="11162-148">ida y vuelta: ida y vuelta</span><span class="sxs-lookup"><span data-stu-id="11162-148">trip: round-trip</span></span>
- <span data-ttu-id="11162-149">opciones: la protección sin interrupciones</span><span class="sxs-lookup"><span data-stu-id="11162-149">options: nonstop</span></span>
- <span data-ttu-id="11162-150">opciones: las fechas</span><span class="sxs-lookup"><span data-stu-id="11162-150">options: dates</span></span>
- <span data-ttu-id="11162-151">asiento: ventana</span><span class="sxs-lookup"><span data-stu-id="11162-151">seat: window</span></span>
