---
title: Bien démarrer avec Swashbuckle et ASP.NET Core
author: zuckerthoben
description: Découvrez comment ajouter Swashbuckle à votre projet d’API web ASP.NET Core pour intégrer l’IU Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 02/06/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 9239a46889691135dce5c99f8fc9b8c7b38ab457
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043936"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="6c655-103">Bien démarrer avec Swashbuckle et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c655-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="6c655-104">De [Shayne Boyer](https://twitter.com/spboyer) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="6c655-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="6c655-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c655-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6c655-106">Swashbuckle compte trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="6c655-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="6c655-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/) : modèle objet Swagger et intergiciel (middleware) pour exposer des objets `SwaggerDocument` sous forme de points de terminaison JSON.</span><span class="sxs-lookup"><span data-stu-id="6c655-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="6c655-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/) : générateur Swagger qui crée des objets `SwaggerDocument` directement à partir de vos routes, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="6c655-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="6c655-109">Il est généralement associé à l’intergiciel de point de terminaison Swagger pour exposer automatiquement Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="6c655-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="6c655-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): version incorporée de l’outil IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="6c655-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="6c655-111">Elle interprète Swagger JSON afin de générer une expérience complète et personnalisable pour décrire la fonctionnalité de l’API web.</span><span class="sxs-lookup"><span data-stu-id="6c655-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="6c655-112">Il inclut des ateliers de test intégrés pour les méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="6c655-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="6c655-113">Installation de package</span><span class="sxs-lookup"><span data-stu-id="6c655-113">Package installation</span></span>

