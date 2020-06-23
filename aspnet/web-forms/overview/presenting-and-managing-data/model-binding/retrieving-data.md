---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Récupération et affichage de données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: d5f1982196c5985b001ca42c2711174e036bb1ec
ms.sourcegitcommit: 0cf7d06071a8ff986e6c028ac9daf0c0e7490412
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240749"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="5daa9-104">Récupération et affichage de données avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="5daa9-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="5daa9-105">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5daa9-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="5daa9-106">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="5daa9-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="5daa9-107">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="5daa9-108">Le modèle de liaison de modèle fonctionne avec n’importe quelle technologie d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="5daa9-109">Dans ce didacticiel, vous allez utiliser Entity Framework, mais vous pouvez utiliser la technologie d’accès aux données qui vous est la plus familière.</span><span class="sxs-lookup"><span data-stu-id="5daa9-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="5daa9-110">À partir d’un contrôle serveur lié aux données, tel qu’un contrôle GridView, ListView, DetailsView ou FormView, vous spécifiez les noms des méthodes à utiliser pour la sélection, la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="5daa9-111">Dans ce didacticiel, vous allez spécifier une valeur pour SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="5daa9-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="5daa9-112">Dans cette méthode, vous fournissez la logique de récupération des données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="5daa9-113">Dans le didacticiel suivant, vous allez définir des valeurs pour UpdateMethod, DeleteMethod et InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="5daa9-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="5daa9-114">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en C# ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="5daa9-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="5daa9-115">Le code téléchargeable fonctionne avec Visual Studio 2012 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="5daa9-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="5daa9-116">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2017 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5daa9-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="5daa9-117">Dans le didacticiel, vous exécutez l’application dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5daa9-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="5daa9-118">Vous pouvez également déployer l’application sur un fournisseur d’hébergement et la rendre disponible sur Internet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="5daa9-119">Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un</span><span class="sxs-lookup"><span data-stu-id="5daa9-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="5daa9-120">[compte d’essai gratuit d’Azure](https://azure.microsoft.com/free/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="5daa9-120">[free Azure trial account](https://azure.microsoft.com/free/dotnet/).</span></span> <span data-ttu-id="5daa9-121">Pour plus d’informations sur le déploiement d’un projet Visual Studio Web sur Azure App Service Web Apps, consultez la série [déploiement web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) .</span><span class="sxs-lookup"><span data-stu-id="5daa9-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="5daa9-122">Ce didacticiel montre également comment utiliser Migrations Entity Framework Code First pour déployer votre base de données SQL Server sur Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5daa9-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5daa9-123">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5daa9-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="5daa9-124">Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="5daa9-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="5daa9-125">Ce didacticiel fonctionne également avec Visual Studio 2012 et Visual Studio 2013, mais il existe des différences dans l’interface utilisateur et le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="5daa9-126">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="5daa9-126">What you'll build</span></span>

<span data-ttu-id="5daa9-127">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="5daa9-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="5daa9-128">Créer des objets de données reflétant une université avec les étudiants inscrits dans des cours</span><span class="sxs-lookup"><span data-stu-id="5daa9-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="5daa9-129">Créer des tables de base de données à partir des objets</span><span class="sxs-lookup"><span data-stu-id="5daa9-129">Build database tables from the objects</span></span>
* <span data-ttu-id="5daa9-130">Remplir la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="5daa9-130">Populate the database with test data</span></span>
* <span data-ttu-id="5daa9-131">Afficher des données dans un formulaire Web</span><span class="sxs-lookup"><span data-stu-id="5daa9-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="5daa9-132">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="5daa9-132">Create the project</span></span>

1. <span data-ttu-id="5daa9-133">Dans Visual Studio 2017, créez un projet d' **application Web ASP.net (.NET Framework)** nommé **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![créer un projet](retrieving-data/_static/image19.png)

2. <span data-ttu-id="5daa9-135">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-135">Select **OK**.</span></span> <span data-ttu-id="5daa9-136">La boîte de dialogue permettant de sélectionner un modèle s’affiche.</span><span class="sxs-lookup"><span data-stu-id="5daa9-136">The dialog box to select a template appears.</span></span>

   ![sélectionner Web Forms](retrieving-data/_static/image3.png)

