---
title: Introducción a Razor Components
author: guardrex
description: 'Explore Razor Components de ASP.NET Core, una forma de compilar la interfaz de usuario web de cliente interactiva con .NET en una aplicación ASP.NET Core.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="fe294-103">Introducción a Razor Components</span><span class="sxs-lookup"><span data-stu-id="fe294-103">Introduction to Razor Components</span></span>

<span data-ttu-id="fe294-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fe294-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fe294-105">*Razor Components* es una manera de compilar la interfaz de usuario web de cliente interactiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="fe294-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="fe294-106">Compile interfaces de usuario completamente interactivas con C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe294-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="fe294-107">Comparta la lógica de aplicación del lado cliente y servidor escrita toda con. NET.</span><span class="sxs-lookup"><span data-stu-id="fe294-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="fe294-108">Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.</span><span class="sxs-lookup"><span data-stu-id="fe294-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="fe294-109">Razor Components admite elementos básicos requeridos por la mayoría de las aplicaciones, como:</span><span class="sxs-lookup"><span data-stu-id="fe294-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="fe294-110">Parámetros</span><span class="sxs-lookup"><span data-stu-id="fe294-110">Parameters</span></span>
* <span data-ttu-id="fe294-111">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="fe294-111">Event handling</span></span>
* <span data-ttu-id="fe294-112">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="fe294-112">Data binding</span></span>
* <span data-ttu-id="fe294-113">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="fe294-113">Routing</span></span>
* <span data-ttu-id="fe294-114">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="fe294-114">Dependency injection</span></span>
* <span data-ttu-id="fe294-115">Diseños</span><span class="sxs-lookup"><span data-stu-id="fe294-115">Layouts</span></span>
* <span data-ttu-id="fe294-116">Plantillas</span><span class="sxs-lookup"><span data-stu-id="fe294-116">Templating</span></span>
* <span data-ttu-id="fe294-117">Valores en cascada</span><span class="sxs-lookup"><span data-stu-id="fe294-117">Cascading values</span></span>

<span data-ttu-id="fe294-118">Razor Components separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="fe294-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="fe294-119">Razor Components de ASP.NET Core en .NET Core 3.0 agrega compatibilidad con el hospedaje de Razor Components en el servidor en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe294-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="fe294-120">Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe294-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="fe294-121">Entorno de ejecución:</span><span class="sxs-lookup"><span data-stu-id="fe294-121">The runtime:</span></span>

* <span data-ttu-id="fe294-122">Controla el envío de eventos de interfaz de usuario desde el explorador al servidor.</span><span class="sxs-lookup"><span data-stu-id="fe294-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="fe294-123">Aplica las actualizaciones de la interfaz de usuario que el servidor devuelve al explorador después de ejecutar los componentes.</span><span class="sxs-lookup"><span data-stu-id="fe294-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="fe294-124">La conexión utilizada por Razor Components para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe294-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="fe294-126">Para obtener más información, consulta <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="fe294-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="fe294-127">*Blazor* es el modelo experimental de hospedaje del lado cliente de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="fe294-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="fe294-128">Blazor se ejecuta en .NET en el explorador mediante estándares web abiertos sin complementos ni transpilación de código.</span><span class="sxs-lookup"><span data-stu-id="fe294-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="fe294-129">Para obtener más información, consulta <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="fe294-129">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="fe294-130">Componentes</span><span class="sxs-lookup"><span data-stu-id="fe294-130">Components</span></span>

<span data-ttu-id="fe294-131">Un *componente Razor* es una parte de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos.</span><span class="sxs-lookup"><span data-stu-id="fe294-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="fe294-132">Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="fe294-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="fe294-133">Los componentes se pueden anidar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="fe294-133">Components can be nested and reused.</span></span>

<span data-ttu-id="fe294-134">Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="fe294-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="fe294-135">La clase se puede escribir como una página de marcado de Razor (*.cshtml*) o como una clase de C# (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="fe294-135">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="fe294-136">[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#.</span><span class="sxs-lookup"><span data-stu-id="fe294-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="fe294-137">Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="fe294-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="fe294-138">Las vistas de Razor Pages y MVC también usan Razor.</span><span class="sxs-lookup"><span data-stu-id="fe294-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="fe294-139">A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="fe294-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="fe294-140">Razor Components se puede usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="fe294-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="fe294-141">El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado en un archivo de Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="fe294-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="fe294-142">Cuando este componente se usa en otra parte de la aplicación, IntelliSense agiliza el desarrollo con la finalización de parámetros y sintaxis.</span><span class="sxs-lookup"><span data-stu-id="fe294-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="fe294-143">Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.</span><span class="sxs-lookup"><span data-stu-id="fe294-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="fe294-144">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fe294-144">JavaScript interop</span></span>

<span data-ttu-id="fe294-145">En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe294-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="fe294-146">Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe294-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="fe294-147">El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#.</span><span class="sxs-lookup"><span data-stu-id="fe294-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="fe294-148">Para obtener más información, vea [Interoperabilidad de JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="fe294-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe294-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fe294-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="fe294-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="fe294-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="fe294-151">Guía de C#</span><span class="sxs-lookup"><span data-stu-id="fe294-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="fe294-152">HTML</span><span class="sxs-lookup"><span data-stu-id="fe294-152">HTML</span></span>](https://www.w3.org/html/)
