---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Presentación de ASP.NET Páginas Web - Conceptos básicos de formularios HTML Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestran los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET páginas web (Razor). Y ahora que...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675926"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Presentación de ASP.NET Web Pages - HTML Form Basics

 por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestran los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET páginas web (Razor). Y ahora que tienes una base de datos, usarás tus habilidades de formulario para permitir que los usuarios encuentren películas específicas en la base de datos. Se supone que ha completado la serie a través de [Introducción a la visualización](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)de datos mediante ASP.NET páginas web .
> 
> Temas que se abordarán:
> 
> - Cómo crear un formulario mediante elementos HTML estándar.
> - Cómo leer la entrada del usuario en un formulario.
> - Cómo crear una consulta SQL que obtiene datos de forma selectiva mediante un término de búsqueda que proporciona el usuario.
> - Cómo tener campos en la página "recordar" lo que el usuario ingresó.
>   
> 
> Características/tecnologías discutidas:
> 
> - Objeto `Request`.
> - La `Where` cláusula SQL.

## <a name="what-youll-build"></a>Lo que creará

En el tutorial anterior, creó una base de datos, le agregó datos y, a continuación, usó la `WebGrid` aplicación auxiliar para mostrar los datos. En este tutorial, agregará un cuadro de búsqueda que le permite encontrar películas de un género específico o cuyo título contiene cualquier palabra que introduzca. (Por ejemplo, podrás encontrar todas las películas cuyo género sea "Acción" o cuyo título contenga "Harry" o "Aventura")..

Cuando termines con este tutorial, tendrás una página como esta:

![Página de películas con búsqueda de género y título](form-basics/_static/image1.png)

La parte de la lista de la &mdash; página es la misma que en el último tutorial una cuadrícula. La diferencia será que la cuadrícula mostrará solo las películas que ha buscado.

## <a name="about-html-forms"></a>Acerca de los formularios HTML

(Si tiene experiencia en la creación de formularios `GET` `POST`HTML y con la diferencia entre y , puede omitir esta sección.)

Un formulario tiene &mdash; cuadros de texto de elementos de entrada de usuario, botones, botones de opción, casillas de verificación, listas desplegables, etc. Los usuarios rellenan estos controles o realizan selecciones y, a continuación, envían el formulario haciendo clic en un botón.

La sintaxis HTML básica de un formulario se ilustra en este ejemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Cuando este marcado se ejecuta en una página, crea un formulario simple similar a esta ilustración:

![Formulario HTML básico tal como se representa en el navegador](form-basics/_static/image2.png)

