---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Relation contenant-contenu dans OData v4 à l’aide de Web API 2.2 | Microsoft Docs
author: rick-anderson
description: En règle générale, une entité est uniquement accessible si elle a été encapsulée dans un jeu d’entités. Mais OData v4 fournit deux options supplémentaires, Singleton et Con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: fed55a4bf01e82af5167018f03e28a6274fcda78
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59382197"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="94e90-104">Relation contenant-contenu dans OData v4 à l’aide de Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="94e90-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="94e90-105">by Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="94e90-105">by Jinfu Tan</span></span>

> <span data-ttu-id="94e90-106">En règle générale, une entité est uniquement accessible si elle a été encapsulée dans un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="94e90-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="94e90-107">Mais OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, les deux API Web 2.2 prend en charge.</span><span class="sxs-lookup"><span data-stu-id="94e90-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="94e90-108">Cette rubrique montre comment définir une relation contenant-contenu dans un point de terminaison OData dans API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="94e90-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="94e90-109">Pour plus d’informations sur la relation contenant-contenu, consultez [relation contenant-contenu sera disponible avec OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="94e90-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="94e90-110">Pour créer un point de terminaison OData V4 dans l’API Web, consultez [créer une Using ASP.NET Web API 2.2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="94e90-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="94e90-111">Tout d’abord, nous allons créer un modèle de domaine de relation contenant-contenu dans le service OData, à l’aide de ce modèle de données :</span><span class="sxs-lookup"><span data-stu-id="94e90-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modèle de données](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="94e90-113">Un compte contient de nombreux PaymentInstruments (PI), mais nous ne définissons une jeu d’entités pour un PI.</span><span class="sxs-lookup"><span data-stu-id="94e90-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="94e90-114">Au lieu de cela, les instructions de traitement sont uniquement accessibles via un compte.</span><span class="sxs-lookup"><span data-stu-id="94e90-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="94e90-115">Vous pouvez télécharger la solution utilisée dans cette rubrique à partir de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="94e90-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="94e90-116">Définition du modèle de données</span><span class="sxs-lookup"><span data-stu-id="94e90-116">Defining the data model</span></span>

1. <span data-ttu-id="94e90-117">Définir les types CLR.</span><span class="sxs-lookup"><span data-stu-id="94e90-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="94e90-118">Le `Contained` attribut est utilisé pour les propriétés de navigation de relation contenant-contenu.</span><span class="sxs-lookup"><span data-stu-id="94e90-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="94e90-119">Générer le modèle EDM basé sur les types CLR.</span><span class="sxs-lookup"><span data-stu-id="94e90-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="94e90-120">Le `ODataConventionModelBuilder` gérera la génération du modèle EDM, si le `Contained` attribut est ajouté à la propriété de navigation correspondante.</span><span class="sxs-lookup"><span data-stu-id="94e90-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="94e90-121">Si la propriété est un type de collection, un `GetCount(string NameContains)` fonction sera également créée.</span><span class="sxs-lookup"><span data-stu-id="94e90-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="94e90-122">Les métadonnées générées ressemblera à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="94e90-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="94e90-123">Le `ContainsTarget` attribut indique que la propriété de navigation est une relation contenant-contenu.</span><span class="sxs-lookup"><span data-stu-id="94e90-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="94e90-124">Définir le contrôleur de jeu d’entité contenant</span><span class="sxs-lookup"><span data-stu-id="94e90-124">Define the containing entity set controller</span></span>

<span data-ttu-id="94e90-125">Entités de relation contenant-contenues ne possèdent pas leur propres contrôleur ; l’action est définie dans le contrôleur de jeu d’entités contenant.</span><span class="sxs-lookup"><span data-stu-id="94e90-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="94e90-126">Dans cet exemple, il est un AccountsController, mais aucun PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="94e90-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="94e90-127">Si le chemin d’accès OData est 4 ou plusieurs segments, uniquement attribut fonctionnement du routage, tel que `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` dans le contrôleur ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="94e90-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="94e90-128">Dans le cas contraire, les attributs et le routage conventionnel fonctionne : par exemple, `GetPayInPIs(int key)` correspond à `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="94e90-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="94e90-129">*Merci à Leo Hu pour le contenu d’origine de cet article.*</span><span class="sxs-lookup"><span data-stu-id="94e90-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
