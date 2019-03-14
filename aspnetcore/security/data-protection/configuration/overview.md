---
title: Configurer la Protection des données ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer la Protection des données dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/13/2018
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0aef2680f48b7923579f90943846f22734f61b50
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061656"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="76946-103">Configurer la Protection des données ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76946-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="76946-104">Lorsque le système de Protection des données est initialisé, il s’applique [paramètres par défaut](xref:security/data-protection/configuration/default-settings) selon l’environnement d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="76946-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="76946-105">Ces paramètres sont généralement appropriés pour les applications exécutées sur un seul ordinateur.</span><span class="sxs-lookup"><span data-stu-id="76946-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="76946-106">Il existe des cas où un développeur peut souhaiter modifier les paramètres par défaut :</span><span class="sxs-lookup"><span data-stu-id="76946-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="76946-107">L’application est répartie sur plusieurs ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="76946-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="76946-108">Pour des raisons de conformité.</span><span class="sxs-lookup"><span data-stu-id="76946-108">For compliance reasons.</span></span>

<span data-ttu-id="76946-109">Pour ces scénarios, le système de Protection des données offre une API de configuration riche.</span><span class="sxs-lookup"><span data-stu-id="76946-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="76946-110">Comme pour les fichiers de configuration, le key ring de protection de données doivent être protégé à l’aide des autorisations appropriées.</span><span class="sxs-lookup"><span data-stu-id="76946-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="76946-111">Vous pouvez choisir de chiffrer les clés au repos, mais cela n’empêche pas les pirates de créer de nouvelles clés.</span><span class="sxs-lookup"><span data-stu-id="76946-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="76946-112">Par conséquent, la sécurité de votre application est affectée.</span><span class="sxs-lookup"><span data-stu-id="76946-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="76946-113">L’emplacement de stockage configuré avec la Protection des données doit avoir son accès limité à l’application elle-même, similaire à la façon dont vous protégera les fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="76946-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="76946-114">Par exemple, si vous choisissez de stocker votre porte-clés sur le disque, utilisez des autorisations de système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="76946-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="76946-115">Vérifiez seulement l’identité sous laquelle s’exécute votre application web a lire, écrire et créer l’accès à ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="76946-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="76946-116">Si vous utilisez le stockage Table Azure, seule l’application web doit avoir la possibilité de lire, écrire ou créer de nouvelles entrées dans le magasin de tables, etc.</span><span class="sxs-lookup"><span data-stu-id="76946-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="76946-117">La méthode d’extension [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) retourne un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="76946-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="76946-118">`IDataProtectionBuilder` expose des méthodes d’extension que vous pouvez chaîner des options pour configurer la Protection des données.</span><span class="sxs-lookup"><span data-stu-id="76946-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="76946-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="76946-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="76946-120">Pour stocker les clés dans [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurer le système avec [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="76946-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="76946-121">Définir l’emplacement de stockage de porte-clés (par exemple, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="76946-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="76946-122">L’emplacement doit être défini, car l’appel `ProtectKeysWithAzureKeyVault` implémente un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) qui désactive les paramètres de protection automatique des données, y compris l’emplacement de stockage de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="76946-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="76946-123">L’exemple précédent utilise le stockage Blob Azure pour conserver le key ring.</span><span class="sxs-lookup"><span data-stu-id="76946-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="76946-124">Pour plus d’informations, consultez [fournisseurs de stockage de clés : Azure et Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="76946-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="76946-125">Vous pouvez également conserver le key ring localement avec [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="76946-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="76946-126">Le `keyIdentifier` est l’identificateur de clé de coffre de clés utilisée pour le chiffrement de clé (par exemple, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="76946-126">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="76946-127">`ProtectKeysWithAzureKeyVault` surcharges :</span><span class="sxs-lookup"><span data-stu-id="76946-127">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="76946-128">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permet d’utiliser un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) pour permettre au système de protection des données à utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="76946-128">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="76946-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permet d’utiliser un `ClientId` et [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) pour permettre au système de protection des données à utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="76946-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="76946-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permet d’utiliser un `ClientId` et `ClientSecret` pour permettre au système de protection des données à utiliser le coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="76946-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="76946-131">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="76946-131">PersistKeysToFileSystem</span></span>

<span data-ttu-id="76946-132">Pour stocker les clés sur un partage UNC plutôt qu’à la *% LocalAppData%* emplacement par défaut, configurez le système avec [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="76946-132">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="76946-133">Si vous modifiez l’emplacement de la persistance des clés, le système chiffre n’est plus automatiquement les clés au repos, dans la mesure où il ne sait pas si DPAPI est un mécanisme de chiffrement approprié.</span><span class="sxs-lookup"><span data-stu-id="76946-133">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="76946-134">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="76946-134">ProtectKeysWith\*</span></span>

<span data-ttu-id="76946-135">Vous pouvez configurer le système pour protéger les clés au repos en appelant une de le [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuration.</span><span class="sxs-lookup"><span data-stu-id="76946-135">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="76946-136">Prenons l’exemple ci-dessous, qui stocke les clés sur un partage UNC et chiffre ces clés au repos avec un certificat X.509 spécifique :</span><span class="sxs-lookup"><span data-stu-id="76946-136">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="76946-137">Dans ASP.NET Core 2.1 ou version ultérieure, vous pouvez fournir un [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) à [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), comme un certificat chargé à partir d’un fichier :</span><span class="sxs-lookup"><span data-stu-id="76946-137">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="76946-138">Consultez [clé de chiffrement au repos](xref:security/data-protection/implementation/key-encryption-at-rest) pour obtenir des exemples et des discussions sur les mécanismes de chiffrement de clé intégrée.</span><span class="sxs-lookup"><span data-stu-id="76946-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="76946-139">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="76946-139">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="76946-140">Dans ASP.NET Core 2.1 ou version ultérieure, vous pouvez faire pivoter des certificats et déchiffrer les clés au repos à l’aide d’un tableau de [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificats avec [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="76946-140">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="76946-141">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="76946-141">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="76946-142">Pour configurer le système pour utiliser une durée de vie de clé de 14 jours au lieu de la valeur par défaut de 90 jours, utilisez [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="76946-142">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="76946-143">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="76946-143">SetApplicationName</span></span>

<span data-ttu-id="76946-144">Par défaut, le système de Protection des données isole les applications à partir de l’autre en fonction de leurs chemins d’accès racine du contenu, même si elles le partagent le même référentiel clé physique.</span><span class="sxs-lookup"><span data-stu-id="76946-144">By default, the Data Protection system isolates apps from one another based on their content root paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="76946-145">Cela empêche les applications de la compréhension des charges protégé entre eux.</span><span class="sxs-lookup"><span data-stu-id="76946-145">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="76946-146">Partage protégé entre les applications des charges utiles :</span><span class="sxs-lookup"><span data-stu-id="76946-146">To share protected payloads among apps:</span></span>

* <span data-ttu-id="76946-147">Configurer <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> dans chaque application avec la même valeur.</span><span class="sxs-lookup"><span data-stu-id="76946-147">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="76946-148">Utilisez la même version de la pile de l’API de Protection des données entre les applications.</span><span class="sxs-lookup"><span data-stu-id="76946-148">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="76946-149">Effectuer **soit** des éléments suivants dans les fichiers de projet d’applications :</span><span class="sxs-lookup"><span data-stu-id="76946-149">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="76946-150">Référencer la même version du framework partagé via le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="76946-150">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="76946-151">Référencent le même [package de Protection des données](xref:security/data-protection/introduction#package-layout) version.</span><span class="sxs-lookup"><span data-stu-id="76946-151">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="76946-152">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="76946-152">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="76946-153">Vous pouvez avoir un scénario dans lequel vous voulez une application à déployer automatiquement des clés (créer des clés) qu’ils approchent d’expiration.</span><span class="sxs-lookup"><span data-stu-id="76946-153">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="76946-154">Un exemple de ce peut être définies dans une relation principal/secondaire, où le principal de l’application est responsable de problèmes de gestion des clés et des applications secondaires ont simplement une vue en lecture seule de l’anneau de clé des applications.</span><span class="sxs-lookup"><span data-stu-id="76946-154">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="76946-155">Les applications secondaire peuvent être configurées pour traiter le key ring en lecture seule en configurant le système avec [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="76946-155">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="76946-156">Isolation par application</span><span class="sxs-lookup"><span data-stu-id="76946-156">Per-application isolation</span></span>

<span data-ttu-id="76946-157">Lorsque le système de Protection des données est fourni par un hôte ASP.NET Core, elle isole automatiquement les applications à partir d’une autre, même si ces applications sont exécutent sous le même compte de processus de travail et que vous utilisez le même matériel de gestion de clés principal.</span><span class="sxs-lookup"><span data-stu-id="76946-157">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="76946-158">Cela s’apparente au modificateur IsolateApps à partir de System.Web  **\<machineKey >** élément.</span><span class="sxs-lookup"><span data-stu-id="76946-158">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="76946-159">Le mécanisme d’isolation fonctionne en tenant compte de chaque application sur l’ordinateur local en tant qu’un locataire unique, donc le [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooté pour une application donnée inclut automatiquement l’ID d’application en tant que discriminateur.</span><span class="sxs-lookup"><span data-stu-id="76946-159">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="76946-160">ID unique de l’application provient d’un des deux emplacements :</span><span class="sxs-lookup"><span data-stu-id="76946-160">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="76946-161">Si l’application est hébergée dans IIS, l’identificateur unique est le chemin d’accès de l’application configuration.</span><span class="sxs-lookup"><span data-stu-id="76946-161">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="76946-162">Si une application est déployée dans un environnement de batterie de serveurs web, cette valeur doit être stable, en supposant que les environnements d’IIS sont configurés de façon similaire sur tous les ordinateurs dans la batterie de serveurs web.</span><span class="sxs-lookup"><span data-stu-id="76946-162">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="76946-163">Si l’application n’est pas hébergée dans IIS, l’identificateur unique est le chemin d’accès physique de l’application.</span><span class="sxs-lookup"><span data-stu-id="76946-163">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="76946-164">L’identificateur unique est conçu pour résister aux réinitialisations &mdash; de l’application individuelle et de la machine elle-même.</span><span class="sxs-lookup"><span data-stu-id="76946-164">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="76946-165">Ce mécanisme d’isolation part du principe que les applications ne sont pas malveillantes.</span><span class="sxs-lookup"><span data-stu-id="76946-165">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="76946-166">Une application malveillante peut toujours avoir un impact sur n’importe quel autre application en cours d’exécution sous le même compte de processus de travail.</span><span class="sxs-lookup"><span data-stu-id="76946-166">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="76946-167">Dans un environnement d’hébergement partagé où les applications sont mutuellement non approuvées, le fournisseur d’hébergement doit prendre étapes pour garantir l’isolation au niveau du système d’exploitation entre les applications, notamment pour séparer les applications sous-jacentes référentiels des clés.</span><span class="sxs-lookup"><span data-stu-id="76946-167">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="76946-168">Si le système de Protection des données n’est pas fourni par un hôte ASP.NET Core (par exemple, si vous instanciez via la `DataProtectionProvider` type concret) isolation de l’application est désactivée par défaut.</span><span class="sxs-lookup"><span data-stu-id="76946-168">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="76946-169">Lors de l’isolation de l’application est désactivée, soutenues par le même jeu de clés de toutes les applications peuvent partager les charges utiles tant qu’ils fournissent approprié [à des fins](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="76946-169">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="76946-170">Pour assurer l’isolation d’application dans cet environnement, appelez le [SetApplicationName](#setapplicationname) méthode sur la configuration de l’objet et fournissez un nom unique pour chaque application.</span><span class="sxs-lookup"><span data-stu-id="76946-170">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="76946-171">Modification des algorithmes avec UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="76946-171">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="76946-172">La pile de Protection des données vous permet de modifier l’algorithme par défaut utilisé par les clés qui vient d’être généré.</span><span class="sxs-lookup"><span data-stu-id="76946-172">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="76946-173">La façon la plus simple pour ce faire consiste à appeler [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) à partir du rappel de configuration :</span><span class="sxs-lookup"><span data-stu-id="76946-173">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="76946-174">La valeur par défaut EncryptionAlgorithm est AES-256-CBC, et la valeur par défaut ValidationAlgorithm est HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="76946-174">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="76946-175">La stratégie par défaut peut être définie par un administrateur système via une [-échelle de l’ordinateur stratégie](xref:security/data-protection/configuration/machine-wide-policy), mais un appel explicite à `UseCryptographicAlgorithms` substitue la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="76946-175">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="76946-176">Appel `UseCryptographicAlgorithms` vous permet de spécifier l’algorithme souhaité à partir d’une liste prédéfinie intégrée.</span><span class="sxs-lookup"><span data-stu-id="76946-176">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="76946-177">Vous n’avez pas besoin de vous soucier de l’implémentation de l’algorithme.</span><span class="sxs-lookup"><span data-stu-id="76946-177">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="76946-178">Dans le scénario ci-dessus, le système de Protection de données tente d’utiliser l’implémentation CNG de AES si s’exécutant sur Windows.</span><span class="sxs-lookup"><span data-stu-id="76946-178">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="76946-179">Sinon, il repasse automatiquement managé [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.</span><span class="sxs-lookup"><span data-stu-id="76946-179">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="76946-180">Vous pouvez spécifier manuellement une implémentation via un appel à [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="76946-180">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="76946-181">Algorithmes de modification n’affecte pas les clés existantes dans le key ring.</span><span class="sxs-lookup"><span data-stu-id="76946-181">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="76946-182">Il affecte uniquement les clés qui vient d’être générées.</span><span class="sxs-lookup"><span data-stu-id="76946-182">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="76946-183">En spécifiant des algorithmes personnalisés gérés</span><span class="sxs-lookup"><span data-stu-id="76946-183">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="76946-184">Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="76946-184">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="76946-185">Pour spécifier les algorithmes managés personnalisés, créez un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance qui pointe vers les types d’implémentation :</span><span class="sxs-lookup"><span data-stu-id="76946-185">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

<span data-ttu-id="76946-186">En règle générale le \*propriétés de Type doivent pointer vers concret, implémentations instanciables (via un constructeur sans paramètre public) de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) et [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), bien que le système spécial cas certaines valeurs telles que `typeof(Aes)` pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="76946-186">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="76946-187">Le SymmetricAlgorithm doit avoir une longueur de clé de ≥ 128 bits et une taille de bloc de ≥ 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec un remplissage de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="76946-187">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="76946-188">L’élément KeyedHashAlgorithm impossible doit avoir une taille de condensat de > = 128 bits, et il doit prendre en charge les clés de longueur égale à la longueur du résumé de l’algorithme de hachage.</span><span class="sxs-lookup"><span data-stu-id="76946-188">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="76946-189">L’élément KeyedHashAlgorithm impossible n’est pas strictement obligatoire d’être HMAC.</span><span class="sxs-lookup"><span data-stu-id="76946-189">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="76946-190">En spécifiant les algorithmes CNG de Windows personnalisés</span><span class="sxs-lookup"><span data-stu-id="76946-190">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="76946-191">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide du chiffrement en mode CBC avec validation de HMAC, créez un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="76946-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="76946-192">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide du chiffrement en mode CBC avec validation de HMAC, créez un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="76946-192">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="76946-193">L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de > = 64 bits, et il doit prendre en charge le chiffrement en mode CBC avec un remplissage de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="76946-193">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="76946-194">L’algorithme de hachage doit avoir une taille de condensat de > = 128 bits et doit prendre en charge en cours d’ouverture avec le BCRYPT\_ALG\_gérer\_HMAC\_indicateur d’indicateur.</span><span class="sxs-lookup"><span data-stu-id="76946-194">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="76946-195">Le \*propriétés de fournisseur peuvent être définies sur la valeur null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="76946-195">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="76946-196">Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="76946-196">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="76946-197">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de compteur Galois/Mode de chiffrement avec la validation, créez un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="76946-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="76946-198">Pour spécifier un algorithme CNG de Windows personnalisé à l’aide de compteur Galois/Mode de chiffrement avec la validation, créez un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance qui contient les informations algorithmiques :</span><span class="sxs-lookup"><span data-stu-id="76946-198">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="76946-199">L’algorithme de chiffrement symétrique doit avoir une longueur de clé > = 128 bits, une taille de bloc de 128 bits exactement, et il doit prendre en charge le chiffrement GCM.</span><span class="sxs-lookup"><span data-stu-id="76946-199">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="76946-200">Vous pouvez définir le [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propriété null pour utiliser le fournisseur par défaut pour l’algorithme spécifié.</span><span class="sxs-lookup"><span data-stu-id="76946-200">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="76946-201">Consultez le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="76946-201">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="76946-202">Spécification d’autres algorithmes personnalisés</span><span class="sxs-lookup"><span data-stu-id="76946-202">Specifying other custom algorithms</span></span>

<span data-ttu-id="76946-203">Bien que ne pas exposée comme une API de première classe, le système de Protection des données est suffisamment extensible pour autoriser la spécification de presque n’importe quel type d’algorithme.</span><span class="sxs-lookup"><span data-stu-id="76946-203">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="76946-204">Par exemple, il est possible de conserver toutes les clés contenues dans un Module de sécurité matériel (HSM) et de fournir une implémentation personnalisée de la base les routines de chiffrement et le déchiffrement.</span><span class="sxs-lookup"><span data-stu-id="76946-204">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="76946-205">Consultez [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) dans [extensibilité de chiffrement de base](xref:security/data-protection/extensibility/core-crypto) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="76946-205">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="76946-206">Clés de persistance lors de l’hébergement dans un conteneur Docker</span><span class="sxs-lookup"><span data-stu-id="76946-206">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="76946-207">Lorsque vous hébergez dans un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) , conteneur de clés doivent être conservées dans le :</span><span class="sxs-lookup"><span data-stu-id="76946-207">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="76946-208">Un dossier qui est un volume Docker qui persiste au-delà de durée de vie du conteneur, comme un volume partagé ou un volume monté par l’hôte.</span><span class="sxs-lookup"><span data-stu-id="76946-208">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="76946-209">Un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="76946-209">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76946-210">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="76946-210">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
