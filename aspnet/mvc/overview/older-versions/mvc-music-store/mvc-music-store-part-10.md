---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Partie 10 : mises à jour finales de la navigation et de la conception de site, conclusion | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 10 couvre les dernières mises à jour de la navigation et des...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539367"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="398a6-104">Partie 10 : mises à jour finales de la navigation et de la conception de site, conclusion</span><span class="sxs-lookup"><span data-stu-id="398a6-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="398a6-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="398a6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="398a6-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="398a6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="398a6-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="398a6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="398a6-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="398a6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="398a6-109">La partie 10 couvre les dernières mises à jour de la navigation et de la conception de site, conclusion.</span><span class="sxs-lookup"><span data-stu-id="398a6-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="398a6-110">Nous avons effectué toutes les fonctionnalités majeures de notre site, mais nous avons toujours des fonctionnalités à ajouter à la navigation sur le site, à la page d’hébergement et à la page de navigation dans le Store.</span><span class="sxs-lookup"><span data-stu-id="398a6-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="398a6-111">Création de la vue partielle du résumé du panier d’achat</span><span class="sxs-lookup"><span data-stu-id="398a6-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="398a6-112">Nous souhaitons exposer le nombre d’éléments dans le panier d’achat de l’utilisateur sur l’ensemble du site.</span><span class="sxs-lookup"><span data-stu-id="398a6-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="398a6-113">Nous pouvons facilement implémenter cela en créant une vue partielle qui est ajoutée à notre site. Master.</span><span class="sxs-lookup"><span data-stu-id="398a6-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="398a6-114">Comme indiqué précédemment, le contrôleur ShoppingCart comprend une méthode d’action CartSummary qui retourne une vue partielle :</span><span class="sxs-lookup"><span data-stu-id="398a6-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="398a6-115">Pour créer la vue partielle CartSummary, cliquez avec le bouton droit sur le dossier views/ShoppingCart et sélectionnez Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="398a6-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="398a6-116">Nommez la vue CartSummary et activez la case à cocher créer une vue partielle, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="398a6-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="398a6-117">La vue partielle CartSummary est vraiment simple : il s’agit simplement d’un lien vers la vue d’index ShoppingCart qui affiche le nombre d’éléments dans le panier.</span><span class="sxs-lookup"><span data-stu-id="398a6-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="398a6-118">Le code complet pour CartSummary. cshtml est le suivant :</span><span class="sxs-lookup"><span data-stu-id="398a6-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="398a6-119">Nous pouvons inclure une vue partielle dans n’importe quelle page du site, y compris le site principal, à l’aide de la méthode html. RenderAction.</span><span class="sxs-lookup"><span data-stu-id="398a6-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="398a6-120">RenderAction requiert que nous spécifiions le nom de l’action (« CartSummary ») et le nom du contrôleur (« ShoppingCart ») comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="398a6-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="398a6-121">Avant de l’ajouter à la disposition du site, nous allons également créer le menu genre pour que nous puissions effectuer toutes nos mises à jour de site. Master à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="398a6-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="398a6-122">Création de la vue partielle du menu genre</span><span class="sxs-lookup"><span data-stu-id="398a6-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="398a6-123">Nous pouvons simplifier considérablement la navigation dans le magasin en ajoutant un menu genre qui répertorie tous les genres disponibles dans notre magasin.</span><span class="sxs-lookup"><span data-stu-id="398a6-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="398a6-124">Nous suivons également les mêmes étapes pour créer une vue partielle GenreMenu, puis nous pouvons les ajouter à la fois sur le site maître.</span><span class="sxs-lookup"><span data-stu-id="398a6-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="398a6-125">Tout d’abord, ajoutez l’action de contrôleur GenreMenu suivante à StoreController :</span><span class="sxs-lookup"><span data-stu-id="398a6-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="398a6-126">Cette action renvoie la liste des genres qui seront affichés par la vue partielle, que nous allons ensuite créer.</span><span class="sxs-lookup"><span data-stu-id="398a6-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="398a6-127">*Remarque : nous avons ajouté l’attribut [ChildActionOnly] à cette action de contrôleur, ce qui indique que cette action doit uniquement être utilisée à partir d’une vue partielle. Cet attribut empêchera l’exécution de l’action du contrôleur en accédant à/Store/GenreMenu. Cela n’est pas obligatoire pour les vues partielles, mais c’est une bonne pratique, car nous voulons nous assurer que nos actions de contrôleur sont utilisées comme nous l’avons prévu. Nous revenons également à PartialView plutôt qu’à View, ce qui permet au moteur de vue de savoir qu’il ne doit pas utiliser la disposition pour cette vue, car elle est incluse dans d’autres vues.*</span><span class="sxs-lookup"><span data-stu-id="398a6-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="398a6-128">Cliquez avec le bouton droit sur l’action du contrôleur GenreMenu et créez une vue partielle nommée GenreMenu, qui est fortement typée à l’aide de la classe de données de la vue de genre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="398a6-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="398a6-129">Mettez à jour le code de vue pour la vue partielle GenreMenu pour afficher les éléments à l’aide d’une liste non triée comme suit.</span><span class="sxs-lookup"><span data-stu-id="398a6-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="398a6-130">Mise à jour de la disposition du site pour afficher nos vues partielles</span><span class="sxs-lookup"><span data-stu-id="398a6-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="398a6-131">Nous pouvons ajouter des vues partielles à la disposition du site (/Views/Shared/\_Layout. cshtml) en appelant html. RenderAction ().</span><span class="sxs-lookup"><span data-stu-id="398a6-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="398a6-132">Nous les ajouterons dans, ainsi que des balises supplémentaires pour les afficher, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="398a6-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="398a6-133">Maintenant, lorsque nous exécutons l’application, nous verrons le genre dans la zone de navigation gauche et le résumé du panier en haut.</span><span class="sxs-lookup"><span data-stu-id="398a6-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="398a6-134">Mettre à jour vers la page de navigation du Store</span><span class="sxs-lookup"><span data-stu-id="398a6-134">Update to the Store Browse page</span></span>

