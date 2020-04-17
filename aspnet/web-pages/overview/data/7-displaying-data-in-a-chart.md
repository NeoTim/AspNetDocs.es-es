---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Visualización de datos en un gráfico con ASP.NET páginas web (Razor) Microsoft Docs
author: rick-anderson
description: En este capítulo se explica cómo mostrar datos en un gráfico. En los capítulos anteriores, aprendió a mostrar los datos manualmente y en una cuadrícula. Este capítulo explica...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: ad55d4898ee39e0239ef8b0bd5eea6387af3c816
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542890"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Visualización de datos en un gráfico con páginas Web ASP.NET (Razor)

por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar un gráfico para mostrar datos `Chart` en un sitio web de ASP.NET Web Pages (Razor) mediante la aplicación auxiliar.
> 
> **Lo que aprenderás**:
> 
> - Cómo mostrar datos en un gráfico.
> - Cómo aplicar estilo a los gráficos utilizando temas integrados.
> - Cómo guardar gráficos y cómo almacenarlos en caché para mejorar el rendimiento.
> 
> Estas son las características de programación ASP.NET introducidas en el artículo:
> 
> - El `Chart` ayudante.
> 
> > [!NOTE]
> > La información de este artículo se aplica a ASP.NET páginas Web 1.0 y páginas Web 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>The Chart Helper

Cuando desee mostrar los datos en forma `Chart` gráfica, puede utilizar la aplicación auxiliar. La `Chart` aplicación auxiliar puede representar una imagen que muestra datos en una variedad de tipos de gráfico. Es compatible con muchas opciones para formatear y etiquetar. La `Chart` aplicación auxiliar puede representar más de 30 tipos de gráficos, incluidos todos los tipos de gráficos con los que podría estar familiarizado desde Microsoft Excel u otras herramientas &#8212; gráficos de área, gráficos de barras, gráficos de columnas, gráficos de líneas y gráficos circulares, junto con gráficos más especializados como gráficos de acciones.

