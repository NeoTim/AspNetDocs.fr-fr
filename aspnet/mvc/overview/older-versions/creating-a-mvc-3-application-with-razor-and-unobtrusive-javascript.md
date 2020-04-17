---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’une application MVC 3 avec Razor et JavaScript discret (fr) Microsoft Docs
author: rick-anderson
description: L’exemple d’application Web de la liste d’utilisateurs démontre à quel point il est simple de créer ASP.NET applications MVC 3 à l’aide du moteur Razor View. L’exemple d’application s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: c57e19f8eeca15e3676b3d490b08f69786d44f93
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542454"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="b9dd6-104">Création d’une application MVC 3 Application avec Razor et JavaScript non obstrusif</span><span class="sxs-lookup"><span data-stu-id="b9dd6-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="b9dd6-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b9dd6-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b9dd6-106">L’exemple d’application Web de la liste d’utilisateurs démontre à quel point il est simple de créer ASP.NET applications MVC 3 à l’aide du moteur Razor View.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="b9dd6-107">L’application d’échantillon montre comment utiliser le nouveau moteur razor view avec ASP.NET version 3 et Visual Studio 2010 pour créer un site Web fictif de la liste des utilisateurs qui comprend des fonctionnalités telles que la création, l’affichage, l’édition et la suppression des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="b9dd6-108">Ce tutoriel décrit les mesures qui ont été prises afin de construire l’échantillon de la liste d’utilisateurs ASP.NET application MVC 3.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="b9dd6-109">Un projet Visual Studio avec le code source C et VB est disponible pour accompagner ce sujet : [Télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="b9dd6-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="b9dd6-110">Si vous avez des questions sur ce tutoriel, s’il vous plaît les poster sur le [forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dd6-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="b9dd6-111">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="b9dd6-111">Overview</span></span>

<span data-ttu-id="b9dd6-112">L’application que vous allez construire est un site Web simple liste d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="b9dd6-113">Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-113">Users can enter, view, and update user information.</span></span>

![Site d’échantillon](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="b9dd6-115">Vous pouvez télécharger le projet VB et C ' terminé [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="b9dd6-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="b9dd6-116">Création de l’application Web</span><span class="sxs-lookup"><span data-stu-id="b9dd6-116">Creating the Web Application</span></span>

<span data-ttu-id="b9dd6-117">Pour commencer le tutoriel, ouvrez Visual Studio 2010 et créez un nouveau projet à l’aide du modèle *d’application Web MVC 3 ASP.NET.*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="b9dd6-118">Nommez &quot;l’application Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="b9dd6-119">[![Nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="b9dd6-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="b9dd6-120">Dans le **new ASP.NET MVC 3 Project** dialogue, sélectionnez application **Internet**, sélectionnez le moteur Razor view, puis cliquez **sur OK**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Nouveau ASP.NET dialogue du projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="b9dd6-122">Dans ce tutoriel, vous n’utiliserez pas le fournisseur d’adhésion ASP.NET, de sorte que vous pouvez supprimer tous les fichiers associés au logon et à l’adhésion.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="b9dd6-123">Dans **Solution Explorer**, supprimez les fichiers et répertoires suivants :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="b9dd6-124">*Contrôleurs-AccountController*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="b9dd6-125">*Modèles-AccountModels*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="b9dd6-126">*Vues-_LogOnPartial\\partagées*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="b9dd6-127">*Vues-Compte* (et tous les fichiers dans cet annuaire)</span><span class="sxs-lookup"><span data-stu-id="b9dd6-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="b9dd6-129">Modifier `<div>` le `logindisplay` <em>&quot;</em>&quot; <em> \_fichier Layout.cshtml</em> et remplacer la balisage à l’intérieur de l’élément nommé par le message Login Disabled .</span><span class="sxs-lookup"><span data-stu-id="b9dd6-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="b9dd6-130">L’exemple suivant montre la nouvelle balisage :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="b9dd6-131">Ajout du modèle</span><span class="sxs-lookup"><span data-stu-id="b9dd6-131">Adding the Model</span></span>

<span data-ttu-id="b9dd6-132">Dans **Solution Explorer**, cliquez à droite sur le dossier *Modèles,* sélectionnez **Ajouter,** puis cliquez sur **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nouvelle classe d’utilisateur Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="b9dd6-134">Nommez la classe `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-134">Name the class `UserModel`.</span></span> <span data-ttu-id="b9dd6-135">Remplacer le contenu du fichier *UserModel* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="b9dd6-136">La `UserModel` classe représente les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="b9dd6-137">Chaque membre de la classe est annoté avec [l’attribut requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) à partir de l’espace de nom [DataAnnotations.](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)</span><span class="sxs-lookup"><span data-stu-id="b9dd6-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="b9dd6-138">Les attributs de l’espace de nom [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournissent une validation automatique côté client et serveur pour les applications Web.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="b9dd6-139">Ouvrez `HomeController` la classe `using` et ajoutez une `UserModel` directive `Users` afin que vous puissiez accéder à la et aux classes :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="b9dd6-140">Juste après `HomeController` la déclaration, ajoutez le commentaire `Users` suivant et la référence à une classe :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="b9dd6-141">La `Users` classe est un magasin de données simplifié et en mémoire que vous utiliserez dans ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="b9dd6-142">Dans une application réelle, vous utiliseriez une base de données pour stocker les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="b9dd6-143">Les premières lignes `HomeController` du fichier sont affichées dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="b9dd6-144">Construisez l’application de sorte que le modèle d’utilisateur sera disponible pour l’assistant d’échafaudage dans la prochaine étape.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="b9dd6-145">Création de la vue par défaut</span><span class="sxs-lookup"><span data-stu-id="b9dd6-145">Creating the Default View</span></span>

<span data-ttu-id="b9dd6-146">L’étape suivante consiste à ajouter une méthode d’action et de visualiser pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="b9dd6-147">Supprimer le fichier *d’index Views-Home.*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="b9dd6-148">Vous créerez un nouveau fichier *Index* pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="b9dd6-149">Dans `HomeController` la classe, remplacez `Index` le contenu de la méthode par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="b9dd6-150">Cliquez à droite `Index` à l’intérieur de la méthode, puis cliquez sur **Ajouter Voir**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Ajouter la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="b9dd6-152">Sélectionnez **l’option Créer une vue fortement typée.**</span><span class="sxs-lookup"><span data-stu-id="b9dd6-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="b9dd6-153">Pour **la classe de données View**, sélectionnez **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="b9dd6-154">(Si vous ne voyez pas **Mvc3Razor.Models.UserModel** dans la boîte **de classe de données View,** vous devez construire le projet.) Assurez-vous que le moteur de vue est réglé sur **Razor**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="b9dd6-155">Définir **le contenu** de la **liste,** puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-155">Set **View content** to **List** and then click **Add**.</span></span>

![Ajouter la vue de l’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="b9dd6-157">La nouvelle vue échafaude automatiquement les données `Index` utilisateur qui sont transmises à la vue.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="b9dd6-158">Examinez le fichier *Views-Home Index* nouvellement généré.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="b9dd6-159">Les **liens Créer de nouveaux,** **modifier,** **détails**et **supprimer** ne fonctionnent pas, mais le reste de la page est fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="b9dd6-160">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-160">Run the page.</span></span> <span data-ttu-id="b9dd6-161">Vous voyez une liste d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-161">You see a list of users.</span></span>

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="b9dd6-163">Ouvrez le fichier *Index.cshtml* et remplacez `ActionLink` la balisage pour **Modifier**, **Détails**, et **Supprimer** avec le code suivant:</span><span class="sxs-lookup"><span data-stu-id="b9dd6-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="b9dd6-164">Le nom d’utilisateur est utilisé comme ID pour trouver l’enregistrement sélectionné dans les liens **Modifier**, **Détails**et **Supprimer.**</span><span class="sxs-lookup"><span data-stu-id="b9dd6-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="b9dd6-165">Création de la vue des détails</span><span class="sxs-lookup"><span data-stu-id="b9dd6-165">Creating the Details View</span></span>

<span data-ttu-id="b9dd6-166">L’étape suivante consiste `Details` à ajouter une méthode d’action et une vue afin d’afficher les détails de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="b9dd6-168">Ajoutez la `Details` méthode suivante au contrôleur à domicile :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="b9dd6-169">Cliquez à droite `Details` à l’intérieur de la méthode, puis sélectionnez <strong>Add View</strong>.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="b9dd6-170">Vérifiez que la boîte <strong>de classe de données View</strong> contient <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="b9dd6-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="b9dd6-171">Définissez le contenu de <strong>l’affiche</strong> <strong>sur les détails,</strong> puis cliquez sur <strong>Ajouter</strong>.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Ajouter la vue des détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="b9dd6-173">Exécutez l’application et sélectionnez un lien de détails.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-173">Run the application and select a details link.</span></span> <span data-ttu-id="b9dd6-174">L’échafaudage automatique montre chaque propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-174">The automatic scaffolding shows each property in the model.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="b9dd6-176">Création de la vue d’édition</span><span class="sxs-lookup"><span data-stu-id="b9dd6-176">Creating the Edit View</span></span>

<span data-ttu-id="b9dd6-177">Ajoutez la `Edit` méthode suivante au contrôleur à domicile.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="b9dd6-178">Ajoutez une vue comme dans les étapes précédentes, mais définissez **le contenu De vue** pour **modifier**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Ajouter la vue Edit](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="b9dd6-180">Exécutez l’application et modifiez le prénom et le nom de famille de l’un des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="b9dd6-181">Si vous `DataAnnotation` violez les contraintes `UserModel` qui ont été appliquées à la classe, lorsque vous soumettez le formulaire, vous verrez des erreurs de validation qui sont produites par le code serveur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="b9dd6-182">Par exemple, si vous &quot;changez le prénom Ann&quot; en &quot;A&quot;, lorsque vous soumettez le formulaire, l’erreur suivante s’affiche sur le formulaire :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="b9dd6-183">Dans ce tutoriel, vous traitez le nom d’utilisateur comme la clé principale.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="b9dd6-184">Par conséquent, la propriété de nom d’utilisateur ne peut pas être changée.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="b9dd6-185">Dans le fichier *Edit.cshtml,* juste après la `Html.BeginForm` déclaration, définissez le nom d’utilisateur comme un champ caché.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="b9dd6-186">Cela provoque la propriété à passer dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="b9dd6-187">Le fragment de code suivant `Hidden` montre le placement de la déclaration :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="b9dd6-188">Remplacez `TextBoxFor` `ValidationMessageFor` le nom d’utilisateur `DisplayFor` et le balisage par un appel.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="b9dd6-189">La `DisplayFor` méthode affiche la propriété comme un élément de lecture seulement.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="b9dd6-190">L'exemple suivant montre le balisage complet.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-190">The following example shows the completed markup.</span></span> <span data-ttu-id="b9dd6-191">`TextBoxFor` L’original `ValidationMessageFor` et les appels sont commentés avec le`@* *@`Razor commence-commentaire et les personnages de fin de commentaire ( )</span><span class="sxs-lookup"><span data-stu-id="b9dd6-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="b9dd6-192">Permettre la validation côté client</span><span class="sxs-lookup"><span data-stu-id="b9dd6-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="b9dd6-193">Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux drapeaux et inclure trois fichiers JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="b9dd6-194">Ouvrez le fichier *Web.config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="b9dd6-195">Vérifier `that ClientValidationEnabled` `UnobtrusiveJavaScriptEnabled` et sont configurés dans les paramètres de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="b9dd6-196">Le fragment suivant à partir du fichier *Web.config* racine affiche les paramètres corrects :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="b9dd6-197">Le `UnobtrusiveJavaScriptEnabled` réglage à vrai permet l’Ajax discret et la validation discrète du client.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="b9dd6-198">Lorsque vous utilisez une validation discrète, les règles de validation sont transformées en attributs HTML5.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="b9dd6-199">Les noms d’attributs HTML5 ne peuvent se composer que de lettres, de chiffres et de tirets minuscules.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="b9dd6-200">Le `ClientValidationEnabled` réglage à vrai permet la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="b9dd6-201">En définissant ces clés dans le fichier *Web.config* d’application, vous activez la validation du client et JavaScript discret pour l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="b9dd6-202">Vous pouvez également activer ou désactiver ces paramètres dans des vues individuelles ou dans les méthodes de contrôleur à l’aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="b9dd6-203">Vous devez également inclure plusieurs fichiers JavaScript dans la vue rendue.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="b9dd6-204">Un moyen facile d’inclure le JavaScript dans toutes les vues est de les ajouter au fichier *Views-Shared\\_Layout.cshtml.*</span><span class="sxs-lookup"><span data-stu-id="b9dd6-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="b9dd6-205">Remplacer `<head>` l’élément du fichier \* \_Layout.cshtml\* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="b9dd6-206">Les deux premiers scripts jQuery sont hébergés par le Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="b9dd6-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="b9dd6-207">En profitant du CDN Microsoft Ajax, vous pouvez améliorer considérablement les performances de vos applications.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="b9dd6-208">Exécutez l’application et cliquez sur un lien de modification.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-208">Run the application and click an edit link.</span></span> <span data-ttu-id="b9dd6-209">Afficher la source de la page dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-209">View the page's source in the browser.</span></span> <span data-ttu-id="b9dd6-210">La source du navigateur affiche `data-val` de nombreux attributs du formulaire (pour la validation des données).</span><span class="sxs-lookup"><span data-stu-id="b9dd6-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="b9dd6-211">Lorsque la validation du client et JavaScript discret sont activés, les `data-val="true"` champs d’entrée avec une règle de validation client contiennent l’attribut pour déclencher une validation discrète du client.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="b9dd6-212">Par exemple, `City` le champ du modèle a été décoré avec [l’attribut requis,](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) ce qui donne le HTML indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="b9dd6-213">Pour chaque règle de validation du client, `data-val-rulename="message"`un attribut est ajouté qui a le formulaire .</span><span class="sxs-lookup"><span data-stu-id="b9dd6-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="b9dd6-214">À `City` l’aide de l’exemple de terrain `data-val-required` montré plus &quot;tôt, la&quot;règle de validation requise du client génère l’attribut et le message Le champ de la Ville est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="b9dd6-215">Exécutez l’application, modifiez l’un des utilisateurs et dégagez le `City` champ.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="b9dd6-216">Lorsque vous sortez du champ, vous voyez un message d’erreur de validation côté client.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ville requise](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="b9dd6-218">De même, pour chaque paramètre de la règle de `data-val-rulename-paramname=paramvalue`validation du client, un attribut est ajouté qui a le formulaire .</span><span class="sxs-lookup"><span data-stu-id="b9dd6-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="b9dd6-219">Par exemple, `FirstName` la propriété est annotée avec l’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) et spécifie une longueur minimale de 3 et une longueur maximale de 8.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="b9dd6-220">La règle de `length` validation des `max` données nommée a le nom du paramètre et la valeur du paramètre 8.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="b9dd6-221">Ce qui suit affiche le `FirstName` HTML qui est généré pour le champ lorsque vous modifiez l’un des utilisateurs:</span><span class="sxs-lookup"><span data-stu-id="b9dd6-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="b9dd6-222">Pour plus d’informations sur la validation discrète du client, voir l’entrée [Nonobtrusive Client Validation dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="b9dd6-223">Dans ASP.NET MVC 3 Beta, vous devez parfois soumettre le formulaire afin de commencer la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="b9dd6-224">Cela pourrait être modifié pour la version finale.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="b9dd6-225">Création de la vue de création</span><span class="sxs-lookup"><span data-stu-id="b9dd6-225">Creating the Create View</span></span>

<span data-ttu-id="b9dd6-226">L’étape suivante consiste `Create` à ajouter une méthode d’action et une vue afin de permettre à l’utilisateur de créer un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="b9dd6-227">Ajoutez la `Create` méthode suivante au contrôleur à domicile :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="b9dd6-228">Ajoutez une vue comme dans les étapes précédentes, mais définissez **le contenu De vue** pour **créer**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="b9dd6-230">Exécutez l’application, sélectionnez le lien **Créer** et ajoutez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="b9dd6-231">La `Create` méthode tire automatiquement parti de la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="b9dd6-232">Essayez d’entrer un nom d’utilisateur &quot;qui&quot;contient de l’espace blanc, comme Ben X .</span><span class="sxs-lookup"><span data-stu-id="b9dd6-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="b9dd6-233">Lorsque vous sortez du champ de nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="b9dd6-234">Ajouter la méthode Supprimer</span><span class="sxs-lookup"><span data-stu-id="b9dd6-234">Add the Delete method</span></span>

<span data-ttu-id="b9dd6-235">Pour compléter le tutoriel, `Delete` ajoutez la méthode suivante au contrôleur à domicile :</span><span class="sxs-lookup"><span data-stu-id="b9dd6-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="b9dd6-236">Ajoutez `Delete` une vue comme dans les étapes précédentes, en définissant **le contenu Afficher** à **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="b9dd6-238">Vous disposez désormais d’une application MVC 3 simple mais entièrement ASP.NET fonctionnelle avec validation.</span><span class="sxs-lookup"><span data-stu-id="b9dd6-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
