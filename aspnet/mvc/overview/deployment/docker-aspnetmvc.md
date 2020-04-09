---
uid: mvc/overview/deployment/docker-aspnetmvc
title: Migration d’applications ASP.NET MVC vers des conteneurs Windows
description: Découvrez comment prendre une application ASP.NET MVC et l’exécuter dans un conteneur Docker Windows
keywords: Conteneurs Windows,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 2c3aefab16673f4d4dd28c74319903fbd25a9e7e
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675196"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migration d’applications ASP.NET MVC vers des conteneurs Windows

L’exécution d’une application .NET Framework existante dans un conteneur Windows ne nécessite aucune modification de votre application. Pour exécuter votre application dans un conteneur Windows, vous créez une image Docker contenant votre application et lancez le conteneur. Cette rubrique décrit comment prendre une [application ASP.NET MVC](http://www.asp.net/mvc) existante et la déployer dans un conteneur Windows.

Vous partez d’une application ASP.NET MVC existante, puis générez les ressources publiées à l’aide de Visual Studio. Vous utilisez Docker pour créer l’image qui contient et exécute votre application. Vous allez accéder au site en cours d’exécution dans un conteneur Windows et vérifier que l’application fonctionne.

Cet article suppose une connaissance élémentaire de Docker. Pour en savoir plus sur Docker, consultez la [présentation de Docker](https://docs.docker.com/engine/understanding-docker/).

L’application que vous allez exécuter dans un conteneur est un site web simple qui répond à des questions de manière aléatoire. Cette application est une application MVC de base sans authentification ni stockage de base de données, qui vous permet de vous concentrer sur le déplacement de la couche web vers un conteneur. Des rubriques futures montreront comment déplacer et gérer le stockage persistant dans des applications en conteneur.

Le déplacement de votre application implique les étapes suivantes :

1. [Création d’une tâche de publication pour générer les ressources d’une image](#publish-script)
1. [Génération d’une image Docker destinée à exécuter votre application](#build-the-image)
1. [Démarrage d’un conteneur Docker qui exécute votre image](#start-a-container)
1. [Vérification de l’application à l’aide de votre navigateur](#verify-in-the-browser)

L’[application terminée](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) se trouve sur GitHub.

## <a name="prerequisites"></a>Prérequis

La machine de développement doit avoir le logiciel suivant :

- [Mise à jour anniversaire de Windows 10](https://www.microsoft.com/software-download/windows10/) (ou plus) ou [Serveur Windows 2016](https://www.microsoft.com/cloud-platform/windows-server) (ou plus)
- [Docker pour Windows](https://docs.docker.com/docker-for-windows/), version stable 1.13.0 ou 1.12 bêta 26 (ou versions plus récentes)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Si vous utilisez Windows Server 2016, suivez les instructions indiquées dans [Déploiement d’un hôte de conteneurs – Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Après l’installation et le démarrage de Docker, cliquez avec le bouton droit de la souris sur l’icône de la barre d’état, puis sélectionnez **Switch to Windows containers** (Basculer vers les conteneurs Windows). Cette action est nécessaire pour exécuter des images Docker basées sur Windows. Cette commande prend quelques secondes :

![Conteneur Windows][windows-container]

## <a name="publish-script"></a>Script de publication

Regroupez toutes les ressources que vous avez besoin de charger dans une image Docker à un seul emplacement. Vous pouvez utiliser la commande **Publier** de Visual Studio pour créer un profil de publication pour votre application. Ce profil placera toutes les ressources dans une arborescence de répertoires que vous copierez dans votre image cible plus loin dans ce didacticiel.

**Étapes de la publication**

1. Cliquez avec le bouton droit sur le projet web dans Visual Studio et sélectionnez **Publier**.
1. Cliquez sur le **bouton profil personnalisé,** puis sélectionnez **système de fichiers** comme méthode.
1. Choisissez le répertoire. Par convention, l’exemple téléchargé utilise `bin\Release\PublishOutput`.

![Connexion pour la publication][publish-connection]

Ouvrez la section **Options de publication** des fichiers de **l’onglet Paramètres.** Sélectionnez **Précompile lors de la publication**. Cette optimisation signifie que, quand vous compilez les vues dans le conteneur Docker, vous copiez les vues précompilées.

![Publier les paramètres][publish-settings]

Cliquez sur **Publier** ; Visual Studio copie alors toutes les ressources nécessaires dans le dossier de destination.

## <a name="build-the-image"></a>Créer l’image

Créez un nouveau fichier nommé *Dockerfile* pour définir votre image Docker. *Dockerfile* contient des instructions pour construire l’image finale et inclut tous les noms d’images de base, les composants requis, l’application que vous souhaitez exécuter, et d’autres images de configuration. *Dockerfile* est l’entrée à la `docker build` commande qui crée l’image.

Pour cet exercice, vous construireez `microsoft/aspnet` une image basée sur l’image située sur [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
L’image de base, `microsoft/aspnet`, est une image Windows Server. Il contient Windows Server Core, IIS et ASP.NET 4.7.2. Quand vous exécutez cette image dans votre conteneur, elle démarre automatiquement IIS et tous les sites web installés.

Le fichier Dockerfile qui crée votre image ressemble à ceci :

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Il n’existe aucune commande `ENTRYPOINT` dans ce fichier Dockerfile. Vous n’en avez pas besoin. Lors de l’exécution de Windows Server avec IIS, le processus IIS est le point d’entrée, qui est configuré pour démarrer dans l’image de base aspnet.

Exécutez la commande de génération Docker pour créer l’image qui exécute votre application ASP.NET. Pour ce faire, ouvrez une fenêtre PowerShell dans l’annuaire de votre projet et tapez la commande suivante dans le répertoire de la solution :

```console
docker build -t mvcrandomanswers .
```

Cette commande construira la nouvelle image en utilisant les instructions dans votre Dockerfile, nommant (-t marquage) l’image comme mvcrandomanswers. Celles-ci peuvent inclure l’extraction de l’image de base du [Hub Docker](http://hub.docker.com), puis l’ajout de votre application à cette image.

Une fois cette commande terminée, vous pouvez exécuter la commande `docker images` pour afficher des informations sur la nouvelle image :

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

L’ID de l’image sera différent sur votre ordinateur. À présent, exécutons l’application.

## <a name="start-a-container"></a>Démarrer un conteneur

Démarrez un conteneur en exécutant la commande `docker run` suivante :

```console
docker run -d --name randomanswers mvcrandomanswers
```

L’argument `-d` indique à Docker de démarrer l’image en mode détaché. Cela signifie que l’image Docker s’exécute déconnectée de l’interpréteur de commandes actuel.

Dans de nombreux exemples de dockers, vous pouvez voir -p pour cartographier le conteneur et les ports hôtes. L’image aspnet par défaut a déjà configuré le conteneur pour écouter sur le port 80 et l’exposer.

La portion `--name randomanswers` donne un nom au conteneur en cours d’exécution. Vous pouvez utiliser ce nom au lieu de l’ID de conteneur dans la plupart des commandes.

`mvcrandomanswers` est le nom de l’image à démarrer.

## <a name="verify-in-the-browser"></a>Vérifier dans le navigateur

Une fois le conteneur démarre, `http://localhost` connectez-vous au conteneur en marche à l’aide de l’exemple indiqué. Tapez cette URL dans votre navigateur ; vous devriez voir le site en cours d’exécution.

> [!NOTE]
> Certains logiciels VPN ou proxy peuvent vous empêcher d’accéder à votre site.
> Vous pouvez les désactiver temporairement pour vérifier que votre conteneur fonctionne.

Le répertoire d’exemples sur GitHub contient un [script PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) qui exécute ces commandes pour vous. Ouvrez une fenêtre PowerShell, accédez au répertoire de votre solution et tapez la commande suivante :

```console
./run.ps1
```

La commande ci-dessus construit l’image, affiche la liste des images sur votre machine, et démarre un conteneur.

Pour arrêter le conteneur, exécutez une commande `docker stop` :

```console
docker stop randomanswers
```

Pour supprimer le conteneur, exécutez une commande `docker rm` :

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Basculer vers le conteneur Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publier dans le système de &fichiers"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Publier les paramètres"
