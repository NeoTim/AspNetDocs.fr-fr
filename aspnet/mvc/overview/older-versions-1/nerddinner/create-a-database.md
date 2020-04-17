---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: rick-anderson
description: L’étape 2 montre les étapes pour créer la base de données contenant toutes les données du dîner et de la RSVP pour notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: d0b87e4a6a27b37d2dbaa6d5b871da477b25586d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541609"
---
# <a name="create-a-database"></a><span data-ttu-id="a0c96-103">Créer une base de données</span><span class="sxs-lookup"><span data-stu-id="a0c96-103">Create a Database</span></span>

<span data-ttu-id="a0c96-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a0c96-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a0c96-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="a0c96-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a0c96-106">Il s’agit de l’étape 2 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="a0c96-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a0c96-107">L’étape 2 montre les étapes pour créer la base de données contenant toutes les données du dîner et de la RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="a0c96-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="a0c96-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="a0c96-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="a0c96-109">NerdDinner Étape 2: Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="a0c96-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="a0c96-110">Nous utiliserons une base de données pour stocker toutes les données Dîner et RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="a0c96-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="a0c96-111">Les étapes ci-dessous montrent la création de la base de données à l’aide de l’édition gratuite SQL Server Express (que vous pouvez facilement installer en utilisant V2 de [l’installateur de plate-forme Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a0c96-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="a0c96-112">Tout le code que nous écrirons fonctionne avec SQL Server Express et le serveur SQL complet.</span><span class="sxs-lookup"><span data-stu-id="a0c96-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="a0c96-113">Création d’une nouvelle base de données SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="a0c96-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="a0c96-114">Nous commencerons par cliquer à droite sur notre projet web, puis sélectionnerons la commande de menu **Add-&gt;New Item** :</span><span class="sxs-lookup"><span data-stu-id="a0c96-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="a0c96-115">Cela fera ressortir le dialogue "Add New Item" de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0c96-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="a0c96-116">Nous filtrerons par la catégorie «Données» et sélectionnerons le modèle d’élément « Base de données de serveurs SQL » :</span><span class="sxs-lookup"><span data-stu-id="a0c96-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="a0c96-117">Nous allons nommer la base de données SQL Server Express que nous voulons créer "NerdDinner.mdf" et frapper ok.</span><span class="sxs-lookup"><span data-stu-id="a0c96-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="a0c96-118">Visual Studio nous demandera alors si nous voulons\_ajouter ce fichier à notre répertoire de données d’application (qui est un répertoire déjà configuré avec des ACL de sécurité de lecture et d’écriture) :</span><span class="sxs-lookup"><span data-stu-id="a0c96-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="a0c96-119">Nous cliquerons sur "Oui" et notre nouvelle base de données sera créée et ajoutée à notre Solution Explorer:</span><span class="sxs-lookup"><span data-stu-id="a0c96-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="a0c96-120">Création de tables dans notre base de données</span><span class="sxs-lookup"><span data-stu-id="a0c96-120">Creating Tables within our Database</span></span>

<span data-ttu-id="a0c96-121">Nous avons maintenant une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="a0c96-121">We now have a new empty database.</span></span> <span data-ttu-id="a0c96-122">Ajoutons quelques tables.</span><span class="sxs-lookup"><span data-stu-id="a0c96-122">Let's add some tables to it.</span></span>

<span data-ttu-id="a0c96-123">Pour ce faire, nous naviguerons vers la fenêtre d’onglet "Server Explorer" dans Visual Studio, ce qui nous permet de gérer les bases de données et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="a0c96-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="a0c96-124">Les bases de données SQL\_Server Express stockées dans le dossier de données app de notre application s’afficheront automatiquement dans le Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="a0c96-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="a0c96-125">Nous pouvons utiliser l’icône "Connect to Database" en haut de la fenêtre "Server Explorer" pour ajouter des bases de données SQL Server supplémentaires (locales et éloignées) à la liste :</span><span class="sxs-lookup"><span data-stu-id="a0c96-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="a0c96-126">Nous ajouterons deux tables à notre base de données NerdDinner, l’une pour stocker nos dîners et l’autre pour suivre les acceptations de RSVP.</span><span class="sxs-lookup"><span data-stu-id="a0c96-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="a0c96-127">Nous pouvons créer de nouvelles tables en cliquant à droite sur le dossier "Tables" dans notre base de données et en choisissant la commande de menu "Ajouter une nouvelle table":</span><span class="sxs-lookup"><span data-stu-id="a0c96-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="a0c96-128">Cela ouvrira un concepteur de table qui nous permet de configurer le schéma de notre table.</span><span class="sxs-lookup"><span data-stu-id="a0c96-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="a0c96-129">Pour notre table "Dîners", nous ajouterons 10 colonnes de données:</span><span class="sxs-lookup"><span data-stu-id="a0c96-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="a0c96-130">Nous voulons que la colonne "DinnerID" soit une clé principale unique pour la table.</span><span class="sxs-lookup"><span data-stu-id="a0c96-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="a0c96-131">Nous pouvons configurer ceci en cliquant à droite sur la colonne « DinnerID » et en choisissant l’élément de menu « Clé primaire » :</span><span class="sxs-lookup"><span data-stu-id="a0c96-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="a0c96-132">En plus de faire de DinnerID une clé principale, nous voulons également la configurer comme une colonne « identitaire » dont la valeur est automatiquement incrémentée au fur et à mesure que de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première rangée insérée aura un DinnerID de 1, la deuxième rangée insérée aura un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="a0c96-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="a0c96-133">Nous pouvons le faire en sélectionnant la colonne "DinnerID" et ensuite utiliser l’éditeur "Column Properties" pour définir la propriété "(Est-identité)" sur la colonne à "Oui".</span><span class="sxs-lookup"><span data-stu-id="a0c96-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="a0c96-134">Nous utiliserons les défauts d’identité standard (à partir de 1 et incrément 1 sur chaque nouvelle rangée de dîner) :</span><span class="sxs-lookup"><span data-stu-id="a0c96-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="a0c96-135">Nous enregistrerons ensuite notre table en tapant Ctrl-S ou en utilisant la commande de menu **File-Enregistrer.&gt;**</span><span class="sxs-lookup"><span data-stu-id="a0c96-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="a0c96-136">Cela nous incitera à nommer la table.</span><span class="sxs-lookup"><span data-stu-id="a0c96-136">This will prompt us to name the table.</span></span> <span data-ttu-id="a0c96-137">Nous l’appellerons "Dîners":</span><span class="sxs-lookup"><span data-stu-id="a0c96-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="a0c96-138">Notre nouvelle table Dîners apparaîtra ensuite dans notre base de données dans l’explorateur serveur.</span><span class="sxs-lookup"><span data-stu-id="a0c96-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="a0c96-139">Nous allons ensuite répéter les étapes ci-dessus et créer une table "RSVP".</span><span class="sxs-lookup"><span data-stu-id="a0c96-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="a0c96-140">Ce tableau avec 3 colonnes.</span><span class="sxs-lookup"><span data-stu-id="a0c96-140">This table with have 3 columns.</span></span> <span data-ttu-id="a0c96-141">Nous allons configurer la colonne RsvpID comme la clé principale, et aussi en faire une colonne d’identité:</span><span class="sxs-lookup"><span data-stu-id="a0c96-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="a0c96-142">Nous allons le sauver et lui donner le nom "RSVP".</span><span class="sxs-lookup"><span data-stu-id="a0c96-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="a0c96-143">Mise en place d’une relation clé à l’étranger entre les tables</span><span class="sxs-lookup"><span data-stu-id="a0c96-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="a0c96-144">Nous avons maintenant deux tableaux dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="a0c96-144">We now have two tables within our database.</span></span> <span data-ttu-id="a0c96-145">Notre dernière étape de conception de schéma sera d’installer une relation « un à plusieurs » entre ces deux tables - afin que nous puissions associer chaque rangée de dîner avec zéro ou plus de lignes RSVP qui s’appliquent à elle.</span><span class="sxs-lookup"><span data-stu-id="a0c96-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="a0c96-146">Nous le ferons en configurant la colonne "DinnerID" de la table RSVP pour avoir une relation à l’étranger avec la colonne "DinnerID" dans la table "Dîners".</span><span class="sxs-lookup"><span data-stu-id="a0c96-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="a0c96-147">Pour ce faire, nous allons ouvrir la table RSVP dans le concepteur de table en le cliquant en double dans l’explorateur serveur.</span><span class="sxs-lookup"><span data-stu-id="a0c96-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="a0c96-148">Nous sélectionnerons ensuite la colonne "DinnerID" en son sein, en clic droit, et choisirons les "Relations..." commande de menu contexte :</span><span class="sxs-lookup"><span data-stu-id="a0c96-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="a0c96-149">Cela fera l’un des cas de dialogue que nous pouvons utiliser pour configurer les relations entre les tables :</span><span class="sxs-lookup"><span data-stu-id="a0c96-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="a0c96-150">Nous allons cliquer sur le bouton "Ajouter" pour ajouter une nouvelle relation au dialogue.</span><span class="sxs-lookup"><span data-stu-id="a0c96-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="a0c96-151">Une fois qu’une relation a été ajoutée, nous allons étendre le "Tables and Column Specification" noeud de vue d’arbre dans la grille de propriété à droite du dialogue, puis cliquez sur le "..." bouton à droite de celui-ci:</span><span class="sxs-lookup"><span data-stu-id="a0c96-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="a0c96-152">En cliquant sur le "..." bouton fera ressortir un autre dialogue qui nous permet de spécifier les tableaux et les colonnes sont impliqués dans la relation, ainsi que nous permettre de nommer la relation.</span><span class="sxs-lookup"><span data-stu-id="a0c96-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="a0c96-153">Nous modifierons la Table clé primaire pour être des « dîners » et sélectionnerons la colonne « DinnerID » dans la table des dîners comme clé principale.</span><span class="sxs-lookup"><span data-stu-id="a0c96-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="a0c96-154">Notre table RSVP sera la table clé à l’étranger, et le RSVP. La colonne DinnerID sera associée à la clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="a0c96-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="a0c96-155">Maintenant, chaque rangée dans la table RSVP sera associée à une rangée dans la table du dîner.</span><span class="sxs-lookup"><span data-stu-id="a0c96-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="a0c96-156">SQL Server maintiendra l’intégrité référentielle pour nous et nous empêchera d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une rangée de dîner valide.</span><span class="sxs-lookup"><span data-stu-id="a0c96-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="a0c96-157">Il nous empêchera également de supprimer une rangée de dîner s’il ya encore des rangées RSVP se référant à elle.</span><span class="sxs-lookup"><span data-stu-id="a0c96-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="a0c96-158">Ajout de données à nos tables</span><span class="sxs-lookup"><span data-stu-id="a0c96-158">Adding Data to our Tables</span></span>

<span data-ttu-id="a0c96-159">Finissons-en en ajoutant quelques exemples de données à notre table dîners.</span><span class="sxs-lookup"><span data-stu-id="a0c96-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="a0c96-160">Nous pouvons ajouter des données à une table en cliquant à droite sur elle dans le Server Explorer et en choisissant la commande "Show Table Data" :</span><span class="sxs-lookup"><span data-stu-id="a0c96-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="a0c96-161">Nous ajouterons quelques lignes de données Dîner que nous pourrons utiliser plus tard que nous commençons à implémenter l’application:</span><span class="sxs-lookup"><span data-stu-id="a0c96-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="a0c96-162">étape suivante</span><span class="sxs-lookup"><span data-stu-id="a0c96-162">Next Step</span></span>

<span data-ttu-id="a0c96-163">Nous avons fini de créer notre base de données.</span><span class="sxs-lookup"><span data-stu-id="a0c96-163">We've finished creating our database.</span></span> <span data-ttu-id="a0c96-164">Créons maintenant des classes modèles que nous pouvons utiliser pour l’interroger et le mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="a0c96-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0c96-165">[Suivant précédent](create-a-new-aspnet-mvc-project.md)
> [Next](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="a0c96-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
