---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Création d’une base de données | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581437"
---
# <a name="creating-a-database"></a><span data-ttu-id="099e8-104">Création d’une base de données</span><span class="sxs-lookup"><span data-stu-id="099e8-104">Creating a Database</span></span>

<span data-ttu-id="099e8-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="099e8-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="099e8-106">Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="099e8-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="099e8-107">Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="099e8-108">Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="099e8-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="099e8-109">Dans cette section, nous allons créer une nouvelle base de données SQL Express que nous allons utiliser pour stocker et récupérer les données de nos films.</span><span class="sxs-lookup"><span data-stu-id="099e8-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="099e8-110">Dans l’environnement de développement intégré (IDE) de Visual Web Developer, sélectionnez Afficher | Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="099e8-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="099e8-111">Cliquez avec le bouton droit sur connexions de données, puis cliquez sur Ajouter une connexion...</span><span class="sxs-lookup"><span data-stu-id="099e8-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="099e8-113">Dans la boîte de dialogue Choisir la source de données, sélectionnez Microsoft SQL Server, puis sélectionnez Continuer.</span><span class="sxs-lookup"><span data-stu-id="099e8-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="099e8-114">Dans la boîte de dialogue Ajouter une connexion, entrez « .\SQLEXPRESS » comme nom de serveur, puis entrez « films » comme nom de votre nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="099e8-115">[![boîte de dialogue Ajouter une connexion](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="099e8-116">Cliquez sur OK. il vous sera demandé si vous souhaitez créer cette base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="099e8-117">Sélectionnez Oui.</span><span class="sxs-lookup"><span data-stu-id="099e8-117">Select yes.</span></span>

<span data-ttu-id="099e8-118">[![créer des films ?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="099e8-119">Vous disposez maintenant d’une base de données vide dans Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="099e8-119">Now you've got an empty database in Server Explorer.</span></span>

![Ajouter une nouvelle table](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="099e8-121">Cliquez avec le bouton droit sur tables, puis cliquez sur Ajouter une table.</span><span class="sxs-lookup"><span data-stu-id="099e8-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="099e8-122">Le Concepteur de tables s’affiche.</span><span class="sxs-lookup"><span data-stu-id="099e8-122">The Table Designer will appear.</span></span> <span data-ttu-id="099e8-123">Ajoutez des colonnes pour ID, title, Released, genre et Price.</span><span class="sxs-lookup"><span data-stu-id="099e8-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="099e8-124">Cliquez avec le bouton droit sur la colonne ID, puis cliquez sur définir la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="099e8-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="099e8-125">Voici à quoi ressemblent mes zones de conception.</span><span class="sxs-lookup"><span data-stu-id="099e8-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="099e8-126">[Éditeur de table de base de données ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="099e8-127">En outre, sélectionnez la colonne ID et, sous propriétés de la colonne sous, modifiez « spécification d’identité » en « Oui ».</span><span class="sxs-lookup"><span data-stu-id="099e8-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="099e8-128">[![IsIdentity-propriétés de colonne](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="099e8-129">Une fois que vous avez terminé, cliquez sur l’icône Enregistrer dans la barre d’outils ou sélectionnez fichier | Enregistrez dans le menu et nommez votre table «**Movie**» (singulier).</span><span class="sxs-lookup"><span data-stu-id="099e8-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="099e8-130">Nous disposons d’une base de données et d’une table !</span><span class="sxs-lookup"><span data-stu-id="099e8-130">We've got a database and a table!</span></span>

<span data-ttu-id="099e8-131">[![choisir un nom](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="099e8-132">Revenez à Explorateur de serveurs et cliquez avec le bouton droit sur la table Movie, puis sélectionnez « Afficher les données de la table ».</span><span class="sxs-lookup"><span data-stu-id="099e8-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="099e8-133">Entrez quelques films pour que notre base de données comporte des données.</span><span class="sxs-lookup"><span data-stu-id="099e8-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="099e8-134">[Modification des tables de base de données ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="099e8-135">Création d’un modèle</span><span class="sxs-lookup"><span data-stu-id="099e8-135">Creating a Model</span></span>

<span data-ttu-id="099e8-136">À présent, revenez au Explorateur de solutions sur le côté droit de l’IDE et cliquez avec le bouton droit sur le dossier Models, puis sélectionnez Ajouter | Nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="099e8-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="099e8-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="099e8-138">Nous allons créer un modèle d’entité à partir de notre nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="099e8-139">Cela permet d’ajouter un ensemble de classes à notre projet qui facilite l’interrogation et la manipulation des données dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="099e8-140">Sélectionnez le nœud de données dans la partie gauche de la boîte de dialogue, puis sélectionnez le modèle d’élément ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="099e8-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="099e8-141">Nommez-le movies. edmx.</span><span class="sxs-lookup"><span data-stu-id="099e8-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="099e8-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="099e8-143">Cliquez sur le bouton « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="099e8-143">Click the "Add" button.</span></span> <span data-ttu-id="099e8-144">Cette opération lance ensuite l' « Assistant Entity Data Model ».</span><span class="sxs-lookup"><span data-stu-id="099e8-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="099e8-145">Dans la boîte de dialogue Nouveau qui s’affiche, sélectionnez générer à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="099e8-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="099e8-146">Étant donné que nous venons de créer une base de données, nous devons uniquement indiquer à la Entity Framework à propos de notre nouvelle base de données et de sa table.</span><span class="sxs-lookup"><span data-stu-id="099e8-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="099e8-147">Cliquez sur suivant pour enregistrer la connexion à la base de données dans la configuration de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="099e8-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="099e8-148">Maintenant, activez la case à cocher tables et films, puis cliquez sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="099e8-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="099e8-149">[Assistant Entity Data Model ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="099e8-150">Nous pouvons maintenant voir notre nouvelle table Movie dans le Entity Framework Designer et y accéder à partir du code.</span><span class="sxs-lookup"><span data-stu-id="099e8-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="099e8-151">[Films ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="099e8-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="099e8-152">Sur l’aire de conception, vous pouvez voir une classe « film ».</span><span class="sxs-lookup"><span data-stu-id="099e8-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="099e8-153">Cette classe est mappée à la table « Movie » de notre base de données, et chaque propriété qu’elle contient est mappée à une colonne avec la table.</span><span class="sxs-lookup"><span data-stu-id="099e8-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="099e8-154">Chaque instance d’une classe « Movie » correspond à une ligne dans la table « Movie ».</span><span class="sxs-lookup"><span data-stu-id="099e8-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="099e8-155">Si vous n’aimez pas les conventions de nommage et de mappage par défaut utilisées par le Entity Framework, vous pouvez utiliser le concepteur de Entity Framework pour les modifier ou les personnaliser.</span><span class="sxs-lookup"><span data-stu-id="099e8-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="099e8-156">Pour cette application, nous allons utiliser les valeurs par défaut et enregistrer simplement le fichier tel quel.</span><span class="sxs-lookup"><span data-stu-id="099e8-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="099e8-157">Nous allons maintenant travailler avec des données réelles !</span><span class="sxs-lookup"><span data-stu-id="099e8-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="099e8-158">[Précédent](getting-started-with-mvc-part3.md)
> [Suivant](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="099e8-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
