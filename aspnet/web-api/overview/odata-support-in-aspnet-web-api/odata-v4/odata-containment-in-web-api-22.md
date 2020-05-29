---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Relation contenant-contenu dans OData v4 à l’aide de l’API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités. Mais OData v4 fournit deux options supplémentaires, Singleton et con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3be81eac9de4686a0d187396e951b121ea65bac4
ms.sourcegitcommit: a4c3c7e04e5f53cf8cd334f036d324976b78d154
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84173002"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="52418-104">Relation contenant-contenu dans OData v4 à l’aide de l’API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="52418-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="52418-105">par Jinfu brun</span><span class="sxs-lookup"><span data-stu-id="52418-105">by Jinfu Tan</span></span>

> <span data-ttu-id="52418-106">Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="52418-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="52418-107">Toutefois, OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, que WebAPI 2,2 prend en charge.</span><span class="sxs-lookup"><span data-stu-id="52418-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="52418-108">Cette rubrique montre comment définir une relation contenant-contenu dans un point de terminaison OData dans WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="52418-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="52418-109">Pour plus d’informations sur la relation contenant-contenu, consultez [relation contenant-contenu dans OData v4](https://devblogs.microsoft.com/odata/tutorial-sample-containment-is-coming-with-odata-v4/).</span><span class="sxs-lookup"><span data-stu-id="52418-109">For more information about containment, see [Containment is coming with OData v4](https://devblogs.microsoft.com/odata/tutorial-sample-containment-is-coming-with-odata-v4/).</span></span> <span data-ttu-id="52418-110">Pour créer un point de terminaison OData v4 dans l’API Web, consultez [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="52418-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="52418-111">Tout d’abord, nous allons créer un modèle de domaine de relation contenant-contenu dans le service OData, à l’aide de ce modèle de données :</span><span class="sxs-lookup"><span data-stu-id="52418-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modèle de données](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="52418-113">Un compte contient de nombreux PaymentInstruments (PI), mais nous ne définissons pas de jeu d’entités pour un PI.</span><span class="sxs-lookup"><span data-stu-id="52418-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="52418-114">Au lieu de cela, les instructions de l’utilisateur sont accessibles uniquement via un compte.</span><span class="sxs-lookup"><span data-stu-id="52418-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="52418-115">Vous pouvez télécharger la solution utilisée dans cette rubrique à partir de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="52418-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="52418-116">Définition du modèle de données</span><span class="sxs-lookup"><span data-stu-id="52418-116">Defining the data model</span></span>

1. <span data-ttu-id="52418-117">Définissez les types CLR.</span><span class="sxs-lookup"><span data-stu-id="52418-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="52418-118">L' `Contained` attribut est utilisé pour les propriétés de navigation de la relation contenant-contenu.</span><span class="sxs-lookup"><span data-stu-id="52418-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="52418-119">Générez le modèle EDM basé sur les types CLR.</span><span class="sxs-lookup"><span data-stu-id="52418-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="52418-120">Le `ODataConventionModelBuilder` gère la génération du modèle EDM si l' `Contained` attribut est ajouté à la propriété de navigation correspondante.</span><span class="sxs-lookup"><span data-stu-id="52418-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="52418-121">Si la propriété est un type de collection, une `GetCount(string NameContains)` fonction est également créée.</span><span class="sxs-lookup"><span data-stu-id="52418-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="52418-122">Les métadonnées générées se présentent comme suit :</span><span class="sxs-lookup"><span data-stu-id="52418-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="52418-123">L' `ContainsTarget` attribut indique que la propriété de navigation est un conteneur.</span><span class="sxs-lookup"><span data-stu-id="52418-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="52418-124">Définir le contrôleur de jeu d’entités conteneur</span><span class="sxs-lookup"><span data-stu-id="52418-124">Define the containing entity set controller</span></span>

<span data-ttu-id="52418-125">Les entités contenues n’ont pas leur propre contrôleur ; l’action est définie dans le contrôleur de jeu d’entités conteneur.</span><span class="sxs-lookup"><span data-stu-id="52418-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="52418-126">Dans cet exemple, il existe un AccountsController, mais pas de PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="52418-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="52418-127">Si le chemin d’accès OData est de 4 segments ou plus, seul le routage des attributs fonctionne, par exemple `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` dans le contrôleur ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="52418-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="52418-128">Dans le cas contraire, l’attribut et le routage conventionnel fonctionnent : par exemple, `GetPayInPIs(int key)` correspond à `GET ~/Accounts(1)/PayinPIs` .</span><span class="sxs-lookup"><span data-stu-id="52418-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="52418-129">*Merci à Leo HU pour le contenu d’origine de cet article.*</span><span class="sxs-lookup"><span data-stu-id="52418-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
