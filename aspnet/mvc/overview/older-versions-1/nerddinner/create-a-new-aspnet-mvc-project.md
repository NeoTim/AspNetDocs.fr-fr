---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet ASP.NET MVC | Microsoft Docs
author: microsoft
description: Étape 1 vous montre comment mettre en place de la structure d’application NerdDinner base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 3f34f17aa35dbfed2d52daf615c8dc81be6e7847
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065806"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="85958-103">Créer un projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="85958-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="85958-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="85958-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="85958-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="85958-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="85958-106">Il s’agit d’étape 1 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="85958-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="85958-107">Étape 1 vous montre comment mettre en place de la structure d’application NerdDinner base.</span><span class="sxs-lookup"><span data-stu-id="85958-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="85958-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85958-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="85958-109">NerdDinner étape 1 : Fichier -&gt;nouveau projet</span><span class="sxs-lookup"><span data-stu-id="85958-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="85958-110">Nous allons commencer notre application NerdDinner en sélectionnant le **fichier -&gt;nouveau projet** élément de menu dans Visual Studio 2008 ou sur le gratuit Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="85958-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="85958-111">Cela fera apparaître la boîte de dialogue « Nouveau projet ».</span><span class="sxs-lookup"><span data-stu-id="85958-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="85958-112">Pour créer une application ASP.NET MVC, nous allons sélectionnez le nœud « Web » sur le côté gauche de la boîte de dialogue, puis choisissez le modèle de projet « Application Web ASP.NET MVC » sur la droite :</span><span class="sxs-lookup"><span data-stu-id="85958-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="85958-113">*Important : Assurez-vous que vous avez téléchargé et installé ASP.NET MVC - sinon il n’apparaît pas dans la boîte de dialogue Nouveau projet. Vous pouvez utiliser la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) si elle ne l’avez pas encore installé (ASP.NET MVC est disponible dans la « plate-forme Web -&gt;infrastructures et les Runtimes » section).*</span><span class="sxs-lookup"><span data-stu-id="85958-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="85958-114">Nous allons nommer le nouveau projet, que nous allons créer « NerdDinner », puis sur le bouton « OK » pour la créer.</span><span class="sxs-lookup"><span data-stu-id="85958-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="85958-115">Lorsque nous cliquons sur « OK » Visual Studio fait apparaître une boîte de dialogue supplémentaire qui nous demande créer éventuellement un projet de test unitaire pour la nouvelle application également.</span><span class="sxs-lookup"><span data-stu-id="85958-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="85958-116">Ce projet de test unitaire permet de créer des tests automatisés qui vérifient les fonctionnalités et le comportement de notre application (quelque chose que nous allons aborder la tâche plus loin dans ce didacticiel).</span><span class="sxs-lookup"><span data-stu-id="85958-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="85958-117">La liste déroulante « Test framework » dans la boîte de dialogue ci-dessus est remplie avec tous les disponible ASP.NET MVC unit test modèles de projet installés sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="85958-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="85958-118">Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit.</span><span class="sxs-lookup"><span data-stu-id="85958-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="85958-119">L’infrastructure de Test unitaire Visual Studio intégrée est également pris en charge.</span><span class="sxs-lookup"><span data-stu-id="85958-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="85958-120">*Remarque : L’infrastructure des tests unitaires Visual Studio est disponible uniquement avec Visual Studio 2008 Professional et versions ultérieures. Si vous utilisez Visual Studio 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devrez télécharger et installer les extensions de NUnit, MBUnit ou XUnit pour ASP.NET MVC pour cette boîte de dialogue à afficher. La boîte de dialogue n’affiche pas si il ne sont pas des infrastructures de test installés.*</span><span class="sxs-lookup"><span data-stu-id="85958-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="85958-121">Nous allons utiliser le nom de « NerdDinner.Tests » par défaut pour le projet de test que nous créons et utiliser l’option de framework « Visual Studio Test unitaire ».</span><span class="sxs-lookup"><span data-stu-id="85958-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="85958-122">Lorsque nous cliquons sur le bouton « OK », Visual Studio crée une solution pour nous avec deux projets à l’intérieur : un pour notre application web et l’autre pour nos tests unitaires :</span><span class="sxs-lookup"><span data-stu-id="85958-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="85958-123">Étude de la structure de répertoire de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85958-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="85958-124">Lorsque vous créez une application ASP.NET MVC avec Visual Studio, il ajoute automatiquement un nombre de répertoires et fichiers au projet :</span><span class="sxs-lookup"><span data-stu-id="85958-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="85958-125">Les projets ASP.NET MVC par défaut ont six répertoires de niveau supérieur :</span><span class="sxs-lookup"><span data-stu-id="85958-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="85958-126">**Répertoire**</span><span class="sxs-lookup"><span data-stu-id="85958-126">**Directory**</span></span> | <span data-ttu-id="85958-127">**Fonction**</span><span class="sxs-lookup"><span data-stu-id="85958-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="85958-128">**/ Contrôleurs**</span><span class="sxs-lookup"><span data-stu-id="85958-128">**/Controllers**</span></span> | <span data-ttu-id="85958-129">Emplacement où vous placez les classes de contrôleur qui gèrent les demandes d’URL</span><span class="sxs-lookup"><span data-stu-id="85958-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="85958-130">**/ Modèles**</span><span class="sxs-lookup"><span data-stu-id="85958-130">**/Models**</span></span> | <span data-ttu-id="85958-131">Emplacement où vous placez des classes qui représentent et manipulent des données</span><span class="sxs-lookup"><span data-stu-id="85958-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="85958-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="85958-132">**/Views**</span></span> | <span data-ttu-id="85958-133">Emplacement où vous placez les fichiers de modèle de l’interface utilisateur qui sont responsables de la sortie de rendu</span><span class="sxs-lookup"><span data-stu-id="85958-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="85958-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="85958-134">**/Scripts**</span></span> | <span data-ttu-id="85958-135">Emplacement où vous placez les fichiers de bibliothèque JavaScript et les scripts (.js)</span><span class="sxs-lookup"><span data-stu-id="85958-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="85958-136">**/ Contenu**</span><span class="sxs-lookup"><span data-stu-id="85958-136">**/Content**</span></span> | <span data-ttu-id="85958-137">Emplacement où vous placez le code CSS et les fichiers image et les autres contenus non-dynamique/non-JavaScript</span><span class="sxs-lookup"><span data-stu-id="85958-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="85958-138">**/ Application\_données**</span><span class="sxs-lookup"><span data-stu-id="85958-138">**/App\_Data**</span></span> | <span data-ttu-id="85958-139">Lorsque vous stockez des fichiers de données voulez-vous en lecture/écriture.</span><span class="sxs-lookup"><span data-stu-id="85958-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="85958-140">ASP.NET MVC ne nécessite pas de cette structure.</span><span class="sxs-lookup"><span data-stu-id="85958-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="85958-141">En fait, les développeurs qui travaillent sur des applications volumineuses généralement partitionne l’application de plusieurs projets pour le rendre plus facile à gérer (par exemple : les classes de modèles de données vont souvent dans un projet de bibliothèque de classes distinct à partir de l’application web).</span><span class="sxs-lookup"><span data-stu-id="85958-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="85958-142">Toutefois, la structure de projet par défaut, offre une convention de répertoire par défaut intéressant que nous pouvons utiliser pour nettoyer nos préoccupations de l’application.</span><span class="sxs-lookup"><span data-stu-id="85958-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="85958-143">Lorsque nous développons le répertoire /Controllers, vous trouverez que Visual Studio a ajouté deux classes de contrôleur – HomeController et AccountController – par défaut pour le projet :</span><span class="sxs-lookup"><span data-stu-id="85958-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="85958-144">Lorsque nous développez le répertoire /Views, nous trouverez trois sous-répertoires – /Home, /Account et /Shared – ainsi que modèle plusieurs fichiers au sein de celles-ci ont été également ajoutés au projet par défaut :</span><span class="sxs-lookup"><span data-stu-id="85958-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="85958-145">Lorsque nous étendons la /Content ; et les répertoires du dossier /Scripts, vous trouverez un fichier Site.css qui est utilisé pour définir le style HTML sur le site, ainsi que les bibliothèques JavaScript qui peuvent activer ASP.NET AJAX et jQuery prennent en charge au sein de l’application :</span><span class="sxs-lookup"><span data-stu-id="85958-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="85958-146">Lorsque nous développons le projet NerdDinner.Tests, vous trouverez deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="85958-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="85958-147">Ces fichiers par défaut ajoutés par Visual Studio nous fournissent une structure de base pour une application opérationnelle - avec la page d’accueil, sur la page, les pages de connexion/déconnexion / d’inscription de compte et une page d’erreur non gérée (tous les accès câblé et du fonctionnement prêts à l’emploi).</span><span class="sxs-lookup"><span data-stu-id="85958-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="85958-148">Exécution de l’Application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85958-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="85958-149">Nous pouvons exécuter le projet en sélectionnant le **Debug -&gt;démarrer le débogage** ou **Debug -&gt;démarrer sans débogage** des éléments de menu :</span><span class="sxs-lookup"><span data-stu-id="85958-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="85958-150">Cette opération lance le serveur Web ASP.NET intégré-qui est fourni avec Visual Studio et exécuter notre application :</span><span class="sxs-lookup"><span data-stu-id="85958-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="85958-151">Voici la page d’accueil pour notre nouveau projet (URL : « / ») lorsqu’il s’exécute :</span><span class="sxs-lookup"><span data-stu-id="85958-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="85958-152">Cliquez sur l’onglet « About » pour afficher un sur la page (URL : « / Home/About ») :</span><span class="sxs-lookup"><span data-stu-id="85958-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="85958-153">Cliquant sur le lien « Ouverture de session » dans l’angle supérieur droit de nous mène à une page de connexion (URL : « / compte / d’ouverture de session »)</span><span class="sxs-lookup"><span data-stu-id="85958-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="85958-154">Si nous n’avons pas un compte de connexion que nous pouvons cliquer sur le lien s’inscrire (URL : « / compte/Register ») pour en créer un :</span><span class="sxs-lookup"><span data-stu-id="85958-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="85958-155">Le code permettant d’implémenter la maison ci-dessus, à propos et déconnexion / inscrire fonctionnalité a été ajoutée par défaut lorsque nous avons créé notre nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="85958-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="85958-156">Nous allons l’utiliser comme point de départ de notre application.</span><span class="sxs-lookup"><span data-stu-id="85958-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="85958-157">Test de l’Application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85958-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="85958-158">Si nous utilisons le Professional Edition ou une version plus récente de Visual Studio 2008, nous pouvons utiliser la prise en charge de l’IDE Visual Studio du test des unités pour le projet de test :</span><span class="sxs-lookup"><span data-stu-id="85958-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="85958-159">En choisissant une des options ci-dessus pour ouvrir le volet « Résultats de Test » dans l’IDE et faites-nous part de l’état de réussite/échec sur les tests 27 unitaires inclus dans notre nouveau projet qui couvrent la fonctionnalité intégrée :</span><span class="sxs-lookup"><span data-stu-id="85958-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="85958-160">Plus loin dans ce didacticiel nous en dirons plus sur les tests automatisés et ajouter des tests unitaires supplémentaires qui couvrent les fonctionnalités de l’application que nous implémentons.</span><span class="sxs-lookup"><span data-stu-id="85958-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="85958-161">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="85958-161">Next Step</span></span>

<span data-ttu-id="85958-162">Nous avons à présent une structure d’application de base en place.</span><span class="sxs-lookup"><span data-stu-id="85958-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="85958-163">Nous allons maintenant [créer une base de données pour stocker les données de notre application](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="85958-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85958-164">[Précédent](introducing-the-nerddinner-tutorial.md)
> [Suivant](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="85958-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