<span data-ttu-id="6c655-114">Vous pouvez ajouter Swashbuckle en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="6c655-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c655-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c655-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6c655-116">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="6c655-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="6c655-117">Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="6c655-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="6c655-118">Accédez au répertoire où se trouve le fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6c655-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="6c655-119">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6c655-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="6c655-120">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="6c655-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="6c655-121">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6c655-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="6c655-122">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="6c655-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="6c655-123">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="6c655-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="6c655-124">Sélectionnez le package « Swashbuckle.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="6c655-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6c655-125">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c655-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6c655-126">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="6c655-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="6c655-127">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="6c655-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="6c655-128">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="6c655-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="6c655-129">Sélectionnez le package « Swashbuckle.AspNetCore » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="6c655-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c655-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c655-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6c655-131">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="6c655-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c655-132">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c655-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6c655-133">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6c655-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="6c655-134">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="6c655-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="6c655-135">Ajoutez le générateur Swagger à la collection de services dans la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="6c655-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="6c655-136">Importez les espaces de noms suivants à utiliser dans la classe `Info` :</span><span class="sxs-lookup"><span data-stu-id="6c655-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="6c655-137">Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter le document JSON généré et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="6c655-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="6c655-138">L’appel de méthode `UseSwaggerUI` précédent active le [middleware (intergiciel) des fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="6c655-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="6c655-139">Si vous ciblez le .NET Framework ou .NET Core 1.x, ajoutez le package NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) au projet.</span><span class="sxs-lookup"><span data-stu-id="6c655-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="6c655-140">Lancez l’application et accédez à `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="6c655-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="6c655-141">Le document généré décrivant les points de terminaison s’affiche comme illustré dans la [spécification Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="6c655-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="6c655-142">L’IU Swagger se trouve à l’adresse `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="6c655-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="6c655-143">Explorez l’API via l’IU Swagger et incorporez-la dans d’autres programmes.</span><span class="sxs-lookup"><span data-stu-id="6c655-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="6c655-144">Pour utiliser l’IU Swagger à la racine de l’application (`http://localhost:<port>/`), définissez la propriété `RoutePrefix` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="6c655-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="6c655-145">Si vous utilisez des répertoires avec IIS ou un proxy inverse, définissez le point de terminaison Swagger sur un chemin relatif avec le préfixe `./`.</span><span class="sxs-lookup"><span data-stu-id="6c655-145">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="6c655-146">Par exemple, `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="6c655-146">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="6c655-147">L’utilisation de `/swagger/v1/swagger.json` indique à l’application de rechercher le fichier JSON à la racine réelle de l’URL (plus le préfixe de la route s’il est utilisé).</span><span class="sxs-lookup"><span data-stu-id="6c655-147">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="6c655-148">Par exemple, utilisez `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` à la place `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="6c655-148">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="6c655-149">Personnaliser et étendre</span><span class="sxs-lookup"><span data-stu-id="6c655-149">Customize and extend</span></span>

<span data-ttu-id="6c655-150">Swagger fournit des options pour documenter le modèle objet et personnaliser l’interface utilisateur en fonction de votre thème.</span><span class="sxs-lookup"><span data-stu-id="6c655-150">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="6c655-151">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="6c655-151">API info and description</span></span>

<span data-ttu-id="6c655-152">L’action de configuration transmise à la méthode `AddSwaggerGen` ajoute des informations comme l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="6c655-152">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="6c655-153">L’IU Swagger affiche les informations de la version :</span><span class="sxs-lookup"><span data-stu-id="6c655-153">The Swagger UI displays the version's information:</span></span>

![Interface utilisateur de Swagger avec les informations de version : description, auteur et lien pour en savoir plus](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="6c655-155">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="6c655-155">XML comments</span></span>

<span data-ttu-id="6c655-156">Vous pouvez activer les commentaires XML en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="6c655-156">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c655-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c655-157">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="6c655-158">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Modifier <nom_projet>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="6c655-158">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="6c655-159">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="6c655-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="6c655-160">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="6c655-160">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="6c655-161">Cochez la case **Fichier de documentation XML** dans la section **Sortie** sous l’onglet **Générer**.</span><span class="sxs-lookup"><span data-stu-id="6c655-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6c655-162">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c655-162">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="6c655-163">Dans le *Panneau Solutions*, appuyez sur **contrôle** et cliquez sur le nom du projet.</span><span class="sxs-lookup"><span data-stu-id="6c655-163">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="6c655-164">Accédez à **Outils** > **Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="6c655-164">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="6c655-165">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="6c655-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="6c655-166">Ouvrez la boîte de dialogue **Options du projet** > **Générer**>**Compilateur**</span><span class="sxs-lookup"><span data-stu-id="6c655-166">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="6c655-167">Cochez la case **Générer la documentation XML** dans la section **Options générales**.</span><span class="sxs-lookup"><span data-stu-id="6c655-167">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c655-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c655-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6c655-169">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="6c655-169">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c655-170">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c655-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6c655-171">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="6c655-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="6c655-172">L’activation de commentaires XML fournit des informations de débogage sur les membres et types publics non documentés.</span><span class="sxs-lookup"><span data-stu-id="6c655-172">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="6c655-173">Les membres et types non documentés sont indiqués par le message d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="6c655-173">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="6c655-174">Par exemple, le message suivant indique une violation du code d’avertissement 1591 :</span><span class="sxs-lookup"><span data-stu-id="6c655-174">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="6c655-175">Pour supprimer des avertissements à l’échelle d’un projet, définissez une liste séparée par des points-virgules de codes d’avertissement à ignorer dans le fichier de projet.</span><span class="sxs-lookup"><span data-stu-id="6c655-175">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="6c655-176">L’ajout des codes d’avertissement à `$(NoWarn);` applique aussi les [valeurs par défaut C#](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16).</span><span class="sxs-lookup"><span data-stu-id="6c655-176">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="6c655-177">Pour supprimer des avertissements uniquement pour des membres spécifiques, placez le code dans les directives de préprocesseur [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning).</span><span class="sxs-lookup"><span data-stu-id="6c655-177">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="6c655-178">Cette approche est utile pour le code qui ne doit pas être exposé par le biais des documentations des API. Dans l’exemple suivant, le code d’avertissement CS1591 est ignoré pour toute la classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="6c655-178">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="6c655-179">La mise en œuvre de code d’avertissement est restaurée à la fin de la définition de classe.</span><span class="sxs-lookup"><span data-stu-id="6c655-179">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="6c655-180">Spécifier plusieurs codes d’avertissement avec une liste délimitée par des virgules.</span><span class="sxs-lookup"><span data-stu-id="6c655-180">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="6c655-181">Configurez Swagger pour utiliser le fichier XML généré.</span><span class="sxs-lookup"><span data-stu-id="6c655-181">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="6c655-182">Pour les systèmes d’exploitation Linux ou non-Windows, les chemins et les noms de fichiers peuvent respecter la casse.</span><span class="sxs-lookup"><span data-stu-id="6c655-182">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="6c655-183">Par exemple, un fichier *ToDoApi.XML* est valide sur Windows, mais pas sur CentOS.</span><span class="sxs-lookup"><span data-stu-id="6c655-183">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="6c655-184">Dans le code précédent, la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection) est utilisée pour générer un nom de fichier XML correspondant à celui du projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="6c655-184">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="6c655-185">La propriété [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) est utilisée pour construire le chemin du fichier XML.</span><span class="sxs-lookup"><span data-stu-id="6c655-185">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="6c655-186">Quand vous ajoutez des commentaires avec trois barres obliques à une action, la description est ajoutée à l’en-tête de section dans l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="6c655-186">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="6c655-187">Ajoutez un élément [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) au dessus de l’action `Delete` :</span><span class="sxs-lookup"><span data-stu-id="6c655-187">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="6c655-188">L’IU Swagger affiche le texte interne de l’élément `<summary>` du code précédent :</span><span class="sxs-lookup"><span data-stu-id="6c655-188">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interface utilisateur de Swagger affichant le commentaire XML « Deletes a specific TodoItem. »](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="6c655-191">L’IU est définie par le schéma JSON généré :</span><span class="sxs-lookup"><span data-stu-id="6c655-191">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="6c655-192">Ajoutez un élément [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) à la documentation de la méthode de l’action `Create`.</span><span class="sxs-lookup"><span data-stu-id="6c655-192">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="6c655-193">Il complète les informations spécifiées dans l’élément `<summary>` et fournit une IU Swagger plus robuste.</span><span class="sxs-lookup"><span data-stu-id="6c655-193">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="6c655-194">Le contenu de l’élément `<remarks>` peut être du texte, du code JSON ou du code XML.</span><span class="sxs-lookup"><span data-stu-id="6c655-194">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="6c655-195">Notez les améliorations de l’IU avec ces commentaires supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="6c655-195">Notice the UI enhancements with these additional comments:</span></span>

![Interface utilisateur de Swagger avec des commentaires supplémentaires affichés](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="6c655-197">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="6c655-197">Data annotations</span></span>

<span data-ttu-id="6c655-198">Décorez le modèle avec des attributs, présents dans l’espace de noms [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations), pour gérer les composants de l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="6c655-198">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="6c655-199">Ajoutez l’attribut `[Required]` à la propriété `Name` de la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="6c655-199">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="6c655-200">La présence de cet attribut change le comportement de l’interface utilisateur et modifie le schéma JSON sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="6c655-200">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="6c655-201">Ajoutez l’attribut `[Produces("application/json")]` au contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="6c655-201">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="6c655-202">Son objectif est de déclarer que les actions du contrôleur prennent en charge une réponse dont le type de contenu est *application/json* :</span><span class="sxs-lookup"><span data-stu-id="6c655-202">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="6c655-203">La zone de liste déroulante **Response Content Type** permet de sélectionner ce type de contenu comme valeur par défaut pour les actions GET du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="6c655-203">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface utilisateur de Swagger avec le type de contenu de réponse par défaut](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="6c655-205">À mesure que l’utilisation des annotations de données dans l’API web augmente, l’interface utilisateur et les pages d’aide de l’API deviennent de plus en plus descriptives et utiles.</span><span class="sxs-lookup"><span data-stu-id="6c655-205">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="6c655-206">Décrire des types de réponse</span><span class="sxs-lookup"><span data-stu-id="6c655-206">Describe response types</span></span>

<span data-ttu-id="6c655-207">Les développeurs consommant une API web s’intéressent surtout à ce qui est retourné&mdash;, en particulier les types de réponse et les codes d’erreur (s’ils ne sont pas standards).</span><span class="sxs-lookup"><span data-stu-id="6c655-207">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="6c655-208">Les types de réponse et les codes d’erreur sont décrits dans les commentaires XML et les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="6c655-208">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="6c655-209">L’action `Create` retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="6c655-209">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="6c655-210">Un code d’état HTTP 400 est retourné quand le corps de la demande postée est null.</span><span class="sxs-lookup"><span data-stu-id="6c655-210">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="6c655-211">Sans documentation appropriée dans l’interface utilisateur de Swagger, le consommateur n’a pas connaissance de ces résultats attendus.</span><span class="sxs-lookup"><span data-stu-id="6c655-211">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="6c655-212">Pour résoudre ce problème, ajoutez les lignes en surbrillance de l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="6c655-212">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="6c655-213">L’interface utilisateur de Swagger documente maintenant clairement les codes de réponse HTTP attendus :</span><span class="sxs-lookup"><span data-stu-id="6c655-213">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interface utilisateur de Swagger affichant la description de la classe de réponse POST « Returns the newly created Todo item » et « 400 - If the item is null » pour le code d’état et la raison sous Response Messages](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6c655-215">Dans ASP.NET Core 2.2 ou une version ultérieure, les conventions peuvent être utilisées comme alternatives à la décoration explicites des actions individuelles avec `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="6c655-215">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="6c655-216">Pour plus d'informations, consultez <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="6c655-216">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="6c655-217">Personnaliser l’IU</span><span class="sxs-lookup"><span data-stu-id="6c655-217">Customize the UI</span></span>

<span data-ttu-id="6c655-218">L’IU de stock est fonctionnelle et conviviale.</span><span class="sxs-lookup"><span data-stu-id="6c655-218">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="6c655-219">Toutefois, les pages de documentation d’API doivent représenter votre marque ou thème.</span><span class="sxs-lookup"><span data-stu-id="6c655-219">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="6c655-220">La personnalisation des composants Swashbuckle nécessite d’ajouter les ressources qui traitent les fichiers statiques et de générer la structure de dossiers pour héberger ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c655-220">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="6c655-221">Si vous ciblez le .NET Framework ou .NET Core 1.x, ajoutez le package NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) au projet :</span><span class="sxs-lookup"><span data-stu-id="6c655-221">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="6c655-222">Le package NuGet précédent est déjà installé si vous ciblez .NET Core 2.x et que vous utilisez le [métapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6c655-222">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="6c655-223">Activez le middleware de fichiers statiques :</span><span class="sxs-lookup"><span data-stu-id="6c655-223">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="6c655-224">Faites l’acquisition du contenu du dossier *dist* à partir du [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="6c655-224">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="6c655-225">Ce dossier contient les ressources nécessaires pour la page de l’interface utilisateur de Swagger.</span><span class="sxs-lookup"><span data-stu-id="6c655-225">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="6c655-226">Créez un dossier *wwwroot/swagger/ui* et copiez dedans le contenu du dossier *dist*.</span><span class="sxs-lookup"><span data-stu-id="6c655-226">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="6c655-227">Créez un fichier *custom.css* dans *wwwroot/swagger/ui* avec le code CSS suivant pour personnaliser l’en-tête de page :</span><span class="sxs-lookup"><span data-stu-id="6c655-227">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="6c655-228">Référencez *custom.css* dans le fichier *index.html*, à la suite de n’importe quel autre fichier CSS :</span><span class="sxs-lookup"><span data-stu-id="6c655-228">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="6c655-229">Accédez à la page *index.html* à l’adresse `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="6c655-229">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="6c655-230">Entrez `http://localhost:<port>/swagger/v1/swagger.json` dans la zone de texte de l’en-tête, puis cliquez sur le bouton **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6c655-230">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="6c655-231">La page résultante ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="6c655-231">The resulting page looks as follows:</span></span>

![Interface utilisateur de Swagger avec titre d’en-tête personnalisé](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="6c655-233">Vous pouvez faire encore beaucoup avec cette page.</span><span class="sxs-lookup"><span data-stu-id="6c655-233">There's much more you can do with the page.</span></span> <span data-ttu-id="6c655-234">Pour découvrir toutes les fonctionnalités pour les ressources d’interface utilisateur, accédez au [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="6c655-234">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
