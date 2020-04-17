---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet de ASP.NET MVC (fr) Microsoft Docs
author: rick-anderson
description: L’étape 1 vous montre comment mettre en place la structure d’application nerdDinner de base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 240c8a04cec3c07f434182775d1c519aab029761
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541479"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="2cbff-103">Créer un projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2cbff-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="2cbff-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2cbff-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="2cbff-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="2cbff-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="2cbff-106">Il s’agit de l’étape 1 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="2cbff-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="2cbff-107">L’étape 1 vous montre comment mettre en place la structure d’application nerdDinner de base.</span><span class="sxs-lookup"><span data-stu-id="2cbff-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="2cbff-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="2cbff-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="2cbff-109">NerdDinner Étape 1:&gt;Fichier- Nouveau projet</span><span class="sxs-lookup"><span data-stu-id="2cbff-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="2cbff-110">Nous commencerons notre application NerdDinner en sélectionnant **l’élément&gt;de** menu File- Nouveau Projet dans Visual Studio 2008 ou le Visual Web Developer 2008 Express gratuit.</span><span class="sxs-lookup"><span data-stu-id="2cbff-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="2cbff-111">Cela fera l’histoire du dialogue « Nouveau projet ».</span><span class="sxs-lookup"><span data-stu-id="2cbff-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="2cbff-112">Pour créer une nouvelle application ASP.NET MVC, nous sélectionnerons le nœud « Web » sur le côté gauche du dialogue, puis choisirons le modèle de projet « ASP.NET MVC Web Application » à droite :</span><span class="sxs-lookup"><span data-stu-id="2cbff-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="2cbff-113">*Important: Assurez-vous que vous avez téléchargé et installé ASP.NET MVC - sinon il ne sera pas apparaître dans le dialogue Nouveau Projet. Vous pouvez utiliser V2 de l’installateur de [plate-forme Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si vous ne l’avez pas encore installé (ASP.NET MVC est disponible dans la section « Plateforme Web-&gt;Frameworks et Runtimes »).*</span><span class="sxs-lookup"><span data-stu-id="2cbff-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="2cbff-114">Nous allons nommer le nouveau projet que nous allons créer "NerdDinner" et ensuite cliquer sur le bouton "ok" pour le créer.</span><span class="sxs-lookup"><span data-stu-id="2cbff-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="2cbff-115">Lorsque nous cliquez sur "ok" Visual Studio apportera un dialogue supplémentaire qui nous invite à créer en option un projet de test unitaire pour la nouvelle application ainsi.</span><span class="sxs-lookup"><span data-stu-id="2cbff-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="2cbff-116">Ce projet de test unitaire nous permet de créer des tests automatisés qui vérifient la fonctionnalité et le comportement de notre application (quelque chose que nous couvrirons comment faire plus tard dans ce tutoriel).</span><span class="sxs-lookup"><span data-stu-id="2cbff-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="2cbff-117">Le dropdown «cadre d’essai» dans le dialogue ci-dessus est peuplé de tous les modèles disponibles ASP.NET projet de test unitaire MVC installés sur la machine.</span><span class="sxs-lookup"><span data-stu-id="2cbff-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="2cbff-118">Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit.</span><span class="sxs-lookup"><span data-stu-id="2cbff-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="2cbff-119">Le cadre intégré visual studio Unit Test est également pris en charge.</span><span class="sxs-lookup"><span data-stu-id="2cbff-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="2cbff-120">*Remarque : Le cadre de test de l’unité de studio visuel n’est disponible qu’avec visual Studio 2008 Versions professionnelles et supérieures. Si vous utilisez VS 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devrez télécharger et installer les extensions NUnit, MBUnit ou XUnit pour ASP.NET MVC afin que ce dialogue soit affiché. Le dialogue ne s’affiche pas s’il n’y a pas de cadres de test installés.*</span><span class="sxs-lookup"><span data-stu-id="2cbff-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="2cbff-121">Nous utiliserons le nom par défaut "NerdDinner.Tests" pour le projet de test que nous créons, et utiliserons l’option cadre "Visual Studio Unit Test".</span><span class="sxs-lookup"><span data-stu-id="2cbff-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="2cbff-122">Lorsque nous cliquez sur le bouton "ok" Visual Studio va créer une solution pour nous avec deux projets en elle - un pour notre application web et un pour nos tests unitaires:</span><span class="sxs-lookup"><span data-stu-id="2cbff-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="2cbff-123">Examen de la structure du répertoire NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2cbff-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="2cbff-124">Lorsque vous créez une nouvelle application ASP.NET MVC avec Visual Studio, elle ajoute automatiquement un certain nombre de fichiers et d’annuaires au projet :</span><span class="sxs-lookup"><span data-stu-id="2cbff-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="2cbff-125">ASP.NET projets MVC par défaut ont six répertoires de haut niveau :</span><span class="sxs-lookup"><span data-stu-id="2cbff-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="2cbff-126">**Directory**</span><span class="sxs-lookup"><span data-stu-id="2cbff-126">**Directory**</span></span> | <span data-ttu-id="2cbff-127">**Objectif**</span><span class="sxs-lookup"><span data-stu-id="2cbff-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="2cbff-128">**/Contrôleurs**</span><span class="sxs-lookup"><span data-stu-id="2cbff-128">**/Controllers**</span></span> | <span data-ttu-id="2cbff-129">Où vous mettez des classes Controller qui traitent les demandes d’URL</span><span class="sxs-lookup"><span data-stu-id="2cbff-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="2cbff-130">**/Modèles**</span><span class="sxs-lookup"><span data-stu-id="2cbff-130">**/Models**</span></span> | <span data-ttu-id="2cbff-131">Où vous mettez des classes qui représentent et manipulent les données</span><span class="sxs-lookup"><span data-stu-id="2cbff-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="2cbff-132">**/Vues**</span><span class="sxs-lookup"><span data-stu-id="2cbff-132">**/Views**</span></span> | <span data-ttu-id="2cbff-133">Lorsque vous mettez des fichiers de modèle d’interface utilisateur qui sont responsables du rendu de la sortie</span><span class="sxs-lookup"><span data-stu-id="2cbff-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="2cbff-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="2cbff-134">**/Scripts**</span></span> | <span data-ttu-id="2cbff-135">Où vous mettez des fichiers et des scripts de bibliothèque JavaScript (.js)</span><span class="sxs-lookup"><span data-stu-id="2cbff-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="2cbff-136">**/Contenu**</span><span class="sxs-lookup"><span data-stu-id="2cbff-136">**/Content**</span></span> | <span data-ttu-id="2cbff-137">Où vous mettez des fichiers CSS et images, et d’autres contenus non dynamiques/non-JavaScript</span><span class="sxs-lookup"><span data-stu-id="2cbff-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="2cbff-138">**/Données\_d’application**</span><span class="sxs-lookup"><span data-stu-id="2cbff-138">**/App\_Data**</span></span> | <span data-ttu-id="2cbff-139">Où vous stockez des fichiers de données que vous souhaitez lire/écrire.</span><span class="sxs-lookup"><span data-stu-id="2cbff-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="2cbff-140">ASP.NET MVC n’a pas besoin de cette structure.</span><span class="sxs-lookup"><span data-stu-id="2cbff-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="2cbff-141">En fait, les développeurs travaillant sur de grandes applications répartiront généralement l’application à travers plusieurs projets pour la rendre plus gérable (par exemple : les classes de modèles de données vont souvent dans un projet de bibliothèque de classe distinct de l’application Web).</span><span class="sxs-lookup"><span data-stu-id="2cbff-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="2cbff-142">La structure de projet par défaut, cependant, fournit une convention d’annuaire par défaut agréable que nous pouvons utiliser pour garder nos préoccupations d’application propre.</span><span class="sxs-lookup"><span data-stu-id="2cbff-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="2cbff-143">Lorsque nous agrandissons le répertoire /Controllers, nous constaterons que Visual Studio a ajouté deux classes de contrôleurs - HomeController et AccountController - par défaut au projet :</span><span class="sxs-lookup"><span data-stu-id="2cbff-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="2cbff-144">Lorsque nous élargissons l’annuaire /Views, nous trouverons trois sous-répertoires - /Home, /Compte et /Partagé - ainsi que plusieurs fichiers de modèle en leur sein ont également été ajoutés au projet par défaut:</span><span class="sxs-lookup"><span data-stu-id="2cbff-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="2cbff-145">Lorsque nous élargissons les répertoires /Content et /Scripts, nous trouverons un fichier Site.css qui est utilisé pour coiffer tous les HTML sur le site, ainsi que les bibliothèques JavaScript qui peuvent activer ASP.NET’AJAX et le support jQuery dans l’application:</span><span class="sxs-lookup"><span data-stu-id="2cbff-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="2cbff-146">Lorsque nous étendrons le projet NerdDinner.Tests, nous trouverons deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="2cbff-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="2cbff-147">Ces fichiers par défaut ajoutés par Visual Studio nous fournissent une structure de base pour une application de travail - avec page d’accueil, sur la page, la connexion de compte / logout / pages d’enregistrement, et une page d’erreur non manipulée (tous câblés et de travail hors de la boîte).</span><span class="sxs-lookup"><span data-stu-id="2cbff-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="2cbff-148">Exécution de l’application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2cbff-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="2cbff-149">Nous pouvons exécuter le projet en choisissant soit le **&gt;Debug- Start Debugging** ou **Debug-&gt;Start Without Debugging** éléments du menu:</span><span class="sxs-lookup"><span data-stu-id="2cbff-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="2cbff-150">Cela lancera le serveur Web intégré ASP.NET qui est livré avec Visual Studio, et exécutera notre application :</span><span class="sxs-lookup"><span data-stu-id="2cbff-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="2cbff-151">Voici la page d’accueil de notre nouveau projet (URL: "/") lorsqu’il s’exécute:</span><span class="sxs-lookup"><span data-stu-id="2cbff-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="2cbff-152">En cliquant sur l’onglet "A propos" affiche une page sur (URL: "/Home/About"):</span><span class="sxs-lookup"><span data-stu-id="2cbff-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="2cbff-153">En cliquant sur le lien "Log On" en haut à droite, nous amène à une page Login (URL: "/Compte/LogOn")</span><span class="sxs-lookup"><span data-stu-id="2cbff-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="2cbff-154">Si nous n’avons pas de compte de connexion, nous pouvons cliquer sur le lien de registre (URL: "/Compte/Register") pour en créer un :</span><span class="sxs-lookup"><span data-stu-id="2cbff-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="2cbff-155">Le code pour implémenter la maison ci-dessus, environ, et logout / fonction d’enregistrement a été ajouté par défaut lorsque nous avons créé notre nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="2cbff-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="2cbff-156">Nous l’utiliserons comme point de départ de notre application.</span><span class="sxs-lookup"><span data-stu-id="2cbff-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="2cbff-157">Test de l’application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2cbff-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="2cbff-158">Si nous utilisons l’édition professionnelle ou la version supérieure de Visual Studio 2008, nous pouvons utiliser le support IDE de test unitaire intégré au sein de Visual Studio pour tester le projet :</span><span class="sxs-lookup"><span data-stu-id="2cbff-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="2cbff-159">Choisir l’une des options ci-dessus ouvrira le volet «Résultats d’essai» au sein de l’IDE et nous donnera un statut de passage/échec sur les 27 tests unitaires inclus dans notre nouveau projet qui couvrent la fonctionnalité intégrée :</span><span class="sxs-lookup"><span data-stu-id="2cbff-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="2cbff-160">Plus tard dans ce tutoriel, nous allons parler plus de tests automatisés et ajouter des tests unitaires supplémentaires qui couvrent la fonctionnalité d’application que nous mettons en œuvre.</span><span class="sxs-lookup"><span data-stu-id="2cbff-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="2cbff-161">étape suivante</span><span class="sxs-lookup"><span data-stu-id="2cbff-161">Next Step</span></span>

<span data-ttu-id="2cbff-162">Nous avons maintenant une structure d’application de base en place.</span><span class="sxs-lookup"><span data-stu-id="2cbff-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="2cbff-163">Créons maintenant [une base de données pour stocker nos données d’application](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="2cbff-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2cbff-164">[Suivant précédent](introducing-the-nerddinner-tutorial.md)
> [Next](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="2cbff-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
