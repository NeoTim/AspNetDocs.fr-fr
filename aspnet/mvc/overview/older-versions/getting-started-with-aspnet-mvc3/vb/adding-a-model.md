---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Ajout d’un modèle (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b1370148018faa8c6c884251bfa86761f45d7e49
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058046"
---
<a name="adding-a-model-vb"></a>Ajout d’un modèle (VB)
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/adding-a-model.md) de ce didacticiel.


## <a name="adding-a-model"></a>Ajout d’un modèle

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes seront la partie « modèle » de l’application ASP.NET MVC.

Vous allez utiliser une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle. Le prend en charge Entity Framework (souvent appelée EF) un paradigme de développement appelé *Code First*. Code tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples. (Ils sont également appelés classes POCO, « objet de CLR traditionnel ».) Vous avez ensuite la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très concis et rapide.

## <a name="adding-model-classes"></a>Ajout de Classes de modèle

Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.

![](adding-a-model/_static/image1.png)

Nommez la classe « Film ».

Ajoutez les propriétés suivantes de cinq à la `Movie` classe :

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Nous allons utiliser le `Movie` classe pour représenter des films dans une base de données. Chaque instance d’un `Movie` objet correspond à une ligne d’une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.

Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

Le `MovieDBContext` classe représente le contexte de base de données de film de Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` instances dans une base de données de la classe. Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base. Pour plus d’informations sur `DbContext` et `DbSet`, consultez [des améliorations de productivité pour Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter le code suivant `imports` instruction en haut du fichier :

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

L’ensemble *Movie.vb* fichier est présenté ci-dessous.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Création d’une chaîne de connexion et l’utilisation de SQL Server Compact

Le `MovieDBContext` classe que vous avez créé gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données. Une question que vous vous demandez peut-être, cependant, consiste à spécifier quelle base de données, il se connecte à. Vous le ferez en ajoutant des informations de connexion dans le *Web.config* fichier de l’application.

Ouvrez la racine de l’application *Web.config* fichier. (Pas le *Web.config* de fichiers dans le *vues* dossier.) L’image ci-dessous affiche les deux *Web.config* fichiers ; ouvrir le *Web.config* fichier entourée en rouge.

![](adding-a-model/_static/image2.png)

## 

Ajoutez la chaîne de connexion suivante à la `<connectionStrings>` élément dans le *Web.config* fichier.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Cette petite quantité de code et le XML est tout ce que vous devez écrire pour représenter et de stocker les données de films dans une base de données.

Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
