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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Sérialisation JSON et XML dans ASP.NET’API Web

par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit les formateurs JSON et XML dans ASP.NET’API Web.

Dans ASP.NET’API Web, un *formateur de type média* est un objet qui peut :

- Lire les objets CLR à partir d’un corps de message HTTP
- Écrivez des objets CLR dans un corps de message HTTP

Web API fournit des formateurs de type multimédia pour JSON et XML. Le cadre insère ces formateurs dans le pipeline par défaut. Les clients peuvent demander JSON ou XML dans l’en-tête Accept de la demande HTTP.

## <a name="contents"></a>Contents

- [Formateur JSON Media-Type](#json_media_type_formatter)

    - [Propriétés Read-Only](#json_readonly)
    - [Dates](#json_dates)
    - [Mise en retrait](#json_indenting)
    - [Casing de chameau](#json_camelcasing)
    - [Objets anonymes et faiblement typés](#json_anon)
- [Formateur XML Média-Type](#xml_media_type_formatter)

    - [Propriétés Read-Only](#xml_readonly)
    - [Dates](#xml_dates)
    - [Mise en retrait](#xml_indenting)
    - [Réglage par type XML Serializers](#xml_pertype)
- [Suppression du formateur JSON ou XML](#removing_the_json_or_xml_formatter)
- [Gestion des références d’objets circulaires](#handling_circular_object_references)
- [Test de sérialisation d’objets](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateur JSON Media-Type

Le formatage JSON est fourni par la classe **JsonMediaTypeFormatter.** Par défaut, **JsonMediaTypeFormatter** utilise la bibliothèque [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) pour effectuer la sérialisation. Json.NET est un projet open source tiers.

Si vous préférez, vous pouvez configurer la classe **JsonMediaTypeFormatter** pour utiliser le **DataContractJsonSerializer** au lieu de Json.NET. Pour ce faire, définissez la propriété **UseDataContractJsonSerializer** à **vrai**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Sérialisation JSON

Cette section décrit certains comportements spécifiques du formateur JSON, en utilisant le sérialisateur par défaut [Json.NET.](https://github.com/JamesNK/Newtonsoft.Json) Il ne s’agit pas d’une documentation complète de la bibliothèque Json.NET; pour plus d’informations, voir la [documentation Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Qu’est-ce qui devient sérialisé?

Par défaut, toutes les propriétés et champs publics sont inclus dans le JSON sérialisé. Pour omettre une propriété ou un champ, décorez-la avec l’attribut **JsonIgnore.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si vous &quot;préférez une&quot; approche d’opt-in, décorez la classe avec l’attribut **DataContract.** Si cet attribut est présent, les membres sont ignorés à moins qu’ils n’aient le **DataMember**. Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriétés Read-Only

Les propriétés de lecture seulement sont sérialisées par défaut.

<a id="json_dates"></a>
### <a name="dates"></a>Dates

Par défaut, Json.NET écrit les dates en format [ISO 8601.](http://www.w3.org/TR/NOTE-datetime) Les dates de l’UTC (Coordinated Universal Time) sont écrites avec un suffixe " Z ". Les dates à l’heure locale comprennent un décalage horaire. Par exemple :

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Par défaut, Json.NET préserve le fuseau horaire. Vous pouvez l’emporter en définissant la propriété DateTimeZoneHandling :

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si vous préférez utiliser [le format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu d’ISO 8601, définissez la propriété **DateFormatHandling** sur les paramètres de sérialisation :

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire en retrait JSON, définissez le paramètre **Formatting** à **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Casing de chameau

Pour écrire les noms de propriété JSON avec boîtier de chameau, sans changer votre modèle de données, définissez le **CamelCasePropertyNamesContractResolver** sur le sérialisateur :

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objets anonymes et faiblement typés

Une méthode d’action peut renvoyer un objet anonyme et le sérialiser à JSON. Par exemple :

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Le corps de message de réponse contiendra le JSON suivant :

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si votre API web reçoit des objets JSON vaguement structurés de clients, vous pouvez déséialiser le corps de demande à un type **Newtonsoft.Json.Linq.JObject.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Cependant, il est généralement préférable d’utiliser des objets de données fortement dactylographiques. Ensuite, vous n’avez pas besoin d’analyser les données vous-même, et vous obtenez les avantages de la validation du modèle.

Le sérialisateur XML ne prend pas en charge les types anonymes ou les instances **JObject.** Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML du pipeline, tel que décrit plus tard dans cet article.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateur XML Média-Type

Le formatage XML est fourni par la classe **XmlMediaTypeFormatter.** Par défaut, **XmlMediaTypeFormatter** utilise la classe **DataContractSerializer** pour effectuer la sérialisation.

Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** pour utiliser le **XmlSerializer** au lieu du **DataContractSerializer**. Pour ce faire, définissez la propriété **UseXmlSerializer** à **vrai**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La classe **XmlSerializer** prend en charge un ensemble plus étroit de types que **DataContractSerializer**, mais donne plus de contrôle sur le XML résultant. Envisagez d’utiliser **XmlSerializer** si vous avez besoin de faire correspondre un schéma XML existant.

### <a name="xml-serialization"></a>Sérialisation XML

Cette section décrit certains comportements spécifiques du formateur XML, en utilisant le **DataContractSerializer**par défaut .

Par défaut, le DataContractSerializer se comporte comme suit :

- Toutes les propriétés et les champs de lecture/écriture du public sont sérialisés. Pour omettre une propriété ou un champ, décorez-la avec l’attribut **IgnoreDataMember.**
- Les membres privés et protégés ne sont pas sérialisés.
- Les propriétés de lecture uniquement ne sont pas sérialisées. (Toutefois, le contenu d’une propriété de collecte de lecture uniquement est sérialisé.)
- Les noms des classes et des membres sont écrits dans le XML exactement comme ils apparaissent dans la déclaration de classe.
- Un espace de nom XML par défaut est utilisé.

Si vous avez besoin de plus de contrôle sur la sérialisation, vous pouvez décorer la classe avec l’attribut **DataContract.** Lorsque cet attribut est présent, la classe est sérialisée comme suit :

- &quot;Optez&quot; pour l’approche : Les propriétés et les champs ne sont pas sérialisés par défaut. Pour sérialiser une propriété ou un champ, décorez-la avec l’attribut **DataMember.**
- Pour sérialiser un membre privé ou protégé, décorez-le avec l’attribut **DataMember.**
- Les propriétés de lecture uniquement ne sont pas sérialisées.
- Pour modifier la façon dont le nom de la classe apparaît dans le XML, définissez le paramètre *Nom* dans l’attribut **DataContract.**
- Pour modifier la façon dont le nom d’un membre apparaît dans le XML, définissez le paramètre *Nom* dans **l’attribut DataMember.**
- Pour modifier l’espace de nom XML, définissez le paramètre *Namespace* dans la classe **DataContract.**

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriétés Read-Only

Les propriétés de lecture uniquement ne sont pas sérialisées. Si une propriété de lecture seulement a un champ privé de support, vous pouvez marquer le champ privé avec l’attribut **DataMember.** Cette approche nécessite l’attribut **DataContract** sur la classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Dates

Les dates sont écrites en format ISO 8601. Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire en retrait XML, définissez la propriété **Indent** à **vrai**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Réglage par type XML Serializers

Vous pouvez définir différents sérialisateurs XML pour différents types de CLR. Par exemple, vous pouvez avoir un objet de données particulier qui nécessite **XmlSerializer** pour la compatibilité vers l’arrière. Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.

Pour définir un sérialisateur XML pour un type particulier, appelez **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Vous pouvez spécifier un **XmlSerializer** ou tout objet qui dérive de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Suppression du formateur JSON ou XML

Vous pouvez supprimer le formateur JSON ou le formateur XML de la liste des formateurs, si vous ne voulez pas les utiliser. Les principales raisons de ce faire sont les suivantes:

- Limiter vos réponses D’API Web à un type multimédia particulier. Par exemple, vous pouvez décider de ne prendre en charge que les réponses JSON et de supprimer le formateur XML.
- Remplacer le formateur par défaut par un formateur personnalisé. Par exemple, vous pouvez remplacer le formateur JSON par votre propre implémentation personnalisée d’un formateur JSON.

Le code suivant montre comment supprimer les formateurs par défaut. Appelez ceci à partir de votre méthode **De démarrage\_d’application,** définie dans Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Gestion des références d’objets circulaires

Par défaut, les formateurs JSON et XML écrivent tous les objets comme valeurs. Si deux propriétés se réfèrent au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialisera l’objet deux fois. Il s’agit d’un problème particulier si votre graphique d’objet contient des cycles, parce que le sérialisateur jettera une exception quand il détecte une boucle dans le graphique.

Considérez les modèles d’objets et le contrôleur suivants.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

L’invocation de cette action amènera le formateur à lancer une exception, ce qui se traduit par une réponse de code de statut 500 (erreur de serveur interne) au client.

Pour préserver les références d’objets dans JSON, ajoutez le code suivant à la méthode **De démarrage\_d’application** dans le fichier Global.asax :

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Maintenant, l’action du contrôleur va retourner JSON qui ressemble à ceci:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Notez que le &quot;sérialisateur&quot; ajoute une propriété $id aux deux objets. En outre, il détecte que la propriété Employee.Department crée une boucle, de&quot;sorte&quot;qu’il remplace la valeur par une référence d’objet: ' $ref :&quot;1&quot;'.

> [!NOTE]
> Les références d’objets ne sont pas standard dans JSON. Avant d’utiliser cette fonctionnalité, pensez si vos clients seront en mesure d’analyser les résultats. Il pourrait être préférable de simplement supprimer les cycles du graphique. Par exemple, le lien entre l’employé et le Ministère n’est pas vraiment nécessaire dans cet exemple.

Pour préserver les références d’objets dans XML, vous avez deux options. L’option la plus `[DataContract(IsReference=true)]` simple est d’ajouter à votre classe de modèle. Le *paramètre IsReference* permet des références d’objets. N’oubliez pas que **DataContract** fait l’opt-in de sérialisation, de sorte que vous devrez également ajouter des attributs **DataMember** aux propriétés :

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Maintenant, le formateur va produire XML similaire à suivre:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si vous voulez éviter les attributs de votre classe de modèle, il existe une autre option : créer une nouvelle instance **de DataContractSerializer** spécifique au type et définir *des réservesobjectReferences* pour **la réalité** dans le constructeur. Ensuite, définissez cette instance comme un sérialisateur par type sur le formateur multimédia XML. Le code suivant montre comment le faire:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Test de sérialisation d’objets

Lorsque vous concevez votre API Web, il est utile de tester la façon dont vos objets de données seront sérialisés. Vous pouvez le faire sans créer de contrôleur ou invoquer une action de contrôleur.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
