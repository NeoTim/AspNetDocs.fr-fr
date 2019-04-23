---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Passage d’Applications Web en mode hors connexion avec Web déployer | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment prendre une application web en mode hors connexion pendant la durée d’un déploiement automatisé à l’aide du dépl Web Internet Information Services (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 017eceb8567859fdbe28bb87af844eee20dfa525
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59415477"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Passage d’applications web hors connexion avec Web Deploy

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment prendre une application web en mode hors connexion pendant la durée d’un déploiement automatisé à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy). Les utilisateurs qui accèdent à l’application web sont redirigés vers un *application\_offline.htm* fichier jusqu'à ce que le déploiement est terminé.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Présentation de la tâche

Dans de nombreux scénarios, vous souhaiterez hors connexion d’une application web pendant que vous apportez des modifications à des composants associés, tels que les bases de données ou des services web. En règle générale, dans IIS et ASP.NET, vous y parvenir en plaçant un fichier nommé *application\_offline.htm* dans le dossier racine de l’application de site Web ou web IIS. Le *application\_offline.htm* fichier est un fichier HTML standard et contient généralement un simple message informant l’utilisateur que le site est temporairement indisponible en raison de la maintenance. Bien que le *application\_offline.htm* le fichier existe dans le dossier racine du site Web, IIS redirige automatiquement toutes les demandes vers le fichier. Lorsque vous avez terminé d’apporter des mises à jour, vous supprimez le *application\_offline.htm* de fichiers et le site Web reprend de servir les requêtes comme d’habitude.

Lorsque vous utilisez Web Deploy pour effectuer des déploiements automatisés ou étape unique à un environnement cible, vous souhaiterez incorporer Ajout et suppression de la *application\_offline.htm* fichier dans votre processus de déploiement. Pour ce faire, vous aurez besoin effectuer ces tâches de haut niveau :

- Dans le fichier de projet Microsoft Build Engine (MSBuild) qui vous permet de contrôler le processus de déploiement, créez une cible MSBuild qui copie un *application\_offline.htm* fichier sur le serveur de destination avant les tâches de déploiement commencer.
- Ajouter une autre cible MSBuild qui supprime le *application\_offline.htm* fichier à partir du serveur de destination lorsque toutes les tâches de déploiement sont terminées.
- Dans le projet d’application web, créez un *..WPP cible* fichier qui garantit qu’un *application\_offline.htm* fichier est ajouté au package de déploiement lorsque Web Deploy est appelé.

