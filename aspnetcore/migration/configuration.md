---
title: Migrer la configuration d’ASP.NET Core
author: ardalis
description: Découvrez comment migrer la configuration à partir d’un projet ASP.NET MVC vers un projet ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048236"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="65cee-103">Migrer la configuration d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65cee-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="65cee-104">Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="65cee-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="65cee-105">Dans l’article précédent, nous avons commencé [migrer un projet ASP.NET MVC vers ASP.NET Core MVC](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="65cee-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="65cee-106">Dans cet article, nous migrer la configuration.</span><span class="sxs-lookup"><span data-stu-id="65cee-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="65cee-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="65cee-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="65cee-108">Configuration de l’installation</span><span class="sxs-lookup"><span data-stu-id="65cee-108">Setup configuration</span></span>

<span data-ttu-id="65cee-109">ASP.NET Core n’utilise plus les fichiers *Global.asax* et *web.config* utilisées par les versions précédentes d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65cee-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="65cee-110">Dans les versions antérieures d’ASP.NET, la logique de démarrage d’application a été placée dans une méthode `Application_StartUp` du fichier *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="65cee-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="65cee-111">Ensuite, dans ASP.NET MVC, un fichier *Startup.cs* a été inclus dans la racine du projet ; et, il été appelée au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="65cee-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="65cee-112">ASP.NET Core a arrêté complètement cette approche, en plaçant toute logique de démarrage dans le *Startup.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="65cee-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="65cee-113">Le fichier *web.config* a également été remplacé dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65cee-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="65cee-114">La configuration proprement dite peut maintenant être configurée, dans le cadre de la procédure de démarrage d’application décrite dans le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="65cee-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="65cee-115">La configuration peut toujours utiliser des fichiers XML, mais en général, les projets ASP.NET Core place les valeurs de configuration dans un fichier au format JSON, tel que *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="65cee-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="65cee-116">Système de configuration d’ASP.NET Core également facilement accessible des variables d’environnement, qui peuvent fournir un [plus un emplacement sécurisé et robuste](xref:security/app-secrets) pour les valeurs spécifiques à l’environnement.</span><span class="sxs-lookup"><span data-stu-id="65cee-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="65cee-117">Cela est particulièrement vrai pour les clés secrètes comme des chaînes de connexion et les clés API qui ne doivent pas être archivés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="65cee-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="65cee-118">Consultez [Configuration](xref:fundamentals/configuration/index) pour en savoir plus sur la configuration dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65cee-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="65cee-119">Pour cet article, nous avons commencé avec le projet ASP.NET Core partiellement migré à partir de [l’article précédent](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="65cee-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="65cee-120">Pour installer la configuration, ajoutez le constructeur et la propriété suivante dans le fichier *Startup.cs* situé à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65cee-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="65cee-121">Notez qu’à ce stade, le fichier *Startup.cs* fichier ne sera pas compilé, qu’il faut toujours ajouter les éléments suivants avec l'instruction `using`:</span><span class="sxs-lookup"><span data-stu-id="65cee-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="65cee-122">Ajouter un fichier *appsettings.json* à la racine du projet à l’aide du modèle d’élément approprié :</span><span class="sxs-lookup"><span data-stu-id="65cee-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Ajouter AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="65cee-124">Migrer les paramètres de configuration à partir de web.config</span><span class="sxs-lookup"><span data-stu-id="65cee-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="65cee-125">Notre projet ASP.NET MVC inclut  la chaîne de connexion de base de données requises dans le fichier *web.config*, dans l'élément `<connectionStrings>`.</span><span class="sxs-lookup"><span data-stu-id="65cee-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="65cee-126">Dans notre projet ASP.NET Core, nous allons stocker ces informations dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="65cee-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="65cee-127">Ouvrez le fichier *appsettings.json*et notez qu’il contient déjà les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="65cee-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="65cee-128">Dans la ligne en surbrillance mentionnée ci-dessus, remplacez le nom de la base de données à partir de **_CHANGE_ME** avec le nom de votre base de données.</span><span class="sxs-lookup"><span data-stu-id="65cee-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="65cee-129">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="65cee-129">Summary</span></span>

<span data-ttu-id="65cee-130">ASP.NET Core place toute la logique de démarrage de l’application dans un seul fichier dans lequel les services nécessaires et les dépendances peuvent être définis et configurés.</span><span class="sxs-lookup"><span data-stu-id="65cee-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="65cee-131">Il remplace le fichier *web.config* avec une fonctionnalité de configuration souples qui peut tirer parti des divers formats de fichier, tels que JSON, ainsi que les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="65cee-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
