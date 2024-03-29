---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Crear una interfaz para seleccionar una cuenta de usuario de muchos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, se creará una interfaz de usuario con una cuadrícula paginable y filtrable. En concreto, nuestra interfaz de usuario constará de una serie de LinkButtons para...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 625e27442c790e7a7228b8f78b8a40dbee8e3b03
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044589"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Crear una interfaz para seleccionar una cuenta de usuario entre varias cuentas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> En este tutorial, se creará una interfaz de usuario con una cuadrícula paginable y filtrable. En concreto, nuestra interfaz de usuario constará de una serie de LinkButtons para filtrar los resultados en función de la letra inicial del nombre de usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos enumerando todas las cuentas de usuario en un control GridView. Después, en el paso 3, agregaremos el filtro LinkButtons. En el paso 4 se examina la paginación de los resultados filtrados. La interfaz construida en los pasos 2 a 4 se usará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.

## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"></a> tutorial [*asignación de roles a usuarios*](../roles/assigning-roles-to-users-vb.md) , hemos creado una interfaz rudimentaria para que un administrador Seleccione un usuario y administre sus roles. En concreto, la interfaz presentó al administrador una lista desplegable de todos los usuarios. Este tipo de interfaz es adecuado cuando hay una docena de cuentas de usuario, pero no lo es, sino que es difícil para sitios con cientos o miles de cuentas. Una cuadrícula filtrable y paginable es una interfaz de usuario más adecuada para sitios web con grandes bases de usuarios.

En este tutorial, se creará una interfaz de usuario de este tipo. En concreto, nuestra interfaz de usuario constará de una serie de LinkButtons para filtrar los resultados en función de la letra inicial del nombre de usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos enumerando todas las cuentas de usuario en un control GridView. Después, en el paso 3, agregaremos el filtro LinkButtons. En el paso 4 se examina la paginación de los resultados filtrados. La interfaz construida en los pasos 2 a 4 se usará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.

Comencemos.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: agregar nuevas páginas de ASP.NET

En este tutorial y en los dos siguientes, examinaremos varias funciones y funcionalidades relacionadas con la administración. Se necesitará una serie de páginas de ASP.NET para implementar los temas examinados en estos tutoriales. Vamos a crear estas páginas y a actualizar el mapa del sitio.

Empiece por crear una nueva carpeta en el proyecto denominado `Administration` . A continuación, agregue dos nuevas páginas ASP.NET a la carpeta, vinculando cada página con la `Site.master` página maestra. Asigne un nombre a las páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Agregue también dos páginas al directorio raíz del sitio web: `ChangePassword.aspx` y `RecoverPassword.aspx` .

En este punto, estas cuatro páginas deben tener dos controles de contenido, uno para cada una de las ContentPlaceHolders de la página maestra: `MainContent` y `LoginContent` .

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Queremos mostrar el marcado predeterminado de la página maestra para `LoginContent` estas páginas. Por consiguiente, quite el marcado declarativo para el `Content2` control de contenido. Después de hacerlo, el marcado de las páginas debe contener solo un control de contenido.

Las páginas de ASP.NET de la `Administration` carpeta están destinadas únicamente a usuarios administrativos. Hemos agregado un rol de administrador al sistema en el tutorial <a id="_msoanchor_2"></a> [*creación y administración de roles*](../roles/creating-and-managing-roles-vb.md) ; restrinja el acceso a estas dos páginas a este rol. Para ello, agregue un `Web.config` archivo a la `Administration` carpeta y configure su `<authorization>` elemento para admitir usuarios en el rol administradores y para denegar todos los demás.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

En este momento el Explorador de soluciones del proyecto debe ser similar a la captura de pantalla que se muestra en la figura 1.

[![Se han agregado cuatro páginas nuevas y un archivo Web.config al sitio web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: se han agregado cuatro páginas nuevas y un `Web.config` archivo al sitio web ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))

Por último, actualice el mapa del sitio ( `Web.sitemap` ) para incluir una entrada en la `ManageUsers.aspx` página. Agregue el siguiente código XML después `<siteMapNode>` de la adición de para los tutoriales de roles.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Con el mapa del sitio actualizado, visite el sitio a través de un explorador. Como se muestra en la figura 2, la navegación de la izquierda incluye ahora elementos para los tutoriales de administración.

