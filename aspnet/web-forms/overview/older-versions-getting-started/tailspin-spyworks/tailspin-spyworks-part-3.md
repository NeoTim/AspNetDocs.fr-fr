---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Partie 3 : Mise en page et Menu catégorie | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. La partie 3 explique Ajout de mise en page et un menu de la catégorie.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: f55b29a271dbdb72d3e2249ed74517b77d78cf5e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034846"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="d8c95-104">Partie 3 : Mise en page et menu Catégorie</span><span class="sxs-lookup"><span data-stu-id="d8c95-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="d8c95-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d8c95-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d8c95-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="d8c95-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d8c95-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="d8c95-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d8c95-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="d8c95-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d8c95-109">La partie 3 explique Ajout de mise en page et un menu de la catégorie.</span><span class="sxs-lookup"><span data-stu-id="d8c95-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="d8c95-110">Ajout d’une mise en page et un Menu catégorie</span><span class="sxs-lookup"><span data-stu-id="d8c95-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="d8c95-111">Dans notre page maître du site, nous allons ajouter une balise div pour la colonne de gauche qui contient notre menu catégorie de produit.</span><span class="sxs-lookup"><span data-stu-id="d8c95-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="d8c95-112">Notez que l’alignement souhaité et autres mises en forme seront fournies par la classe CSS que nous avons ajouté à notre fichier Style.css.</span><span class="sxs-lookup"><span data-stu-id="d8c95-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="d8c95-113">Le menu de catégorie de produit sera être créé dynamiquement lors de l’exécution en interrogeant la base de données de Commerce pour existant catégories de produits et à créer les éléments de menu et correspondant lie.</span><span class="sxs-lookup"><span data-stu-id="d8c95-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="d8c95-114">Pour ce faire, nous allons utiliser deux des ASP. Contrôles de données puissantes de NET.</span><span class="sxs-lookup"><span data-stu-id="d8c95-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="d8c95-115">Le contrôle de « Source de données d’entité » et le contrôle « ListView ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="d8c95-116">Nous allons passer en « Mode Design » et utiliser les programmes d’assistance pour configurer nos contrôles.</span><span class="sxs-lookup"><span data-stu-id="d8c95-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="d8c95-117">Nous allons définir la propriété d’ID de contrôle EntityDataSource à EDS\_catégorie\_Menu et cliquez sur « Configurer la Source de données ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="d8c95-118">Sélectionnez la connexion CommerceEntities qui a été créé pour nous lorsque nous avons créé le modèle de Source de données Entity pour notre base de données de Commerce, puis cliquez sur « Suivant ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="d8c95-119">Sélectionnez le nom de jeu d’entités de « Catégories » et laisser le reste des options comme valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="d8c95-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="d8c95-120">Cliquez sur « Terminer ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-120">Click "Finish".</span></span>

<span data-ttu-id="d8c95-121">Maintenant nous allons définir la propriété d’ID de l’instance de contrôle ListView que nous avons placé sur notre page de ListView\_ProductsMenu et activer ses d’assistance.</span><span class="sxs-lookup"><span data-stu-id="d8c95-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="d8c95-122">Bien que nous pourrions utiliser des options de contrôle pour mettre en forme l’affichage d’élément de données et mise en forme, notre la création du menu uniquement nécessiteront un balisage simple nous sera de saisir le code dans la vue de source.</span><span class="sxs-lookup"><span data-stu-id="d8c95-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="d8c95-123">Notez l’instruction « Eval » : &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="d8c95-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="d8c95-124">La syntaxe ASP.NET &lt;% # %&gt; est une convention de raccourci qui indique à l’exécution pour exécuter tout ce qui est contenu dans et générer les résultats « en ligne ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="d8c95-125">L’instruction Eval("CategoryName") indique que, pour l’entrée actuelle dans la collection liée d’éléments de données, extraire la valeur de noms d’élément de modèle d’entité « CatagoryName ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="d8c95-126">Il s’agit d’une syntaxe concise pour une fonctionnalité très puissante.</span><span class="sxs-lookup"><span data-stu-id="d8c95-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="d8c95-127">Permet d’exécuter l’application maintenant.</span><span class="sxs-lookup"><span data-stu-id="d8c95-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="d8c95-128">Notez que notre menu catégorie de produit s’affiche désormais lorsque nous déplacez le pointeur sur les catégorie d’éléments de menu nous pouvons voir les points de lien d’élément menu à une page, nous devons encore implémenter nommé ProductsList.aspx et que nous avons créé un argument de chaîne de requête dynamique qui contient le  id de catégorie.</span><span class="sxs-lookup"><span data-stu-id="d8c95-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8c95-129">[Précédent](tailspin-spyworks-part-2.md)
> [Suivant](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d8c95-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
