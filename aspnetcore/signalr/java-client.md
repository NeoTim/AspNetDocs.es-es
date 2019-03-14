---
title: Cliente ASP.NET Core SignalR Java
author: mikaelm12
description: Obtenga información sobre cómo usar al cliente de Java de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/java-client
ms.openlocfilehash: d0eff38c1f622b896ed1dc3002238aec7b6bfd38
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030072"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="702e5-103">Cliente ASP.NET Core SignalR Java</span><span class="sxs-lookup"><span data-stu-id="702e5-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="702e5-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="702e5-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="702e5-105">El cliente de Java que permite conectarse a un servidor de ASP.NET Core SignalR desde el código de Java, incluidas las aplicaciones Android.</span><span class="sxs-lookup"><span data-stu-id="702e5-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="702e5-106">Al igual que el [cliente JavaScript](xref:signalr/javascript-client) y el [cliente .NET](xref:signalr/dotnet-client), el cliente de Java que le permite recibir y enviar mensajes a un concentrador en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="702e5-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="702e5-107">El cliente de Java está disponible en ASP.NET Core 2.2 y posteriores.</span><span class="sxs-lookup"><span data-stu-id="702e5-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="702e5-108">En este artículo hace referencia a la aplicación de consola de Java de ejemplo usa al cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="702e5-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="702e5-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="702e5-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="702e5-110">Instale el paquete de cliente de SignalR Java</span><span class="sxs-lookup"><span data-stu-id="702e5-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="702e5-111">El *signalr 1.0.0* archivo JAR permite a los clientes para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="702e5-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="702e5-112">Para buscar el número de versión de archivo JAR más reciente, consulte el [los resultados de búsqueda de Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="702e5-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="702e5-113">Si usa Gradle, agregue la siguiente línea a la `dependencies` sección de su *build.gradle* archivo:</span><span class="sxs-lookup"><span data-stu-id="702e5-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="702e5-114">Si con Maven, agregue las siguientes líneas dentro de la `<dependencies>` elemento de su *pom.xml* archivo:</span><span class="sxs-lookup"><span data-stu-id="702e5-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="702e5-115">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="702e5-115">Connect to a hub</span></span>

<span data-ttu-id="702e5-116">Para establecer un `HubConnection`, el `HubConnectionBuilder` se debe usar.</span><span class="sxs-lookup"><span data-stu-id="702e5-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="702e5-117">El nivel de registro y la dirección URL del centro se puede configurar durante la compilación de una conexión.</span><span class="sxs-lookup"><span data-stu-id="702e5-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="702e5-118">Configurar las opciones necesarias mediante una llamada a cualquiera de los `HubConnectionBuilder` métodos antes de `build`.</span><span class="sxs-lookup"><span data-stu-id="702e5-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="702e5-119">Iniciar la conexión con `start`.</span><span class="sxs-lookup"><span data-stu-id="702e5-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="702e5-120">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="702e5-120">Call hub methods from client</span></span>

<span data-ttu-id="702e5-121">Una llamada a `send` invoca un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="702e5-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="702e5-122">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `send`.</span><span class="sxs-lookup"><span data-stu-id="702e5-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="702e5-123">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="702e5-123">Call client methods from hub</span></span>

<span data-ttu-id="702e5-124">Use `hubConnection.on` para definir métodos en el cliente que se puede llamar la central.</span><span class="sxs-lookup"><span data-stu-id="702e5-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="702e5-125">Definir los métodos después de compilar, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="702e5-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="702e5-126">Agregue un registro</span><span class="sxs-lookup"><span data-stu-id="702e5-126">Add logging</span></span>

<span data-ttu-id="702e5-127">El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro.</span><span class="sxs-lookup"><span data-stu-id="702e5-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="702e5-128">Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico.</span><span class="sxs-lookup"><span data-stu-id="702e5-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="702e5-129">El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="702e5-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="702e5-130">Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="702e5-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="702e5-131">Esto puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="702e5-131">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="702e5-132">Notas de desarrollo de Android</span><span class="sxs-lookup"><span data-stu-id="702e5-132">Android development notes</span></span>

<span data-ttu-id="702e5-133">Con respecto a la compatibilidad con el SDK de Android para las características de cliente de SignalR, tenga en cuenta los siguientes elementos cuando se especifica la versión del SDK de Android de destino:</span><span class="sxs-lookup"><span data-stu-id="702e5-133">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="702e5-134">El cliente de Java de SignalR se ejecutará en el nivel de API Android 16 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="702e5-134">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="702e5-135">Conexión a través de Azure SignalR Service requerirá el nivel de API de Android 20 y versiones posterior porque la [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requiere TLS 1.2 y no es compatible con conjuntos de cifrado basado en SHA-1.</span><span class="sxs-lookup"><span data-stu-id="702e5-135">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="702e5-136">Android [agrega compatibilidad con SHA-256 (y versiones posteriores) conjuntos de cifrado](https://developer.android.com/reference/javax/net/ssl/SSLSocket) en el nivel de API 20.</span><span class="sxs-lookup"><span data-stu-id="702e5-136">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="702e5-137">Configurar la autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="702e5-137">Configure bearer token authentication</span></span>

<span data-ttu-id="702e5-138">En el cliente de SignalR Java, puede configurar un token de portador a usar para la autenticación mediante un "generador de tokens de acceso" para el [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="702e5-138">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="702e5-139">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar una [RxJava](https://github.com/ReactiveX/RxJava) [único<String>](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="702e5-139">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="702e5-140">Con una llamada a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir la lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="702e5-140">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="702e5-141">Limitaciones conocidas</span><span class="sxs-lookup"><span data-stu-id="702e5-141">Known limitations</span></span>

* <span data-ttu-id="702e5-142">Se admite solo el protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="702e5-142">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="702e5-143">Se admite sólo el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="702e5-143">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="702e5-144">Transmisión por secuencias no se admite todavía.</span><span class="sxs-lookup"><span data-stu-id="702e5-144">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="702e5-145">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="702e5-145">Additional resources</span></span>

* [<span data-ttu-id="702e5-146">Referencia de API de Java</span><span class="sxs-lookup"><span data-stu-id="702e5-146">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