| Descripción del **gráfico de área:** ![Imagen del tipo de gráfico de área](7-displaying-data-in-a-chart/_static/image1.jpg) | Descripción del **gráfico** ![de barras: Imagen del tipo de gráfico de barras](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| Descripción **del gráfico** ![de columnas: Imagen del tipo de gráfico de columnas](7-displaying-data-in-a-chart/_static/image3.jpg) | Descripción del **gráfico** ![de líneas: Imagen del tipo de gráfico de líneas](7-displaying-data-in-a-chart/_static/image4.jpg) |
| Descripción **del gráfico** ![circular: Imagen del tipo de gráfico circular](7-displaying-data-in-a-chart/_static/image5.jpg) | **Descripción del gráfico** ![de acciones: Imagen del tipo de gráfico de acciones](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elemento de gráfico

Los gráficos muestran datos y elementos adicionales como leyendas, ejes, series, etc. La siguiente imagen muestra muchos de los elementos `Chart` del gráfico que puede personalizar cuando se utiliza la aplicación auxiliar. En este artículo se muestra cómo establecer algunos (no todos) de estos elementos.

![Descripción: Imagen que muestra los elementos del gráfico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Creación de un gráfico a partir de datos

Los datos que se muestran en un gráfico pueden ser de una matriz, de los resultados devueltos desde una base de datos o de datos que se encuentra en un archivo XML.

### <a name="using-an-array"></a>Uso de una matriz

Como se explica en [Introducción a ASP.NET programación de páginas Web mediante la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202890)de Razor , una matriz le permite almacenar una colección de elementos similares en una sola variable. Puede utilizar matrices para contener los datos que desea incluir en el gráfico.

Este procedimiento muestra cómo puede crear un gráfico a partir de datos en matrices, utilizando el tipo de gráfico predeterminado. También muestra cómo mostrar el gráfico dentro de la página.

1. Cree un nuevo archivo denominado *ChartArrayBasic.cshtml*.
2. Reemplace el contenido existente por el siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    El código crea primero un nuevo gráfico y establece su ancho y alto. El título del gráfico `AddTitle` se especifica mediante el método. Para agregar datos, `AddSeries` utilice el método. En este ejemplo, `name`se `xValue`utilizan `yValues` los `AddSeries` parámetros , , y del método. El `name` parámetro se muestra en la leyenda del gráfico. El `xValue` parámetro contiene una matriz de datos que se muestra a lo largo del eje horizontal del gráfico. El `yValues` parámetro contiene una matriz de datos que se utiliza para trazar los puntos verticales del gráfico.

    El `Write` método representa realmente el gráfico. En este caso, dado que no especificó un tipo de gráfico, la `Chart` aplicación auxiliar representa su gráfico predeterminado, que es un gráfico de columnas.
3. Ejecute la página en el explorador. El navegador muestra el gráfico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Uso de una consulta de base de datos para datos de gráficos

Si la información que desea trazar está en una base de datos, puede ejecutar una consulta de base de datos y, a continuación, usar datos de los resultados para crear el gráfico. Este procedimiento muestra cómo leer y mostrar los datos de la base de datos creada en el artículo [Introducción al trabajo con una base](https://go.microsoft.com/fwlink/?LinkId=202893)de datos en ASP.NET sitios de páginas Web .

1. Agregue una carpeta Datos de la *aplicación\_* a la raíz del sitio web si la carpeta aún no existe.
2. En la carpeta *\_Datos* de la aplicación, agregue el archivo de base de datos denominado *SmallBakery.sdf* que se describe en [Introducción al trabajo con una base](https://go.microsoft.com/fwlink/?LinkId=202893)de datos en ASP.NET sitios de páginas web .
3. Cree un nuevo archivo denominado *ChartDataQuery.cshtml*.
4. Reemplace el contenido existente por el siguiente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    El código abre primero la base de datos `db`SmallBakery y la asigna a una variable denominada . Esta variable representa `Database` un objeto que se puede utilizar para leer y escribir en la base de datos. A continuación, el código ejecuta una consulta SQL para obtener el nombre y el precio de cada producto. El código crea un nuevo gráfico y le pasa la `DataBindTable` consulta de base de datos llamando al método del gráfico. Este método toma dos `dataSource` parámetros: el parámetro es `xField` para los datos de la consulta y el parámetro le permite establecer qué columna de datos se utiliza para el eje X del gráfico.

    Como alternativa al `DataBindTable` uso del método, `AddSeries` puede `Chart` usar el método de la aplicación auxiliar. El `AddSeries` método le `xValue` permite `yValues` establecer los parámetros y. Por ejemplo, en `DataBindTable` lugar de usar el método de la siguiente manera:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Puede utilizar `AddSeries` el método de la siguiente manera:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Ambos representan los mismos resultados. El `AddSeries` método es más flexible porque puede especificar el tipo `DataBindTable` de gráfico y los datos de forma más explícita, pero el método es más fácil de usar si no necesita la flexibilidad adicional.
5. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Usar datos XML

La tercera opción para el gráfico es utilizar un archivo XML como datos para el gráfico. Esto requiere que el archivo XML también tenga un archivo de esquema (archivo *.xsd)* que describa la estructura XML. Este procedimiento muestra cómo leer datos de un archivo XML.

1. En la carpeta Datos de la *aplicación,\_* cree un nuevo archivo XML denominado *data.xml*.
2. Reemplace el XML existente por el siguiente, que son algunos datos XML sobre los empleados de una empresa ficticia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. En la carpeta Datos de la *aplicación,\_* cree un nuevo archivo XML denominado *data.xsd*. (Tenga en cuenta que la extensión esta vez es *.xsd*.)
4. Reemplace el XML existente por lo siguiente: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. En la raíz del sitio web, cree un nuevo archivo denominado *ChartDataXML.cshtml*.
6. Reemplace el contenido existente por el siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    El código crea `DataSet` primero un objeto. Este objeto se utiliza para administrar los datos que se leen desde el archivo XML y organizarlos según la información del archivo de esquema. (Tenga en cuenta que la `using SystemData`parte superior del código incluye la instrucción . Esto es necesario para poder trabajar `DataSet` con el objeto. Para obtener más [ &quot;&quot; ](#SB_UsingStatements) información, consulte Uso de instrucciones y nombres completos más adelante en este artículo.)

    A continuación, el `DataView` código crea un objeto basado en el conjunto de datos. La vista de datos proporciona un objeto que el gráfico puede enlazar a &#8212; es decir, leer y trazar. El gráfico se enlaza a `AddSeries` los datos mediante el método, como se vio anteriormente al trazar los datos de la matriz, excepto que esta vez los `xValue` parámetros y `yValues` se establecen en el `DataView` objeto.

    En este ejemplo también se muestra cómo especificar un tipo de gráfico determinado. Cuando se agregan los `AddSeries` datos `chartType` en el método, el parámetro también se establece para mostrar un gráfico circular.
7. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Declaraciones de "uso" y nombres completos
> 
> .NET Framework que ASP.NET páginas Web con sintaxis DeRazor se basa en muchos miles de componentes (clases). Para que sea manejable trabajar con todas estas clases, se organizan en espacios de *nombres,* que son algo así como bibliotecas. Por ejemplo, `System.Web` el espacio de nombres contiene `System.Xml` clases que admiten la comunicación entre `System.Data` el explorador y el servidor, el espacio de nombres contiene clases que se usan para crear y leer archivos XML y el espacio de nombres contiene clases que permiten trabajar con datos.
> 
> Para tener acceso a cualquier clase determinada en .NET Framework, el código debe conocer no solo el nombre de clase, sino también el espacio de nombres en el que se encuentra la clase. Por ejemplo, para utilizar `Chart` la aplicación auxiliar, `System.Web.Helpers.Chart` el código necesita encontrar`System.Web.Helpers`la clase,`Chart`que combina el espacio de nombres ( ) con el nombre de clase ( ). Esto se conoce como nombre *completo de* la clase &#8212; su ubicación completa e inequívoca dentro de la inmensidad de .NET Framework. En el código, esto tendría el siguiente aspecto:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Sin embargo, es engorroso (y propenso a errores) tener que usar estos nombres largos y completos cada vez que desee hacer referencia a una clase o ayudante. Por lo tanto, para facilitar el uso de nombres de clase, puede *importar* los espacios de nombres que le interesan, que normalmente es solo un puñado de entre los muchos espacios de nombres de .NET Framework. Si ha importado un espacio de nombres, puede`Chart`usar solo un nombre`System.Web.Helpers.Chart`de clase ( ) en lugar del nombre completo ( ). Cuando el código se ejecuta y encuentra un nombre de clase, puede buscar solo en los espacios de nombres que ha importado para encontrar esa clase.
> 
> Cuando se usa ASP.NET páginas Web con sintaxis de Razor para crear páginas `WebPage` web, normalmente se usa el mismo conjunto de clases cada vez, incluida la clase, las distintas aplicaciones auxiliares, etc. Para ahorrarle el trabajo de importar los espacios de nombres relevantes cada vez que crea un sitio web, ASP.NET está configurado para que importe automáticamente un conjunto de espacios de nombres principales para cada sitio web. Es por eso que no ha tenido que lidiar con espacios de nombres o importar hasta ahora; Todas las clases con las que ha trabajado están en espacios de nombres que ya se han importado para usted.
> 
> Sin embargo, a veces es necesario trabajar con una clase que no está en un espacio de nombres que se importa automáticamente. En ese caso, puede usar el nombre completo de esa clase o importar manualmente el espacio de nombres que contiene la clase. Para importar un espacio `using` de`import` nombres, use la instrucción (en Visual Basic), como se vio en un ejemplo anterior del artículo.
> 
> Por ejemplo, `DataSet` la clase `System.Data` está en el espacio de nombres. El `System.Data` espacio de nombres no está disponible automáticamente para ASP.NET páginas Razor. Por lo tanto, `DataSet` para trabajar con la clase usando su nombre completo, puede usar código como este:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Si tiene que `DataSet` utilizar la clase repetidamente, puede importar un espacio de nombres como este y, a continuación, usar solo el nombre de clase en el código:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Puede agregar `using` instrucciones para cualquier otro espacio de nombres de .NET Framework al que desee hacer referencia. Sin embargo, como se ha indicado, no tendrá que hacer esto con frecuencia, porque la mayoría de las clases con las que trabajará se encuentran en espacios de nombres que se importan automáticamente por ASP.NET para su uso en *páginas .cshtml* y *.vbhtml.*

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Visualización de gráficos dentro de una página web

En los ejemplos que ha visto hasta ahora, se crea un gráfico y, a continuación, el gráfico se representa directamente en el explorador como un gráfico. En muchos casos, sin embargo, desea mostrar un gráfico como parte de una página, no sólo por sí mismo en el navegador. Para ello se requiere un proceso de dos pasos. El primer paso es crear una página que genere el gráfico, como ya ha visto.

El segundo paso es mostrar la imagen resultante en otra página. Para mostrar la imagen, `<img>` utilice un elemento HTML, de la misma manera que lo haría para mostrar cualquier imagen. Sin embargo, en lugar de hacer referencia `<img>` a un archivo *.jpg* o `Chart` *.png,* el elemento hace referencia al archivo *.cshtml* que contiene la aplicación auxiliar que crea el gráfico. Cuando se ejecuta la `<img>` página para mostrar, `Chart` el elemento obtiene la salida de la aplicación auxiliar y representa el gráfico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Cree un archivo denominado *ShowChart.cshtml*.
2. Reemplace el contenido existente por el siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    El código `<img>` utiliza el elemento para mostrar el gráfico que creó anteriormente en el *ChartArrayBasic.cshtml* archivo.
3. Ejecute la página web en un explorador. El *ShowChart.cshtml* archivo muestra la imagen del gráfico basado en el código contenido en el *ChartArrayBasic.cshtml* archivo.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Estilizar un gráfico

La `Chart` aplicación auxiliar admite un gran número de opciones que le permiten personalizar la apariencia del gráfico. Puede establecer colores, fuentes, bordes, etc. Una manera fácil de personalizar la apariencia de un gráfico es utilizar un *tema.* Los temas son colecciones de información que especifican cómo presentar un gráfico utilizando fuentes, colores, etiquetas, paletas, bordes y efectos. (Tenga en cuenta que el estilo de un gráfico no indica el tipo de gráfico.)

En la tabla siguiente se enumeran los temas integrados.

| Tema | Descripción |
| --- | --- |
| `Vanilla` | Muestra columnas rojas sobre un fondo blanco. |
| `Blue` | Muestra columnas azules sobre un fondo degradado azul. |
| `Green` | Muestra columnas azules sobre un fondo degradado verde. |
| `Yellow` | Muestra columnas naranjas sobre un fondo degradado amarillo. |
| `Vanilla3D` | Muestra columnas rojas 3D sobre un fondo blanco. |

Puede especificar el tema que se utilizará al crear un nuevo gráfico.

1. Cree un nuevo archivo denominado *ChartStyleGreen.cshtml*.
2. Reemplace el contenido existente en la página por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Este código es el mismo que el ejemplo anterior que usa `theme` la base `Chart` de datos para los datos, pero agrega el parámetro cuando crea el objeto. A continuación se muestra el código modificado:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Ejecute la página en un explorador. Verá los mismos datos que antes, pero el gráfico se ve más pulido: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Guardar un gráfico

Cuando usa `Chart` la aplicación auxiliar como ha visto hasta ahora en este artículo, la aplicación auxiliar vuelve a crear el gráfico desde cero cada vez que se invoca. Si es necesario, el código del gráfico también vuelve a consultar la base de datos o vuelve a leer el archivo XML para obtener los datos. En algunos casos, hacer esto puede ser una operación compleja, como si la base de datos que está consultando es grande o si el archivo XML contiene una gran cantidad de datos. Incluso si el gráfico no implica una gran cantidad de datos, el proceso de creación dinámica de una imagen ocupa los recursos del servidor, y si muchas personas solicitan la página o páginas que muestran el gráfico, puede haber un impacto en el rendimiento de su sitio web.

Para ayudarle a reducir el impacto potencial en el rendimiento de la creación de un gráfico, puede crear un gráfico la primera vez que lo necesite y, a continuación, guardarlo. Cuando se necesita el gráfico de nuevo, en lugar de regenerarlo, puede recuperar la versión guardada y representarla.

Puede guardar un gráfico de las siguientes maneras:

- Almacene en caché el gráfico en la memoria del equipo (en el servidor).
- Guarde el gráfico como un archivo de imagen.
- Guarde el gráfico como un archivo XML. Esta opción le permite modificar el gráfico antes de guardarlo.

### <a name="caching-a-chart"></a>Almacenamiento en caché de un gráfico

Después de crear un gráfico, puede almacenarlo en caché. Almacenar en caché un gráfico significa que no es necesario volver a crearlo si es necesario volver a mostrarlo. Cuando guarda un gráfico en la memoria caché, le da una clave que debe ser única para ese gráfico.

Los gráficos guardados en la memoria caché podrían eliminarse si el servidor se queda sin memoria. Además, la memoria caché se borra si la aplicación se reinicia por cualquier motivo. Por lo tanto, la forma estándar de trabajar con un gráfico almacenado en caché es comprobar siempre primero si está disponible en la memoria caché y, si no, crearlo o volver a crearlo.

1. En la raíz de su sitio web, cree un archivo denominado *ShowCachedChart.cshtml*.
2. Reemplace el contenido existente por el siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    La `<img>` etiqueta `src` incluye un atributo que apunta al archivo *ChartSaveToCache.cshtml* y pasa una clave a la página como una cadena de consulta. La clave contiene &quot;el&quot;valor myChartKey . El *ChartSaveToCache.cshtml* archivo `Chart` contiene la aplicación auxiliar que crea el gráfico. Creará esta página en un momento.

    Al final de la página hay un vínculo a una página denominada *ClearCache.cshtml*. Esa es una página que también creará en breve. Necesita el *ClearCache.cshtml* solo para probar el almacenamiento en caché para este ejemplo, no es un vínculo o página que normalmente incluiría al trabajar con gráficos almacenados en caché.
3. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSaveToCache.cshtml*.
4. Reemplace el contenido existente por el siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    El código comprueba primero si se pasó algo como valor de clave en la cadena de consulta. Si es así, el código intenta leer un `GetFromCache` gráfico fuera de la memoria caché llamando al método y pasándole la clave. Si resulta que no hay nada en la memoria caché bajo esa clave (lo que ocurriría la primera vez que se solicita el gráfico), el código crea el gráfico como de costumbre. Cuando el gráfico ha terminado, el código lo `SaveToCache`guarda en la memoria caché llamando a . Ese método requiere una clave (por lo que el gráfico se puede solicitar más adelante) y la cantidad de tiempo que el gráfico debe guardarse en la memoria caché. (La hora exacta en la que almacenaría en caché un gráfico dependerá de la frecuencia con la que pensaba que los datos que representa podrían cambiar.) El `SaveToCache` método también `slidingExpiration` requiere un parámetro &#8212; si se establece en true, el contador de tiempo de espera se restablece cada vez que se accede al gráfico. En este caso, en efecto significa que la entrada de caché del gráfico expira 2 minutos después de la última vez que alguien accedió al gráfico. (La alternativa a la expiración deslizante es la expiración absoluta, lo que significa que la entrada de caché expiraría exactamente 2 minutos después de que se puso en la memoria caché, sin importar la frecuencia con la que se había accedido.)

    Por último, el `WriteFromCache` código utiliza el método para capturar y representar el gráfico desde la memoria caché. Tenga en cuenta que `if` este método está fuera del bloque que comprueba la memoria caché, ya que obtendrá el gráfico de la memoria caché si el gráfico estaba allí para comenzar o tenía que generarse y guardarse en la memoria caché.

    Observe que en el `AddTitle` ejemplo, el método incluye una marca de tiempo. (Agrega la fecha y hora `DateTime.Now` actuales &#8212; &#8212; al título.)
5. Cree una nueva página denominada *ClearCache.cshtml* y reemplace su contenido por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Esta página `WebCache` utiliza la aplicación auxiliar para quitar el gráfico almacenado en caché en *ChartSaveToCache.cshtml*. Como se indicó anteriormente, normalmente no tiene que tener una página como esta. Lo está creando aquí solo para facilitar la prueba del almacenamiento en caché.
6. Ejecute la página web *ShowCachedChart.cshtml* en un explorador. La página muestra la imagen del gráfico en función del código contenido en el *ChartSaveToCache.cshtml* archivo. Tome nota de lo que dice la marca de tiempo en el título del gráfico. 

    ![Descripción: Imagen del gráfico básico con marca de tiempo en el título del gráfico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Cierre el explorador.
8. Ejecute el *ShowCachedChart.cshtml* de nuevo. Observe que la marca de tiempo es la misma que antes, lo que indica que el gráfico no se ha regenerado, sino que se leyó desde la memoria caché.
9. En *ShowCachedChart.cshtml*, haga clic en el vínculo **Borrar caché.** Esto le lleva a *ClearCache.cshtml*, que informa de que se ha borrado la memoria caché.
10. Haga clic en el **volver a ShowCachedChart.cshtml** enlace, o vuelva a ejecutar *ShowCachedChart.cshtml* desde WebMatrix. Observe que esta vez la marca de tiempo ha cambiado, porque la memoria caché se ha borrado. Por lo tanto, el código tenía que regenerar el gráfico y volver a colocarlo en la memoria caché.

### <a name="saving-a-chart-as-an-image-file"></a>Guardar un gráfico como un archivo de imagen

También puede guardar un gráfico como un archivo de imagen (por ejemplo, como un archivo *.jpg)* en el servidor. A continuación, puede utilizar el archivo de imagen de la forma en que lo haría con cualquier imagen. La ventaja es que el archivo se almacena en lugar de guardarse en una memoria caché temporal. Puede guardar una nueva imagen de gráfico en diferentes momentos (por ejemplo, cada hora) y, a continuación, mantener un registro permanente de los cambios que se producen a lo largo del tiempo. Tenga en cuenta que debe asegurarse de que la aplicación web tiene permiso para guardar un archivo en la carpeta del servidor donde desea colocar el archivo de imagen.

1. En la raíz de su sitio web, cree una carpeta denominada * \_ChartFiles* si aún no existe.
2. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSave.cshtml*.
3. Reemplace el contenido existente por el siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    El código comprueba primero si el archivo *.jpg* `File.Exists` existe llamando al método. Si el archivo no existe, el `Chart` código crea un nuevo a partir de una matriz. Esta vez, el `Save` código llama al `path` método y pasa el parámetro para especificar la ruta de acceso del archivo y el nombre de archivo de dónde guardar el gráfico. En el cuerpo de `<img>` la página, un elemento utiliza la ruta de acceso para apuntar al archivo *.jpg* que se va a mostrar.
4. Ejecute el *ChartSave.cshtml* archivo.
5. Vuelva a WebMatrix. Observe que se ha guardado un archivo de imagen denominado *chart01.jpg* en la * \_carpeta ChartFiles.*

### <a name="saving-a-chart-as-an-xml-file"></a>Guardar un gráfico como un archivo XML

Por último, puede guardar un gráfico como un archivo XML en el servidor. Una ventaja de usar este método sobre el almacenamiento en caché del gráfico o guardar el gráfico en un archivo es que podría modificar el XML antes de mostrar el gráfico si lo desea. La aplicación debe tener permisos de lectura y escritura para la carpeta en el servidor donde desea colocar el archivo de imagen.

1. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSaveXml.cshtml*.
2. Reemplace el contenido existente por el siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Este código es similar al código que vio anteriormente para almacenar un gráfico en la memoria caché, excepto que usa un archivo XML. El código comprueba primero si el archivo XML `File.Exists` existe llamando al método. Si el archivo existe, el `Chart` código crea un nuevo objeto `themePath` y pasa el nombre de archivo como parámetro. Esto crea el gráfico basado en lo que hay en el archivo XML. Si el archivo XML aún no existe, el código crea `SaveXml` un gráfico como normal y, a continuación, llama para guardarlo. El gráfico se representa `Write` mediante el método, como se ha visto antes.

    Al igual que con la página que mostraba el almacenamiento en caché, este código incluye una marca de tiempo en el título del gráfico.
3. Cree una nueva página denominada *ChartDisplayXMLChart.cshtml* y agréguele el siguiente marcado: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Ejecute la página *ChartDisplayXMLChart.cshtml.* Se muestra el gráfico. Tome nota de la marca de tiempo en el título del gráfico.
5. Cierre el explorador.
6. En WebMatrix, haga clic con el **Refresh**botón secundario en la * \_carpeta ChartFiles,* haga clic en Actualizar y, a continuación, abra la carpeta. El archivo *XMLChart.xml* de esta `Chart` carpeta fue creado por la aplicación auxiliar. 

    ![Descripción: _ChartFiles carpeta que muestra el archivo XMLChart.xml creado por la aplicación auxiliar Chart.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Ejecute el *ChartDisplayXMLChart.cshtml* página de nuevo. El gráfico muestra la misma marca de tiempo que la primera vez que ejecutó la página. Esto se debe a que el gráfico se genera a partir del XML que guardó anteriormente.
8. En WebMatrix, * \_* abra la carpeta ChartFiles y elimine el archivo *XMLChart.xml.*
9. Ejecute el *ChartDisplayXMLChart.cshtml* página una vez más. Esta vez, la marca de `Chart` tiempo se actualiza, porque la aplicación auxiliar tenía que volver a crear el archivo XML. Si lo desea, * \_* compruebe la carpeta ChartFiles y observe que el archivo XML está de vuelta.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Introducción al trabajo con una base de datos en ASP.NET sitios de páginas web](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Uso del almacenamiento en caché en ASP.NET sitios de páginas web para mejorar el rendimiento](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Clase Chart](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (ASP.NET referencia de la API web Web Pages en MSDN)
