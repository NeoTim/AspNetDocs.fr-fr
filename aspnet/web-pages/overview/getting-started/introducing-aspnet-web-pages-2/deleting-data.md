---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Présentation de la pages Web ASP.NET-suppression de données de base de données | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment supprimer une entrée de base de données individuelle. Il part du principe que vous avez terminé la série pour mettre à jour les données de la base de données dans ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629037"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="e9567-104">Présentation de la pages Web ASP.NET-suppression de données de base de données</span><span class="sxs-lookup"><span data-stu-id="e9567-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="e9567-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e9567-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e9567-106">Ce didacticiel vous montre comment supprimer une entrée de base de données individuelle.</span><span class="sxs-lookup"><span data-stu-id="e9567-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="e9567-107">Il part du principe que vous avez terminé la série via la [mise à jour des données de la base de données dans pages Web ASP.net](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="e9567-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="e9567-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="e9567-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e9567-109">Comment sélectionner un enregistrement individuel à partir d’une liste d’enregistrements.</span><span class="sxs-lookup"><span data-stu-id="e9567-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="e9567-110">Comment supprimer un enregistrement unique d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="e9567-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="e9567-111">Comment vérifier qu’un clic a été effectué sur un bouton spécifique dans un formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9567-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="e9567-112">Fonctionnalités/technologies présentées :</span><span class="sxs-lookup"><span data-stu-id="e9567-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="e9567-113">Le programme d’assistance `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="e9567-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="e9567-114">Commande SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e9567-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="e9567-115">La méthode `Database.Execute` pour exécuter une commande SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e9567-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="e9567-116">Contenu</span><span class="sxs-lookup"><span data-stu-id="e9567-116">What You'll Build</span></span>

<span data-ttu-id="e9567-117">Dans le didacticiel précédent, vous avez appris à mettre à jour un enregistrement de base de données existant.</span><span class="sxs-lookup"><span data-stu-id="e9567-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="e9567-118">Ce didacticiel est similaire, à ceci près qu’au lieu de mettre à jour l’enregistrement, vous allez le supprimer.</span><span class="sxs-lookup"><span data-stu-id="e9567-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="e9567-119">Les processus sont pratiquement les mêmes, mais la suppression est plus simple. ce didacticiel est donc concis.</span><span class="sxs-lookup"><span data-stu-id="e9567-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="e9567-120">Dans la page *films* , vous allez mettre à jour le `WebGrid` Helper afin qu’il affiche un lien **supprimer** en regard de chaque film pour accompagner le lien de **modification** que vous avez ajouté précédemment.</span><span class="sxs-lookup"><span data-stu-id="e9567-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Page films présentant un lien supprimer pour chaque film](deleting-data/_static/image1.png)

<span data-ttu-id="e9567-122">Comme pour la modification, lorsque vous cliquez sur le lien **supprimer** , vous accédez à une autre page, où les informations sur le film se trouvent déjà dans un formulaire :</span><span class="sxs-lookup"><span data-stu-id="e9567-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Supprimer la page de film avec un film affiché](deleting-data/_static/image2.png)

<span data-ttu-id="e9567-124">Vous pouvez ensuite cliquer sur le bouton pour supprimer définitivement l’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e9567-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="e9567-125">Ajout d’un lien supprimer à la liste des films</span><span class="sxs-lookup"><span data-stu-id="e9567-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="e9567-126">Vous allez commencer par ajouter un lien **supprimer** à l’application auxiliaire `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="e9567-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="e9567-127">Ce lien est similaire au lien **modifier** que vous avez ajouté dans un didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="e9567-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="e9567-128">Ouvrez le fichier *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e9567-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="e9567-129">Modifiez le balisage `WebGrid` dans le corps de la page en ajoutant une colonne.</span><span class="sxs-lookup"><span data-stu-id="e9567-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="e9567-130">Voici le balisage modifié :</span><span class="sxs-lookup"><span data-stu-id="e9567-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="e9567-131">La nouvelle colonne est celle-ci :</span><span class="sxs-lookup"><span data-stu-id="e9567-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="e9567-132">La façon dont la grille est configurée, la colonne de **modification** est la plus à gauche dans la grille et la colonne de **suppression** est la plus à droite.</span><span class="sxs-lookup"><span data-stu-id="e9567-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="e9567-133">(Il y a une virgule après la `Year` colonne maintenant, au cas où vous ne l’auriez pas remarqué.) Il n’y a rien de spécial quant à l’emplacement de ces colonnes de lien, et vous pourriez les placer facilement les unes à côté des autres.</span><span class="sxs-lookup"><span data-stu-id="e9567-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="e9567-134">Dans ce cas, ils sont distincts pour compliquer la combinaison.</span><span class="sxs-lookup"><span data-stu-id="e9567-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Page films avec les liens de modification et détails marqués pour montrer qu’ils ne sont pas les uns à côté des autres](deleting-data/_static/image3.png)

