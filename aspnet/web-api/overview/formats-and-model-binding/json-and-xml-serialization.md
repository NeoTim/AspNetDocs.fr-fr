---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans ASP.NET’API Web - ASP.NET 4.x
author: MikeWasson
description: Décrit les formateurs JSON et XML dans ASP.NET’API Web pour ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e6e02fa1c48e9c5fb8499379508619ddb317ccc9
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676222"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="7e7bd-103">Sérialisation JSON et XML dans ASP.NET’API Web</span><span class="sxs-lookup"><span data-stu-id="7e7bd-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="7e7bd-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7e7bd-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7e7bd-105">Cet article décrit les formateurs JSON et XML dans ASP.NET’API Web.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="7e7bd-106">Dans ASP.NET’API Web, un *formateur de type média* est un objet qui peut :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="7e7bd-107">Lire les objets CLR à partir d’un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="7e7bd-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="7e7bd-108">Écrivez des objets CLR dans un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="7e7bd-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="7e7bd-109">Web API fournit des formateurs de type multimédia pour JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="7e7bd-110">Le cadre insère ces formateurs dans le pipeline par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="7e7bd-111">Les clients peuvent demander JSON ou XML dans l’en-tête Accept de la demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="7e7bd-112">Contents</span><span class="sxs-lookup"><span data-stu-id="7e7bd-112">Contents</span></span>

