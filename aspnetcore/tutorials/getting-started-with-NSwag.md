---
title: Bien démarrer avec NSwag et ASP.NET Core
author: zuckerthoben
description: Découvrez comment utiliser NSwag pour générer des pages d’aide et de documentation pour une API web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/30/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c03e7513edc3240f3f13f0c190e1ca9480e476af
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037766"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="d88e2-103">Bien démarrer avec NSwag et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d88e2-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="d88e2-104">Par [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com) et [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="d88e2-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d88e2-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d88e2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d88e2-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d88e2-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="d88e2-107">NSwag offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="d88e2-107">NSwag offers the following capabilities:</span></span>

 * <span data-ttu-id="d88e2-108">La possibilité d’utiliser l’interface utilisateur de Swagger et le générateur Swagger.</span><span class="sxs-lookup"><span data-stu-id="d88e2-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
 * <span data-ttu-id="d88e2-109">Fonctionnalités de génération de code flexibles.</span><span class="sxs-lookup"><span data-stu-id="d88e2-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="d88e2-110">Avec NSwag, vous n’avez pas besoin d’une API&mdash;existante, vous pouvez utiliser des API de tiers qui intègrent Swagger et génèrent une implémentation du client.</span><span class="sxs-lookup"><span data-stu-id="d88e2-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="d88e2-111">NSwag vous permet d’accélérer le cycle de développement et de vous adapter facilement aux modifications de l’API.</span><span class="sxs-lookup"><span data-stu-id="d88e2-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="d88e2-112">Inscrire le middleware NSwag</span><span class="sxs-lookup"><span data-stu-id="d88e2-112">Register the NSwag middleware</span></span>

<span data-ttu-id="d88e2-113">Inscrivez le middleware NSwag pour :</span><span class="sxs-lookup"><span data-stu-id="d88e2-113">Register the NSwag middleware to:</span></span>

 * <span data-ttu-id="d88e2-114">Générer la spécification Swagger pour l’API web implémentée.</span><span class="sxs-lookup"><span data-stu-id="d88e2-114">Generate the Swagger specification for the implemented web API.</span></span>
 * <span data-ttu-id="d88e2-115">Utiliser l’IU Swagger pour parcourir et tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="d88e2-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="d88e2-116">Pour utiliser le middleware [NSwag](https://github.com/RSuter/NSwag) avec ASP.NET Core, installez le package NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="d88e2-116">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="d88e2-117">Ce package contient le middleware nécessaire pour générer et utiliser la spécification Swagger, l’IU Swagger (v2 et v3) et [l’IU ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="d88e2-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="d88e2-118">Vous pouvez installer le package NuGet NSwag avec l’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d88e2-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d88e2-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d88e2-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d88e2-120">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="d88e2-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="d88e2-121">Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="d88e2-122">Accédez au répertoire où se trouve le fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d88e2-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="d88e2-123">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d88e2-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="d88e2-124">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="d88e2-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="d88e2-125">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="d88e2-126">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="d88e2-127">Entrez « NSwag.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="d88e2-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="d88e2-128">Sélectionnez le package « NSwag.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d88e2-129">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d88e2-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d88e2-130">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="d88e2-131">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="d88e2-132">Entrez « NSwag.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="d88e2-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="d88e2-133">Sélectionnez le package « NSwag.AspNetCore » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d88e2-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d88e2-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d88e2-135">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="d88e2-135">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d88e2-136">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d88e2-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d88e2-137">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d88e2-137">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="d88e2-138">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="d88e2-138">Add and configure Swagger middleware</span></span>

 <span data-ttu-id="d88e2-139">Ajoutez et configurez Swagger dans votre application ASP.NET Core en exécutant les étapes suivantes dans la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="d88e2-139">Add and configure Swagger in your ASP.NET Core app by performing the following steps in the `Startup` class:</span></span>

* <span data-ttu-id="d88e2-140">Importez les espaces de noms suivants :</span><span class="sxs-lookup"><span data-stu-id="d88e2-140">Import the following namespaces:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

