---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 453a55bd9f16c7b33525de18bc49493d22776f47
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045115"
---
# <a name="adding-a-model"></a>Ajout d’un modèle

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes seront la &quot; partie modèle &quot; de l’application ASP.NET MVC.

Vous utiliserez une technologie d’accès aux données .NET Framework appelée [Entity Framework](https://docs.microsoft.com/ef/) pour définir et utiliser ces classes de modèle. Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*. Code First vous permet de créer des objets de modèle en écrivant des classes simples. (Ils sont également appelés classes POCO, à partir d' &quot; objets CLR ordinaires. &quot; ) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide. Si vous devez d’abord créer la base de données, vous pouvez tout de même suivre ce didacticiel pour en savoir plus sur le développement d’applications MVC et EF. Vous pouvez ensuite suivre le didacticiel de [génération de modèles](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) automatique Tom Fizmakens ASP.net, qui couvre la première approche de la base de données.

## <a name="adding-model-classes"></a>Ajout de classes de modèle

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.

![](adding-a-model/_static/image1.png)

Entrez le nom de la *classe* &quot; Movie &quot; .

Ajoutez les cinq propriétés suivantes à la `Movie` classe :

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Nous allons utiliser la `Movie` classe pour représenter des films dans une base de données. Chaque instance d’un `Movie` objet correspond à une ligne dans une table de base de données, et chaque propriété de la `Movie` classe est mappée à une colonne de la table.

Remarque : pour utiliser System. Data. Entity et la classe connexe, vous devez installer le [package NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Suivez le lien pour obtenir des instructions supplémentaires.

Dans le même fichier, ajoutez la `MovieDBContext` classe suivante :

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

La `MovieDBContext` classe représente le contexte de la base de données de films Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` des instances de classe dans une base de données. Le `MovieDBContext` dérive de la `DbContext` classe de base fournie par l’Entity Framework.

Pour pouvoir référencer `DbContext` et `DbSet` , vous devez ajouter l' `using` instruction suivante en haut du fichier :

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Pour ce faire, vous pouvez ajouter manuellement l’instruction using ou vous pouvez pointer sur les lignes ondulées rouges, cliquer `Show potential fixes` et cliquer sur `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Remarque : plusieurs instructions inutilisées `using` ont été supprimées. Visual Studio affiche les dépendances inutilisées en gris. Vous pouvez supprimer les dépendances inutilisées en pointant sur les dépendances grises, puis en cliquant sur `Show potential fixes` **Supprimer les using inutilisées.**

![](adding-a-model/_static/image3.png)

Nous avons enfin ajouté un modèle (le M dans MVC). Dans la section suivante, vous allez utiliser la chaîne de connexion à la base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md) 
>  [Suivant](creating-a-connection-string.md)
