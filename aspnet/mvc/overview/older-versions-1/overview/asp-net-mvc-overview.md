---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Vue d’ensemble d’ASP.NET MVC | Microsoft Docs
author: microsoft
description: En savoir plus sur les différences entre l’application ASP.NET MVC et les applications Web Forms ASP.NET. Découvrez comment déterminer quand créer une application ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 61a7841ee238ec365b7d1909221bbe3d834faf84
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025536"
---
<a name="aspnet-mvc-overview"></a><span data-ttu-id="06eed-104">Vue d’ensemble d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="06eed-104">ASP.NET MVC Overview</span></span>
====================
<span data-ttu-id="06eed-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="06eed-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="06eed-106">En savoir plus sur les différences entre l’application ASP.NET MVC et les applications Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06eed-106">Learn about the differences between ASP.NET MVC application and ASP.NET Web Forms applications.</span></span> <span data-ttu-id="06eed-107">Découvrez comment déterminer quand créer une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="06eed-107">Learn how to decide when to build an ASP.NET MVC application.</span></span>


<span data-ttu-id="06eed-108">Le modèle d’architecture Model-View-Controller (MVC) sépare une application en trois composants principaux : le modèle, la vue et le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="06eed-108">The Model-View-Controller (MVC) architectural pattern separates an application into three main components: the model, the view, and the controller.</span></span> <span data-ttu-id="06eed-109">L’infrastructure ASP.NET MVC fournit une alternative au modèle Web Forms ASP.NET pour la création d’applications Web basées sur MVC.</span><span class="sxs-lookup"><span data-stu-id="06eed-109">The ASP.NET MVC framework provides an alternative to the ASP.NET Web Forms pattern for creating MVC-based Web applications.</span></span> <span data-ttu-id="06eed-110">L’infrastructure ASP.NET MVC est une infrastructure de présentation simple et facilement testable, qui (comme les applications basées sur des Web Forms) est intégré avec des fonctionnalités ASP.NET existantes, telles que les pages maîtres et l’authentification basée sur l’appartenance.</span><span class="sxs-lookup"><span data-stu-id="06eed-110">The ASP.NET MVC framework is a lightweight, highly testable presentation framework that (as with Web Forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication.</span></span> <span data-ttu-id="06eed-111">L’infrastructure MVC est définie dans le **System.Web.Mvc** espace de noms, est une partie essentielle et pris en charge de la **System.Web** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="06eed-111">The MVC framework is defined in the **System.Web.Mvc** namespace and is a fundamental, supported part of the **System.Web** namespace.</span></span>   
  
<span data-ttu-id="06eed-112">MVC est un modèle de conception standard que de nombreux développeurs connaissent.</span><span class="sxs-lookup"><span data-stu-id="06eed-112">MVC is a standard design pattern that many developers are familiar with.</span></span> <span data-ttu-id="06eed-113">Certains types d’applications Web bénéficieront de l’infrastructure MVC.</span><span class="sxs-lookup"><span data-stu-id="06eed-113">Some types of Web applications will benefit from the MVC framework.</span></span> <span data-ttu-id="06eed-114">D’autres continueront à utiliser le modèle d’application ASP.NET traditionnel qui est basé sur des Web Forms et les publications (postback).</span><span class="sxs-lookup"><span data-stu-id="06eed-114">Others will continue to use the traditional ASP.NET application pattern that is based on Web Forms and postbacks.</span></span> <span data-ttu-id="06eed-115">Autres types d’applications Web combine les deux approches ; aucune de ces approches exclut l’autre.</span><span class="sxs-lookup"><span data-stu-id="06eed-115">Other types of Web applications will combine the two approaches; neither approach excludes the other.</span></span>   
  
<span data-ttu-id="06eed-116">L’infrastructure MVC inclut les composants suivants :</span><span class="sxs-lookup"><span data-stu-id="06eed-116">The MVC framework includes the following components:</span></span>


<span data-ttu-id="06eed-117">[![Appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06eed-117">[![Invoking a controller action that expects a parameter value](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)</span></span>

<span data-ttu-id="06eed-118">**Figure 01**: Appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-overview/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="06eed-118">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-overview/_static/image2.png))</span></span>


- <span data-ttu-id="06eed-119">**Modèles**.</span><span class="sxs-lookup"><span data-stu-id="06eed-119">**Models**.</span></span> <span data-ttu-id="06eed-120">Objets de modèle sont les parties de l’application qui implémentent la logique du domaine d’application s données.</span><span class="sxs-lookup"><span data-stu-id="06eed-120">Model objects are the parts of the application that implement the logic for the application s data domain.</span></span> <span data-ttu-id="06eed-121">Souvent, les objets de modèle récupèrent et stockent l’état de modèle dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="06eed-121">Often, model objects retrieve and store model state in a database.</span></span> <span data-ttu-id="06eed-122">Par exemple, un objet Product peut récupérer des informations à partir d’une base de données, exploiter, puis réécrire les informations mises à jour à une table Products dans SQL Server.</span><span class="sxs-lookup"><span data-stu-id="06eed-122">For example, a Product object might retrieve information from a database, operate on it, and then write updated information back to a Products table in SQL Server.</span></span>

<span data-ttu-id="06eed-123">Dans les petites applications, le modèle est souvent une séparation conceptuelle plutôt que physique.</span><span class="sxs-lookup"><span data-stu-id="06eed-123">In small applications, the model is often a conceptual separation instead of a physical one.</span></span> <span data-ttu-id="06eed-124">Par exemple, si l’application lit un jeu de données uniquement et il envoie à la vue, l’application n’a pas une couche de modèle physique et des classes associées.</span><span class="sxs-lookup"><span data-stu-id="06eed-124">For example, if the application only reads a data set and sends it to the view, the application does not have a physical model layer and associated classes.</span></span> <span data-ttu-id="06eed-125">Dans ce cas, le jeu de données joue le rôle d’un objet de modèle.</span><span class="sxs-lookup"><span data-stu-id="06eed-125">In that case, the data set takes on the role of a model object.</span></span>

- <span data-ttu-id="06eed-126">**Vues**.</span><span class="sxs-lookup"><span data-stu-id="06eed-126">**Views**.</span></span> <span data-ttu-id="06eed-127">Les vues sont les composants qui affichent l’interface utilisateur s (IU).</span><span class="sxs-lookup"><span data-stu-id="06eed-127">Views are the components that display the application s user interface (UI).</span></span> <span data-ttu-id="06eed-128">En règle générale, cette interface utilisateur est créée à partir des données de modèle.</span><span class="sxs-lookup"><span data-stu-id="06eed-128">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="06eed-129">Un exemple serait une vue d’édition d’une table Products affichant des zones de texte, des listes déroulantes et des cases à cocher en fonction de l’état actuel d’un objet de produits.</span><span class="sxs-lookup"><span data-stu-id="06eed-129">An example would be an edit view of a Products table that displays text boxes, drop-down lists, and check boxes based on the current state of a Products object.</span></span>

- <span data-ttu-id="06eed-130">**Contrôleurs**.</span><span class="sxs-lookup"><span data-stu-id="06eed-130">**Controllers**.</span></span> <span data-ttu-id="06eed-131">Les contrôleurs sont les composants qui gèrent l’interaction utilisateur, utiliser le modèle et finalement sélectionnent une vue qui affiche l’interface utilisateur à afficher.</span><span class="sxs-lookup"><span data-stu-id="06eed-131">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render that displays UI.</span></span> <span data-ttu-id="06eed-132">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="06eed-132">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="06eed-133">Par exemple, le contrôleur gère les valeurs de chaîne de requête et transmet ces valeurs au modèle, qui à son tour interroge la base de données en utilisant les valeurs.</span><span class="sxs-lookup"><span data-stu-id="06eed-133">For example, the controller handles query-string values, and passes these values to the model, which in turn queries the database by using the values.</span></span>

<span data-ttu-id="06eed-134">Le modèle MVC vous permet de créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en assurant un couplage lâche entre ces éléments.</span><span class="sxs-lookup"><span data-stu-id="06eed-134">The MVC pattern helps you create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="06eed-135">Le modèle spécifie l’emplacement de chaque type de logique dans l’application.</span><span class="sxs-lookup"><span data-stu-id="06eed-135">The pattern specifies where each kind of logic should be located in the application.</span></span> <span data-ttu-id="06eed-136">La logique de l’interface utilisateur appartient à la vue.</span><span class="sxs-lookup"><span data-stu-id="06eed-136">The UI logic belongs in the view.</span></span> <span data-ttu-id="06eed-137">La logique d’entrée appartient au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="06eed-137">Input logic belongs in the controller.</span></span> <span data-ttu-id="06eed-138">La logique métier appartient au modèle.</span><span class="sxs-lookup"><span data-stu-id="06eed-138">Business logic belongs in the model.</span></span> <span data-ttu-id="06eed-139">Cette séparation vous permet de gérer la complexité lorsque vous générez une application, car elle vous permet de se concentrer sur l’un des aspects de l’implémentation à la fois.</span><span class="sxs-lookup"><span data-stu-id="06eed-139">This separation helps you manage complexity when you build an application, because it enables you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="06eed-140">Par exemple, vous pouvez vous concentrer sur la vue sans dépendre de la logique métier.</span><span class="sxs-lookup"><span data-stu-id="06eed-140">For example, you can focus on the view without depending on the business logic.</span></span>   
  
<span data-ttu-id="06eed-141">Outre la gestion de la complexité, le modèle MVC rend plus facile de tester les applications qu’il est de tester une application Web ASP.NET basée sur des Web Forms.</span><span class="sxs-lookup"><span data-stu-id="06eed-141">In addition to managing complexity, the MVC pattern makes it easier to test applications than it is to test a Web Forms-based ASP.NET Web application.</span></span> <span data-ttu-id="06eed-142">Par exemple, dans une application Web ASP.NET basée sur des Web Forms, une classe unique est utilisée pour afficher la sortie et répondre aux entrées utilisateur.</span><span class="sxs-lookup"><span data-stu-id="06eed-142">For example, in a Web Forms-based ASP.NET Web application, a single class is used both to display output and to respond to user input.</span></span> <span data-ttu-id="06eed-143">Écriture de tests automatisés pour les applications ASP.NET basées sur des Web Forms peut être complexe, car pour tester une page individuelle, vous devez instancier la classe de page, tous ses contrôles enfants et les autres classes dépendantes dans l’application.</span><span class="sxs-lookup"><span data-stu-id="06eed-143">Writing automated tests for Web Forms-based ASP.NET applications can be complex, because to test an individual page, you must instantiate the page class, all its child controls, and additional dependent classes in the application.</span></span> <span data-ttu-id="06eed-144">Étant donné que c’est le cas de nombreuses classes sont instanciées pour exécuter la page, il peut être difficile d’écrire des tests centrés exclusivement sur des parties individuelles de l’application.</span><span class="sxs-lookup"><span data-stu-id="06eed-144">Because so many classes are instantiated to run the page, it can be hard to write tests that focus exclusively on individual parts of the application.</span></span> <span data-ttu-id="06eed-145">Tests pour les applications ASP.NET basées sur des Web Forms peuvent donc être plus difficiles à implémenter que les tests dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="06eed-145">Tests for Web Forms-based ASP.NET applications can therefore be more difficult to implement than tests in an MVC application.</span></span> <span data-ttu-id="06eed-146">En outre, les tests dans une application ASP.NET basée sur des Web Forms requièrent un serveur Web.</span><span class="sxs-lookup"><span data-stu-id="06eed-146">Moreover, tests in a Web Forms-based ASP.NET application require a Web server.</span></span> <span data-ttu-id="06eed-147">L’infrastructure MVC dissocie les composants et une utilisation intensive des interfaces, ce qui rend possible de tester des composants individuels en les isolant du reste de l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="06eed-147">The MVC framework decouples the components and makes heavy use of interfaces, which makes it possible to test individual components in isolation from the rest of the framework.</span></span>   
  
<span data-ttu-id="06eed-148">Le couplage lâche entre les trois principaux composants d’une application MVC favorise également le développement en parallèle.</span><span class="sxs-lookup"><span data-stu-id="06eed-148">The loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="06eed-149">Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur et un troisième peut se concentrer sur la logique métier dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="06eed-149">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

## <a name="deciding-when-to-create-an-mvc-application"></a><span data-ttu-id="06eed-150">Quand faut-il créer une Application MVC</span><span class="sxs-lookup"><span data-stu-id="06eed-150">Deciding When to Create an MVC Application</span></span>

<span data-ttu-id="06eed-151">Vous devez déterminer avec soin s’il faut implémenter une application Web à l’aide de l’infrastructure ASP.NET MVC ou le modèle Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06eed-151">You must consider carefully whether to implement a Web application by using either the ASP.NET MVC framework or the ASP.NET Web Forms model.</span></span> <span data-ttu-id="06eed-152">L’infrastructure MVC ne remplace pas le modèle Web Forms ; Vous pouvez utiliser un framework pour les applications Web.</span><span class="sxs-lookup"><span data-stu-id="06eed-152">The MVC framework does not replace the Web Forms model; you can use either framework for Web applications.</span></span> <span data-ttu-id="06eed-153">(Si vous avez des applications Web Forms existantes, ils continuent de fonctionner exactement comme ils ont toujours.)</span><span class="sxs-lookup"><span data-stu-id="06eed-153">(If you have existing Web Forms-based applications, these continue to work exactly as they always have.)</span></span>   
  
<span data-ttu-id="06eed-154">Avant de décider d’utiliser l’infrastructure MVC ou le modèle Web Forms pour un site Web spécifique, évaluez les avantages de chaque approche.</span><span class="sxs-lookup"><span data-stu-id="06eed-154">Before you decide to use the MVC framework or the Web Forms model for a specific Web site, weigh the advantages of each approach.</span></span>

### <a name="advantages-of-an-mvc-based-web-application"></a><span data-ttu-id="06eed-155">Avantages d’une Application Web basée sur MVC</span><span class="sxs-lookup"><span data-stu-id="06eed-155">Advantages of an MVC-Based Web Application</span></span>

<span data-ttu-id="06eed-156">L’infrastructure ASP.NET MVC offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="06eed-156">The ASP.NET MVC framework offers the following advantages:</span></span>

- <span data-ttu-id="06eed-157">Il est plus facile à gérer la complexité en décomposant une application dans le modèle, la vue et le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="06eed-157">It makes it easier to manage complexity by dividing an application into the model, the view, and the controller.</span></span>
- <span data-ttu-id="06eed-158">Il n’utilise pas l’état d’affichage ou de formulaires basés sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="06eed-158">It does not use view state or server-based forms.</span></span> <span data-ttu-id="06eed-159">L’infrastructure MVC est donc idéal pour les développeurs qui veulent un contrôle total sur le comportement d’une application.</span><span class="sxs-lookup"><span data-stu-id="06eed-159">This makes the MVC framework ideal for developers who want full control over the behavior of an application.</span></span>
- <span data-ttu-id="06eed-160">Il utilise un modèle de contrôleur frontal qui traite les demandes d’application Web via un seul contrôleur.</span><span class="sxs-lookup"><span data-stu-id="06eed-160">It uses a Front Controller pattern that processes Web application requests through a single controller.</span></span> <span data-ttu-id="06eed-161">Cela vous permet de concevoir une application qui prend en charge une riche infrastructure de routage.</span><span class="sxs-lookup"><span data-stu-id="06eed-161">This enables you to design an application that supports a rich routing infrastructure.</span></span> <span data-ttu-id="06eed-162">Pour plus d’informations, consultez [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") sur le site Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="06eed-162">For more information, see [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") on the MSDN Web site.</span></span>
- <span data-ttu-id="06eed-163">Il fournit une meilleure prise en charge pour le développement piloté par tests (TDD).</span><span class="sxs-lookup"><span data-stu-id="06eed-163">It provides better support for test-driven development (TDD).</span></span>
- <span data-ttu-id="06eed-164">Il fonctionne bien pour les applications Web qui sont pris en charge par de grandes équipes de développeurs et concepteurs Web qui ont besoin d’un degré élevé de contrôle sur le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="06eed-164">It works well for Web applications that are supported by large teams of developers and Web designers who need a high degree of control over the application behavior.</span></span>

### <a name="advantages-of-a-web-forms-based-web-application"></a><span data-ttu-id="06eed-165">Avantages d’une Application Web basée sur les formulaires de Web</span><span class="sxs-lookup"><span data-stu-id="06eed-165">Advantages of a Web Forms-Based Web Application</span></span>

<span data-ttu-id="06eed-166">L’infrastructure basée sur des Web Forms offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="06eed-166">The Web Forms-based framework offers the following advantages:</span></span>

- <span data-ttu-id="06eed-167">Il prend en charge un modèle d’événement qui conserve l’état via HTTP, ce qui améliore le développement d’applications Web line-of-business.</span><span class="sxs-lookup"><span data-stu-id="06eed-167">It supports an event model that preserves state over HTTP, which benefits line-of-business Web application development.</span></span> <span data-ttu-id="06eed-168">L’application basée sur des Web Forms fournit des dizaines d’événements qui sont pris en charge dans des centaines de contrôles serveur.</span><span class="sxs-lookup"><span data-stu-id="06eed-168">The Web Forms-based application provides dozens of events that are supported in hundreds of server controls.</span></span>
- <span data-ttu-id="06eed-169">Elle utilise un modèle de contrôleur de Page qui ajoute des fonctionnalités à des pages individuelles.</span><span class="sxs-lookup"><span data-stu-id="06eed-169">It uses a Page Controller pattern that adds functionality to individual pages.</span></span> <span data-ttu-id="06eed-170">Pour plus d’informations, consultez [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") sur le site Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="06eed-170">For more information, see [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") on the MSDN Web site.</span></span>
- <span data-ttu-id="06eed-171">Elle utilise l’état d’affichage ou les formulaires de serveur, qui peuvent faciliter la gestion des informations d’état.</span><span class="sxs-lookup"><span data-stu-id="06eed-171">It uses view state or server-based forms, which can make managing state information easier.</span></span>
- <span data-ttu-id="06eed-172">Il fonctionne bien pour les petites équipes de développeurs et concepteurs qui souhaitent tirer parti du grand nombre de composants disponibles pour le développement rapide d’applications Web.</span><span class="sxs-lookup"><span data-stu-id="06eed-172">It works well for small teams of Web developers and designers who want to take advantage of the large number of components available for rapid application development.</span></span>
- <span data-ttu-id="06eed-173">En général, il est moins complexe pour le développement d’applications, car les composants (la **Page** classe, les contrôles, etc.) sont étroitement intégrés et requièrent habituellement moins de code que le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="06eed-173">In general, it is less complex for application development, because the components (the **Page** class, controls, and so on) are tightly integrated and usually require less code than the MVC model.</span></span>

## <a name="features-of-the-aspnet-mvc-framework"></a><span data-ttu-id="06eed-174">Fonctionnalités de l’infrastructure ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="06eed-174">Features of the ASP.NET MVC Framework</span></span>

<span data-ttu-id="06eed-175">L’infrastructure ASP.NET MVC fournit les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="06eed-175">The ASP.NET MVC framework provides the following features:</span></span>

- <span data-ttu-id="06eed-176">Séparation des tâches de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), testabilité et développement piloté par tests (TDD) par défaut.</span><span class="sxs-lookup"><span data-stu-id="06eed-176">Separation of application tasks (input logic, business logic, and UI logic), testability, and test-driven development (TDD) by default.</span></span> <span data-ttu-id="06eed-177">Tous les contrats de base de l’infrastructure MVC sont basés sur une interface et peuvent être testés à l’aide d’objets fictifs, qui sont des objets simulés qui simulent le comportement d’objets réels de l’application.</span><span class="sxs-lookup"><span data-stu-id="06eed-177">All core contracts in the MVC framework are interface-based and can be tested by using mock objects, which are simulated objects that imitate the behavior of actual objects in the application.</span></span> <span data-ttu-id="06eed-178">Vous pouvez les effectuer l’application sans avoir à exécuter les contrôleurs dans un processus ASP.NET, ce qui rend les tests unitaires rapide et flexible.</span><span class="sxs-lookup"><span data-stu-id="06eed-178">You can unit-test the application without having to run the controllers in an ASP.NET process, which makes unit testing fast and flexible.</span></span> <span data-ttu-id="06eed-179">Vous pouvez utiliser toute infrastructure de test unitaire qui est compatible avec le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="06eed-179">You can use any unit-testing framework that is compatible with the .NET Framework.</span></span>
- <span data-ttu-id="06eed-180">Une infrastructure extensible et enfichable.</span><span class="sxs-lookup"><span data-stu-id="06eed-180">An extensible and pluggable framework.</span></span> <span data-ttu-id="06eed-181">Les composants de l’infrastructure ASP.NET MVC sont conçus afin que peut être facilement remplacées ou personnalisés.</span><span class="sxs-lookup"><span data-stu-id="06eed-181">The components of the ASP.NET MVC framework are designed so that they can be easily replaced or customized.</span></span> <span data-ttu-id="06eed-182">Vous pouvez incorporer dans votre propre moteur d’affichage, de stratégie de routage d’URL, de sérialisation de paramètre de méthode d’action et d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="06eed-182">You can plug in your own view engine, URL routing policy, action-method parameter serialization, and other components.</span></span> <span data-ttu-id="06eed-183">L’infrastructure ASP.NET MVC prend également en charge l’utilisation des modèles de conteneur d’Injection de dépendance (DI) et Inversion de contrôle (IOC).</span><span class="sxs-lookup"><span data-stu-id="06eed-183">The ASP.NET MVC framework also supports the use of Dependency Injection (DI) and Inversion of Control (IOC) container models.</span></span> <span data-ttu-id="06eed-184">L’injection de dépendance vous permet d’injecter des objets dans une classe, au lieu d’utiliser la classe pour créer l’objet lui-même.</span><span class="sxs-lookup"><span data-stu-id="06eed-184">DI allows you to inject objects into a class, instead of relying on the class to create the object itself.</span></span> <span data-ttu-id="06eed-185">Inversion de contrôle spécifie que si un objet requiert un autre objet, le premier objet doit obtenir le deuxième objet à partir d’une source externe, tel qu’un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="06eed-185">IOC specifies that if an object requires another object, the first objects should get the second object from an outside source such as a configuration file.</span></span> <span data-ttu-id="06eed-186">Cela facilite les tests.</span><span class="sxs-lookup"><span data-stu-id="06eed-186">This makes testing easier.</span></span>
- <span data-ttu-id="06eed-187">Un composant de mappage d’URL puissant qui vous permet de créer des applications ayant des URLs compréhensibles et découvrables.</span><span class="sxs-lookup"><span data-stu-id="06eed-187">A powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="06eed-188">URL est inutile d’inclure des extensions de nom de fichier et sont conçues pour prendre en charge les modèles d’affectation de noms d’URL qui fonctionnent bien pour la recherche du moteur d’optimisation (SEO) et transfert d’état representational (REST) d’adressage.</span><span class="sxs-lookup"><span data-stu-id="06eed-188">URLs do not have to include file-name extensions, and are designed to support URL naming patterns that work well for search engine optimization (SEO) and representational state transfer (REST) addressing.</span></span>
- <span data-ttu-id="06eed-189">Prise en charge à l’aide de la balise dans la page ASP.NET existante (fichiers .aspx), le contrôle utilisateur (fichiers .ascx) et fichiers de balisage (fichiers .master) page maître en tant que modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="06eed-189">Support for using the markup in existing ASP.NET page (.aspx files), user control (.ascx files), and master page (.master files) markup files as view templates.</span></span> <span data-ttu-id="06eed-190">Vous pouvez utiliser les fonctionnalités ASP.NET existantes avec l’infrastructure ASP.NET MVC, telles que les pages maîtres imbriquées, les expressions en ligne (&lt;% = %&gt;), les contrôles serveur déclaratifs, les modèles, liaison de données, localisation et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="06eed-190">You can use existing ASP.NET features with the ASP.NET MVC framework, such as nested master pages, in-line expressions (&lt;%= %&gt;), declarative server controls, templates, data-binding, localization, and so on.</span></span>
- <span data-ttu-id="06eed-191">Prise en charge pour les fonctionnalités ASP.NET existantes.</span><span class="sxs-lookup"><span data-stu-id="06eed-191">Support for existing ASP.NET features.</span></span> <span data-ttu-id="06eed-192">ASP.NET MVC vous permet d’utiliser des fonctionnalités telles que l’authentification par formulaire et l’authentification Windows, l’autorisation d’URL, l’appartenance et rôles, sortie et la mise en cache des données, gestion d’état de session et de profil, l’analyse du fonctionnement, le système de configuration et le fournisseur architecture.</span><span class="sxs-lookup"><span data-stu-id="06eed-192">ASP.NET MVC lets you use features such as forms authentication and Windows authentication, URL authorization, membership and roles, output and data caching, session and profile state management, health monitoring, the configuration system, and the provider architecture.</span></span>