<span data-ttu-id="e9567-136">La nouvelle colonne affiche un lien (élément`<a>`) dont le texte indique « Delete ».</span><span class="sxs-lookup"><span data-stu-id="e9567-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="e9567-137">La cible du lien (son attribut `href`) est du code qui se résout finalement en un type similaire à cette URL, avec la valeur `id` différente pour chaque film :</span><span class="sxs-lookup"><span data-stu-id="e9567-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="e9567-138">Ce lien appellera une page nommée *DeleteMovie* et lui transmet l’ID de la vidéo que vous avez sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="e9567-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="e9567-139">Ce didacticiel n’aborde pas en détail la façon dont ce lien est construit, car il est quasiment identique au lien **Edit** du didacticiel précédent ([mise à jour des données de la base de données dans pages Web ASP.net](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="e9567-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="e9567-140">Création de la page de suppression</span><span class="sxs-lookup"><span data-stu-id="e9567-140">Creating the Delete Page</span></span>

<span data-ttu-id="e9567-141">Vous pouvez maintenant créer la page qui sera la cible du lien **supprimer** dans la grille.</span><span class="sxs-lookup"><span data-stu-id="e9567-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e9567-142">**Important** La technique de la première sélection d’un enregistrement à supprimer, puis l’utilisation d’une page et d’un bouton distincts pour confirmer le processus est extrêmement importante pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="e9567-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="e9567-143">Comme vous avez lu *dans les* didacticiels précédents, la modification de votre site Web doit *toujours* être effectuée à l’aide d’un formulaire &mdash; à l’aide d’une opération http postale.</span><span class="sxs-lookup"><span data-stu-id="e9567-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="e9567-144">Si vous avez fait en sorte qu’il soit possible de modifier le site simplement en cliquant sur un lien (c’est-à-dire à l’aide d’une opération d’extraction), les utilisateurs peuvent effectuer des requêtes simples sur votre site et supprimer vos données.</span><span class="sxs-lookup"><span data-stu-id="e9567-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="e9567-145">Même un robot de moteur de recherche qui indexe votre site peut supprimer par inadvertance des données en suivant les liens ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e9567-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="e9567-146">Lorsque votre application permet aux utilisateurs de modifier un enregistrement, vous devez tout de même présenter l’enregistrement à l’utilisateur pour le modifier.</span><span class="sxs-lookup"><span data-stu-id="e9567-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="e9567-147">Mais vous pouvez être tenté d’ignorer cette étape pour supprimer un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e9567-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="e9567-148">N’ignorez pas cette étape.</span><span class="sxs-lookup"><span data-stu-id="e9567-148">Don't skip that step, though.</span></span> <span data-ttu-id="e9567-149">(Il est également utile pour les utilisateurs de voir l’enregistrement et de confirmer qu’ils suppriment l’enregistrement auquel ils sont destinés.)</span><span class="sxs-lookup"><span data-stu-id="e9567-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="e9567-150">Dans une série de didacticiels suivante, vous verrez comment ajouter une fonctionnalité de connexion afin qu’un utilisateur ait à se connecter avant de supprimer un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e9567-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="e9567-151">Créez une page nommée *DeleteMovie. cshtml* et remplacez ce qui se trouve dans le fichier par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="e9567-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="e9567-152">Ce balisage est similaire aux pages *EditMovie* , sauf qu’au lieu d’utiliser des zones de texte (`<input type="text">`), le balisage comprend `<span>` éléments.</span><span class="sxs-lookup"><span data-stu-id="e9567-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="e9567-153">Il n’y a rien à modifier.</span><span class="sxs-lookup"><span data-stu-id="e9567-153">There's nothing here to edit.</span></span> <span data-ttu-id="e9567-154">Il vous suffit d’afficher les détails du film pour que les utilisateurs puissent s’assurer qu’ils suppriment le film approprié.</span><span class="sxs-lookup"><span data-stu-id="e9567-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="e9567-155">Le balisage contient déjà un lien qui permet à l’utilisateur de revenir à la page de liste des films.</span><span class="sxs-lookup"><span data-stu-id="e9567-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="e9567-156">Comme dans la page *EditMovie* , l’ID du film sélectionné est stocké dans un champ masqué.</span><span class="sxs-lookup"><span data-stu-id="e9567-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="e9567-157">(Elle est transmise dans la page à la première place sous la forme d’une valeur de chaîne de requête.) Il y a un appel de `Html.ValidationSummary` qui affichera des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="e9567-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="e9567-158">Dans ce cas, l’erreur peut être qu’aucun ID de film n’a été transmis à la page ou que l’ID de film n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="e9567-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="e9567-159">Cette situation peut se produire si quelqu’un a exécuté cette page sans avoir au préalable sélectionné un film dans la page *films* .</span><span class="sxs-lookup"><span data-stu-id="e9567-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="e9567-160">La légende du bouton est **supprimer le film**et son attribut de nom est défini sur `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="e9567-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="e9567-161">L’attribut `name` sera utilisé dans le code pour identifier le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9567-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="e9567-162">Vous devrez écrire du code sur 1) lire les détails du film lorsque la page est affichée pour la première fois et 2) supprimer le film lorsque l’utilisateur clique sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="e9567-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="e9567-163">Ajout de code pour lire un seul film</span><span class="sxs-lookup"><span data-stu-id="e9567-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="e9567-164">En haut de la page *DeleteMovie. cshtml* , ajoutez le bloc de code suivant :</span><span class="sxs-lookup"><span data-stu-id="e9567-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="e9567-165">Ce balisage est identique au code correspondant dans la page *EditMovie* .</span><span class="sxs-lookup"><span data-stu-id="e9567-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="e9567-166">Elle récupère l’ID de film de la chaîne de requête et utilise l’ID pour lire un enregistrement de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9567-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="e9567-167">Le code comprend le test de validation (`IsInt()` et `row != null`) pour s’assurer que l’ID de film passé à la page est valide.</span><span class="sxs-lookup"><span data-stu-id="e9567-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="e9567-168">N’oubliez pas que ce code ne doit s’exécuter que lors de la première exécution de la page.</span><span class="sxs-lookup"><span data-stu-id="e9567-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="e9567-169">Vous ne souhaitez pas relire l’enregistrement de film de la base de données quand l’utilisateur clique sur le bouton **supprimer le film** .</span><span class="sxs-lookup"><span data-stu-id="e9567-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="e9567-170">Par conséquent, le code permettant de lire le film est à l’intérieur d’un test qui indique `if(!IsPost)` &mdash; autrement dit, *si la demande n’est pas une opération de publication (envoi de formulaire)* .</span><span class="sxs-lookup"><span data-stu-id="e9567-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="e9567-171">Ajout de code pour supprimer le film sélectionné</span><span class="sxs-lookup"><span data-stu-id="e9567-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="e9567-172">Pour supprimer le film lorsque l’utilisateur clique sur le bouton, ajoutez le code suivant juste à l’intérieur de l’accolade fermante du bloc `@` :</span><span class="sxs-lookup"><span data-stu-id="e9567-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="e9567-173">Ce code est similaire au code de mise à jour d’un enregistrement existant, mais plus simple.</span><span class="sxs-lookup"><span data-stu-id="e9567-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="e9567-174">Le code exécute une instruction SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e9567-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="e9567-175">Comme dans la page *EditMovie* , le code se trouve dans un bloc `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="e9567-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="e9567-176">Cette fois-ci, la condition de `if()` est un peu plus compliquée :</span><span class="sxs-lookup"><span data-stu-id="e9567-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="e9567-177">Deux conditions s’imposent ici.</span><span class="sxs-lookup"><span data-stu-id="e9567-177">There are two conditions here.</span></span> <span data-ttu-id="e9567-178">La première est que la page est envoyée, comme vous l’avez vu avant &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="e9567-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="e9567-179">La deuxième condition est `!Request["buttonDelete"].IsEmpty()`, ce qui signifie que la demande a un objet nommé `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="e9567-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="e9567-180">Il est vrai qu’il s’agit d’un moyen indirect de tester le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9567-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="e9567-181">Si un formulaire contient plusieurs boutons Envoyer, seul le nom du bouton sur lequel l’utilisateur a cliqué apparaît dans la demande.</span><span class="sxs-lookup"><span data-stu-id="e9567-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="e9567-182">Par conséquent, logiquement, si le nom d’un bouton particulier apparaît dans la demande &mdash; ou comme indiqué dans le code, si ce bouton n’est pas vide &mdash; c’est le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9567-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="e9567-183">L’opérateur `&&` signifie « and » (AND logique).</span><span class="sxs-lookup"><span data-stu-id="e9567-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="e9567-184">Par conséquent, la condition `if` entière est...</span><span class="sxs-lookup"><span data-stu-id="e9567-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="e9567-185">*Cette demande est une publication (et non une demande initiale)*</span><span class="sxs-lookup"><span data-stu-id="e9567-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="e9567-186">AND</span><span class="sxs-lookup"><span data-stu-id="e9567-186">AND</span></span>  
  