* <span data-ttu-id="d88e2-141">Dans la méthode `ConfigureServices`, inscrivez les services Swagger requis :</span><span class="sxs-lookup"><span data-stu-id="d88e2-141">In the `ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

 * <span data-ttu-id="d88e2-142">Dans la méthode `Configure`, activez l’intergiciel pour traiter la spécification Swagger générée et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="d88e2-142">In the `Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

 * <span data-ttu-id="d88e2-143">Lancez l’application.</span><span class="sxs-lookup"><span data-stu-id="d88e2-143">Launch the app.</span></span> <span data-ttu-id="d88e2-144">Accédez à :</span><span class="sxs-lookup"><span data-stu-id="d88e2-144">Navigate to:</span></span>
   * <span data-ttu-id="d88e2-145">`http://localhost:<port>/swagger` pour voir l’IU de Swagger.</span><span class="sxs-lookup"><span data-stu-id="d88e2-145">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
   * <span data-ttu-id="d88e2-146">`http://localhost:<port>/swagger/v1/swagger.json` pour voir la spécification Swagger.</span><span class="sxs-lookup"><span data-stu-id="d88e2-146">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="d88e2-147">Génération de code</span><span class="sxs-lookup"><span data-stu-id="d88e2-147">Code generation</span></span>

<span data-ttu-id="d88e2-148">Vous pouvez tirer parti des fonctionnalités de génération de code de NSwag en choisissant l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="d88e2-148">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

 * <span data-ttu-id="d88e2-149">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; une application de bureau Windows pour générer du code client en C# ou TypeScript pour une API.</span><span class="sxs-lookup"><span data-stu-id="d88e2-149">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
 * <span data-ttu-id="d88e2-150">Les packages NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pour générer du code dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="d88e2-150">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="d88e2-151">NSwag à partir de la [ligne de commande](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="d88e2-151">NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
 * <span data-ttu-id="d88e2-152">Le package NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="d88e2-152">The [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>


### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="d88e2-153">Générer du code avec NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="d88e2-153">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="d88e2-154">Installez NSwagStudio en suivant les instructions fournies dans le [référentiel GitHub de NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="d88e2-154">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
 * <span data-ttu-id="d88e2-155">Lancez NSwagStudio et entrez l’URL du fichier *swagger.json* dans la zone de texte **Swagger Specification URL** (URL de spécification Swagger).</span><span class="sxs-lookup"><span data-stu-id="d88e2-155">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="d88e2-156">Par exemple, *http://localhost:44354/swagger/v1/swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="d88e2-156">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="d88e2-157">Cliquez sur le bouton **Créer une copie locale** pour générer la représentation JSON de votre spécification Swagger.</span><span class="sxs-lookup"><span data-stu-id="d88e2-157">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Créer une copie locale de la spécification Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

 * <span data-ttu-id="d88e2-159">Dans la zone **Sorties**, cliquez sur la case **CSharp Client**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-159">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="d88e2-160">Selon votre projet, vous pouvez également choisir **TypeScript Client** ou **CSharp Web API Controller**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-160">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="d88e2-161">Si vous sélectionnez **CSharp Web API Controller**, une spécification de service reconstruit le service, agissant comme une génération inverse.</span><span class="sxs-lookup"><span data-stu-id="d88e2-161">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="d88e2-162">Cliquez sur **Générer des sorties** pour produire une implémentation client C# complète du projet *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="d88e2-162">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="d88e2-163">Pour afficher le code client généré, cliquez sur l’onglet **CSharp Client** :</span><span class="sxs-lookup"><span data-stu-id="d88e2-163">To see the generated client code, click the **CSharp Client** tab:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient 
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;
    
        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient; 
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() => 
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }
    
        public string BaseUrl 
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
 > <span data-ttu-id="d88e2-164">Le code client C# est généré en fonction des sélections dans l’onglet **Paramètres**. Modifiez les paramètres pour effectuer des tâches telles que le renommage de l’espace de noms par défaut et la génération de méthodes synchrones.</span><span class="sxs-lookup"><span data-stu-id="d88e2-164">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

 * <span data-ttu-id="d88e2-165">Copiez le code C# généré dans un fichier dans le projet client qui utilisera l’API.</span><span class="sxs-lookup"><span data-stu-id="d88e2-165">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="d88e2-166">Démarrez l’utilisation de l’API web :</span><span class="sxs-lookup"><span data-stu-id="d88e2-166">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="d88e2-167">Personnaliser la documentation sur l’API</span><span class="sxs-lookup"><span data-stu-id="d88e2-167">Customize API documentation</span></span>

<span data-ttu-id="d88e2-168">Swagger fournit des options pour documenter le modèle objet pour faciliter l’utilisation de l’API web.</span><span class="sxs-lookup"><span data-stu-id="d88e2-168">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="d88e2-169">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="d88e2-169">API info and description</span></span>

<span data-ttu-id="d88e2-170">Dans la méthode `Startup.ConfigureServices`, une action de configuration passée à la méthode `AddSwaggerDocument` ajoute des informations comme l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="d88e2-170">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="d88e2-171">L’IU Swagger affiche les informations de la version :</span><span class="sxs-lookup"><span data-stu-id="d88e2-171">The Swagger UI displays the version's information:</span></span>

![Interface utilisateur de Swagger avec des informations de version](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="d88e2-173">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="d88e2-173">XML comments</span></span>

 <span data-ttu-id="d88e2-174">Pour activer les commentaires XML, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="d88e2-174">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="d88e2-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d88e2-175">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="d88e2-176">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Modifier <nom_projet>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-176">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="d88e2-177">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="d88e2-177">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="d88e2-178">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-178">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="d88e2-179">Cochez la case **Fichier de documentation XML** dans la section **Sortie** sous l’onglet **Générer**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-179">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="d88e2-180">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d88e2-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="d88e2-181">Dans le *Panneau Solutions*, appuyez sur **contrôle** et cliquez sur le nom du projet.</span><span class="sxs-lookup"><span data-stu-id="d88e2-181">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="d88e2-182">Accédez à **Outils** > **Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-182">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="d88e2-183">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="d88e2-183">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="d88e2-184">Ouvrez la boîte de dialogue **Options du projet** > **Générer**>**Compilateur**</span><span class="sxs-lookup"><span data-stu-id="d88e2-184">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="d88e2-185">Cochez la case **Générer la documentation XML** dans la section **Options générales**.</span><span class="sxs-lookup"><span data-stu-id="d88e2-185">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="d88e2-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d88e2-186">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="d88e2-187">Ajoutez manuellement les lignes en surbrillance au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="d88e2-187">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="d88e2-188">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="d88e2-188">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

 <span data-ttu-id="d88e2-189">Étant donné que NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et le type de retour recommandé pour les actions d’API web est [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), il ne peut pas déduire ce que fait votre action, ni ce qu’elle retourne.</span><span class="sxs-lookup"><span data-stu-id="d88e2-189">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="d88e2-190">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="d88e2-190">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

 <span data-ttu-id="d88e2-191">L’action précédente retourne `IActionResult`, mais à l’intérieur de l’action, [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) ou [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) sont retournés.</span><span class="sxs-lookup"><span data-stu-id="d88e2-191">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="d88e2-192">Utilisez les annotations de données pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner.</span><span class="sxs-lookup"><span data-stu-id="d88e2-192">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="d88e2-193">Décorez l’action avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="d88e2-193">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="d88e2-194">Étant donné que NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et que le type de retour recommandé pour les actions d’API web est [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult`1), il ne peut que déduire le type de retour défini par `T`.</span><span class="sxs-lookup"><span data-stu-id="d88e2-194">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult`1), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="d88e2-195">Vous ne pouvez pas déduire automatiquement d’autres types de retour possibles.</span><span class="sxs-lookup"><span data-stu-id="d88e2-195">You can't automatically infer other possible return types.</span></span> 

<span data-ttu-id="d88e2-196">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="d88e2-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="d88e2-197">L’action précédente retourne `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="d88e2-197">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="d88e2-198">À l’intérieur de l’action, il retourne [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="d88e2-198">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="d88e2-199">Étant donné que le contrôleur est décoré avec l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), une réponse [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) est également possible.</span><span class="sxs-lookup"><span data-stu-id="d88e2-199">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="d88e2-200">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="d88e2-200">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="d88e2-201">Utilisez les annotations de données pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner.</span><span class="sxs-lookup"><span data-stu-id="d88e2-201">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="d88e2-202">Décorez l’action avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="d88e2-202">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="d88e2-203">Dans ASP.NET Core 2.2 ou une version ultérieure, vous pouvez utiliser les conventions comme alternatives à la décoration explicites des actions individuelles avec `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="d88e2-203">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="d88e2-204">Pour plus d'informations, consultez <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="d88e2-204">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

 <span data-ttu-id="d88e2-205">Le générateur Swagger peut maintenant décrire précisément cette action et les clients générés savent ce qu’ils reçoivent quand ils appellent le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="d88e2-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="d88e2-206">Nous vous recommandons vivement de décorer toutes les actions avec ces attributs.</span><span class="sxs-lookup"><span data-stu-id="d88e2-206">As a recommendation, decorate all actions with these attributes.</span></span> 

<span data-ttu-id="d88e2-207">Pour obtenir des indications sur les réponses HTTP que doivent retourner vos actions d’API, consultez la [spécification RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="d88e2-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