<span data-ttu-id="398a6-135">La page de navigation dans le magasin est fonctionnelle, mais ne semble pas très bonne.</span><span class="sxs-lookup"><span data-stu-id="398a6-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="398a6-136">Nous pouvons mettre à jour la page pour afficher les albums dans une meilleure disposition en mettant à jour le code de vue (disponible dans/Views/Store/Browse.cshtml) comme suit :</span><span class="sxs-lookup"><span data-stu-id="398a6-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="398a6-137">Ici, nous utilisons URL. action plutôt que html. ActionLink afin de pouvoir appliquer une mise en forme spéciale au lien pour inclure l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="398a6-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="398a6-138">*Remarque : nous affichons une couverture d’album générique pour ces albums. Ces informations sont stockées dans la base de données et sont modifiables via le responsable du magasin. Vous êtes invité à ajouter votre propre illustration.*</span><span class="sxs-lookup"><span data-stu-id="398a6-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="398a6-139">Maintenant, lorsque nous parcourons un genre, nous verrons les albums affichés dans une grille avec l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="398a6-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="398a6-140">Mise à jour de la page d’hébergement pour afficher les meilleurs albums de vente</span><span class="sxs-lookup"><span data-stu-id="398a6-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="398a6-141">Nous souhaitons utiliser nos principaux Albums de vente sur la page d’hébergement pour augmenter les ventes.</span><span class="sxs-lookup"><span data-stu-id="398a6-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="398a6-142">Nous allons apporter des mises à jour à notre HomeController pour les gérer, et ajouter également des graphiques supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="398a6-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="398a6-143">Tout d’abord, nous allons ajouter une propriété de navigation à notre classe album afin que l’EntityFramework sache qu’ils sont associés.</span><span class="sxs-lookup"><span data-stu-id="398a6-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="398a6-144">Les dernières lignes de notre classe **album** doivent maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="398a6-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="398a6-145">*Remarque : cette opération nécessite l’ajout d’une instruction using pour importer l’espace de noms System. Collections. Generic.*</span><span class="sxs-lookup"><span data-stu-id="398a6-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="398a6-146">Tout d’abord, nous allons ajouter un champ storeDB et les instructions MvcMusicStore. Models à l’aide d’instructions, comme dans nos autres contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="398a6-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="398a6-147">Ensuite, nous allons ajouter la méthode suivante au HomeController qui interroge notre base de données pour trouver les meilleurs albums de vente en fonction de OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="398a6-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="398a6-148">Il s’agit d’une méthode privée, car nous ne voulons pas la rendre disponible en tant qu’action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="398a6-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="398a6-149">Nous l’incluons dans le HomeController pour des raisons de simplicité, mais il est recommandé de déplacer votre logique métier dans des classes de service distinctes, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="398a6-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="398a6-150">Cela étant en place, nous pouvons mettre à jour l’action du contrôleur d’index pour interroger les 5 premiers albums de vente et les renvoyer à la vue.</span><span class="sxs-lookup"><span data-stu-id="398a6-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="398a6-151">Le code complet pour le HomeController mis à jour est comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="398a6-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="398a6-152">Enfin, nous devrons mettre à jour notre vue d’index de base afin de pouvoir afficher une liste d’albums en mettant à jour le type de modèle et en ajoutant la liste d’albums en bas.</span><span class="sxs-lookup"><span data-stu-id="398a6-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="398a6-153">Nous allons également ajouter un titre et une section de promotion à la page.</span><span class="sxs-lookup"><span data-stu-id="398a6-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="398a6-154">Maintenant, lorsque nous exécutons l’application, nous verrons notre page d’hébergement mise à jour avec les meilleurs albums de vente et notre message promotionnel.</span><span class="sxs-lookup"><span data-stu-id="398a6-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="398a6-155">Conclusion</span><span class="sxs-lookup"><span data-stu-id="398a6-155">Conclusion</span></span>

<span data-ttu-id="398a6-156">Nous avons vu que ASP.NET MVC facilite la création d’un site Web sophistiqué avec accès aux bases de données, appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="398a6-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="398a6-157">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="398a6-157">pretty quickly.</span></span> <span data-ttu-id="398a6-158">Nous espérons que ce didacticiel vous a donné les outils dont vous avez besoin pour commencer à créer vos propres applications ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="398a6-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="398a6-159">Précédent</span><span class="sxs-lookup"><span data-stu-id="398a6-159">Previous</span></span>](mvc-music-store-part-9.md)
