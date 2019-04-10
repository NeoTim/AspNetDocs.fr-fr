---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Applications auxiliaires ASP.NET MVC 4, formulaires et la Validation | Microsoft Docs
author: rick-anderson
description: Dans ASP.NET MVC 4 modèles et pratiques de données Access, vous avez été le chargement et affichage des données à partir de la base de données. Dans cet atelier pratique, vous allez ajouter à la...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 639a8e0e5fd9557221c95aee1bef0294df047ae8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59406312"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="09306-104">Helpers, formulaires et validation d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="09306-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="09306-105">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="09306-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="09306-106">Télécharger le Kit de formation de Web Camps</span><span class="sxs-lookup"><span data-stu-id="09306-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="09306-107">Dans **accès aux données et les modèles ASP.NET MVC 4** les ateliers pratiques, vous avez été le chargement et l’affichage des données à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="09306-108">Dans cet atelier pratique, vous allez ajouter à la **Music Store** application la possibilité de modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="09306-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="09306-109">Dans ce but à l’esprit, vous allez d’abord créer le contrôleur qui prendra en charge les actions de création, lecture, mise à jour et suppression (CRUD) des albums.</span><span class="sxs-lookup"><span data-stu-id="09306-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="09306-110">Vous allez générer un modèle de vue de l’Index en tirant parti de la fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés des albums dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="09306-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="09306-111">Pour des raisons de cette vue, vous allez ajouter un programme d’assistance HTML personnalisée qui va tronquer les longues descriptions.</span><span class="sxs-lookup"><span data-stu-id="09306-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="09306-112">Vous ajouterez par la suite, le modifier et créer des vues qui vous permettra d’altèrent albums dans la base de données, à l’aide des éléments de formulaire tels que des listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="09306-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="09306-113">Enfin, vous permettra aux utilisateurs de supprimer un album et vous serez également les empêcher d’entrer des données erronées en validant leur entrée.</span><span class="sxs-lookup"><span data-stu-id="09306-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="09306-114">Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="09306-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="09306-115">Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de consulter **principes de base ASP.NET MVC** les ateliers pratiques.</span><span class="sxs-lookup"><span data-stu-id="09306-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="09306-116">Ce laboratoire vous guide les améliorations et les nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web dans le dossier Source.</span><span class="sxs-lookup"><span data-stu-id="09306-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="09306-117">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="09306-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="09306-118">Le projet spécifique à ce laboratoire est disponible à l’adresse [ASP.NET MVC 4 Helpers, formulaires et la Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="09306-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="09306-119">Objectifs</span><span class="sxs-lookup"><span data-stu-id="09306-119">Objectives</span></span>

<span data-ttu-id="09306-120">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="09306-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="09306-121">Créer un contrôleur pour prendre en charge les opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="09306-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="09306-122">Générer une vue d’Index pour afficher les propriétés de l’entité dans un tableau HTML</span><span class="sxs-lookup"><span data-stu-id="09306-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="09306-123">Ajouter un programme d’assistance HTML personnalisée</span><span class="sxs-lookup"><span data-stu-id="09306-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="09306-124">Créer et personnaliser une vue Modifier</span><span class="sxs-lookup"><span data-stu-id="09306-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="09306-125">Faire la distinction entre les méthodes d’action qui réagissent aux HTTP-GET ou d’appels HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="09306-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="09306-126">Ajouter et personnaliser une instruction Create View</span><span class="sxs-lookup"><span data-stu-id="09306-126">Add and customize a Create View</span></span>
- <span data-ttu-id="09306-127">Handle de la suppression d’une entité</span><span class="sxs-lookup"><span data-stu-id="09306-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="09306-128">Valider l’entrée utilisateur</span><span class="sxs-lookup"><span data-stu-id="09306-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="09306-129">Prérequis</span><span class="sxs-lookup"><span data-stu-id="09306-129">Prerequisites</span></span>

<span data-ttu-id="09306-130">Vous devez disposer des éléments suivants pour terminer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="09306-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="09306-131">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).</span><span class="sxs-lookup"><span data-stu-id="09306-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="09306-132">Installation</span><span class="sxs-lookup"><span data-stu-id="09306-132">Setup</span></span>

**<span data-ttu-id="09306-133">L’installation des extraits de Code</span><span class="sxs-lookup"><span data-stu-id="09306-133">Installing Code Snippets</span></span>**

<span data-ttu-id="09306-134">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09306-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="09306-135">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="09306-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="09306-136">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [annexe b : À l’aide d’extraits de Code](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="09306-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="09306-137">Exercices</span><span class="sxs-lookup"><span data-stu-id="09306-137">Exercises</span></span>

<span data-ttu-id="09306-138">Les exercices suivants composent ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="09306-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="09306-139">Création du contrôleur de Store Manager et sa vue Index</span><span class="sxs-lookup"><span data-stu-id="09306-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="09306-140">Ajout d’une application d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="09306-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="09306-141">Création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="09306-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="09306-142">Ajout d’une vue de créer</span><span class="sxs-lookup"><span data-stu-id="09306-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="09306-143">Suppression de la gestion</span><span class="sxs-lookup"><span data-stu-id="09306-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="09306-144">Ajout de la validation</span><span class="sxs-lookup"><span data-stu-id="09306-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="09306-145">À l’aide de jQuery non obstructive côté Client</span><span class="sxs-lookup"><span data-stu-id="09306-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="09306-146">Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="09306-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="09306-147">Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="09306-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="09306-148">Durée estimée pour effectuer ce laboratoire : **60 minutes**</span><span class="sxs-lookup"><span data-stu-id="09306-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="09306-149">Exercice 1 : Création du contrôleur de Store Manager et sa vue Index</span><span class="sxs-lookup"><span data-stu-id="09306-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="09306-150">Dans cet exercice, vous allez apprendre à créer un nouveau contrôleur de prendre en charge les opérations CRUD, personnaliser sa méthode d’action Index pour retourner une liste d’albums à partir de la base de données et enfin générer un modèle de vue de l’Index en tirant parti de la structure de ASP.NET MVC fonction pour afficher les propriétés des albums dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="09306-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="09306-151">Tâche 1 : création de la StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="09306-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="09306-152">Dans cette tâche, vous allez créer un nouveau contrôleur appelé **StoreManagerController** pour prendre en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="09306-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="09306-153">Ouvrez le **commencer** solution situé dans **/Ex1-CreatingTheStoreManagerController/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="09306-154">Vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-155">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-156">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-157">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-158">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-159">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-160">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-161">Ajoutez un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="09306-161">Add a new controller.</span></span> <span data-ttu-id="09306-162">Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande.</span><span class="sxs-lookup"><span data-stu-id="09306-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="09306-163">Modifier le **contrôleur** **nom** à **StoreManagerController** et assurez-vous que l’option **contrôleur MVC avec des actions de lecture/écriture vides**est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="09306-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="09306-164">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="09306-164">Click **Add**.</span></span>

    <span data-ttu-id="09306-165">![Boîte de dialogue Ajouter contrôleur](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "boîte de dialogue Ajouter contrôleur")</span><span class="sxs-lookup"><span data-stu-id="09306-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    *<span data-ttu-id="09306-166">Ajouter la boîte de dialogue contrôleur</span><span class="sxs-lookup"><span data-stu-id="09306-166">Add Controller Dialog</span></span>*

    <span data-ttu-id="09306-167">Une nouvelle classe de contrôleur est générée.</span><span class="sxs-lookup"><span data-stu-id="09306-167">A new Controller class is generated.</span></span> <span data-ttu-id="09306-168">Étant donné que vous avez indiqué pour ajouter des actions de lecture/écriture, les méthodes stub pour ceux, des actions CRUD courantes sont créées avec des commentaires TODO renseignés, vous invitant à inclure la logique d’application spécifique.</span><span class="sxs-lookup"><span data-stu-id="09306-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="09306-169">Tâche 2 - Personnalisation de l’Index StoreManager</span><span class="sxs-lookup"><span data-stu-id="09306-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="09306-170">Dans cette tâche, vous allez personnaliser la méthode d’action StoreManager Index pour retourner une vue avec la liste des albums à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="09306-171">Dans la classe StoreManagerController, ajoutez le code suivant *à l’aide de* directives.</span><span class="sxs-lookup"><span data-stu-id="09306-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="09306-172">(Code Snippet - *formulaires et Validation - Ex1 de programmes d’assistance ASP.NET MVC 4 et à l’aide de MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="09306-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="09306-173">Ajoutez un champ à la **StoreManagerController** devant contenir une instance de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="09306-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="09306-174">(Code Snippet - *applications auxiliaires ASP.NET MVC 4 et de formulaires et de Validation - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="09306-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="09306-175">Implémenter l’action de l’Index de StoreManagerController pour retourner une vue avec la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="09306-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="09306-176">La logique d’action de contrôleur sera très similaire à l’action d’Index de la StoreController écrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="09306-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="09306-177">Utiliser LINQ pour récupérer tous les albums, y compris les informations de Genre et artiste pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="09306-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="09306-178">(Code Snippet - *applications auxiliaires ASP.NET MVC 4 et de formulaires et de Validation - Ex1 StoreManagerController Index*)</span><span class="sxs-lookup"><span data-stu-id="09306-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="09306-179">Tâche 3 : création de la vue Index</span><span class="sxs-lookup"><span data-stu-id="09306-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="09306-180">Dans cette tâche, vous allez créer le modèle de vue de l’Index pour afficher la liste des albums retourné par la **StoreManager** contrôleur.</span><span class="sxs-lookup"><span data-stu-id="09306-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="09306-181">Avant de créer le nouveau modèle de vue, vous devez créer le projet afin que le **boîte de dialogue Vue ajouter** connaît le **Album** classe à utiliser.</span><span class="sxs-lookup"><span data-stu-id="09306-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="09306-182">Sélectionnez **générer | Build MvcMusicStore** pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="09306-183">Avec le bouton droit à l’intérieur de la **Index** méthode d’action, puis **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="09306-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="09306-184">Cela fera apparaître le **ajouter une vue** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="09306-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="09306-185">![Ajouter une vue](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ajouter une vue")</span><span class="sxs-lookup"><span data-stu-id="09306-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    *<span data-ttu-id="09306-186">Ajout d’une vue à partir de la méthode Index</span><span class="sxs-lookup"><span data-stu-id="09306-186">Adding a View from within the Index method</span></span>*
