---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configuration d’un serveur de base de données pour le Web de publication Deploy | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment configurer un serveur de base de données SQL Server 2008 R2 pour prendre en charge la publication et déploiement web. Les tâches décrites dans cette rubrique sont co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131573"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configuration d’un serveur de base de données pour la publication Web Deploy

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment configurer un serveur de base de données SQL Server 2008 R2 pour prendre en charge la publication et déploiement web.
> 
> Les tâches décrites dans cette rubrique sont communes à chaque scénario de déploiement&#x2014;peu importe si vos serveurs web sont configurés pour utiliser le Service de l’Agent distant d’outil de déploiement Web IIS (Web Deploy), le Gestionnaire de déploiement Web ou le déploiement en mode hors connexion ou votre application s’exécute sur un serveur web unique ou une batterie de serveurs. La façon de déployer la base de données peut-être changer en fonction des exigences de sécurité et d’autres considérations. Par exemple, vous pouvez déployer la base de données avec ou sans les exemples de données, et peut déployer des mappages de rôle d’utilisateur ou les configurer manuellement après le déploiement. Toutefois, la manière dont vous configurez le serveur de base de données reste le même.

Vous n’êtes pas obligé d’installer des produits supplémentaires ou des outils à la configuration d’un serveur de base de données pour prendre en charge le déploiement web. En supposant que votre serveur de base de données et votre serveur web s’exécutent sur des ordinateurs différents, vous devez simplement :

- Autoriser SQL Server à communiquer à l’aide de TCP/IP.
- Autoriser le trafic de SQL Server via les pare-feux.
- Donner le compte ordinateur du serveur web à une connexion SQL Server.
- Mapper la connexion de compte d’ordinateur à des rôles de base de données requis.
- Accordez au compte qui exécutera le déploiement une autorisations de créateur de connexion et de la base de données SQL Server.
- Pour prendre en charge les déploiements répétés, mapper la connexion de compte de déploiement pour le **db\_propriétaire** rôle de base de données.

Cette rubrique sera vous montrent comment effectuer chacune de ces procédures. Les tâches et les procédures pas à pas dans cette rubrique supposent que vous démarrez avec une instance par défaut de SQL Server 2008 R2 est en cours d’exécution sur Windows Server 2008 R2. Avant de continuer, vérifiez que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installés.
- Le serveur est joint au domaine.
- Le serveur a une adresse IP statique.
- SQL Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installés.

L’instance de SQL Server doit uniquement inclure la **Services moteur de base de données** rôle, qui est inclus automatiquement dans toute installation de SQL Server. Toutefois, pour faciliter la configuration et de maintenance, nous vous conseillons d’inclure le **outils de gestion – de base** et **outils de gestion – complet** rôles de serveur.

