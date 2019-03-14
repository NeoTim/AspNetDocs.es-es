---
ms.openlocfilehash: 528dafa1ef39982fde2e11428a74c5c26f341554
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033662"
---
La plantilla predeterminada crea los vínculos y las páginas **RazorPagesMovie**, **Inicio**, **Sobre** y **Contacto**. Según el tamaño de la ventana del explorador, tendrá que hacer clic en el icono de navegación para mostrar los vínculos.

![Página principal o de índice](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Pruebe los vínculos. Los vínculos **RazorPagesMovie** e **Inicio** dirigen a la página de índice. Los vínculos **Acerca de** y **Contacto** dirigen a las páginas `About` y `Contact` respectivamente.

## <a name="project-files-and-folders"></a>Archivos y carpetas del proyecto

En la tabla siguiente se enumeran los archivos y las carpetas del proyecto. En este tutorial, el archivo *Startup.cs* es el más importante. No es necesario revisar todos los vínculos siguientes. Los vínculos se proporcionan como referencia para cuando necesite más información sobre un archivo o una carpeta del proyecto.

| Archivo o carpeta | Propósito |
| -------------- | ------- |
| *wwwroot* | Contiene recursos estáticos. Vea [Archivos estáticos](xref:fundamentals/static-files). |
| *Páginas* | Carpeta para [páginas de Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configuración](xref:fundamentals/configuration/index) |
| *Program.cs* | Configura el [host](xref:fundamentals/index#host) de la aplicación ASP.NET Core. |
| *Startup.cs* | Configura los servicios y la canalización de solicitudes. Consulte [Inicio](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>La carpeta Pages/Shared

El archivo *_Layout.cshtml* contiene elementos HTML comunes (vínculos de scripts y hojas de estilos) y establece el diseño de la aplicación. Por ejemplo, cuando se selecciona **RazorPagesMovie**, **Home**, **About** o **Contact**, aparece un conjunto común de elementos en la página web. Los elementos comunes incluyen el menú de navegación de la parte superior y el encabezado de la parte inferior de la ventana. Para obtener más información, vea [Diseño](xref:mvc/views/layout).

El archivo *_ValidationScriptsPartial.cshtml* proporciona una referencia de los scripts de validación de [jQuery](https://jquery.com/). Al agregar las páginas `Create` y `Edit` más adelante en el tutorial, se usa el archivo *_ValidationScriptsPartial.cshtml*.

El archivo *_CookieConsentPartial.cshtml* proporciona una barra de navegación y contenido para resumir la directiva de privacidad y uso de cookies. Para obtener más información sobre los activos del RGPD que se incluyen en el proyecto, consulte [Compatibilidad con el Reglamento general de protección de datos (RGPD) de la UE](xref:security/gdpr).

### <a name="the-pages-folder"></a>Carpeta Páginas

El archivo *_ViewStart.cshtml* establece la propiedad `Layout` de las páginas de Razor que para usar el archivo *_Layout.cshtml*. Vea [Layout](xref:mvc/views/layout) (Diseño) para más información.

El archivo *_ViewImports.cshtml* contiene directivas de Razor que se importan en cada página de Razor. Consulte [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) (Importar directivas compartidas) para obtener más información.

Las páginas `About`, `Contact` y `Index` son páginas básicas que puede usar para iniciar una aplicación. La página `Error` se usa para mostrar información de errores.
