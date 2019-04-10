---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Ajout de la Validation au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 3db6947f36eb51b41d929f8c7d8835a95db8ea75
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392350"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="c08cf-104">Ajout de la validation au modèle</span><span class="sxs-lookup"><span data-stu-id="c08cf-104">Adding Validation to the Model</span></span>

<span data-ttu-id="c08cf-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c08cf-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c08cf-106">Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c08cf-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c08cf-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="c08cf-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c08cf-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="c08cf-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c08cf-109">Dans cette section, nous allons implémenter la prise en charge nécessaire pour activer la validation d’entrée au sein de notre application.</span><span class="sxs-lookup"><span data-stu-id="c08cf-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="c08cf-110">Nous allons nous assurer que notre contenu de la base de données est toujours correct et fournir des messages d’erreur utiles aux utilisateurs finaux lorsqu’ils essayent et entrer des données de film qui ne sont pas valides.</span><span class="sxs-lookup"><span data-stu-id="c08cf-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="c08cf-111">Nous allons commencer en ajoutant un peu de logique de validation à la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="c08cf-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="c08cf-112">Cliquez avec le bouton droit sur le dossier de modèle et sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="c08cf-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="c08cf-113">Nommez votre film de la classe.</span><span class="sxs-lookup"><span data-stu-id="c08cf-113">Name your class Movie.</span></span>

<span data-ttu-id="c08cf-114">Lorsque nous avons créé le modèle d’entité film précédemment, l’IDE créé une classe de film.</span><span class="sxs-lookup"><span data-stu-id="c08cf-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="c08cf-115">En fait, la partie de la classe de film peut être dans un fichier et partie dans un autre.</span><span class="sxs-lookup"><span data-stu-id="c08cf-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="c08cf-116">Il s’agit d’une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="c08cf-116">This is called a Partial Class.</span></span> <span data-ttu-id="c08cf-117">Nous allons étendre la classe Movie à partir d’un autre fichier.</span><span class="sxs-lookup"><span data-stu-id="c08cf-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="c08cf-118">Nous allons créer une classe partielle de film qui pointe vers une classe « buddy » avec certains attributs qui donnent des indicateurs de validation pour le système.</span><span class="sxs-lookup"><span data-stu-id="c08cf-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="c08cf-119">Nous allons marquer le titre et le prix comme requis et également insistez pour que le prix est dans une certaine plage.</span><span class="sxs-lookup"><span data-stu-id="c08cf-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="c08cf-120">Cliquez avec le bouton droit sur le dossier Modèles et sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="c08cf-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="c08cf-121">Nommer votre classe de film et cliquez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="c08cf-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="c08cf-122">Voici à quoi notre partielle film classe ressemble.</span><span class="sxs-lookup"><span data-stu-id="c08cf-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="c08cf-123">Exécutez de nouveau votre application et essayez d’entrer un film avec un prix supérieures à 100.</span><span class="sxs-lookup"><span data-stu-id="c08cf-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="c08cf-124">Vous obtiendrez une erreur lorsque vous avez envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="c08cf-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="c08cf-125">L’erreur est détectée sur le côté serveur et se produit après que le formulaire est publié.</span><span class="sxs-lookup"><span data-stu-id="c08cf-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="c08cf-126">Notez comment les HTML helpers intégrés d’ASP.NET MVC ont été suffisamment intelligents pour afficher le message d’erreur et conservez les valeurs pour nous dans les éléments de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="c08cf-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

[![C<span data-ttu-id="c08cf-127">reateMovieWithValidation]</span><span class="sxs-lookup"><span data-stu-id="c08cf-127">reateMovieWithValidation]</span></span>(getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

<span data-ttu-id="c08cf-128">Cela fonctionne très bien, mais il serait intéressant que nous pourrions l’utilisateur sur le côté client, immédiatement, avant que le serveur obtient impliqué.</span><span class="sxs-lookup"><span data-stu-id="c08cf-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="c08cf-129">Nous allons activer une validation côté client avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c08cf-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="c08cf-130">Ajout d’une Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="c08cf-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="c08cf-131">Étant donné que notre classe Movie a déjà certains attributs de validation, nous devons simplement ajouter quelques fichiers JavaScript à notre modèle de vue de Create.aspx et ajoutez une ligne de code pour activer la validation côté client se déroulent.</span><span class="sxs-lookup"><span data-stu-id="c08cf-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="c08cf-132">Depuis VWD atteindre notre dossier vues/film et ouvrez Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="c08cf-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="c08cf-133">Ouvrez le dossier de Scripts dans l’Explorateur de solutions et faites glisser les trois scripts suivants pour dans le &lt;head&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="c08cf-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="c08cf-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="c08cf-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="c08cf-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="c08cf-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="c08cf-136">Vous souhaitez que ces fichiers de script pour apparaître dans cet ordre.</span><span class="sxs-lookup"><span data-stu-id="c08cf-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="c08cf-137">En outre, ajoutez la ligne unique ci-dessus le Html.BeginForm :</span><span class="sxs-lookup"><span data-stu-id="c08cf-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="c08cf-138">Voici le code indiqué dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="c08cf-138">Here's the code shown within the IDE.</span></span>

[![M<span data-ttu-id="c08cf-139">ovies - Microsoft Visual Web Developer 2010 Express (10)]</span><span class="sxs-lookup"><span data-stu-id="c08cf-139">ovies - Microsoft Visual Web Developer 2010 Express (10)]</span></span>(getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

<span data-ttu-id="c08cf-140">Exécutez votre application et consultez à nouveau /Movies/Create et cliquez sur créer sans entrer de données.</span><span class="sxs-lookup"><span data-stu-id="c08cf-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="c08cf-141">Les messages d’erreur apparaissent immédiatement sans la page flash que nous associons à l’envoi des données jusqu’au serveur.</span><span class="sxs-lookup"><span data-stu-id="c08cf-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="c08cf-142">C’est parce que ASP.NET MVC est désormais la validation l’entrée à la fois sur le client (à l’aide de JavaScript) et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c08cf-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

[![C<span data-ttu-id="c08cf-143">Windows Internet Explorer - reate]</span><span class="sxs-lookup"><span data-stu-id="c08cf-143">reate - Windows Internet Explorer]</span></span>(getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

<span data-ttu-id="c08cf-144">Cela est en ordre.</span><span class="sxs-lookup"><span data-stu-id="c08cf-144">This is looking good!</span></span> <span data-ttu-id="c08cf-145">Nous allons maintenant ajouter une colonne supplémentaire à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c08cf-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c08cf-146">[Précédent](getting-started-with-mvc-part6.md)
> [Suivant](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="c08cf-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
