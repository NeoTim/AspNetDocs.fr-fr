---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personnalisation des déploiements de base de données pour plusieurs environnements | Microsoft Docs
author: jrjlee
description: 'Cette rubrique décrit comment personnaliser les propriétés d’une base de données pour les environnements cibles spécifiques dans le cadre du processus de déploiement. Remarque : La rubrique suppose th...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65108310"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Personnalisation de déploiements de base de données pour plusieurs environnements

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment personnaliser les propriétés d’une base de données pour les environnements cibles spécifiques dans le cadre du processus de déploiement.
> 
> > [!NOTE]
> > La rubrique suppose que vous déployez un projet de base de données de Visual Studio 2010 à l’aide de MSBuild.exe et VSDBCMD.exe. Pour plus d’informations sur la raison pour laquelle vous pouvez choisir cette approche, consultez [déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) et [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Lorsque vous déployez un projet de base de données vers plusieurs destinations, vous aurez souvent besoin de personnaliser les propriétés de déploiement de base de données pour chaque environnement cible. Par exemple, dans les environnements de test vous serez généralement recréer la base de données sur chaque déploiement, tandis que dans les environnements de production ou intermédiaire vous serait beaucoup plus susceptibles de rendre les mises à jour incrémentielles pour préserver vos données.
> 
> Dans un projet de base de données Visual Studio 2010, les paramètres de déploiement sont contenus dans un fichier de configuration (.sqldeployment) de déploiement. Cette rubrique sera vous montrent comment créer des fichiers de configuration spécifiques à l’environnement de déploiement et de spécifier celui que vous souhaitez utiliser comme paramètre VSDBCMD.

Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Présentation de la tâche

Cette rubrique suppose que :

- Vous utilisez l’approche des fichiers projet partagé pour le déploiement d’une solution, comme décrit dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Vous appelez VSDBCMD à partir du fichier projet pour déployer votre projet de base de données, comme décrit dans [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Pour créer un système de déploiement qui prend en charge de faire varier les propriétés de déploiement de base de données entre les environnements cibles, vous devrez :

- Créez un fichier de configuration (.sqldeployment) de déploiement pour chaque environnement cible.
- Créer une commande VSDBCMD qui spécifie le fichier de configuration de déploiement comme un commutateur de ligne de commande.
- Paramétrer la commande VSDBCMD dans un fichier de projet Microsoft Build Engine (MSBuild), afin que les options VSDBCMD sont appropriées à l’environnement cible.

Cette rubrique sera vous montrent comment effectuer chacune de ces procédures.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Création de fichiers de Configuration spécifiques à l’environnement de déploiement

Par défaut, un projet de base de données contient un fichier de configuration de déploiement unique nommé *Database.sqldeployment*. Si vous ouvrez ce fichier dans Visual Studio 2010, vous pouvez voir différentes options de déploiement qui sont à votre disposition :

- **Classement de la comparaison déploiement**. Cela vous permet de choisir s’il faut utiliser le classement de base de données de votre projet (le *source* classement) ou le classement de base de données de votre serveur de destination (le *cible* classement). Dans la plupart des cas, vous souhaiterez utiliser le classement source lors de la déployer dans un environnement de développement ou de l’environnement de test. Lorsque vous déployez dans un environnement intermédiaire ou de production, vous souhaiterez généralement conserver le classement cible inchangé pour éviter tout problème d’interopérabilité.
- **Déployer les propriétés de la base de données**. Cela vous permet de choisir s’il faut appliquer les propriétés de la base de données, tel que défini dans le *Database.sqlsettings* fichier. Lorsque vous déployez une base de données pour la première fois, vous devez déployer les propriétés de la base de données. Si vous mettez à jour une base de données existante, les propriétés doivent être déjà en place et vous n’avez pas besoin de les déployer de nouveau.
- **Toujours recréer la base de données**. Ce vous permet de choisir s’il faut recréer la base de données cible chaque fois que vous déployez ou apportez des modifications incrémentielles à mettre la base de données cible à jour avec votre schéma. Si vous recréez la base de données, vous perdrez toutes les données dans la base de données existante. Par conséquent, vous devez généralement définir cela sur **false** pour les déploiements dans un environnement intermédiaire ou de production.
- **Bloquer le déploiement incrémentiel si une perte de données peut se produire**. Cela vous permet de choisir si déploiement doit s’arrêter si une modification au schéma de base de données entraîne la perte de données. Vous définissez ces propriétés **true** pour un déploiement dans un environnement de production, pour vous donner la possibilité d’intervenir et de protéger toutes les données importantes. Si vous avez défini **base de données de la recréer toujours** à **false**, ce paramètre n’a aucun effet.
- **Exécuter un déploiement en mode mono-utilisateur**. Cela n’est pas généralement un problème dans les environnements de développement ou de test. Toutefois, vous devez généralement définir cela **true** pour les déploiements dans un environnement intermédiaire ou de production. Cela empêche les utilisateurs d’apporter des modifications à la base de données pendant le déploiement.
- **Sauvegarder la base de données avant le déploiement**. Vous définissez ces propriétés **true** lorsque vous déployez dans un environnement de production, afin d’éviter une perte de données. Vous pouvez également lui affecter **true** lorsque vous déployez dans un environnement intermédiaire, si votre base de données intermédiaire contient une grande quantité de données.
- **Générer des instructions DROP pour les objets dans la base de données cible, mais qui ne sont pas dans le projet de base de données**. Dans la plupart des cas, il s’agit d’une partie intégrale et essentielle des modifications incrémentielles apportées à une base de données. Si vous avez défini **base de données de la recréer toujours** à **false**, ce paramètre n’a aucun effet.
- **N’utilisez pas d’instructions ALTER ASSEMBLY pour mettre à jour les types CLR**. Ce paramètre détermine comment SQL Server doit mettre à jour les types common language runtime (CLR) aux plus récentes versions d’assembly. Cela doit être défini sur **false** dans la plupart des scénarios.

Ce tableau montre les paramètres de déploiement classique pour différents environnements de destination. Toutefois, vos paramètres peuvent différer selon vos besoins exacts.

|  | Développement/Test | Intégration/la mise en lots | Production |
| --- | --- | --- | --- |
| **Classement de comparaison de déploiement** | Source | une cible | une cible |
| **Déployer les propriétés de la base de données** | True | Uniquement la première fois | Uniquement la première fois |
| **Toujours recréer la base de données** | True | False | False |
| **Bloquer le déploiement incrémentiel si une perte de données peut se produire.** | False | Peut-être | True |
| **Exécutez le script de déploiement en mode mono-utilisateur** | False | True | True |
| **Sauvegarder la base de données avant le déploiement** | False | Peut-être | True |
| **Générer des instructions DROP pour les objets dans la base de données cible, mais qui ne sont pas dans le projet de base de données** | False | True | True |
| **N’utilisez pas d’instructions ALTER ASSEMBLY pour mettre à jour les types CLR** | False | False | False |

> [!NOTE]
> Pour plus d’informations sur les propriétés de déploiement de base de données et les considérations sur l’environnement, consultez [une vue d’ensemble de base de données de paramètres de projet](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [Comment : Configurer les propriétés des détails du déploiement](https://msdn.microsoft.com/library/dd172125.aspx), [générer et déployer la base de données à un environnement de développement isolé](https://msdn.microsoft.com/library/dd193409.aspx), et [créer et déployer des bases de données dans un environnement intermédiaire ou d’un environnement de Production](https://msdn.microsoft.com/library/dd193413.aspx).

Pour prendre en charge le déploiement d’un projet de base de données vers plusieurs destinations, vous devez créer un fichier de configuration de déploiement pour chaque environnement cible.

**Pour créer un fichier de configuration spécifiques à l’environnement**

1. Dans Visual Studio 2010, dans le **l’Explorateur de solutions** fenêtre, cliquez sur votre projet de base de données, puis cliquez sur **propriétés**.
2. Dans la page de propriétés de projet de base de données, sur le **déployer** sous l’onglet le **fichier de configuration de déploiement** de ligne, cliquez sur **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Dans le **nouveau fichier de Configuration de déploiement** boîte de dialogue zone, attribuez au fichier un nom explicite (par exemple, **TestEnvironment.sqldeployment**), puis cliquez sur **enregistrer**.
4. Sur le *[nom_fichier] *** .sqldeployment** page, définissez les propriétés de déploiement qui répondent aux besoins de votre environnement de destination, puis enregistrez le fichier.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Notez que le nouveau fichier est ajouté au dossier des propriétés dans votre projet de base de données.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>En spécifiant le fichier de Configuration de déploiement dans VSDBCMD

Lorsque vous utilisez des configurations de solution (par exemple, Debug et Release) de Visual Studio 2010, vous pouvez associer un fichier de configuration de déploiement avec chaque configuration. Lorsque vous créez une configuration particulière, le processus de génération génère un fichier de manifeste de déploiement spécifique de la configuration qui pointe vers le fichier de configuration de déploiement spécifique de la configuration. Toutefois, un des objectifs principaux de l’approche de déploiement décrite dans ces didacticiels est donnons la possibilité de contrôler le processus de déploiement sans utiliser Visual Studio 2010 et les configurations de solutions. Dans cette approche, la configuration de solution est le même quel que soit l’environnement de déploiement cible. Pour personnaliser votre déploiement de base de données dans un environnement de destination spécifique, vous pouvez utiliser les options de ligne de commande VSDBCMD pour spécifier votre fichier de configuration de déploiement.

Pour spécifier un fichier de configuration de déploiement dans votre VSDBCMD, utilisez le **p:/DeploymentConfigurationFile** basculer et fournir le chemin d’accès complet à votre fichier. Cela remplace le fichier de configuration de déploiement qui identifie le manifeste de déploiement. Par exemple, vous pouvez utiliser cette commande VSDBCMD pour déployer le **ContactManager** base de données à un environnement de test :

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Notez que le processus de génération peut renommer votre fichier .sqldeployment lorsqu’il copie le fichier vers le répertoire de sortie.

Si vous utilisez des variables de commande SQL dans vos scripts SQL de prédéploiement ou de post-déploiement, vous pouvez utiliser une approche similaire pour associer un fichier spécifique à l’environnement .sqlcmdvars à votre déploiement. Dans ce cas, vous utilisez le **p:/SqlCommandVariablesFile** commutateur pour identifier votre fichier .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>La commande VSDBCMD en cours d’exécution à partir d’un fichier projet MSBuild

Vous pouvez appeler une commande VSDBCMD à partir d’un fichier projet MSBuild à l’aide un **Exec** tâche dans une cible MSBuild. Dans sa forme la plus simple, elle ressemblerait à ceci :

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- Dans la pratique, pour rendre vos fichiers projet facile à lire et à réutiliser, vous souhaiterez créer des propriétés pour stocker les différents paramètres de ligne de commande. Cela rend plus facile pour les utilisateurs à fournir des valeurs de propriété dans un fichier de projet spécifique à l’environnement ou à remplacer les valeurs par défaut à partir de la ligne de commande MSBuild. Si vous utilisez l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier de projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), vous devez diviser vos instructions de génération et les propriétés entre les deux fichiers en conséquence :
- Les paramètres spécifiques à l’environnement, telles que le nom de fichier de configuration de déploiement, la chaîne de connexion de base de données et le nom de la base de données cible doit être placé dans le fichier de projet spécifique à l’environnement.
- La cible MSBuild qui exécute la commande VSDBCMD, ainsi que toutes les propriétés telles que l’emplacement de l’exécutable VSDBCMD, universelles doit être placé dans le fichier de projet d’application universelle.

Vous devez également vous assurer que vous générez le projet de base de données avant de vous appelez VSDBCMD afin que le fichier .deploymanifest est créé et prêt à être utilisé. Vous pouvez voir un exemple complet de cette approche dans la rubrique [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), qui vous guide dans les fichiers projet dans le [solution d’exemple de gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment vous pouvez adapter les propriétés de base de données pour différents environnements de destination lorsque vous déployez des projets de base de données à l’aide de MSBuild et VSDBCMD. Cette approche est utile lorsque vous avez besoin déployer des projets de base de données dans le cadre de la plus grande, solutions d’échelle de l’entreprise. Ces solutions sont souvent déployées vers plusieurs destinations, telles que des environnements sandbox de développement ou de test, intermédiaire ou l’intégration des plates-formes et production ou des environnements en direct. Chacun de ces environnements cible nécessite généralement un ensemble unique de propriétés de déploiement de base de données.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur le déploiement des projets de base de données à l’aide de VSDBCMD.exe, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisées pour contrôler le processus de déploiement, consultez [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Ces articles sur MSDN fournissent des instructions plus générales sur le déploiement de la base de données :

- [Une vue d’ensemble de paramètres de projet de base de données](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Guide pratique pour Configurer les propriétés des détails du déploiement](https://msdn.microsoft.com/library/dd172125.aspx)
- [Créer et déployer des bases de données dans un environnement de développement isolé](https://msdn.microsoft.com/library/dd193409.aspx)
- [Créer et déployer des bases de données dans un environnement intermédiaire ou d’un environnement de Production](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Précédent](performing-a-what-if-deployment.md)
> [Suivant](deploying-database-role-memberships-to-test-environments.md)
