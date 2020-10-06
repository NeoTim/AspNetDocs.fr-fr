---
uid: mvc/overview/deployment/docker-aspnetmvc
title: Migration d’applications ASP.NET MVC vers des conteneurs Windows
description: Découvrez comment prendre une application ASP.NET MVC et l’exécuter dans un conteneur Docker Windows
keywords: Conteneurs Windows, Dockr, ASP. NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 1c5e6af79c87123891ddd4d30c60e3a427910e9d
ms.sourcegitcommit: 09a34635ed0e74d6c2625f6a485c78f201c689ee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763484"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="4993f-104">Migration d’applications ASP.NET MVC vers des conteneurs Windows</span><span class="sxs-lookup"><span data-stu-id="4993f-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="4993f-105">L’exécution d’une application .NET Framework existante dans un conteneur Windows ne nécessite aucune modification de votre application.</span><span class="sxs-lookup"><span data-stu-id="4993f-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="4993f-106">Pour exécuter votre application dans un conteneur Windows, vous créez une image Docker contenant votre application et lancez le conteneur.</span><span class="sxs-lookup"><span data-stu-id="4993f-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="4993f-107">Cette rubrique décrit comment prendre une [application ASP.NET MVC](http://www.asp.net/mvc) existante et la déployer dans un conteneur Windows.</span><span class="sxs-lookup"><span data-stu-id="4993f-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="4993f-108">Vous partez d’une application ASP.NET MVC existante, puis générez les ressources publiées à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4993f-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="4993f-109">Vous utilisez Docker pour créer l’image qui contient et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="4993f-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="4993f-110">Vous allez accéder au site en cours d’exécution dans un conteneur Windows et vérifier que l’application fonctionne.</span><span class="sxs-lookup"><span data-stu-id="4993f-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="4993f-111">Cet article suppose une connaissance élémentaire de Docker.</span><span class="sxs-lookup"><span data-stu-id="4993f-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="4993f-112">Pour en savoir plus sur Docker, consultez la [présentation de Docker](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="4993f-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="4993f-113">L’application que vous allez exécuter dans un conteneur est un site web simple qui répond à des questions de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="4993f-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="4993f-114">Cette application est une application MVC de base sans authentification ni stockage de base de données, qui vous permet de vous concentrer sur le déplacement de la couche web vers un conteneur.</span><span class="sxs-lookup"><span data-stu-id="4993f-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="4993f-115">Des rubriques futures montreront comment déplacer et gérer le stockage persistant dans des applications en conteneur.</span><span class="sxs-lookup"><span data-stu-id="4993f-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="4993f-116">Le déplacement de votre application implique les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4993f-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="4993f-117">Création d’une tâche de publication pour générer les ressources d’une image</span><span class="sxs-lookup"><span data-stu-id="4993f-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="4993f-118">Génération d’une image Docker destinée à exécuter votre application</span><span class="sxs-lookup"><span data-stu-id="4993f-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="4993f-119">Démarrage d’un conteneur Docker qui exécute votre image</span><span class="sxs-lookup"><span data-stu-id="4993f-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="4993f-120">Vérification de l’application à l’aide de votre navigateur</span><span class="sxs-lookup"><span data-stu-id="4993f-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="4993f-121">L’[application terminée](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/deployment/docker-aspnetmvc/samples/MVCRandomAnswerGenerator) se trouve sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="4993f-121">The [finished application](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/deployment/docker-aspnetmvc/samples/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4993f-122">Prérequis</span><span class="sxs-lookup"><span data-stu-id="4993f-122">Prerequisites</span></span>

<span data-ttu-id="4993f-123">L’ordinateur de développement doit disposer des logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="4993f-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="4993f-124">[Mise à jour anniversaire Windows 10](https://www.microsoft.com/software-download/windows10/) (ou version ultérieure) ou [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="4993f-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="4993f-125">[Docker pour Windows](https://docs.docker.com/docker-for-windows/), version stable 1.13.0 ou 1.12 bêta 26 (ou versions plus récentes)</span><span class="sxs-lookup"><span data-stu-id="4993f-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="4993f-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4993f-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="4993f-127">Si vous utilisez Windows Server 2016, suivez les instructions indiquées dans [Déploiement d’un hôte de conteneurs – Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="4993f-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="4993f-128">Après l’installation et le démarrage de Docker, cliquez avec le bouton droit de la souris sur l’icône de la barre d’état, puis sélectionnez **Switch to Windows containers** (Basculer vers les conteneurs Windows).</span><span class="sxs-lookup"><span data-stu-id="4993f-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="4993f-129">Cette action est nécessaire pour exécuter des images Docker basées sur Windows.</span><span class="sxs-lookup"><span data-stu-id="4993f-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="4993f-130">Cette commande prend quelques secondes :</span><span class="sxs-lookup"><span data-stu-id="4993f-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="4993f-131">![Conteneur Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="4993f-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="4993f-132">Script de publication</span><span class="sxs-lookup"><span data-stu-id="4993f-132">Publish script</span></span>

<span data-ttu-id="4993f-133">Regroupez toutes les ressources que vous avez besoin de charger dans une image Docker à un seul emplacement.</span><span class="sxs-lookup"><span data-stu-id="4993f-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="4993f-134">Vous pouvez utiliser la commande **Publier** de Visual Studio pour créer un profil de publication pour votre application.</span><span class="sxs-lookup"><span data-stu-id="4993f-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="4993f-135">Ce profil placera toutes les ressources dans une arborescence de répertoires que vous copierez dans votre image cible plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4993f-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="4993f-136">**Étapes de la publication**</span><span class="sxs-lookup"><span data-stu-id="4993f-136">**Publish Steps**</span></span>

1. <span data-ttu-id="4993f-137">Cliquez avec le bouton droit sur le projet web dans Visual Studio et sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="4993f-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="4993f-138">Cliquez sur le **bouton profil personnalisé**, puis sélectionnez **système de fichiers** comme méthode.</span><span class="sxs-lookup"><span data-stu-id="4993f-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="4993f-139">Choisissez le répertoire.</span><span class="sxs-lookup"><span data-stu-id="4993f-139">Choose the directory.</span></span> <span data-ttu-id="4993f-140">Par convention, l’exemple téléchargé utilise `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="4993f-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="4993f-141">![Connexion pour la publication][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="4993f-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="4993f-142">Ouvrez la section **options de publication de fichier** de l’onglet **paramètres** . Sélectionnez **précompiler durant la publication**.</span><span class="sxs-lookup"><span data-stu-id="4993f-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="4993f-143">Cette optimisation signifie que, quand vous compilez les vues dans le conteneur Docker, vous copiez les vues précompilées.</span><span class="sxs-lookup"><span data-stu-id="4993f-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="4993f-144">![Paramètres de publication][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="4993f-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="4993f-145">Cliquez sur **Publier** ; Visual Studio copie alors toutes les ressources nécessaires dans le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="4993f-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="4993f-146">Créer l’image</span><span class="sxs-lookup"><span data-stu-id="4993f-146">Build the image</span></span>

<span data-ttu-id="4993f-147">Créez un nouveau fichier nommé *fichier dockerfile* pour définir votre image d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="4993f-147">Create a new file named *Dockerfile* to define your Docker image.</span></span> <span data-ttu-id="4993f-148">*Fichier dockerfile* contient des instructions pour générer l’image finale et inclut tous les noms d’images de base, les composants requis, l’application que vous souhaitez exécuter et d’autres images de configuration.</span><span class="sxs-lookup"><span data-stu-id="4993f-148">*Dockerfile* contains instructions to build the final image and includes any base image names, required components, the app you want to run, and other configuration images.</span></span> <span data-ttu-id="4993f-149">*Fichier dockerfile* est l’entrée de la `docker build` commande qui crée l’image.</span><span class="sxs-lookup"><span data-stu-id="4993f-149">*Dockerfile* is the input to the `docker build` command that creates the image.</span></span>

<span data-ttu-id="4993f-150">Pour cet exercice, vous allez créer une image basée sur l' `microsoft/aspnet` image située sur le [hub d’ancrage](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="4993f-150">For this exercise, you will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="4993f-151">L’image de base, `microsoft/aspnet`, est une image Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4993f-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="4993f-152">Il contient Windows Server Core, IIS et ASP.NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="4993f-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="4993f-153">Quand vous exécutez cette image dans votre conteneur, elle démarre automatiquement IIS et tous les sites web installés.</span><span class="sxs-lookup"><span data-stu-id="4993f-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="4993f-154">Le fichier Dockerfile qui crée votre image ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="4993f-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="4993f-155">Il n’existe aucune commande `ENTRYPOINT` dans ce fichier Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="4993f-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="4993f-156">Vous n’en avez pas besoin.</span><span class="sxs-lookup"><span data-stu-id="4993f-156">You don't need one.</span></span> <span data-ttu-id="4993f-157">Lors de l’exécution de Windows Server avec IIS, le processus IIS est le point d’entrée, qui est configuré pour démarrer dans l’image de base Aspnet.</span><span class="sxs-lookup"><span data-stu-id="4993f-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="4993f-158">Exécutez la commande de génération Docker pour créer l’image qui exécute votre application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4993f-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="4993f-159">Pour ce faire, ouvrez une fenêtre PowerShell dans le répertoire de votre projet et tapez la commande suivante dans le répertoire de la solution :</span><span class="sxs-lookup"><span data-stu-id="4993f-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="4993f-160">Cette commande génère la nouvelle image à l’aide des instructions de votre fichier dockerfile, en nommant (-t tagging) l’image en tant que mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="4993f-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="4993f-161">Celles-ci peuvent inclure l’extraction de l’image de base du [Hub Docker](http://hub.docker.com), puis l’ajout de votre application à cette image.</span><span class="sxs-lookup"><span data-stu-id="4993f-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="4993f-162">Une fois cette commande terminée, vous pouvez exécuter la commande `docker images` pour afficher des informations sur la nouvelle image :</span><span class="sxs-lookup"><span data-stu-id="4993f-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="4993f-163">L’ID de l’image sera différent sur votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="4993f-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="4993f-164">À présent, exécutons l’application.</span><span class="sxs-lookup"><span data-stu-id="4993f-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="4993f-165">Démarrer un conteneur</span><span class="sxs-lookup"><span data-stu-id="4993f-165">Start a container</span></span>

<span data-ttu-id="4993f-166">Démarrez un conteneur en exécutant la commande `docker run` suivante :</span><span class="sxs-lookup"><span data-stu-id="4993f-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="4993f-167">L’argument `-d` indique à Docker de démarrer l’image en mode détaché.</span><span class="sxs-lookup"><span data-stu-id="4993f-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="4993f-168">Cela signifie que l’image Docker s’exécute déconnectée de l’interpréteur de commandes actuel.</span><span class="sxs-lookup"><span data-stu-id="4993f-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="4993f-169">Dans de nombreux exemples d’ancrage, vous pouvez voir-p mapper le conteneur et les ports hôtes.</span><span class="sxs-lookup"><span data-stu-id="4993f-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="4993f-170">L’image ASPNET par défaut a déjà configuré le conteneur pour écouter sur le port 80 et l’exposer.</span><span class="sxs-lookup"><span data-stu-id="4993f-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="4993f-171">La portion `--name randomanswers` donne un nom au conteneur en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="4993f-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="4993f-172">Vous pouvez utiliser ce nom au lieu de l’ID de conteneur dans la plupart des commandes.</span><span class="sxs-lookup"><span data-stu-id="4993f-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="4993f-173">`mvcrandomanswers` est le nom de l’image à démarrer.</span><span class="sxs-lookup"><span data-stu-id="4993f-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="4993f-174">Vérifier dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="4993f-174">Verify in the browser</span></span>

<span data-ttu-id="4993f-175">Une fois le conteneur démarré, connectez-vous au conteneur en cours d’exécution à l’aide `http://localhost` de dans l’exemple indiqué.</span><span class="sxs-lookup"><span data-stu-id="4993f-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="4993f-176">Tapez cette URL dans votre navigateur ; vous devriez voir le site en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="4993f-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="4993f-177">Certains logiciels VPN ou proxy peuvent vous empêcher d’accéder à votre site.</span><span class="sxs-lookup"><span data-stu-id="4993f-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="4993f-178">Vous pouvez les désactiver temporairement pour vérifier que votre conteneur fonctionne.</span><span class="sxs-lookup"><span data-stu-id="4993f-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="4993f-179">Le répertoire d’exemples sur GitHub contient un [script PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) qui exécute ces commandes pour vous.</span><span class="sxs-lookup"><span data-stu-id="4993f-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="4993f-180">Ouvrez une fenêtre PowerShell, accédez au répertoire de votre solution et tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4993f-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="4993f-181">La commande ci-dessus génère l’image, affiche la liste des images sur votre ordinateur et démarre un conteneur.</span><span class="sxs-lookup"><span data-stu-id="4993f-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="4993f-182">Pour arrêter le conteneur, exécutez une commande `docker stop` :</span><span class="sxs-lookup"><span data-stu-id="4993f-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="4993f-183">Pour supprimer le conteneur, exécutez une commande `docker rm` :</span><span class="sxs-lookup"><span data-stu-id="4993f-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Basculer vers le conteneur Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publier dans le système de &fichiers"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Paramètres de publication"