3. <span data-ttu-id="09306-187">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **Index**.</span><span class="sxs-lookup"><span data-stu-id="09306-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="09306-188">Sélectionnez le **créer une vue fortement typée** option, puis sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="09306-189">Sélectionnez **liste** à partir de la **modèle de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="09306-190">Laissez le **moteur d’affichage** à **Razor** et les autres champs leur valeur par défaut de valeur, puis cliquez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="09306-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="09306-191">![Ajout d’une vue index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Ajout d’une vue d’index")</span><span class="sxs-lookup"><span data-stu-id="09306-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    *<span data-ttu-id="09306-192">Ajout d’une vue d’Index</span><span class="sxs-lookup"><span data-stu-id="09306-192">Adding an Index View</span></span>*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="09306-193">Tâche 4 : personnalisation de la structure de la vue Index</span><span class="sxs-lookup"><span data-stu-id="09306-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="09306-194">Dans cette tâche, vous allez ajuster le modèle de vue simple créé avec la fonctionnalité de génération de modèles automatique ASP.NET MVC pour qu’il affiche les champs souhaités.</span><span class="sxs-lookup"><span data-stu-id="09306-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="09306-195">Le **la structure** prise en charge dans ASP.NET MVC génère un simple modèle de vue qui répertorie tous les champs dans le modèle de l’Album.</span><span class="sxs-lookup"><span data-stu-id="09306-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="09306-196">**Génération de modèles automatique** offre un moyen rapide pour commencer sur une vue fortement typée : plutôt que d’écrire le modèle de vue manuellement, la génération de modèles automatique rapidement génère un modèle par défaut et vous pouvez ensuite modifier le code généré.</span><span class="sxs-lookup"><span data-stu-id="09306-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="09306-197">Passez en revue le code créé.</span><span class="sxs-lookup"><span data-stu-id="09306-197">Review the code created.</span></span> <span data-ttu-id="09306-198">La liste de champs générée fera partie des éléments suivants de table HTML qui **la structure** utilise pour afficher les données tabulaires.</span><span class="sxs-lookup"><span data-stu-id="09306-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="09306-199">Remplacez le **&lt;table&gt;** code avec le code suivant pour afficher uniquement les **Genre**, **artiste**, **titre d’Album**, et **prix** champs.</span><span class="sxs-lookup"><span data-stu-id="09306-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="09306-200">Cette opération supprime le **AlbumId** et **Album Art URL** colonnes.</span><span class="sxs-lookup"><span data-stu-id="09306-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="09306-201">En outre, il modifie les colonnes GenreId et ArtistId pour afficher leurs propriétés de classe lié de **Artist.Name** et **Genre.Name**et supprime la **détails** lien.</span><span class="sxs-lookup"><span data-stu-id="09306-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="09306-202">Modifier les descriptions suivantes.</span><span class="sxs-lookup"><span data-stu-id="09306-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="09306-203">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="09306-204">Dans cette tâche, vous allez tester que le **StoreManager** **Index** modèle de vue affiche une liste d’albums en fonction de la conception des étapes précédentes.</span><span class="sxs-lookup"><span data-stu-id="09306-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="09306-205">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-206">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-206">The project starts in the Home page.</span></span> <span data-ttu-id="09306-207">Remplacez l’URL par **/StoreManager** pour vérifier que la liste des albums est affichée, indiquant leur **titre**, **artiste** et **Genre**.</span><span class="sxs-lookup"><span data-stu-id="09306-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="09306-208">![Parcourir la liste des albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "parcourir la liste des albums")</span><span class="sxs-lookup"><span data-stu-id="09306-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    *<span data-ttu-id="09306-209">Parcourir la liste des albums</span><span class="sxs-lookup"><span data-stu-id="09306-209">Browsing the list of albums</span></span>*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="09306-210">Exercice 2 : Ajout d’une application d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="09306-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="09306-211">La page d’Index de StoreManager a un problème potentiel : Propriétés du titre et un nom d’artiste peuvent tous deux être suffisamment longue pour décaler la mise en forme de table.</span><span class="sxs-lookup"><span data-stu-id="09306-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="09306-212">Dans cet exercice, vous allez apprendre à ajouter un programme d’assistance HTML personnalisée pour tronquer ce texte.</span><span class="sxs-lookup"><span data-stu-id="09306-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="09306-213">Dans l’illustration suivante, vous pouvez voir la manière dont le format est modifié en raison de la longueur du texte lorsque vous utilisez une taille petite navigateur.</span><span class="sxs-lookup"><span data-stu-id="09306-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="09306-214">![Parcourir la liste des Albums avec pas tronqué texte](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "parcourir la liste des Albums avec texte pas est tronqué")</span><span class="sxs-lookup"><span data-stu-id="09306-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

*<span data-ttu-id="09306-215">Parcourir la liste des Albums avec texte pas est tronqué</span><span class="sxs-lookup"><span data-stu-id="09306-215">Browsing the list of Albums with not truncated text</span></span>*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="09306-216">Tâche 1 - extension du programme d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="09306-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="09306-217">Dans cette tâche, vous allez ajouter une nouvelle méthode **Truncate** à la **HTML** objet exposé dans les vues ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09306-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="09306-218">Pour ce faire, vous allez implémenter un **méthode d’extension** à intégrés **System.Web.Mvc.HtmlHelper** classe fournie par ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09306-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="09306-219">Pour en savoir plus sur **méthodes d’Extension**, consultez cet article de msdn.</span><span class="sxs-lookup"><span data-stu-id="09306-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="09306-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="09306-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="09306-221">Ouvrez le **commencer** solution situé dans **/Ex2-AddingAnHTMLHelper/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="09306-222">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-223">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-224">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-225">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-226">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-227">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-228">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-229">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-230">Ouvrez la vue de l’Index de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="09306-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="09306-231">Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="09306-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="09306-232">Ajoutez le code suivant ci-dessous le <strong>@model</strong> directive pour définir le <strong>Truncate</strong> méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="09306-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="09306-233">Tâche 2 - troncation de texte dans la Page</span><span class="sxs-lookup"><span data-stu-id="09306-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="09306-234">Dans cette tâche, vous allez utiliser le **Truncate** méthode pour tronquer le texte dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="09306-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="09306-235">Ouvrez la vue de l’Index de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="09306-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="09306-236">Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="09306-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="09306-237">Remplacez les lignes qui indiquent la **un nom d’artiste** et d’Album **titre**.</span><span class="sxs-lookup"><span data-stu-id="09306-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="09306-238">Pour ce faire, remplacez les lignes suivantes.</span><span class="sxs-lookup"><span data-stu-id="09306-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="09306-239">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="09306-240">Dans cette tâche, vous allez tester que le **StoreManager** **Index** afficher le modèle tronque le titre et le nom de l’artiste l’Album.</span><span class="sxs-lookup"><span data-stu-id="09306-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="09306-241">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-242">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-242">The project starts in the Home page.</span></span> <span data-ttu-id="09306-243">Remplacez l’URL par **/StoreManager** pour vérifier ce long textes en le **titre** et **artiste** colonne sont tronqués.</span><span class="sxs-lookup"><span data-stu-id="09306-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="09306-244">![Tronqué les noms des titres et des artistes](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "tronqué les noms des titres et des artistes")</span><span class="sxs-lookup"><span data-stu-id="09306-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    *<span data-ttu-id="09306-245">Titres tronquées et des artistes</span><span class="sxs-lookup"><span data-stu-id="09306-245">Truncated Titles and Artist Names</span></span>*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="09306-246">Exercice 3 : Création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="09306-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="09306-247">Dans cet exercice, vous allez apprendre à créer un formulaire pour permettre aux responsables de magasin modifier un Album.</span><span class="sxs-lookup"><span data-stu-id="09306-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="09306-248">Ils seront parcourent le **/StoreManager/Edit/id** URL (**id** en cours de l’id unique de l’album à modifier), ce qui rend un appel HTTP GET au serveur.</span><span class="sxs-lookup"><span data-stu-id="09306-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="09306-249">La méthode d’action contrôleur modifier se récupérer l’Album approprié à partir de la base de données, créez un **StoreManagerViewModel** objet à encapsuler (avec une liste d’artistes et Genres) et à transmettre à un modèle de vue afficher la page HTML à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09306-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="09306-250">Cette page contient un **&lt;formulaire&gt;** élément avec les zones de texte et les listes déroulantes pour modifier les propriétés de l’Album.</span><span class="sxs-lookup"><span data-stu-id="09306-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="09306-251">Une fois que l’utilisateur met à jour les valeurs de formulaire d’Album et clique sur le **enregistrer** bouton, les modifications sont soumises une requête HTTP-POST rappeler **/StoreManager/Edit/id**. Bien que l’URL reste le même que dans le dernier appel, ASP.NET MVC qui identifie cette fois, il est une requête HTTP-POST et par conséquent s’exécute une autre méthode d’action de modification (une décorée avec **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="09306-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="09306-252">Tâche 1 : implémentation de la méthode d’Action de modification HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="09306-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="09306-253">Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action de modification pour récupérer l’Album approprié à partir de la base de données, ainsi qu’une liste de tous les Genres et artistes.</span><span class="sxs-lookup"><span data-stu-id="09306-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="09306-254">Il sera empaqueter ces données dans le **StoreManagerViewModel** objet défini dans la dernière étape, qui est ensuite transmise à un modèle de vue pour afficher la réponse avec.</span><span class="sxs-lookup"><span data-stu-id="09306-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="09306-255">Ouvrez le **commencer** solution situé dans **/Ex3-CreatingTheEditView/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="09306-256">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-257">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-258">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-259">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-260">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-261">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-262">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-263">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-264">Ouvrez le **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="09306-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="09306-265">Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="09306-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="09306-266">Remplacez le **HTTP-GET modifier** méthode d’action avec le code suivant pour récupérer le texte approprié **Album** ainsi que le **Genres** et **artistes**répertorie.</span><span class="sxs-lookup"><span data-stu-id="09306-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="09306-267">(Code Snippet - *Validation - Ex3 StoreManagerController HTTP-GET modifier l’action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="09306-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="09306-268">Vous utilisez **System.Web.Mvc** **SelectList** pour les artistes et Genres au lieu du **System.Collections.Generic** liste.</span><span class="sxs-lookup"><span data-stu-id="09306-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="09306-269">**SelectList** est un moyen plus propre pour remplir les listes déroulantes HTML et de gérer des éléments comme la sélection actuelle.</span><span class="sxs-lookup"><span data-stu-id="09306-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="09306-270">Instanciation et la configuration ultérieure ces objets ViewModel dans l’action du contrôleur rendra le scénario de formulaire d’édition plus propre.</span><span class="sxs-lookup"><span data-stu-id="09306-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="09306-271">Tâche 2 : création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="09306-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="09306-272">Dans cette tâche, vous allez créer un modèle de modifier la vue qui affiche les propriétés de l’album ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="09306-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="09306-273">Créer la vue Edit.</span><span class="sxs-lookup"><span data-stu-id="09306-273">Create the Edit View.</span></span> <span data-ttu-id="09306-274">Pour ce faire, le bouton droit dans le **modifier** méthode d’action, puis **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="09306-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="09306-275">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **modifier**.</span><span class="sxs-lookup"><span data-stu-id="09306-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="09306-276">Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **afficher la classe de données** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="09306-277">Sélectionnez **modifier** à partir de la **modèle de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="09306-278">Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="09306-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="09306-279">![Ajout d’une vue d’édition](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Ajout d’une vue d’édition")</span><span class="sxs-lookup"><span data-stu-id="09306-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    *<span data-ttu-id="09306-280">Ajout d’une vue d’édition</span><span class="sxs-lookup"><span data-stu-id="09306-280">Adding an Edit view</span></span>*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="09306-281">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="09306-282">Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les valeurs des propriétés de l’album passée comme paramètre.</span><span class="sxs-lookup"><span data-stu-id="09306-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="09306-283">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-284">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-284">The project starts in the Home page.</span></span> <span data-ttu-id="09306-285">Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier que les valeurs des propriétés de l’album passé sont affichés.</span><span class="sxs-lookup"><span data-stu-id="09306-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="09306-286">![Parcourir la vue de modification d’Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "parcourir la vue de modification d’Album")</span><span class="sxs-lookup"><span data-stu-id="09306-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    *<span data-ttu-id="09306-287">Vue d’édition d’Album de navigation</span><span class="sxs-lookup"><span data-stu-id="09306-287">Browsing Album's Edit view</span></span>*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="09306-288">Tâche 4 : implémentation des listes déroulantes sur le modèle d’éditeur d’Album</span><span class="sxs-lookup"><span data-stu-id="09306-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="09306-289">Dans cette tâche, vous allez ajouter des listes déroulantes du modèle de vue créé dans la dernière tâche, afin que l’utilisateur peut sélectionner à partir d’une liste d’artistes et Genres.</span><span class="sxs-lookup"><span data-stu-id="09306-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="09306-290">Remplacez tout le **Album** code fieldset par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09306-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="09306-291">Un **Html.DropDownList** helper a été ajouté pour afficher les listes déroulantes pour le choix d’artistes et Genres.</span><span class="sxs-lookup"><span data-stu-id="09306-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="09306-292">Les paramètres passés à **Html.DropDownList** sont :</span><span class="sxs-lookup"><span data-stu-id="09306-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="09306-293">Le nom du champ de formulaire (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="09306-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="09306-294">Le **SelectList** des valeurs pour la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="09306-295">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="09306-296">Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les listes déroulantes au lieu de champs de texte artiste et de l’ID du Genre.</span><span class="sxs-lookup"><span data-stu-id="09306-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="09306-297">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-298">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-298">The project starts in the Home page.</span></span> <span data-ttu-id="09306-299">Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier qu’il affiche des listes déroulantes au lieu de champs de texte artiste et de l’ID du Genre.</span><span class="sxs-lookup"><span data-stu-id="09306-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="09306-300">![Exploration modifier la vue d’Album avec les listes déroulantes](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "modifier la vue d’Album de navigation avec les listes déroulantes")</span><span class="sxs-lookup"><span data-stu-id="09306-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    *<span data-ttu-id="09306-301">Vue de modification d’Album, cette fois avec des listes déroulantes de navigation</span><span class="sxs-lookup"><span data-stu-id="09306-301">Browsing Album's Edit view, this time with dropdowns</span></span>*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="09306-302">Tâche 6 : implémentation de la méthode d’action HTTP-POST modifier</span><span class="sxs-lookup"><span data-stu-id="09306-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="09306-303">Maintenant que la vue Modifier affiche comme prévu, vous devez implémenter la méthode de modifier l’Action HTTP-POST pour enregistrer les modifications apportées à l’Album.</span><span class="sxs-lookup"><span data-stu-id="09306-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="09306-304">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09306-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="09306-305">Ouvrez **StoreManagerController** à partir de la **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="09306-306">Remplacez **HTTP-POST modifier** code de méthode d’action par le code suivant (Notez que la méthode qui doit être remplacée est la version surchargée qui reçoit deux paramètres) :</span><span class="sxs-lookup"><span data-stu-id="09306-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="09306-307">(Code Snippet - *Validation - Ex3 StoreManagerController HTTP-POST modifier l’action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="09306-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="09306-308">Cette méthode est exécutée quand l’utilisateur clique sur le **enregistrer** bouton de la vue et effectue une requête HTTP-POST des valeurs de formulaire sur le serveur pour les conserver dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="09306-309">L’élément décoratif **[HttpPost]** indique que la méthode doit être utilisée pour les scénarios HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="09306-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="09306-310">La méthode accepte un **Album** objet.</span><span class="sxs-lookup"><span data-stu-id="09306-310">The method takes an **Album** object.</span></span> <span data-ttu-id="09306-311">ASP.NET MVC crée automatiquement l’objet d’Album à partir de la validé &lt;formulaire&gt; valeurs.</span><span class="sxs-lookup"><span data-stu-id="09306-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="09306-312">La méthode effectuera ces étapes :</span><span class="sxs-lookup"><span data-stu-id="09306-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="09306-313">Si le modèle est valide :</span><span class="sxs-lookup"><span data-stu-id="09306-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="09306-314">Mettre à jour l’entrée de l’album dans le contexte pour la marquer comme un objet modifié.</span><span class="sxs-lookup"><span data-stu-id="09306-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="09306-315">Enregistrer les modifications et rediriger vers la vue index.</span><span class="sxs-lookup"><span data-stu-id="09306-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="09306-316">Si le modèle n’est pas valid, elle remplira ViewBag avec le **GenreId** et **ArtistId**, il retourne la vue avec l’objet Album reçu pour autoriser l’utilisateur effectuer une mise à jour obligatoire.</span><span class="sxs-lookup"><span data-stu-id="09306-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="09306-317">Tâche 7 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="09306-318">Dans cette tâche, vous allez tester que le **StoreManager modifier** page de vue enregistre réellement les données d’Album mis à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="09306-319">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-320">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-320">The project starts in the Home page.</span></span> <span data-ttu-id="09306-321">Remplacez l’URL par **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="09306-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="09306-322">Remplacez le titre d’Album par **charge** , puis cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="09306-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="09306-323">Vérifiez que le titre d’album réellement modifié dans la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="09306-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="09306-324">![La mise à jour un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "mise à jour d’un album")</span><span class="sxs-lookup"><span data-stu-id="09306-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    *<span data-ttu-id="09306-325">La mise à jour d’un Album</span><span class="sxs-lookup"><span data-stu-id="09306-325">Updating an Album</span></span>*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="09306-326">Exercice 4 : Ajout d’une vue de créer</span><span class="sxs-lookup"><span data-stu-id="09306-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="09306-327">Maintenant que le **StoreManagerController** prend en charge la **modifier** capacité, dans cet exercice, vous allez apprendre à ajouter un modèle Create View pour permettent de stocker des gestionnaires Ajouter nouveau Albums à l’application.</span><span class="sxs-lookup"><span data-stu-id="09306-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="09306-328">Comme vous l’avez fait avec la fonctionnalité d’édition, vous allez implémenter le scénario de créer à l’aide de deux méthodes distinctes au sein de la **StoreManagerController** classe :</span><span class="sxs-lookup"><span data-stu-id="09306-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="09306-329">Une méthode d’action affiche un formulaire vide lorsque des responsables de magasin d’abord visiter le **/StoreManager/créer** URL.</span><span class="sxs-lookup"><span data-stu-id="09306-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="09306-330">Une deuxième méthode d’action gère le scénario où le directeur du magasin clique sur le **enregistrer** bouton dans le formulaire et soumet les valeurs de retour à la **/StoreManager/créer** URL sous la forme d’une requête HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="09306-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="09306-331">Tâche 1 : implémentation de la méthode d’action HTTP-GET créer</span><span class="sxs-lookup"><span data-stu-id="09306-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="09306-332">Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action Create pour récupérer une liste de tous les Genres et artistes, empaqueter ces données dans un **StoreManagerViewModel** objet, qui est ensuite transmis à un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="09306-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="09306-333">Ouvrez le **commencer** solution situé dans **/Ex4-AddingACreateView/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="09306-334">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-335">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-336">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-337">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-338">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-339">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-340">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-341">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-342">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="09306-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="09306-343">Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="09306-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="09306-344">Remplacez le **créer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09306-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="09306-345">(Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action de création de HTTP-GET Ex4 StoreManagerController*)</span><span class="sxs-lookup"><span data-stu-id="09306-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="09306-346">Tâche 2 : ajout de la vue Create</span><span class="sxs-lookup"><span data-stu-id="09306-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="09306-347">Dans cette tâche, vous allez ajouter le modèle de créer une vue qui affiche un nouveau formulaire d’Album (vide).</span><span class="sxs-lookup"><span data-stu-id="09306-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="09306-348">Avec le bouton droit à l’intérieur de la **créer** méthode d’action, puis **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="09306-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="09306-349">Cela fera apparaître la boîte de dialogue Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="09306-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="09306-350">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **créer**.</span><span class="sxs-lookup"><span data-stu-id="09306-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="09306-351">Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante et **créer** à partir de la **modèle de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="09306-352">Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="09306-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="09306-353">![Ajout d’une vue de créer](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ajout-a-créer-view.png")</span><span class="sxs-lookup"><span data-stu-id="09306-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    *<span data-ttu-id="09306-354">Ajout de la vue Create</span><span class="sxs-lookup"><span data-stu-id="09306-354">Adding the Create View</span></span>*
3. <span data-ttu-id="09306-355">Mise à jour le **GenreId** et **ArtistId** champs à utiliser une liste déroulante, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="09306-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="09306-356">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="09306-357">Dans cette tâche, vous allez tester que le **StoreManager** **créer** afficher la page affiche un formulaire d’Album vide.</span><span class="sxs-lookup"><span data-stu-id="09306-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="09306-358">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-359">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-359">The project starts in the Home page.</span></span> <span data-ttu-id="09306-360">Remplacez l’URL par **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="09306-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="09306-361">Vérifiez qu’un formulaire vide s’affiche pour remplir les nouvelles propriétés de l’Album.</span><span class="sxs-lookup"><span data-stu-id="09306-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="09306-362">![Créer une vue avec un formulaire vide](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "créer une vue avec un formulaire vide")</span><span class="sxs-lookup"><span data-stu-id="09306-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    *<span data-ttu-id="09306-363">Créer une vue avec un formulaire vide</span><span class="sxs-lookup"><span data-stu-id="09306-363">Create View with an empty form</span></span>*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="09306-364">Tâche 4 : implémentation d’une requête HTTP-POST créer la méthode d’Action</span><span class="sxs-lookup"><span data-stu-id="09306-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="09306-365">Dans cette tâche, vous allez implémenter la version de HTTP-POST de la méthode d’action de création qui sera appelée quand un utilisateur clique sur le **enregistrer** bouton.</span><span class="sxs-lookup"><span data-stu-id="09306-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="09306-366">La méthode doit enregistrer le nouvel album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="09306-367">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09306-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="09306-368">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="09306-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="09306-369">Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="09306-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="09306-370">Remplacez **HTTP-POST créer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09306-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="09306-371">(Code Snippet - *Validation - Ex4 StoreManagerController HTTP - POST permet de créer une action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="09306-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="09306-372">L’action de création est assez similaire à la méthode d’action de modification précédente, mais au lieu de définir l’objet comme modifiée, il est ajouté au contexte.</span><span class="sxs-lookup"><span data-stu-id="09306-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="09306-373">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="09306-374">Dans cette tâche, vous allez tester que le **StoreManager créer** page de vue vous permet de créer un nouvel Album, il redirige vers la vue Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="09306-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="09306-375">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-376">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-376">The project starts in the Home page.</span></span> <span data-ttu-id="09306-377">Remplacez l’URL par **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="09306-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="09306-378">Remplissez tous les champs de formulaire avec des données pour un nouvel Album, comme celui de la figure suivante :</span><span class="sxs-lookup"><span data-stu-id="09306-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="09306-379">![Création d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "création d’un Album")</span><span class="sxs-lookup"><span data-stu-id="09306-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    *<span data-ttu-id="09306-380">Création d’un Album</span><span class="sxs-lookup"><span data-stu-id="09306-380">Creating an Album</span></span>*
3. <span data-ttu-id="09306-381">Vérifiez que vous êtes redirigé vers la vue Index StoreManager qui inclut le nouvel Album venez de créer.</span><span class="sxs-lookup"><span data-stu-id="09306-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="09306-382">![Nouvel Album créé](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nouvel Album créé")</span><span class="sxs-lookup"><span data-stu-id="09306-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    *<span data-ttu-id="09306-383">Nouvel Album créé</span><span class="sxs-lookup"><span data-stu-id="09306-383">New Album Created</span></span>*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="09306-384">Exercice 5 : Suppression de la gestion</span><span class="sxs-lookup"><span data-stu-id="09306-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="09306-385">La possibilité de supprimer des albums n’est pas encore implémentée.</span><span class="sxs-lookup"><span data-stu-id="09306-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="09306-386">Voici ce que cet exercice sera sur.</span><span class="sxs-lookup"><span data-stu-id="09306-386">This is what this exercise will be about.</span></span> <span data-ttu-id="09306-387">Comme auparavant, vous allez implémenter le scénario de suppression à l’aide de deux méthodes distinctes au sein de la **StoreManagerController** classe :</span><span class="sxs-lookup"><span data-stu-id="09306-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="09306-388">Une méthode d’action affiche un écran de confirmation</span><span class="sxs-lookup"><span data-stu-id="09306-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="09306-389">Une deuxième méthode d’action gère l’envoi du formulaire</span><span class="sxs-lookup"><span data-stu-id="09306-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="09306-390">Tâche 1 : implémentation de la méthode d’Action Delete HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="09306-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="09306-391">Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action de suppression pour récupérer les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="09306-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="09306-392">Ouvrez le **commencer** solution situé dans **/Ex5-HandlingDeletion/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="09306-393">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-394">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-395">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-396">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-397">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-398">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-399">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-400">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-401">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="09306-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="09306-402">Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="09306-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="09306-403">L’action de contrôleur de suppression est exactement le même que l’action de contrôleur Store détails précédente : elle interroge le **album** objet à partir de la base de données à l’aide de la **id** fourni dans l’URL et retourne le appropriée **vue**.</span><span class="sxs-lookup"><span data-stu-id="09306-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="09306-404">Pour ce faire, remplacez la HTTP-GET **supprimer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09306-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="09306-405">(Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action Ex5 gestion suppression HTTP-GET Delete*)</span><span class="sxs-lookup"><span data-stu-id="09306-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="09306-406">Avec le bouton droit à l’intérieur de la **supprimer** méthode d’action, puis **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="09306-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="09306-407">Cela fera apparaître la boîte de dialogue Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="09306-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="09306-408">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="09306-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="09306-409">Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="09306-410">Sélectionnez **supprimer** à partir de la **modèle de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="09306-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="09306-411">Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="09306-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="09306-412">![Ajout d’une vue Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Ajout d’une vue de la suppression")</span><span class="sxs-lookup"><span data-stu-id="09306-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    *<span data-ttu-id="09306-413">Ajout d’une vue de la suppression</span><span class="sxs-lookup"><span data-stu-id="09306-413">Adding a Delete View</span></span>*
6. <span data-ttu-id="09306-414">Le modèle de suppression affiche tous les champs à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="09306-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="09306-415">Vous afficherez uniquement les titre d’album.</span><span class="sxs-lookup"><span data-stu-id="09306-415">You will show only the album's title.</span></span> <span data-ttu-id="09306-416">Pour ce faire, remplacez le contenu de la vue par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09306-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="09306-417">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="09306-418">Dans cette tâche, vous allez tester que le **StoreManager** **supprimer** afficher la page affiche un formulaire de suppression de confirmation.</span><span class="sxs-lookup"><span data-stu-id="09306-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="09306-419">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-420">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-420">The project starts in the Home page.</span></span> <span data-ttu-id="09306-421">Remplacez l’URL par **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="09306-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="09306-422">Sélectionnez un album à supprimer en cliquant sur **supprimer** et vérifiez que la nouvelle vue est téléchargée.</span><span class="sxs-lookup"><span data-stu-id="09306-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="09306-423">![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "suppression d’un Album")</span><span class="sxs-lookup"><span data-stu-id="09306-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    *<span data-ttu-id="09306-424">Suppression d’un Album</span><span class="sxs-lookup"><span data-stu-id="09306-424">Deleting an Album</span></span>*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="09306-425">Tâche 3-implémentation de la méthode d’Action Delete HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="09306-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="09306-426">Dans cette tâche, vous allez implémenter la version de HTTP-POST de la méthode d’action Delete qui est appelée lorsqu’un utilisateur clique sur le **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="09306-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="09306-427">La méthode doit supprimer l’album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="09306-428">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09306-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="09306-429">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="09306-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="09306-430">Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="09306-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="09306-431">Remplacez **HTTP-POST et Delete** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09306-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="09306-432">(Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action Ex5 gestion suppression HTTP-POST et Delete*)</span><span class="sxs-lookup"><span data-stu-id="09306-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="09306-433">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="09306-434">Dans cette tâche, vous allez tester que le **StoreManager Delete** page de vue vous permet de supprimer un Album, il redirige vers la vue Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="09306-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="09306-435">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-436">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-436">The project starts in the Home page.</span></span> <span data-ttu-id="09306-437">Remplacez l’URL par **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="09306-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="09306-438">Sélectionnez un album à supprimer en cliquant sur **supprimer.**</span><span class="sxs-lookup"><span data-stu-id="09306-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="09306-439">Confirmer la suppression en cliquant sur **supprimer** bouton :</span><span class="sxs-lookup"><span data-stu-id="09306-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="09306-440">![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "suppression d’un Album")</span><span class="sxs-lookup"><span data-stu-id="09306-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    *<span data-ttu-id="09306-441">Suppression d’un Album</span><span class="sxs-lookup"><span data-stu-id="09306-441">Deleting an Album</span></span>*
3. <span data-ttu-id="09306-442">Vérifiez que l’album a été supprimée, car il n’apparaît pas dans le **Index** page.</span><span class="sxs-lookup"><span data-stu-id="09306-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="09306-443">Exercice 6 : Ajout de la validation</span><span class="sxs-lookup"><span data-stu-id="09306-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="09306-444">Actuellement, les formulaires de créer et modifier en place sans effectuer n’importe quel type de validation.</span><span class="sxs-lookup"><span data-stu-id="09306-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="09306-445">Si l’utilisateur quitte un champ obligatoire vide ou les lettres de type dans le champ de prix, la première erreur, que vous obtenez sera effectué depuis la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="09306-446">Vous pouvez ajouter la validation à l’application en ajoutant des Annotations de données à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="09306-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="09306-447">Annotations de données décrivant les règles appliquées aux propriétés de votre modèle et de ASP.NET MVC s’occupera de l’application et afficher le message approprié pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="09306-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="09306-448">Tâche 1 : ajouter des Annotations de données</span><span class="sxs-lookup"><span data-stu-id="09306-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="09306-449">Dans cette tâche, vous allez ajouter des Annotations de données au modèle Album qui rend la page Create et Edit affiche des messages de validation lorsque cela est approprié.</span><span class="sxs-lookup"><span data-stu-id="09306-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="09306-450">Pour une classe de modèle simple, ajout d’une Annotation de données est uniquement géré en ajoutant un **à l’aide de** instruction pour **System.ComponentModel.DataAnnotation**, puis placer un **[obligatoire]** attribut sur les propriétés appropriées.</span><span class="sxs-lookup"><span data-stu-id="09306-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="09306-451">L’exemple suivant rendrait la **nom** propriété un champ obligatoire dans la vue.</span><span class="sxs-lookup"><span data-stu-id="09306-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="09306-452">Il s’agit un peu plus complexe dans les cas de cette application où l’Entity Data Model est généré.</span><span class="sxs-lookup"><span data-stu-id="09306-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="09306-453">Si vous avez ajouté des Annotations de données directement sur les classes de modèle, ils seront remplacées si vous mettez à jour le modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="09306-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="09306-454">Au lieu de cela, vous pouvez rendre l’utilisation des métadonnées des classes partielles qui existera pour contenir les annotations et sont associées avec le modèle de classes à l’aide de la **[MetadataType]** attribut.</span><span class="sxs-lookup"><span data-stu-id="09306-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="09306-455">Ouvrez le **commencer** solution situé dans **/Ex6-AddingValidation/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="09306-456">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-457">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-458">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-459">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-460">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-461">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-462">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-463">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-464">Ouvrez le **Album.cs** à partir de la **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="09306-465">Remplacez **Album.cs** de contenu avec le code en surbrillance, afin qu’il ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="09306-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="09306-466">La ligne **[DisplayFormat(ConvertEmptyStringToNull=false)]** indique que des chaînes vides à partir du modèle ne sont pas être converties en valeurs null lorsque le champ de données est mis à jour dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="09306-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="09306-467">Ce paramètre permet d’éviter une exception lorsqu’Entity Framework attribue des valeurs null pour le modèle avant de l’Annotation de données valide les champs.</span><span class="sxs-lookup"><span data-stu-id="09306-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="09306-468">(Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - classe partielle de métadonnées Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="09306-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="09306-469">Cela **Album** classe partielle a une **MetadataType** attribut qui pointe vers le **AlbumMetaData** classe pour les Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="09306-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="09306-470">Voici certains des attributs d’Annotation de données que vous utilisez pour annoter le modèle d’Album :</span><span class="sxs-lookup"><span data-stu-id="09306-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="09306-471">Obligatoire - indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="09306-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="09306-472">DisplayName : définit le texte à utiliser sur les champs de formulaire et les messages de validation</span><span class="sxs-lookup"><span data-stu-id="09306-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="09306-473">DisplayFormat - Spécifie le mode d’affichage et de mise en forme des champs de données.</span><span class="sxs-lookup"><span data-stu-id="09306-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="09306-474">StringLength - définit une longueur maximale pour un champ de chaîne</span><span class="sxs-lookup"><span data-stu-id="09306-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="09306-475">Plage - donne la valeur minimale et maximale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="09306-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="09306-476">ScaffoldColumn - permet de masquer des champs de formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="09306-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="09306-477">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="09306-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="09306-478">Dans cette tâche, vous allez tester les pages Create et Edit valident des champs, en utilisant les noms d’affichage choisis dans la dernière tâche.</span><span class="sxs-lookup"><span data-stu-id="09306-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="09306-479">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="09306-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="09306-480">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-480">The project starts in the Home page.</span></span> <span data-ttu-id="09306-481">Remplacez l’URL par **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="09306-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="09306-482">Vérifiez que les noms d’affichage correspondent à celles de la classe partielle (tels que **Album Art URL** au lieu de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="09306-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="09306-483">Cliquez sur **créer**, sans le remplir le formulaire.</span><span class="sxs-lookup"><span data-stu-id="09306-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="09306-484">Vérifiez que vous obtenez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="09306-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="09306-485">![Validé des champs dans la page Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validé des champs dans la page Create")</span><span class="sxs-lookup"><span data-stu-id="09306-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    *<span data-ttu-id="09306-486">Champs validés dans la page Create</span><span class="sxs-lookup"><span data-stu-id="09306-486">Validated fields in the Create page</span></span>*
4. <span data-ttu-id="09306-487">Vous pouvez vérifier que s’applique également à la **modifier** page.</span><span class="sxs-lookup"><span data-stu-id="09306-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="09306-488">Remplacez l’URL par **/StoreManager/Edit/1** et vérifiez que les noms d’affichage correspondent à celles de la classe partielle (tels que **Album Art URL** au lieu de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="09306-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="09306-489">Vide le **titre** et **prix** champs et cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="09306-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="09306-490">Vérifiez que vous obtenez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="09306-490">Verify that you get the corresponding validation messages.</span></span>

    ![Champs validés dans la page de modification](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *<span data-ttu-id="09306-492">Champs validés dans la page de modification</span><span class="sxs-lookup"><span data-stu-id="09306-492">Validated fields in the Edit page</span></span>*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="09306-493">Exercice 7 : À l’aide de jQuery non obstructive côté Client</span><span class="sxs-lookup"><span data-stu-id="09306-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="09306-494">Dans cet exercice, vous allez apprendre à activer MVC 4 Unobtrusive validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="09306-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="09306-495">Le jQuery non obstructive utilise le préfixe de données-ajax JavaScript pour appeler des méthodes d’action sur le serveur et non intrusive émettant les scripts client.</span><span class="sxs-lookup"><span data-stu-id="09306-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="09306-496">Tâche 1 : exécution de l’Application avant l’activation non obstructive jQuery</span><span class="sxs-lookup"><span data-stu-id="09306-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="09306-497">Dans cette tâche, vous allez exécuter l’application avant d’inclure jQuery pour comparer les deux modèles de validation.</span><span class="sxs-lookup"><span data-stu-id="09306-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="09306-498">Ouvrez le **commencer** solution situé dans **/Ex7-UnobtrusivejQueryValidation/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="09306-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="09306-499">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="09306-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="09306-500">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09306-501">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09306-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="09306-502">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="09306-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="09306-503">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="09306-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="09306-504">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="09306-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09306-505">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="09306-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09306-506">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="09306-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09306-507">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="09306-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="09306-508">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-508">The project starts in the Home page.</span></span> <span data-ttu-id="09306-509">Parcourir **/StoreManager/créer** et cliquez sur **créer** sans remplir le formulaire pour vérifier que vous obtenez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="09306-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="09306-510">![Validation côté client désactivée](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validation côté Client désactivée")</span><span class="sxs-lookup"><span data-stu-id="09306-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    *<span data-ttu-id="09306-511">Validation côté client désactivée</span><span class="sxs-lookup"><span data-stu-id="09306-511">Client validation disabled</span></span>*
4. <span data-ttu-id="09306-512">Dans le navigateur, ouvrez le code source HTML :</span><span class="sxs-lookup"><span data-stu-id="09306-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="09306-513">Tâche 2 - Validation de Client non obstructive l’activation</span><span class="sxs-lookup"><span data-stu-id="09306-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="09306-514">Dans cette tâche, vous allez activer jQuery **validation client non obstructive** de **Web.config** fichier, qui est définie sur false dans tous les nouveaux projets ASP.NET MVC 4 par défaut.</span><span class="sxs-lookup"><span data-stu-id="09306-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="09306-515">En outre, vous allez ajouter de que références pour rendre le travail de Validation Client discrète de jQuery les scripts nécessaires.</span><span class="sxs-lookup"><span data-stu-id="09306-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="09306-516">Ouvrez **Web.Config** de fichiers à la racine du projet et vous assurer que le **ClientValidationEnabled** et **UnobtrusiveJavaScriptEnabled** les valeurs de clés sont définies sur **true**.</span><span class="sxs-lookup"><span data-stu-id="09306-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="09306-517">Vous pouvez également activer la validation côté client par le code à Global.asax.cs pour obtenir les mêmes résultats :</span><span class="sxs-lookup"><span data-stu-id="09306-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > **<span data-ttu-id="09306-518">HtmlHelper.ClientValidationEnabled = true;</span><span class="sxs-lookup"><span data-stu-id="09306-518">HtmlHelper.ClientValidationEnabled = true;</span></span>**
    > 
    > <span data-ttu-id="09306-519">En outre, vous pouvez affecter ClientValidationEnabled attribut dans n’importe quel contrôleur pour avoir un comportement personnalisé.</span><span class="sxs-lookup"><span data-stu-id="09306-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="09306-520">Ouvrez **Create.cshtml** à **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="09306-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="09306-521">Assurez-vous que les fichiers de script suivant, **jquery.validate** et **jquery.validate.unobtrusive**, sont référencées dans la vue via la &quot; **~/bundles/jqueryval** &quot; offre groupée.</span><span class="sxs-lookup"><span data-stu-id="09306-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="09306-522">Toutes ces bibliothèques jQuery sont inclus dans les nouveaux projets de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="09306-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="09306-523">Vous trouverez d’autres bibliothèques dans le **/Scripts** dossier de votre projet.</span><span class="sxs-lookup"><span data-stu-id="09306-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="09306-524">Pour effectuer cette validation bibliothèques fonctionnent, vous devez ajouter une référence à la bibliothèque de framework de jQuery.</span><span class="sxs-lookup"><span data-stu-id="09306-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="09306-525">Étant donné que cette référence est déjà ajoutée dans le  **\_Layout.cshtml** fichier, vous n’avez pas besoin de l’ajouter dans cette vue particulière.</span><span class="sxs-lookup"><span data-stu-id="09306-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="09306-526">Tâche 3 : l’Application à l’aide de discrète jQuery Validation en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="09306-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="09306-527">Dans cette tâche, vous allez tester que le **StoreManager** créer la vue modèle effectue la validation côté client à l’aide des bibliothèques de jQuery lorsque l’utilisateur crée un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="09306-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="09306-528">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="09306-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="09306-529">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="09306-529">The project starts in the Home page.</span></span> <span data-ttu-id="09306-530">Parcourir **/StoreManager/créer** et cliquez sur **créer** sans remplir le formulaire pour vérifier que vous obtenez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="09306-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="09306-531">![Validation côté client avec jQuery activé](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validation côté Client avec jQuery activé")</span><span class="sxs-lookup"><span data-stu-id="09306-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    *<span data-ttu-id="09306-532">Validation côté client avec jQuery activé</span><span class="sxs-lookup"><span data-stu-id="09306-532">Client validation with jQuery enabled</span></span>*
3. <span data-ttu-id="09306-533">Dans le navigateur, ouvrez le code source pour créer une vue :</span><span class="sxs-lookup"><span data-stu-id="09306-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="09306-534">Pour chaque règle de validation client jQuery non obstructive ajoute un attribut avec des données-val -*rulename*=&quot;*message*&quot;.</span><span class="sxs-lookup"><span data-stu-id="09306-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="09306-535">Voici une liste de balises que discrète jQuery insère dans le champ d’entrée html pour effectuer la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="09306-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="09306-536">Données-val</span><span class="sxs-lookup"><span data-stu-id="09306-536">Data-val</span></span>
   > - <span data-ttu-id="09306-537">Numéro de données val</span><span class="sxs-lookup"><span data-stu-id="09306-537">Data-val-number</span></span>
   > - <span data-ttu-id="09306-538">Plage de données val</span><span class="sxs-lookup"><span data-stu-id="09306-538">Data-val-range</span></span>
   > - <span data-ttu-id="09306-539">Données-val-range-min / données-val-range-max</span><span class="sxs-lookup"><span data-stu-id="09306-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="09306-540">Données val requises</span><span class="sxs-lookup"><span data-stu-id="09306-540">Data-val-required</span></span>
   > - <span data-ttu-id="09306-541">Longueur des données-val</span><span class="sxs-lookup"><span data-stu-id="09306-541">Data-val-length</span></span>
   > - <span data-ttu-id="09306-542">Données-val-longueur-max / données val-Longueur-min.</span><span class="sxs-lookup"><span data-stu-id="09306-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="09306-543">Toutes les valeurs de données sont remplis avec modèle **Annotation de données**.</span><span class="sxs-lookup"><span data-stu-id="09306-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="09306-544">Ensuite, toute la logique qui fonctionne au côté serveur peut être exécutée côté client.</span><span class="sxs-lookup"><span data-stu-id="09306-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="09306-545">Par exemple, attribut prix a l’annotation de données suivante dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="09306-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="09306-546">Après avoir utilisé jQuery non obstructive, le code généré est :</span><span class="sxs-lookup"><span data-stu-id="09306-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="09306-547">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="09306-547">Summary</span></span>

<span data-ttu-id="09306-548">En fin de cet atelier pratique, que vous savez comment permettre aux utilisateurs de modifier les données stockées dans la base de données à l’aide des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09306-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="09306-549">Actions de contrôleur comme Index, Create, Edit, Delete</span><span class="sxs-lookup"><span data-stu-id="09306-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="09306-550">Fonctionnalité de génération de modèles automatique de ASP.NET MVC pour l’affichage des propriétés dans un tableau HTML</span><span class="sxs-lookup"><span data-stu-id="09306-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="09306-551">Expérience de programmes d’assistance HTML personnalisées afin d’améliorer l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="09306-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="09306-552">Méthodes d’action qui réagissent aux HTTP-GET ou d’appels HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="09306-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="09306-553">Un modèle d’éditeur partagé pour les modèles de vue similaires telles que Create et Edit</span><span class="sxs-lookup"><span data-stu-id="09306-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="09306-554">Éléments de formulaire tels que des listes déroulantes</span><span class="sxs-lookup"><span data-stu-id="09306-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="09306-555">Annotations de données pour la validation de modèle</span><span class="sxs-lookup"><span data-stu-id="09306-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="09306-556">Validation côté client à l’aide de la bibliothèque jQuery non obstructive</span><span class="sxs-lookup"><span data-stu-id="09306-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="09306-557">Annexe a : Installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="09306-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="09306-558">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="09306-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="09306-559">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="09306-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="09306-560">Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="09306-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="09306-561">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="09306-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="09306-562">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="09306-562">Click on **Install Now**.</span></span> <span data-ttu-id="09306-563">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="09306-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="09306-564">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="09306-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="09306-565">![Installer Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="09306-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    *<span data-ttu-id="09306-566">Installer Visual Studio Express</span><span class="sxs-lookup"><span data-stu-id="09306-566">Install Visual Studio Express</span></span>*
4. <span data-ttu-id="09306-567">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="09306-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *<span data-ttu-id="09306-569">Accepter les termes du contrat de licence</span><span class="sxs-lookup"><span data-stu-id="09306-569">Accepting the license terms</span></span>*
5. <span data-ttu-id="09306-570">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="09306-570">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *<span data-ttu-id="09306-572">Progression de l'installation</span><span class="sxs-lookup"><span data-stu-id="09306-572">Installation progress</span></span>*
6. <span data-ttu-id="09306-573">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="09306-573">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *<span data-ttu-id="09306-575">Installation est terminée</span><span class="sxs-lookup"><span data-stu-id="09306-575">Installation completed</span></span>*
7. <span data-ttu-id="09306-576">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="09306-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="09306-577">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="09306-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *<span data-ttu-id="09306-579">VS Express pour une vignette de Web</span><span class="sxs-lookup"><span data-stu-id="09306-579">VS Express for Web tile</span></span>*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="09306-580">Annexe b : À l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="09306-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="09306-581">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="09306-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="09306-582">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="09306-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="09306-583">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="09306-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

*<span data-ttu-id="09306-584">À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet</span><span class="sxs-lookup"><span data-stu-id="09306-584">Using Visual Studio code snippets to insert code into your project</span></span>*

***<span data-ttu-id="09306-585">Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)</span><span class="sxs-lookup"><span data-stu-id="09306-585">To add a code snippet using the keyboard (C# only)</span></span>***

1. <span data-ttu-id="09306-586">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="09306-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="09306-587">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="09306-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="09306-588">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="09306-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="09306-589">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="09306-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="09306-590">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="09306-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="09306-591">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="09306-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

*<span data-ttu-id="09306-592">Commencez à taper le nom de l’extrait de code</span><span class="sxs-lookup"><span data-stu-id="09306-592">Start typing the snippet name</span></span>*

<span data-ttu-id="09306-593">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="09306-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

*<span data-ttu-id="09306-594">Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance</span><span class="sxs-lookup"><span data-stu-id="09306-594">Press Tab to select the highlighted snippet</span></span>*

<span data-ttu-id="09306-595">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="09306-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

*<span data-ttu-id="09306-596">Appuyez sur Tab à nouveau et l’extrait de code seront développe.</span><span class="sxs-lookup"><span data-stu-id="09306-596">Press Tab again and the snippet will expand</span></span>*

<span data-ttu-id="09306-597">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="09306-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="09306-598">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="09306-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="09306-599">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="09306-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="09306-600">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="09306-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="09306-601">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="09306-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

*<span data-ttu-id="09306-602">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait</span><span class="sxs-lookup"><span data-stu-id="09306-602">Right-click where you want to insert the code snippet and select Insert Snippet</span></span>*

<span data-ttu-id="09306-603">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="09306-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

*<span data-ttu-id="09306-604">Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci</span><span class="sxs-lookup"><span data-stu-id="09306-604">Pick the relevant snippet from the list, by clicking on it</span></span>*
