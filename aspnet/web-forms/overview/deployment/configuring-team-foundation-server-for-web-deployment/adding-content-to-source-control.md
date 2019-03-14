---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Ajout de contenu au contrôle de code Source | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à un projet d’équipe...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050856"
---
<a name="adding-content-to-source-control"></a>Ajout de contenu au contrôle de code source
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à un projet d’équipe dans TFS, et elle explique comment ajouter des dépendances externes telles que des infrastructures ou des assemblys au contrôle de code source.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

## <a name="task-overview"></a>Présentation de la tâche

Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter du contenu à un contrôle de code source. Pour ajouter une solution au contrôle de code source dans TFS, vous devez suivre ces étapes générales :

- Se connecter à un projet d’équipe.
- Mapper la structure de dossiers du projet équipe sur le serveur à une structure de dossiers sur votre ordinateur local.
- Contrôle de code source, ajoutez la solution et son contenu.
- Ajoutez toutes les dépendances externes pour le contrôle de code source.

Cette rubrique sera vous montrent comment effectuer ces procédures.

Les tâches et les procédures pas à pas dans cette rubrique supposent que vous avez déjà créé un nouveau projet d’équipe TFS pour gérer votre contenu. Pour plus d’informations sur la création d’un nouveau projet d’équipe, consultez [création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Qui exécute ces procédures ?

Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter et modifier le contenu dans des projets d’équipe spécifique.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Connectez-vous à un projet d’équipe et créez un mappage de dossier

Avant d’ajouter n’importe quel contenu pour le contrôle de code source, vous devez vous connecter à un projet d’équipe et créer un mappage entre la structure de dossiers sur le serveur et le système de fichiers sur votre ordinateur local.

**Se connecter à un projet d’équipe et mapper un chemin d’accès local**

1. Sur votre station de travail de développement, ouvrez Visual Studio 2010.
2. Dans Visual Studio, sur le **équipe** menu, cliquez sur **se connecter à Team Foundation Server**.

    > [!NOTE]
    > Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 3 à 6.
3. Dans le **connexion au projet d’équipe** boîte de dialogue, cliquez sur **serveurs**.
4. Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **ajouter**.
5. Dans le **Ajouter Team Foundation Server** boîte de dialogue, fournissez les détails de votre instance TFS, puis cliquez sur **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **fermer**.
7. Dans le **se connecter au projet d’équipe** boîte de dialogue, sélectionnez l’instance TFS que vous souhaitez vous connecter à, sélectionnez l’équipe de collection de projets, sélectionnez le projet d’équipe que vous souhaitez ajouter à et puis cliquez sur **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Sur le **Explorateur du contrôle de Source** , cliquez sur **ne pas mappé**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Dans le **carte** boîte de dialogue le **dossier Local** boîte, accédez à (ou créez) un dossier local à agir en tant que le dossier racine du projet d’équipe, puis cliquez sur **carte**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Lorsque vous êtes invité à télécharger les fichiers sources, cliquez sur **Oui**.

    ![](adding-content-to-source-control/_static/image6.png)

À ce stade, vous avez mappé le dossier côté serveur pour le projet d’équipe à un dossier local sur votre station de travail de développeur. Vous avez également téléchargé le contenu existant du projet d’équipe à votre structure de dossier local. Vous pouvez maintenant commencer à ajouter votre propre contenu pour le contrôle de code source.

## <a name="add-projects-and-solutions-to-source-control"></a>Ajouter des projets et Solutions au contrôle de code Source

Pour ajouter des projets et solutions au contrôle de code source, vous devez tout d’abord les déplacer vers le dossier du projet d’équipe mappé sur votre ordinateur local. Vous pouvez ensuite vérifier dans le contenu à synchroniser vos ajouts avec le serveur.

**Pour ajouter des projets au contrôle de code source**

1. Sur votre station de travail de développeur, déplacez vos projets et solutions vers un emplacement approprié dans la structure de dossier mappé pour le projet d’équipe.

    > [!NOTE]
    > De nombreuses organisations ont une meilleure approche à comment les projets et solutions doivent être répartis dans le contrôle de code source. Pour obtenir des conseils sur la façon de structurer les dossiers, consultez [How To : Structure de vos dossiers de contrôle de code Source dans Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Ouvrez la solution dans Visual Studio 2010.
3. Dans le **l’Explorateur de solutions** fenêtre, avec le bouton droit de la solution, puis cliquez sur **ajouter la Solution au contrôle de code Source**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Dans certains cas, selon la façon dont votre organisation souhaite de structure de contenu dans TFS, vous devrez peut-être ajouter des projets au contrôle de code source individuellement pour fournir un contrôle plus précis sur la façon dont votre code source est organisé.
4. Vérifiez que le **Explorateur du contrôle de Source** onglet affiche le contenu que vous avez ajouté au sein de la structure de dossiers de serveur pour le projet d’équipe.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Le **Explorateur du contrôle de Source** onglet affiche votre contenu avec aucune autre invite puisque vous avez ajouté votre solution à un dossier mappé sur le système de fichiers local. Si votre solution se trouvait dans un emplacement non mappé, vous êtes invité à spécifier des emplacements de dossier dans TFS et votre système de fichiers local.
5. Sur le **Explorateur du contrôle de Source** sous l’onglet le **dossiers** volet, cliquez sur le projet d’équipe (par exemple, **ContactManager**), puis cliquez sur **archiver Modifications en attente**.
6. Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.

    ![](adding-content-to-source-control/_static/image9.png)

À ce stade, vous avez ajouté votre solution au contrôle de code source dans TFS.

## <a name="add-external-dependencies-to-source-control"></a>Ajouter des dépendances externes pour le contrôle de code Source

Lorsque vous ajoutez un projet ou une solution au contrôle de code source, tous les fichiers et dossiers au sein de votre projet ou solution seront également l’ajouter. Toutefois, dans de nombreux cas, projets et solutions également reposent sur des dépendances externes, tels que des assemblys locaux pour fonctionner correctement. Vous devez ajouter toutes ces ressources pour le contrôle de code source pour permettre à Team Build et des autres membres de l’équipe de développement générer votre code avec succès.

Par exemple, la structure de dossiers pour l’exemple de solution Contact Manager inclut un dossier nommé packages. Il contient l’assembly et les différentes ressources de prise en charge pour ADO.NET Entity Framework 4.1. Le dossier packages ne fait pas partie de la solution de Contact Manager, mais la solution ne sera pas généré avec succès sans lui. Pour activer Team Build générer la solution, vous devez ajouter le dossier packages de contrôle de code source.

> [!NOTE]
> L’inclusion d’un dossier de packages est typique de ce qui se passe lorsque vous ajoutez l’Entity Framework, ou des ressources similaires, à votre solution à l’aide de l’extension NuGet pour Visual Studio 2010.


**Pour ajouter du contenu de projet au contrôle de code source**

1. Assurez-vous que les éléments que vous souhaitez ajouter (par exemple, le dossier packages) sont dans un emplacement approprié au sein d’un dossier mappé sur votre système de fichiers local.
2. Dans Visual Studio 2010, dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Sur le **Explorateur du contrôle de Source** sous l’onglet le **dossiers** volet, sélectionnez le dossier qui contient l’élément ou d’éléments que vous souhaitez ajouter.
4. Cliquez sur le **ajouter des éléments au dossier** bouton.

    ![](adding-content-to-source-control/_static/image11.png)
5. Dans le **ajouter au contrôle de code Source** boîte de dialogue, sélectionnez le dossier ou les éléments que vous souhaitez ajouter, puis cliquez sur **suivant**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Sur le **éléments exclus** , sélectionnez tous les éléments requis qui ont été exclus automatiquement (par exemple, il s’agit d’assemblys), puis cliquez sur **inclure les éléments**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Sur le **éléments à ajouter** , vérifiez que tous les fichiers que vous souhaitez inclure sont répertoriées, puis cliquez sur **Terminer**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Dans le **Explorateur du contrôle de Source** fenêtre, cliquez sur le **archiver** bouton.

    ![](adding-content-to-source-control/_static/image15.png)
9. Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.

À ce stade, vous avez ajouté les dépendances externes pour votre solution au contrôle de code source.

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment se connecter à un projet d’équipe, mappez une structure de dossiers et ajouter du contenu à un contrôle de code source. Pour plus d’informations sur la façon de travailler avec des éléments sous contrôle de code source, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/library/ms181368.aspx).

La rubrique suivante, [configuration d’un serveur de Build TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md), explique comment préparer un serveur TFS Team Build pour générer et déployer votre solution.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des informations plus complètes sur l’utilisation de contrôle de code source dans TFS, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Précédent](creating-a-team-project-in-tfs.md)
> [Suivant](configuring-a-tfs-build-server-for-web-deployment.md)
