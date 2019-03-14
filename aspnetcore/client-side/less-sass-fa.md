---
title: Less, Sass y Font Awesome en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar Less, Sass y Font Awesome en aplicaciones ASP.NET Core.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065562"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="d00c8-103">Less, Sass y Font Awesome en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d00c8-103">Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="d00c8-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d00c8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d00c8-105">Los usuarios de aplicaciones web tienen expectativas cada vez más altos en cuanto a estilo y la experiencia general.</span><span class="sxs-lookup"><span data-stu-id="d00c8-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="d00c8-106">Aplicaciones web modernas aprovechan con frecuencia completas herramientas y marcos de trabajo para definir y administrar su apariencia y comportamiento de una manera coherente.</span><span class="sxs-lookup"><span data-stu-id="d00c8-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="d00c8-107">Los marcos de [Bootstrap](http://getbootstrap.com/) puede ir un largo camino para definir un conjunto común de los estilos y opciones de diseño para los sitios web.</span><span class="sxs-lookup"><span data-stu-id="d00c8-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="d00c8-108">Sin embargo, la mayoría de los sitios no trivial también se beneficiarán de poder definir de forma eficaz y mantener archivos (CSS) de la hoja de estilos en cascada y estilos, así como tener un acceso fácil a los iconos que no sean image que ayudan a hacer más intuitiva interfaz de la carpeta del sitio.</span><span class="sxs-lookup"><span data-stu-id="d00c8-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="d00c8-109">Es ahí donde lenguajes y herramientas que admiten [menos](http://lesscss.org/) y [Sass](http://sass-lang.com/), y las bibliotecas como [Font Awesome](http://fontawesome.io/), vienen en.</span><span class="sxs-lookup"><span data-stu-id="d00c8-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="d00c8-110">Idiomas de preprocesador de CSS</span><span class="sxs-lookup"><span data-stu-id="d00c8-110">CSS preprocessor languages</span></span>

<span data-ttu-id="d00c8-111">Los idiomas que se compilan en otros lenguajes, con el fin de mejorar la experiencia de trabajar con el lenguaje subyacente, se conocen como preprocesadores.</span><span class="sxs-lookup"><span data-stu-id="d00c8-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="d00c8-112">Hay dos preprocesadores populares de CSS: Menos y Sass.</span><span class="sxs-lookup"><span data-stu-id="d00c8-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="d00c8-113">Estos preprocesadores agregar características a CSS, como la compatibilidad con variables y reglas anidadas, lo que mejora la facilidad de mantenimiento de las hojas de estilo grandes y complejas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="d00c8-114">CSS como un lenguaje es muy básico, la falta de compatibilidad con incluso algo tan sencillo como variables, y esto tiende a hacer que los archivos CSS recargada y repetitivas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="d00c8-115">Agregar características de lenguaje de programación real a través de preprocesadores puede ayudar a reducir la duplicación y proporcionar una mejor organización de las reglas de estilo.</span><span class="sxs-lookup"><span data-stu-id="d00c8-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="d00c8-116">Visual Studio proporciona compatibilidad integrada para ambos Less y Sass, así como las extensiones que pueden mejorar aún más la experiencia de desarrollo cuando se trabaja con estos lenguajes.</span><span class="sxs-lookup"><span data-stu-id="d00c8-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="d00c8-117">Un ejemplo rápido de cómo preprocesadores pueden mejorar la legibilidad y facilidad de mantenimiento de información de estilo, tenga en cuenta este CSS:</span><span class="sxs-lookup"><span data-stu-id="d00c8-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="d00c8-118">Con menor, esto se puede reescribir para eliminar la duplicación, todos con un *mixin* (denominados así porque permite "¡mix" propiedades de una clase o conjunto de reglas a otro):</span><span class="sxs-lookup"><span data-stu-id="d00c8-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="d00c8-119">menos</span><span class="sxs-lookup"><span data-stu-id="d00c8-119">Less</span></span>

<span data-ttu-id="d00c8-120">El preprocesador de CSS menos se ejecuta mediante Node.js.</span><span class="sxs-lookup"><span data-stu-id="d00c8-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="d00c8-121">Para instalar menor, usar Node Package Manager (npm) desde un símbolo del sistema (-g significa "global"):</span><span class="sxs-lookup"><span data-stu-id="d00c8-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="d00c8-122">Si usa Visual Studio, puede empezar a trabajar con menos agregando uno o más archivos Less al proyecto y, a continuación, configurando Gulp (o Grunt) para procesarlos en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d00c8-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="d00c8-123">Agregar un *estilos* carpeta al proyecto y, a continuación, agregue un nuevo archivo denominado Less *main.less* a esta carpeta.</span><span class="sxs-lookup"><span data-stu-id="d00c8-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Agregar archivo less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="d00c8-125">Una vez agregado, la estructura de carpetas debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d00c8-125">Once added, your folder structure should look something like this:</span></span>

![Estructura de carpetas](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="d00c8-127">Ahora puede agregar algún estilo básico para el archivo, que se compila en CSS y se implementan en la carpeta wwwroot por Gulp.</span><span class="sxs-lookup"><span data-stu-id="d00c8-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="d00c8-128">Modificar *main.less* para incluir el contenido siguiente, que crea una paleta de colores simple desde un único color base.</span><span class="sxs-lookup"><span data-stu-id="d00c8-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="d00c8-129">`@base` y el otro @-prefixed elementos son variables.</span><span class="sxs-lookup"><span data-stu-id="d00c8-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="d00c8-130">Cada una de ellas representa un color.</span><span class="sxs-lookup"><span data-stu-id="d00c8-130">Each of them represents a color.</span></span> <span data-ttu-id="d00c8-131">Excepto para `@base`, establecerlos mediante funciones de color: aclarar, oscurecer y rotación.</span><span class="sxs-lookup"><span data-stu-id="d00c8-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="d00c8-132">Aclarar y oscurecer hacer prácticamente todo lo que cabría esperar; número ajusta el tono de color por un número de grados (alrededor de la rueda de colores).</span><span class="sxs-lookup"><span data-stu-id="d00c8-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="d00c8-133">El procesador de menos es lo suficientemente inteligente como para ignorar las variables que no se utilizan, necesitamos para demostrar cómo funcionan estas variables usarlos en algún lugar.</span><span class="sxs-lookup"><span data-stu-id="d00c8-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="d00c8-134">Las clases `.baseColor`, etc. mostrará los valores calculados de cada una de las variables en el archivo CSS que se genera.</span><span class="sxs-lookup"><span data-stu-id="d00c8-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="d00c8-135">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="d00c8-135">Get started</span></span>

<span data-ttu-id="d00c8-136">Crear un **archivo de configuración de npm** (*package.json*) en la carpeta del proyecto y editarlo para hacer referencia a `gulp` y `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="d00c8-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="d00c8-137">Instalar las dependencias ya sea en un símbolo del sistema en la carpeta del proyecto o en Visual Studio **el Explorador de soluciones** (**dependencias > npm > Restaurar paquetes**).</span><span class="sxs-lookup"><span data-stu-id="d00c8-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Restauración de paquetes de VS](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="d00c8-139">En la carpeta del proyecto, cree un **archivo de configuración de Gulp** (*gulpfile.js*) para definir el proceso automatizado.</span><span class="sxs-lookup"><span data-stu-id="d00c8-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="d00c8-140">Agregue una variable en la parte superior del archivo para representar menos y una tarea se ejecute menor:</span><span class="sxs-lookup"><span data-stu-id="d00c8-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d00c8-141">Abra el **Task Runner Explorer** (**Vista > otras Windows > Explorador de ejecutores de tareas**).</span><span class="sxs-lookup"><span data-stu-id="d00c8-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="d00c8-142">Entre las tareas, verá una nueva tarea denominada `less`.</span><span class="sxs-lookup"><span data-stu-id="d00c8-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="d00c8-143">Es posible que deba actualizar la ventana.</span><span class="sxs-lookup"><span data-stu-id="d00c8-143">You might have to refresh the window.</span></span>

<span data-ttu-id="d00c8-144">Ejecute el `less` tarea y ver un resultado similar al que se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="d00c8-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![menos ejecutor de tareas.](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="d00c8-146">El *wwwroot/css* carpeta contiene ahora un nuevo archivo, *main.css*:</span><span class="sxs-lookup"><span data-stu-id="d00c8-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principal creado](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="d00c8-148">Abra *main.css* y verá algo parecido a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d00c8-148">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="d00c8-149">Agregar una página HTML sencilla para la *wwwroot* carpeta y referencia *main.css* para ver la paleta de colores en acción.</span><span class="sxs-lookup"><span data-stu-id="d00c8-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="d00c8-150">Puede ver que ponga el 180 grados en `@base` utilizado para generar `@background` dieron opuestas color de la rueda de colores `@base`:</span><span class="sxs-lookup"><span data-stu-id="d00c8-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![ejemplo menos prueba](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="d00c8-152">Menor también proporciona compatibilidad para las reglas anidadas, así como las consultas de medios anidados.</span><span class="sxs-lookup"><span data-stu-id="d00c8-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="d00c8-153">Por ejemplo, definir las jerarquías anidadas, como menús pueden dar lugar a reglas CSS detalladas como estos:</span><span class="sxs-lookup"><span data-stu-id="d00c8-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="d00c8-154">Lo ideal es que todas las reglas de estilo relacionados se colocarán juntos en el archivo CSS, pero en la práctica que no hay nada aplicar esta regla, excepto la convención y quizás los comentarios del bloque.</span><span class="sxs-lookup"><span data-stu-id="d00c8-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="d00c8-155">Definición de estas reglas con menor tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="d00c8-155">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="d00c8-156">Tenga en cuenta que en este caso, todos los elementos subordinados de `nav` están dentro de su ámbito.</span><span class="sxs-lookup"><span data-stu-id="d00c8-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="d00c8-157">Ya no hay ninguna repetición de elementos primarios (`nav`, `li`, `a`), y también se ha reducido el recuento total de línea (aunque algunas de que es el resultado de colocar los valores en las mismas líneas en el segundo ejemplo).</span><span class="sxs-lookup"><span data-stu-id="d00c8-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="d00c8-158">Puede ser muy útil, una organización ver todas las reglas para un elemento determinado de la interfaz de usuario dentro de un ámbito delimitado explícitamente, en este caso activados del resto del archivo por llaves.</span><span class="sxs-lookup"><span data-stu-id="d00c8-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="d00c8-159">El `&` sintaxis es una característica menos selector, con & que representa el elemento primario del selector actual.</span><span class="sxs-lookup"><span data-stu-id="d00c8-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="d00c8-160">Es así, dentro de la una {...}</span><span class="sxs-lookup"><span data-stu-id="d00c8-160">So, within the a {...}</span></span> <span data-ttu-id="d00c8-161">bloque, `&` representa un `a` etiqueta y, por tanto, `&:link` es equivalente a `a:link`.</span><span class="sxs-lookup"><span data-stu-id="d00c8-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="d00c8-162">Las consultas de medios, muy útiles para crear diseños con capacidad de respuesta, también pueden contribuir considerablemente a repeticiones y la complejidad en CSS.</span><span class="sxs-lookup"><span data-stu-id="d00c8-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="d00c8-163">Menor permite que las consultas de medios se anida dentro de las clases, por lo que no necesita la definición de clase completa se repite en diferentes nivel superior `@media` elementos.</span><span class="sxs-lookup"><span data-stu-id="d00c8-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="d00c8-164">Por ejemplo, aquí es CSS para un menú con capacidad de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d00c8-164">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d00c8-165">Esto se puede definir mejor en menos como:</span><span class="sxs-lookup"><span data-stu-id="d00c8-165">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d00c8-166">Otra característica de menos que ya hemos visto es su compatibilidad para operaciones matemáticas, lo que permite a los atributos de estilo se construyera a partir de variables predefinidas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="d00c8-167">Esto facilita la actualización relacionados estilos mucho más fáciles, ya que se puede modificar la variable de base y todos los valores dependientes cambian automáticamente.</span><span class="sxs-lookup"><span data-stu-id="d00c8-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="d00c8-168">Los archivos CSS, especialmente para los sitios de gran tamaño (y especialmente si se utilizan las consultas de medios), tienden a aumentar bastante grande con el tiempo, lo difícil trabajar con ellas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="d00c8-169">Archivos less pueden definirse por separado, a continuación, extrae entre sí mediante `@import` directivas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="d00c8-170">También se puede usar menor para importar CSS archivos individuales, también, si lo desea.</span><span class="sxs-lookup"><span data-stu-id="d00c8-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="d00c8-171">*Mixins* puede aceptar parámetros, y menor es compatible con lógica condicional en forma de guardias mixin, que proporcionan una manera declarativa para definir cuándo determinados mixins surten efecto.</span><span class="sxs-lookup"><span data-stu-id="d00c8-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="d00c8-172">Un uso común para las restricciones de mixin es ajustar los colores en función de cómo la luz u oscuro el color de origen.</span><span class="sxs-lookup"><span data-stu-id="d00c8-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="d00c8-173">Dado un mixin que acepta un parámetro para el color, un guardia mixin puede usarse para modificar el mixin según ese color:</span><span class="sxs-lookup"><span data-stu-id="d00c8-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="d00c8-174">Según nuestro actual `@base` valor de `#663333`, esta secuencia de comandos menos generará el código CSS siguiente:</span><span class="sxs-lookup"><span data-stu-id="d00c8-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="d00c8-175">Menor proporciona una serie de características adicionales, pero esto debería proporcionarle una idea de la eficacia de este lenguaje de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="d00c8-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="d00c8-176">SASS</span><span class="sxs-lookup"><span data-stu-id="d00c8-176">Sass</span></span>

<span data-ttu-id="d00c8-177">SASS es similar a un valor inferior, que proporciona compatibilidad con muchas de las mismas características, pero con una sintaxis ligeramente diferente.</span><span class="sxs-lookup"><span data-stu-id="d00c8-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="d00c8-178">Se genera mediante Ruby, en lugar de JavaScript y, por lo que tiene requisitos de configuración diferentes.</span><span class="sxs-lookup"><span data-stu-id="d00c8-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="d00c8-179">El lenguaje Sass original no usa llaves o punto y coma, sino que define el ámbito mediante espacios en blanco y sangría.</span><span class="sxs-lookup"><span data-stu-id="d00c8-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="d00c8-180">En la versión 3 de Sass, se introdujo una nueva sintaxis, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="d00c8-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="d00c8-181">SCSS es similar a CSS, ya que omite los espacios en blanco y los niveles de sangría y usa en su lugar, puntos y comas y llaves.</span><span class="sxs-lookup"><span data-stu-id="d00c8-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="d00c8-182">Para instalar Sass, normalmente se haría instala primero Ruby (preinstalado en Mac OS) y, a continuación, ejecute:</span><span class="sxs-lookup"><span data-stu-id="d00c8-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="d00c8-183">Sin embargo, si ejecuta Visual Studio, puede comenzar con Sass prácticamente la misma manera como lo haría con menos.</span><span class="sxs-lookup"><span data-stu-id="d00c8-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="d00c8-184">Abra *package.json* y agregar el paquete "gulp-sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="d00c8-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="d00c8-185">A continuación, modifique *gulpfile.js* para agregar una variable sass y una tarea para compilar los archivos Sass y colocar los resultados en la carpeta wwwroot:</span><span class="sxs-lookup"><span data-stu-id="d00c8-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d00c8-186">Ahora puede agregar el archivo Sass *main2.scss* a la *estilos* carpeta en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="d00c8-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Agregar archivo de scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="d00c8-188">Abra *main2.scss* y agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d00c8-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="d00c8-189">Guarde todos los archivos.</span><span class="sxs-lookup"><span data-stu-id="d00c8-189">Save all of your files.</span></span> <span data-ttu-id="d00c8-190">Ahora, cuando actualice **Task Runner Explorer**, verá un `sass` tarea.</span><span class="sxs-lookup"><span data-stu-id="d00c8-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="d00c8-191">Ejecútelo y buscar en el */wwwroot/css* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d00c8-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="d00c8-192">Ahora hay un *main2.css* archivo con este contenido:</span><span class="sxs-lookup"><span data-stu-id="d00c8-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="d00c8-193">SASS admite el anidamiento prácticamente lo mismo era que menor no, lo que proporciona ventajas similares.</span><span class="sxs-lookup"><span data-stu-id="d00c8-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="d00c8-194">Los archivos pueden dividirse por función e incluido mediante el `@import` directiva:</span><span class="sxs-lookup"><span data-stu-id="d00c8-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="d00c8-195">SASS admite el uso de mixins así, la `@mixin` palabra clave para definirlos y `@include` para incluirlos, como en este ejemplo de [sass-lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="d00c8-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="d00c8-196">Además de mixins, Sass también admite el concepto de herencia, lo que permite una clase puede ampliar otra.</span><span class="sxs-lookup"><span data-stu-id="d00c8-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="d00c8-197">Es conceptualmente similar a un mixin, aunque los resultados en menos código CSS.</span><span class="sxs-lookup"><span data-stu-id="d00c8-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="d00c8-198">Se logra mediante el `@extend` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="d00c8-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="d00c8-199">Para probar mixins, agregue lo siguiente a su *main2.scss* archivo:</span><span class="sxs-lookup"><span data-stu-id="d00c8-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d00c8-200">Examine la salida en *main2.css* después de ejecutar el `sass` de tareas en **Task Runner Explorer**:</span><span class="sxs-lookup"><span data-stu-id="d00c8-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d00c8-201">Tenga en cuenta que todas las propiedades comunes de la alerta mixin se repiten en cada clase.</span><span class="sxs-lookup"><span data-stu-id="d00c8-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="d00c8-202">El mixin hizo un buen trabajo para ayudar a eliminar la duplicación en tiempo de desarrollo, pero todavía se está creando CSS con una gran cantidad de duplicación, lo que produce mayores que los archivos necesarios de CSS - un posible problema de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d00c8-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="d00c8-203">Ahora, reemplace el mixin de alerta con un `.alert` clase y cambiar `@include` a `@extend` (teniendo en cuenta al ampliar `.alert`, no `alert`):</span><span class="sxs-lookup"><span data-stu-id="d00c8-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d00c8-204">Ejecute una vez más Sass y examine el CSS resultante:</span><span class="sxs-lookup"><span data-stu-id="d00c8-204">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d00c8-205">Ahora las propiedades se definen sólo como tantas veces como sea necesario y mejor se genera el CSS.</span><span class="sxs-lookup"><span data-stu-id="d00c8-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="d00c8-206">SASS también incluye funciones y operaciones de lógica condicional, similares a menor.</span><span class="sxs-lookup"><span data-stu-id="d00c8-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="d00c8-207">De hecho, las capacidades de los dos lenguajes son muy similares.</span><span class="sxs-lookup"><span data-stu-id="d00c8-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="d00c8-208">¿Menor o Sass?</span><span class="sxs-lookup"><span data-stu-id="d00c8-208">Less or Sass?</span></span>

<span data-ttu-id="d00c8-209">Todavía no hay ningún consenso sobre si es suele ser mejor usar una cantidad menor o Sass (o incluso si se prefiere el Sass original o la sintaxis SCSS más reciente en Sass).</span><span class="sxs-lookup"><span data-stu-id="d00c8-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="d00c8-210">Probablemente la decisión más importante es **utilizar una de estas herramientas**en lugar de simplemente-codificación manual los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="d00c8-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="d00c8-211">Una vez que haya realizado que la decisión, ambos Less y Sass son opciones válidas.</span><span class="sxs-lookup"><span data-stu-id="d00c8-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="d00c8-212">Font Awesome</span><span class="sxs-lookup"><span data-stu-id="d00c8-212">Font Awesome</span></span>

<span data-ttu-id="d00c8-213">Además de los preprocesadores CSS, otro excelente recurso para las aplicaciones web modernas de estilo es Font Awesome.</span><span class="sxs-lookup"><span data-stu-id="d00c8-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="d00c8-214">Font Awesome es un Kit de herramientas que ofrece más de 500 iconos vectoriales escalables que pueden usarse libremente en sus aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="d00c8-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="d00c8-215">Se diseñó originalmente para trabajar con Bootstrap, pero no tiene dependencias en ese marco de trabajo o en las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d00c8-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="d00c8-216">La manera más fácil empezar a trabajar con Font Awesome consiste en Agregar una referencia a él, mediante su ubicación de entrega de contenido público network (CDN):</span><span class="sxs-lookup"><span data-stu-id="d00c8-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="d00c8-217">También puede agregarlo al proyecto de Visual Studio, éste se agrega a la sección "dependencias" en *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="d00c8-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="d00c8-218">Una vez que tenga una referencia a Font Awesome en una página, puede agregar iconos a la aplicación aplicando clases Font Awesome, normalmente el prefijo "fa-", para los elementos HTML en línea (como `<span>` o `<i>`).</span><span class="sxs-lookup"><span data-stu-id="d00c8-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="d00c8-219">Por ejemplo, puede agregar iconos para listas simples y los menús con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d00c8-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="d00c8-220">Esto produce lo siguiente en el explorador: observe el icono junto a cada elemento:</span><span class="sxs-lookup"><span data-stu-id="d00c8-220">This produces the following in the browser - note the icon beside each item:</span></span>

![iconos de la lista](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="d00c8-222">Puede ver una lista completa de los iconos disponibles aquí:</span><span class="sxs-lookup"><span data-stu-id="d00c8-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="d00c8-223">Resumen</span><span class="sxs-lookup"><span data-stu-id="d00c8-223">Summary</span></span>

<span data-ttu-id="d00c8-224">Aplicaciones web modernas demandan cada vez más fluidos, con capacidad de respuesta diseños que se limpia, intuitiva y fácil de usar desde una variedad de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="d00c8-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="d00c8-225">Administrar la complejidad de las hojas de estilo CSS necesarios para lograr estos objetivos se realiza mejor mediante un me gusta preprocesador menos o Sass.</span><span class="sxs-lookup"><span data-stu-id="d00c8-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="d00c8-226">Además, kits de herramientas, como Font Awesome rápidamente proporcionan iconos conocidos para los menús de navegación textual y experimentan de los botones, mejorar la global del usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d00c8-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
