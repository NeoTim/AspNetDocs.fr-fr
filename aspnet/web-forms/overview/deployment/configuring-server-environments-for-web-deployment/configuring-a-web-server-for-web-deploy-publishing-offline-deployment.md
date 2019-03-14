---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configuration d’un serveur Web pour le Web de publication (déploiement hors connexion) Deploy | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment configurer un serveur web IIS pour prendre en charge le déploiement et la publication web en mode hors connexion. Lorsque vous travaillez avec Internet Information Services (je...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: eab69e93417ddedea9214523de34697b044a871e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063206"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configuration d’un serveur web pour la publication Web Deploy (Déploiement hors connexion)
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment configurer un serveur web IIS pour prendre en charge le déploiement et la publication web en mode hors connexion.
> 
> Lorsque vous travaillez avec Internet Information Services (IIS) outil de déploiement Web (Web Deploy) 2.0 ou version ultérieure, il existe trois approches principales, que vous pouvez utiliser pour obtenir vos applications ou les sites sur un serveur web. Vous pouvez :
> 
> - Utilisez le *Web Deploy Service de l’Agent distant*. Cette approche nécessite moins de configuration du serveur web, mais vous devez fournir les informations d’identification d’un administrateur de serveur local pour pouvoir déployer quoi que ce soit sur le serveur.
> - Utilisez le *Gestionnaire Web Deploy*. Cette approche est beaucoup plus complexe et nécessite plus d’efforts initiale pour configurer le serveur web. Toutefois, lorsque vous utilisez cette approche, vous pouvez configurer IIS pour autoriser les utilisateurs non administrateurs effectuer le déploiement. Le Gestionnaire de déploiement Web est uniquement disponible dans IIS 7 ou version ultérieure.
> - Utilisez *déploiement hors connexion*. Cette approche nécessite une configuration minimale du serveur web, mais un administrateur de serveur doit manuellement copier le package web sur le serveur et importez-le via le Gestionnaire des services Internet.
> 
> Pour plus d’informations sur les fonctionnalités clés, les avantages et les inconvénients de ces approches, consultez [choix de l’approche de droite pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md).


Oui, si vos restrictions de sécurité ou d’infrastructure de réseau empêchent le déploiement à distance. Cela est probablement le cas dans les environnements de production sur Internet, où les serveurs web sont isolés&#x2014;soit physiquement ou par des pare-feu et des sous-réseaux&#x2014;du reste de votre infrastructure de serveur.

Évidemment, cette approche se révèle moins souhaitable si vos applications web sont mis à jour régulièrement. Si votre infrastructure autorise, vous souhaiterez l’activation du déploiement à distance, à l’aide du Gestionnaire de déploiement Web ou Web Deploy Remote Agent Service.

## <a name="task-overview"></a>Présentation de la tâche

Pour configurer le serveur web pour prendre en charge d’importation hors connexion et déploiement des packages web, vous devrez :

- Installer IIS 7.5 et la configuration recommandée pour IIS 7.
- Installez Web Deploy 2.1 ou ultérieure.
- Créer un site Web IIS pour héberger le contenu a été déployé.
- Désactiver le Service de l’Agent de déploiement Web.

Pour héberger l’exemple de solution en particulier, vous devez également :

- Installer le Kit de développement .NET Framework 4.0.
- Installez ASP.NET MVC 3.

Cette rubrique sera vous montrent comment effectuer chacune de ces procédures. Les tâches et les procédures pas à pas dans cette rubrique supposent que vous démarrez avec une version de serveur propre exécutant Windows Server 2008 R2. Avant de continuer, vérifiez que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installés.
- Le serveur est joint au domaine.
- Le serveur a une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction des ordinateurs à un domaine, consultez [joindre des ordinateurs au domaine et journalisation](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration des adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Installer les produits et composants

Cette section va vous guider dans l’installation des produits requis et composants sur le serveur web. Avant de commencer, une bonne pratique consiste à exécuter des mises à jour de Windows pour vous assurer que votre serveur est à jour.

Dans ce cas, vous devez installer ces éléments :

- **Configuration recommandée pour IIS 7**. Cela permet la **serveur Web (IIS)** rôle sur votre serveur web et installe le jeu de modules IIS et les composants dont vous avez besoin pour héberger une application ASP.NET.
- **.NET framework 4.0**. Cela est nécessaire pour exécuter des applications qui ont été créées sur cette version du .NET Framework.
- **Outil de déploiement 2.1 ou ultérieure Web**. Cette opération installe Web Deploy (et son fichier exécutable sous-jacent, MSDeploy.exe) sur votre serveur. Web Deploy s’intègre à IIS et vous permet d’importer et exporter des packages web.
- **ASP.NET MVC 3**. Cette opération installe les assemblys que vous avez besoin pour exécuter des applications MVC 3.

> [!NOTE]
> Cette procédure pas à pas décrit l’utilisation de Web Platform Installer pour installer et configurer les différents composants. Bien que vous n’êtes pas obligé d’utiliser Web Platform Installer, il simplifie le processus d’installation en détectant les dépendances automatiquement et en garantissant que vous obtenez toujours les dernières versions de produit. Pour plus d’informations, consultez [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).


**Pour installer les produits requis et les composants**

1. Téléchargez et installez le [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Lors de l’installation est terminée, le programme d’installation de la plateforme Web se lance automatiquement.

    > [!NOTE]
    > Vous pouvez maintenant lancer le programme Web Platform Installer à tout moment à partir de la **Démarrer** menu. Pour ce faire, sur le **Démarrer** menu, cliquez sur **tous les programmes**, puis cliquez sur **Microsoft Web Platform Installer**.
3. En haut de la **Web Platform Installer 3.0** fenêtre, cliquez sur **produits**.
4. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **infrastructures**.
5. Dans le **Microsoft .NET Framework 4** de ligne, si le .NET Framework n’est pas déjà installé, cliquez sur **ajouter**.

    > [!NOTE]
    > Vous avez peut-être déjà installé le .NET Framework 4.0 via Windows Update. Si un produit ou composant est déjà installé, le programme d’installation de la plateforme Web indiquera en remplaçant le **ajouter** bouton avec le texte **installé**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Dans le **ASP.NET MVC 3 (Visual Studio 2010)** de ligne, cliquez sur **ajouter**.
7. Dans le volet de navigation, cliquez sur **Server**.
8. Dans le **Configuration IIS 7 recommandée** de ligne, cliquez sur **ajouter**.
9. Dans le **2.1 d’outil de déploiement Web** de ligne, cliquez sur **ajouter**.
10. Cliquez sur **Installer**. Le programme d’installation de la plateforme Web affichera une liste de produits&#x2014;ainsi que les dépendances associées&#x2014;doit être installé et vous invite à accepter les termes du contrat de licence.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Passez en revue les termes du contrat de licence et si vous acceptez les termes du contrat, cliquez sur **J’accepte**.
12. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez le **Web Platform Installer 3.0** fenêtre.

Si vous avez installé le .NET Framework 4.0 avant d’avoir installé IIS, vous devez exécuter le [outil ASP.NET IIS Registration](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) pour inscrire la dernière version d’ASP.NET avec IIS. Si vous ne le faites pas, vous trouverez que IIS distribuera le contenu statique (par exemple, des fichiers HTML) sans problème, mais elle retournera **404.0 d’erreur HTTP – introuvable** lorsque vous tentez d’accéder au contenu ASP.NET. Vous pouvez utiliser la procédure suivante pour vous assurer que ASP.NET 4.0 est inscrit.

**Pour inscrire ASP.NET 4.0 avec IIS**

1. Cliquez sur **Démarrer**, puis tapez **invite de commandes**.
2. Dans les résultats de recherche, cliquez sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.
3. Dans la fenêtre d’invite de commandes, accédez à la **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Tapez la commande suivante et appuyez sur ENTRÉE :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Si vous envisagez d’héberger des applications web 64 bits à tout moment, vous devez également inscrire la version 64 bits d’ASP.NET avec IIS. Pour ce faire, dans la fenêtre d’invite de commandes, accédez à la **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Tapez la commande suivante et appuyez sur ENTRÉE :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Comme une bonne pratique, utilisez Windows Update à nouveau à ce stade pour télécharger et installer les mises à jour disponibles pour les nouveaux produits et les composants que vous avez installé.

## <a name="configure-the-iis-website"></a>Configurer le site Web IIS

Avant de pouvoir déployer un contenu web à votre serveur, vous devez créer et configurer un site Web IIS pour héberger le contenu. Web Deploy peut déployer uniquement les packages web pour un site de Web IIS existant ; Il ne peut pas créer le site Web pour vous. À un niveau élevé, vous aurez besoin effectuer ces tâches :

- Créez un dossier sur le système de fichiers pour héberger votre contenu.
- Créer un site Web IIS pour fournir le contenu et l’associer avec le dossier local.
- Des permissions de lecture à l’identité de pool d’applications sur le dossier local.

Bien que rien ne vous empêche de déployer du contenu sur le site Web par défaut dans IIS, cette approche n’est pas recommandée pour autre chose que les scénarios de test ou de démonstration. Pour simuler un environnement de production, vous devez créer un nouveau site Web IIS avec des paramètres qui sont spécifiques aux besoins de votre application.

**Pour créer et configurer un site Web IIS**

1. Sur le système de fichiers local, créez un dossier pour stocker votre contenu (par exemple, **C:\DemoSite**).
2. Sur le **Démarrer** menu, pointez sur **outils d’administration**, puis cliquez sur **Internet Information Services (IIS) Manager**.
3. Dans le Gestionnaire des services Internet, dans le **connexions** volet, développez le nœud du serveur (par exemple, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Cliquez sur le **Sites** nœud, puis cliquez sur **ajouter un Site Web**.
5. Dans le **nom du Site** , tapez un nom pour le site Web IIS (par exemple, **DemoSite**).
6. Dans le **chemin d’accès physique** , tapez (ou accédez à) le chemin d’accès à votre dossier local (par exemple, **C:\DemoSite**).
7. Dans le **Port** , tapez le numéro de port sur lequel vous souhaitez héberger le site Web (par exemple, **85**).

    > [!NOTE]
    > Les numéros de port standard sont 80 pour HTTP et 443 pour HTTPS. Toutefois, si vous hébergez ce site Web sur le port 80, vous devez arrêter le site Web par défaut avant que vous pouvez accéder à votre site.
8. Laissez le **nom d’hôte** boîte vide, sauf si vous souhaitez configurer un enregistrement de nom de domaine (DNS) pour le site Web, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > Dans un environnement de production, vous souhaiterez probablement héberger votre site Web sur le port 80 et de configurer un en-tête d’hôte, ainsi que les enregistrements DNS correspondants. Pour plus d’informations sur la configuration des en-têtes d’hôte dans IIS 7, consultez [configurer un en-tête d’hôte pour un Site Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Pour plus d’informations sur le rôle de serveur DNS dans Windows Server 2008 R2, consultez [vue d’ensemble du serveur DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) et [serveur DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Dans le volet **Actions** , sous **Modifier le Site**, cliquez sur **Liaisons**.
10. Dans le **liaisons de Site** boîte de dialogue, cliquez sur **ajouter**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Dans le **ajouter la liaison de Site** boîte de dialogue, définissez la **adresse IP** et **Port** pour correspondre à la configuration de votre site existant.
12. Dans le **nom d’hôte** , tapez le nom de votre serveur web (par exemple, **PROWEB1**), puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > La première liaison de site vous permet d’accéder au site localement à l’aide de l’adresse IP et le port ou `http://localhost:85`. La deuxième liaison de site vous permet d’accéder au site à partir d’autres ordinateurs sur le domaine à l’aide du nom de l’ordinateur (par exemple, http://proweb1:85).
13. Dans le **liaisons de Site** boîte de dialogue, cliquez sur **fermer**.
14. Dans le **connexions** volet, cliquez sur **Pools d’applications**.
15. Dans le **Pools d’applications** volet, cliquez sur le nom de votre pool d’applications, puis cliquez sur **paramètres de base**. Par défaut, le nom de votre pool d’applications correspond au nom de votre site Web (par exemple, **DemoSite**).
16. Dans le **version du .NET Framework** liste, sélectionnez **.NET Framework v4.0.30319**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > L’exemple de solution nécessite .NET Framework 4.0. Cela n’est pas en général une exigence pour Web Deploy.

Dans l’ordre de votre site Web de distribuer du contenu, l’identité de pool d’applications doit avoir autorisations de lecture sur le dossier local qui stocke le contenu. Dans IIS 7.5, les pools d’applications s’exécutent avec une identité de pool d’applications unique, par défaut (contrairement aux versions précédentes d’IIS, où des pools d’applications exécutent typiquement en utilisant le compte de Service réseau). L’identité de pool d’applications n’est pas un compte d’utilisateur réel et n’apparaît pas sur toutes les listes d’utilisateurs ou groupes&#x2014;au lieu de cela, il est créé dynamiquement lorsque le pool d’applications est démarré. Chaque identité de pool d’applications est ajoutée à la variable locale **IIS\_IUSRS** groupe de sécurité comme un élément caché.

Pour accorder des autorisations à une identité de pool d’applications sur un fichier ou dossier, que vous avez deux options :

- Affecter des autorisations à l’identité de pool d’applications directement, en utilisant le format <strong>IIS AppPool\</strong ><em>[nom de pool d’applications]</em>(par exemple, <strong>IIS AppPool\DemoSite</strong>).
- Assigner des autorisations pour le **IIS\_IUSRS** groupe.

L’approche la plus courante consiste à attribuer des autorisations à l’ordinateur local **IIS\_IUSRS** de groupe, étant donné que cette approche vous permet de modifier des pools d’applications sans reconfigurer les autorisations de système de fichiers. La procédure suivante utilise cette approche basée sur le groupe.

> [!NOTE]
> Pour plus d’informations sur les identités du pool d’applications dans IIS 7.5, consultez [identités du Pool d’applications](https://go.microsoft.com/?linkid=9805123).


**Pour configurer les autorisations de dossier pour un site Web IIS**

1. Dans l’Explorateur Windows, accédez à l’emplacement de votre dossier local.
2. Cliquez avec le bouton droit, puis cliquez sur **propriétés**.
3. Sur le **sécurité** , cliquez sur **modifier**, puis cliquez sur **ajouter**.
4. Cliquez sur **emplacements**. Dans le **emplacements** boîte de dialogue, sélectionnez le serveur local, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Dans le **sélectionner des utilisateurs ou groupes** boîte de dialogue, tapez **IIS\_IUSRS**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.
6. Dans le <strong>autorisations pour</strong><em>[nom du dossier]</em> boîte de dialogue, notez que le nouveau groupe a été affecté le <strong>en lecture &amp; exécuter</strong>, <strong>Lister le dossier contenu</strong>, et <strong>en lecture</strong> autorisations par défaut. Laissez ce inchangé et cliquez sur <strong>OK</strong>.
7. Cliquez sur <strong>OK</strong> pour fermer la <em>[nom du dossier]</em><strong>propriétés</strong> boîte de dialogue.

## <a name="disable-the-remote-agent-service"></a>Désactiver le Service de l’Agent distant

Lorsque vous installez Web Deploy, le Service de l’Agent de déploiement Web est installé et démarré automatiquement. Ce service vous permet de déployer et publier des packages web à partir d’un emplacement distant. Vous n’utiliserez pas la fonctionnalité de déploiement à distance dans ce scénario, donc vous devez arrêter et désactiver le service.

> [!NOTE]
> Vous n’avez pas besoin d’arrêter le service de l’agent distant afin d’importer et déployer un package web manuellement. Toutefois, il est conseillé d’arrêter et désactiver le service si vous ne souhaitez pas l’utiliser.


Vous pouvez arrêter et désactiver un service de plusieurs façons, à l’aide de différents utilitaires de ligne de commande ou les applets de commande Windows PowerShell. Cette procédure décrit une approche basée sur l’interface utilisateur simple.

**Pour arrêter et désactiver le service de l’agent distant**

1. Sur le menu **Démarrer** , pointez sur **Outils d'administration**, puis cliquez sur **Services**.
2. Dans la console Services, recherchez le **Service de l’Agent de déploiement Web** ligne.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Avec le bouton droit **Service de l’Agent de déploiement Web**, puis cliquez sur **propriétés**.
4. Dans le **propriétés de Service de l’Agent de déploiement Web** boîte de dialogue, cliquez sur **arrêter**.
5. Dans le **type de démarrage** liste, sélectionnez **désactivé**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusion

À ce stade, votre serveur web est prêt pour le déploiement du package web hors connexion. Avant de tenter d’importer des packages web pour un site Web IIS, vous souhaiterez probablement vérifier ces points clés :

- Se sont inscrits d’ASP.NET 4.0 avec IIS ?
- L’identité de pool d’applications dispose d’un accès en lecture au dossier source pour votre site Web ?
- Avez arrêté le Service de l’Agent de déploiement de Web ?

> [!div class="step-by-step"]
> [Précédent](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [Suivant](configuring-a-database-server-for-web-deploy-publishing.md)
