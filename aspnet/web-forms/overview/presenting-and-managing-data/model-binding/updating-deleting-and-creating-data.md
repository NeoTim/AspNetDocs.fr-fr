---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Mise à jour, suppression et création de données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586645"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="9af87-104">Mise à jour, suppression et création de données avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="9af87-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="9af87-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9af87-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9af87-106">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9af87-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="9af87-107">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="9af87-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="9af87-108">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="9af87-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="9af87-109">Ce didacticiel montre comment créer, mettre à jour et supprimer des données avec la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9af87-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="9af87-110">Vous allez définir les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="9af87-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="9af87-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="9af87-111">DeleteMethod</span></span>
> - <span data-ttu-id="9af87-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="9af87-112">InsertMethod</span></span>
> - <span data-ttu-id="9af87-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="9af87-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="9af87-114">Ces propriétés reçoivent le nom de la méthode qui gère l’opération correspondante.</span><span class="sxs-lookup"><span data-stu-id="9af87-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="9af87-115">Dans cette méthode, vous fournissez la logique permettant d’interagir avec les données.</span><span class="sxs-lookup"><span data-stu-id="9af87-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="9af87-116">Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="9af87-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="9af87-117">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB.</span><span class="sxs-lookup"><span data-stu-id="9af87-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="9af87-118">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9af87-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="9af87-119">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="9af87-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="9af87-120">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="9af87-120">What you'll build</span></span>

<span data-ttu-id="9af87-121">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="9af87-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="9af87-122">Ajouter des modèles de données dynamiques</span><span class="sxs-lookup"><span data-stu-id="9af87-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="9af87-123">Activer la mise à jour et la suppression des données par le biais de méthodes de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="9af87-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="9af87-124">Appliquer des règles de validation des données-activer la création d’un enregistrement dans la base de données</span><span class="sxs-lookup"><span data-stu-id="9af87-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="9af87-125">Ajouter des modèles de données dynamiques</span><span class="sxs-lookup"><span data-stu-id="9af87-125">Add dynamic data templates</span></span>

