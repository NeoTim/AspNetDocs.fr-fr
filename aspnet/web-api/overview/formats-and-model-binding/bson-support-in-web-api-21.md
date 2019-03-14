---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Prise en charge BSON dans ASP.NET Web API 2.1 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061756"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Prise en charge BSON dans ASP.NET Web API 2.1
====================
par [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 introduit la prise en charge de BSON. Cette rubrique montre comment utiliser BSON dans votre contrôleur d’API Web (côté serveur) et dans une application cliente .NET.

## <a name="what-is-bson"></a>Qu’est BSON ?

[BSON](http://bsonspec.org/) est un format de sérialisation binaire. « BSON » est l’acronyme « JSON binaire », mais BSON and JSON sont sérialisés de manière très différente. BSON est « Similaires à JSON », car les objets sont représentés sous forme de paires nom-valeur, similaires au format JSON. Contrairement à JSON, les types de données numériques sont stockés sous forme d’octets, pas des chaînes

BSON a été conçu pour être léger, facile à analyser et rapides à Encoder/décoder.

- BSON est comparable au format JSON. En fonction des données, une charge utile BSON peut être inférieure ou supérieure à une charge utile JSON. Pour sérialiser les données binaires, comme un fichier image, BSON est inférieure à JSON, car les données binaires ne sont pas codée en base64.
- BSON documents sont faciles à analyser, car les éléments sont préfixés avec un champ de longueur, un analyseur et pouvez donc ignorer éléments sans leur décodage.
- Encodage et décodage sont efficaces, car les types de données numériques sont stockés sous forme de nombres, pas des chaînes.

Les clients natifs, tels que les applications clientes .NET, peuvent tirer parti de BSON à la place de formats texte tels que JSON ou XML. Pour les clients de navigateur, vous souhaiterez probablement cantonner à JSON, étant donné que JavaScript peut convertir directement de la charge utile JSON.

Heureusement, les API Web utilise [négociation de contenu](content-negotiation.md), de sorte que votre API peut prendre en charge les deux formats et choisir le client.

## <a name="enabling-bson-on-the-server"></a>L’activation de BSON sur le serveur

Dans la configuration de votre API Web, ajoutez le **BsonMediaTypeFormatter** à la collection de formateurs.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Maintenant si le client demande « application/bson », API Web utilise le formateur BSON.

Pour associer BSON avec d’autres types de média, ajoutez-les à la collection SupportedMediaTypes. Le code suivant ajoute « application/vnd.contoso » pour les types de médias pris en charge :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Exemple de Session HTTP

Pour cet exemple, nous allons utiliser la classe de modèle suivante ainsi qu’un contrôleur d’API Web simple :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un client peut envoyer la requête HTTP suivante :

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Voici la réponse :

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Ici, j’ai remplacé les données binaires avec &quot;.&quot; caractères. La capture d’écran suivante de montre de Fiddler les valeurs hexadécimales brutes.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>À l’aide de BSON avec HttpClient

Applications de clients .NET peuvent utiliser le formateur BSON avec **HttpClient**. Pour plus d’informations sur **HttpClient**, consultez [appelant une Web API à partir d’un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Le code suivant envoie une requête GET qui accepte BSON et puis désérialise la charge utile BSON dans la réponse.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Pour demander BSON à partir du serveur, définissez l’en-tête Accept par « application/bson » :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Pour désérialiser le corps de réponse, utilisez le **BsonMediaTypeFormatter**. Ce formateur n’est pas dans la collection de formateurs par défaut, afin d’avoir à le spécifier lorsque vous lisez le corps de réponse :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

L’exemple suivant montre comment envoyer une demande POST qui contient BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Une grande partie de ce code est identique à l’exemple précédent. Mais, dans le **PostAsync** (méthode), spécifiez **BsonMediaTypeFormatter** en tant que le formateur :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Sérialisation des Types primitifs de niveau supérieur

Chaque document BSON est une liste de paires clé/valeur. La spécification BSON ne définit pas une syntaxe pour la sérialisation d’une seule valeur brute, tel qu’un entier ou une chaîne.

Pour contourner cette limitation, le **BsonMediaTypeFormatter** traite des types primitifs comme un cas spécial. Avant de sérialiser, il convertit la valeur en une paire clé/valeur avec la clé « Valeur ». Par exemple, supposons que votre contrôleur d’API retourne un entier :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Avant de sérialiser, le formateur BSON convertit à la paire clé/valeur suivantes :

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Lorsque vous désérialisez, le formateur convertit les données à la valeur d’origine. Toutefois, les clients à l’aide d’un autre analyseur BSON devront gérer ce cas, si votre API web renvoie les valeurs brutes. En règle générale, vous devez envisager de retourner des données structurées, plutôt que les valeurs brutes.

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple d’API BSON Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formateurs de médias](media-formatters.md)
