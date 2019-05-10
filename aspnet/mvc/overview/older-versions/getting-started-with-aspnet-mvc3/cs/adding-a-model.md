---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Ajout d’un modèle (c#) | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 8cf8693b71509163860c78a87370c4765414fd5d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130333"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="4e4c3-104">Ajout d’un modèle (C#)</span><span class="sxs-lookup"><span data-stu-id="4e4c3-104">Adding a Model (C#)</span></span>

<span data-ttu-id="4e4c3-105">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4e4c3-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="4e4c3-106">Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="4e4c3-107">Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="4e4c3-108">Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="4e4c3-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="4e4c3-109">Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :</span><span class="sxs-lookup"><span data-stu-id="4e4c3-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="4e4c3-110">Prérequis pour le Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="4e4c3-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="4e4c3-111">Mettre à jour des outils ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="4e4c3-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="4e4c3-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)</span><span class="sxs-lookup"><span data-stu-id="4e4c3-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="4e4c3-113">Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="4e4c3-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="4e4c3-114">Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="4e4c3-115">[Téléchargez la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="4e4c3-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="4e4c3-116">Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/adding-a-model.md) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="4e4c3-117">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="4e4c3-117">Adding a Model</span></span>

<span data-ttu-id="4e4c3-118">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="4e4c3-119">Ces classes seront la partie « modèle » de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="4e4c3-120">Vous allez utiliser une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="4e4c3-121">Le prend en charge Entity Framework (souvent appelée EF) un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="4e4c3-122">Code tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="4e4c3-123">(Ils sont également appelés classes POCO, « objet de CLR traditionnel ».) Vous avez ensuite la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très concis et rapide.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="4e4c3-124">Ajout de Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="4e4c3-124">Adding Model Classes</span></span>

<span data-ttu-id="4e4c3-125">Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="4e4c3-126">Nom de la *classe* « Film ».</span><span class="sxs-lookup"><span data-stu-id="4e4c3-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="4e4c3-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="4e4c3-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="4e4c3-128">Ajoutez les propriétés suivantes de cinq à la `Movie` classe :</span><span class="sxs-lookup"><span data-stu-id="4e4c3-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="4e4c3-129">Nous allons utiliser le `Movie` classe pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="4e4c3-130">Chaque instance d’un `Movie` objet correspond à une ligne d’une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="4e4c3-131">Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :</span><span class="sxs-lookup"><span data-stu-id="4e4c3-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="4e4c3-132">Le `MovieDBContext` classe représente le contexte de base de données de film de Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` instances dans une base de données de la classe.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="4e4c3-133">Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="4e4c3-134">Pour plus d’informations sur `DbContext` et `DbSet`, consultez [des améliorations de productivité pour Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="4e4c3-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="4e4c3-135">Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter le code suivant `using` instruction en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="4e4c3-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="4e4c3-136">L’ensemble *Movie.cs* fichier est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="4e4c3-137">Création d’une chaîne de connexion et l’utilisation de SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4e4c3-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="4e4c3-138">Le `MovieDBContext` classe que vous avez créé gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="4e4c3-139">Une question que vous vous demandez peut-être, cependant, consiste à spécifier quelle base de données, il se connecte à.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="4e4c3-140">Vous le ferez en ajoutant des informations de connexion dans le *Web.config* fichier de l’application.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="4e4c3-141">Ouvrez la racine de l’application *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="4e4c3-142">(Pas le *Web.config* de fichiers dans le *vues* dossier.) L’image ci-dessous affiche les deux *Web.config* fichiers ; ouvrir le *Web.config* fichier entourée en rouge.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="4e4c3-143">Ajoutez la chaîne de connexion suivante à la `<connectionStrings>` élément dans le *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="4e4c3-144">L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="4e4c3-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="4e4c3-145">Cette petite quantité de code et le XML est tout ce que vous devez écrire pour représenter et de stocker les données de films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="4e4c3-146">Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.</span><span class="sxs-lookup"><span data-stu-id="4e4c3-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4e4c3-147">[Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="4e4c3-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
