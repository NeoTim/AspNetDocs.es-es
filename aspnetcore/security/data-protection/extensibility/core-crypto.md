---
title: Extensibilidad de criptografía de núcleo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer y el generador de nivel superior.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 47432cfefe0a52c9f815d717f7269ec68fdb6af3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036182"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="5752b-103">Extensibilidad de criptografía de núcleo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5752b-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="5752b-104">Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.</span><span class="sxs-lookup"><span data-stu-id="5752b-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="5752b-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5752b-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5752b-106">El **IAuthenticatedEncryptor** interfaz es el bloque de creación básico del subsistema criptográfico.</span><span class="sxs-lookup"><span data-stu-id="5752b-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="5752b-107">Generalmente es una IAuthenticatedEncryptor por clave, y la instancia IAuthenticatedEncryptor encapsula toda la información algorítmica necesaria para realizar operaciones criptográficas y material clave criptográfico.</span><span class="sxs-lookup"><span data-stu-id="5752b-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="5752b-108">Como sugiere su nombre, el tipo es responsable de proporcionar servicios de cifrado y descifrado autenticados.</span><span class="sxs-lookup"><span data-stu-id="5752b-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="5752b-109">Expone las siguientes dos API.</span><span class="sxs-lookup"><span data-stu-id="5752b-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="5752b-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5752b-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="5752b-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5752b-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="5752b-112">El método Encrypt devuelve un blob que incluye el texto no cifrado cifrado y una etiqueta de autenticación.</span><span class="sxs-lookup"><span data-stu-id="5752b-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="5752b-113">La etiqueta de autenticación debe incluir los datos adicionales de autenticado (AAD), aunque el propio AAD no se debe recuperar de la carga final.</span><span class="sxs-lookup"><span data-stu-id="5752b-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="5752b-114">El método Decrypt valida la etiqueta a la autenticación y devuelve la carga deciphered.</span><span class="sxs-lookup"><span data-stu-id="5752b-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="5752b-115">Todos los errores (excepto ArgumentNullException y similares) deben homogeneizarse a CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="5752b-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="5752b-116">La propia instancia IAuthenticatedEncryptor realmente no tiene que contener el material de clave.</span><span class="sxs-lookup"><span data-stu-id="5752b-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="5752b-117">Por ejemplo, la implementación puede delegar a un HSM para todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="5752b-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="5752b-118">Cómo crear un IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5752b-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5752b-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5752b-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5752b-120">El **IAuthenticatedEncryptorFactory** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia.</span><span class="sxs-lookup"><span data-stu-id="5752b-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5752b-121">Su API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="5752b-121">Its API is as follows.</span></span>

* <span data-ttu-id="5752b-122">CreateEncryptorInstance (clave IKey): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5752b-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5752b-123">Para cualquier instancia dada de IKey, cualquier rechazarán autenticado creados por su método CreateEncryptorInstance debe considerarse equivalente, como en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5752b-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5752b-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5752b-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5752b-125">El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia.</span><span class="sxs-lookup"><span data-stu-id="5752b-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5752b-126">Su API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="5752b-126">Its API is as follows.</span></span>

* <span data-ttu-id="5752b-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5752b-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="5752b-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5752b-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="5752b-129">Al igual que IAuthenticatedEncryptor, se supone que una instancia de IAuthenticatedEncryptorDescriptor para encapsular una clave específica.</span><span class="sxs-lookup"><span data-stu-id="5752b-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="5752b-130">Esto significa que para cualquier instancia IAuthenticatedEncryptorDescriptor, cualquier rechazarán autenticado creados por su método CreateEncryptorInstance debe considerarse equivalente, como en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5752b-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="5752b-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x solo)</span><span class="sxs-lookup"><span data-stu-id="5752b-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5752b-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5752b-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5752b-133">El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo exportar a XML.</span><span class="sxs-lookup"><span data-stu-id="5752b-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="5752b-134">Su API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="5752b-134">Its API is as follows.</span></span>

* <span data-ttu-id="5752b-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5752b-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5752b-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5752b-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="5752b-137">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="5752b-137">XML Serialization</span></span>

