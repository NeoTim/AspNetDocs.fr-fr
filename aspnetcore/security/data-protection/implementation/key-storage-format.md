---
title: Format de stockage de clés dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation du format de stockage de clés de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bca19ad001dd20b5d02ae5470f7d928082496037
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044816"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="89ae5-103">Format de stockage de clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89ae5-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="89ae5-104">Les objets sont stockées au repos dans la représentation XML.</span><span class="sxs-lookup"><span data-stu-id="89ae5-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="89ae5-105">Le répertoire par défaut pour le stockage de clés est % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="89ae5-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="89ae5-106">Le \<clé > élément</span><span class="sxs-lookup"><span data-stu-id="89ae5-106">The \<key> element</span></span>

<span data-ttu-id="89ae5-107">Clés existent en tant qu’objets de niveau supérieur dans le référentiel de clé.</span><span class="sxs-lookup"><span data-stu-id="89ae5-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="89ae5-108">Par convention, les clés ont le nom de fichier **key-{guid} .xml**, où {guid} est l’id de la clé.</span><span class="sxs-lookup"><span data-stu-id="89ae5-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="89ae5-109">Chacun de ces fichiers contient une clé unique.</span><span class="sxs-lookup"><span data-stu-id="89ae5-109">Each such file contains a single key.</span></span> <span data-ttu-id="89ae5-110">Voici le format du fichier.</span><span class="sxs-lookup"><span data-stu-id="89ae5-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="89ae5-111">Le \<clé > élément contient les attributs suivants et les éléments enfants :</span><span class="sxs-lookup"><span data-stu-id="89ae5-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="89ae5-112">L’id de clé. Cette valeur est traitée comme faisant autorité ; le nom de fichier est simplement une subtilité pour une meilleure lisibilité humaine.</span><span class="sxs-lookup"><span data-stu-id="89ae5-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="89ae5-113">La version de la \<clé > élément, actuellement fixée à 1.</span><span class="sxs-lookup"><span data-stu-id="89ae5-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="89ae5-114">Dates de création, l’activation et l’expiration de la clé.</span><span class="sxs-lookup"><span data-stu-id="89ae5-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="89ae5-115">Un \<descripteur > élément, qui contient des informations sur l’implémentation du chiffrement authentifié contenue dans cette clé.</span><span class="sxs-lookup"><span data-stu-id="89ae5-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="89ae5-116">Dans l’exemple ci-dessus, l’id de la clé est {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, il a été créé et activé sur le 19 mars 2015, et il a une durée de vie de 90 jours.</span><span class="sxs-lookup"><span data-stu-id="89ae5-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="89ae5-117">(Il peut arriver que la date d’activation peut être légèrement avant la date de création, comme dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="89ae5-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="89ae5-118">Cela est dû à un CD/m² dans le fonctionnement des API et ne présente aucun danger dans la pratique.)</span><span class="sxs-lookup"><span data-stu-id="89ae5-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="89ae5-119">Le \<descripteur > élément</span><span class="sxs-lookup"><span data-stu-id="89ae5-119">The \<descriptor> element</span></span>

<span data-ttu-id="89ae5-120">Externe \<descripteur > élément contient un attribut deserializerType, qui est le nom qualifié d’assembly d’un type qui implémente IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="89ae5-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="89ae5-121">Ce type est responsable de lire les interne \<descripteur > élément et pour analyser les informations contenues dans.</span><span class="sxs-lookup"><span data-stu-id="89ae5-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="89ae5-122">Le format particulier de la \<descripteur > élément dépend de l’implémentation de chiffreur authentifié encapsulée par la clé, et chaque type de désérialiseur attend un format légèrement différent pour cela.</span><span class="sxs-lookup"><span data-stu-id="89ae5-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="89ae5-123">En général, cependant, cet élément contiendra des informations algorithmiques (les noms, types, OID, ou similaire) et le matériel de clé secrète.</span><span class="sxs-lookup"><span data-stu-id="89ae5-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="89ae5-124">Dans l’exemple ci-dessus, le descripteur spécifie que cette clé encapsule le chiffrement AES-256-CBC + HMACSHA256 validation.</span><span class="sxs-lookup"><span data-stu-id="89ae5-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="89ae5-125">Le \<encryptedSecret > élément</span><span class="sxs-lookup"><span data-stu-id="89ae5-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="89ae5-126">Un **&lt;encryptedSecret&gt;** élément qui contient la forme chiffrée de la clé secrète peut être présent si [le chiffrement des secrets au repos est activé](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="89ae5-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="89ae5-127">L’attribut `decryptorType` est le nom qualifié d’assembly d’un type qui implémente [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="89ae5-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="89ae5-128">Ce type est responsable de lire les interne **&lt;encryptedKey&gt;** élément et le déchiffrement pour récupérer le texte brut d’origine.</span><span class="sxs-lookup"><span data-stu-id="89ae5-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="89ae5-129">Comme avec \<descripteur >, le format particulier de la <encryptedSecret> élément varie selon le mécanisme de chiffrement au repos en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="89ae5-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="89ae5-130">Dans l’exemple ci-dessus, la clé principale est chiffrée à l’aide de DPAPI de Windows par le commentaire.</span><span class="sxs-lookup"><span data-stu-id="89ae5-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="89ae5-131">Le \<révocation > élément</span><span class="sxs-lookup"><span data-stu-id="89ae5-131">The \<revocation> element</span></span>

<span data-ttu-id="89ae5-132">Révocations existent en tant qu’objets de niveau supérieur dans le référentiel de clé.</span><span class="sxs-lookup"><span data-stu-id="89ae5-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="89ae5-133">Par convention révocations ont le nom de fichier **révocation-{horodatage} .xml** (pour la révocation de toutes les clés avant une date spécifique) ou **révocation-{guid} .xml** (pour la révocation d’une clé spécifique).</span><span class="sxs-lookup"><span data-stu-id="89ae5-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="89ae5-134">Chaque fichier contienne un seul \<révocation > élément.</span><span class="sxs-lookup"><span data-stu-id="89ae5-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="89ae5-135">Pour les révocations de clés individuelles, le contenu du fichier sera comme suit.</span><span class="sxs-lookup"><span data-stu-id="89ae5-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="89ae5-136">Dans ce cas, uniquement la clé spécifiée est révoquée.</span><span class="sxs-lookup"><span data-stu-id="89ae5-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="89ae5-137">Si l’id de clé est « \* », toutefois, comme dans l’exemple ci-dessous, dont la date de création est antérieure à la date spécifiée de la révocation de toutes les clés sont révoquées.</span><span class="sxs-lookup"><span data-stu-id="89ae5-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="89ae5-138">Le \<raison > élément n’est jamais lue par le système.</span><span class="sxs-lookup"><span data-stu-id="89ae5-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="89ae5-139">Il est simplement un emplacement pratique pour stocker une raison explicite de la révocation.</span><span class="sxs-lookup"><span data-stu-id="89ae5-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
