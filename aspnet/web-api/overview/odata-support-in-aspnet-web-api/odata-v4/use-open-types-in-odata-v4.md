---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Types ouverts en OData v4 avec ASP.NET’API Web (fr) Microsoft Docs
author: rick-anderson
description: Dans OData v4, un type ouvert est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Ouvrir…
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d63c96df6614896b3b67eef94a8907e528572567
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543728"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Open Types en OData v4 avec ASP.NET’API Web

par [Microsoft](https://github.com/microsoft)

> Dans OData v4, un *type ouvert* est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données. Ce tutoriel montre comment utiliser les types ouverts dans ASP.NET Web API OData.
> 
> Ce tutoriel suppose que vous savez déjà comment créer un point de terminaison OData dans ASP.NET’API Web. Si ce n’est pas le cas, commencez par lire [Créer un point de terminaison OData v4 d’abord.](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le tutoriel
> 
> 
> - Web API OData 5.3
> - OData v4

Tout d’abord, une certaine terminologie OData:

- Type d’entité : Type structuré avec une clé.
- Type complexe : Type structuré sans clé.
- Type ouvert : Type avec des propriétés dynamiques. Les deux types d’entités et les types complexes peuvent être ouverts.

La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type d’énumération; ou une collection de l’un de ces types. Pour plus d’informations sur les types ouverts, voir la [spécification OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installer les bibliothèques Web OData

Utilisez NuGet Package Manager pour installer les dernières bibliothèques Web API OData. À partir de la fenêtre Console du Gestionnaire de package :

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Décrivez les types CLR

Commencez par définir les modèles EDM comme types CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Lorsque le modèle de données d’entité (EDM) est créé,

- `Category`est un type d’énumération.
- `Address`est un type complexe. (Il n’a pas de clé, donc ce n’est pas un type d’entité.)
- `Customer`est un type d’entité. (Il a une clé.)
- `Press`est un type complexe ouvert.
- `Book`est un type d’entité ouverte.

Pour créer un type ouvert, le type CLR doit avoir une propriété de type, `IDictionary<string, object>`qui détient les propriétés dynamiques.

## <a name="build-the-edm-model"></a>Construire le modèle EDM

Si vous utilisez **ODataConventionModelBuilder** pour créer `Press` `Book` l’EDM, et sont automatiquement `IDictionary<string, object>` ajoutés en tant que types ouverts, en fonction de la présence d’une propriété.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Vous pouvez également construire l’EDM explicitement, en utilisant **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Ajouter un contrôleur OData

Ensuite, ajoutez un contrôleur OData. Pour ce tutoriel, nous allons utiliser un contrôleur simplifié qui prend simplement en charge les demandes GET et POST, et utilise une liste de mémoire pour stocker des entités.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Notez que `Book` la première instance n’a pas de propriétés dynamiques. La `Book` deuxième instance a les propriétés dynamiques suivantes :

- "Publié": Type primitif
- "Auteurs": Collection de types primitifs
- "Autrescategories": Collection de types d’énumération.

En outre, `Press` la `Book` propriété de cette instance a les propriétés dynamiques suivantes:

- "Blog": Type primitif
- "Adresse": Type complexe

## <a name="query-the-metadata"></a>Requête des métadonnées

Pour obtenir le document de métadonnées OData, envoyez une demande GET à `~/$metadata`. L’organisme de réponse devrait ressembler à ceci :

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

D’après le document sur les métadonnées, vous pouvez voir que :

- Pour `Book` les `Press` types et les `OpenType` types, la valeur de l’attribut est vraie. Les `Customer` `Address` types et les types n’ont pas cet attribut.
- Le `Book` type d’entité a trois propriétés déclarées : ISBN, Titre et Presse. Les métadonnées OData `Book.Properties` n’incluent pas la propriété de la classe CLR.
- De même, le `Press` type complexe n’a que deux propriétés déclarées : nom et catégorie. Les métadonnées n’incluent pas la `Press.DynamicProperties` propriété de la classe CLR.

## <a name="query-an-entity"></a>Requête d’une entité

Pour obtenir le livre avec ISBN égal à "978-0-7356-7942-9", envoyer une demande GET à `~/Books('978-0-7356-7942-9')`. Le corps de réponse doit ressembler à ce qui suit. (Indented pour le rendre plus lisible.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Notez que les propriétés dynamiques sont incluses en ligne avec les propriétés déclarées.

## <a name="post-an-entity"></a>POST une entité

Pour ajouter une entité De livre, envoyez une demande POST à `~/Books`. Le client peut définir des propriétés dynamiques dans la charge utile de demande.

Voici un exemple de demande. Notez les propriétés "Prix" et "Publié".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si vous définissez un point d’arrêt dans la méthode du `Properties` contrôleur, vous pouvez voir que l’API Web a ajouté ces propriétés au dictionnaire.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Échantillon de type ouvert OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