<span data-ttu-id="9af87-126">Pour offrir la meilleure expérience utilisateur et réduire la répétition du code, vous allez utiliser des modèles de données dynamiques.</span><span class="sxs-lookup"><span data-stu-id="9af87-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="9af87-127">Vous pouvez facilement intégrer des modèles de données dynamiques prédéfinis dans votre site existant en installant un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9af87-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="9af87-128">Dans **gérer les packages NuGet**, installez **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="9af87-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modèles Dynamic Data](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="9af87-130">Notez que votre projet comprend maintenant un dossier nommé **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="9af87-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="9af87-131">Dans ce dossier, vous trouverez les modèles qui sont appliqués automatiquement aux contrôles dynamiques dans vos Web Forms.</span><span class="sxs-lookup"><span data-stu-id="9af87-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![dossier Dynamic Data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="9af87-133">Activer la mise à jour et la suppression</span><span class="sxs-lookup"><span data-stu-id="9af87-133">Enable updating and deleting</span></span>

<span data-ttu-id="9af87-134">La possibilité pour les utilisateurs de mettre à jour et de supprimer des enregistrements dans la base de données est très similaire au processus de récupération des données.</span><span class="sxs-lookup"><span data-stu-id="9af87-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="9af87-135">Dans les propriétés **UpdateMethod** et **DeleteMethod** , vous spécifiez les noms des méthodes qui effectuent ces opérations.</span><span class="sxs-lookup"><span data-stu-id="9af87-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="9af87-136">Avec un contrôle GridView, vous pouvez également spécifier la génération automatique des boutons modifier et supprimer.</span><span class="sxs-lookup"><span data-stu-id="9af87-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="9af87-137">Le code en surbrillance suivant montre les ajouts apportés au code GridView.</span><span class="sxs-lookup"><span data-stu-id="9af87-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="9af87-138">Dans le fichier code-behind, ajoutez une instruction using pour **System. Data. Entity. infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="9af87-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="9af87-139">Ensuite, ajoutez les méthodes Update et Delete suivantes.</span><span class="sxs-lookup"><span data-stu-id="9af87-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="9af87-140">La méthode **TryUpdateModel** applique les valeurs liées aux données correspondantes du formulaire Web à l’élément de données.</span><span class="sxs-lookup"><span data-stu-id="9af87-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="9af87-141">L’élément de données est récupéré en fonction de la valeur du paramètre ID.</span><span class="sxs-lookup"><span data-stu-id="9af87-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="9af87-142">Appliquer les exigences de validation</span><span class="sxs-lookup"><span data-stu-id="9af87-142">Enforce validation requirements</span></span>

<span data-ttu-id="9af87-143">Les attributs de validation que vous avez appliqués aux propriétés FirstName, LastName et Year de la classe Student sont automatiquement appliqués lors de la mise à jour des données.</span><span class="sxs-lookup"><span data-stu-id="9af87-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="9af87-144">Les contrôles DynamicField ajoutent des validateurs de client et de serveur basés sur les attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="9af87-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="9af87-145">Les propriétés FirstName et LastName sont toutes deux requises.</span><span class="sxs-lookup"><span data-stu-id="9af87-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="9af87-146">FirstName ne peut pas dépasser 20 caractères et LastName ne peut pas dépasser 40 caractères.</span><span class="sxs-lookup"><span data-stu-id="9af87-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="9af87-147">L’année doit être une valeur valide pour l’énumération AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="9af87-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="9af87-148">Si l’utilisateur ne respecte pas l’une des exigences de validation, la mise à jour ne se poursuit pas.</span><span class="sxs-lookup"><span data-stu-id="9af87-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="9af87-149">Pour afficher le message d’erreur, ajoutez un contrôle ValidationSummary au-dessus de GridView.</span><span class="sxs-lookup"><span data-stu-id="9af87-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="9af87-150">Pour afficher les erreurs de validation de la liaison de modèle, affectez la valeur **true**à la propriété **ShowModelStateErrors** .</span><span class="sxs-lookup"><span data-stu-id="9af87-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="9af87-151">Exécutez l’application Web, puis mettez à jour et supprimez tous les enregistrements.</span><span class="sxs-lookup"><span data-stu-id="9af87-151">Run the web application, and update and delete any of the records.</span></span>

![mettre à jour les données](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="9af87-153">Notez que dans le mode d’édition, la valeur de la propriété Year est automatiquement rendue sous la forme d’une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="9af87-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="9af87-154">La propriété Year est une valeur d’énumération, et le modèle Dynamic Data pour une valeur d’énumération spécifie une liste déroulante pour la modification.</span><span class="sxs-lookup"><span data-stu-id="9af87-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="9af87-155">Vous pouvez trouver ce modèle en ouvrant l' **énumération\_fichier Edit. ascx** dans le dossier **DynamicData**/**FieldTemplates** .</span><span class="sxs-lookup"><span data-stu-id="9af87-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="9af87-156">Si vous fournissez des valeurs valides, la mise à jour se termine correctement.</span><span class="sxs-lookup"><span data-stu-id="9af87-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="9af87-157">Si vous ne respectez pas l’une des exigences de validation, la mise à jour ne se poursuit pas et un message d’erreur s’affiche au-dessus de la grille.</span><span class="sxs-lookup"><span data-stu-id="9af87-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![message d’erreur](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="9af87-159">Ajouter de nouveaux enregistrements</span><span class="sxs-lookup"><span data-stu-id="9af87-159">Add new records</span></span>

<span data-ttu-id="9af87-160">Le contrôle GridView n’inclut pas la propriété **InsertMethod** et ne peut donc pas être utilisé pour ajouter un nouvel enregistrement avec une liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9af87-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="9af87-161">Vous pouvez trouver la propriété InsertMethod dans les contrôles **FormView**, **DetailsView**ou **ListView** .</span><span class="sxs-lookup"><span data-stu-id="9af87-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="9af87-162">Dans ce didacticiel, vous allez utiliser un contrôle FormView pour ajouter un nouvel enregistrement.</span><span class="sxs-lookup"><span data-stu-id="9af87-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="9af87-163">Tout d’abord, ajoutez un lien vers la nouvelle page que vous allez créer pour ajouter un nouvel enregistrement.</span><span class="sxs-lookup"><span data-stu-id="9af87-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="9af87-164">Au-dessus du ValidationSummary, ajoutez :</span><span class="sxs-lookup"><span data-stu-id="9af87-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="9af87-165">Le nouveau lien s’affiche en haut du contenu de la page des étudiants.</span><span class="sxs-lookup"><span data-stu-id="9af87-165">The new link will appear at the top of the content for the Students page.</span></span>

![nouveau lien](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="9af87-167">Ajoutez ensuite un nouveau formulaire Web à l’aide d’une page maître, puis nommez-le **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="9af87-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="9af87-168">Sélectionnez site. Master comme page maître.</span><span class="sxs-lookup"><span data-stu-id="9af87-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="9af87-169">Vous allez afficher les champs pour ajouter un nouvel étudiant à l’aide d’un contrôle **DynamicEntity** .</span><span class="sxs-lookup"><span data-stu-id="9af87-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="9af87-170">Le contrôle DynamicEntity restitue les propriétés modifiables dans la classe spécifiée dans la propriété ItemType.</span><span class="sxs-lookup"><span data-stu-id="9af87-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="9af87-171">La propriété StudentID a été marquée avec l’attribut **[ScaffoldColumn (false)]** de sorte qu’elle ne soit pas rendue.</span><span class="sxs-lookup"><span data-stu-id="9af87-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="9af87-172">Dans l’espace réservé MainContent de la page AddStudent, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="9af87-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="9af87-173">Dans le fichier code-behind (AddStudent.aspx.cs), ajoutez une instruction **using** pour l’espace de noms **ContosoUniversityModelBinding. Models** .</span><span class="sxs-lookup"><span data-stu-id="9af87-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="9af87-174">Ensuite, ajoutez les méthodes suivantes pour spécifier comment insérer un nouvel enregistrement et un gestionnaire d’événements pour le bouton Annuler.</span><span class="sxs-lookup"><span data-stu-id="9af87-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="9af87-175">Enregistrez toutes les modifications.</span><span class="sxs-lookup"><span data-stu-id="9af87-175">Save all of the changes.</span></span>

<span data-ttu-id="9af87-176">Exécutez l’application Web et créez un nouveau Student.</span><span class="sxs-lookup"><span data-stu-id="9af87-176">Run the web application and create a new student.</span></span>

![Ajouter un nouvel étudiant](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="9af87-178">Cliquez sur **Insérer** et notez que le nouvel étudiant a été créé.</span><span class="sxs-lookup"><span data-stu-id="9af87-178">Click **Insert** and notice the new student has been created.</span></span>

![afficher un nouvel étudiant](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="9af87-180">Conclusion</span><span class="sxs-lookup"><span data-stu-id="9af87-180">Conclusion</span></span>

<span data-ttu-id="9af87-181">Dans ce didacticiel, vous avez activé la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="9af87-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="9af87-182">Vous avez vérifié que les règles de validation sont appliquées lors de l’interaction avec les données.</span><span class="sxs-lookup"><span data-stu-id="9af87-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="9af87-183">Dans le [didacticiel](sorting-paging-and-filtering-data.md) suivant de cette série, vous allez activer le tri, la pagination et le filtrage des données.</span><span class="sxs-lookup"><span data-stu-id="9af87-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9af87-184">[Précédent](retrieving-data.md)
> [Suivant](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="9af87-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