El `<form>` elemento encierra los elementos HTML que se van a enviar. (Un error fácil de cometer es agregar elementos a la `<form>` página, pero luego olvidarse de ponerlos dentro de un elemento. En ese caso, no se presenta nada.) El `method` atributo indica al explorador cómo enviar la entrada del usuario. Establezca esta `post` configuración si va a realizar una actualización `get` en el servidor o en si solo está obteniendo datos del servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST y HTTP Verb Safety**
> 
> HTTP, el protocolo que utilizan los navegadores y servidores para intercambiar información, es notablemente simple en sus operaciones básicas. Los navegadores utilizan solo unos pocos verbos para realizar solicitudes a los servidores. Al escribir código para la web, es útil comprender estos verbos y cómo los utilizan el navegador y el servidor. Lejos y lejos los verbos más utilizados son los siguientes:
> 
> - `GET`. El navegador utiliza este verbo para obtener algo del servidor. Por ejemplo, al escribir una dirección URL en `GET` el explorador, el explorador realiza una operación para solicitar la página que desea. Si la página incluye gráficos, `GET` el explorador realiza operaciones adicionales para obtener las imágenes. Si `GET` la operación tiene que pasar información al servidor, la información se pasa como parte de la dirección URL de la cadena de consulta.
> - `POST`. El navegador `POST` envía una petición para enviar los datos para ser agregados o cambiados en el servidor. Por ejemplo, `POST` el verbo se utiliza para crear registros en una base de datos o cambiar los existentes. La mayoría de las veces, cuando rellena un formulario y `POST` hace clic en el botón Enviar, el explorador realiza una operación. En `POST` una operación, los datos que se pasan al servidor se encuentra en el cuerpo de la página.
> 
> Una distinción importante entre estos `GET` verbos es que una operación no se supone que cambie `GET` nada en el servidor, o para ponerlo de una manera un poco más abstracta, una operación no da lugar a un cambio de estado en el servidor. Puede realizar `GET` una operación con los mismos recursos tantas veces como desee y esos recursos no cambian. (A `GET` menudo se dice que una operación es "segura", o que utiliza un término técnico, es *idempotente*.) Por el contrario, `POST` por supuesto, una solicitud cambia algo en el servidor cada vez que realiza la operación.
> 
> Dos ejemplos ayudarán a ilustrar esta distinción. Cuando realiza una búsqueda con un motor como Bing o Google, rellena un formulario que consta de un cuadro de texto y, a continuación, hace clic en el botón de búsqueda. El explorador realiza `GET` una operación, con el valor especificado en el cuadro pasado como parte de la dirección URL. El `GET` uso de una operación para este tipo de formulario está bien, ya que una operación de búsqueda no cambia ningún recurso en el servidor, solo recupera información.
> 
> Ahora considere el proceso de pedir algo en línea. Rellene los detalles del pedido y, a continuación, haga clic en el botón Enviar. Esta operación `POST` será una solicitud, porque la operación dará lugar a cambios en el servidor, como un nuevo registro de pedido, un cambio en la información de su cuenta y tal vez muchos otros cambios. A `GET` diferencia de la `POST` operación, no puede repetir su solicitud: si lo hizo, cada vez que vuelva a enviar la solicitud, generaría un nuevo pedido en el servidor. (En casos como este, los sitios web a menudo le advertirán que no haga clic en un botón de envío más de una vez, o desactivará el botón de envío para que no vuelva a enviar el formulario accidentalmente.)
> 
> En el transcurso de este tutorial, `GET` usará `POST` una operación y una operación para trabajar con formularios HTML. Explicaremos en cada caso por qué el verbo que usas es el adecuado.
> 
> (Para obtener más información sobre los verbos HTTP, consulte el artículo [Definiciones](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) de método en el sitio W3C.)

La mayoría de `<input>` los elementos de entrada del usuario son elementos HTML. Parecen `<input type="type" name="name">,` donde *el tipo* indica el tipo de control de entrada de usuario que desea. Estos elementos son los comunes:

- Cuadro de texto:`<input type="text">`
- Casilla de verificación:`<input type="check">`
- Botón de radio:`<input type="radio">`
- Botón:`<input type="button">`
- Botón Enviar:`<input type="submit">`

