---
title: Autorización basada en roles en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo restringir el acceso de acción y controlador de ASP.NET Core pasando roles para el atributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: c38e7144166ce7741eee6e3acb4d1c952ad4f024
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054382"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="bf986-103">Autorización basada en roles en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf986-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="bf986-104">Cuando se crea una identidad puede pertenecer a uno o varios roles.</span><span class="sxs-lookup"><span data-stu-id="bf986-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="bf986-105">Por ejemplo, mujer puede pertenecer a los roles de administrador y usuario mientras Scott solo puede pertenecer al rol de usuario.</span><span class="sxs-lookup"><span data-stu-id="bf986-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="bf986-106">Cómo se crean y administran estas funciones depende del almacén de respaldo del proceso de autorización.</span><span class="sxs-lookup"><span data-stu-id="bf986-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="bf986-107">Las funciones se exponen a los desarrolladores a través de la [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) método en el [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) clase.</span><span class="sxs-lookup"><span data-stu-id="bf986-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="bf986-108">Agregar comprobaciones de la función</span><span class="sxs-lookup"><span data-stu-id="bf986-108">Adding role checks</span></span>

<span data-ttu-id="bf986-109">Comprobaciones de autorización basada en roles son declarativas&mdash;el desarrollador incrusta dentro de su código, con un controlador o una acción dentro de un controlador, especificar las funciones que el usuario actual debe ser miembro de tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="bf986-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="bf986-110">Por ejemplo, el siguiente código limita el acceso a todas las acciones en el `AdministrationController` a los usuarios que son miembros de la `Administrator` rol:</span><span class="sxs-lookup"><span data-stu-id="bf986-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="bf986-111">Puede especificar varios roles como una lista separada por comas:</span><span class="sxs-lookup"><span data-stu-id="bf986-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="bf986-112">Este controlador sería sólo puede tener acceso a los usuarios que son miembros de la `HRManager` rol o la `Finance` rol.</span><span class="sxs-lookup"><span data-stu-id="bf986-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="bf986-113">Si aplica varios atributos de un usuario que obtiene acceso debe ser miembro de todas las funciones especificadas; el ejemplo siguiente requiere que un usuario debe ser miembro de la `PowerUser` y `ControlPanelUser` rol.</span><span class="sxs-lookup"><span data-stu-id="bf986-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="bf986-114">Puede limitar aún más el acceso aplicando los atributos de autorización de rol adicionales en el nivel de acción:</span><span class="sxs-lookup"><span data-stu-id="bf986-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="bf986-115">En los miembros de fragmento de código anterior de la `Administrator` rol o la `PowerUser` rol puede acceder al controlador y el `SetTime` acción, pero solo los miembros de la `Administrator` rol puede tener acceso el `ShutDown` acción.</span><span class="sxs-lookup"><span data-stu-id="bf986-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="bf986-116">También puede bloquear un controlador, pero permitir el acceso anónimo, no autenticado a acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="bf986-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bf986-117">Para las páginas de Razor, el `AuthorizeAttribute` puede aplicarse, ya sea por:</span><span class="sxs-lookup"><span data-stu-id="bf986-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="bf986-118">Mediante un [convención](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), o</span><span class="sxs-lookup"><span data-stu-id="bf986-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="bf986-119">Aplicar el `AuthorizeAttribute` a la `PageModel` instancia:</span><span class="sxs-lookup"><span data-stu-id="bf986-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="bf986-120">Filtrar los atributos, como `AuthorizeAttribute`, sólo puede aplicarse a PageModel y no se puede aplicar a métodos de controlador de página específica.</span><span class="sxs-lookup"><span data-stu-id="bf986-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end


<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="bf986-121">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="bf986-121">Policy based role checks</span></span>

<span data-ttu-id="bf986-122">Requisitos del rol también se pueden expresar utilizando la sintaxis de directiva nueva, donde el desarrollador registra una directiva en el inicio como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="bf986-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="bf986-123">Normalmente, esto ocurre en `ConfigureServices()` en su *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="bf986-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="bf986-124">Las directivas se aplican mediante el `Policy` propiedad en el `AuthorizeAttribute` atributo:</span><span class="sxs-lookup"><span data-stu-id="bf986-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="bf986-125">Si desea especificar varios roles permitidos en un requisito, puede especificarlas como parámetros a la `RequireRole` método:</span><span class="sxs-lookup"><span data-stu-id="bf986-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="bf986-126">En este ejemplo, autoriza a los usuarios que pertenecen a la `Administrator`, `PowerUser` o `BackupAdministrator` roles.</span><span class="sxs-lookup"><span data-stu-id="bf986-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="bf986-127">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="bf986-127">Add Role services to Identity</span></span>

<span data-ttu-id="bf986-128">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="bf986-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
