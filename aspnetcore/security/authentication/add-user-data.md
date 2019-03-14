---
title: Ajouter, télécharger et supprimer des données utilisateur à l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées à l’identité dans un projet ASP.NET Core. Supprimer des données par RGPD.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 5465117e5db880e8298e6c2075a27699e4081894
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57057296"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="ed18e-104">Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed18e-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="ed18e-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed18e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed18e-106">Cet article explique comment :</span><span class="sxs-lookup"><span data-stu-id="ed18e-106">This article shows how to:</span></span>

* <span data-ttu-id="ed18e-107">Ajouter des données utilisateur personnalisées pour une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed18e-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="ed18e-108">Décorez le modèle de données utilisateur personnalisée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) par conséquent, il est automatiquement disponible pour téléchargement et la suppression d’attributs.</span><span class="sxs-lookup"><span data-stu-id="ed18e-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="ed18e-109">Rendre les données peuvent être téléchargés et supprimés à mieux répondre à [RGPD](xref:security/gdpr) configuration requise.</span><span class="sxs-lookup"><span data-stu-id="ed18e-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="ed18e-110">L’exemple de projet est créé à partir d’une application web de Pages Razor, mais les instructions sont similaires pour une application web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ed18e-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ed18e-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ed18e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed18e-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ed18e-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ed18e-113">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="ed18e-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed18e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed18e-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ed18e-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="ed18e-116">Nommez le projet **application Web 1** si vous souhaitez qu’il correspond à l’espace de noms de la [télécharger l’exemple](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span><span class="sxs-lookup"><span data-stu-id="ed18e-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="ed18e-117">Sélectionnez **Application Web ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="ed18e-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="ed18e-118">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="ed18e-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="ed18e-119">Sélectionnez **Application Web**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="ed18e-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="ed18e-120">Générez et exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="ed18e-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed18e-121">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed18e-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="ed18e-122">Exécuter le Générateur de modèles automatique identité</span><span class="sxs-lookup"><span data-stu-id="ed18e-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed18e-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed18e-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ed18e-124">À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="ed18e-125">Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="ed18e-126">Dans le **identité ADD** boîte de dialogue, les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed18e-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="ed18e-127">Sélectionnez le fichier de disposition existante *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ed18e-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="ed18e-128">Sélectionnez les fichiers suivants pour remplacer :</span><span class="sxs-lookup"><span data-stu-id="ed18e-128">Select the following files to override:</span></span>
    * <span data-ttu-id="ed18e-129">**Compte/inscrire**</span><span class="sxs-lookup"><span data-stu-id="ed18e-129">**Account/Register**</span></span>
    * <span data-ttu-id="ed18e-130">**Compte/gérer/Index**</span><span class="sxs-lookup"><span data-stu-id="ed18e-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="ed18e-131">Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="ed18e-132">Acceptez le type (**WebApp1.Models.WebApp1Context** si le projet est nommé **application Web 1**).</span><span class="sxs-lookup"><span data-stu-id="ed18e-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="ed18e-133">Sélectionnez le **+** bouton pour créer un nouveau **classe utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="ed18e-134">Acceptez le type (**WebApp1User** si le projet est nommé **application Web 1**) > **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="ed18e-135">Sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ed18e-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed18e-136">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed18e-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ed18e-137">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="ed18e-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="ed18e-138">Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au fichier projet (.csproj).</span><span class="sxs-lookup"><span data-stu-id="ed18e-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="ed18e-139">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ed18e-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="ed18e-140">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="ed18e-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="ed18e-141">Dans le dossier du projet, exécutez le Générateur de modèles automatique identité :</span><span class="sxs-lookup"><span data-stu-id="ed18e-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="ed18e-142">Suivez les instructions de [Migrations, UseAuthentication et disposition](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ed18e-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="ed18e-143">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="ed18e-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="ed18e-144">Ajoutez `UseAuthentication` à `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ed18e-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="ed18e-145">Ajouter `<partial name="_LoginPartial" />` pour le fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="ed18e-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="ed18e-146">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="ed18e-146">Test the app:</span></span>
  * <span data-ttu-id="ed18e-147">Inscrire un utilisateur</span><span class="sxs-lookup"><span data-stu-id="ed18e-147">Register a user</span></span>
  * <span data-ttu-id="ed18e-148">Sélectionnez le nouveau nom d’utilisateur (à côté du **déconnexion** lien).</span><span class="sxs-lookup"><span data-stu-id="ed18e-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="ed18e-149">Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.</span><span class="sxs-lookup"><span data-stu-id="ed18e-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="ed18e-150">Sélectionnez le **les données personnelles** onglet.</span><span class="sxs-lookup"><span data-stu-id="ed18e-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="ed18e-151">Sélectionnez le **télécharger** bouton et examiné la *PersonalData.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="ed18e-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="ed18e-152">Test du **supprimer** bouton, ce qui supprime le connecté sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ed18e-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="ed18e-153">Ajouter des données utilisateur personnalisées pour la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="ed18e-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="ed18e-154">Mise à jour le `IdentityUser` dérivée de la classe avec des propriétés personnalisées.</span><span class="sxs-lookup"><span data-stu-id="ed18e-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="ed18e-155">Si vous avez nommé le projet d’application Web 1, le fichier est nommé *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed18e-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="ed18e-156">Mettre à jour le fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ed18e-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="ed18e-157">Propriétés décorée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribut sont :</span><span class="sxs-lookup"><span data-stu-id="ed18e-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="ed18e-158">Supprimé quand le *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Page Razor appelle `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="ed18e-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="ed18e-159">Inclus dans les données téléchargées par le *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Page Razor.</span><span class="sxs-lookup"><span data-stu-id="ed18e-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="ed18e-160">Mettre à jour de la page Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ed18e-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="ed18e-161">Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* code en surbrillance avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ed18e-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="ed18e-162">Mise à jour le *Areas/Identity/Pages/Account/Manage/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="ed18e-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="ed18e-163">Mettre à jour de la page Account/Register.cshtml en</span><span class="sxs-lookup"><span data-stu-id="ed18e-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="ed18e-164">Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Register.cshtml.cs* code en surbrillance avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ed18e-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="ed18e-165">Mise à jour le *Areas/Identity/Pages/Account/Register.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="ed18e-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="ed18e-166">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="ed18e-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="ed18e-167">Ajoutez une migration pour les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="ed18e-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed18e-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed18e-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ed18e-169">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="ed18e-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed18e-170">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed18e-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="ed18e-171">Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="ed18e-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="ed18e-172">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="ed18e-172">Test the app:</span></span>

* <span data-ttu-id="ed18e-173">Inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ed18e-173">Register a new user.</span></span>
* <span data-ttu-id="ed18e-174">Afficher les données utilisateur personnalisées sur le `/Identity/Account/Manage` page.</span><span class="sxs-lookup"><span data-stu-id="ed18e-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="ed18e-175">Télécharger et afficher les données personnelles les utilisateurs à partir de la `/Identity/Account/Manage/PersonalData` page.</span><span class="sxs-lookup"><span data-stu-id="ed18e-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
