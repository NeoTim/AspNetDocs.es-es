---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteración #2 – Hacer que la aplicación se vea bien (VB) Microsoft Docs'
author: rick-anderson
description: En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a97958db442c48bd696f6414f7226127984bc34
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542435"
---
# <a name="iteration-2--make-the-application-look-nice-vb"></a>Iteración n.º 2: Hacer que la aplicación tenga un buen aspecto (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de administración de contactos ASP.NET MVC (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto (nombres, números de teléfono y direcciones de correo electrónico) para una lista de personas.

Creamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de iteración múltiple es permitirle comprender el motivo de cada cambio.

- Iteración #1: cree la aplicación. En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos compatibilidad con operaciones básicas de base de datos: Crear, leer, actualizar y eliminar (CRUD).

- Iteración #2: haga que la aplicación se vea bien. En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.

- Iteración #3: agregar validación de formulario. En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos direcciones de correo electrónico y números de teléfono.

- #4 de iteración: haga que la aplicación se acople libremente. En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, refactorizamos nuestra aplicación para usar el patrón Repository y el patrón Dependency Injection.

- #5 de iteración: cree pruebas unitarias. En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelo de datos y creamos pruebas unitarias para nuestros controladores y lógica de validación.

- Iteración #6: use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- #7 de iteración: agregue la funcionalidad de Ajax. En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

