---
ms.openlocfilehash: 1f36b95063094e891c1e6b44fbf4ee7546853d89
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050462"
---
# <a name="contributing-to-the-jquery-validation-plugin"></a><span data-ttu-id="65631-101">Contribución al complemento de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="65631-101">Contributing to the jQuery Validation Plugin</span></span>

## <a name="reporting-an-issue"></a><span data-ttu-id="65631-102">Notificación de un problema</span><span class="sxs-lookup"><span data-stu-id="65631-102">Reporting an Issue</span></span>

1. <span data-ttu-id="65631-103">Asegúrese de que el problema que intenta solucionar es reproducible.</span><span class="sxs-lookup"><span data-stu-id="65631-103">Make sure the problem you're addressing is reproducible.</span></span>
2. <span data-ttu-id="65631-104">Use http://jsbin.com o http://jsfiddle.net para proporcionar una página de prueba.</span><span class="sxs-lookup"><span data-stu-id="65631-104">Use http://jsbin.com or http://jsfiddle.net to provide a test page.</span></span>
3. <span data-ttu-id="65631-105">Indique en qué exploradores se puede reproducir el problema.</span><span class="sxs-lookup"><span data-stu-id="65631-105">Indicate what browsers the issue can be reproduced in.</span></span> <span data-ttu-id="65631-106">**Nota: No se resolverá problemas del modo de compatibilidad de IE. Asegúrese de que realiza la prueba en un explorador real.**</span><span class="sxs-lookup"><span data-stu-id="65631-106">**Note: IE Compatibilty mode issues will not be addressed. Make sure you test in a real browser!**</span></span>
4. <span data-ttu-id="65631-107">Indique en qué versión del complemento se puede reproducir el problema.</span><span class="sxs-lookup"><span data-stu-id="65631-107">What version of the plug-in is the issue reproducible in.</span></span> <span data-ttu-id="65631-108">Señale también si se puede reproducir después de actualizar a la última versión.</span><span class="sxs-lookup"><span data-stu-id="65631-108">Is it reproducible after updating to the latest version.</span></span>

