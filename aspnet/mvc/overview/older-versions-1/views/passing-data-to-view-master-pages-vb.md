---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passage de données pour afficher les Pages maîtres (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue. Nous examinons les deux stratégies pour passer des données à une vue m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b840e0a5cc325a043ae88c10f52cca418589119
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055796"
---
<a name="passing-data-to-view-master-pages-vb"></a><span data-ttu-id="ac27e-104">Passage de données à des pages maîtres de vue (VB)</span><span class="sxs-lookup"><span data-stu-id="ac27e-104">Passing Data to View Master Pages (VB)</span></span>
====================
<span data-ttu-id="ac27e-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ac27e-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ac27e-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="ac27e-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> <span data-ttu-id="ac27e-107">L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-107">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="ac27e-108">Nous allons examiner deux stratégies pour passer des données à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-108">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="ac27e-109">Tout d’abord, nous abordons une solution facile qui résulte dans une application qui est difficile à gérer.</span><span class="sxs-lookup"><span data-stu-id="ac27e-109">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="ac27e-110">Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initiale, mais les résultats dans une application beaucoup plus facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="ac27e-110">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>


## <a name="passing-data-to-view-master-pages"></a><span data-ttu-id="ac27e-111">Passage de données à des Pages maîtres de vue</span><span class="sxs-lookup"><span data-stu-id="ac27e-111">Passing Data to View Master Pages</span></span>

<span data-ttu-id="ac27e-112">L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-112">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="ac27e-113">Nous allons examiner deux stratégies pour passer des données à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-113">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="ac27e-114">Tout d’abord, nous abordons une solution facile qui résulte dans une application qui est difficile à gérer.</span><span class="sxs-lookup"><span data-stu-id="ac27e-114">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="ac27e-115">Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initiale, mais les résultats dans une application beaucoup plus facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="ac27e-115">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>

### <a name="the-problem"></a><span data-ttu-id="ac27e-116">Le problème</span><span class="sxs-lookup"><span data-stu-id="ac27e-116">The Problem</span></span>

<span data-ttu-id="ac27e-117">Imaginez que vous générez une application de base de données de films et que vous souhaitez afficher la liste des catégories de films sur chaque page dans votre application (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="ac27e-117">Imagine that you are building a movie database application and you want to display the list of movie categories on every page in your application (see Figure 1).</span></span> <span data-ttu-id="ac27e-118">En outre, imaginez que la liste des catégories de films est stockée dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-118">Imagine, furthermore, that the list of movie categories is stored in a database table.</span></span> <span data-ttu-id="ac27e-119">Dans ce cas, il serait judicieux pour récupérer les catégories à partir de la base de données et de restituer la liste des catégories de films dans une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-119">In that case, it would make sense to retrieve the categories from the database and render the list of movie categories within a view master page.</span></span>


<span data-ttu-id="ac27e-120">[![Affichage des catégories de films dans une page maître de vue](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ac27e-120">[![Displaying movie categories in a view master page](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span></span>

<span data-ttu-id="ac27e-121">**Figure 01**: Affichage des catégories de films dans une page maître de vue ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ac27e-121">**Figure 01**: Displaying movie categories in a view master page ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image3.png))</span></span>


<span data-ttu-id="ac27e-122">Voici le problème.</span><span class="sxs-lookup"><span data-stu-id="ac27e-122">Here's the problem.</span></span> <span data-ttu-id="ac27e-123">Comment pour récupérer la liste des catégories de film dans la page maître ?</span><span class="sxs-lookup"><span data-stu-id="ac27e-123">How do you retrieve the list of movie categories in the master page?</span></span> <span data-ttu-id="ac27e-124">Il est tentant d’appeler directement les méthodes de vos classes de modèle dans la page maître.</span><span class="sxs-lookup"><span data-stu-id="ac27e-124">It is tempting to call methods of your model classes in the master page directly.</span></span> <span data-ttu-id="ac27e-125">En d’autres termes, il est tentant d’inclure le code de récupération des données à partir de la droite de la base de données dans votre page maître.</span><span class="sxs-lookup"><span data-stu-id="ac27e-125">In other words, it is tempting to include the code for retrieving the data from the database right in your master page.</span></span> <span data-ttu-id="ac27e-126">Toutefois, en ignorant vos contrôleurs MVC pour accéder à la base de données risque de violer la séparation claire des préoccupations qui est un des principaux avantages de la création d’une application MVC.</span><span class="sxs-lookup"><span data-stu-id="ac27e-126">However, bypassing your MVC controllers to access the database would violate the clean separation of concerns that is one of the primary benefits of building an MVC application.</span></span>

<span data-ttu-id="ac27e-127">Dans une application MVC, vous souhaitez que toutes les interactions entre les vues MVC et votre modèle MVC d’être gérés par vos contrôleurs MVC.</span><span class="sxs-lookup"><span data-stu-id="ac27e-127">In an MVC application, you want all interaction between your MVC views and your MVC model to be handled by your MVC controllers.</span></span> <span data-ttu-id="ac27e-128">Cette séparation des préoccupations des résultats dans une application plus facile à gérer, adaptable et testable.</span><span class="sxs-lookup"><span data-stu-id="ac27e-128">This separation of concerns results in a more maintainable, adaptable, and testable application.</span></span>

<span data-ttu-id="ac27e-129">Dans une application MVC, toutes les données passées à une vue, y compris une page maître de vue, doivent être passées à une vue par une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ac27e-129">In an MVC application, all data passed to a view – including a view master page – should be passed to a view by a controller action.</span></span> <span data-ttu-id="ac27e-130">En outre, les données doivent être passées en tirant parti des données d’affichage.</span><span class="sxs-lookup"><span data-stu-id="ac27e-130">Furthermore, the data should be passed by taking advantage of view data.</span></span> <span data-ttu-id="ac27e-131">Dans le reste de ce didacticiel, j’examine les deux méthodes de transmission des données de la vue à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-131">In the remainder of this tutorial, I examine two methods of passing view data to a view master page.</span></span>

### <a name="the-simple-solution"></a><span data-ttu-id="ac27e-132">La Solution Simple</span><span class="sxs-lookup"><span data-stu-id="ac27e-132">The Simple Solution</span></span>

<span data-ttu-id="ac27e-133">Commençons par la solution la plus simple pour passer des données de la vue à partir d’un contrôleur à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-133">Let's start with the simplest solution to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="ac27e-134">La solution la plus simple consiste à transmettre les données d’affichage pour la page maître dans chaque action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ac27e-134">The simplest solution is to pass the view data for the master page in each and every controller action.</span></span>

<span data-ttu-id="ac27e-135">Envisagez le contrôleur dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="ac27e-135">Consider the controller in Listing 1.</span></span> <span data-ttu-id="ac27e-136">Il expose deux actions nommées `Index()` et `Details()`.</span><span class="sxs-lookup"><span data-stu-id="ac27e-136">It exposes two actions named `Index()` and `Details()`.</span></span> <span data-ttu-id="ac27e-137">Le `Index()` méthode d’action retourne chaque film dans la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="ac27e-137">The `Index()` action method returns every movie in the Movies database table.</span></span> <span data-ttu-id="ac27e-138">Le `Details()` méthode d’action retourne chaque film dans une catégorie particulière de film.</span><span class="sxs-lookup"><span data-stu-id="ac27e-138">The `Details()` action method returns every movie in a particular movie category.</span></span>

<span data-ttu-id="ac27e-139">**Liste 1 : `Controllers\HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="ac27e-139">**Listing 1 – `Controllers\HomeController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

<span data-ttu-id="ac27e-140">Notez que les deux le `Index()` et le `Details()` actions ajoutent deux éléments pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-140">Notice that both the `Index()` and the `Details()` actions add two items to view data.</span></span> <span data-ttu-id="ac27e-141">Le `Index()` action ajoute deux clés : catégories et des films.</span><span class="sxs-lookup"><span data-stu-id="ac27e-141">The `Index()` action adds two keys: categories and movies.</span></span> <span data-ttu-id="ac27e-142">La clé de catégories représente la liste des catégories de film affiché par la page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-142">The categories key represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="ac27e-143">La clé de films représente la liste de films affichées par la page de vue d’Index.</span><span class="sxs-lookup"><span data-stu-id="ac27e-143">The movies key represents the list of movies displayed by the Index view page.</span></span>

<span data-ttu-id="ac27e-144">Le `Details()` action ajoute également deux clés nommée catégories et des films.</span><span class="sxs-lookup"><span data-stu-id="ac27e-144">The `Details()` action also adds two keys named categories and movies.</span></span> <span data-ttu-id="ac27e-145">La clé de catégories, représente une fois encore, la liste des catégories de film affiché par la page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-145">The categories key, once again, represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="ac27e-146">La clé de films représente la liste de films dans une catégorie particulière, affiché par la page de vue de détails (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="ac27e-146">The movies key represents the list of movies in a particular category displayed by the Details view page (see Figure 2).</span></span>


<span data-ttu-id="ac27e-147">[![La vue Détails](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ac27e-147">[![The Details view](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span></span>

<span data-ttu-id="ac27e-148">**Figure 02**: La vue Détails ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ac27e-148">**Figure 02**: The Details view ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image6.png))</span></span>


<span data-ttu-id="ac27e-149">La vue Index est contenue dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="ac27e-149">The Index view is contained in Listing 2.</span></span> <span data-ttu-id="ac27e-150">Il itère simplement la liste de films représenté par l’élément de films dans les données d’affichage.</span><span class="sxs-lookup"><span data-stu-id="ac27e-150">It simply iterates through the list of movies represented by the movies item in view data.</span></span>

<span data-ttu-id="ac27e-151">**Listing 2 : `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ac27e-151">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

<span data-ttu-id="ac27e-152">La page maître de vue est contenue dans le Listing 3.</span><span class="sxs-lookup"><span data-stu-id="ac27e-152">The view master page is contained in Listing 3.</span></span> <span data-ttu-id="ac27e-153">La page maître en mode effectue une itération et restitue toutes les catégories de film représentés par l’élément de catégories à partir des données de la vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-153">The view master page iterates and renders all of the movie categories represented by the categories item from view data.</span></span>

<span data-ttu-id="ac27e-154">**Liste 3 : `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="ac27e-154">**Listing 3 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

<span data-ttu-id="ac27e-155">Toutes les données est transmis à la vue et la page maître en mode Afficher les données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-155">All data is passed to the view and the view master page through view data.</span></span> <span data-ttu-id="ac27e-156">C’est la méthode correcte pour passer des données à la page maître.</span><span class="sxs-lookup"><span data-stu-id="ac27e-156">That is the correct way to pass data to the master page.</span></span>

<span data-ttu-id="ac27e-157">Par conséquent, quel est le problème avec cette solution ?</span><span class="sxs-lookup"><span data-stu-id="ac27e-157">So, what's wrong with this solution?</span></span> <span data-ttu-id="ac27e-158">Le problème est que cette solution enfreint le principe DRY (ne vous répétez pas).</span><span class="sxs-lookup"><span data-stu-id="ac27e-158">The problem is that this solution violates the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="ac27e-159">Chaque action de contrôleur doit ajouter la même liste des catégories de films pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-159">Each and every controller action must add the very same list of movie categories to view data.</span></span> <span data-ttu-id="ac27e-160">Des doublons de code dans votre application, votre application beaucoup plus difficile à gérer, de s’adapter et de modifier.</span><span class="sxs-lookup"><span data-stu-id="ac27e-160">Having duplicate code in your application makes your application much more difficult to maintain, adapt, and modify.</span></span>

### <a name="the-good-solution"></a><span data-ttu-id="ac27e-161">La bonne Solution.</span><span class="sxs-lookup"><span data-stu-id="ac27e-161">The Good Solution</span></span>

<span data-ttu-id="ac27e-162">Dans cette section, nous examinons une solution alternative et préférable, pour passer des données à partir d’une action de contrôleur à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-162">In this section, we examine an alternative, and better, solution to passing data from a controller action to a view master page.</span></span> <span data-ttu-id="ac27e-163">Au lieu d’ajouter les catégories de films pour la page maître dans chaque action du contrôleur, nous ajoutons les catégories de films à afficher les données qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="ac27e-163">Instead of adding the movie categories for the master page in each and every controller action, we add the movie categories to the view data only once.</span></span> <span data-ttu-id="ac27e-164">Toutes les données d’affichage utilisées par la page maître de vue est ajoutée dans une Application controller.</span><span class="sxs-lookup"><span data-stu-id="ac27e-164">All view data used by the view master page is added in an Application controller.</span></span>

<span data-ttu-id="ac27e-165">La classe ApplicationController est contenue dans la liste 4.</span><span class="sxs-lookup"><span data-stu-id="ac27e-165">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="ac27e-166">La classe ApplicationController est contenue dans la liste 4.</span><span class="sxs-lookup"><span data-stu-id="ac27e-166">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="ac27e-167">**Liste 4 – `Controllers\ApplicationController.vb`**</span><span class="sxs-lookup"><span data-stu-id="ac27e-167">**Listing 4 – `Controllers\ApplicationController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

<span data-ttu-id="ac27e-168">Voici trois choses que vous devriez remarquer que sur le contrôleur de l’Application sur la liste 4.</span><span class="sxs-lookup"><span data-stu-id="ac27e-168">There are three things that you should notice about the Application controller in Listing 4.</span></span> <span data-ttu-id="ac27e-169">Tout d’abord, notez que la classe hérite de la classe de base System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="ac27e-169">First, notice that the class inherits from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="ac27e-170">Le contrôleur de l’Application est une classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ac27e-170">The Application controller is a controller class.</span></span>

<span data-ttu-id="ac27e-171">En second lieu, notez que la classe de contrôleur d’Application est une classe MustInherit.</span><span class="sxs-lookup"><span data-stu-id="ac27e-171">Second, notice that the Application controller class is a MustInherit class.</span></span> <span data-ttu-id="ac27e-172">Une classe MustInherit est une classe qui doit être implémentée par une classe concrète.</span><span class="sxs-lookup"><span data-stu-id="ac27e-172">An MustInherit class is a class that must be implemented by a concrete class.</span></span> <span data-ttu-id="ac27e-173">Étant donné que le contrôleur de l’Application est une classe MustInherit, vous ne peut pas pas appeler les méthodes définies dans la classe directement.</span><span class="sxs-lookup"><span data-stu-id="ac27e-173">Because the Application controller is an MustInherit class, you cannot not invoke any methods defined in the class directly.</span></span> <span data-ttu-id="ac27e-174">Si vous essayez d’appeler directement de la classe d’Application vous obtiendrez un message d’erreur ressource est introuvable.</span><span class="sxs-lookup"><span data-stu-id="ac27e-174">If you attempt to invoke the Application class directly then you'll get a Resource Cannot Be Found error message.</span></span>

<span data-ttu-id="ac27e-175">Troisièmement, notez que le contrôleur de l’Application contient un constructeur qui ajoute la liste des catégories de films pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-175">Third, notice that the Application controller contains a constructor that adds the list of movie categories to view data.</span></span> <span data-ttu-id="ac27e-176">Chaque classe de contrôleur qui hérite de contrôleur d’Application appelle automatiquement constructeur du contrôleur de l’Application.</span><span class="sxs-lookup"><span data-stu-id="ac27e-176">Every controller class that inherits from the Application controller calls the Application controller's constructor automatically.</span></span> <span data-ttu-id="ac27e-177">Chaque fois que vous appelez une action sur n’importe quel contrôleur qui hérite de l’Application controller, les catégories de films est automatiquement inclus dans les données d’affichage.</span><span class="sxs-lookup"><span data-stu-id="ac27e-177">Whenever you call any action on any controller that inherits from the Application controller, the movie categories is included in the view data automatically.</span></span>

<span data-ttu-id="ac27e-178">Le contrôleur de films sur la liste 5 hérite le contrôleur de l’Application.</span><span class="sxs-lookup"><span data-stu-id="ac27e-178">The Movies controller in Listing 5 inherits from the Application controller.</span></span>

<span data-ttu-id="ac27e-179">**Liste 5 – `Controllers\MoviesController.vb`**</span><span class="sxs-lookup"><span data-stu-id="ac27e-179">**Listing 5 – `Controllers\MoviesController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

<span data-ttu-id="ac27e-180">Le contrôleur de films, tout comme le contrôleur Home abordé dans la section précédente, expose deux méthodes d’action nommées `Index()` et `Details()`.</span><span class="sxs-lookup"><span data-stu-id="ac27e-180">The Movies controller, just like the Home controller discussed in the previous section, exposes two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="ac27e-181">Notez que la liste des catégories de film affiché par la page maître de vue n’est pas ajouté à afficher des données dans le `Index()` ou `Details()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ac27e-181">Notice that the list of movie categories displayed by the view master page is not added to view data in either the `Index()` or `Details()` method.</span></span> <span data-ttu-id="ac27e-182">Étant donné que le contrôleur de films hérite de contrôleur d’Application, la liste des catégories de films est ajoutée pour afficher les données automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ac27e-182">Because the Movies controller inherits from the Application controller, the list of movie categories is added to view data automatically.</span></span>

<span data-ttu-id="ac27e-183">Notez que cette solution à l’ajout de données d’affichage pour une page maître de vue ne viole pas le principe DRY (ne vous répétez pas).</span><span class="sxs-lookup"><span data-stu-id="ac27e-183">Notice that this solution to adding view data for a view master page does not violate the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="ac27e-184">Le code pour l’ajout de la liste des catégories de films pour afficher des données est contenu dans un seul emplacement : le constructeur pour le contrôleur de l’Application.</span><span class="sxs-lookup"><span data-stu-id="ac27e-184">The code for adding the list of movie categories to view data is contained in only one location: the constructor for the Application controller.</span></span>

### <a name="summary"></a><span data-ttu-id="ac27e-185">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="ac27e-185">Summary</span></span>

<span data-ttu-id="ac27e-186">Dans ce didacticiel, nous avons abordé les deux approches pour passer des données de la vue à partir d’un contrôleur à une page maître de vue.</span><span class="sxs-lookup"><span data-stu-id="ac27e-186">In this tutorial, we discussed two approaches to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="ac27e-187">Tout d’abord, nous avons examiné une simple, mais difficile à maintenir l’approche.</span><span class="sxs-lookup"><span data-stu-id="ac27e-187">First, we examined a simple, but difficult to maintain approach.</span></span> <span data-ttu-id="ac27e-188">Dans la première section, nous avons abordé comment vous pouvez ajouter des données d’affichage pour une page maître de vue dans chaque chaque action du contrôleur dans votre application.</span><span class="sxs-lookup"><span data-stu-id="ac27e-188">In the first section, we discussed how you can add view data for a view master page in each every controller action in your application.</span></span> <span data-ttu-id="ac27e-189">Nous avons conclu que c’était une approche incorrecte, car elle enfreint le principe DRY (ne vous répétez pas).</span><span class="sxs-lookup"><span data-stu-id="ac27e-189">We concluded that this was a bad approach because it violates the DRY (Don't Repeat Yourself) principle.</span></span>

<span data-ttu-id="ac27e-190">Ensuite, nous avons examiné une bien meilleure stratégie pour l’ajout de données requises par une page maître de vue pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="ac27e-190">Next, we examined a much better strategy for adding data required by a view master page to view data.</span></span> <span data-ttu-id="ac27e-191">Au lieu d’ajouter les données d’affichage dans chaque action du contrôleur, nous avons ajouté les données d’affichage qu’une seule fois au sein d’un contrôleur d’Application.</span><span class="sxs-lookup"><span data-stu-id="ac27e-191">Instead of adding the view data in each and every controller action, we added the view data only once within an Application controller.</span></span> <span data-ttu-id="ac27e-192">De cette façon, vous pouvez éviter code en double lors du passage des données à une page maître de vue dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ac27e-192">That way, you can avoid duplicate code when passing data to a view master page in an ASP.NET MVC application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac27e-193">Précédent</span><span class="sxs-lookup"><span data-stu-id="ac27e-193">Previous</span></span>](creating-page-layouts-with-view-master-pages-vb.md)