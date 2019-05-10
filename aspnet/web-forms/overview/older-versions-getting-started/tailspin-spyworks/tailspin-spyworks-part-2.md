---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Partie 2 : Couche d’accès aux données | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 2 couvre l’ajout de la couche d’accès aux données.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130615"
---
# <a name="part-2-data-access-layer"></a><span data-ttu-id="e146d-104">Partie 2 : Couche d'accès aux données</span><span class="sxs-lookup"><span data-stu-id="e146d-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="e146d-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e146d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e146d-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="e146d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e146d-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="e146d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e146d-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e146d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e146d-109">Partie 2 couvre l’ajout de la couche d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="e146d-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a>  <span data-ttu-id="e146d-110">Ajout de la couche d’accès aux données</span><span class="sxs-lookup"><span data-stu-id="e146d-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="e146d-111">Notre application de commerce électronique dépend de deux bases de données.</span><span class="sxs-lookup"><span data-stu-id="e146d-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="e146d-112">Pour plus d’informations client, nous allons utiliser la base de données d’appartenance ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="e146d-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="e146d-113">Pour notre catalogue de produit et de panier d’achat nous allons implémenter une base de données SQL Express comme suit.</span><span class="sxs-lookup"><span data-stu-id="e146d-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="e146d-114">Après avoir créé la base de données (Commerce.mdf) dans application l’application\_dossier de données que nous pouvons passer à la création de notre couche d’accès aux données à l’aide de l’entité de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e146d-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="e146d-115">Nous allons créer un dossier nommé « données\_accès » et cliquez avec le bouton droit sur ce dossier et sélectionnez « Ajouter un nouvel élément ».</span><span class="sxs-lookup"><span data-stu-id="e146d-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="e146d-116">Dans « Modèles installés » élément et puis sélectionnez « ADO.NET Entity Data Model » Entrez EDM\_Commerce.edmx comme nom et cliquez sur le bouton « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="e146d-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="e146d-117">Choisissez « Générer à partir de la base de données ».</span><span class="sxs-lookup"><span data-stu-id="e146d-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="e146d-118">Enregistrez et générez.</span><span class="sxs-lookup"><span data-stu-id="e146d-118">Save and build.</span></span>

<span data-ttu-id="e146d-119">Nous sommes désormais prêts à ajouter notre première fonctionnalité – un menu de catégorie de produit.</span><span class="sxs-lookup"><span data-stu-id="e146d-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e146d-120">[Précédent](tailspin-spyworks-part-1.md)
> [Suivant](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="e146d-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