> [!NOTE]
> Pour plus d’informations sur la jonction des ordinateurs à un domaine, consultez [joindre des ordinateurs au domaine et journalisation](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration des adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Pour plus d’informations sur l’installation de SQL Server, consultez [l’installation de SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Activer l’accès à distance à SQL Server

SQL Server utilise TCP/IP pour communiquer avec des ordinateurs distants. Si votre serveur de base de données et votre serveur web se trouvent sur des ordinateurs différents, vous devez :

- Configurer les paramètres de mise en réseau de SQL Server pour permettre la communication via TCP/IP.
- Configurer les pare-feu matériels ou logiciels pour autoriser le trafic TCP (et dans certains cas protocole UDP (User Datagram) le trafic) sur les ports utilisés par l’instance de SQL Server.

Pour activer SQL Server de communiquer via TCP/IP, utilisez le Gestionnaire de Configuration SQL Server pour modifier la configuration réseau pour votre instance de SQL Server.

**Pour activer SQL Server de communiquer à l’aide de TCP/IP**

1. Sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, cliquez sur **outils de Configuration**, puis cliquez sur **Gestionnaire de Configuration SQL Server**.
2. Dans le volet d’arborescence, développez **Configuration du réseau SQL Server**, puis cliquez sur **protocoles pour MSSQLSERVER**.

   > [!NOTE]
   > Si vous avez installé plusieurs instances de SQL Server, vous verrez un <strong>protocoles pour</strong><em>[NOM_INSTANCE]</em> élément pour chaque instance. Vous devez configurer les paramètres réseau sur une instance par instance base.
3. Dans le volet détails, cliquez sur le **TCP/IP** de ligne, puis cliquez sur **activer**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Dans le **avertissement** boîte de dialogue, cliquez sur **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Vous devez redémarrer le service MSSQLSERVER que votre nouvelle configuration de réseau prennent effet. Vous pouvez faire à une invite de commandes, à partir de la console Services, ou à partir de SQL Server Management Studio. Dans cette procédure, vous allez utiliser SQL Server Management Studio.
6. Fermez le Gestionnaire de Configuration SQL Server.
7. Sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, puis cliquez sur **SQL Server Management Studio**.
8. Dans le **se connecter au serveur** boîte de dialogue le **nom du serveur** zone, tapez le nom du serveur de base de données, puis cliquez sur **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Dans le **Explorateur d’objets** volet, cliquez sur le nœud de serveur parent (par exemple, **TESTDB1**), puis cliquez sur **redémarrer**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Dans le **Microsoft SQL Server Management Studio** boîte de dialogue, cliquez sur **Oui**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Lorsque le service a redémarré, fermez SQL Server Management Studio.

Pour autoriser le trafic de SQL Server à travers un pare-feu, vous devez d’abord savoir quels ports à l’aide de votre instance de SQL Server. Cela varie selon la façon dont l’instance de SQL Server a été créé et configuré :

- Un *instance par défaut* de SQL Server écoute (et répond aux) demandes sur le port TCP 1433.
- Un *instance nommée* de SQL Server écoute (et répond aux) demandes sur un port TCP attribué dynamiquement.
- Si le service SQL Server Browser est activé, les clients peuvent interroger le service sur le port UDP 1434 pour déterminer le port TCP à utiliser pour une instance particulière de SQL Server. Toutefois, ce service est souvent désactivé pour des raisons de sécurité.

En supposant que vous utilisez une instance par défaut de SQL Server, vous devez configurer votre pare-feu pour autoriser le trafic.

| Sens | À partir du Port | Au Port | Type de port |
| --- | --- | --- | --- |
| Trafic entrant | Any | 1433 | TCP |
| Sortant | 1433 | Any | TCP |

> [!NOTE]
> Techniquement, un ordinateur client utilisera un port TCP attribué de façon aléatoire entre 1024 et 5000 pour communiquer avec SQL Server, et vous pouvez limiter vos règles de pare-feu en conséquence. Pour plus d’informations sur les ports SQL Server et les pare-feu, consultez [les numéros de port TCP/IP nécessaires pour communiquer à SQL via un pare-feu](https://go.microsoft.com/?linkid=9805125) et [Comment : Configurer un serveur pour écouter un port de TCP spécifique (Gestionnaire de Configuration SQL Server)](https://msdn.microsoft.com/library/ms177440.aspx).

Dans la plupart des environnements Windows Server, vous devrez probablement configurer des pare-feu de Windows sur le serveur de base de données. Par défaut, les pare-feu Windows autorise tout le trafic sortant, sauf si une règle lui interdit spécifiquement. Pour activer votre serveur web pour atteindre votre base de données, vous devez configurer une règle entrante qui autorise le trafic TCP sur le numéro de port qui utilise l’instance de SQL Server. Si vous utilisez une instance par défaut de SQL Server, vous pouvez utiliser la procédure suivante pour configurer cette règle.

**Pour configurer le pare-feu Windows pour permettre la communication avec une instance de SQL Server par défaut**

1. Sur le serveur de base de données, sur le **Démarrer** menu, pointez sur **outils d’administration**, puis cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**.
2. Dans le volet d’arborescence, cliquez sur **règles de trafic entrant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Dans le **Actions** volet, sous **règles de trafic entrant**, cliquez sur **nouvelle règle**.
4. Dans le trafic entrant Assistant Nouvelle règle, sur le **Type de règle** page, sélectionnez **Port**, puis cliquez sur **suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Sur le **protocole et Ports** page, vérifiez que **TCP** est sélectionnée, puis, dans le **ports locaux spécifiques** , tapez **1433**, puis cliquez sur **Suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Sur le **Action** page, laissez l’option **autoriser la connexion** sélectionné, cliquez sur **suivant**.
7. Sur le **profil** page, laissez l’option **domaine** sélectionné, désactivez le **privé** et **Public** cases à cocher, puis cliquez sur  **Suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Sur le **nom** page, donnez un nom descriptif convenablement à la règle (par exemple, **instance par défaut de SQL Server – accès réseau**), puis cliquez sur **Terminer**.

Pour plus d’informations sur la configuration des pare-feu de Windows pour SQL Server, en particulier si vous avez besoin communiquer avec SQL Server sur les ports non standards ou dynamiques, consultez [Comment : Configurer un pare-feu Windows pour accéder au moteur de base de données](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurer des connexions et autorisations de base de données

Lorsque vous déployez une application web pour Internet Information Services (IIS), l’application s’exécute à l’aide de l’identité du pool d’applications. Dans un environnement de domaine, les identités du pool d’applications utilisent le compte d’ordinateur du serveur sur lequel elles s’exécutent à accéder aux ressources réseau. Comptes d’ordinateur prennent la forme <em>[Nom_domaine]</em><strong>\</strong ><em>[nom_machine]</em><strong>$</strong>&#x2014;, par exemple, <strong>FABRIKAM\TESTWEB1$</strong>. Pour permettre à votre application web pour accéder à une base de données sur le réseau, vous devez :

- Ajouter une connexion pour le compte ordinateur du serveur web à l’instance de SQL Server.
- Mapper la connexion de compte d’ordinateur à des rôles de base de données requis (en général **db\_datareader** et **db\_datawriter**).

Si votre application web s’exécute sur une batterie de serveurs, plutôt que sur un seul serveur, vous devez répéter ces procédures pour chaque serveur web de la batterie de serveurs.

> [!NOTE]
> Pour plus d’informations sur les identités du pool d’applications et l’accès aux ressources réseau, consultez [identités du Pool d’applications](https://go.microsoft.com/?linkid=9805123).

Vous pouvez considérer ces tâches de différentes manières. Pour créer la connexion, vous pouvez :

- Créez la connexion manuellement sur le serveur de base de données, à l’aide de Transact-SQL ou SQL Server Management Studio.
- Utiliser un projet de serveur SQL Server 2008 dans Visual Studio pour créer et déployer la connexion.

Une connexion SQL Server est un objet au niveau du serveur, plutôt que sur un objet de niveau de base de données, il n’est pas dépendante de la base de données que vous souhaitez déployer. Par conséquent, vous pouvez créer la connexion à tout moment, et l’approche la plus simple est souvent créer manuellement la connexion sur le serveur de base de données avant de commencer le déploiement de bases de données. Vous pouvez utiliser la procédure suivante pour créer une connexion dans SQL Server Management Studio.

**Pour créer une connexion SQL Server pour le compte ordinateur du serveur web**

1. Sur le serveur de base de données, sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, puis cliquez sur **SQL Server Management Studio** .
2. Dans le **se connecter au serveur** boîte de dialogue le **nom du serveur** zone, tapez le nom du serveur de base de données, puis cliquez sur **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Dans le **Explorateur d’objets** volet, avec le bouton droit **sécurité**, pointez sur **New**, puis cliquez sur **connexion**.
4. Dans le **nouvelle connexion** boîte de dialogue le **nom_compte_de_connexion** , tapez le nom de votre compte ordinateur du serveur web (par exemple, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Cliquez sur **OK**.

À ce stade, votre serveur de base de données est prêt pour la publication Web Deploy. Toutefois, les solutions que vous déployez ne fonctionnent pas jusqu'à ce que vous mappez la connexion de compte d’ordinateur pour les rôles de base de données requis. Mappage de la connexion à des rôles de base de données nécessite beaucoup plus de réflexion, en tant que vous ne peut pas rôles de la carte jusqu'à ce qu’après avoir avez déployé la base de données. Pour mapper la connexion de compte d’ordinateur pour les rôles de base de données requis, vous pouvez :

- Attribuer les rôles de base de données à la connexion manuellement, une fois que vous avez déployé la base de données pour la première fois.
- Utiliser un script de post-déploiement pour attribuer les rôles de base de données à la connexion.

Pour plus d’informations sur l’automatisation de la création des connexions et des mappages de rôle de base de données, consultez [déploiement des appartenances de rôle de base de données pour les environnements de Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Ou bien, vous pouvez utiliser la procédure suivante pour mapper la connexion de compte d’ordinateur pour les rôles de base de données requis manuellement. N’oubliez pas que vous ne pouvez pas effectuer cette procédure jusqu'à ce que *après* vous avez déployé la base de données.

**Pour mapper des rôles de base de données pour la connexion au compte ordinateur serveur web**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans le **Explorateur d’objets** volet, développez le **sécurité** nœud, développez le **connexions** nœud, puis double-cliquez sur la connexion de compte d’ordinateur (par exemple,  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Dans le **propriétés de la connexion** boîte de dialogue, cliquez sur **mappage de l’utilisateur**.
4. Dans le **utilisateurs mappés à cette connexion** table, sélectionnez le nom de votre base de données (par exemple, **ContactManager**).
5. Dans le **appartenance au rôle de la base de données :** *[nom de la base de données]* , sélectionnez les autorisations requises. Dans le cas de l’exemple de solution du Gestionnaire de contacts, vous devez sélectionner le **db\_datareader** et **db\_datawriter** rôles.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Cliquez sur **OK**.

Alors que le mappage manuellement les rôles de base de données est souvent plus largement suffisante pour les environnements de test, il est moins précise pour les déploiements automatisés ou un seul clic aux environnements de production ou intermédiaire. Vous trouverez plus d’informations sur l’automatisation de ce type de tâche à l’aide de scripts de post-déploiement dans [déploiement des appartenances de rôle de base de données pour les environnements de Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Pour plus d’informations sur les projets serveur et de base de données, consultez [projets de base de données Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Configurer des autorisations pour le compte de déploiement

Si le compte que vous utiliserez pour exécuter le déploiement n’est pas un administrateur de SQL Server, vous devrez également créer une connexion pour ce compte. Pour créer la base de données, le compte doit être un membre de la **dbcreator** rôle de serveur ou disposer d’autorisations équivalentes.

> [!NOTE]
> Lorsque vous utilisez Web Deploy ou VSDBCMD pour déployer une base de données, vous pouvez utiliser les informations d’identification Windows ou SQL Server (si votre instance de SQL Server est configuré pour prendre en charge l’authentification en mode mixte). La procédure suivante suppose que vous souhaitez utiliser les informations d’identification Windows, mais rien ne vous empêche de spécifier un nom d’utilisateur SQL Server et le mot de passe dans votre chaîne de connexion lorsque vous configurez le déploiement.

**Pour définir des autorisations pour le compte de déploiement**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans le **Explorateur d’objets** volet, avec le bouton droit **sécurité**, pointez sur **New**, puis cliquez sur **connexion**.
3. Dans le **nouvelle connexion** boîte de dialogue le **nom_compte_de_connexion** , tapez le nom de votre compte de déploiement (par exemple, **FABRIKAM\matt**).
4. Dans le **sélectionner une page** volet, cliquez sur **rôles de serveur**.
5. Sélectionnez **dbcreator**, puis cliquez sur **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Pour prendre en charge les déploiements suivants, vous devrez également ajouter le compte de déploiement à le **db\_propriétaire** sur la base de données après le premier déploiement. Il s’agit, car sur les déploiements suivants vous êtes la modification du schéma de base de données existante, au lieu de créer une nouvelle base de données. Comme décrit dans la section précédente, vous ne pouvez pas ajouter un utilisateur à un rôle de base de données jusqu'à ce que vous avez créé la base de données, pour des raisons évidentes.

**Pour mapper la connexion de compte de déploiement pour la base de données\_rôle de propriétaire de la base de données**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans le **Explorateur d’objets** fenêtre, développez le **sécurité** nœud, développez le **connexions** nœud, puis double-cliquez sur la connexion de compte d’ordinateur (par exemple,  **FABRIKAM\matt**).
3. Dans le **propriétés de la connexion** boîte de dialogue, cliquez sur **mappage de l’utilisateur**.
4. Dans le **utilisateurs mappés à cette connexion** table, sélectionnez le nom de votre base de données (par exemple, **ContactManager**).
5. Dans le **appartenance au rôle de la base de données :** *[nom de la base de données]* liste, sélectionnez le **db\_propriétaire** rôle.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Cliquez sur **OK**.

## <a name="conclusion"></a>Conclusion

Votre serveur de base de données doit désormais être prêt à accepter des déploiements de base de données distante et pour autoriser les serveurs web IIS à distance accéder à vos bases de données. Avant de tenter de déployer et utiliser des bases de données, vous souhaiterez probablement vérifier ces points clés :

- Vous avez configuré SQL Server pour accepter les connexions TCP/IP distantes ?
- Avez-vous configuré tous les pare-feu pour autoriser le trafic SQL Server ?
- Si vous avez créé une connexion de compte d’ordinateur pour chaque serveur web qui accède à SQL Server ?
- Votre déploiement de base de données inclut-il un script pour créer des mappages de rôle d’utilisateur, ou vous devez les créer manuellement une fois que vous déployez la base de données pour la première fois ?
- Vous avez créé une connexion pour le compte de déploiement et ajouté à la **dbcreator** rôle de serveur ?

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur le déploiement des projets de base de données, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour obtenir des conseils sur la création des appartenances aux rôles de base de données en exécutant un script de post-déploiement, consultez [déploiement des appartenances de rôle de base de données pour les environnements de Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pour obtenir des conseils sur la façon de relever les défis de déploiement unique qui présentent des bases de données d’appartenance, consultez [déploiement de bases de données d’appartenance pour les environnements d’entreprise](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Précédent](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Suivant](creating-a-server-farm-with-the-web-farm-framework.md)