3. <span data-ttu-id="5daa9-138">Sélectionnez le modèle **Web Forms** .</span><span class="sxs-lookup"><span data-stu-id="5daa9-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="5daa9-139">Si nécessaire, modifiez l’authentification en **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="5daa9-140">Sélectionnez **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="5daa9-141">Modifier l’apparence du site</span><span class="sxs-lookup"><span data-stu-id="5daa9-141">Modify site appearance</span></span>

   <span data-ttu-id="5daa9-142">Apportez quelques modifications pour personnaliser l’apparence du site.</span><span class="sxs-lookup"><span data-stu-id="5daa9-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="5daa9-143">Ouvrez le fichier site. Master.</span><span class="sxs-lookup"><span data-stu-id="5daa9-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="5daa9-144">Modifiez le titre pour afficher **Contoso University** et non **mon application ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="5daa9-145">Remplacez le texte d’en-tête du nom de l' **application** par **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="5daa9-146">Modifiez les liens de l’en-tête de navigation vers le site approprié.</span><span class="sxs-lookup"><span data-stu-id="5daa9-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="5daa9-147">Supprimez les liens relatifs à à **propos** de et **Contactez** et, à la place, créez un lien vers une page **étudiants** , que vous allez créer.</span><span class="sxs-lookup"><span data-stu-id="5daa9-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="5daa9-148">Enregistrez site. Master.</span><span class="sxs-lookup"><span data-stu-id="5daa9-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="5daa9-149">Ajouter un formulaire Web pour afficher les données des étudiants</span><span class="sxs-lookup"><span data-stu-id="5daa9-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="5daa9-150">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, sélectionnez **Ajouter** , puis **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="5daa9-151">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le **formulaire Web avec le modèle de page maître** et nommez-le Student **. aspx**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![créer une page](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="5daa9-153">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="5daa9-154">Pour la page maître du formulaire Web, sélectionnez **site. Master**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="5daa9-155">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="5daa9-156">Ajouter le modèle de données</span><span class="sxs-lookup"><span data-stu-id="5daa9-156">Add the data model</span></span>

<span data-ttu-id="5daa9-157">Dans le dossier **Models** , ajoutez une classe nommée **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="5daa9-158">Cliquez avec le bouton droit sur **modèles**, sélectionnez **Ajouter**, puis **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="5daa9-159">La boîte de dialogue **Ajouter un nouvel élément** s'affiche.</span><span class="sxs-lookup"><span data-stu-id="5daa9-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="5daa9-160">Dans le menu de navigation gauche, sélectionnez **code**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![créer une classe de modèle](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="5daa9-162">Nommez la classe **UniversityModels.cs** , puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="5daa9-163">Dans ce fichier, définissez les `SchoolContext` classes,, `Student` `Enrollment` et `Course` comme suit :</span><span class="sxs-lookup"><span data-stu-id="5daa9-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="5daa9-164">La `SchoolContext` classe dérive de `DbContext` , qui gère la connexion à la base de données et les modifications apportées aux données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="5daa9-165">Dans la `Student` classe, notez les attributs appliqués aux `FirstName` Propriétés, `LastName` et `Year` .</span><span class="sxs-lookup"><span data-stu-id="5daa9-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="5daa9-166">Ce didacticiel utilise ces attributs pour la validation des données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="5daa9-167">Pour simplifier le code, seules ces propriétés sont marquées avec des attributs de validation de données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="5daa9-168">Dans un projet réel, vous devez appliquer des attributs de validation à toutes les propriétés nécessitant une validation.</span><span class="sxs-lookup"><span data-stu-id="5daa9-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="5daa9-169">Enregistrez UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="5daa9-170">Configurer la base de données en fonction des classes</span><span class="sxs-lookup"><span data-stu-id="5daa9-170">Set up the database based on classes</span></span>

<span data-ttu-id="5daa9-171">Ce didacticiel utilise [migrations code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) pour créer des objets et des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="5daa9-172">Ces tables stockent des informations sur les étudiants et leurs cours.</span><span class="sxs-lookup"><span data-stu-id="5daa9-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="5daa9-173">Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="5daa9-174">Dans la **console du gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5daa9-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="5daa9-175">Si la commande se termine correctement, un message indiquant que la migration a été activée s’affiche.</span><span class="sxs-lookup"><span data-stu-id="5daa9-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![activer les migrations](retrieving-data/_static/image8.png)

      <span data-ttu-id="5daa9-177">Notez qu’un fichier nommé *Configuration.cs* a été créé.</span><span class="sxs-lookup"><span data-stu-id="5daa9-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="5daa9-178">La `Configuration` classe a une `Seed` méthode qui peut préremplir les tables de base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="5daa9-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="5daa9-179">Pré-remplir la base de données</span><span class="sxs-lookup"><span data-stu-id="5daa9-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="5daa9-180">Ouvrez Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="5daa9-181">Ajoutez le code suivant à la méthode `Seed` .</span><span class="sxs-lookup"><span data-stu-id="5daa9-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="5daa9-182">Ajoutez également une `using` instruction pour l' `ContosoUniversityModelBinding. Models` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="5daa9-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="5daa9-183">Enregistrez Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="5daa9-184">Dans la console du gestionnaire de package, exécutez la commande **Add-migration initial**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="5daa9-185">Exécutez la commande **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="5daa9-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="5daa9-186">Si vous recevez une exception lors de l’exécution de cette commande, les `StudentID` `CourseID` valeurs et peuvent être différentes des `Seed` valeurs de méthode.</span><span class="sxs-lookup"><span data-stu-id="5daa9-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="5daa9-187">Ouvrez ces tables de base de données et recherchez les valeurs existantes pour `StudentID` et `CourseID` .</span><span class="sxs-lookup"><span data-stu-id="5daa9-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="5daa9-188">Ajoutez ces valeurs au code pour amorcer la `Enrollments` table.</span><span class="sxs-lookup"><span data-stu-id="5daa9-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="5daa9-189">Ajouter un contrôle GridView</span><span class="sxs-lookup"><span data-stu-id="5daa9-189">Add a GridView control</span></span>

