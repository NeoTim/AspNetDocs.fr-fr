---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: La Solution de gestionnaire de contacts | Microsoft Docs
author: jrjlee
description: Cette série de didacticiels utilise un exemple de solution&#x2014;la solution Gestionnaire de contacts&#x2014;pour représenter une application d’entreprise avec un niveau réaliste...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130934"
---
# <a name="the-contact-manager-solution"></a>La solution Gestionnaire de contacts

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cela [série de didacticiels](web-deployment-in-the-enterprise.md) utilise un exemple de solution&#x2014;la solution Gestionnaire de contacts&#x2014;pour représenter une application d’entreprise avec un niveau réaliste de complexité. Cette rubrique présente la solution de gestionnaire de contacts, décrit les composants clés de la solution et identifie les problèmes liés au déploiement de ce type d’application sur différentes plateformes de destination dans un environnement d’entreprise.
> 
> Lorsque vous travaillez dans les rubriques dans ces didacticiels, vous pouvez utiliser la solution de gestionnaire de contacts en tant qu’une implémentation de référence qui montre comment vous pouvez répondre aux défis spécifiques dans les scénarios de déploiement d’entreprise. La rubrique suivante, [paramètre de la Solution Gestionnaire de contacts](setting-up-the-contact-manager-solution.md), décrit comment télécharger et exécuter la solution sur votre station de travail de développeur.

## <a name="solution-overview"></a>Présentation de la solution

La solution de Contact Manager se compose de quatre projets individuels :

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Il s’agit d’un projet d’application web ASP.NET MVC 3 qui représente le point d’entrée pour la solution. Il offre des fonctionnalités d’application web de base, comme offrant aux utilisateurs la possibilité de créer et d’afficher les détails du contact. L’application s’appuie sur un service Windows Communication Foundation (WCF) pour gérer les contacts et une base de données ASP.NET application services pour gérer l’authentification et l’autorisation.
- **ContactManager.Database**. Il s’agit d’un projet de base de données Visual Studio. Le projet définit le schéma d’une base de données que les magasins de coordonnées.
- **ContactManager.Service**. Il s’agit d’un projet de service web WCF. L’expose de service WCF un point de terminaison qui permet aux appelants d’effectuer créer, récupérer, mettre à jour, opérations et de suppression (CRUD) sur le **ContactManager** base de données. Le service s’appuie sur le **ContactManager** base de données et la **ContactManager.Common.dll** assembly.
- **ContactManager.Common**. Il s’agit d’un projet de bibliothèque de classes. Le service WCF s’appuie sur les types définis dans cet assembly.

La solution inclut également un dossier de solution nommé publier. Il contient les différents fichiers de projet personnalisés et les fichiers de commandes qui illustrent comment vous pouvez contrôler et manipuler le processus de génération et de déploiement. Elles sont traitées plus en détail plus loin dans ce didacticiel.

À un niveau conceptuel, les composants de la solution s’assemblent comme suit :

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Bien que l’application web ASP.NET MVC 3 utilise le fournisseur d’appartenances ASP.NET, toutes les pages au sein de l’application web autorise l’accès anonyme. Cette configuration n’est clairement pas réaliste. Toutefois, la solution est configurée de cette façon pour le rendre plus facile à déployer et tester la solution sans configuration des rôles et des comptes d’utilisateur.

## <a name="deployment-challenges"></a>Défis du déploiement

La solution de gestionnaire de contacts illustre plusieurs défis du déploiement qui sont communs à un grand nombre de scénarios de déploiement d’entreprise :

- La solution se compose de plusieurs projets dépendants. Vous devez déployer ces projets simultanément.
- Chaînes de connexion et les points de terminaison de service doivent être mis à jour pour chaque environnement, et dans de nombreux cas ces informations seront pas disponibles pour les développeurs.
- Lorsque vous déployez le **ContactManager** base de données pour les environnements intermédiaire et de production, vous devez préserver les données existantes sur les déploiements suivants.
- Lorsque vous déployez la base de services d’application ASP.NET, vous devez déployer certaines données de configuration, mais ignorez les données de compte utilisateur.
- Les projets incluent des fichiers et dossiers qui ne doivent pas être déployés. Vous devez exclure ces fichiers et dossiers à partir du processus de déploiement.
- La solution doit prendre en charge de déploiement automatisé à partir d’un serveur de builds de Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusion

Cette rubrique fourni une vue d’ensemble de la solution de gestionnaire de contacts et identifié certains des défis inhérents de déploiement qui sont communs à un grand nombre de scénarios de déploiement d’entreprise. Les autres rubriques de ce didacticiel décrivent certaines des techniques que vous pouvez utiliser pour relever ces défis.

La rubrique suivante, [paramètre de la Solution Gestionnaire de contacts](setting-up-the-contact-manager-solution.md), décrit comment télécharger et exécuter la solution sur votre station de travail de développeur.

> [!div class="step-by-step"]
> [Précédent](web-deployment-in-the-enterprise.md)
> [Suivant](setting-up-the-contact-manager-solution.md)