[![El mapa del sitio incluye un nodo denominado administración de usuarios](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: el mapa del sitio incluye un nodo denominado administración de usuarios ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Paso 2: Mostrar todas las cuentas de usuario en un control GridView

El objetivo final de este tutorial es crear una cuadrícula paginable y filtrable a través de la cual un administrador puede seleccionar una cuenta de usuario para administrarla. Comencemos por una lista de *todos* los usuarios en un control GridView. Una vez completado, se agregarán las interfaces y la funcionalidad de filtrado y paginación.

Abra la `ManageUsers.aspx` página en la `Administration` carpeta y agregue un control GridView, estableciendo su en `ID` `UserAccounts` en un momento, se escribirá código para enlazar el conjunto de cuentas de usuario a GridView mediante el `Membership` método de la clase `GetAllUsers` . Como se describe en los tutoriales anteriores, el `GetAllUsers` método devuelve un `MembershipUserCollection` objeto, que es una colección de `MembershipUser` objetos. Cada `MembershipUser` de la colección incluye propiedades como `UserName` , `Email` , `IsApproved` , etc.

Para mostrar la información de la cuenta de usuario deseada en GridView, establezca la propiedad de GridView en `AutoGenerateColumns` false y agregue BoundFields para las `UserName` propiedades, y `Email` `Comment` y CheckBoxFields para las `IsApproved` propiedades, `IsLockedOut` y `IsOnline` . Esta configuración se puede aplicar a través del marcado declarativo del control o a través del cuadro de diálogo campos. En la ilustración 3 se muestra una captura de pantalla del cuadro de diálogo campos después de que la casilla generar automáticamente campos se haya desactivado y se hayan agregado y configurado BoundFields y CheckBoxFields.

[![Agregar tres BoundFields y tres CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: agregar tres BoundFields y tres CheckBoxFields a GridView ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))

Después de configurar GridView, asegúrese de que su marcado declarativo es similar al siguiente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

A continuación, es necesario escribir código que enlaza las cuentas de usuario a GridView. Cree un método denominado `BindUserAccounts` para realizar esta tarea y, a continuación, llámelo desde el `Page_Load` controlador de eventos en la primera visita de la página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Dedique un momento a probar la página a través de un explorador. Como se muestra en la figura 4, el control `UserAccounts` GridView muestra el nombre de usuario, la dirección de correo electrónico y otra información pertinente de la cuenta para todos los usuarios del sistema.

[![Las cuentas de usuario se muestran en GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: las cuentas de usuario se muestran en GridView ([haga clic para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Paso 3: filtrar los resultados por la primera letra del nombre de usuario

Actualmente, `UserAccounts` GridView muestra *todas* las cuentas de usuario. En el caso de los sitios web con cientos o miles de cuentas de usuario, es imperativo que el usuario pueda reducir rápidamente las cuentas mostradas. Esto puede realizarse agregando filtro LinkButtons a la página. Vamos a agregar 27 LinkButtons a la página: uno titulado todo junto con un LinkButton para cada letra del alfabeto. Si un visitante hace clic en el control LinkButton, el control GridView mostrará todos los usuarios. Si hace clic en una letra determinada, solo se mostrarán los usuarios cuyo nombre de usuario empiece por la letra seleccionada.

La primera tarea es agregar los 27 controles LinkButton. Una opción sería crear el 27 LinkButtons mediante declaración, de uno en uno. Un enfoque más flexible es usar un control Repeater con un `ItemTemplate` que representa un LinkButton y, a continuación, enlaza las opciones de filtrado al repetidor como una `String` matriz.

Empiece agregando un control Repeater a la página sobre `UserAccounts` GridView. Establezca la propiedad del repetidor `ID` para `FilteringUI` configurar las plantillas del repetidor de modo que `ItemTemplate` represente un objeto LinkButton `Text` cuyas `CommandName` propiedades y estén enlazadas al elemento de la matriz actual. Como vimos en el tutorial <a id="_msoanchor_3"></a> [*asignar roles a usuarios*](../roles/assigning-roles-to-users-vb.md) , esto se puede realizar mediante la `Container.DataItem` Sintaxis de DataBinding. Use el repetidor `SeparatorTemplate` para mostrar una línea vertical entre cada vínculo.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Para rellenar este repetidor con las opciones de filtrado deseadas, cree un método denominado `BindFilteringUI` . Asegúrese de llamar a este método desde el `Page_Load` controlador de eventos en la primera carga de la página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Este método especifica las opciones de filtrado como elementos en la `String` matriz `filterOptions` para cada elemento de la matriz, el repetidor representará un LinkButton con sus `Text` `CommandName` propiedades y asignadas al valor del elemento de la matriz.

En la ilustración 5 se muestra la `ManageUsers.aspx` página cuando se ve a través de un explorador.

[![El repetidor muestra 27 filtros LinkButtons](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: el repetidor enumera 27 Filtering LinkButtons ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))

> [!NOTE]
> Los nombres de usuario pueden comenzar con cualquier carácter, incluidos números y signos de puntuación. Para ver estas cuentas, el administrador tendrá que usar la opción todos los LinkButton. Como alternativa, puede Agregar un LinkButton para devolver todas las cuentas de usuario que comienzan con un número. Dejo esto como un ejercicio para el lector.

Al hacer clic en cualquiera de los LinkButtons de filtrado, se produce un postback y se genera el evento del repetidor `ItemCommand` , pero no hay ningún cambio en la cuadrícula porque todavía hemos escrito cualquier código para filtrar los resultados. La `Membership` clase incluye un [ `FindUsersByName` método](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que devuelve las cuentas de usuario cuyo nombre de usuario coincide con un modelo de búsqueda especificado. Podemos utilizar este método para recuperar solo las cuentas de usuario cuyos nombres de usuario empiecen por la letra especificada por el `CommandName` del LinkButton filtrado en el que se hizo clic.

Empiece por actualizar la `ManageUser.aspx` clase de código subyacente de la página para que incluya una propiedad denominada `UsernameToMatch` esta propiedad conserve la cadena de filtro de nombre de usuario entre los postbacks:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

La `UsernameToMatch` propiedad almacena su valor se asigna a la `ViewState` colección mediante la clave ' usernameToMatch '. Cuando se lee el valor de esta propiedad, comprueba si existe un valor en la `ViewState` colección; de lo contrario, devuelve el valor predeterminado, una cadena vacía. La `UsernameToMatch` propiedad exhibe un patrón común, y, a su vez, conserva un valor en el estado de vista para que los cambios en la propiedad se conserven en los postbacks. Para obtener más información sobre este patrón, lea [Descripción del estado de vista de ASP.net](https://msdn.microsoftn-us/library/ms972976.aspx).

A continuación, actualice el `BindUserAccounts` método para que, en lugar de llamar a `Membership.GetAllUsers` , llame a `Membership.FindUsersByName` , pasando el valor de la `UsernameToMatch` propiedad anexado con el carácter comodín de SQL,%.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Para mostrar solo los usuarios cuyo nombre de usuario comienza por la letra A, establezca la `UsernameToMatch` propiedad en y, a continuación, llame a `BindUserAccounts` , lo que dará como resultado una llamada a `Membership.FindUsersByName("A%")` , que devolverá todos los usuarios cuyo nombre de usuario comienza con. del mismo modo, para devolver *todos* los usuarios, asigne una cadena vacía a la `UsernameToMatch` propiedad para que el `BindUserAccounts` método lo invoque `Membership.FindUsersByName("%")` , devolviendo todas las cuentas

Cree un controlador de eventos para el evento del repetidor `ItemCommand` . Este evento se desencadena cuando se hace clic en uno de los LinkButtons de filtro. se pasa el valor de LinkButton en el que se hace clic `CommandName` a través del `RepeaterCommandEventArgs` objeto. Necesitamos asignar el valor adecuado a la `UsernameToMatch` propiedad y, a continuación, llamar al `BindUserAccounts` método. Si `CommandName` es All, asigne una cadena vacía a para `UsernameToMatch` que se muestren todas las cuentas de usuario. De lo contrario, asigne el `CommandName` valor a `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Con este código en su lugar, pruebe la funcionalidad de filtrado. Cuando se visite la página por primera vez, se mostrarán todas las cuentas de usuario (consulte la figura 5). Al hacer clic en un LinkButton, se genera un postback y se filtran los resultados, mostrando solo las cuentas de usuario que comienzan con.

[![Use el LinkButtons de filtrado para mostrar los usuarios cuyo nombre de usuario comienza por una letra determinada](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: uso del LinkButtons de filtrado para mostrar los usuarios cuyo nombre de usuario comienza por una letra determinada ([haga clic para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Paso 4: actualizar GridView para usar la paginación

El control GridView mostrado en las figuras 5 y 6 enumera todos los registros devueltos por el `FindUsersByName` método. Si hay cientos o miles de cuentas de usuario, esto puede provocar una sobrecarga de la información al ver todas las cuentas (como ocurre al hacer clic en todos los LinkButton o al visitar inicialmente la página). Para ayudar a presentar las cuentas de usuario en fragmentos más fáciles de administrar, vamos a configurar GridView para mostrar 10 cuentas de usuario a la vez.

El control GridView ofrece dos tipos de paginación:

- **Paginación predeterminada** : fácil de implementar, pero ineficaz. En pocas palabras, con la paginación predeterminada, GridView espera *todos* los registros de su origen de datos. A continuación, solo se muestra la página de registros adecuada.
- **Paginación personalizada** : requiere más trabajo para implementar, pero es más eficaz que la paginación predeterminada, ya que con la paginación personalizada el origen de datos solo devuelve el conjunto preciso de registros que se van a mostrar.

La diferencia de rendimiento entre la paginación predeterminada y la paginación personalizada puede ser bastante importante al paginar a través de miles de registros. Dado que estamos creando esta interfaz suponiendo que puede haber cientos o miles de cuentas de usuario, vamos a usar la paginación personalizada.

> [!NOTE]
> Para obtener una explicación más detallada sobre las diferencias entre la paginación predeterminada y la paginación personalizada, así como los desafíos que conlleva la implementación de la paginación personalizada, consulte [paginación eficaz a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Para ver algún análisis de la diferencia de rendimiento entre la paginación predeterminada y la paginación personalizada, consulte [paginación personalizada en ASP.net con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Para implementar la paginación personalizada, primero necesitamos algún mecanismo por el que recuperar el subconjunto preciso de los registros que se muestran en GridView. La buena noticia es que el `Membership` método de la clase `FindUsersByName` tiene una sobrecarga que nos permite especificar el índice de página y el tamaño de página, y solo devuelve las cuentas de usuario que se encuentran dentro de ese intervalo de registros.

En concreto, esta sobrecarga tiene la firma siguiente: [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx) .

El parámetro *pageIndex* especifica la página de cuentas de usuario que se va a devolver; *pageSize* indica el número de registros que se van a mostrar por página. El parámetro *totalRecords* es un `ByRef` parámetro que devuelve el número de cuentas de usuario totales en el almacén de usuario.

> [!NOTE]
> Los datos devueltos por `FindUsersByName` se ordenan por nombre de usuario; los criterios de ordenación no se pueden personalizar.

GridView puede configurarse para utilizar la paginación personalizada, pero solo cuando se enlaza a un control ObjectDataSource. Para que el control ObjectDataSource implemente la paginación personalizada, requiere dos métodos: uno que se pasa a un índice de fila inicial y el número máximo de registros que se van a mostrar, y devuelve el subconjunto preciso de registros que se encuentran dentro de ese intervalo; y un método que devuelve el número total de registros que se van a paginar. La `FindUsersByName` sobrecarga acepta un índice de página y un tamaño de página, y devuelve el número total de registros a través de un `ByRef` parámetro. Por lo tanto, aquí no coinciden las interfaces.

Una opción sería crear una clase de proxy que exponga la interfaz que ObjectDataSource espera y, a continuación, llamar internamente al `FindUsersByName` método. Otra opción, y la que usaremos en este artículo, es crear nuestra propia interfaz de paginación y usarla en lugar de la interfaz de paginación integrada de GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Crear una interfaz de paginación primera, anterior, siguiente y última

Vamos a crear una interfaz de paginación con los primeros, anteriores, siguiente y último LinkButtons. Al hacer clic en el primer LinkButton, se llevará al usuario a la primera página de datos, mientras que el anterior lo devolverá a la página anterior. Del mismo modo, el siguiente y el último moverán al usuario a la página siguiente y última, respectivamente. Agregue los cuatro controles LinkButton debajo de `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

A continuación, cree un controlador de eventos para cada uno de los eventos de LinkButton `Click` .

En la figura 7 se muestran los cuatro LinkButtons cuando se ven a través de Visual Web Developer Vista de diseño.

[![Agregar primero, anterior, siguiente y último LinkButtons debajo de GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: agregar primero, anterior, siguiente y último LinkButtons debajo de GridView ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Seguimiento del índice de la página actual

Cuando un usuario visita por primera vez la `ManageUsers.aspx` página o hace clic en uno de los botones de filtrado, queremos mostrar la primera página de datos en el control GridView. Sin embargo, cuando el usuario hace clic en uno de los LinkButtons de navegación, es necesario actualizar el índice de la página. Para mantener el índice de la página y el número de registros que se van a mostrar por página, agregue las dos propiedades siguientes a la clase de código subyacente de la página:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Al igual que la `UsernameToMatch` propiedad, la `PageIndex` propiedad conserva su valor en el estado de vista. La propiedad de solo lectura `PageSize` devuelve un valor codificado de forma rígida, 10. Deseo invitar al lector interesado a actualizar esta propiedad para que use el mismo patrón que `PageIndex` y, a continuación, aumentar la `ManageUsers.aspx` Página para que la persona que visita la página pueda especificar el número de cuentas de usuario que se mostrarán por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperar solo los registros de la página actual, actualizar el índice de la página y habilitar y deshabilitar la interfaz de paginación LinkButtons

Con la interfaz de paginación en contexto y las `PageIndex` `PageSize` propiedades y agregadas, estamos preparados para actualizar el `BindUserAccounts` método de modo que use la `FindUsersByName` sobrecarga adecuada. Además, es necesario que este método habilite o deshabilite la interfaz de paginación en función de la página que se muestre. Al ver la primera página de datos, se deben deshabilitar los vínculos primero y anterior; El siguiente y el último deben deshabilitarse al ver la última página.

Actualice el método `BindUserAccounts` con el código siguiente:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Tenga en cuenta que el número total de registros que se van a paginar viene determinado por el último parámetro del `FindUsersByName` método. Una vez que se devuelve la página especificada de cuentas de usuario, los cuatro LinkButtons se habilitan o deshabilitan, dependiendo de si se está viendo la primera o la última página de datos.

El último paso es escribir el código para los cuatro controladores de eventos de LinkButtons `Click` . Estos controladores de eventos deben actualizar la `PageIndex` propiedad y volver a enlazar los datos a GridView a través de una llamada a `BindUserAccounts` los controladores de eventos primero, anterior y siguiente son muy simples. `Click`Sin embargo, el controlador de eventos para el último LinkButton es un poco más complejo porque es necesario determinar el número de registros que se muestran para determinar el índice de la última página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

En las figuras 8 y 9 se muestra la interfaz de paginación personalizada en acción. En la ilustración 8 se muestra la `ManageUsers.aspx` página al ver la primera página de datos de todas las cuentas de usuario. Tenga en cuenta que solo se muestran 10 de las 13 cuentas. Al hacer clic en el vínculo siguiente o último, se genera un postback, se actualiza el `PageIndex` a 1 y se enlaza la segunda página de las cuentas de usuario a la cuadrícula (vea la ilustración 9).

[![Se muestran las 10 primeras cuentas de usuario](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: se muestran las 10 primeras cuentas de usuario ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))

[![Al hacer clic en el vínculo siguiente, se muestra la segunda página de cuentas de usuario](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: hacer clic en el vínculo siguiente muestra la segunda página de cuentas de usuario ([haga clic para ver la imagen de tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))

## <a name="summary"></a>Resumen

A menudo, los administradores necesitan seleccionar un usuario de la lista de cuentas. En los tutoriales anteriores, analizamos el uso de una lista desplegable que se rellenó con los usuarios, pero este enfoque no se escala bien. En este tutorial se ha explorado una alternativa mejor: una interfaz filtrable cuyos resultados se muestran en un control GridView paginado. Con esta interfaz de usuario, los administradores pueden localizar y seleccionar de forma rápida y eficaz una cuenta de usuario entre miles.

¡ Feliz programación!

### <a name="further-reading"></a>Más información

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Paginación personalizada en ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paginación eficaz a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Implementación de su propia herramienta de administración de sitios web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/) .

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Alicja Maziarz. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, suéltelo una línea en

> [!div class="step-by-step"]
> [Anterior](unlocking-and-approving-user-accounts-cs.md)
> [Siguiente](recovering-and-changing-passwords-vb.md)