<span data-ttu-id="5752b-138">La diferencia principal entre IAuthenticatedEncryptor y IAuthenticatedEncryptorDescriptor es que el descriptor sabe cómo crear el sistema de cifrado y suministrarle los argumentos válidos.</span><span class="sxs-lookup"><span data-stu-id="5752b-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="5752b-139">Considere la posibilidad de un IAuthenticatedEncryptor cuya implementación se basa en SymmetricAlgorithm y KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="5752b-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="5752b-140">Trabajo del sistema de cifrado es consumen estos tipos, pero no sabe necesariamente estos tipos de proceden, por lo que realmente no se puede escribir una descripción de cómo volver a sí mismo si se reinicia la aplicación adecuada.</span><span class="sxs-lookup"><span data-stu-id="5752b-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="5752b-141">El descriptor actúa como un nivel más alto por encima de esto.</span><span class="sxs-lookup"><span data-stu-id="5752b-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="5752b-142">Puesto que el descriptor sabe cómo crear la instancia de sistema de cifrado (p. ej., sabe cómo crear los algoritmos necesarios), puede serializar ese conocimiento en formato XML para que la instancia de sistema de cifrado se puede volver a crear después una aplicación de restablecimiento.</span><span class="sxs-lookup"><span data-stu-id="5752b-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="5752b-143">El descriptor se puede serializar a través de su rutina ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="5752b-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="5752b-144">Esta rutina devuelve un XmlSerializedDescriptorInfo que contiene dos propiedades: la representación de XElement de descriptor y el tipo que representa un [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) que puede ser se usa para resucitar este descriptor dada la XElement correspondiente.</span><span class="sxs-lookup"><span data-stu-id="5752b-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="5752b-145">El descriptor serializado puede contener información confidencial, como material de clave criptográfica.</span><span class="sxs-lookup"><span data-stu-id="5752b-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="5752b-146">El sistema de protección de datos tiene compatibilidad integrada para cifrar la información antes de que se conserva en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="5752b-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="5752b-147">Para aprovechar las ventajas de esto, el descriptor debe marcar el elemento que contiene información confidencial con el nombre de atributo "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), valor "true".</span><span class="sxs-lookup"><span data-stu-id="5752b-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="5752b-148">Hay una API auxiliar para establecer este atributo.</span><span class="sxs-lookup"><span data-stu-id="5752b-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="5752b-149">Llame al método de extensión que XElement.markasrequiresencryption() ubicado en el espacio de nombres Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="5752b-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="5752b-150">También puede haber casos donde el descriptor serializado no contiene información confidencial.</span><span class="sxs-lookup"><span data-stu-id="5752b-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="5752b-151">Considere el caso de una clave criptográfica que se almacenan en un HSM de nuevo.</span><span class="sxs-lookup"><span data-stu-id="5752b-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="5752b-152">El descriptor no se puede escribir el material de clave al serializar propio dado que el HSM no expone el material en texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="5752b-152">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="5752b-153">En su lugar, puede escribir el descriptor de la versión de encapsulado de clave de la clave (si el HSM permite la exportación de este modo) o el identificador único del HSM para la clave.</span><span class="sxs-lookup"><span data-stu-id="5752b-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="5752b-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="5752b-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="5752b-155">El **IAuthenticatedEncryptorDescriptorDeserializer** interfaz representa un tipo que sabe cómo deserializar una instancia de IAuthenticatedEncryptorDescriptor de un XElement.</span><span class="sxs-lookup"><span data-stu-id="5752b-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="5752b-156">Expone un único método:</span><span class="sxs-lookup"><span data-stu-id="5752b-156">It exposes a single method:</span></span>

* <span data-ttu-id="5752b-157">ImportFromXml (elemento de XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5752b-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5752b-158">El método ImportFromXml toma el XElement que devolvió [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) y crea un equivalente de la IAuthenticatedEncryptorDescriptor original.</span><span class="sxs-lookup"><span data-stu-id="5752b-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="5752b-159">Tipos que implementan IAuthenticatedEncryptorDescriptorDeserializer deben tener uno de los dos constructores públicos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5752b-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="5752b-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="5752b-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="5752b-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="5752b-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="5752b-162">El elemento IServiceProvider pasado al constructor puede ser null.</span><span class="sxs-lookup"><span data-stu-id="5752b-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="5752b-163">El generador de nivel superior</span><span class="sxs-lookup"><span data-stu-id="5752b-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5752b-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5752b-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5752b-165">El **AlgorithmConfiguration** clase representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias.</span><span class="sxs-lookup"><span data-stu-id="5752b-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5752b-166">Expone una sola API.</span><span class="sxs-lookup"><span data-stu-id="5752b-166">It exposes a single API.</span></span>

* <span data-ttu-id="5752b-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5752b-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5752b-168">Piense en AlgorithmConfiguration como el generador de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="5752b-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="5752b-169">La configuración actúa como una plantilla.</span><span class="sxs-lookup"><span data-stu-id="5752b-169">The configuration serves as a template.</span></span> <span data-ttu-id="5752b-170">Encapsula información algorítmico (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada con una clave específica.</span><span class="sxs-lookup"><span data-stu-id="5752b-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="5752b-171">Cuando se llama a CreateNewDescriptor, nuevo material de clave se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material.</span><span class="sxs-lookup"><span data-stu-id="5752b-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5752b-172">El material de clave podría creó en software (y se mantiene en memoria), podría crearse y contenido en un HSM y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="5752b-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5752b-173">El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crear instancias de IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="5752b-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5752b-174">El tipo AlgorithmConfiguration actúa como punto de entrada para las rutinas de creación de claves como [clave automatic gradual](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="5752b-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="5752b-175">Para cambiar la implementación para todas las claves de futuras, establezca la propiedad AuthenticatedEncryptorConfiguration en KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="5752b-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5752b-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5752b-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5752b-177">El **IAuthenticatedEncryptorConfiguration** interfaz representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias.</span><span class="sxs-lookup"><span data-stu-id="5752b-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5752b-178">Expone una sola API.</span><span class="sxs-lookup"><span data-stu-id="5752b-178">It exposes a single API.</span></span>

* <span data-ttu-id="5752b-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5752b-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5752b-180">Piense en IAuthenticatedEncryptorConfiguration como el generador de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="5752b-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="5752b-181">La configuración actúa como una plantilla.</span><span class="sxs-lookup"><span data-stu-id="5752b-181">The configuration serves as a template.</span></span> <span data-ttu-id="5752b-182">Encapsula información algorítmico (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada con una clave específica.</span><span class="sxs-lookup"><span data-stu-id="5752b-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="5752b-183">Cuando se llama a CreateNewDescriptor, nuevo material de clave se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material.</span><span class="sxs-lookup"><span data-stu-id="5752b-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5752b-184">El material de clave podría creó en software (y se mantiene en memoria), podría crearse y contenido en un HSM y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="5752b-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5752b-185">El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crear instancias de IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="5752b-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5752b-186">El tipo IAuthenticatedEncryptorConfiguration actúa como punto de entrada para las rutinas de creación de claves como [clave automatic gradual](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="5752b-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="5752b-187">Para cambiar la implementación para todas las claves futuras, registre un singleton IAuthenticatedEncryptorConfiguration en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5752b-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
