---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)'
author: rick-anderson
description: En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos especificando reglas de formato, validación y asignación.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 1dc9f1278e502cd5040e82c18d99e2da6f139568
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052812"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="2cda8-103">Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)</span><span class="sxs-lookup"><span data-stu-id="2cda8-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2cda8-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2cda8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="2cda8-105">En los tutoriales anteriores se trabajaba con un modelo de datos básico que se componía de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="2cda8-106">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="2cda8-106">In this tutorial:</span></span>

* <span data-ttu-id="2cda8-107">Se agregan más entidades y relaciones.</span><span class="sxs-lookup"><span data-stu-id="2cda8-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="2cda8-108">Se personaliza el modelo de datos especificando el formato, la validación y las reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="2cda8-109">Las clases de entidad para el modelo de datos completo se muestran en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="2cda8-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

<span data-ttu-id="2cda8-111">Si experimenta problemas que no puede resolver, descargue la [aplicación completada](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="2cda8-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="2cda8-112">Personalizar el modelo de datos con atributos</span><span class="sxs-lookup"><span data-stu-id="2cda8-112">Customize the data model with attributes</span></span>

<span data-ttu-id="2cda8-113">En esta sección, se personaliza el modelo de datos mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="2cda8-114">El atributo DataType</span><span class="sxs-lookup"><span data-stu-id="2cda8-114">The DataType attribute</span></span>

<span data-ttu-id="2cda8-115">Las páginas de alumno actualmente muestran la hora de la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="2cda8-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="2cda8-116">Normalmente, los campos de fecha muestran solo la fecha y no la hora.</span><span class="sxs-lookup"><span data-stu-id="2cda8-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="2cda8-117">Actualice *Models/Student.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="2cda8-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="2cda8-118">El atributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) especifica un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="2cda8-119">En este caso solo se debe mostrar la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="2cda8-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="2cda8-120">La [enumeración DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona muchos tipos de datos, como Date (Fecha), Time (Hora), PhoneNumber (Número de teléfono), Currency (Divisa), EmailAddress (Dirección de correo electrónico), etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="2cda8-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="2cda8-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2cda8-121">For example:</span></span>

* <span data-ttu-id="2cda8-122">El vínculo `mailto:` se crea automáticamente para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="2cda8-123">El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="2cda8-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="2cda8-124">El atributo `DataType` emite atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="2cda8-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="2cda8-125">Los atributos `DataType` no proporcionan validación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="2cda8-126">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="2cda8-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="2cda8-127">De manera predeterminada, el campo de fecha se muestra según los formatos predeterminados basados en el elemento [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del servidor.</span><span class="sxs-lookup"><span data-stu-id="2cda8-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="2cda8-128">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="2cda8-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="2cda8-129">La configuración `ApplyFormatInEditMode` especifica que el formato también debe aplicarse a la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="2cda8-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="2cda8-130">Algunos campos no deben usar `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="2cda8-131">Por ejemplo, el símbolo de divisa generalmente no debe mostrarse en un cuadro de texto de edición.</span><span class="sxs-lookup"><span data-stu-id="2cda8-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="2cda8-132">El atributo `DisplayFormat` puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="2cda8-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="2cda8-133">Normalmente se recomienda usar el atributo `DataType` con el atributo `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="2cda8-134">El atributo `DataType` transmite la semántica de los datos en lugar de cómo se representan en una pantalla.</span><span class="sxs-lookup"><span data-stu-id="2cda8-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="2cda8-135">El atributo `DataType` proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="2cda8-136">El explorador puede habilitar características de HTML5.</span><span class="sxs-lookup"><span data-stu-id="2cda8-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="2cda8-137">Por ejemplo, mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico, validación de entradas del lado cliente, etc.</span><span class="sxs-lookup"><span data-stu-id="2cda8-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="2cda8-138">De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="2cda8-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="2cda8-139">Para obtener más información, vea la [documentación del asistente de etiquetas \<entrada&gt;](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2cda8-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="2cda8-140">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-140">Run the app.</span></span> <span data-ttu-id="2cda8-141">Vaya a la página de índice de Students.</span><span class="sxs-lookup"><span data-stu-id="2cda8-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="2cda8-142">Ya no se muestran las horas.</span><span class="sxs-lookup"><span data-stu-id="2cda8-142">Times are no longer displayed.</span></span> <span data-ttu-id="2cda8-143">Todas las vistas que usa el modelo `Student` muestran la fecha sin hora.</span><span class="sxs-lookup"><span data-stu-id="2cda8-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Página de índice de estudiantes en la que se muestran las fechas sin horas](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="2cda8-145">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="2cda8-145">The StringLength attribute</span></span>

<span data-ttu-id="2cda8-146">Las reglas de validación de datos y los mensajes de error de validación se pueden especificar con atributos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="2cda8-147">El atributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) especifica la longitud mínima y máxima de caracteres que se permite en un campo de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="2cda8-148">El atributo `StringLength` también proporciona validación del lado cliente y del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="2cda8-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="2cda8-149">El valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="2cda8-150">Actualice el modelo `Student` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="2cda8-151">El código anterior limita los nombres a no más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="2cda8-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="2cda8-152">El atributo `StringLength` no impide que un usuario escriba un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="2cda8-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="2cda8-153">El atributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) se usa para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="2cda8-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="2cda8-154">Por ejemplo, el código siguiente requiere que el primer carácter sea una letra mayúscula y el resto de caracteres sean alfabéticos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="2cda8-155">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2cda8-155">Run the app:</span></span>

* <span data-ttu-id="2cda8-156">Vaya a la página Students.</span><span class="sxs-lookup"><span data-stu-id="2cda8-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="2cda8-157">Seleccione **Create New** y escriba un nombre más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="2cda8-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="2cda8-158">Seleccione **Create**, la validación del lado cliente muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="2cda8-158">Select **Create**, client-side validation shows an error message.</span></span>

![Página de índice de estudiantes en la que se muestran errores de longitud de cadena](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="2cda8-160">En el **Explorador de objetos de SQL Server**, (SSOX) abra el diseñador de tablas de Student haciendo doble clic en la tabla **Student**.</span><span class="sxs-lookup"><span data-stu-id="2cda8-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="2cda8-162">La imagen anterior muestra el esquema para la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="2cda8-163">Los campos de nombre tienen tipo `nvarchar(MAX)` porque las migraciones no se han ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="2cda8-164">Cuando se ejecutan las migraciones más adelante en este tutorial, los campos de nombre se convierten en `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="2cda8-165">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="2cda8-165">The Column attribute</span></span>

<span data-ttu-id="2cda8-166">Los atributos pueden controlar cómo se asignan las clases y propiedades a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="2cda8-167">En esta sección, el atributo `Column` se usa para asignar el nombre de la propiedad `FirstMidName` a "FirstName" en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="2cda8-168">Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando se usa el atributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="2cda8-169">El modelo `Student` usa `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre.</span><span class="sxs-lookup"><span data-stu-id="2cda8-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="2cda8-170">Actualice el archivo *Models/Student.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="2cda8-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="2cda8-171">Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la columna `FirstName` de la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="2cda8-172">La adición del atributo `Column` cambia el modelo de respaldo de `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="2cda8-173">El modelo que está haciendo la copia de seguridad de `SchoolContext` ya no coincide con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="2cda8-174">Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="2cda8-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="2cda8-175">Para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-175">To update the DB:</span></span>

* <span data-ttu-id="2cda8-176">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2cda8-176">Build the project.</span></span>
* <span data-ttu-id="2cda8-177">Abra una ventana de comandos en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2cda8-177">Open a command window in the project folder.</span></span> <span data-ttu-id="2cda8-178">Escriba los comandos siguientes para crear una migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cda8-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cda8-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cda8-180">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cda8-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="2cda8-181">El comando `migrations add ColumnFirstName` genera el siguiente mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="2cda8-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="2cda8-182">La advertencia se genera porque los campos de nombre ahora están limitados a 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="2cda8-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="2cda8-183">Si un nombre en la base de datos tenía más de 50 caracteres, se perderían desde el 51 hasta el último carácter.</span><span class="sxs-lookup"><span data-stu-id="2cda8-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="2cda8-184">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-184">Test the app.</span></span>

<span data-ttu-id="2cda8-185">Abra la tabla de estudiantes en SSOX:</span><span class="sxs-lookup"><span data-stu-id="2cda8-185">Open the Student table in SSOX:</span></span>

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="2cda8-187">Antes de aplicar la migración, las columnas de nombre eran de tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="2cda8-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="2cda8-188">Las columnas de nombre ahora son `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="2cda8-189">El nombre de columna ha cambiado de `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="2cda8-190">En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="2cda8-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="2cda8-191">Las instrucciones especifican cuándo se debe compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="2cda8-192">Actualizar la entidad Student</span><span class="sxs-lookup"><span data-stu-id="2cda8-192">Student entity update</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="2cda8-194">Actualice *Models/Student.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2cda8-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="2cda8-195">El atributo Required</span><span class="sxs-lookup"><span data-stu-id="2cda8-195">The Required attribute</span></span>

<span data-ttu-id="2cda8-196">El atributo `Required` hace que las propiedades de nombre sean campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="2cda8-197">El atributo `Required` no es necesario para los tipos que no aceptan valores NULL, como los tipos de valor (`DateTime`, `int`, `double`, etc.).</span><span class="sxs-lookup"><span data-stu-id="2cda8-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="2cda8-198">Los tipos que no aceptan valores NULL se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="2cda8-199">El atributo `Required` se podría reemplazar con un parámetro de longitud mínima en el atributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="2cda8-200">El atributo Display</span><span class="sxs-lookup"><span data-stu-id="2cda8-200">The Display attribute</span></span>

<span data-ttu-id="2cda8-201">El atributo `Display` especifica que el título de los cuadros de texto debe ser "First Name", "Last Name", "Full Name" y "Enrollment Date".</span><span class="sxs-lookup"><span data-stu-id="2cda8-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="2cda8-202">Los títulos predeterminados no tenían ningún espacio de división de palabras, por ejemplo "Lastname".</span><span class="sxs-lookup"><span data-stu-id="2cda8-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="2cda8-203">La propiedad calculada FullName</span><span class="sxs-lookup"><span data-stu-id="2cda8-203">The FullName calculated property</span></span>

<span data-ttu-id="2cda8-204">`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="2cda8-205">No se puede establecer `FullName`, tiene solo un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="2cda8-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="2cda8-206">No se crea ninguna columna `FullName` en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="2cda8-207">Crear la entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="2cda8-207">Create the Instructor Entity</span></span>

![La entidad Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="2cda8-209">Cree *Models/Instructor.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="2cda8-210">En una sola línea puede haber varios atributos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="2cda8-211">Los atributos `HireDate` pudieron escribirse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="2cda8-212">Las propiedades de navegación CourseAssignments y OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2cda8-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="2cda8-213">`CourseAssignments` y `OfficeAssignment` son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="2cda8-214">Un instructor puede impartir cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="2cda8-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2cda8-215">Si una propiedad de navegación contiene varias entidades:</span><span class="sxs-lookup"><span data-stu-id="2cda8-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="2cda8-216">Debe ser un tipo de lista, donde se pueden agregar, eliminar y actualizar las entradas.</span><span class="sxs-lookup"><span data-stu-id="2cda8-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="2cda8-217">Los tipos de propiedad de navegación incluyen:</span><span class="sxs-lookup"><span data-stu-id="2cda8-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="2cda8-218">Si se especifica `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2cda8-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="2cda8-219">La entidad `CourseAssignment` se explica en la sección sobre las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="2cda8-220">Las reglas de negocio de Contoso University establecen que un instructor puede tener, a lo sumo, una oficina.</span><span class="sxs-lookup"><span data-stu-id="2cda8-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="2cda8-221">La propiedad `OfficeAssignment` contiene una única instancia de `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="2cda8-222">`OfficeAssignment` es NULL si no se asigna ninguna oficina.</span><span class="sxs-lookup"><span data-stu-id="2cda8-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="2cda8-223">Crear la entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2cda8-223">Create the OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="2cda8-225">Cree *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="2cda8-226">El atributo Key</span><span class="sxs-lookup"><span data-stu-id="2cda8-226">The Key attribute</span></span>

<span data-ttu-id="2cda8-227">El atributo `[Key]` se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es diferente de classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="2cda8-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="2cda8-228">Hay una relación de uno a cero o uno entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="2cda8-229">Solo existe una asignación de oficina en relación con el instructor a la que está asignada.</span><span class="sxs-lookup"><span data-stu-id="2cda8-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="2cda8-230">La clave principal de `OfficeAssignment` también es la clave externa (FK) para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="2cda8-231">EF Core no reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:</span><span class="sxs-lookup"><span data-stu-id="2cda8-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="2cda8-232">`InstructorID` no sigue la convención de nomenclatura de ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="2cda8-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="2cda8-233">Por tanto, se usa el atributo `Key` para identificar `InstructorID` como la clave principal:</span><span class="sxs-lookup"><span data-stu-id="2cda8-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="2cda8-234">De forma predeterminada, EF Core trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="2cda8-235">La propiedad de navegación Instructor</span><span class="sxs-lookup"><span data-stu-id="2cda8-235">The Instructor navigation property</span></span>

<span data-ttu-id="2cda8-236">La propiedad de navegación `OfficeAssignment` para la entidad `Instructor` acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="2cda8-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="2cda8-237">Los tipos de referencia, como las clases, aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="2cda8-238">Un instructor podría no tener una asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="2cda8-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="2cda8-239">La entidad `OfficeAssignment` tiene una propiedad de navegación `Instructor` que no acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="2cda8-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="2cda8-240">`InstructorID` no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="2cda8-241">Una asignación de oficina no puede existir sin un instructor.</span><span class="sxs-lookup"><span data-stu-id="2cda8-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="2cda8-242">Cuando una entidad `Instructor` tiene una entidad `OfficeAssignment` relacionada, cada entidad tiene una referencia a la otra en su propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="2cda8-243">El atributo `[Required]` puede aplicarse a la propiedad de navegación `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="2cda8-244">El código anterior especifica que debe haber un instructor relacionado.</span><span class="sxs-lookup"><span data-stu-id="2cda8-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="2cda8-245">El código anterior no es necesario porque la clave externa `InstructorID`, que también es la clave principal, no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="2cda8-246">Modificar la entidad Course</span><span class="sxs-lookup"><span data-stu-id="2cda8-246">Modify the Course Entity</span></span>

![La entidad Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="2cda8-248">Actualice *Models/Course.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2cda8-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="2cda8-249">La entidad `Course` tiene una propiedad de clave externa (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="2cda8-250">`DepartmentID` apunta a la entidad relacionada `Department`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="2cda8-251">La entidad `Course` tiene una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="2cda8-252">EF Core no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="2cda8-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="2cda8-253">EF Core crea automáticamente claves externas en la base de datos siempre que se necesiten.</span><span class="sxs-lookup"><span data-stu-id="2cda8-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="2cda8-254">EF Core crea [propiedades paralelas](/ef/core/modeling/shadow-properties) para las claves externas creadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="2cda8-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="2cda8-255">Tener la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces.</span><span class="sxs-lookup"><span data-stu-id="2cda8-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="2cda8-256">Por ejemplo, considere la posibilidad de un modelo donde la propiedad de la clave externa `DepartmentID` *no* está incluida.</span><span class="sxs-lookup"><span data-stu-id="2cda8-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="2cda8-257">Cuando se captura una entidad de curso para editar:</span><span class="sxs-lookup"><span data-stu-id="2cda8-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="2cda8-258">La entidad `Department` es NULL si no se carga explícitamente.</span><span class="sxs-lookup"><span data-stu-id="2cda8-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="2cda8-259">Para actualizar la entidad Course, la entidad `Department` debe capturarse en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="2cda8-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="2cda8-260">Cuando se incluye la propiedad de clave externa `DepartmentID` en el modelo de datos, no es necesario capturar la entidad `Department` antes de una actualización.</span><span class="sxs-lookup"><span data-stu-id="2cda8-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="2cda8-261">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="2cda8-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="2cda8-262">El atributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` especifica que la aplicación proporciona la clave principal, en lugar de generarla la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="2cda8-263">De forma predeterminada, EF Core da por supuesto que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="2cda8-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="2cda8-264">Los valores de clave principal generados por la base de datos suelen ser el mejor método.</span><span class="sxs-lookup"><span data-stu-id="2cda8-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="2cda8-265">Para las entidades `Course`, el usuario especifica la clave principal.</span><span class="sxs-lookup"><span data-stu-id="2cda8-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="2cda8-266">Por ejemplo, un número de curso como una serie de 1000 para el departamento de matemáticas, una serie de 2000 para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="2cda8-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="2cda8-267">También se puede usar el atributo `DatabaseGenerated` para generar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="2cda8-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="2cda8-268">Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="2cda8-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="2cda8-269">Para obtener más información, vea [Propiedades generadas](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="2cda8-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2cda8-270">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="2cda8-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="2cda8-271">Las propiedades de clave externa (FK) y las de navegación de la entidad `Course` reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2cda8-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="2cda8-272">Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="2cda8-273">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="2cda8-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="2cda8-274">Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `CourseAssignments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="2cda8-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2cda8-275">`CourseAssignment` se explica [más adelante](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="2cda8-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="2cda8-276">Crear la entidad Department</span><span class="sxs-lookup"><span data-stu-id="2cda8-276">Create the Department entity</span></span>

![La entidad Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="2cda8-278">Cree *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="2cda8-279">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="2cda8-279">The Column attribute</span></span>

<span data-ttu-id="2cda8-280">Anteriormente se usó el atributo `Column` para cambiar la asignación de nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="2cda8-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="2cda8-281">En el código de la entidad `Department`, se usó el atributo `Column` para cambiar la asignación de tipos de datos de SQL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="2cda8-282">La columna `Budget` se define mediante el tipo money de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="2cda8-283">Por lo general, la asignación de columnas no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="2cda8-283">Column mapping is generally not required.</span></span> <span data-ttu-id="2cda8-284">EF Core generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="2cda8-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="2cda8-285">El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2cda8-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="2cda8-286">`Budget` es para la divisa, y el tipo de datos money es más adecuado para la divisa.</span><span class="sxs-lookup"><span data-stu-id="2cda8-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2cda8-287">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="2cda8-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="2cda8-288">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2cda8-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="2cda8-289">Un departamento puede tener o no un administrador.</span><span class="sxs-lookup"><span data-stu-id="2cda8-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="2cda8-290">Un administrador siempre es un instructor.</span><span class="sxs-lookup"><span data-stu-id="2cda8-290">An administrator is always an instructor.</span></span> <span data-ttu-id="2cda8-291">Por lo tanto, la propiedad `InstructorID` se incluye como la clave externa para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="2cda8-292">La propiedad de navegación se denomina `Administrator` pero contiene una entidad `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="2cda8-293">El signo de interrogación (?) en el código anterior especifica que la propiedad acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="2cda8-294">Un departamento puede tener varios cursos, por lo que hay una propiedad de navegación Courses:</span><span class="sxs-lookup"><span data-stu-id="2cda8-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="2cda8-295">Nota: Por convención, EF Core permite la eliminación en cascada de las claves externas que no acepten valores NULL ni relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="2cda8-296">La eliminación en cascada puede dar lugar a reglas de eliminación en cascada circular.</span><span class="sxs-lookup"><span data-stu-id="2cda8-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="2cda8-297">Las reglas de eliminación en cascada circular provocan una excepción cuando se agrega una migración.</span><span class="sxs-lookup"><span data-stu-id="2cda8-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="2cda8-298">Por ejemplo, si no se ha definido que la propiedad `Department.InstructorID` acepta valores NULL:</span><span class="sxs-lookup"><span data-stu-id="2cda8-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="2cda8-299">EF Core configura una regla de eliminación en cascada para eliminar el instructor cuando se elimine el departamento.</span><span class="sxs-lookup"><span data-stu-id="2cda8-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="2cda8-300">Eliminar el instructor cuando se elimine el departamento no es el comportamiento previsto.</span><span class="sxs-lookup"><span data-stu-id="2cda8-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="2cda8-301">Si las reglas de negocios requieren que la propiedad `InstructorID` no acepte valores NULL, use la siguiente instrucción de API fluida:</span><span class="sxs-lookup"><span data-stu-id="2cda8-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="2cda8-302">El código anterior deshabilita la eliminación en cascada en la relación de instructor y departamento.</span><span class="sxs-lookup"><span data-stu-id="2cda8-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="2cda8-303">Actualizar la entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="2cda8-303">Update the Enrollment entity</span></span>

<span data-ttu-id="2cda8-304">Un registro de inscripción corresponde a un curso realizado por un alumno.</span><span class="sxs-lookup"><span data-stu-id="2cda8-304">An enrollment record is for one course taken by one student.</span></span>

![La entidad Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="2cda8-306">Actualice *Models/Enrollment.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2cda8-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2cda8-307">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="2cda8-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="2cda8-308">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2cda8-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="2cda8-309">Un registro de inscripción es para un curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="2cda8-310">Un registro de inscripción es para un alumno, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="2cda8-311">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="2cda8-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="2cda8-312">Hay una relación de varios a varios entre las entidades `Student` y `Course`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="2cda8-313">La entidad `Enrollment` funciona como una tabla combinada varios a varios *con carga útil* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="2cda8-314">"Con carga útil" significa que la tabla `Enrollment` contiene datos adicionales, además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="2cda8-315">En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="2cda8-316">(Este diagrama se ha generado mediante [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="2cda8-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="2cda8-317">Crear el diagrama no forma parte del tutorial).</span><span class="sxs-lookup"><span data-stu-id="2cda8-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Relación de varios a varios entre estudiantes y cursos](complex-data-model/_static/student-course.png)

<span data-ttu-id="2cda8-319">Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, que indica una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="2cda8-320">Si la tabla `Enrollment` no incluyera información de calificaciones, solo tendría que contener las dos claves externas (`CourseID` y `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="2cda8-321">Una tabla combinada de varios a varios sin carga útil se suele denominar una tabla combinada pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="2cda8-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="2cda8-322">Las entidades `Instructor` y `Course` tienen una relación de varios a varios con una tabla combinada pura.</span><span class="sxs-lookup"><span data-stu-id="2cda8-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="2cda8-323">Nota: EF 6.x es compatible con las tablas de combinación implícitas con relaciones de varios a varios, pero EF Core, no.</span><span class="sxs-lookup"><span data-stu-id="2cda8-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="2cda8-324">Para obtener más información, consulte [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relaciones de varios a varios en EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="2cda8-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="2cda8-325">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="2cda8-325">The CourseAssignment entity</span></span>

![La entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="2cda8-327">Cree *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="2cda8-328">Relación Instructor-to-Courses</span><span class="sxs-lookup"><span data-stu-id="2cda8-328">Instructor-to-Courses</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="2cda8-330">La relación de varios a varios Instructor-to-Courses:</span><span class="sxs-lookup"><span data-stu-id="2cda8-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="2cda8-331">Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="2cda8-332">Es una tabla combinada pura (tabla sin carga útil).</span><span class="sxs-lookup"><span data-stu-id="2cda8-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="2cda8-333">Es común denominar una entidad de combinación `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="2cda8-334">Por ejemplo, la tabla de combinación Instructor-to-Courses usando este patrón es `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="2cda8-335">Pero se recomienda usar un nombre que describa la relación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="2cda8-336">Los modelos de datos empiezan de manera sencilla y crecen.</span><span class="sxs-lookup"><span data-stu-id="2cda8-336">Data models start out simple and grow.</span></span> <span data-ttu-id="2cda8-337">Las tablas combinadas sin carga útil (PJT) evolucionan con frecuencia para incluir la carga útil.</span><span class="sxs-lookup"><span data-stu-id="2cda8-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="2cda8-338">A partir de un nombre de entidad descriptivo, no es necesario cambiar el nombre cuando la tabla combinada cambia.</span><span class="sxs-lookup"><span data-stu-id="2cda8-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="2cda8-339">Idealmente, la entidad de combinación tendrá su propio nombre natural (posiblemente una sola palabra) en el dominio de empresa.</span><span class="sxs-lookup"><span data-stu-id="2cda8-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="2cda8-340">Por ejemplo, Books y Customers podrían vincularse a través de una entidad combinada denominada Ratings.</span><span class="sxs-lookup"><span data-stu-id="2cda8-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="2cda8-341">Para la relación de varios a varios Instructor-to-Courses, se prefiere `CourseAssignment` a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="2cda8-342">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="2cda8-342">Composite key</span></span>

<span data-ttu-id="2cda8-343">Las claves externas no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2cda8-343">FKs are not nullable.</span></span> <span data-ttu-id="2cda8-344">Las dos claves externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntas identifican de forma única cada fila de la tabla `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="2cda8-345">`CourseAssignment` no requiere una clave principal dedicada.</span><span class="sxs-lookup"><span data-stu-id="2cda8-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="2cda8-346">Las propiedades `InstructorID` y `CourseID` funcionan como una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="2cda8-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="2cda8-347">La única manera de especificar claves principales compuestas en EF Core es con la *API fluida*.</span><span class="sxs-lookup"><span data-stu-id="2cda8-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="2cda8-348">La sección siguiente muestra cómo configurar la clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="2cda8-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="2cda8-349">La clave compuesta asegura que:</span><span class="sxs-lookup"><span data-stu-id="2cda8-349">The composite key ensures:</span></span>

* <span data-ttu-id="2cda8-350">Se permiten varias filas para un curso.</span><span class="sxs-lookup"><span data-stu-id="2cda8-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="2cda8-351">Se permiten varias filas para un instructor.</span><span class="sxs-lookup"><span data-stu-id="2cda8-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="2cda8-352">No se permiten varias filas para el mismo instructor y curso.</span><span class="sxs-lookup"><span data-stu-id="2cda8-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="2cda8-353">La entidad de combinación `Enrollment` define su propia clave principal, por lo que este tipo de duplicados son posibles.</span><span class="sxs-lookup"><span data-stu-id="2cda8-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="2cda8-354">Para evitar los duplicados:</span><span class="sxs-lookup"><span data-stu-id="2cda8-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="2cda8-355">Agregue un índice único en los campos de clave externa, o</span><span class="sxs-lookup"><span data-stu-id="2cda8-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="2cda8-356">Configure `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="2cda8-357">Para obtener más información, vea [Índices](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="2cda8-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="2cda8-358">Actualizar el contexto de la base de datos</span><span class="sxs-lookup"><span data-stu-id="2cda8-358">Update the DB context</span></span>

<span data-ttu-id="2cda8-359">Agregue el código resaltado siguiente a *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cda8-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="2cda8-360">El código anterior agrega las nuevas entidades y configura la clave principal compuesta de la entidad `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="2cda8-361">Alternativa de la API fluida a los atributos</span><span class="sxs-lookup"><span data-stu-id="2cda8-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="2cda8-362">El método `OnModelCreating` del código anterior usa la *API fluida* para configurar el comportamiento de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2cda8-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="2cda8-363">La API se denomina "fluida" porque a menudo se usa para encadenar una serie de llamadas de método en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="2cda8-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="2cda8-364">El [código siguiente](/ef/core/modeling/#methods-of-configuration) es un ejemplo de la API fluida:</span><span class="sxs-lookup"><span data-stu-id="2cda8-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="2cda8-365">En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="2cda8-366">Pero la API fluida puede especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="2cda8-367">Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="2cda8-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="2cda8-368">`MinimumLength` no cambia el esquema, solo aplica una regla de validación de longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="2cda8-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="2cda8-369">Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="2cda8-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="2cda8-370">Se pueden mezclar atributos y la API fluida.</span><span class="sxs-lookup"><span data-stu-id="2cda8-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="2cda8-371">Hay algunas configuraciones que solo se pueden realizar con la API fluida (especificando una clave principal compuesta).</span><span class="sxs-lookup"><span data-stu-id="2cda8-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="2cda8-372">Hay algunas configuraciones que solo se pueden realizar con atributos (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="2cda8-373">La práctica recomendada para el uso de atributos o API fluida:</span><span class="sxs-lookup"><span data-stu-id="2cda8-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="2cda8-374">Elija uno de estos dos enfoques.</span><span class="sxs-lookup"><span data-stu-id="2cda8-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="2cda8-375">Use el enfoque elegido de forma tan coherente como sea posible.</span><span class="sxs-lookup"><span data-stu-id="2cda8-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="2cda8-376">Algunos de los atributos utilizados en este tutorial se usan para:</span><span class="sxs-lookup"><span data-stu-id="2cda8-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="2cda8-377">Solo validación (por ejemplo, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="2cda8-378">Solo configuración de EF Core (por ejemplo, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="2cda8-379">Validación y configuración de EF Core (por ejemplo, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="2cda8-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="2cda8-380">Para obtener más información sobre la diferencia entre los atributos y la API fluida, vea [Métodos de configuración](/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="2cda8-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="2cda8-381">Diagrama de entidades en el que se muestran las relaciones</span><span class="sxs-lookup"><span data-stu-id="2cda8-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="2cda8-382">En la siguiente ilustración se muestra el diagrama creado por EF Power Tools para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="2cda8-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

<span data-ttu-id="2cda8-384">El diagrama anterior muestra:</span><span class="sxs-lookup"><span data-stu-id="2cda8-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="2cda8-385">Varias líneas de relación uno a varios (1 a \*).</span><span class="sxs-lookup"><span data-stu-id="2cda8-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="2cda8-386">La línea de relación de uno a cero o uno (1 a 0..1) entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="2cda8-387">La línea de relación de cero o uno o varios (0..1 a \*) entre las entidades `Instructor` y `Department`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="2cda8-388">Inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="2cda8-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="2cda8-389">Actualice el código en *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cda8-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="2cda8-390">El código anterior proporciona datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="2cda8-391">La mayor parte de este código crea objetos de entidad y carga los datos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2cda8-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="2cda8-392">Los datos de ejemplo se usan para pruebas.</span><span class="sxs-lookup"><span data-stu-id="2cda8-392">The sample data is used for testing.</span></span> <span data-ttu-id="2cda8-393">Consulte `Enrollments` y `CourseAssignments` para obtener ejemplos de cómo pueden inicializarse las tablas de combinación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="2cda8-393">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="2cda8-394">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="2cda8-394">Add a migration</span></span>

<span data-ttu-id="2cda8-395">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2cda8-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cda8-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cda8-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cda8-397">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cda8-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="2cda8-398">El comando anterior muestra una advertencia sobre la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="2cda8-399">Si se ejecuta el comando `database update`, se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="2cda8-400">Aplicar la migración</span><span class="sxs-lookup"><span data-stu-id="2cda8-400">Apply the migration</span></span>

<span data-ttu-id="2cda8-401">Ahora que tiene una base de datos existente, debe pensar cómo aplicar los cambios futuros en ella.</span><span class="sxs-lookup"><span data-stu-id="2cda8-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="2cda8-402">En este tutorial se muestran dos enfoques:</span><span class="sxs-lookup"><span data-stu-id="2cda8-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="2cda8-403">Quitar y volver a crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="2cda8-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="2cda8-404">[Aplicar la migración a la base de datos existente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="2cda8-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="2cda8-405">Aunque este método es más complejo y lento, es el método preferido para entornos de producción del mundo real.</span><span class="sxs-lookup"><span data-stu-id="2cda8-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="2cda8-406">**Nota**: Esta sección del tutorial es opcional.</span><span class="sxs-lookup"><span data-stu-id="2cda8-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="2cda8-407">Puede realizar la operación de quitar y volver a crear, y omitir esta sección.</span><span class="sxs-lookup"><span data-stu-id="2cda8-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="2cda8-408">Si quiere seguir los pasos descritos en esta sección, no realice la operación de quitar y volver a crear.</span><span class="sxs-lookup"><span data-stu-id="2cda8-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="2cda8-409">Quitar y volver a crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="2cda8-409">Drop and re-create the database</span></span>

<span data-ttu-id="2cda8-410">El código en la `DbInitializer` actualizada agrega los datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="2cda8-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="2cda8-411">Para obligar a EF Core a crear una base de datos, quite y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cda8-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cda8-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2cda8-413">En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="2cda8-414">Ejecute `Get-Help about_EntityFrameworkCore` desde PMC para obtener información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="2cda8-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cda8-415">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cda8-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2cda8-416">Abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2cda8-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="2cda8-417">La carpeta del proyecto contiene el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2cda8-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="2cda8-418">Escriba lo siguiente en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="2cda8-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="2cda8-419">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cda8-419">Run the app.</span></span> <span data-ttu-id="2cda8-420">Ejecutar la aplicación ejecuta el método `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="2cda8-421">`DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="2cda8-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="2cda8-422">Abra la base de datos en SSOX:</span><span class="sxs-lookup"><span data-stu-id="2cda8-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="2cda8-423">Si anteriormente se abrió SSOX, haga clic en el botón **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="2cda8-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="2cda8-424">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="2cda8-424">Expand the **Tables** node.</span></span> <span data-ttu-id="2cda8-425">Se muestran las tablas creadas.</span><span class="sxs-lookup"><span data-stu-id="2cda8-425">The created tables are displayed.</span></span>

![Tablas en SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="2cda8-427">Examine la tabla **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="2cda8-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="2cda8-428">Haga clic con el botón derecho en la tabla **CourseAssignment** y seleccione **Ver datos**.</span><span class="sxs-lookup"><span data-stu-id="2cda8-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="2cda8-429">Compruebe que la tabla **CourseAssignment** contiene datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-429">Verify the **CourseAssignment** table contains data.</span></span>

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="2cda8-431">Aplicar la migración a la base de datos existente</span><span class="sxs-lookup"><span data-stu-id="2cda8-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="2cda8-432">Esta sección es opcional.</span><span class="sxs-lookup"><span data-stu-id="2cda8-432">This section is optional.</span></span> <span data-ttu-id="2cda8-433">Estos pasos solo funcionan si pasó por alto la sección [Quitar y volver a crear la base de datos](#drop).</span><span class="sxs-lookup"><span data-stu-id="2cda8-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="2cda8-434">Cuando se ejecutan migraciones con datos existentes, puede haber restricciones de clave externa que no se cumplen con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="2cda8-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="2cda8-435">Con los datos de producción, se deben realizar algunos pasos para migrar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="2cda8-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="2cda8-436">En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="2cda8-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="2cda8-437">No realice estos cambios de código sin hacer una copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2cda8-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="2cda8-438">No realice estos cambios de código si realizó la sección anterior y actualizó la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cda8-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="2cda8-439">El archivo *{marca_de_tiempo}_ComplexDataModel.cs* contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2cda8-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="2cda8-440">El código anterior agrega una clave externa `DepartmentID` que acepta valores NULL a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="2cda8-441">La base de datos del tutorial anterior contiene filas en `Course`, por lo que no se puede actualizar esa tabla mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="2cda8-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="2cda8-442">Para realizar la migración de `ComplexDataModel`, trabaje con los datos existentes:</span><span class="sxs-lookup"><span data-stu-id="2cda8-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="2cda8-443">Cambie el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2cda8-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="2cda8-444">Cree un departamento falso denominado "Temp" para que actúe como el departamento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2cda8-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="2cda8-445">Corregir las restricciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="2cda8-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="2cda8-446">Actualice el método `Up` de las clases `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="2cda8-447">Abra el archivo *{marca_de_tiempo}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="2cda8-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="2cda8-448">Convierta en comentario la línea de código que agrega la columna `DepartmentID` a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="2cda8-449">Agregue el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="2cda8-449">Add the following highlighted code.</span></span> <span data-ttu-id="2cda8-450">El nuevo código va después del bloque `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="2cda8-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="2cda8-451">Con los cambios anteriores, las filas `Course` existentes estarán relacionadas con el departamento "Temp" después de ejecutar el método `ComplexDataModel` de `Up`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="2cda8-452">Una aplicación de producción debería:</span><span class="sxs-lookup"><span data-stu-id="2cda8-452">A production app would:</span></span>

* <span data-ttu-id="2cda8-453">Incluir código o scripts para agregar filas de `Department` y filas de `Course` relacionadas a las nuevas filas de `Department`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="2cda8-454">No use el departamento "Temp" o el valor predeterminado de `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="2cda8-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="2cda8-455">El siguiente tutorial trata los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="2cda8-455">The next tutorial covers related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="2cda8-456">[Anterior](xref:data/ef-rp/migrations)
> [Siguiente](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="2cda8-456">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
