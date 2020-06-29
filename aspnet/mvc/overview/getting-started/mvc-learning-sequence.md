---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Tutoriales y artículos recomendados para MVC | Microsoft Docs
author: Rick-Anderson
description: Esta página contiene vínculos a tutoriales de ASP.NET MVC y una secuencia sugerida para seguirlos.
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 7dc81cf09309194df4471fedfc74d4051f0fdb78
ms.sourcegitcommit: 8d34fb54e790cfba2d64097afc8276da5b22283e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85484222"
---
# <a name="mvc-recommended-tutorials-and-articles"></a>Artículos y tutoriales recomendados de MVC

por [Rick Anderson](https://twitter.com/RickAndMSFT)

<a id="pwd"></a>
## <a name="getting-started"></a>Introducción

- [Introducción con MVC 5 de ASP.net](introduction/getting-started.md) Esta serie de 11 partes es un buen punto de partida.
- [Aspectos básicos de Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (curso de vídeo)
- [Introducción a ASP.NET MVC](https://channel9.msdn.com/Series/Introduction-to-ASP-NET-MVC) by Jon Galloway y Christopher Harrison
- [Ciclo de vida de una aplicación ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) Documento PDF que gráfico del ciclo de vida de una aplicación ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Trabajar con datos

- [Introducción con EF 6 Code First con MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) La serie premiada de Tom Dykstra se profundiza en EF.

<a id="wj"></a>
## <a name="security"></a>Seguridad

- [Creación de una aplicación de ASP.NET MVC con autenticación y base de SQL Database e implementación en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Este tutorial popular le guiará a través de la creación de una aplicación sencilla y la adición de pertenencia y roles.
- [Creación de una aplicación de ASP.NET MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) En este tutorial se muestra cómo crear una aplicación web MVC 5 de ASP.NET que permite a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google.
- [Creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) En primer lugar, en una serie de identidad, incluye código para [volver a enviar un vínculo de confirmación](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Aplicación ASP.NET MVC 5 con autenticación en dos fases de SMS y correo electrónico](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Segundo en la serie de identidades.
- [Prácticas recomendadas para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` y la cookie de seguridad, código para requerir a un usuario que tenga una cuenta de correo electrónico validada antes de que pueda iniciar sesión, cómo SignInManager comprueba los requisitos de 2FA y mucho más.
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Proporciona detalles sobre la identidad no encontrada en [creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , por ejemplo, cómo permitir a los usuarios restablecer su contraseña olvidada.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Creación de una aplicación Web de ASP.net en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) Tutorial breve y sencillo para la implementación en Azure.
- [Creación de una aplicación de ASP.NET MVC con autenticación y base de SQL Database e implementación en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Rendimiento y depuración

- [Crear un perfil y depurar la aplicación de ASP.NET MVC con Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)

## <a name="aspnet-mvc-dropdownlistfor-with-selectlistitem"></a>ASP.NET MVC DropDownListFor con SelectListItem

Cuando se usa el <xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A> ayudante y se le pasa la colección de `SelectListItem` desde la que se rellena, el `DropdownListFor` modifica la colección pasada después de llamarla. `DropdownListFor`cambia las `SelectListItems` propiedades seleccionadas a lo que haya seleccionado la lista desplegable. Esto provoca un comportamiento inesperado.

<xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A>,, <xref:System.Web.Mvc.Html.SelectExtensions.DropDownList%2A> <xref:System.Web.Mvc.Html.SelectExtensions.EnumDropDownListFor%2A> , <xref:System.Web.Mvc.Html.SelectExtensions.ListBox%2A> Y <xref:System.Web.Mvc.Html.SelectExtensions.ListBoxFor%2A> actualizan la propiedad seleccionada de cualquier `IEnumerable<SelectListItem>` pasada o encontrada en ViewData.

La solución consiste en crear enumerables independientes, que contienen `SelectListItem` instancias distintas, para cada propiedad del modelo.

Para obtener más información, vea

* [DropdownListFor modifica la colección SelectListItem que se le ha pasado](http://web.archive.org/web/20140902031437/http://aspnetwebstack.codeplex.com/workitem/1913)
* [GetSelectListWithDefaultValue modifica IEnumerable <SelectListItem> selectList](https://github.com/aspnet/AspNetWebStack/issues/271)