Cette rubrique sera vous montrent comment effectuer ces procédures. Les tâches et les procédures pas à pas dans cette rubrique supposent que vous avez déjà créé une solution qui contient au moins un projet d’application web, et que vous utilisez un fichier de projet personnalisé pour contrôler le processus de déploiement comme décrit dans [déploiement Web dans le Entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Vous pouvez également utiliser le [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemple de solution pour suivre les exemples dans la rubrique.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Ajout d’une application\_fichier hors connexion à un projet d’Application Web

La première tâche, vous devez suivre consiste à ajouter un *application\_hors connexion* fichier à votre projet d’application web :

- Pour empêcher le fichier d’interférer avec le processus de développement (vous ne souhaitez que votre application soit définitivement hors connexion), vous devez l’appeler quelque chose autre que *application\_offline.htm*. Par exemple, vous pouvez nommer le fichier *application\_hors connexion template.htm*.
- Pour empêcher le fichier d’être déployés en tant que-est, vous devez définir l’action de génération sur **aucun**.

**Pour ajouter une application\_fichier hors connexion à un projet d’application web**

1. Ouvrez votre solution dans Visual Studio 2010.
2. Dans le **l’Explorateur de solutions** fenêtre, cliquez sur votre projet d’application web, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.
3. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **HTML Page**.
4. Dans le **nom** , tapez **application\_hors connexion template.htm**, puis cliquez sur **ajouter**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Ajouter du code HTML simple pour informer les utilisateurs que l’application n’est pas disponible, puis enregistrez le fichier. N’incluez pas de balises côté serveur (par exemple, toutes les balises qui sont précédés de « asp : »). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Dans le **l’Explorateur de solutions** fenêtre, cliquez sur le nouveau fichier, puis cliquez sur **propriétés**.
7. Dans le **propriétés** fenêtre, dans le **Action de génération** ligne, sélectionnez **aucun**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Déploiement et la suppression d’une application\_fichier hors connexion

L’étape suivante consiste à modifier votre logique de déploiement pour copier le fichier vers le serveur de destination au début du processus de déploiement et le supprimer à la fin.

> [!NOTE]
> La procédure suivante suppose que vous utilisez un fichier de projet MSBuild personnalisé pour contrôler votre processus de déploiement, comme décrit dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Si vous effectuez un déploiement direct à partir de Visual Studio, vous devez utiliser une approche différente. Sayed Ibrahim Hashimi décrit une telle approche dans [comment prendre votre application en mode hors connexion au cours de publication sur le Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Pour déployer un *application\_hors connexion* fichier vers un site Web IIS de destination, vous devez appeler à l’aide de MSDeploy.exe le [Web Deploy **contentPath** fournisseur](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Le **contentPath** fournisseur prend en charge les chemins d’accès du répertoire physique et les chemins de site Web ou application IIS, ce qui rend le choix idéal pour la synchronisation d’un fichier entre un dossier de projet Visual Studio et une application web IIS. Pour déployer le fichier, votre commande MSDeploy doit ressembler à ceci :


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Pour supprimer le fichier à partir du site de destination à la fin du processus de déploiement, votre commande MSDeploy doit ressembler à ceci :


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Pour automatiser ces commandes en tant que partie d’un processus de génération et de déploiement, vous devez les intégrer dans votre fichier de projet MSBuild personnalisée. La procédure suivante décrit comment effectuer cette opération.

**Pour déployer et supprimer une application\_fichier hors connexion**

1. Dans Visual Studio 2010, ouvrez le fichier projet MSBuild qui contrôle le processus de déploiement. Dans le [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemple de solution, il s’agit du *Publish.proj* fichier.
2. Dans la racine **projet** élément, créez un **PropertyGroup** élément à stocker les variables pour le *application\_hors connexion* déploiement :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Le **SourceRoot** propriété est définie ailleurs dans le *Publish.proj* fichier. Cette propriété indique l’emplacement du dossier racine pour le contenu source par rapport à l’emplacement actuel&#x2014;en d’autres termes, par rapport à l’emplacement de la *Publish.proj* fichier.
4. Le **contentPath** fournisseur n’accepte pas les chemins d’accès relatif du fichier, vous devez obtenir un chemin d’accès absolu au fichier source avant de pouvoir le déployer. Vous pouvez utiliser la [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) tâche pour ce faire.
5. Ajouter un nouveau **cible** élément nommé **GetAppOfflineAbsolutePath**. Dans cette cible, utilisez le **ConvertToAbsolutePath** tâche pour obtenir un chemin d’accès absolu pour le *application\_hors connexion de modèle* fichier dans votre dossier de projet.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Cette cible prend le chemin d’accès relatif à la *application\_hors connexion de modèle* fichier dans votre dossier de projet et l’enregistre dans une nouvelle propriété comme un chemin d’accès absolu du fichier. Le **BeforeTargets** attribut spécifie que vous souhaitez que cette cible à exécuter avant le **DeployAppOffline** cible, que vous allez créer à l’étape suivante.
7. Ajouter une nouvelle cible nommée **DeployAppOffline**. Dans cette cible, appeler la commande MSDeploy.exe qui déploie votre *application\_hors connexion* fichier sur le serveur web de destination.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Dans cet exemple, le **ContactManagerIisPath** propriété est définie ailleurs dans le fichier projet. Il s’agit simplement un chemin d’application IIS, sous la forme *[nom du site Web IIS] / [nom de l’Application]*. Y compris une condition dans la cible permet aux utilisateurs de basculer le *application\_hors connexion* déploiement ou désactiver en modifiant une valeur de propriété ou en fournissant un paramètre de ligne de commande.
9. Ajouter une nouvelle cible nommée **DeleteAppOffline**. Dans cette cible, appeler la commande MSDeploy.exe qui supprime votre *application\_hors connexion* fichier à partir du serveur web de destination.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. La dernière tâche consiste à appeler ces nouvelles cibles aux endroits appropriés lors de l’exécution de votre fichier projet. Pour cela, de différentes manières. Par exemple, dans le *Publish.proj* fichier, le **FullPublishDependsOn** propriété spécifie une liste de cibles qui doivent être exécutées en ordre lorsque la **FullPublish** par défaut la cible est appelée.
11. Modifiez votre fichier de projet MSBuild pour appeler le **DeployAppOffline** et **DeleteAppOffline** cibles aux endroits appropriés dans le processus de publication.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Lorsque vous exécutez votre fichier projet MSBuild personnalisé, le *application\_hors connexion* fichier sera déployé sur le serveur immédiatement après une génération réussie. Il sera supprimé à partir du serveur puis une fois que toutes les tâches de déploiement sont terminées.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Ajout d’une application\_fichier hors connexion aux Packages de déploiement

Selon la façon dont vous configurez votre déploiement, les contenus existants à la destination IIS application web&#x2014;comme le *application\_offline.htm* fichier&#x2014;peuvent être supprimées automatiquement lorsque vous déployez un site web package à la destination. Pour vous assurer que le *application\_offline.htm* fichier reste en place pour la durée du déploiement, vous devez inclure le fichier dans le package de déploiement web lui-même en outre à déployer le fichier directement au début de le processus de déploiement.

- Si vous avez suivi les tâches précédentes dans cette rubrique, vous aurez ajouté le *application\_offline.htm* fichier à votre projet d’application web sous un nom de fichier différent (nous avons utilisé *application\_ en mode hors connexion template.htm*) et vous devez définir l’action de génération **aucun**. Ces modifications sont nécessaires pour éviter que le fichier à partir d’interférer avec le développement et le débogage. Par conséquent, vous devez personnaliser le processus d’empaquetage pour vous assurer que le *application\_offline.htm* fichier est inclus dans le package de déploiement web.

Web Publishing Pipeline (WPP) utilise une liste d’éléments nommée **FilesForPackagingFromProject** pour générer une liste de fichiers qui doivent être inclus dans le package de déploiement web. Vous pouvez personnaliser le contenu de vos packages web en ajoutant vos propres éléments à cette liste. Pour ce faire, vous devez suivre ces étapes générales :

1. Créez un fichier de projet personnalisé nommé *[nom_projet].wpp.targets* dans le même dossier que votre fichier projet.

    > [!NOTE]
    > Le *..WPP cible* fichier doit aller dans le même dossier que votre fichier de projet d’application web&#x2014;par exemple, *ContactManager.Mvc.csproj*&#x2014;plutôt que dans le même dossier que n’importe quel personnalisé fichiers de projet que vous utilisez pour contrôler le processus de génération et de déploiement.
2. Dans le *..WPP cible* de fichiers, créez une nouvelle cible MSBuild qui exécute *avant* le **CopyAllFilesToSingleFolderForPackage** cible. Il s’agit de la cible WPP qui génère la liste des éléments à inclure dans le package.
3. Dans la nouvelle cible, créez un **ItemGroup** élément.
4. Dans le **ItemGroup** élément, ajoutez un **FilesForPackagingFromProject** d’élément et spécifiez le *application\_offline.htm* fichier.

Le *..WPP cible* fichier doit ressembler à ceci :


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Voici les points clés de la Remarque Dans cet exemple :

- Le **BeforeTargets** d’insertions d’attribut dans les fournisseurs de services en spécifiant cette cible qui il doit être exécuté immédiatement avant le **CopyAllFilesToSingleFolderForPackage** cible.
- Le **FilesForPackagingFromProject** élément utilise le **DestinationRelativePath** valeur des métadonnées pour renommer le fichier à partir de *application\_hors connexion template.htm* pour *application\_offline.htm* car il est ajouté à la liste.

La procédure suivante vous montre comment ajouter cela *..WPP cible* fichier à un projet d’application web.

**Pour ajouter un. fichier.WPP cible à un package de déploiement web**

1. Ouvrez votre solution dans Visual Studio 2010.
2. Dans le **l’Explorateur de solutions** fenêtre, avec le bouton droit de votre nœud de projet d’application web (par exemple, **ContactManager.Mvc**), pointez sur **ajouter**, puis cliquez sur **Un nouvel élément**.
3. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **fichier XML** modèle.
4. Dans le **nom** , tapez *[nom_projet] ***.wpp.targets** (par exemple, **ContactManager.Mvc.wpp.targets**), puis cliquez sur **ajouter**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Si vous ajoutez un nouvel élément vers le nœud racine d’un projet, le fichier est créé dans le même dossier que le fichier projet. Vous pouvez le vérifier en ouvrant le dossier dans l’Explorateur Windows.
5. Dans le fichier, ajoutez le balisage de MSBuild décrit précédemment.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Enregistrez et fermez le *[nom_projet].wpp.targets* fichier.

La prochaine génération et package de votre projet d’application web, les fournisseurs de services détecte automatiquement le *..WPP cible* fichier. Le *application\_hors connexion template.htm* fichier figureront dans le package de déploiement web qui en résulte que *application\_offline.htm*.

> [!NOTE]
> Si votre déploiement échoue, le *application\_offline.htm* fichier restent en place et de votre application reste hors connexion. C’est généralement le comportement souhaité. Pour remettre votre application en ligne, vous pouvez supprimer le *application\_offline.htm* fichier à partir de votre serveur web. Vous pouvez également, si vous corrigez les erreurs éventuelles et que vous exécutez un déploiement réussi, le *application\_offline.htm* fichier sera supprimé.


## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment prendre une application web en mode hors connexion pendant la durée d’un déploiement, en publiant un *application\_offline.htm* de fichiers sur le serveur de destination au début du processus de déploiement et la suppression à la fin. Vous avez également vu comment inclure un *application\_offline.htm* fichier dans un package de déploiement web.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur l’empaquetage et le processus de déploiement, consultez [génération et empaquetage Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configuration des paramètres pour le déploiement du Package Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), et [ Déploiement de Packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Si vous publiez vos applications web directement à partir de Visual Studio, plutôt que d’à l’aide de l’approche de fichier de projet MSBuild personnalisée décrite dans ces didacticiels, vous devez utiliser une approche légèrement différente pour mettre votre application hors ligne pendant la publication processus. Pour plus d’informations, consultez [comment faire passer votre application web en mode hors connexion lors de la publication](https://go.microsoft.com/?linkid=9805135) (billet de blog).

> [!div class="step-by-step"]
> [Précédent](excluding-files-and-folders-from-deployment.md)
> [Suivant](running-windows-powershell-scripts-from-msbuild-project-files.md)
