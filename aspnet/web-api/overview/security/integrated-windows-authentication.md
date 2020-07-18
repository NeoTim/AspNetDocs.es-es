---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticación integrada de Windows | Microsoft Docs
author: MikeWasson
description: Describe el uso de la autenticación integrada de Windows en ASP.NET Web API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: c5fe57c4a20e28fa434659a75484e3a4c37195f8
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424785"
---
# <a name="integrated-windows-authentication"></a>Autenticación integrada de Windows

por [Mike Wasson](https://github.com/MikeWasson)

La autenticación integrada de Windows permite a los usuarios iniciar sesión con sus credenciales de Windows, mediante Kerberos o NTLM. El cliente envía las credenciales en el encabezado de autorización. La autenticación de Windows es más adecuada para un entorno de intranet. Para obtener más información, vea [Autenticación de Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Ventajas | Inconvenientes |
| --- | --- |
| Integrado en IIS. | No se recomienda para las aplicaciones de Internet. | 
| No envía las credenciales de usuario en la solicitud. | Requiere compatibilidad con Kerberos o NTLM en el cliente. |
| Si el equipo cliente pertenece al dominio (por ejemplo, una aplicación de intranet), no es necesario que el usuario escriba las credenciales. | El cliente debe estar en el dominio de Active Directory. |

> [!NOTE]
> Si la aplicación se hospeda en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federar su AD local con Azure Active Directory. De este modo, los usuarios pueden iniciar sesión con sus credenciales locales, pero la autenticación se realiza mediante Azure AD. Para obtener más información, consulte [autenticación de Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Para crear una aplicación que use la autenticación integrada de Windows, seleccione la plantilla "aplicación de intranet" en el Asistente para proyectos de MVC 4. Esta plantilla de proyecto coloca la siguiente configuración en el archivo Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

En el lado cliente, la autenticación integrada de Windows funciona con cualquier explorador que admita el esquema de autenticación [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que incluye la mayoría de los exploradores principales. En el caso de las aplicaciones cliente de .NET, la clase **HttpClient** admite la autenticación de Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

La autenticación de Windows es vulnerable a los ataques de falsificación de solicitud entre sitios (CSRF). Consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