<span data-ttu-id="5daa9-190">Avec les données de base de données remplies, vous êtes maintenant prêt à récupérer ces données et à les afficher.</span><span class="sxs-lookup"><span data-stu-id="5daa9-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="5daa9-191">Ouvrez students. aspx.</span><span class="sxs-lookup"><span data-stu-id="5daa9-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="5daa9-192">Recherchez l' `MainContent` espace réservé.</span><span class="sxs-lookup"><span data-stu-id="5daa9-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="5daa9-193">Dans cet espace réservé, ajoutez un contrôle **GridView** qui comprend ce code.</span><span class="sxs-lookup"><span data-stu-id="5daa9-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="5daa9-194">Points à noter :</span><span class="sxs-lookup"><span data-stu-id="5daa9-194">Things to note:</span></span>
   * <span data-ttu-id="5daa9-195">Notez la valeur définie pour la `SelectMethod` propriété dans l’élément GridView.</span><span class="sxs-lookup"><span data-stu-id="5daa9-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="5daa9-196">Cette valeur spécifie la méthode utilisée pour récupérer les données GridView, que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="5daa9-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="5daa9-197">La `ItemType` propriété est définie sur la `Student` classe créée précédemment.</span><span class="sxs-lookup"><span data-stu-id="5daa9-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="5daa9-198">Ce paramètre vous permet de référencer des propriétés de classe dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="5daa9-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="5daa9-199">Par exemple, la `Student` classe a une collection nommée `Enrollments` .</span><span class="sxs-lookup"><span data-stu-id="5daa9-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="5daa9-200">Vous pouvez utiliser `Item.Enrollments` pour récupérer cette collection, puis utiliser la [syntaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) pour récupérer la somme des crédits inscrits de chaque étudiant.</span><span class="sxs-lookup"><span data-stu-id="5daa9-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="5daa9-201">Enregistrez students. aspx.</span><span class="sxs-lookup"><span data-stu-id="5daa9-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="5daa9-202">Ajouter du code pour récupérer des données</span><span class="sxs-lookup"><span data-stu-id="5daa9-202">Add code to retrieve data</span></span>

   <span data-ttu-id="5daa9-203">Dans le fichier code-behind studentss. aspx, ajoutez la méthode spécifiée pour la `SelectMethod` valeur.</span><span class="sxs-lookup"><span data-stu-id="5daa9-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="5daa9-204">Ouvrez Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="5daa9-205">Ajoutez `using` des instructions pour `ContosoUniversityModelBinding. Models` les `System.Data.Entity` espaces de noms et.</span><span class="sxs-lookup"><span data-stu-id="5daa9-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="5daa9-206">Ajoutez la méthode que vous avez spécifiée pour `SelectMethod` :</span><span class="sxs-lookup"><span data-stu-id="5daa9-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="5daa9-207">La `Include` clause améliore les performances des requêtes, mais n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="5daa9-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="5daa9-208">Sans la `Include` clause, les données sont récupérées à l’aide [*du chargement différé*](https://en.wikipedia.org/wiki/Lazy_loading), ce qui implique l’envoi d’une requête distincte à la base de données chaque fois que des données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="5daa9-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="5daa9-209">Avec la `Include` clause, les données sont récupérées à l’aide *du chargement hâtif*, ce qui signifie qu’une requête de base de données unique récupère toutes les données associées.</span><span class="sxs-lookup"><span data-stu-id="5daa9-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="5daa9-210">Si les données associées ne sont pas utilisées, le chargement hâtif est moins efficace, car davantage de données sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="5daa9-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="5daa9-211">Toutefois, dans ce cas, le chargement hâtif offre des performances optimales, car les données associées sont affichées pour chaque enregistrement.</span><span class="sxs-lookup"><span data-stu-id="5daa9-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="5daa9-212">Pour plus d’informations sur les considérations relatives aux performances lors du chargement de données associées, consultez la section **chargement différé, hâtif et explicite de données connexes** dans l’article [lecture des données associées avec l’Entity Framework dans une application MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="5daa9-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="5daa9-213">Par défaut, les données sont triées en fonction des valeurs de la propriété marquée comme clé.</span><span class="sxs-lookup"><span data-stu-id="5daa9-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="5daa9-214">Vous pouvez ajouter une `OrderBy` clause pour spécifier une autre valeur de tri.</span><span class="sxs-lookup"><span data-stu-id="5daa9-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="5daa9-215">Dans cet exemple, la propriété par défaut `StudentID` est utilisée pour le tri.</span><span class="sxs-lookup"><span data-stu-id="5daa9-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="5daa9-216">Dans l’article [Trier, paginer et filtrer les données](sorting-paging-and-filtering-data.md) , l’utilisateur est autorisé à sélectionner une colonne pour le tri.</span><span class="sxs-lookup"><span data-stu-id="5daa9-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="5daa9-217">Enregistrez Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="5daa9-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="5daa9-218">Exécuter votre application</span><span class="sxs-lookup"><span data-stu-id="5daa9-218">Run your application</span></span> 

<span data-ttu-id="5daa9-219">Exécutez votre application Web (**F5**) et accédez à la page des **étudiants** , qui affiche les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="5daa9-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![afficher les données](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="5daa9-221">Génération automatique de méthodes de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="5daa9-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="5daa9-222">Lorsque vous utilisez cette série de didacticiels, vous pouvez simplement copier le code du didacticiel vers votre projet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="5daa9-223">Toutefois, l’un des inconvénients de cette approche est que vous ne connaissez peut-être pas la fonctionnalité fournie par Visual Studio pour générer automatiquement du code pour les méthodes de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="5daa9-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="5daa9-224">Lorsque vous travaillez sur vos propres projets, la génération automatique de code peut vous faire gagner du temps et vous aider à mieux comprendre comment implémenter une opération.</span><span class="sxs-lookup"><span data-stu-id="5daa9-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="5daa9-225">Cette section décrit la fonctionnalité de génération de code automatique.</span><span class="sxs-lookup"><span data-stu-id="5daa9-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="5daa9-226">Cette section est uniquement à titre d’information et ne contient pas de code que vous devez implémenter dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="5daa9-227">Lorsque vous définissez une valeur pour `SelectMethod` les `UpdateMethod` Propriétés,, `InsertMethod` ou `DeleteMethod` dans le code de balisage, vous pouvez sélectionner l’option **créer une nouvelle méthode** .</span><span class="sxs-lookup"><span data-stu-id="5daa9-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![créer une méthode](retrieving-data/_static/image18.png)

<span data-ttu-id="5daa9-229">Visual Studio crée non seulement une méthode dans le code-behind avec la signature appropriée, mais génère également le code d’implémentation pour effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="5daa9-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="5daa9-230">Si vous définissez d’abord la `ItemType` propriété avant d’utiliser la fonctionnalité de génération de code automatique, le code généré utilise ce type pour les opérations.</span><span class="sxs-lookup"><span data-stu-id="5daa9-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="5daa9-231">Par exemple, lors de la définition de la `UpdateMethod` propriété, le code suivant est généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="5daa9-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="5daa9-232">Là encore, ce code n’a pas besoin d’être ajouté à votre projet.</span><span class="sxs-lookup"><span data-stu-id="5daa9-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="5daa9-233">Dans le didacticiel suivant, vous allez implémenter des méthodes de mise à jour, de suppression et d’ajout de nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="5daa9-234">Résumé</span><span class="sxs-lookup"><span data-stu-id="5daa9-234">Summary</span></span>

<span data-ttu-id="5daa9-235">Dans ce didacticiel, vous avez créé des classes de modèle de données et généré une base de données à partir de ces classes.</span><span class="sxs-lookup"><span data-stu-id="5daa9-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="5daa9-236">Vous avez rempli les tables de base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="5daa9-236">You filled the database tables with test data.</span></span> <span data-ttu-id="5daa9-237">Vous avez utilisé la liaison de modèle pour récupérer des données de la base de données, puis affiché les données dans un GridView.</span><span class="sxs-lookup"><span data-stu-id="5daa9-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="5daa9-238">Dans le [didacticiel](updating-deleting-and-creating-data.md) suivant de cette série, vous allez activer la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="5daa9-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5daa9-239">Suivant</span><span class="sxs-lookup"><span data-stu-id="5daa9-239">Next</span></span>](updating-deleting-and-creating-data.md)
