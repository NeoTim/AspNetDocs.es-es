---
title: Migrar de autenticación e identidad a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar de autenticación e identidad de un proyecto ASP.NET MVC a un proyecto de ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052992"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrar de autenticación e identidad a ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

En el artículo anterior, hemos [migrar configuración de un proyecto de ASP.NET MVC a ASP.NET Core MVC](xref:migration/configuration). En este artículo, nos migrar las características de administración de registro, inicio de sesión y usuario.

## <a name="configure-identity-and-membership"></a>Configurar la identidad y pertenencia

En ASP.NET MVC, las características de autenticación e identidad se configuran mediante ASP.NET Identity en *Startup.Auth.cs* y *IdentityConfig.cs*, que se encuentra en la *App_Start* carpeta. En ASP.NET Core MVC, estas características se configuran en *Startup.cs*.

Instalar el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` y `Microsoft.AspNetCore.Authentication.Cookies` paquetes de NuGet.

A continuación, abra *Startup.cs* y actualizar la `Startup.ConfigureServices` método para usar los servicios de identidad y Entity Framework:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

En este momento, hay dos tipos que se hace referencia en el código anterior que hemos aún no hemos migrado desde el proyecto de ASP.NET MVC: `ApplicationDbContext` y `ApplicationUser`. Cree un nuevo *modelos* carpeta en el núcleo de ASP.NET del proyecto y agregarle dos clases correspondientes a estos tipos. Encontrará el ASP.NET MVC versiones de estas clases en */Models/IdentityModels.cs*, pero vamos a utilizar un archivo por cada clase en el proyecto migrado, ya que es más clara.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

El proyecto Web de ASP.NET Core MVC Starter no incluye mucha personalización de los usuarios, o la `ApplicationDbContext`. Al migrar una aplicación real, también deberá migrar todas las propiedades personalizadas y los métodos de usuario de la aplicación y `DbContext` clases, así como otras clases de modelo usa la aplicación. Por ejemplo, si su `DbContext` tiene un `DbSet<Album>`, tiene que migrar la `Album` clase.

Con estos archivos en su lugar, el *Startup.cs* archivo se puede realizar para compilar mediante la actualización de su `using` instrucciones:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Nuestra aplicación ahora está lista para admitir la autenticación y los servicios de identidad. Basta con que tenga estas características que se exponen a los usuarios.

## <a name="migrate-registration-and-login-logic"></a>Migrar la lógica de inicio de sesión y registro

Con la configurada para la aplicación de servicios de identidad y acceso a datos configurado mediante Entity Framework y SQL Server, ya estamos listos para agregar compatibilidad con inicio de sesión y registro de la aplicación. Recuerde que [anteriormente en el proceso de migración](xref:migration/mvc#migrate-the-layout-file) nos comentadas una referencia a *_LoginPartial* en *_Layout.cshtml*. Ahora es momento de volver a ese código, quite los comentarios y agregue en las vistas para admitir la funcionalidad de inicio de sesión y los controladores necesarios.

Quite el `@Html.Partial` línea *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Ahora, agregue una nueva vista de Razor denominada *_LoginPartial* a la *Views/Shared* carpeta:

Actualización *_LoginPartial.cshtml* con el código siguiente (reemplace todo su contenido):

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

En este momento, podrá actualizar el sitio en el explorador.

## <a name="summary"></a>Resumen

ASP.NET Core presenta cambios en las características de ASP.NET Identity. En este artículo, ha visto cómo migrar las características de administración de usuario y autenticación de ASP.NET Identity a ASP.NET Core.
