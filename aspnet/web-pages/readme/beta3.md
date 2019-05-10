---
uid: web-pages/readme/beta3
title: WebMatrix y ASP.NET Web Pages (Razor) archivo Léame de versión Beta 3 | Microsoft Docs
author: rick-anderson
description: Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124110"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="22d66-103">Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="22d66-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

> <span data-ttu-id="22d66-104">Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="22d66-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="22d66-105">9 de noviembre de 2010</span><span class="sxs-lookup"><span data-stu-id="22d66-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="22d66-106">Contenido</span><span class="sxs-lookup"><span data-stu-id="22d66-106">Contents</span></span>

- [<span data-ttu-id="22d66-107">Información general</span><span class="sxs-lookup"><span data-stu-id="22d66-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="22d66-108">Instalación</span><span class="sxs-lookup"><span data-stu-id="22d66-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="22d66-109">Nuevas características, cambios y problemas conocidos de la versión Beta 3</span><span class="sxs-lookup"><span data-stu-id="22d66-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="22d66-110">Problemas de instalación de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="22d66-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - <span data-ttu-id="22d66-111">[ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="22d66-111">[ASP.NET Web Pages](#Known_Issues_ASPNET)</span></span>
    - [<span data-ttu-id="22d66-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="22d66-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="22d66-113">Instalación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="22d66-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="22d66-114">Publicación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="22d66-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="22d66-115">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="22d66-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="22d66-116">Para obtener más información</span><span class="sxs-lookup"><span data-stu-id="22d66-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="22d66-117">Información general</span><span class="sxs-lookup"><span data-stu-id="22d66-117">Overview</span></span>

> <span data-ttu-id="22d66-118">Microsoft WebMatrix Beta es una pila de desarrollo web gratuito que se instala en pocos minutos.</span><span class="sxs-lookup"><span data-stu-id="22d66-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="22d66-119">Se integra un servidor web con la base de datos y marcos para crear una única experiencia integrada de programación.</span><span class="sxs-lookup"><span data-stu-id="22d66-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="22d66-120">Puede usar el WebMatrix Beta para agilizar la manera de codificar, probar y publicar su propio sitio Web ASP.NET o PHP o puede usar la versión WebMatrix Beta para iniciar un nuevo sitio Web con aplicaciones de código abierto populares como DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="22d66-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="22d66-121">Versión WebMatrix Beta usa el mismo servidor web eficaz, el motor de base de datos y el entorno de marcos que se ejecutará el sitio Web en internet, lo que hace la transición de desarrollo a producción homogénea y sin problemas.</span><span class="sxs-lookup"><span data-stu-id="22d66-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="22d66-122">Instalación</span><span class="sxs-lookup"><span data-stu-id="22d66-122">Installation</span></span>

> <span data-ttu-id="22d66-123">Para instalar la versión Beta 3 de WebMatrix, use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="22d66-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="22d66-124">Después de instalar al instalador de plataforma Web, puede usarlo para instalar la versión Beta 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="22d66-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="22d66-125">Si tiene problemas durante la instalación, consulte [solución de problemas con el instalador de plataforma Web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="22d66-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="22d66-126">Instrucciones para publicar aplicaciones</span><span class="sxs-lookup"><span data-stu-id="22d66-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="22d66-127">Consulte [instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="22d66-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="22d66-128">Problemas nuevas características, cambios, andKnown</span><span class="sxs-lookup"><span data-stu-id="22d66-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="22d66-129">Instalación de WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="22d66-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="22d66-130">Problema: Solo está disponible en plataformas compatibles con Microsoft .NET Framework 4 Beta 3 de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="22d66-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="22d66-131">La versión 4 de .NET Framework es necesaria para la versión WebMatrix Beta.</span><span class="sxs-lookup"><span data-stu-id="22d66-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="22d66-132">En algunos casos, el instalador WebMatrix Beta le permitirá intenta instalar en una plataforma que no forma parte del conjunto de configuración admitidos.</span><span class="sxs-lookup"><span data-stu-id="22d66-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="22d66-133">En concreto, Windows Vista sin la actualización SP1 le permiten comenzar la instalación de la versión WebMatrix Beta, pero producirá un error en el componente de .NET Framework 4 y bloquear la instalación.</span><span class="sxs-lookup"><span data-stu-id="22d66-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="22d66-134">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-134">**Workaround**</span></span>  
> <span data-ttu-id="22d66-135">Instalar en una plataforma compatible, que incluye:</span><span class="sxs-lookup"><span data-stu-id="22d66-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="22d66-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="22d66-136">Windows 7</span></span>
> - <span data-ttu-id="22d66-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="22d66-137">Windows Server 2008</span></span>
> - <span data-ttu-id="22d66-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="22d66-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="22d66-139">Windows Vista SP1 o posterior</span><span class="sxs-lookup"><span data-stu-id="22d66-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="22d66-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="22d66-140">Windows XP SP3</span></span>
> - <span data-ttu-id="22d66-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="22d66-141">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="22d66-142">Problema: No se puede instalar la versión Beta 3 de WebMatrix si se instala Microsoft Visual Studio 2008 sin Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="22d66-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="22d66-143">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-143">**Workaround**</span></span>  
> <span data-ttu-id="22d66-144">Instalar [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) desde el centro de descarga de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="22d66-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="22d66-145">Problema: Algunos ensamblados de SQL Server Compact 4.0 no están instalados en la GAC</span><span class="sxs-lookup"><span data-stu-id="22d66-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="22d66-146">Los ensamblados administrados para SQL Server Compact 4.0 no se colocan en la caché de ensamblados global (GAC) al instalar SQL Server Compact 4.0 en un equipo de 64 bits y el equipo tiene solo .NET Framework 3.5 SP1 Client Profile instalado.</span><span class="sxs-lookup"><span data-stu-id="22d66-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="22d66-147">Son los ensamblados administrados que no están instalados en la GAC:</span><span class="sxs-lookup"><span data-stu-id="22d66-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="22d66-148">*System.Data.SqlServerCe.dll* (proveedor de ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="22d66-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="22d66-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="22d66-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="22d66-150">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-150">**Workaround**</span></span>  
> <span data-ttu-id="22d66-151">Desinstalar SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="22d66-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="22d66-152">Descargue e instale la versión completa de .NET Framework 3.5 SP1 desde la ubicación siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="22d66-153">Paquete de Microsoft .NET Framework 3.5 con Service 1 (paquete completo)</span><span class="sxs-lookup"><span data-stu-id="22d66-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="22d66-154">A continuación, vuelva a instalar SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="22d66-154">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="22d66-155">Problema: No se puede desinstalar SQL Server Compact usando la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="22d66-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="22d66-156">Desinstalación de SQL Server Compact usando las opciones de línea de comandos no funciona en esta versión.</span><span class="sxs-lookup"><span data-stu-id="22d66-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="22d66-157">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-157">**Workaround**</span></span>  
> <span data-ttu-id="22d66-158">Use *programas y características* en el Panel de Control de Windows para desinstalar Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="22d66-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="22d66-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="22d66-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="22d66-160">Esta sección del documento describen las nuevas características, cambios y problemas conocidos con la versión Beta 3 de ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="22d66-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="22d66-161">Características nuevas</span><span class="sxs-lookup"><span data-stu-id="22d66-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="22d66-162">Cambios</span><span class="sxs-lookup"><span data-stu-id="22d66-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="22d66-163">Problemas</span><span class="sxs-lookup"><span data-stu-id="22d66-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="22d66-164">Nuevas características en versión Beta 3 para ASP.NET Web Pages con sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="22d66-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="22d66-165">New: Método de "Html.Raw" representa el marcado sin codificar</span><span class="sxs-lookup"><span data-stu-id="22d66-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="22d66-166">El nuevo `Html.Raw` método le permite presentar HTML como marcado en lugar de representar la salida codificada.</span><span class="sxs-lookup"><span data-stu-id="22d66-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="22d66-167">(De forma predeterminada, ASP.NET Razor codifica las cadenas antes de presentarlo a ellas.) La sintaxis es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="22d66-168">En el siguiente ejemplo se muestra cómo usar `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="22d66-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="22d66-169">Cambios en la versión Beta 3 para ASP.NET Web Pages con sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="22d66-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="22d66-170">Cambio: Método de "atributo href" quitado</span><span class="sxs-lookup"><span data-stu-id="22d66-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="22d66-171">El `HrefAttribute` método de la `WebPage` ha quitado la clase.</span><span class="sxs-lookup"><span data-stu-id="22d66-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="22d66-172">Esta aplicación auxiliar se usa para codificar caracteres no seguros en las direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="22d66-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="22d66-173">Ya no es necesario porque ASP.NET Razor automáticamente codifica las cadenas.</span><span class="sxs-lookup"><span data-stu-id="22d66-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="22d66-174">(Use la nueva `Html.Raw` método para representar cadenas sin codificar.)</span><span class="sxs-lookup"><span data-stu-id="22d66-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="22d66-175">Cambio: Sintaxis declarativa "@helper" aplicaciones auxiliares cambiadas</span><span class="sxs-lookup"><span data-stu-id="22d66-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="22d66-176">En la versión Beta 3, ASP.NET cambia cómo analiza las aplicaciones auxiliares que se crean mediante el `@helper` sintaxis.</span><span class="sxs-lookup"><span data-stu-id="22d66-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="22d66-177">En esencia, el `@helper` sintaxis ahora se analiza como un bloque de código en lugar de como un bloque de marcado que puede incluir código.</span><span class="sxs-lookup"><span data-stu-id="22d66-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="22d66-178">Por lo tanto, no es necesario que incluirse en el código dentro de la aplicación auxiliar `@{ }` bloques.</span><span class="sxs-lookup"><span data-stu-id="22d66-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="22d66-179">Por el contrario, marcado dentro de la aplicación auxiliar debe incluirse explícitamente en los elementos HTML o ASP.NET Razor `<text></text>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="22d66-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="22d66-180">Por ejemplo, la siguiente `@helper` sintaxis funciona en la versión Beta 3:</span><span class="sxs-lookup"><span data-stu-id="22d66-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="22d66-181">En la versión Beta 3, debe cambiar esta aplicación auxiliar para ser similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="22d66-182">Tenga en cuenta que el `@{ }` ya no se utiliza caracteres en torno al código inicial en la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="22d66-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="22d66-183">Esto es porque el contenido de las aplicaciones auxiliares se trata como un bloque de código de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="22d66-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="22d66-184">La aplicación auxiliar representa el marcado, que comienza con la apertura `<a>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="22d66-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="22d66-185">Si debe representar la aplicación auxiliar de etiquetas que no incluyen una etiqueta de cierre o texto sin formato (por ejemplo, `<meta>` etiquetas), debe ser el contenido que se representará en `<text></text>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="22d66-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>

#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="22d66-186">Cambio: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="22d66-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="22d66-187">El `WebPageContext.HttpContext` ha eliminado la propiedad.</span><span class="sxs-lookup"><span data-stu-id="22d66-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="22d66-188">Utilice `HttpContext.Current` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="22d66-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="22d66-189">(El `WebPageContext.HttpContext` propiedad contenida esto.)</span><span class="sxs-lookup"><span data-stu-id="22d66-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>

#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="22d66-190">Cambio: Aplicación auxiliar de "Facebook" movido al nuevo paquete</span><span class="sxs-lookup"><span data-stu-id="22d66-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="22d66-191">El `Facebook` auxiliar se ha movido a la *Facebook.Helper* library, que incluye el `Facebook` auxiliar y funcionalidad adicional.</span><span class="sxs-lookup"><span data-stu-id="22d66-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="22d66-192">Debe instalar esta biblioteca como un paquete independiente, como se describe en "Instalar las aplicaciones auxiliares con Administrador de paquetes" en el tutorial [Introducción a las páginas ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="22d66-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="22d66-193">Cambio: Pertenencia a roles y seguridad de tipos se mueve al nuevo ensamblado</span><span class="sxs-lookup"><span data-stu-id="22d66-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="22d66-194">Los tipos siguientes se han movido a la `WebMatrix.WebData` ensamblado:</span><span class="sxs-lookup"><span data-stu-id="22d66-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="22d66-195">Cambio: Clase "TagBuilder" se movió al ensamblado de System.Web.WebPages.dll</span><span class="sxs-lookup"><span data-stu-id="22d66-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="22d66-196">La `TagBuilde` clase r se ha movido al ensamblado System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="22d66-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="22d66-197">Anteriormente, esto era en un ensamblado que formaba parte de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22d66-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="22d66-198">Este cambio significa que no es necesario instalar ASP.NET MVC para usar el `TagBuilder` clase.</span><span class="sxs-lookup"><span data-stu-id="22d66-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="22d66-199">Sin embargo, la clase aún está en el `System.Web.Mvc` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="22d66-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="22d66-200">Para poder usar el `TagBuilder` clase (por ejemplo, una auxiliar Razor de ASP.NET personalizada), debe hacer referencia el espacio de nombres (por ejemplo, si agrega `@using System.Web.Mvc` a su código).</span><span class="sxs-lookup"><span data-stu-id="22d66-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="22d66-201">Cambio: Sintaxis de validación ha cambiado; de la solicitud Clase de "Validación" quitado</span><span class="sxs-lookup"><span data-stu-id="22d66-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="22d66-202">En la versión Beta 3, para deshabilitar la validación de un campo individual o un conjunto de campos, se puede llamar a la `Validation.Exclude` método, pasando el nombre o nombres de los campos para excluir de la validación.</span><span class="sxs-lookup"><span data-stu-id="22d66-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="22d66-203">Una nueva sintaxis está disponible en la versión Beta 3 para omitir la validación.</span><span class="sxs-lookup"><span data-stu-id="22d66-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="22d66-204">El `Validation` se ha quitado el método utilizado en la versión Beta 3.</span><span class="sxs-lookup"><span data-stu-id="22d66-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="22d66-205">Si deshabilita la validación de la solicitud, si los usuarios intentan cargar marcado HTML (por ejemplo, mediante un editor de texto enriquecido en una página), el sitio Web notificará un error como *ha detectado un valor de Request.Form potencialmente peligroso del cliente*y no se acepta la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="22d66-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="22d66-206">Si deshabilita la validación de solicitudes, debe comprobar manualmente la entrada del usuario para asegurarse de que no contienen marcado potencialmente peligroso o script con algo parecido a la [V4.0 de Microsoft Anti-Cross Site Scripting Library](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="22d66-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="22d66-207">Para deshabilitar la validación automática de solicitud, llame a la `Request.Unvalidated` método y pásele el nombre del campo o de otro objeto de entrada que desea omitir la validación de solicitud de.</span><span class="sxs-lookup"><span data-stu-id="22d66-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="22d66-208">Puede usar este método para omitir la validación de todos los elementos en el `Form`, `QueryString`, `Cookies`, y `ServerVariables` colecciones.</span><span class="sxs-lookup"><span data-stu-id="22d66-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="22d66-209">Los ejemplos siguientes muestran cómo usar el `Unvalidated` método:</span><span class="sxs-lookup"><span data-stu-id="22d66-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="22d66-210">Problemas conocidos de ASP.NET Web Pages con sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="22d66-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="22d66-211">Problema: Comportamiento inesperado cuando se usa una tabla de usuario personalizada para la suscripción</span><span class="sxs-lookup"><span data-stu-id="22d66-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="22d66-212">Para inicializar el proveedor de pertenencia para un sitio Web de ASP.NET Razor, llame a la `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="22d66-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="22d66-213">(En WebMatrix, la plantilla de sitio inicial incluye una llamada a este método en el  *\_AppStart.cshtml* archivo.) Si el `autoCreateTables` parámetro de este método está establecido en true (de forma predeterminada, se establece en true en la plantilla de sitio de inicio), y si un nombre de tabla no reconocida se pasa al método (el segundo parámetro), el método no produce un error.</span><span class="sxs-lookup"><span data-stu-id="22d66-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="22d66-214">En su lugar, crea automáticamente la tabla.</span><span class="sxs-lookup"><span data-stu-id="22d66-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="22d66-215">Esto puede ser un problema si va a usar una tabla de usuario personalizada para la suscripción pero pase el nombre de tabla incorrecto para el `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="22d66-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="22d66-216">Dado que el método no predeterminada genera un error si no existe la tabla que se especifique, y porque crea una nueva tabla en su lugar, la aplicación puede parecer a trabajar.</span><span class="sxs-lookup"><span data-stu-id="22d66-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="22d66-217">Sin embargo, código de aplicación que se basa en la tabla de usuario personalizada (y en los campos de él) puede producir un error con errores inesperados.</span><span class="sxs-lookup"><span data-stu-id="22d66-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="22d66-218">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-218">**Workaround**</span></span>  
> <span data-ttu-id="22d66-219">Asegúrese de que pasa el nombre de la `InitializeDatabaseConnection` coincidencias de método, el perfil de usuario de la tabla en la base de datos de pertenencia o asegúrese de que el `autoCreateTables` parámetro se establece en false.</span><span class="sxs-lookup"><span data-stu-id="22d66-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="22d66-220">Problema: Error "No se pudo generar una instancia de usuario de SQL Server"</span><span class="sxs-lookup"><span data-stu-id="22d66-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="22d66-221">Si una aplicación WebMatrix Web usa SQL Server Express y se está ejecutando IIS 7.5 en Windows 7 o Windows Server 2008 R2, puede aparecer un error que indica que SQL Server no se puede recuperar la ruta de acceso de aplicación local del usuario en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="22d66-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="22d66-222">**Solución alternativa** Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente, el servicio de red) tiene los permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas como *aplicación\_datos*.</span><span class="sxs-lookup"><span data-stu-id="22d66-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="22d66-223">Información más detallada está disponible en el artículo de Knowledge Base [problemas con las instancias de usuario de SQL Server Express y proyectos de aplicación Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="22d66-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="22d66-224">Problema: En Visual Studio, los espacios de nombres para los ensamblados personalizados (DLL) no se importan automáticamente</span><span class="sxs-lookup"><span data-stu-id="22d66-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="22d66-225">Si utiliza los ensamblados personalizados en un proyecto en Visual Studio, no se importan automáticamente los espacios de nombres declarados en esos ensamblados en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="22d66-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="22d66-226">Como resultado, las referencias a tipos personalizados podrían no reconocer en tiempo de diseño y se marcan como no reconocido en Visual Studio (con un "subrayado ondulado").</span><span class="sxs-lookup"><span data-stu-id="22d66-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="22d66-227">Este problema se produce solo en tiempo de diseño en Visual Studio; la propia aplicación se ejecute correctamente.</span><span class="sxs-lookup"><span data-stu-id="22d66-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="22d66-228">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-228">**Workaround**</span></span>  
> <span data-ttu-id="22d66-229">Incluir un `using` instrucción (`imports` en Visual Basic) que hace referencia a las entidades que no se reconocen en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="22d66-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="22d66-230">Problema: Visuales Studio IntelliSense y plantillas de proyectos disponibles sólo en ASP.NET MVC versión 3</span><span class="sxs-lookup"><span data-stu-id="22d66-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="22d66-231">Instalar ASP.NET Web Pages no también instalar tools para Visual Studio, como IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="22d66-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="22d66-232">**Solución alternativa** para usar IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages en Visual Studio, instalar RC de ASP.NET MVC 3 mediante el instalador de plataforma Web o el [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="22d66-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="22d66-233">Problema: "&lt;auxiliar&gt; no se encuentra la clase" error</span><span class="sxs-lookup"><span data-stu-id="22d66-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="22d66-234">Después de actualizar a la versión Beta 3, es posible que vea un error que una clase auxiliar (por ejemplo, el `Facebook` clase) no se encuentra.</span><span class="sxs-lookup"><span data-stu-id="22d66-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="22d66-235">A partir de la versión Beta 2 y continuar en la versión Beta 3, las aplicaciones auxiliares se han movido a los paquetes que se deben instalar de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="22d66-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="22d66-236">No se actualizan los sitios existentes para incluir estos paquetes; Esto incluye los sitios en el *\My Documents\IISExpress* o *\My Documents\My sitios Web* carpetas.</span><span class="sxs-lookup"><span data-stu-id="22d66-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="22d66-237">En concreto, verá este error si usa el sitio predeterminado en *Mis sitios* (WebSite1), que incluye una referencia a la `Twitter` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="22d66-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="22d66-238">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-238">**Workaround**</span></span>  
> <span data-ttu-id="22d66-239">Comente las llamadas a las aplicaciones auxiliares en el sitio, ejecute el  *\_Admin* página e instale el paquete o paquetes que incluyen las aplicaciones auxiliares que desea usar.</span><span class="sxs-lookup"><span data-stu-id="22d66-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="22d66-240">Después de instalar el paquete, puede quitar el comentario de las líneas que hacen referencia a las aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="22d66-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="22d66-241">Problema: Implementar ensamblados de Razor de ASP.NET Beta 3 en la carpeta Bin podría no funcionar en sitios que hospeda</span><span class="sxs-lookup"><span data-stu-id="22d66-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="22d66-242">Si implementa un sitio Web de ASP.NET Web Pages en un sitio de hospedaje, y si implementa los ensamblados de la versión Beta 3 de ASP.NET Razor en la carpeta del sitio *Bin* carpeta, podría experimentar errores, incluidos los siguientes:</span><span class="sxs-lookup"><span data-stu-id="22d66-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="22d66-243">Esto puede ocurrir si el proveedor de hospedaje ha instalado a los ensamblados de ASP.NET Web Pages versión Beta 1 en la caché del servidor aplicación global (GAC).</span><span class="sxs-lookup"><span data-stu-id="22d66-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="22d66-244">Los ensamblados en la GAC obtención prioridad sobre los ensamblados instalados localmente en el *Bin* carpeta.</span><span class="sxs-lookup"><span data-stu-id="22d66-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="22d66-245">**Solución alternativa** póngase en contacto con su proveedor de hospedaje para confirmar que los errores que ve son debido a un conflicto entre las versiones del proveedor de los ensamblados a los suyos.</span><span class="sxs-lookup"><span data-stu-id="22d66-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="22d66-246">Si es así, solicite que el proveedor de hospedaje de actualizar los ensamblados en la GAC del servidor.</span><span class="sxs-lookup"><span data-stu-id="22d66-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="22d66-247">Problema: Las fuentes de lectura o de otros datos externos a través de un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="22d66-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="22d66-248">Si el servidor que ejecuta el sitio está detrás de un servidor proxy, deberá configurar la información de proxy en el *Web.config* archivo para poder leer la información que proviene de fuera de su sitio.</span><span class="sxs-lookup"><span data-stu-id="22d66-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="22d66-249">Por ejemplo, si usa el `ReCaptcha` auxiliar, la aplicación auxiliar se comunica con el servicio de reCAPTCHA, pero podría estar bloqueada por el servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="22d66-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="22d66-250">De forma similar, las fuentes que se usan en ASP.NET Web Pages, como la fuente utilizada por el Administrador de paquetes, pueden requerir la configuración de proxy.</span><span class="sxs-lookup"><span data-stu-id="22d66-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="22d66-251">Si experimenta problemas en trabajar con un servicio externo o trabajar con la fuente del paquete, colocar los elementos siguientes en la raíz de la aplicación *Web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="22d66-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="22d66-252">Para obtener más información acerca de cómo configurar un servidor proxy, consulte [ &lt;proxy&gt; (configuración de red) del elemento](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio Web de MSDN.</span><span class="sxs-lookup"><span data-stu-id="22d66-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="22d66-253">Problema: Error "No se puede cargar Microsoft.Web.Infrastructure.dll"</span><span class="sxs-lookup"><span data-stu-id="22d66-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="22d66-254">Si instaló la versión Beta 1 de ASP.NET Web Pages con sintaxis Razor y, a continuación, instalar la versión Beta 3, todos los ensamblados adecuados están instalados en la GAC, excepto *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="22d66-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="22d66-255">Como consecuencia, al ejecutar las páginas de Razor de ASP.NET, verá un error que indica que *Microsoft.Web.Infrastructure.dll* no se pudo cargar.</span><span class="sxs-lookup"><span data-stu-id="22d66-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="22d66-256">Este problema no ocurre si se carga la versión Beta 3 en un equipo limpio.</span><span class="sxs-lookup"><span data-stu-id="22d66-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="22d66-257">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-257">**Workaround**</span></span>  
> <span data-ttu-id="22d66-258">En el Panel de Control, desinstale ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="22d66-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="22d66-259">A continuación, vuelva a instalar la versión Beta 3.</span><span class="sxs-lookup"><span data-stu-id="22d66-259">Then reinstall the Beta 3 release.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="22d66-260">Problema: Al desinstalar la versión 4 de .NET Framework se deshabilita ASP.NET Web Pages con sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="22d66-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="22d66-261">Si desinstala la versión 4 de .NET Framework y, a continuación, vuelva a instalarlo, se deshabilita ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="22d66-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="22d66-262">Las páginas con la *.cshtml* extensión no se ejecutan correctamente.</span><span class="sxs-lookup"><span data-stu-id="22d66-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="22d66-263">Páginas Web de ASP.NET, se registra un ensamblado en la raíz de la máquina *Web.config* archivo y quitar .NET Framework se elimina el archivo.</span><span class="sxs-lookup"><span data-stu-id="22d66-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="22d66-264">Volver a instalar .NET Framework instala una nueva versión del archivo de configuración, pero no agrega la referencia del ensamblado de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="22d66-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="22d66-265">**Solución alternativa** después de volver a instalar .NET Framework, reinstale ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="22d66-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="22d66-266">Esto agrega el siguiente elemento a la *Web.config* archivo en la raíz de la máquina, que se encuentra normalmente en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="22d66-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="22d66-267">Problema: Las aplicaciones implementadas previamente con los ensamblados ASP.NET en la carpeta Bin experimentar errores</span><span class="sxs-lookup"><span data-stu-id="22d66-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="22d66-268">Durante la implementación, copias de los ensamblados de ASP.NET Web Pages (por ejemplo, *Microsoft.WebPages.dll*) a la *Bin* carpeta del sitio Web en el servidor.</span><span class="sxs-lookup"><span data-stu-id="22d66-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="22d66-269">(Esto podría haber ocurrido automáticamente durante la implementación o porque el desarrollador copiarse de forma explícita los ensamblados.) Sin embargo, cuando se instala la versión Beta 3, error se produce, como los errores que no se encuentra determinados tipos.</span><span class="sxs-lookup"><span data-stu-id="22d66-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="22d66-270">Esto ocurre porque varios tipos de páginas Web de ASP.NET se han movido a diferentes espacios de nombres para la versión Beta 3.</span><span class="sxs-lookup"><span data-stu-id="22d66-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="22d66-271">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="22d66-271">**Workaround** </span></span>  
> <span data-ttu-id="22d66-272">Desactive el *Bin* carpeta de la aplicación implementada, copie los nuevos ensamblados en la carpeta (o volver a implementar la aplicación) y, a continuación, reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22d66-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="22d66-273">Problema: Direcciones URL sin extensión no encuentra los archivos de.cshtml/.vbhtml en IIS 7 o IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="22d66-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="22d66-274">En IIS 7 o IIS 7.5, las solicitudes con una dirección URL similar a la siguiente no están posible encontrar las páginas que tienen la *.cshtml* o *.vbhtml* extensión:</span><span class="sxs-lookup"><span data-stu-id="22d66-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="22d66-275">El problema surge porque la reescritura de URL no está habilitada de forma predeterminada para IIS 7 o IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="22d66-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="22d66-276">El escenario más probable es que no ve el problema al realizar pruebas localmente mediante IIS Express, pero la experiencia al implementar su sitio Web en un sitio Web de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="22d66-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="22d66-277">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="22d66-278">Si tiene control sobre el equipo del servidor, en el equipo servidor instalar la actualización que se describe en [hay una actualización disponible que habilita ciertos controladores de IIS 7.0 o IIS 7.5 para controlar las solicitudes cuyas direcciones URL no termina con un punto](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="22d66-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="22d66-279">Si no tiene control sobre el equipo del servidor (por ejemplo, va a implementar en un sitio de hospedaje Web), agregue lo siguiente del sitio Web *Web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="22d66-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="22d66-280">Problema: Uso de proyecto de aplicación Web o páginas Web de ASP.NET y ASP.NET MVC en la misma aplicación</span><span class="sxs-lookup"><span data-stu-id="22d66-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="22d66-281">Si estaba usando las páginas Web ASP.NET en un proyecto de aplicación Web o una aplicación de ASP.NET MVC, es posible que vea un error que *WebPageHttpApplication* no se encuentra.</span><span class="sxs-lookup"><span data-stu-id="22d66-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="22d66-282">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-282">**Workaround**</span></span>  
> <span data-ttu-id="22d66-283">Si recibe este error, cambie la clase base del que se deriva la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22d66-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="22d66-284">En el *Global.asax* archivo, cambie la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="22d66-285">A esto:</span><span class="sxs-lookup"><span data-stu-id="22d66-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="22d66-286">Se invierte en vigor un cambio que se introdujo para la versión Beta 1 de ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="22d66-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="22d66-287">Problema: Implementar una aplicación en un equipo que no tiene SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="22d66-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="22d66-288">Las aplicaciones que incluyen bases de datos de SQL Server Compact pueden ejecutar en un equipo donde SQL Server Compact no está instalado.</span><span class="sxs-lookup"><span data-stu-id="22d66-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="22d66-289">Versión Beta 3 de Microsoft WebMatrix automáticamente copia estos archivos binarios para usted y realiza adecuado *Web.config* transformaciones de archivos.</span><span class="sxs-lookup"><span data-stu-id="22d66-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="22d66-290">**Solución alternativa** si tiene que copiar estos archivos y realizar el *Web.config* cambios en los archivos manualmente, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="22d66-291">Copie los ensamblados del motor de base de datos a la *Bin* carpeta (y las subcarpetas) de la aplicación en el equipo de destino:</span><span class="sxs-lookup"><span data-stu-id="22d66-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="22d66-292">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **a** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="22d66-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="22d66-293">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **a** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="22d66-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="22d66-294">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **a** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="22d66-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="22d66-295">En la carpeta raíz del sitio Web, cree o abra un *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="22d66-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="22d66-296">(En versión Beta 3 de WebMatrix, está disponible si hace clic en este tipo de archivo **todas** en el **elegir un tipo de archivo** cuadro de diálogo.)</span><span class="sxs-lookup"><span data-stu-id="22d66-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="22d66-297">Agregue el siguiente elemento como elemento secundario de la **&lt;configuración&gt;** elemento (no está dentro de la **&lt;system.web&gt;** elemento):</span><span class="sxs-lookup"><span data-stu-id="22d66-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="22d66-298">Problema: Aplicaciones auxiliares de WebGrid y de base de datos no funcionan en el nivel de confianza medio en Visual Basic</span><span class="sxs-lookup"><span data-stu-id="22d66-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="22d66-299">Si está utilizando Visual Basic (crear *.vbhtml* archivos), el `Database` y `WebGrid` aplicaciones auxiliares no funcionará si la aplicación está configurada para usar el nivel de confianza medio.</span><span class="sxs-lookup"><span data-stu-id="22d66-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="22d66-300">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-300">**Workaround**</span></span>  
> <span data-ttu-id="22d66-301">Establecer temporalmente la aplicación para usar la plena confianza.</span><span class="sxs-lookup"><span data-stu-id="22d66-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="22d66-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="22d66-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="22d66-303">Problema: No se reconoce la propiedad "Cifrar"</span><span class="sxs-lookup"><span data-stu-id="22d66-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="22d66-304">SQL Server Compact 4.0 no reconoce el `Encrypt` propiedad de la `SqlCeConnection` clase.</span><span class="sxs-lookup"><span data-stu-id="22d66-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="22d66-305">Esta propiedad no se debe usar para cifrar los archivos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="22d66-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="22d66-306">El `Encrypt` propiedad ha quedado obsoleta en la versión SQL Server Compact 3.5 y se conserva únicamente por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="22d66-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="22d66-307">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-307">**Workaround**</span></span>  
> <span data-ttu-id="22d66-308">Use la `Encryption Mode` propiedad de la `SqlCeConnection` clase para cifrar los archivos de base de datos de SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="22d66-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="22d66-309">El ejemplo siguiente muestra cómo crear una base de datos de SQL Server Compact 4.0 cifrados mediante el `Encryption Mode` propiedad:</span><span class="sxs-lookup"><span data-stu-id="22d66-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="22d66-310">Para cambiar el modo de cifrado de una base de datos existente de SQL Server Compact 4.0, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="22d66-311">Para cifrar una base de datos de SQL Server Compact 4.0 sin cifrar, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="22d66-312">Problema: Se requieren las bibliotecas en tiempo de ejecución de Microsoft Visual C++ 2008</span><span class="sxs-lookup"><span data-stu-id="22d66-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="22d66-313">La nativa archivos DLL de SQL Server Compact 4.0 necesita los Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 y x 64), Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="22d66-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="22d66-314">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-314">**Workaround**</span></span>  
> <span data-ttu-id="22d66-315">Instale .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="22d66-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="22d66-316">También se instala Visual C++ 2008 en tiempo de ejecución bibliotecas SP1.</span><span class="sxs-lookup"><span data-stu-id="22d66-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="22d66-317">Puede descargar las bibliotecas en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="22d66-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="22d66-318">Actualización de seguridad ATL paquete redistribuible de Microsoft Visual C++ 2008 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="22d66-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="22d66-319">Tenga en cuenta que al instalar .NET Framework 2.0, 3.0, o hace 4 *no* instalar Visual C++ 2008 en tiempo de ejecución bibliotecas SP1.</span><span class="sxs-lookup"><span data-stu-id="22d66-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="22d66-320">Problema: Si está instalado SQL Server Compact antes de instalar .NET Framework en el equipo, su nombre invariable del proveedor no está registrado en el archivo machine.config de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="22d66-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="22d66-321">SQL Server Compact se puede instalar en un equipo que no tiene .NET Framework instalado porque SQL Server Compact requieren .NET framework.</span><span class="sxs-lookup"><span data-stu-id="22d66-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="22d66-322">Si está instalado .NET Framework versión 3.5 ni 4 antes de instalar SQL Server Compact, el programa de instalación de SQL Server Compact no registra su nombre invariable del proveedor en el *machine.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="22d66-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="22d66-323">Cualquier aplicación que se basa en la entrada de SQL Server Compact en el *machine.config* archivo se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="22d66-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="22d66-324">La entrada de registro del nombre invariable en *machine.config* parece el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="22d66-325">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-325">**Workaround**</span></span>  
> <span data-ttu-id="22d66-326">Desinstalar SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="22d66-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="22d66-327">Descargue e instale las versiones completas de .NET Framework en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="22d66-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="22d66-328">Paquete de Microsoft .NET Framework 3.5 con Service 1 (paquete completo)</span><span class="sxs-lookup"><span data-stu-id="22d66-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="22d66-329">Versión de Microsoft .NET Framework 4.0 (paquete completo)</span><span class="sxs-lookup"><span data-stu-id="22d66-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="22d66-330">A continuación, vuelva a instalar [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="22d66-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="22d66-331">Instalación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="22d66-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="22d66-332">Problema: Instalar una aplicación puede tardar mucho tiempo si la carpeta Mis documentos del usuario se redirige a un recurso compartido de red</span><span class="sxs-lookup"><span data-stu-id="22d66-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="22d66-333">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-333">**Workaround**</span></span>  
> <span data-ttu-id="22d66-334">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="22d66-334">None.</span></span> <span data-ttu-id="22d66-335">La aplicación puede tardar un rato para instalar, pero se instalará correctamente.</span><span class="sxs-lookup"><span data-stu-id="22d66-335">The application might take a while to install, but will install correctly.</span></span>

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="22d66-336">Publicación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="22d66-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="22d66-337">Problema: Sitio no funcionen después de publicar si el campo "Dirección URL de destino" no es el prefijo http:// o https://</span><span class="sxs-lookup"><span data-stu-id="22d66-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="22d66-338">En el **configuración de publicación** cuadro de diálogo, si la dirección URL de destino no comienza con `http://` o `https://`, el sitio no funcionen después de la implementación.</span><span class="sxs-lookup"><span data-stu-id="22d66-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="22d66-339">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-339">**Workaround**</span></span>  
> <span data-ttu-id="22d66-340">Asegúrese de que antes de publicar un sitio, la dirección URL de destino en el **configuración de publicación** inicia el cuadro de diálogo con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="22d66-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="22d66-341">Problema: Publicación de una base de datos MySQL produce el error "no se pudo publicar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="22d66-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="22d66-342">Esto puede ocurrir si la base de datos remoto no puede ejecutar la secuencia de comandos."</span><span class="sxs-lookup"><span data-stu-id="22d66-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="22d66-343">El error puede producirse por una serie de motivos.</span><span class="sxs-lookup"><span data-stu-id="22d66-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="22d66-344">Una razón que ve este error es si la secuencia de comandos de base de datos contiene un carácter de comillas simples (') y el juego de caracteres de destino MySQL la base de datos predeterminado no es UTF-8.</span><span class="sxs-lookup"><span data-stu-id="22d66-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="22d66-345">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-345">**Workaround**</span></span>  
> <span data-ttu-id="22d66-346">Establece el carácter predeterminado establecido para la base de datos MySQL remota en UTF-8.</span><span class="sxs-lookup"><span data-stu-id="22d66-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="22d66-347">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="22d66-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="22d66-348">Problema: Búsqueda y filtrado no funciona en los informes para agrupar por: Tipo de problema</span><span class="sxs-lookup"><span data-stu-id="22d66-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="22d66-349">Al ejecutar un informe para un sitio, si escribe texto en el *filtrar por dirección URL* y haga clic en *búsqueda*, no ocurre nada.</span><span class="sxs-lookup"><span data-stu-id="22d66-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="22d66-350">Esto es porque este control no es funcional mientras se encuentre el *Group By* estado del informe se establece en *tipo de problema*, que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="22d66-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="22d66-351">**Solución alternativa** en el *Group By* ficha de cinta de opciones, haga clic en *URL* para agrupar las entradas por su dirección URL de origen.</span><span class="sxs-lookup"><span data-stu-id="22d66-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="22d66-352">El cuadro de texto y el botón para filtrar las entradas son funcionales mientras se encuentre en este estado.</span><span class="sxs-lookup"><span data-stu-id="22d66-352">The text box and button to filter the entries are functional while in this state.</span></span>

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="22d66-353">Problema: Aplicaciones WCF no se pueden ejecutar con IIS Express</span><span class="sxs-lookup"><span data-stu-id="22d66-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="22d66-354">Vaya a una aplicación de WCF produce un error como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="22d66-355">*No se pudo cargar el archivo o ensamblado ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o uno de sus dependencias. El sistema no puede encontrar el archivo especificado.*</span><span class="sxs-lookup"><span data-stu-id="22d66-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="22d66-356">Esto ocurre porque la versión Beta de IIS Express no es compatible con WCF de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="22d66-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="22d66-357">**Solución alternativa** usar cualquiera de las siguientes soluciones alternativas (solución #2 requiere Microsoft Windows Vista o posterior):</span><span class="sxs-lookup"><span data-stu-id="22d66-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="22d66-358">Copia el *Microsoft.Web.dll* y *Microsoft.Web.Administration.dll* ensamblados desde la ubicación de instalación de WebMatrix para el *bin* directorio de WCF aplicación.</span><span class="sxs-lookup"><span data-stu-id="22d66-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="22d66-359">De forma predeterminada, WebMatrix está instalado en el *Microsoft WebMatrix* subcarpeta en el sistema *archivos de programa* carpeta.</span><span class="sxs-lookup"><span data-stu-id="22d66-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="22d66-360">En Microsoft Windows Vista o versiones posteriores, cree un vínculo simbólico a los ensamblados en el *bin* directorio mediante los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="22d66-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="22d66-361">(Este enfoque tiene la ventaja de que no crea una copia de los ensamblados).</span><span class="sxs-lookup"><span data-stu-id="22d66-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="22d66-362">Instale a los dos ensamblados en la GAC.</span><span class="sxs-lookup"><span data-stu-id="22d66-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="22d66-363">Desde un símbolo del sistema con privilegios elevados, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="22d66-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="22d66-364">Problema: Versión Beta 3 de WebMatrix no puede realizar ciertas tareas que requieren la elevación</span><span class="sxs-lookup"><span data-stu-id="22d66-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="22d66-365">Versión Beta 3 de WebMatrix es no se puede realizar ciertas tareas que requieren la elevación, como la instalación de componentes adicionales en las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="22d66-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="22d66-366">En Windows Vista o Windows 7, que ha iniciado sesión con una cuenta que no tiene privilegios administrativos y Control de cuentas de usuario (UAC) está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="22d66-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="22d66-367">Está utilizando Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="22d66-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="22d66-368">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-368">**Workaround**</span></span>  
> <span data-ttu-id="22d66-369">Mayoría de las tareas en la versión Beta 3 de WebMatrix no requiere permisos administrativos.</span><span class="sxs-lookup"><span data-stu-id="22d66-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="22d66-370">Para aquellos que lo hacen, puede realizar la operación porque un administrador o siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="22d66-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="22d66-371">En Windows Vista o Windows 7, habilitar UAC.</span><span class="sxs-lookup"><span data-stu-id="22d66-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="22d66-372">En Windows XP, agregue el usuario al grupo de seguridad Administradores.</span><span class="sxs-lookup"><span data-stu-id="22d66-372">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="22d66-373">Problema: "Sitio desde galería Web" está deshabilitado</span><span class="sxs-lookup"><span data-stu-id="22d66-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="22d66-374">El **sitio desde galería Web** opción está deshabilitada si el instalador de plataforma Web 3.0 no está instalado.</span><span class="sxs-lookup"><span data-stu-id="22d66-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="22d66-375">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-375">**Workaround**</span></span>  
> <span data-ttu-id="22d66-376">Instalar el [instalador de plataforma Web de Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="22d66-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="22d66-377">Problema: En Windows Server 2003, IIS Express no se inicia para un usuario sin derechos administrativos</span><span class="sxs-lookup"><span data-stu-id="22d66-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="22d66-378">En Windows Server 2003, cuando se inicia una página o iniciar IIS Express, IIS Express no se inicia.</span><span class="sxs-lookup"><span data-stu-id="22d66-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="22d66-379">Para las páginas Web, se muestra un error que indica que la aplicación se ha iniciado por un usuario sin derechos administrativos.</span><span class="sxs-lookup"><span data-stu-id="22d66-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="22d66-380">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-380">**Workaround**</span></span>  
> <span data-ttu-id="22d66-381">Inicie la versión Beta 3 de WebMatrix como usuario administrativo.</span><span class="sxs-lookup"><span data-stu-id="22d66-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="22d66-382">Para obtener más información, consulte el artículo de Knowledge Base siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="22d66-383">Una aplicación que se inicia con un usuario sin derechos administrativos no puede escuchar el tráfico HTTP del equipo donde se ejecuta la aplicación en Windows Vista, Windows Server 2003 o Windows XP.</span><span class="sxs-lookup"><span data-stu-id="22d66-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="22d66-384">Problema: Google Chrome no está disponible como una opción de ejecución</span><span class="sxs-lookup"><span data-stu-id="22d66-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="22d66-385">Google Chrome no se muestra en la lista de exploradores en **ejecutar** en el **inicio** ficha.</span><span class="sxs-lookup"><span data-stu-id="22d66-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="22d66-386">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-386">**Workaround**</span></span>  
> <span data-ttu-id="22d66-387">Algunas versiones de Google Chrome no se registran correctamente con la característica de programas predeterminados en Windows.</span><span class="sxs-lookup"><span data-stu-id="22d66-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="22d66-388">Como alternativa, inicie Google Chrome, haga clic en el *control Google Chrome y personalizar* menú, haga clic en *opciones*y, a continuación, haga clic en *Make Google Chrome mi explorador predeterminado*.</span><span class="sxs-lookup"><span data-stu-id="22d66-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="22d66-389">Problema: El cuadro de diálogo "Foreign Key" no permite escribir una clave principal</span><span class="sxs-lookup"><span data-stu-id="22d66-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="22d66-390">El **Foreign Key** cuadro de diálogo no permite especificar el nombre de clave principal de la tabla de clave principal.</span><span class="sxs-lookup"><span data-stu-id="22d66-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="22d66-391">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-391">**Workaround**</span></span>  
> <span data-ttu-id="22d66-392">Esto tiene su porqué.</span><span class="sxs-lookup"><span data-stu-id="22d66-392">This is intentional.</span></span> <span data-ttu-id="22d66-393">No es necesario que escriba el nombre de la clave principal de la tabla de clave principal.</span><span class="sxs-lookup"><span data-stu-id="22d66-393">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="22d66-394">Problema: El botón "Relaciones" está deshabilitado</span><span class="sxs-lookup"><span data-stu-id="22d66-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="22d66-395">El **relaciones** situado bajo el **tabla** pestaña en el **bases de datos** área de trabajo está deshabilitado para las bases de datos de SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="22d66-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="22d66-396">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-396">**Workaround**</span></span>  
> <span data-ttu-id="22d66-397">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="22d66-397">None.</span></span> <span data-ttu-id="22d66-398">SQL Server Compact no admite relaciones entre tablas.</span><span class="sxs-lookup"><span data-stu-id="22d66-398">SQL Server Compact does not support relationships between tables.</span></span>

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="22d66-399">Problema: Consultas SQL parametrizadas producen excepciones</span><span class="sxs-lookup"><span data-stu-id="22d66-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="22d66-400">En SQL Server Compact 4.0, si no especifica un tipo de datos, como `SqlDbType` o `DbType` para los parámetros en consultas con parámetros, se produce una excepción cuando se ejecuta la consulta.</span><span class="sxs-lookup"><span data-stu-id="22d66-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="22d66-401">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="22d66-401">**Workaround**</span></span>  
> <span data-ttu-id="22d66-402">Establecer explícitamente el tipo de datos para los parámetros como `SqlDbType` o `DbType`.</span><span class="sxs-lookup"><span data-stu-id="22d66-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="22d66-403">Esto es muy importante en el caso de los tipos de datos BLOB (`image` y `ntext`).</span><span class="sxs-lookup"><span data-stu-id="22d66-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="22d66-404">Use código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="22d66-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="22d66-405">Para obtener más información</span><span class="sxs-lookup"><span data-stu-id="22d66-405">For More Information</span></span>

<span data-ttu-id="22d66-406">Para obtener más información acerca de la versión Beta 3 de WebMatrix, consulte los siguientes sitios Web:</span><span class="sxs-lookup"><span data-stu-id="22d66-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="22d66-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="22d66-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="22d66-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22d66-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="22d66-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="22d66-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

---

<span data-ttu-id="22d66-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="22d66-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="22d66-411">Reservados todos los derechos.</span><span class="sxs-lookup"><span data-stu-id="22d66-411">All Rights Reserved.</span></span> <span data-ttu-id="22d66-412">[Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="22d66-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
