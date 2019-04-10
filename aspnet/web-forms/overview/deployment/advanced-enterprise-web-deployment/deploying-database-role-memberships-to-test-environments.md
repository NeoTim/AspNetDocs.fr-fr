---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Déploiement d’appartenances aux rôles de base de données pour les environnements de Test | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment ajouter des comptes d’utilisateurs aux rôles de base de données en tant que partie d’un déploiement de solution dans un environnement de test. Lorsque vous déployez une solution contenant...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: fd0914ed62a280fea290b9f1b150fc25c8ed6d40
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59385330"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Déploiement d’appartenances aux rôles de base de données dans les environnements de test

par [Jason Lee](https://github.com/jrjlee)

[Télécharger le PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment ajouter des comptes d’utilisateurs aux rôles de base de données en tant que partie d’un déploiement de solution dans un environnement de test.
> 
> Lorsque vous déployez une solution contenant un projet de base de données dans un environnement intermédiaire ou de production, vous ne voulez généralement le développeur pour automatiser l’ajout de comptes d’utilisateurs aux rôles de base de données. Dans la plupart des cas, le développeur ne saura pas les comptes d’utilisateur doivent être ajoutés pour les rôles de base de données, et ces exigences peut changer à tout moment. Toutefois, lorsque vous déployez une solution contenant un projet de base de données à un environnement de développement ou l’environnement de test, la situation est généralement assez différente :
> 
> - Le développeur généralement ré-déploie la solution sur une base régulière, souvent plusieurs fois par jour.
> - La base de données est généralement recréé à chaque déploiement, ce qui signifie que les utilisateurs de base de données doivent être créés et ajoutés aux rôles après chaque déploiement.
> - Le développeur a généralement un contrôle total sur l’environnement de développement ou de test cible.
> 
> Dans ce scénario, il est souvent avantageux de créer des utilisateurs de base de données automatiquement et d’affecter des appartenances aux rôles de base de données dans le cadre du processus de déploiement.
> 
> Le facteur clé est que cette opération doit être conditionnel en fonction de l’environnement cible. Si vous déployez dans un environnement intermédiaire ou un environnement de production, vous souhaitez ignorer l’opération. Si vous effectuez un déploiement à un développeur ou l’environnement de test, que vous souhaitez déployer des appartenances aux rôles sans aucune autre intervention. Cette rubrique décrit une approche que vous pouvez utiliser pour relever ce défi.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Présentation de la tâche

Cette rubrique suppose que :

- Vous utilisez l’approche des fichiers projet partagé pour le déploiement d’une solution, comme décrit dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Vous appelez VSDBCMD à partir du fichier projet pour déployer votre projet de base de données, comme décrit dans [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Pour créer des utilisateurs de base de données et affecter des appartenances aux rôles lorsque vous déployez un projet de base de données dans un environnement de test, vous devrez :

- Créer un script Transact Structured Query Language (Transact-SQL) qui apporte les modifications de base de données nécessaires.
- Créer une cible de Microsoft Build Engine (MSBuild) qui utilise l’utilitaire sqlcmd.exe pour exécuter le script SQL.
- Configurez vos fichiers projet pour appeler la cible lorsque vous déployez votre solution dans un environnement de test.

Cette rubrique sera vous montrent comment effectuer chacune de ces procédures.

## <a name="scripting-the-database-role-memberships"></a>Les appartenances aux rôles de base de données de script

Vous pouvez créer un script Transact-SQL dans un grand nombre de façons différentes, et dans n’importe quel emplacement que vous choisissez. L’approche la plus simple consiste à créer le script au sein de votre solution dans Visual Studio 2010.

**Pour créer un script SQL**

1. Dans le **l’Explorateur de solutions** fenêtre, développez le nœud de votre projet de base de données.
2. Cliquez sur le **Scripts** dossier, pointez sur **ajouter**, puis cliquez sur **nouveau dossier**.
3. Type **Test** comme nom de dossier, puis appuyez sur ENTRÉE.
4. Cliquez sur le **Test** dossier, pointez sur **ajouter**, puis cliquez sur **Script**.
5. Dans le **ajouter un nouvel élément** boîte de dialogue zone, donnez un nom significatif à votre script (par exemple, **AddRoleMemberships.sql**), puis cliquez sur **ajouter**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Dans le *AddRoleMemberships.sql* , ajoutez les instructions Transact-SQL qui :

    1. Créer un utilisateur de base de données pour la connexion SQL Server qui accède à votre base de données.
    2. Ajouter l’utilisateur de base de données à des rôles de base de données requis.
7. Le fichier doit ressembler à ceci :

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Enregistrez le fichier.

## <a name="executing-the-script-on-the-target-database"></a>L’exécution du Script sur la base de données cible

Dans l’idéal, vous exécuteriez tous les scripts Transact-SQL nécessaires dans le cadre d’un script de post-déploiement lorsque vous déployez votre projet de base de données. Toutefois, scripts de post-déploiement ne vous permettent d’exécuter la logique conditionnelle basée sur les configurations de solutions ou les propriétés de génération. L’alternative consiste à exécuter vos scripts SQL directement à partir du fichier de projet MSBuild, en créant un **cible** élément qui exécute une commande sqlcmd.exe. Vous pouvez utiliser cette commande pour exécuter votre script sur la base de données cible :


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Pour plus d’informations sur les options de ligne de commande sqlcmd, consultez [utilitaire sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Avant de vous incorporez cette commande dans une cible MSBuild, vous devez envisager les conditions dans lesquelles le script à exécuter :

- La base de données cible doit exister avant de modifier ses appartenances aux rôles. Par conséquent, vous devez exécuter ce script *après* le déploiement de la base de données.
- Vous devez inclure une condition afin que le script est exécuté uniquement pour les environnements de test.
- Si vous exécutez un déploiement « que se passe-t-il si »&#x2014;en d’autres termes, si vous êtes génération de scripts de déploiement, mais pas en cours d’exécution les&#x2014;vous ne devez pas exécuter le script SQL.

Si vous utilisez l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier de projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), comme illustré dans l’exemple de solution de gestionnaire de contacts, vous pouvez fractionner les instructions de génération pour votre script SQL comme suit :

- Toute requise des propriétés spécifiques à l’environnement, ainsi que la propriété qui détermine s’il faut déployer des autorisations, doit être placé dans le fichier de projet spécifique à l’environnement (par exemple, *Env-Dev.proj*).
- La cible MSBuild, ainsi que toutes les propriétés qui ne change pas entre les environnements de destination, doit être placé dans le fichier de projet d’application universelle (par exemple, *Publish.proj*).

Dans le fichier de projet spécifique à un environnement, vous devez définir le nom du serveur de base de données, le nom de la base de données cible et une propriété booléenne qui permet à l’utilisateur de spécifier s’il faut déployer les appartenances aux rôles.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


Dans le fichier de projet d’application universelle, vous devez fournir l’emplacement de l’exécutable sqlcmd et l’emplacement du script SQL que vous souhaitez exécuter. Ces propriétés restent les mêmes, quel que soit l’environnement de destination. Vous devez également créer une cible MSBuild pour exécuter la commande sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Notez que vous ajoutez l’emplacement de l’exécutable sqlcmd une propriété statique, comme cela peut être utile à d’autres cibles. En revanche, définir l’emplacement de votre script SQL et la syntaxe de la commande sqlcmd en tant que propriétés dynamiques au sein de la cible, car ils ne seront pas requis avant l’exécution de la cible. Dans ce cas, le **DeployTestDBPermissions** cible est exécutée uniquement si ces conditions sont remplies :

- Le **DeployTestDBRoleMemberships** propriété est définie sur **true**.
- L’utilisateur n’a pas spécifié un **WhatIf = true** indicateur.

Enfin, n’oubliez pas d’appeler la cible. Dans le *Publish.proj* fichier, vous pouvez ce faire, ajoutez la cible à la liste des dépendances pour la valeur par défaut **FullPublish** cible. Vous devez vous assurer que le **DeployTestDBPermissions** cible n’est pas exécutée avant la **PublishDbPackages** cible a été exécutée.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusion

Cette rubrique décrit une façon dans laquelle vous pouvez ajouter des utilisateurs de base de données et les appartenances aux rôles comme une action de post-déploiement lorsque vous déployez un projet de base de données. Cela est généralement utile lorsque vous régulièrement re-créer une base de données dans un environnement de test, mais il doit généralement être évitée lorsque vous déployez des bases de données dans les environnements de production ou intermédiaire. Par conséquent, vous devez vous assurer que vous utilisez la logique conditionnelle nécessaire afin que les utilisateurs de base de données et les appartenances aux rôles sont créés uniquement quand il convient de le faire.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur l’utilisation de VSDBCMD pour déployer des projets de base de données, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour obtenir des conseils sur la personnalisation des déploiements de base de données pour les environnements cibles différents, consultez [personnalisation des déploiements de base de données pour plusieurs environnements](customizing-database-deployments-for-multiple-environments.md). Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisées pour contrôler le processus de déploiement, consultez [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Pour plus d’informations sur les options de ligne de commande sqlcmd, consultez [utilitaire sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Précédent](customizing-database-deployments-for-multiple-environments.md)
> [Suivant](deploying-membership-databases-to-enterprise-environments.md)