El objetivo de esta iteración es mejorar el aspecto de la aplicación Contact Manager. Actualmente, el Administrador de contactos usa la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada (consulte la figura 1). Estos no se ven mal, pero no quiero que el Administrador de contactos se vea como todos los demás ASP.NET sitio web MVC. Quiero reemplazar estos archivos con archivos personalizados.

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Figura 01**: La apariencia predeterminada de una aplicación ASP.NET MVC[(haga clic para ver la imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image2.png)

En esta iteración, analizo dos enfoques para mejorar el diseño visual de nuestra aplicación. En primer lugar, le muestro cómo aprovechar la galería de diseño ASP.NET MVC para descargar una plantilla de diseño ASP.NET MVC gratuita. La galería de diseño de ASP.NET MVC le permite crear una aplicación web profesional sin realizar ningún trabajo.

Decidí no usar una plantilla de la galería de diseño ASP.NET MVC para la aplicación Contact Manager. En su lugar, tenía un diseño personalizado creado por una firma de diseño profesional. En la segunda parte de este tutorial, explico cómo trabajé con una empresa de diseño profesional para crear el diseño final ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La Galería de diseño de ASP.NET MVC

La Galería de diseño de ASP.NET MVC es un recurso gratuito proporcionado por Microsoft. La Galería ASP.NET MVC se encuentra en la siguiente dirección:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La Galería de diseño de ASP.NET MVC hospeda una colección de diseños de sitio web gratuitos que se crearon específicamente para su uso en un proyecto de ASP.NET MVC. Los diseños son cargados por miembros de la comunidad. Los visitantes de la Galería pueden votar por sus diseños favoritos (ver Figura 2).

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Figura 02**: la ASP.NET MVC Design Gallery[(Haga clic para ver la imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image4.png)

Mientras escribo este tutorial, el diseño más popular en la galería es un diseño llamado Octubre por David Hauser. Puede usar este diseño para un proyecto de ASP.NET MVC completando los pasos siguientes:

1. Haga clic en el botón **Descargar** para descargar el archivo October.zip en su computadora.
2. Haga clic con el botón derecho en el archivo October.zip descargado y haga clic en el botón **Desbloquear** (consulte la figura 3).
3. Descomprima el archivo en una carpeta denominada Octubre.
4. Seleccione todos los archivos de la carpeta DesignTemplate contenidos en la carpeta de octubre, haga clic con el botón derecho en los archivos y seleccione la opción de menú **Copiar**.
5. Haga clic con el botón secundario en el nodo del proyecto ContactManager en la ventana Explorador de soluciones de Visual Studio y seleccione la opción de menú **Pegar** (consulte la figura 4).
6. Seleccione la opción de menú de Visual Studio **Editar, Buscar y reemplazar, Reemplazar y** reemplazar *rápidamente [MyProjectName]* por *ContactManager* (consulte la figura 5).

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Figura 03**: Desbloqueo de un archivo descargado de la web (Haga clic para ver la[imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image6.png)

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Figura 04**: Sobrescribir archivos en el Explorador de soluciones ([Haga clic para ver la imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image8.png))

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Figura 05**: Sustitución de [ProjectName] por ContactManager ( Haga clic para ver la[imagen de tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image10.png))

Después de completar estos pasos, la aplicación web usará el nuevo diseño. La página de la Figura 6 ilustra la apariencia de la aplicación Contact Manager con el diseño de octubre.

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Figura 06**: ContactManager con la plantilla de octubre[(Haga clic para ver la imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image12.png)

## <a name="creating-a-custom-aspnet-mvc-design"></a>Creación de un diseño de MVC de ASP.NET personalizada

La Galería de diseño ASP.NET MVC tiene una buena selección de diferentes estilos de diseño. La Galería le proporciona una manera indolora de personalizar la apariencia de las aplicaciones mvc ASP.NET. Y, por supuesto, la Galería tiene la gran ventaja de ser completamente libre.

Sin embargo, es posible que deba crear un diseño completamente único para su sitio web. En ese caso, tiene sentido trabajar con una empresa de diseño de sitios web. Decidí adoptar este enfoque para el diseño de la aplicación Contact Manager.

Comuniqué el Contact Manager de Iteration #1 y envié el proyecto a la empresa de diseño. No eran propietarios de Visual Studio (verguenza en ellos!), pero eso no presentó un problema. Fueron capaces de descargar Microsoft Visual [https://www.asp.net](https://www.asp.net) Web Developer de forma gratuita desde el sitio web y abrir la aplicación Contact Manager en Visual Web Developer. En un par de días, habían producido el diseño en la Figura 7.

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Figura 07**: el diseño del administrador de contactos de ASP.NET MVC[(haga clic para ver la imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image14.png)

El nuevo diseño consistía en dos archivos principales: un nuevo archivo de hoja de estilos en cascada y un nuevo archivo de página maestra de vista. Una página maestra de vista contiene el diseño y el contenido compartido para las vistas en una aplicación ASP.NET MVC. Por ejemplo, la página maestra de vista incluye el encabezado, las pestañas de navegación y el pie de página que aparecen en la figura 7. He sobreescrito la página maestra de la vista Site.Master existente en la carpeta Views-Shared con el nuevo archivo Site.Master de la empresa de diseño,

La empresa de diseño también creó una nueva hoja de estilos en cascada y un conjunto de imágenes. Puse estos nuevos archivos en la carpeta Contenido y sobreescribí el archivo Site.css existente. Debe colocar todo el contenido estático en la carpeta Contenido.

Tenga en cuenta que el nuevo diseño del Administrador de contactos incluye imágenes para editar y eliminar contactos. Aparece una imagen Editar y Eliminar junto a cada contacto en la tabla HTML de contactos.

Originalmente, estos vínculos que se representaban con el HTML. ActionLink() ayudante de la siguiente manera:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

El método Html.ActionLink() no admite imágenes (el método HTML codifica el texto del vínculo por motivos de seguridad). Por lo tanto, reemplacé las llamadas a Html.ActionLink() con llamadas a Url.Action() como esta:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

El método Html.ActionLink() representa un hipervínculo HTML completo. El método Url.Action(), por otro lado, representa solo &lt;&gt; la URL sin la etiqueta a.

Observe, además, que el nuevo diseño incluye pestañas seleccionadas y no seleccionadas. Por ejemplo, en la Figura 8, se selecciona la pestaña **Crear nuevo contacto** y la pestaña Mis **contactos** no está seleccionada.

[![Cuadro de diálogo Nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Figura 08**: Pestañas seleccionadas y no seleccionadas[(Haga clic para ver la imagen de tamaño completo)](iteration-2-make-the-application-look-nice-vb/_static/image16.png)

Para admitir la representación de pestañas seleccionadas y no seleccionadas, he creado una aplicación auxiliar HTML personalizada denominada MenuItemHelper. Este método auxiliar representa &lt;&gt; una etiqueta &lt;li o una&gt; etiqueta li class-"selected" dependiendo de si el controlador actual y la acción corresponden al controlador y el nombre de acción pasados a la aplicación auxiliar. El código de MenuItemHelper se encuentra en el listado 1.

**Listado 1 – Ayudantes-MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper utiliza la clase TagBuilder &lt;internamente para crear la etiqueta HTML li.&gt; La clase TagBuilder es una clase de utilidad muy útil que puede utilizar siempre que necesite crear una nueva etiqueta HTML. Incluye métodos para agregar atributos, agregar clases CSS, generar identificadores y modificar el HTML interno de la etiqueta s.

## <a name="summary"></a>Resumen

En esta iteración, hemos mejorado el diseño visual de nuestra aplicación ASP.NET MVC. En primer lugar, se le presentó la ASP.NET MVC Design Gallery. Ha aprendido a descargar plantillas de diseño gratuitas desde la Galería de diseño de ASP.NET MVC que puede usar en las aplicaciones MVC de ASP.NET.

A continuación, analizamos cómo puede crear un diseño personalizado modificando el archivo de hoja de estilos en cascada predeterminado y el archivo de página de vista maestra. Con el fin de apoyar el nuevo diseño, tuvimos que hacer algunos cambios menores a nuestra aplicación Contact Manager. Por ejemplo, agregamos una nueva aplicación auxiliar HTML denominada MenuItemHelper que muestra las pestañas seleccionadas y no seleccionadas.

En la siguiente iteración, abordamos el tema muy importante de la validación. Agregamos código de validación a nuestra aplicación para que un usuario no pueda crear un nuevo contacto sin proporcionar los valores necesarios, como el nombre y el apellido de una persona.

> [!div class="step-by-step"]
> [Anterior](iteration-1-create-the-application-vb.md)
> [Siguiente](iteration-3-add-form-validation-vb.md)