<span data-ttu-id="65631-109">También se realiza un seguimiento de los problemas de la documentación en el controlador de problemas de [validación de jQuery](https://github.com/jzaefferer/jquery-validation/issues).</span><span class="sxs-lookup"><span data-stu-id="65631-109">Documentation issues are also tracked at the [jQuery Validation](https://github.com/jzaefferer/jquery-validation/issues) issue tracker.</span></span>
<span data-ttu-id="65631-110">Se aceptan las solicitudes de incorporación de cambios para mejorar los documentos en el repositorio de [documentos de validación de jQuery](https://github.com/jzaefferer/validation-content).</span><span class="sxs-lookup"><span data-stu-id="65631-110">Pull Requests to improve the docs are welcome at the [jQuery Validation docs](https://github.com/jzaefferer/validation-content) repository, though.</span></span>

<span data-ttu-id="65631-111">**NOTA IMPORTANTE SOBRE LA VALIDACIÓN DE CORREO ELECTRÓNICO**.</span><span class="sxs-lookup"><span data-stu-id="65631-111">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="65631-112">A partir de la versión 1.12.0, este complemento usa la misma expresión regular que la [especificación HTML5 sugiere para que los exploradores la usen](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="65631-112">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="65631-113">Seguiremos el mismo ejemplo y usaremos la misma comprobación.</span><span class="sxs-lookup"><span data-stu-id="65631-113">We will follow their lead and use the same check.</span></span> <span data-ttu-id="65631-114">Si cree que la especificación es incorrecta, notifíqueles el problema.</span><span class="sxs-lookup"><span data-stu-id="65631-114">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="65631-115">Si sus requisitos son distintos, considere la opción de [usar un método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="65631-115">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="contributing-code"></a><span data-ttu-id="65631-116">Código de contribución</span><span class="sxs-lookup"><span data-stu-id="65631-116">Contributing code</span></span>

<span data-ttu-id="65631-117">Gracias por contribuir.</span><span class="sxs-lookup"><span data-stu-id="65631-117">Thanks for contributing!</span></span> <span data-ttu-id="65631-118">A continuación, se detallan algunas instrucciones para ayudarle a que su contribución quede reflejada.</span><span class="sxs-lookup"><span data-stu-id="65631-118">Here's a few guidelines to help your contribution get landed.</span></span>

1. <span data-ttu-id="65631-119">Asegúrese de que el problema que intenta solucionar es reproducible.</span><span class="sxs-lookup"><span data-stu-id="65631-119">Make sure the problem you're addressing is reproducible.</span></span> <span data-ttu-id="65631-120">Use jsbin.com o jsfiddle.net para proporcionar una página de prueba.</span><span class="sxs-lookup"><span data-stu-id="65631-120">Use jsbin.com or jsfiddle.net to provide a test page.</span></span>
2. <span data-ttu-id="65631-121">Siga la [guía de estilo de jQuery](http://contribute.jquery.com/style-guides/js).</span><span class="sxs-lookup"><span data-stu-id="65631-121">Follow the [jQuery style guide](http://contribute.jquery.com/style-guides/js)</span></span>
3. <span data-ttu-id="65631-122">Agregue o actualice pruebas unitarias junto con la revisión.</span><span class="sxs-lookup"><span data-stu-id="65631-122">Add or update unit tests along with your patch.</span></span> <span data-ttu-id="65631-123">Ejecute las pruebas unitarias al menos en un explorador (vea la información correspondiente más abajo).</span><span class="sxs-lookup"><span data-stu-id="65631-123">Run the unit tests in at least one browser (see below).</span></span>
4. <span data-ttu-id="65631-124">Ejecute `grunt` (vea la información correspondiente más abajo) para buscar errores y otros problemas.</span><span class="sxs-lookup"><span data-stu-id="65631-124">Run `grunt` (see below) to check for linting and a few other issues.</span></span>
5. <span data-ttu-id="65631-125">Describa el cambio en el mensaje de confirmación y haga referencia al vale, similar al siguiente: "Demostraciones: Error de delegado corregido para demostración de totales dinámicos.</span><span class="sxs-lookup"><span data-stu-id="65631-125">Describe the change in your commit message and reference the ticket, like this: "Demos: Fixed delegate bug for dynamic-totals demo.</span></span> <span data-ttu-id="65631-126">Corrige #51".</span><span class="sxs-lookup"><span data-stu-id="65631-126">Fixes #51".</span></span> <span data-ttu-id="65631-127">Si va a agregar un nuevo archivo de localización, use algo parecido a esto: "Localización: Localización de agregado croata (HR)"</span><span class="sxs-lookup"><span data-stu-id="65631-127">If you're adding a new localization file, use something like this: "Localization: Added croatian (HR) localization"</span></span>

## <a name="build-setup"></a><span data-ttu-id="65631-128">Configuración de la compilación</span><span class="sxs-lookup"><span data-stu-id="65631-128">Build setup</span></span>

1. <span data-ttu-id="65631-129">Instale [NodeJS](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="65631-129">Install [NodeJS](http://nodejs.org).</span></span>
2. <span data-ttu-id="65631-130">Instale el script Grunt CLI To install mediante la ejecución de `npm install -g grunt-cli`.</span><span class="sxs-lookup"><span data-stu-id="65631-130">Install the Grunt CLI To install by running `npm install -g grunt-cli`.</span></span> <span data-ttu-id="65631-131">Encontrará más detalles en su sitio web, http://gruntjs.com/getting-started.</span><span class="sxs-lookup"><span data-stu-id="65631-131">More details are available on their website http://gruntjs.com/getting-started.</span></span>
3. <span data-ttu-id="65631-132">Instale las dependencias de NPM mediante la ejecución de `npm install`.</span><span class="sxs-lookup"><span data-stu-id="65631-132">Install the NPM dependencies by running `npm install`.</span></span>
4. <span data-ttu-id="65631-133">Ahora puede llamar a la compilación mediante la ejecución de `grunt`.</span><span class="sxs-lookup"><span data-stu-id="65631-133">The build can now be called by running `grunt`.</span></span>

## <a name="creating-a-new-additional-method"></a><span data-ttu-id="65631-134">Creación de un método adicional</span><span class="sxs-lookup"><span data-stu-id="65631-134">Creating a new Additional Method</span></span>

<span data-ttu-id="65631-135">Si escribió métodos personalizados con los que desea contribuir a additional-methods.js:</span><span class="sxs-lookup"><span data-stu-id="65631-135">If you've wrote custom methods that you'd like to contribute to additional-methods.js:</span></span>

1. <span data-ttu-id="65631-136">Crear una bifurcación</span><span class="sxs-lookup"><span data-stu-id="65631-136">Create a branch</span></span>
2. <span data-ttu-id="65631-137">Agregue el método como un archivo nuevo en `src/additional`.</span><span class="sxs-lookup"><span data-stu-id="65631-137">Add the method as a new file in `src/additional`</span></span>
3. <span data-ttu-id="65631-138">(Opcional) Agregue traducciones a `src/localization`.</span><span class="sxs-lookup"><span data-stu-id="65631-138">(Optional) Add translations to `src/localization`</span></span>
4. <span data-ttu-id="65631-139">Envíe una solicitud de incorporación de cambios a la bifurcación principal.</span><span class="sxs-lookup"><span data-stu-id="65631-139">Send a pull request to the master branch.</span></span>

## <a name="unit-tests"></a><span data-ttu-id="65631-140">Pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="65631-140">Unit Tests</span></span>

<span data-ttu-id="65631-141">Para ejecutar pruebas unitarias, solo tiene que abrir `test/index.html` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="65631-141">To run unit tests, just open `test/index.html` within your browser.</span></span> <span data-ttu-id="65631-142">Asegúrese de que ejecutó `npm install` antes de que todas las dependencias requeridas estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="65631-142">Make sure you ran `npm install` before so all required dependencies are available.</span></span>
<span data-ttu-id="65631-143">Comience con un explorador mientras desarrolla la corrección y después, ejecútela en otros antes de confirmar.</span><span class="sxs-lookup"><span data-stu-id="65631-143">Start with one browser while developing the fix, then run against others before committing.</span></span> <span data-ttu-id="65631-144">Normalmente, se usan las últimas versiones de Chrome, Firefox, Safari y Opera y, con menos frecuencia, IE.</span><span class="sxs-lookup"><span data-stu-id="65631-144">Usually latest Chrome, Firefox, Safari and Opera and a few IEs.</span></span>

## <a name="documentation"></a><span data-ttu-id="65631-145">Documentación</span><span class="sxs-lookup"><span data-stu-id="65631-145">Documentation</span></span>

<span data-ttu-id="65631-146">Notifique los problemas de documentación en el controlador de problemas de [validación de jQuery](https://github.com/jzaefferer/jquery-validation/issues).</span><span class="sxs-lookup"><span data-stu-id="65631-146">Please report documentation issues at the [jQuery Validation](https://github.com/jzaefferer/jquery-validation/issues) issue tracker.</span></span>
<span data-ttu-id="65631-147">En caso de que la solicitud de incorporación de cambios implemente o cambie la API pública, sería importante que realice una solicitud de incorporación de cambios en el repositorio de [documentos de validación de jQuery](https://github.com/jzaefferer/validation-content).</span><span class="sxs-lookup"><span data-stu-id="65631-147">In case your pull request implements or changes public API it would be a plus you would provide a pull request against the [jQuery Validation docs](https://github.com/jzaefferer/validation-content) repository.</span></span>

## <a name="linting"></a><span data-ttu-id="65631-148">Detección de errores</span><span class="sxs-lookup"><span data-stu-id="65631-148">Linting</span></span>

<span data-ttu-id="65631-149">Para ejecutar JSHint y otras herramientas, use `grunt`.</span><span class="sxs-lookup"><span data-stu-id="65631-149">To run JSHint and other tools, use `grunt`.</span></span>