También puede utilizar `<textarea>` el elemento para crear un `<select>` cuadro de texto de varias líneas y el elemento para crear una lista desplegable o una lista desplazable. (Para obtener más información sobre los elementos de formulario HTML, consulte [Formularios HTML y entrada](http://www.w3schools.com/html/html_forms.asp) en el sitio de W3Schools.)

El `name` atributo es muy importante, porque el nombre es cómo obtendrá el valor del elemento más adelante, como verá en breve.

La parte interesante es lo que usted, el desarrollador de la página, hace con la entrada del usuario. No hay ningún comportamiento integrado asociado con estos elementos. En su lugar, tiene que obtener los valores que el usuario ha introducido o seleccionado y hacer algo con ellos. Eso es lo que aprenderás en este tutorial.

> [!TIP] 
> 
> **HTML5 y formularios de entrada**
> 
> Como usted puede saber, HTML está en transición y la última versión (HTML5) incluye soporte para formas más intuitivas para que los usuarios ingresen información. Por ejemplo, en HTML5, usted (el desarrollador de la página) puede indicar a la página que desea que el usuario escriba una fecha. El navegador puede mostrar automáticamente un calendario en lugar de requerir que el usuario introduzca una fecha manualmente. Sin embargo, HTML5 es nuevo y no es compatible con todos los navegadores todavía.
> 
> ASP.NET Web Pages admite la entrada HTML5 en la medida en que lo hace el explorador del usuario. Para obtener una idea de `<input>` los nuevos atributos para el elemento en HTML5, consulte Tipo de [entrada &lt;&gt; HTML Atributo](http://www.w3schools.com/html/html_form_input_types.asp) en el sitio W3Schools.

## <a name="creating-the-form"></a>Crear el formulario

En WebMatrix, en el **archivos** área de trabajo, abra el *Movies.cshtml* página.

Después `</h1>` de la etiqueta `<div>` de `grid.GetHtml` cierre y antes de la etiqueta de apertura de la llamada, agregue el siguiente marcado:

[!code-html[Main](form-basics/samples/sample2.html)]

Este marcado crea un formulario que `searchGenre` tiene un cuadro de texto denominado y un botón de envío. El cuadro de texto y el `<form>` botón Enviar se incluyen en un elemento cuyo `method` atributo está establecido en `get`. (Recuerde que si no coloca el cuadro de `<form>` texto y envía el botón dentro de un elemento, no se enviará nada al hacer clic en el botón.) Utilice el `GET` verbo aquí porque está creando un formulario que no realiza ningún cambio en el servidor, solo da como resultado una búsqueda. (En el tutorial anterior, `post` usó un método, que es cómo enviar los cambios al servidor. Lo verá en el siguiente tutorial de nuevo.)

Ejecute la página. Aunque no ha definido ningún comportamiento para el formulario, puede ver cómo se ve:

![Página De películas con cuadro de búsqueda para Género](form-basics/_static/image3.png)

Introduzca un valor en el cuadro de texto, como "Comedia". A continuación, haga clic en **Buscar género**.

Tome nota de la URL de la página. Dado que `<form>` establece el `method` atributo `get`del elemento en , el valor que ha especificado ahora forma parte de la cadena de consulta en la dirección URL, como esta:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Valores del formulario de lectura

La página ya contiene código que obtiene datos de base de datos y muestra los resultados en una cuadrícula. Ahora tiene que agregar código que lea el valor del cuadro de texto para que pueda ejecutar una consulta SQL que incluya el término de búsqueda.

Dado que establece el método `get`del formulario en , puede leer el valor que se ha especificado en el cuadro de texto mediante código como el siguiente:

`var searchTerm = Request.QueryString["searchGenre"];`

El `Request.QueryString` objeto `QueryString` (la `Request` propiedad del objeto) incluye los valores de `GET` los elementos que se enviaron como parte de la operación. La `Request.QueryString` propiedad contiene una *colección* (una lista) de los valores que se envían en el formulario. Para obtener cualquier valor individual, especifique el nombre del elemento que desee. Es por eso que tiene `name` que `<input>` tener`searchTerm`un atributo en el elemento ( ) que crea el cuadro de texto. (Para obtener `Request` más información sobre el objeto, consulte la [barra lateral](#BKMK_TheRequestObject) más adelante.)

Es bastante simple leer el valor del cuadro de texto. Pero si el usuario no ingresó nada en absoluto en el cuadro de texto, pero hizo clic en **Buscar** de todos modos, puede ignorar ese clic, ya que no hay nada que buscar.

El código siguiente es un ejemplo que muestra cómo implementar estas condiciones. (No tiene que agregar este código todavía; lo hará en un momento.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

La prueba se descompone de esta manera:

- Obtenga el `Request.QueryString["searchGenre"]`valor de , es decir, `<input>` el `searchGenre`valor que se ha especificado en el elemento denominado .
- Averigua si está vacío usando `IsEmpty` el método. Este método es la forma estándar de determinar si algo (por ejemplo, un elemento de formulario) contiene un valor. Pero en realidad, te importa sólo si *no* está vacío, por lo tanto ...
- Agregue `!` el operador delante `IsEmpty` de la prueba. (El `!` operador significa NOT lógico).

En inglés simple, toda `if` la condición se traduce en lo siguiente: Si el elemento *searchGenre del formulario no está vacío, entonces ...*

Este bloque establece el escenario para crear una consulta que usa el término de búsqueda. Lo hará en la sección siguiente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **El objeto Request**
> 
> El `Request` objeto contiene toda la información que el explorador envía a la aplicación cuando se solicita o envía una página. Este objeto incluye cualquier información que el usuario proporcione, como valores de cuadro de texto o un archivo para cargar. También incluye todo tipo de información adicional, como cookies, valores en la cadena de consulta URL (si existe), la ruta de acceso del archivo de la página que se está ejecutando, el tipo de navegador que utiliza el usuario, la lista de idiomas que se establecen en el explorador y mucho más.
> 
> El `Request` objeto es una *colección* (lista) de valores. Para obtener un valor individual de la colección, especifique su nombre:
> 
> `var someValue = Request["name"];`
> 
> El `Request` objeto expone realmente varios subconjuntos. Por ejemplo:
> 
> - `Request.Form`proporciona valores de elementos `<form>` dentro del elemento `POST` enviado si la solicitud es una solicitud.
> - `Request.QueryString`le da solo los valores de la cadena de consulta de la dirección URL. (En una `http://mysite/myapp/page?searchGenre=action&page=2`dirección `?searchGenre=action&page=2` URL como , la sección de la dirección URL es la cadena de consulta.)
> - `Request.Cookies`colección le da acceso a las cookies que el navegador ha enviado.
> 
> Para obtener un valor que sabe que está `Request["name"]`en el formulario enviado, puede usar . Como alternativa, puede utilizar las `Request.Form["name"]` versiones más específicas (para `POST` solicitudes) o `Request.QueryString["name"]` (para `GET` solicitudes). Por supuesto, *el nombre* es el nombre del elemento que se va a obtener.
> 
> El nombre del elemento que desea obtener tiene que ser único dentro de la colección que está utilizando. Es por eso `Request` que el objeto `Request.Form` `Request.QueryString`proporciona los subconjuntos like y . Supongamos que la página `userName` contiene un elemento `userName`de formulario denominado y *también* contiene una cookie denominada . Si obtiene `Request["userName"]`, es ambiguo si desea el valor del formulario o la cookie. Sin embargo, `Request.Form["userName"]` `Request.Cookie["userName"]`si obtiene o , está siendo explícito sobre qué valor obtener.
> 
> Es una buena práctica ser específico y `Request` usar el subconjunto del `Request.Form` que `Request.QueryString`te interesa, como o . Para las páginas simples que está creando en este tutorial, probablemente no haga ninguna diferencia. Sin embargo, a medida que crea `Request.Form` `Request.QueryString` páginas más complejas, utilizando la versión explícita o puede ayudarle a evitar problemas que pueden surgir cuando la página contiene un formulario (o varios formularios), cookies, valores de cadena de consulta, etc.

## <a name="creating-a-query-by-using-a-search-term"></a>Creación de una consulta mediante un término de búsqueda

Ahora que sabe cómo obtener el término de búsqueda que el usuario ha especificado, puede crear una consulta que lo use. Recuerde que para obtener todos los elementos de película de la base de datos, está utilizando una consulta SQL que se parece a esta instrucción:

`SELECT * FROM Movies`

Para obtener solo determinadas películas, debe `Where` usar una consulta que incluya una cláusula. Esta cláusula le permite establecer una condición en la que la consulta devuelve las filas. Este es un ejemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

El formato `WHERE column = value`básico es . Puede utilizar diferentes operadores `=`además de , como `>` (mayor que), `<` (menor que), `<>` (no igual a), `<=` (menor o igual que), etc., dependiendo de lo que esté buscando.

En caso de que se pregunte, las &mdash; `SELECT` instrucciones SQL `Select` no `select`distinguen entre mayúsculas y minúsculas es la misma que (o incluso ). Sin embargo, las personas a menudo `SELECT` capitalizan las palabras clave en una instrucción SQL, como y `WHERE`, para que sea más fácil de leer.

### <a name="passing-the-search-term-as-a-parameter"></a>Pasar el término de búsqueda como parámetro

Buscar un género específico es`WHERE Genre = 'Action'`bastante fácil ( ), pero quieres poder buscar cualquier género que el usuario introduzca. Para ello, cree como consulta SQL que incluya un marcador de posición para el valor que se va a buscar. Se verá como este comando:

`SELECT * FROM Movies WHERE Genre = @0`

El marcador `@` de posición es el carácter seguido de cero. Como puede adivinar, una consulta puede contener varios marcadores `@0` `@1`de `@2`posición y se denominaría , , , etc.

Para configurar la consulta y pasarle realmente el valor, utilice el código como el siguiente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Este código es similar a lo que ya ha hecho para mostrar datos en la cuadrícula. Las únicas diferencias son:

- La consulta contiene`WHERE Genre = @0"`un marcador de posición ( ).
- La consulta se coloca`selectCommand`en una variable ( ); antes, pasó la consulta `db.Query` directamente al método.
- Cuando se `db.Query` llama al método, se pasa la consulta y el valor que se usará para el marcador de posición. (Si la consulta tuviera varios marcadores de posición, los pasaría todos como valores independientes al método.)

Si junta todos estos elementos, obtendrá el siguiente código:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **¡Importante!** El uso de `@0`marcadores de posición (como ) para pasar valores a un comando SQL es *extremadamente importante* para la seguridad. La forma en que lo ve aquí, con marcadores de posición para datos variables, es la única manera de construir comandos SQL.
> 
> Nunca construya una instrucción SQL reuniendo (concatenando) texto literal y los valores que obtiene del usuario. La concatenación de la entrada del usuario en una instrucción SQL abre el sitio a un ataque de *inyección SQL* en el que un usuario malintencionado envía valores a la página que hackea la base de datos. (Puede leer más en el artículo [Inyección SQL](https://msdn.microsoft.com/library/ms161953.aspx) el sitio web de MSDN.)

## <a name="updating-the-movies-page-with-search-code"></a>Actualización de la página De las películas con código de búsqueda

Ahora puede actualizar el código en el *Movies.cshtml* archivo. Para empezar, reemplace el código del bloque de código en la parte superior de la página con este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La diferencia aquí es que ha puesto `selectCommand` la consulta en la `db.Query` variable, que pasará más adelante. Colocar la instrucción SQL en una variable le permite cambiar la instrucción, que es lo que hará para realizar la búsqueda.

También has eliminado estas dos líneas, que volverás a colocar más adelante:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Todavía no desea ejecutar la consulta (es `db.Query`decir, llame) y tampoco `WebGrid` desea inicializar la aplicación auxiliar. Lo hará después de descubrir qué instrucción SQL tiene que ejecutarse.

Después de este bloque reescrito, puede agregar la nueva lógica para controlar la búsqueda. El código completado tendrá el siguiente aspecto. Actualice el código de la página para que coincida con este ejemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La página ahora funciona así. Cada vez que se ejecuta la página, el código abre la base de datos y la `selectCommand` variable se establece en la instrucción SQL que obtiene todos los registros de la `Movies` tabla. El código también `searchTerm` inicializa la variable.

Sin embargo, si la solicitud `searchGenre` actual incluye `selectCommand` un valor para el elemento, el `Where` código se establece en una consulta diferente, es decir, en una que incluya la cláusula para buscar un género. También se `searchTerm` establece en lo que se pasó para el cuadro de búsqueda (que podría ser nada).

Independientemente de la instrucción SQL `selectCommand`en `db.Query` , el código llama a `searchTerm`continuación para ejecutar la consulta, pasándole lo que sea en . Si no hay `searchTerm`nada en , no importa, porque en ese caso no `selectCommand` hay ningún parámetro para pasar el valor de todos modos.

Por último, el `WebGrid` código inicializa la aplicación auxiliar mediante los resultados de la consulta, como antes.

Puede ver que al colocar la instrucción SQL y el término de búsqueda en variables, ha agregado flexibilidad al código. Como verá más adelante en este tutorial, puede usar este marco básico y seguir agregando lógica para diferentes tipos de búsquedas.

## <a name="testing-the-search-by-genre-feature"></a>Prueba de la función Buscar por género

En WebMatrix, ejecute el *Movies.cshtml* página. Verá la página con el cuadro de texto para el género.

Escriba un género que haya introducido para uno de los registros de prueba y, a continuación, haga clic en **Buscar**. Esta vez verás una lista de las películas que coinciden con ese género:

![Lista de páginas de películas después de buscar el género 'Comedies'](form-basics/_static/image4.png)

Introduzca un género diferente y vuelva a buscar. Intente introducir el género utilizando todas las letras minúsculas o mayúsculas para que pueda ver que la búsqueda no distingue mayúsculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Recordar" lo que el usuario ingresó

Es posible que haya notado que después de entrar en un género y haga clic en **Buscar género**, vio un listado para ese género. Sin embargo, el &mdash; cuadro de texto de búsqueda estaba vacío en otras palabras, no recordaba lo que había introducido.

Es importante entender por qué se produce este comportamiento. Cuando envía una página, el explorador envía una solicitud al servidor web. Cuando ASP.NET obtiene la solicitud, crea una nueva instancia de la página, ejecuta el código en ella y, a continuación, vuelve a representar la página en el explorador. En efecto, sin embargo, la página no sabe que usted estaba trabajando con una versión anterior de sí mismo. Todo lo que sabe es que recibió una solicitud que tenía algunos datos de formulario en ella.

Cada vez que &mdash; solicites una página, ya &mdash; sea por primera vez o enviándola, obtendrás una nueva página. El servidor web no tiene memoria de la última solicitud. Tampoco ASP.NET, y tampoco lo hace el navegador. La única conexión entre estas instancias independientes de la página es cualquier dato que se transmita entre ellas. Si envía una página, por ejemplo, la nueva instancia de página puede obtener los datos del formulario enviados por la instancia anterior. (Otra forma de pasar datos entre páginas es utilizar cookies.)

Una forma formal de describir esta situación es decir que las páginas web son *sin estado.* Los servidores web y las propias páginas y los elementos de la página no mantienen ninguna información sobre el estado anterior de una página. La web fue diseñada de esta manera porque mantener el estado de las solicitudes individuales agotaría rápidamente los recursos de los servidores web, que a menudo manejan miles, tal vez incluso cientos de miles, de solicitudes por segundo.

Por eso el cuadro de texto estaba vacío. Después de enviar la página, ASP.NET creó una nueva instancia de la página y corrió a través del código y el marcado. No había nada en ese código que ASP.NET dijera que pusiera un valor en el cuadro de texto. Así que ASP.NET no hizo nada, y el cuadro de texto se representó sin un valor en él.

En realidad, hay una manera fácil de evitar este problema. El género que ha introducido en el cuadro &mdash; de texto `Request.QueryString["searchGenre"]` *está* disponible en el código en el que se encuentra.

Actualice el marcado del cuadro `value` de texto para `searchTerm`que el atributo obtenga su valor de , como en este ejemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

En esta página, también podría `value` haber `searchTerm` establecido el atributo en la variable, ya que esa variable también contiene el género que ha introducido. Pero el `Request` uso del `value` objeto para establecer el atributo como se muestra aquí es la forma estándar de realizar esta tarea. (Suponiendo que incluso desea &mdash; hacer esto en algunas situaciones, es posible que desee representar la página *sin* valores en los campos. Todo depende de lo que está pasando con su aplicación.)

> [!NOTE]
> No puede "recordar" el valor de un cuadro de texto que se usa para las contraseñas. Sería un agujero de seguridad para permitir que las personas rellenen un campo de contraseña mediante el uso de código.

Vuelva a ejecutar la página, escriba un género y haga clic en **Buscar género**. Esta vez no solo verá los resultados de la búsqueda, sino que el cuadro de texto recuerda lo que ingresó la última vez:

![Página que muestra que el cuadro de texto ha 'recordado' la entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Búsqueda de cualquier palabra en el título

Ahora puede buscar cualquier género, pero también es posible que desee buscar un título. Es difícil obtener un título exactamente correcto cuando se busca, por lo que en su lugar se puede buscar una palabra que aparece en cualquier lugar dentro de un título. Para ello en SQL, `LIKE` utilice el operador y la sintaxis como la siguiente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Este comando obtiene todas las películas cuyos títulos contienen "aventura". Cuando se `LIKE` utiliza el operador, `%` se incluye el carácter comodín como parte del término de búsqueda. La `LIKE 'adventure%'` búsqueda significa "empezar con 'aventura'". (Técnicamente, significa "La cadena 'aventura' seguida de cualquier cosa.") Del mismo modo, el término `LIKE '%adventure'` de búsqueda significa "cualquier cosa seguida de la cadena 'aventura'", que es otra manera de decir "terminar con 'aventura'".

Por lo `LIKE '%adventure%'` tanto, el término de búsqueda significa "con 'aventura' en cualquier parte del título". (Técnicamente, "cualquier cosa en el título, seguido de 'aventura', seguido de cualquier cosa.")

Dentro `<form>` del elemento, agregue el siguiente `</div>` marcado justo debajo de `</form>` la etiqueta de cierre para la búsqueda de género (justo antes del elemento de cierre):

[!code-html[Main](form-basics/samples/sample10.html)]

El código para controlar esta búsqueda es similar al código para la `LIKE` búsqueda de género, excepto que tiene que ensamblar la búsqueda. Dentro del bloque de código en la `if` parte superior `if` de la página, agregue este bloque justo después del bloque para la búsqueda de género:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Este código utiliza la misma lógica que vio anteriormente, excepto que la búsqueda utiliza un `LIKE` operador y el código coloca "`%`" antes y después del término de búsqueda.

Observe cómo era fácil agregar otra búsqueda a la página. Todo lo que tenías que hacer era:

- Cree `if` un bloque que haya probado para ver si el cuadro de búsqueda correspondiente tenía un valor.
- Establezca `selectCommand` la variable en una nueva instrucción SQL.
- Establezca `searchTerm` la variable en el valor que se va a pasar a la consulta.

Aquí está el bloque de código completo, que contiene la nueva lógica para una búsqueda de título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Este es un resumen de lo que hace este código:

- Las `searchTerm` variables `selectCommand` y se inicializan en la parte superior. Va a establecer estas variables en el término de búsqueda adecuado (si existe) y el comando SQL adecuado en función de lo que hace el usuario en la página. La búsqueda predeterminada es el caso simple de obtener todas las películas de la base de datos.
- En las `searchGenre` pruebas `searchTitle`para y `searchTerm` , el código se establece en el valor que desea buscar. Esos bloques `selectCommand` de código también se establecen en un comando SQL adecuado para esa búsqueda.
- El `db.Query` método se invoca una sola vez, utilizando cualquier comando SQL en `selectedCommand` y cualquier valor en `searchTerm`. Si no hay ningún término de búsqueda (sin `searchTerm` género y sin palabra de título), el valor de es una cadena vacía. Sin embargo, eso no importa, porque en ese caso la consulta no requiere un parámetro.

## <a name="testing-the-title-search-feature"></a>Probar la función de búsqueda de títulos

Ahora puede probar su página de búsqueda completada. Ejecute *Movies.cshtml*.

Introduzca un género y haga clic en **Buscar género**. La cuadrícula muestra películas de ese género, como antes.

Introduzca una palabra de título y haga clic en **Buscar título**. La cuadrícula muestra las películas que tienen esa palabra en el título.

![Lista de páginas de películas después de buscar 'The' en el título](form-basics/_static/image6.png)

Deje ambos cuadros de texto en blanco y haga clic en cualquiera de los botones. La cuadrícula muestra todas las películas.

## <a name="combining-the-queries"></a>Combinación de las consultas

Es posible que observe que las búsquedas que puede realizar son exclusivas. No se puede buscar el título y el género al mismo tiempo, incluso si ambos cuadros de búsqueda tienen valores en ellos. Por ejemplo, no puede buscar todas las películas de acción cuyo título contenga "Aventura". (Como la página está codificada ahora, si introduce valores tanto para el género como para el título, la búsqueda de título obtiene prioridad.) Para crear una búsqueda que combine las condiciones, tendría que crear una consulta SQL que tenga una sintaxis como la siguiente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Y tendría que ejecutar la consulta mediante una instrucción como la siguiente (en términos generales):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

La creación de lógica para permitir muchas permutaciones de criterios de búsqueda puede implicarse un poco, como puede ver. Por lo tanto, nos detendremos aquí.

## <a name="coming-up-next"></a>Próximamente

En el siguiente tutorial, creará una página que usa un formulario para permitir a los usuarios agregar películas a la base de datos.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Listado completo para la página de la película (actualizado con la búsqueda)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET usando la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) en el sitio de W3Schools
- [Artículo Definiciones](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) de métodos en el sitio W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md)
> [Siguiente](entering-data.md)