<span data-ttu-id="e9567-187">*Le* *bouton `buttonDelete`était le bouton qui a envoyé le formulaire.*</span><span class="sxs-lookup"><span data-stu-id="e9567-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="e9567-188">Ce formulaire (en fait, cette page) ne contient qu’un seul bouton. par conséquent, le test supplémentaire pour `buttonDelete` n’est techniquement pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e9567-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="e9567-189">Toutefois, vous allez effectuer une opération qui supprimera définitivement les données.</span><span class="sxs-lookup"><span data-stu-id="e9567-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="e9567-190">Vous souhaitez donc être aussi sûr que possible d’effectuer l’opération uniquement lorsque l’utilisateur l’a demandée explicitement.</span><span class="sxs-lookup"><span data-stu-id="e9567-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="e9567-191">Par exemple, supposons que vous développiez cette page par la suite et y ajoutiez d’autres boutons.</span><span class="sxs-lookup"><span data-stu-id="e9567-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="e9567-192">Même si vous avez cliqué sur le bouton `buttonDelete`, le code qui supprime le film s’exécutera uniquement.</span><span class="sxs-lookup"><span data-stu-id="e9567-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="e9567-193">Comme dans la page *EditMovie* , vous récupérez l’ID à partir du champ masqué, puis exécutez la commande SQL.</span><span class="sxs-lookup"><span data-stu-id="e9567-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="e9567-194">La syntaxe de l’instruction `Delete` est la suivante :</span><span class="sxs-lookup"><span data-stu-id="e9567-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="e9567-195">Il est essentiel d’inclure la clause `WHERE` et l’ID.</span><span class="sxs-lookup"><span data-stu-id="e9567-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="e9567-196">Si vous omettez la clause WHERE, *tous les enregistrements de la table seront supprimés*.</span><span class="sxs-lookup"><span data-stu-id="e9567-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="e9567-197">Comme vous l’avez vu, vous transmettez la valeur d’ID à la commande SQL à l’aide d’un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="e9567-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="e9567-198">Test du processus de suppression du film</span><span class="sxs-lookup"><span data-stu-id="e9567-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="e9567-199">Vous pouvez maintenant tester.</span><span class="sxs-lookup"><span data-stu-id="e9567-199">Now you can test.</span></span> <span data-ttu-id="e9567-200">Exécutez la page *films* , puis cliquez sur **supprimer** en regard d’un film.</span><span class="sxs-lookup"><span data-stu-id="e9567-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="e9567-201">Lorsque la page *DeleteMovie* s’affiche, cliquez sur **supprimer le film**.</span><span class="sxs-lookup"><span data-stu-id="e9567-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Page supprimer le film avec le bouton supprimer le film mis en surbrillance](deleting-data/_static/image4.png)

<span data-ttu-id="e9567-203">Lorsque vous cliquez sur le bouton, le code supprime les films et retourne à la liste des films.</span><span class="sxs-lookup"><span data-stu-id="e9567-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="e9567-204">Vous pouvez y rechercher le film supprimé et confirmer qu’il a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="e9567-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="e9567-205">À venir suivant</span><span class="sxs-lookup"><span data-stu-id="e9567-205">Coming Up Next</span></span>

<span data-ttu-id="e9567-206">Le didacticiel suivant vous montre comment donner une apparence et une disposition communes à toutes les pages sur votre site.</span><span class="sxs-lookup"><span data-stu-id="e9567-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="e9567-207">Liste complète des pages de films (mise à jour avec les liens supprimer)</span><span class="sxs-lookup"><span data-stu-id="e9567-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="e9567-208">Liste complète de la page DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="e9567-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="e9567-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e9567-209">Additional Resources</span></span>

- [<span data-ttu-id="e9567-210">Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="e9567-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="e9567-211">[Instruction SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) sur le site W3Schools</span><span class="sxs-lookup"><span data-stu-id="e9567-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9567-212">[Précédent](updating-data.md)
> [Suivant](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="e9567-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
