---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor ? Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 mejora los controles del servidor de muchas maneras. En este módulo, cubriremos algunos de los cambios arquitectónicos en la forma ASP.NET 2.0 y Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 7109f10e87abfadf1e7e08795cf9d3d6bf5df122
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543748"
---
# <a name="server-controls"></a>Controles de servidor

por [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 mejora los controles del servidor de muchas maneras. En este módulo, cubriremos algunos de los cambios arquitectónicos en la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupan de los controles de servidor.

ASP.NET 2.0 mejora los controles del servidor de muchas maneras. En este módulo, cubriremos algunos de los cambios arquitectónicos en la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupan de los controles de servidor.

## <a name="view-state"></a>Ver estado

El cambio principal en el estado de vista en ASP.NET 2.0 es una reducción drástica en el tamaño. Considere una página con solo un control Calendar. Aquí está el estado de vista en ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Aquí está el estado de vista en una página idéntica en ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Eso es un cambio bastante significativo, y teniendo en cuenta que el estado de vista se lleva de un lado a otro a través del cable, este cambio puede dar a los desarrolladores un aumento significativo del rendimiento. La reducción en el tamaño del estado de vista se debe en gran medida a la forma en que lo manejamos internamente. Recuerde que el estado de vista es una cadena codificada en Base64. Para comprender mejor el cambio en el estado de vista en ASP.NET 2.0, echemos un vistazo a los valores descodificados de los ejemplos anteriores.

Aquí está el estado de la vista 1.1 decodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Esto puede parecer un poco un galimatías, pero hay un patrón aquí. En ASP.NET 1.x, usamos caracteres únicos para identificar &lt; &gt; tipos de datos y valores delimitados mediante los caracteres. La "t" en el ejemplo de estado de vista anterior representa un triplete. El triplete contiene un par de ArrayLists (la "l" representa un ArrayList.) Uno de esos ArrayLists contiene un Int32 ("i") con un valor de 1 y el otro contiene otro Triplet. El triplete contiene un par de ArrayLists, etc. Lo importante es recordar que usamos trillizos que contienen pares, identificamos los &lt; tipos &gt; de datos a través de una letra y usamos los caracteres y como delimitadores.

En ASP.NET 2.0, el estado de vista descodificado se ve un poco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Debe notar un gran cambio en la apariencia del estado de vista descodificado. Este cambio tiene varios fundamentos arquitectónicos. El estado de vista en ASP.NET 1.x utilizó ElFormatter para serializar datos. En 2.0, usamos la nueva clase ObjectStateFormatter. Esta clase se diseñó específicamente para ayudar en la serialización y deserialización del estado de vista y el estado de control. (El estado de control se cubrirá en la siguiente sección.) Hay muchos beneficios obtenidos al cambiar el método por el que se lleva a cabo la serialización y la deserialización. Uno de los más dramáticos es el hecho de que a diferencia de LosFormatter que utiliza un TextWriter, el ObjectStateFormatter utiliza un BinaryWriter. Esto permite que ASP.NET 2.0 almacene el estado de vista una serie de bytes en lugar de cadenas. Tomemos, por ejemplo, un entero. En ASP.NET 1.1, un entero requería 4 bytes de estado de vista. En ASP.NET 2.0, ese mismo entero solo requiere 1 byte. Se realizaron otras mejoras para reducir la cantidad de estado de vista que se almacena. Los valores De FechaY, por ejemplo, ahora se almacenan mediante un TickCount en lugar de una cadena.

Como si todo eso no fuera suficiente, se prestó especial atención al hecho de que uno de los mayores consumidores de estado de vista en 1.x era el DataGrid y controles similares. Un inconveniente importante de controles como el DataGrid donde el estado de vista está preocupado es que a menudo contiene grandes cantidades de información repetida. En ASP.NET 1.x, esa información repetida simplemente se almacenaba una y otra vez, lo que resultaba en un estado de vista hinchado. En ASP.NET 2.0, usamos la nueva clase IndexedString para almacenar estos datos. Si se repite una cadena, solo almacenamos el token para el IndexedString y el índice dentro de una tabla en ejecución de IndexedString objetos.

## <a name="control-state"></a>Estado de control

Una de las principales quejas que los desarrolladores tenían con el estado de vista era el tamaño que agregaba a la carga HTTP. Como se mencionó anteriormente, uno de los mayores consumidores de estado de vista es el DataGrid control. Para evitar las enormes cantidades de estado de vista generadas por un DataGrid, muchos desarrolladores simplemente deshabilitaron el estado de vista para ese control. Desafortunadamente, esa solución no siempre fue buena. El estado de vista en ASP.NET 1.x no solo contiene los datos necesarios para la funcionalidad correcta del control. También contiene información sobre el estado de la interfaz de usuario del control. Esto significa que si desea permitir la paginación en un DataGrid, debe habilitar el estado de vista incluso si no necesita toda la información de la interfaz de usuario que contiene el estado de vista. Es un escenario de todo o nada.

En ASP.NET 2.0, el estado de control resuelve ese problema muy bien a través de la introducción del estado de control. El estado de control contiene los datos que son absolutamente necesarios para la funcionalidad adecuada de un control. A diferencia del estado de vista, el estado de control no se puede deshabilitar. Por lo tanto, es importante que los datos que se almacenan en estado de control se controlen cuidadosamente.

> [!NOTE]
> El estado de control se conserva \_ \_junto con el estado de vista en el campo formulario oculto VIEWSTATE.

Este vídeo es un tutorial del estado de vista y el estado de control.

![](server-controls/_static/image1.png)

[Abrir vídeo a pantalla completa](server-controls/_static/state1.wmv)

Para que un control de servidor lea y escriba en el estado de control, debe realizar tres pasos.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Paso 1: Llame al método RegisterRequiresControlState

El RegisterRequiresControlState método informa a ASP.NET que un control debe conservar el estado de control. Toma un argumento de tipo Control que es el control que se está registrando.

Es importante tener en cuenta que el registro no persiste de la solicitud a la solicitud. Por lo tanto, se debe llamar a este método en cada solicitud si un control debe conservar el estado de control. Se recomienda llamar al método en OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Paso 2: Invalidar SaveControlState

El SaveControlState método guarda los cambios de estado de control para un control desde la última devolución de la publicación. Devuelve un objeto que representa el estado del control.

## <a name="step-3-override-loadcontrolstate"></a>Paso 3: Reemplazar LoadControlState

El LoadControlState método carga el estado guardado en un control. El método toma un argumento de tipo Object que contiene el estado guardado para el control.

## <a name="full-xhtml-compliance"></a>Cumplimiento completo de XHTML

Cualquier desarrollador web conoce la importancia de los estándares en las aplicaciones web. Para mantener un entorno de desarrollo basado en estándares, ASP.NET 2.0 es totalmente compatible con XHTML. Por lo tanto, todas las etiquetas se representan de acuerdo con los estándares XHTML en navegadores que admiten HTML 4.0 o superior.

La definición de DOCTYPE en ASP.NET 1.1 era la siguiente:

[!code-html[Main](server-controls/samples/sample6.html)]

En ASP.NET 2.0, la definición DOCTYPE predeterminada es la siguiente:

[!code-html[Main](server-controls/samples/sample7.html)]

Si lo desea, puede modificar el cumplimiento XHTML predeterminado a través del nodo xhtmlConformance en el archivo de configuración. Por ejemplo, el siguiente nodo del archivo web.config cambiará el cumplimiento de XHTML a XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si lo desea, también puede configurar ASP.NET para utilizar la configuración heredada utilizada en ASP.NET 1.x de la siguiente manera:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Renderizado adaptativo mediante adaptadores

En ASP.NET 1.x, el archivo &lt;de&gt; configuración contenía una sección browserCaps que rellenaba un objeto HttpBrowserCapabilities. Este objeto permitía a un desarrollador determinar qué dispositivo realiza una solicitud determinada y representar el código de forma adecuada. En ASP.NET 2.0, el modelo ha mejorado y ahora usa la nueva clase ControlAdapter. La clase ControlAdapter reemplaza los eventos en el ciclo de vida del control y controla la representación de controles en función de las capacidades del agente de usuario. Las capacidades de un agente de usuario específico se definen mediante un archivo de definición de explorador (un archivo con una extensión de archivo .browser) almacenado en el archivo c:-windows-microsoft.net-framework-v2.0. \* \*Carpeta \*de los navegadores \*DE CONFIG.

> [!NOTE]
> La clase ControlAdapter es una clase abstracta.

Al igual &lt;que&gt; la sección browserCaps en 1.x, el archivo de definición del navegador utiliza una expresión regular para analizar la cadena del agente de usuario con el fin de identificar el navegador solicitante. Define capacidades particulares para ese agente de usuario. El ControlAdapter representa el control a través de la Render método. Por lo tanto, si invalida el render método, no debe llamar a Render en la clase base. Si lo hace, puede provocar que la representación se produzca dos veces, una para el adaptador y otra para el propio control.

## <a name="developing-a-custom-adapter"></a>Desarrollo de un adaptador personalizado

Puede desarrollar su propio adaptador personalizado heredando de ControlAdapter. Además, puede heredar de la clase abstracta PageAdapter en los casos en que se necesita un adaptador para una página. La asignación de controles al &lt;adaptador personalizado&gt; se realiza a través del elemento controlAdapters en el archivo de definición del explorador. Por ejemplo, el siguiente XML de un archivo de definición de explorador asigna el control Menu a la clase MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Usando este modelo, se vuelve bastante fácil para un desarrollador de control apuntar a un dispositivo o navegador en particular. También es bastante sencillo para un desarrollador tener un control completo sobre cómo se representan las páginas en cada dispositivo.

## <a name="per-device-rendering"></a>Representación por dispositivo

Las propiedades de control de servidor en ASP.NET 2.0 se pueden especificar por dispositivo mediante un prefijo específico del explorador. Por ejemplo, el código siguiente cambiará el texto de una etiqueta dependiendo del dispositivo que se utilice para navegar por la página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Cuando la página que contiene esta etiqueta se navega desde Internet Explorer, la etiqueta mostrará texto que dice "Está navegando desde Internet Explorer." Cuando la página se navega desde Firefox, la etiqueta mostrará el texto "Estás navegando desde Firefox." Cuando la página se navega desde cualquier otro dispositivo, se mostrará "Está navegando desde un dispositivo desconocido." Cualquier propiedad se puede especificar mediante esta sintaxis especial.

## <a name="setting-focus"></a>Ajuste del enfoque

ASP.NET desarrolladores de 1.x con frecuencia preguntaban sobre cómo establecer el foco inicial en un control determinado. Por ejemplo, en una página de inicio de sesión, es útil que el cuadro de texto ID de usuario obtenga el foco cuando se cargue la página por primera vez. En ASP.NET 1.x, hacer esto requería escribir algún script del lado cliente. Aunque un script de este tipo es una tarea trivial, ya no es necesario en ASP.NET 2.0 gracias al método SetFocus. El SetFocus método toma un argumento que indica el control que debe recibir el foco. Este argumento puede ser el identificador de cliente del control como una cadena o el nombre del control Server como un objeto Control. Por ejemplo, para establecer el foco inicial en un control TextBox denominado txtUserID\_cuando se carga la página por primera vez, agregue el código siguiente a Carga de página:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-- o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 usa el controlador Webresource.axd (discutido anteriormente) para representar una función del lado cliente que establece el foco. El nombre de la función\_del lado cliente es WebForm AutoFocus como se muestra aquí:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, puede usar el método Focus para un control para establecer el foco inicial en ese control. El Focus método deriva de la Control clase y está disponible para todos los ASP.NET 2.0 controles. También es posible establecer el foco en un control determinado cuando se produce un error de validación. Eso se tratará en un módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Nuevos controles de servidor en ASP.NET 2.0

Los siguientes son nuevos controles de servidor en ASP.NET 2.0. Vamos a entrar en más detalles sobre algunos de ellos en módulos posteriores.

## <a name="imagemap-control"></a>ImageMap Control

El ImageMap control le permite agregar puntos de acceso a una imagen que puede iniciar una publicación de nuevo o navegar a una dirección URL. Hay tres tipos de puntos de acceso disponibles; CircleHotSpot, RectangleHotSpot y PolygonHotSpot. Los puntos de acceso se agregan a través de un editor de colecciones en Visual Studio o mediante programación en el código. No hay ninguna interfaz de usuario disponible para dibujar puntos de acceso en una imagen. Las coordenadas y el tamaño o el radio del punto de acceso deben especificarse mediante declaración. Tampoco hay representación visual de un punto de acceso en el diseñador. Si un punto de acceso está configurado para navegar a una dirección URL, la dirección URL se especifica a través de la NavigateUrl propiedad del punto de acceso. En el caso de un punto de acceso de devolución de correo, la propiedad PostBackValue permite pasar una cadena en la devolución de correos que se puede recuperar en el código del lado servidor.

![Editor de colecciones HotSpot en Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor de colecciones HotSpot en Visual Studio

## <a name="bulletedlist-control"></a>BulletedList Control

El BulletedList control es una lista con viñetas que se puede enlazar fácilmente a datos. La lista se puede ordenar (numerar) o desordenar a través de la BulletStyle propiedad. Cada elemento de la lista se representa mediante un ListItem objeto.

![Control BulletedList en Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: BulletedList Control en Visual Studio

## <a name="hiddenfield-control"></a>HiddenField Control

El HiddenField control agrega un campo de formulario oculto a la página, cuyo valor está disponible en el código del lado servidor. Por lo general, se espera que el valor de un campo de formulario oculto permanezca sin cambios entre las devoluciones de correos. Sin embargo, es posible que un usuario malintencionado cambie el valor antes de devolver la publicación. Si esto sucede, el HiddenField control generará el ValueChanged eventos. Si tiene información confidencial en el HiddenField control y desea asegurarse de que permanece sin cambios, debe controlar el ValueChanged eventos en el código.

## <a name="fileupload-control"></a>FileUpload Control

El control FileUpload de ASP.NET 2.0 permite cargar archivos en un servidor Web a través de una página ASP.NET. Este control es bastante similar a la ASP.NET 1.x HtmlInputFile clase con algunas excepciones. En ASP.NET 1.x, se recomendó que la propiedad PostedFile se comprobara como null para determinar si tenía un buen archivo. El control FileUpload de ASP.NET 2.0 agrega una nueva propiedad HasFile que puede usar para el mismo propósito y es un poco más eficaz.

El PostedFile propiedad todavía está disponible para el acceso a un HttpPostedFile objeto, pero parte de la funcionalidad de la HttpPostedFile ahora está disponible intrínsecamente con el FileUpload control. Por ejemplo, para guardar un archivo cargado en ASP.NET 1.x, llame al método SaveAs en el objeto HttpPostedFile. Mediante el control FileUpload en ASP.NET 2.0, llamaría al método SaveAs en el propio control FileUpload.

Otro cambio significativo en el comportamiento 2.0 (y probablemente el cambio más significativo) es que ya no es necesario cargar un archivo cargado completo en la memoria antes de guardarlo. En 1.x, cualquier archivo que se cargó se guarda por completo en la memoria antes de ser escrito en el disco. Esta arquitectura impide la carga de archivos de gran tamaño.

En ASP.NET 2.0, el atributo requestLengthDiskThreshold del elemento httpRuntime permite configurar cuántos Kilobytes se mantienen en un búfer en memoria antes de escribirse en el disco.

**IMPORTANTE:** La documentación de MSDN (y la documentación en otro lugar) especifica que este valor está en bytes (no Kilobytes) y que el valor predeterminado es 256. El valor se especifica realmente en Kilobytes y el valor predeterminado es 80. Al tener un valor predeterminado de 80K, nos aseguramos de que el búfer no termine en el montón de objetos grandes.

## <a name="wizard-control"></a>Control de asistente

Es bastante común encontrarse con ASP.NET desarrolladores que luchan con el intento de recopilar información en una serie de "páginas" utilizando paneles o mediante la transferencia de página en página. Más a menudo que no, el esfuerzo es frustrante y consume mucho tiempo. El nuevo control Wizard resuelve los problemas al permitir pasos lineales y no lineales en una interfaz de asistente con la que los usuarios están familiarizados. El Wizard control presenta formularios de entrada en una serie de pasos. Cada paso es de un tipo determinado especificado por el StepType propiedad del control. Los tipos de pasos disponibles son los siguientes:

| **Tipo de paso** | **Explicación** |
| --- | --- |
| Automático | El asistente determina automáticamente el tipo de paso en función de su posición dentro de la jerarquía de pasos. |
| Start | El primer paso, a menudo utilizado para presentar una declaración introductoria. |
| Paso | Un paso normal. |
| Finalizar | El paso final, que normalmente se usa para presentar un botón para finalizar el asistente. |
| Operación completada | Presenta un mensaje que comunica el éxito o el error. |

> [!NOTE]
> El Wizard control realiza un seguimiento de su estado mediante ASP.NET estado de control. Por lo tanto, el EnableViewState propiedad se puede establecer en false sin ningún perjuicio.

Este vídeo es un tutorial del control Wizard.

![](server-controls/_static/image2.png)

[Abrir vídeo a pantalla completa](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localizar control

El control Localize es similar a un control Literal. Sin embargo, el control Localize tiene una propiedad **Mode** que controla cómo se representa el marcado que se agrega a él. La propiedad Mode admite los siguientes valores:

| **Modo** | **Explicación** |
| --- | --- |
| Transformación | El marcado se transforma de acuerdo con el protocolo del navegador que realiza la solicitud. |
| Passthrough | El marcado se representa tal como está. |
| Codificación | El marcado que se agrega al control se codifica mediante HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Controles MultiView y View

El control MultiView actúa como un contenedor para View controles y el View control actúa como un contenedor (muy parecido a un Panel control) para otros controles. Cada vista en un Control MultiView se representa mediante un único control View. El primer control View en el MultiView es view 0, el segundo es view 1, etc. Puede cambiar las vistas especificando el ActiveViewIndex de la MultiView control.

## <a name="substitution-control"></a>Control de sustitución

El control Substitution se utiliza junto con ASP.NET almacenamiento en caché. En los casos en los que desea aprovechar el almacenamiento en caché, pero tiene partes de una página que se deben actualizar en cada solicitud (en otras palabras, partes de una página que están exentas del almacenamiento en caché), el componente substitution proporciona una gran solución. El control no representa realmente ninguna salida por sí solo. En su lugar, está enlazado a un método en el código del lado servidor. Cuando se solicita la página, se llama al método y el marcado devuelto se representa en lugar del control de sustitución.

El método al que se enlaza el control Substitution se especifica a través de la **propiedad MethodName.** Este método debe cumplir los siguientes criterios:

- Debe ser un método estático (compartido en VB).
- Acepta un parámetro de tipo HttpContext.
- Devuelve una cadena que representa el marcado que debe reemplazar el control en la página.

El control Substitution no tiene la capacidad de modificar ningún otro control de la página, pero tiene acceso a httpcontext actual a través de su parámetro.

## <a name="gridview-control"></a>GridView Control

El control GridView es el reemplazo del control DataGrid. Este control se tratará con más detalle en un módulo posterior.

## <a name="detailsview-control"></a>DetailsView Control

El DetailsView control le permite mostrar un único registro de un origen de datos y editarlo o eliminarlo. Se trata con más detalle en un módulo posterior.

## <a name="formview-control"></a>FormView Control

El FormView control se utiliza para mostrar un único registro de un origen de datos en una interfaz configurable. Se trata con más detalle en un módulo posterior.

## <a name="accessdatasource-control"></a>AccessDataSource Control

El AccessDataSource control se utiliza para enlazar datos de una base de datos de Access. Se trata con más detalle en un módulo posterior.

## <a name="objectdatasource-control"></a>ObjectDataSource Control

El ObjectDataSource control se usa para admitir una arquitectura de tres niveles para que los controles se pueden enlazar a datos a un objeto de negocio de nivel intermedio en lugar de un modelo de dos niveles donde los controles se enlazan directamente al origen de datos. Se discutirá con más detalle en un módulo posterior.

## <a name="xmldatasource-control"></a>XmlDataSource Control

El XmlDataSource control se utiliza para enlazar datos a un origen de datos XML. Se trata con más detalle en un módulo posterior.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource Control

El SiteMapDataSource control proporciona enlace de datos para los controles de navegación del sitio en función de un mapa del sitio. Se discutirá con más detalle en un módulo posterior.

## <a name="sitemappath-control"></a>SiteMapPath Control

El SiteMapPath control muestra una serie de vínculos de navegación comúnmente denominados rutas de navegación. Se trata con más detalle en un módulo posterior.

## <a name="menu-control"></a>Control Menu

El control Menú muestra menús dinámicos mediante DHTML. Se trata con más detalle en un módulo posterior.

## <a name="treeview-control"></a>TreeView (Control)

El TreeView control se utiliza para mostrar una vista jerárquica de árbol de datos. Se trata con más detalle en un módulo posterior.

## <a name="login-control"></a>Control de inicio de sesión

El control login proporciona un mecanismo para iniciar sesión en un sitio Web. Se trata con más detalle en un módulo posterior.

## <a name="loginview-control"></a>Control LoginView

El control LoginView permite la visualización de diferentes plantillas en función del estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="passwordrecovery-control"></a>PasswordRecovery Control

El control PasswordRecovery se utiliza para recuperar contraseñas olvidadas por los usuarios de una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="loginstatus"></a>LoginStatus

El control LoginStatus muestra el estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="loginname"></a>LoginName

El control LoginName muestra el nombre de usuario de un usuario después de iniciar sesión en una aplicación de ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="createuserwizard"></a>Createuserwizard

El CreateUserWizard es un asistente configurable que ofrece a los usuarios la capacidad de crear una cuenta de pertenencia ASP.NET para su uso en una aplicación de ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="changepassword"></a>ChangePassword

El control ChangePassword permite a los usuarios cambiar su contraseña para una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="various-webparts"></a>Varias WebParts

ASP.NET 2.0 se suministra con varios elementos web. Estos se tratarán en detalle en un módulo posterior.