- [<span data-ttu-id="7e7bd-113">Formateur JSON Media-Type</span><span class="sxs-lookup"><span data-stu-id="7e7bd-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="7e7bd-114">Propriétés Read-Only</span><span class="sxs-lookup"><span data-stu-id="7e7bd-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="7e7bd-115">Dates</span><span class="sxs-lookup"><span data-stu-id="7e7bd-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="7e7bd-116">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="7e7bd-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="7e7bd-117">Casing de chameau</span><span class="sxs-lookup"><span data-stu-id="7e7bd-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="7e7bd-118">Objets anonymes et faiblement typés</span><span class="sxs-lookup"><span data-stu-id="7e7bd-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="7e7bd-119">Formateur XML Média-Type</span><span class="sxs-lookup"><span data-stu-id="7e7bd-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="7e7bd-120">Propriétés Read-Only</span><span class="sxs-lookup"><span data-stu-id="7e7bd-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="7e7bd-121">Dates</span><span class="sxs-lookup"><span data-stu-id="7e7bd-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="7e7bd-122">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="7e7bd-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="7e7bd-123">Réglage par type XML Serializers</span><span class="sxs-lookup"><span data-stu-id="7e7bd-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="7e7bd-124">Suppression du formateur JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="7e7bd-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="7e7bd-125">Gestion des références d’objets circulaires</span><span class="sxs-lookup"><span data-stu-id="7e7bd-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="7e7bd-126">Test de sérialisation d’objets</span><span class="sxs-lookup"><span data-stu-id="7e7bd-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="7e7bd-127">Formateur JSON Media-Type</span><span class="sxs-lookup"><span data-stu-id="7e7bd-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="7e7bd-128">Le formatage JSON est fourni par la classe **JsonMediaTypeFormatter.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="7e7bd-129">Par défaut, **JsonMediaTypeFormatter** utilise la bibliothèque [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="7e7bd-130">Json.NET est un projet open source tiers.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="7e7bd-131">Si vous préférez, vous pouvez configurer la classe **JsonMediaTypeFormatter** pour utiliser le **DataContractJsonSerializer** au lieu de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="7e7bd-132">Pour ce faire, définissez la propriété **UseDataContractJsonSerializer** à **vrai**:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="7e7bd-133">Sérialisation JSON</span><span class="sxs-lookup"><span data-stu-id="7e7bd-133">JSON Serialization</span></span>

<span data-ttu-id="7e7bd-134">Cette section décrit certains comportements spécifiques du formateur JSON, en utilisant le sérialisateur par défaut [Json.NET.](https://github.com/JamesNK/Newtonsoft.Json)</span><span class="sxs-lookup"><span data-stu-id="7e7bd-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="7e7bd-135">Il ne s’agit pas d’une documentation complète de la bibliothèque Json.NET; pour plus d’informations, voir la [documentation Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="7e7bd-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="7e7bd-136">Qu’est-ce qui devient sérialisé?</span><span class="sxs-lookup"><span data-stu-id="7e7bd-136">What Gets Serialized?</span></span>

<span data-ttu-id="7e7bd-137">Par défaut, toutes les propriétés et champs publics sont inclus dans le JSON sérialisé.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="7e7bd-138">Pour omettre une propriété ou un champ, décorez-la avec l’attribut **JsonIgnore.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="7e7bd-139">Si vous &quot;préférez une&quot; approche d’opt-in, décorez la classe avec l’attribut **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="7e7bd-140">Si cet attribut est présent, les membres sont ignorés à moins qu’ils n’aient le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="7e7bd-141">Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="7e7bd-142">Propriétés Read-Only</span><span class="sxs-lookup"><span data-stu-id="7e7bd-142">Read-Only Properties</span></span>

<span data-ttu-id="7e7bd-143">Les propriétés de lecture seulement sont sérialisées par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="7e7bd-144">Dates</span><span class="sxs-lookup"><span data-stu-id="7e7bd-144">Dates</span></span>

<span data-ttu-id="7e7bd-145">Par défaut, Json.NET écrit les dates en format [ISO 8601.](http://www.w3.org/TR/NOTE-datetime)</span><span class="sxs-lookup"><span data-stu-id="7e7bd-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="7e7bd-146">Les dates de l’UTC (Coordinated Universal Time) sont écrites avec un suffixe " Z ".</span><span class="sxs-lookup"><span data-stu-id="7e7bd-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="7e7bd-147">Les dates à l’heure locale comprennent un décalage horaire.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="7e7bd-148">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="7e7bd-149">Par défaut, Json.NET préserve le fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="7e7bd-150">Vous pouvez l’emporter en définissant la propriété DateTimeZoneHandling :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="7e7bd-151">Si vous préférez utiliser [le format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu d’ISO 8601, définissez la propriété **DateFormatHandling** sur les paramètres de sérialisation :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="7e7bd-152">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="7e7bd-152">Indenting</span></span>

<span data-ttu-id="7e7bd-153">Pour écrire en retrait JSON, définissez le paramètre **Formatting** à **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="7e7bd-154">Casing de chameau</span><span class="sxs-lookup"><span data-stu-id="7e7bd-154">Camel Casing</span></span>

<span data-ttu-id="7e7bd-155">Pour écrire les noms de propriété JSON avec boîtier de chameau, sans changer votre modèle de données, définissez le **CamelCasePropertyNamesContractResolver** sur le sérialisateur :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="7e7bd-156">Objets anonymes et faiblement typés</span><span class="sxs-lookup"><span data-stu-id="7e7bd-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="7e7bd-157">Une méthode d’action peut renvoyer un objet anonyme et le sérialiser à JSON.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="7e7bd-158">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="7e7bd-159">Le corps de message de réponse contiendra le JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="7e7bd-160">Si votre API web reçoit des objets JSON vaguement structurés de clients, vous pouvez déséialiser le corps de demande à un type **Newtonsoft.Json.Linq.JObject.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="7e7bd-161">Cependant, il est généralement préférable d’utiliser des objets de données fortement dactylographiques.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="7e7bd-162">Ensuite, vous n’avez pas besoin d’analyser les données vous-même, et vous obtenez les avantages de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="7e7bd-163">Le sérialisateur XML ne prend pas en charge les types anonymes ou les instances **JObject.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="7e7bd-164">Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML du pipeline, tel que décrit plus tard dans cet article.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="7e7bd-165">Formateur XML Média-Type</span><span class="sxs-lookup"><span data-stu-id="7e7bd-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="7e7bd-166">Le formatage XML est fourni par la classe **XmlMediaTypeFormatter.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="7e7bd-167">Par défaut, **XmlMediaTypeFormatter** utilise la classe **DataContractSerializer** pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="7e7bd-168">Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** pour utiliser le **XmlSerializer** au lieu du **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="7e7bd-169">Pour ce faire, définissez la propriété **UseXmlSerializer** à **vrai**:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="7e7bd-170">La classe **XmlSerializer** prend en charge un ensemble plus étroit de types que **DataContractSerializer**, mais donne plus de contrôle sur le XML résultant.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="7e7bd-171">Envisagez d’utiliser **XmlSerializer** si vous avez besoin de faire correspondre un schéma XML existant.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="7e7bd-172">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="7e7bd-172">XML Serialization</span></span>

<span data-ttu-id="7e7bd-173">Cette section décrit certains comportements spécifiques du formateur XML, en utilisant le **DataContractSerializer**par défaut .</span><span class="sxs-lookup"><span data-stu-id="7e7bd-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="7e7bd-174">Par défaut, le DataContractSerializer se comporte comme suit :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="7e7bd-175">Toutes les propriétés et les champs de lecture/écriture du public sont sérialisés.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="7e7bd-176">Pour omettre une propriété ou un champ, décorez-la avec l’attribut **IgnoreDataMember.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="7e7bd-177">Les membres privés et protégés ne sont pas sérialisés.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="7e7bd-178">Les propriétés de lecture uniquement ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="7e7bd-179">(Toutefois, le contenu d’une propriété de collecte de lecture uniquement est sérialisé.)</span><span class="sxs-lookup"><span data-stu-id="7e7bd-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="7e7bd-180">Les noms des classes et des membres sont écrits dans le XML exactement comme ils apparaissent dans la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="7e7bd-181">Un espace de nom XML par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-181">A default XML namespace is used.</span></span>

<span data-ttu-id="7e7bd-182">Si vous avez besoin de plus de contrôle sur la sérialisation, vous pouvez décorer la classe avec l’attribut **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="7e7bd-183">Lorsque cet attribut est présent, la classe est sérialisée comme suit :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="7e7bd-184">&quot;Optez&quot; pour l’approche : Les propriétés et les champs ne sont pas sérialisés par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="7e7bd-185">Pour sérialiser une propriété ou un champ, décorez-la avec l’attribut **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="7e7bd-186">Pour sérialiser un membre privé ou protégé, décorez-le avec l’attribut **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="7e7bd-187">Les propriétés de lecture uniquement ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="7e7bd-188">Pour modifier la façon dont le nom de la classe apparaît dans le XML, définissez le paramètre *Nom* dans l’attribut **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="7e7bd-189">Pour modifier la façon dont le nom d’un membre apparaît dans le XML, définissez le paramètre *Nom* dans **l’attribut DataMember.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="7e7bd-190">Pour modifier l’espace de nom XML, définissez le paramètre *Namespace* dans la classe **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="7e7bd-191">Propriétés Read-Only</span><span class="sxs-lookup"><span data-stu-id="7e7bd-191">Read-Only Properties</span></span>

<span data-ttu-id="7e7bd-192">Les propriétés de lecture uniquement ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="7e7bd-193">Si une propriété de lecture seulement a un champ privé de support, vous pouvez marquer le champ privé avec l’attribut **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="7e7bd-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="7e7bd-194">Cette approche nécessite l’attribut **DataContract** sur la classe.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="7e7bd-195">Dates</span><span class="sxs-lookup"><span data-stu-id="7e7bd-195">Dates</span></span>

<span data-ttu-id="7e7bd-196">Les dates sont écrites en format ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="7e7bd-197">Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="7e7bd-198">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="7e7bd-198">Indenting</span></span>

<span data-ttu-id="7e7bd-199">Pour écrire en retrait XML, définissez la propriété **Indent** à **vrai**:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="7e7bd-200">Réglage par type XML Serializers</span><span class="sxs-lookup"><span data-stu-id="7e7bd-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="7e7bd-201">Vous pouvez définir différents sérialisateurs XML pour différents types de CLR.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="7e7bd-202">Par exemple, vous pouvez avoir un objet de données particulier qui nécessite **XmlSerializer** pour la compatibilité vers l’arrière.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="7e7bd-203">Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="7e7bd-204">Pour définir un sérialisateur XML pour un type particulier, appelez **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="7e7bd-205">Vous pouvez spécifier un **XmlSerializer** ou tout objet qui dérive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="7e7bd-206">Suppression du formateur JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="7e7bd-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="7e7bd-207">Vous pouvez supprimer le formateur JSON ou le formateur XML de la liste des formateurs, si vous ne voulez pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="7e7bd-208">Les principales raisons de ce faire sont les suivantes:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="7e7bd-209">Limiter vos réponses D’API Web à un type multimédia particulier.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="7e7bd-210">Par exemple, vous pouvez décider de ne prendre en charge que les réponses JSON et de supprimer le formateur XML.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="7e7bd-211">Remplacer le formateur par défaut par un formateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="7e7bd-212">Par exemple, vous pouvez remplacer le formateur JSON par votre propre implémentation personnalisée d’un formateur JSON.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="7e7bd-213">Le code suivant montre comment supprimer les formateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="7e7bd-214">Appelez ceci à partir de votre méthode **De démarrage\_d’application,** définie dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="7e7bd-215">Gestion des références d’objets circulaires</span><span class="sxs-lookup"><span data-stu-id="7e7bd-215">Handling Circular Object References</span></span>

<span data-ttu-id="7e7bd-216">Par défaut, les formateurs JSON et XML écrivent tous les objets comme valeurs.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="7e7bd-217">Si deux propriétés se réfèrent au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialisera l’objet deux fois.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="7e7bd-218">Il s’agit d’un problème particulier si votre graphique d’objet contient des cycles, parce que le sérialisateur jettera une exception quand il détecte une boucle dans le graphique.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="7e7bd-219">Considérez les modèles d’objets et le contrôleur suivants.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="7e7bd-220">L’invocation de cette action amènera le formateur à lancer une exception, ce qui se traduit par une réponse de code de statut 500 (erreur de serveur interne) au client.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-220">Invoking this action will cause the formatter to throw an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="7e7bd-221">Pour préserver les références d’objets dans JSON, ajoutez le code suivant à la méthode **De démarrage\_d’application** dans le fichier Global.asax :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="7e7bd-222">Maintenant, l’action du contrôleur va retourner JSON qui ressemble à ceci:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="7e7bd-223">Notez que le &quot;sérialisateur&quot; ajoute une propriété $id aux deux objets.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="7e7bd-224">En outre, il détecte que la propriété Employee.Department crée une boucle, de&quot;sorte&quot;qu’il remplace la valeur par une référence d’objet: ' $ref :&quot;1&quot;'.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="7e7bd-225">Les références d’objets ne sont pas standard dans JSON.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="7e7bd-226">Avant d’utiliser cette fonctionnalité, pensez si vos clients seront en mesure d’analyser les résultats.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="7e7bd-227">Il pourrait être préférable de simplement supprimer les cycles du graphique.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="7e7bd-228">Par exemple, le lien entre l’employé et le Ministère n’est pas vraiment nécessaire dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="7e7bd-229">Pour préserver les références d’objets dans XML, vous avez deux options.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="7e7bd-230">L’option la plus `[DataContract(IsReference=true)]` simple est d’ajouter à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="7e7bd-231">Le *paramètre IsReference* permet des références d’objets.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="7e7bd-232">N’oubliez pas que **DataContract** fait l’opt-in de sérialisation, de sorte que vous devrez également ajouter des attributs **DataMember** aux propriétés :</span><span class="sxs-lookup"><span data-stu-id="7e7bd-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="7e7bd-233">Maintenant, le formateur va produire XML similaire à suivre:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="7e7bd-234">Si vous voulez éviter les attributs de votre classe de modèle, il existe une autre option : créer une nouvelle instance **de DataContractSerializer** spécifique au type et définir *des réservesobjectReferences* pour **la réalité** dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="7e7bd-235">Ensuite, définissez cette instance comme un sérialisateur par type sur le formateur multimédia XML.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="7e7bd-236">Le code suivant montre comment le faire:</span><span class="sxs-lookup"><span data-stu-id="7e7bd-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="7e7bd-237">Test de sérialisation d’objets</span><span class="sxs-lookup"><span data-stu-id="7e7bd-237">Testing Object Serialization</span></span>

<span data-ttu-id="7e7bd-238">Lorsque vous concevez votre API Web, il est utile de tester la façon dont vos objets de données seront sérialisés.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="7e7bd-239">Vous pouvez le faire sans créer de contrôleur ou invoquer une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7e7bd-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
