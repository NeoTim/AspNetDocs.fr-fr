---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : résolution des problèmes | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b42476fca18b04f4557a216ee205cfd9220023e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576096"
---
# <a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : dépannage

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

Cette page décrit certains problèmes courants qui peuvent survenir lorsque vous déployez une application Web ASP.NET à l’aide de Visual Studio. Pour chacune d’elles, une ou plusieurs causes possibles et les solutions correspondantes sont fournies.

Les scénarios indiqués s’appliquent à la fois aux fournisseurs d’hébergement Azure et tiers. Pour en savoir plus sur la résolution des applications web dans le Service d’application Microsoft Azure, consultez les ressources suivantes :

- [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Surveiller les applications web dans Microsoft Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Annonce de la publication du kit de développement logiciel (SDK) Windows Azure 2,0 pour .net](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (blog de ScottGu, qui montre comment obtenir les journaux de diagnostic dans Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Erreur de serveur dans l’application « / »-les paramètres d’erreur personnalisés actuels empêchent l’affichage à distance des détails de l’erreur

### <a name="scenario"></a>Scénario

Après avoir déployé un site sur un hôte distant, vous recevez un message d’erreur qui mentionne le paramètre customErrors dans le fichier Web. config, mais n’indique pas la cause réelle de l’erreur :

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, ASP.NET affiche des informations d’erreur détaillées uniquement lorsque votre application Web s’exécute sur l’ordinateur local. En règle générale, vous ne souhaitez pas afficher les informations d’erreur détaillées lorsque votre application Web est publiquement disponible sur Internet, car les pirates peuvent utiliser ces informations pour identifier les vulnérabilités dans l’application. Toutefois, lorsque vous déployez un site ou des mises à jour sur un site, il arrive parfois qu’un problème se produise et que vous deviez obtenir le message d’erreur réel.

Pour permettre à l’application d’afficher des messages d’erreur détaillés lorsqu’elle s’exécute sur l’hôte distant, modifiez le fichier Web. config pour définir le mode customErrors désactivé, redéployez l’application et réexécutez l’application :

1. Si le fichier Web. config de l’application a un élément customErrors dans l’élément System. Web, remplacez l’attribut mode par « OFF ». Sinon, ajoutez un élément customErrors dans l’élément System. Web avec l’attribut mode défini sur OFF, comme indiqué dans l’exemple suivant : 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Déployez l’application.
3. Exécutez l’application et répétez les étapes que vous avez effectuées précédemment, ce qui a provoqué l’erreur. Vous pouvez maintenant voir le message d’erreur réel.
4. Une fois l’erreur résolue, restaurez le paramètre customErrors d’origine et redéployez l’application.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Impossible de créer le cliché instantané’ContosoUniversity’lorsque ce fichier existe déjà.

### <a name="scenario"></a>Scénario

Lorsque vous essayez d’exécuter un projet dans Visual Studio, vous recevez une page d’erreur avec un message semblable à l’exemple suivant :

Erreur de serveur dans l’application « / ». Impossible de créer le cliché instantané’ContosoUniversity’lorsque ce fichier existe déjà.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Attendez une minute et actualisez le navigateur, ou recompilez le site et réessayez de l’exécuter.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>L’accès est refusé dans une page Web qui utilise SQL Server Compact

### <a name="scenario"></a>Scénario

Lorsque vous déployez un site qui utilise SQL Server Compact et que vous exécutez une page du site déployé qui accède à la base de données, le message d’erreur suivant s’affiche :

Accès refusé. (Exception de HRESULT : 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le compte de SERVICE réseau sur le serveur doit être en mesure de lire les binaires natifs de service SQL qui se trouvent dans le dossier *bin\amd64* ou *bin\x86* , mais il ne dispose pas des autorisations de lecture pour ces dossiers. Définissez l’autorisation d’accès en lecture pour SERVICE réseau sur le dossier *bin* , en veillant à étendre les autorisations aux sous-dossiers.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Impossible de lire le fichier de configuration en raison d’autorisations insuffisantes

### <a name="scenario"></a>Scénario

Lorsque vous cliquez sur le bouton publier de Visual Studio pour déployer une application sur votre ordinateur local, la publication échoue et la fenêtre **sortie** affiche un message d’erreur similaire à celui-ci :

Une erreur s’est produite lors de la lecture du fichier de configuration IIS « MACHINE/redirection ». L’identité effectuant cette opération a été... Erreur : impossible de lire le fichier de configuration en raison d’autorisations insuffisantes.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Pour utiliser la publication en un clic sur IIS sur votre ordinateur local, vous devez exécuter Visual Studio avec des autorisations d’administrateur. Fermez Visual Studio et redémarrez-le avec les autorisations d’administrateur.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Impossible de se connecter à l’ordinateur de destination... Utilisation du processus spécifié

### <a name="scenario"></a>Scénario

Lorsque vous cliquez sur le bouton publier de Visual Studio pour déployer une application, la publication échoue et la fenêtre **sortie** affiche un message d’erreur similaire à celui-ci :

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Un serveur proxy interrompt la communication avec le serveur de destination. Dans le panneau de configuration Windows ou dans Internet Explorer, sélectionnez **Options Internet** et sélectionnez l’onglet **connexions** . Dans la boîte de dialogue **Propriétés Internet** , cliquez sur **paramètres réseau**. Dans la boîte de dialogue **paramètres du réseau local (LAN)** , désactivez la case à cocher **détecter automatiquement les paramètres** . Cliquez à nouveau sur le bouton publier.

Si le problème persiste, contactez votre administrateur système pour déterminer ce qui peut être fait avec les paramètres de proxy ou de pare-feu. Le problème se produit car Web Deploy utilise un port non standard pour le déploiement du service de gestion Web (8172); pour les autres connexions, Web Deploy utilise le port 80. Lorsque vous déployez sur un fournisseur d’hébergement tiers, vous utilisez généralement le service de gestion Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Le pool d’applications .NET 4,0 par défaut n’existe pas

### <a name="scenario"></a>Scénario

Lorsque vous déployez une application qui requiert le .NET Framework 4, le message d’erreur suivant s’affiche :

Le pool d’applications .NET 4,0 par défaut n’existe pas ou l’application n’a pas pu être ajoutée. Vérifiez que ASP.NET 4,0 est installé sur cet ordinateur.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

ASP.NET 4 n’est pas installé dans IIS. Si le serveur sur lequel vous effectuez le déploiement est votre ordinateur de développement et que Visual Studio 2010 est installé sur celui-ci, ASP.NET 4 est installé sur l’ordinateur, mais il n’est peut-être pas installé dans IIS. Sur le serveur sur lequel vous effectuez le déploiement, ouvrez une invite de commandes avec élévation de privilèges et installez ASP.NET 4 dans IIS en exécutant les commandes suivantes :

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Vous devrez peut-être également définir manuellement la version .NET Framework du pool d’applications par défaut. Pour plus d’informations, consultez le didacticiel sur le déploiement d’IIS en tant qu’environnement de test dans cette série.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Le format de la chaîne d’initialisation n’est pas conforme à la spécification commençant à l’index 0.

### <a name="scenario"></a>Scénario

Une fois que vous avez déployé une application à l’aide de la publication en un clic, lorsque vous exécutez une page qui accède à la base de données, vous recevez le message d’erreur suivant :

Le format de la chaîne d’initialisation n’est pas conforme à la spécification commençant à l’index 0.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Ouvrez le fichier *Web. config* dans le site déployé et vérifiez si les valeurs de chaîne de connexion commencent par `$(ReplaceableToken_`, comme dans l’exemple suivant :

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Si les chaînes de connexion ressemblent à cet exemple, modifiez le fichier projet et ajoutez la propriété suivante à l’élément PropertyGroup pour toutes les configurations de build :

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Ensuite, redéployez l’application.

## <a name="http-500-internal-server-error"></a>Erreur de serveur interne HTTP 500

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, le message d’erreur suivant s’affiche sans informations spécifiques indiquant la cause de l’erreur :

Erreur HTTP 500-erreur de serveur interne.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Il existe de nombreuses causes d’erreurs 500, mais l’une des causes possibles de ces didacticiels est que vous placez un élément XML au mauvais endroit dans l’un des fichiers de transformation Web. config. Par exemple, vous obtiendriez cette erreur si vous placez la transformation qui insère un &lt;emplacement&gt; élément sous &lt;&gt; System. Web au lieu de juste sous &lt;configuration&gt;. Vous pouvez utiliser la fonctionnalité aperçu de la transformation Web. config pour vérifier que les transformations fonctionnent comme prévu. La solution si vous trouvez une transformation qui a été codée de manière incorrecte consiste à corriger le fichier de transformation et à le redéployer. Si une erreur n’est pas évidente, essayez de mettre en commentaire les transformations et de redéployer pour déterminer celle qui est à l’origine de l’erreur 500.

## <a name="http-50021-internal-server-error"></a>Erreur de serveur interne HTTP 500,21

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, le message d’erreur suivant s’affiche :

Erreur HTTP 500,21-erreur de serveur interne. Le gestionnaire « que PageHandlerFactory-Integrated » a un module « ManagedPipelineHandler » incorrect dans sa liste de modules.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le site que vous avez déployé cible ASP.NET 4, mais ASP.NET 4 n’est pas enregistré dans IIS sur le serveur. Sur le serveur, ouvrez une invite de commandes avec élévation de privilèges et inscrivez ASP.NET 4 en exécutant les commandes suivantes :

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Vous devrez peut-être également définir manuellement la version .NET Framework du pool d’applications par défaut. Pour plus d’informations, consultez le didacticiel sur le déploiement d’IIS en tant qu’environnement de test dans cette série.

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Échec de la connexion lors de l’ouverture de la base de données SQL Server Express dans l’application\_données

### <a name="scenario"></a>Scénario

Vous avez mis à jour la chaîne de connexion du fichier *Web. config* pour pointer vers une base de données SQL Server Express en tant que fichier *. mdf* dans votre *application\_dossier Data* , et la première fois que vous exécutez l’application, le message d’erreur suivant s’affiche :

System. Data. SqlClient. SqlException : impossible d’ouvrir la base de données "DatabaseName" demandée par la connexion. La connexion a échoué.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le nom du fichier *. mdf* ne peut pas correspondre au nom d’une base de données SQL Server Express qui existait déjà sur votre ordinateur, même si vous avez supprimé le fichier *. mdf* de la base de données existante. Remplacez le nom du fichier *. mdf* par un nom qui n’a jamais été utilisé comme nom de base de données et modifiez le fichier *Web. config* pour qu’il utilise le nouveau nom. Vous pouvez également utiliser [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) pour supprimer les bases de données SQL Server Express précédemment existantes.

## <a name="model-compatibility-cannot-be-checked"></a>Impossible de vérifier la compatibilité du modèle

### <a name="scenario"></a>Scénario

Vous avez mis à jour la chaîne de connexion du fichier *Web. config* pour pointer vers une nouvelle base de données de SQL Server Express, et la première fois que vous exécutez l’application, vous voyez le message d’erreur suivant :

Impossible de vérifier la compatibilité du modèle, car la base de données ne contient pas de métadonnées de modèle. Assurez-vous que IncludeMetadataConvention a été ajouté aux conventions DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Si le nom de la base de données que vous avez placé dans le fichier Web. config a jamais été utilisé auparavant sur votre ordinateur, il se peut qu’une base de données existe déjà avec des tables. Sélectionnez un nouveau nom qui n’a pas encore été utilisé sur votre ordinateur, puis modifiez le fichier *Web. config* pour qu’il pointe vers utiliser ce nouveau nom de base de données. Vous pouvez également utiliser [SQL Server Express utilitaire](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) ou [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) pour supprimer la base de données existante.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Erreur SQL lorsqu’un script tente de créer des utilisateurs ou des rôles

### <a name="scenario"></a>Scénario

Vous utilisez le déploiement de base de données configuré sous l’onglet **Package/Publication SQL** , les scripts SQL qui s’exécutent pendant le déploiement incluent les commandes créer un utilisateur ou créer un rôle, et l’exécution du script échoue lorsque ces commandes sont exécutées. Vous pouvez voir des messages plus détaillés, tels que les suivants :

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Si cette erreur se produit lorsque vous avez configuré le déploiement de base de données dans l’Assistant **publier le site Web** au lieu de l’onglet **Package/Publication SQL** , créez un thread dans le Forum de [configuration et de déploiement](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) , et la solution sera ajoutée à cette page de résolution des problèmes.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le compte d’utilisateur que vous utilisez pour effectuer le déploiement n’a pas l’autorisation de créer des utilisateurs ou des rôles. Par exemple, la société d’hébergement peut attribuer les rôles DB\_DataReader, DB\_DataWriter et DB\_ddladmin au compte d’utilisateur qu’elle configure pour vous. Celles-ci sont suffisantes pour créer la plupart des objets de base de données, mais pas pour créer des utilisateurs ou des rôles. Pour éviter cette erreur, il est possible d’exclure des utilisateurs et des rôles du déploiement de base de données. Pour ce faire, vous pouvez modifier l’élément de présource pour le script généré automatiquement de la base de données afin qu’il comprenne les attributs suivants :

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Pour plus d’informations sur la modification de l’élément de présource dans le fichier projet, consultez [Comment : modifier les paramètres de déploiement dans le fichier projet](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si les utilisateurs ou les rôles de votre base de données de développement doivent se trouver dans la base de données de destination, contactez votre fournisseur d’hébergement pour obtenir de l’aide.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server erreur de délai d’attente lors de l’exécution de scripts personnalisés pendant le déploiement

### <a name="scenario"></a>Scénario

Vous avez spécifié des scripts SQL personnalisés à exécuter pendant le déploiement, et lorsque Web Deploy les exécutent, ils expirent.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

L’exécution de plusieurs scripts qui ont des modes de transaction différents peut entraîner des erreurs de délai d’attente. Par défaut, les scripts générés automatiquement s’exécutent dans une transaction, contrairement aux scripts personnalisés. Si vous sélectionnez l’option **extraire les données et/ou le schéma à partir d’une base de données existante** sous l’onglet **Package/Publication SQL** , et si vous ajoutez un script SQL personnalisé, vous devez modifier les paramètres des transactions sur certains scripts afin que tous les scripts utilisent les mêmes paramètres de transaction. Pour plus d’informations, consultez [Comment : déployer une base de données avec un projet d’application Web](https://msdn.microsoft.com/library/dd465343.aspx).

Si vous avez configuré des paramètres de transaction afin que tous soient identiques mais que vous obteniez toujours cette erreur, une solution de contournement possible consiste à exécuter les scripts séparément. Dans la grille **scripts de base de données** de l’onglet **Package/Publication** SQL, désactivez la case à cocher **inclure** du script qui provoque l’erreur de délai d’attente, puis publiez le projet. Revenez ensuite à la grille **scripts de la base de données** , sélectionnez la case à cocher **inclure** du script, puis désactivez les cases à cocher **inclure** pour les autres scripts. Publiez à nouveau le projet. Cette fois, lorsque vous publiez, seul le script personnalisé sélectionné s’exécute.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Les données de flux du manifeste de site ne sont pas encore disponibles

### <a name="scenario"></a>Scénario

Lorsque vous installez un package à l’aide du fichier *Deploy. cmd* avec l’option t (test), le message d’erreur suivant s’affiche :

Erreur : les données de flux de’sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript’ne sont pas encore disponibles.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le message d’erreur signifie que la commande ne peut pas générer de rapport de test. Toutefois, la commande peut s’exécuter si vous utilisez l’option y (installation réelle). Le message indique uniquement qu’il y a un problème avec l’exécution de la commande en mode test.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Cette application requiert ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Scénario

Lorsque vous essayez de déployer, le message d’erreur suivant s’affiche :

La propriété « managedRuntimeVersion » du pool d’applications que vous essayez d’utiliser a la valeur « v 2.0 ». Cette application nécessite « v 4.0 ».

### <a name="possible-cause-and-solution"></a>Cause possible et solution

ASP.NET 4 n’est pas installé dans IIS. Si le serveur sur lequel vous effectuez le déploiement est votre ordinateur de développement et que Visual Studio 2010 est installé sur celui-ci, ASP.NET 4 est installé sur l’ordinateur, mais il n’est peut-être pas installé dans IIS. Sur le serveur sur lequel vous effectuez le déploiement, ouvrez une invite de commandes avec élévation de privilèges et installez ASP.NET 4 dans IIS en exécutant les commandes suivantes :

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Impossible d’effectuer un cast de Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Scénario

Lorsque vous déployez un package, le message d’erreur suivant s’affiche :

Impossible d’effectuer un cast d’un objet de type « Microsoft. Web. Deployment. DeploymentProviderOptions » en « Microsoft. Web. Deployment. DeploymentProviderOptions ».

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Vous essayez de déployer à partir du gestionnaire des services Internet à l’aide de l’interface utilisateur Web Deploy 1,1 sur un serveur sur lequel Web Deploy 2,0 est installé. Si vous utilisez l’outil d’administration à distance IIS pour effectuer un déploiement en important un package, activez la boîte de dialogue **nouvelles fonctionnalités disponibles** lorsque vous établissez la connexion. (Cette boîte de dialogue ne peut s’afficher qu’une seule fois lorsque la connexion est établie pour la première fois. Pour effacer la connexion et recommencer, fermez le gestionnaire des services Internet, puis redémarrez-le en entrant inetmgr/Reset à l’invite de commandes.) Si l’une des fonctionnalités répertoriées est **Web Deploy interface utilisateur**et que son numéro de version est inférieur à 8, le serveur sur lequel vous effectuez le déploiement peut avoir 1,1 et 2,0 versions de Web Deploy installées. Pour déployer à partir d’un client sur lequel est installé 2,0, le serveur ne doit avoir que Web Deploy 2,0. Vous devrez contacter votre fournisseur d’hébergement pour résoudre ce problème.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Impossible de charger les composants natifs de SQL Server Compact

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, le message d’erreur suivant s’affiche :

Impossible de charger les composants natifs de SQL Server Compact correspondant au fournisseur ADO.NET de la version 8482. Installez la version appropriée de SQL Server Compact. Pour plus d’informations, consultez l’article 974247 de la base de connaissances.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le site déployé n’a pas de sous-dossiers *amd64* et *x86* avec les assemblys natifs qu’ils contiennent dans le dossier *bin* de l’application. Sur un ordinateur sur lequel SQL Server Compact installé, les assemblys natifs se trouvent dans *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. La meilleure façon d’obtenir les fichiers corrects dans les dossiers appropriés dans un projet Visual Studio consiste à installer le package NuGet SqlServerCompact. L’installation du package ajoute un script de publication pour copier les assemblys natifs dans *amd64* et *x86*. Toutefois, pour que ces derniers soient déployés, vous devez les inclure manuellement dans le projet. Pour plus d’informations, consultez le didacticiel sur le [déploiement de SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Erreur « le chemin d’accès n’est pas valide » après le déploiement d’une application Entity Framework Code First

### <a name="scenario"></a>Scénario

Vous déployez une application qui utilise Migrations Entity Framework Code First et un SGBD comme SQL Server Compact qui stocke sa base de données dans un fichier dans le dossier des données de\_de l’application. Vous avez Migrations Code First configuré pour créer la base de données après votre premier déploiement. Lorsque vous exécutez l’application, vous recevez un message d’erreur semblable à l’exemple suivant :

Le chemin d'accès n'est pas valide. Vérifiez le répertoire de la base de données. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Code First tente de créer la base de données, mais le dossier application\_Data n’existe pas. Vous n’avez pas de fichiers dans le dossier de données de l' *application\_* lorsque vous avez déployé, ou vous avez sélectionné l’option **exclure les données de\_** de l’application dans l’onglet **Package/Publication Web** du fenêtre Propriétés de projet. Le processus de déploiement ne crée pas de dossier sur le serveur s’il n’y a aucun fichier dans le dossier à copier sur le serveur. Si vous avez déjà configuré la base de données sur le site, le processus de déploiement supprimera les fichiers et le dossier de données de l' *application\_* même si vous avez sélectionné **Supprimer les fichiers supplémentaires à la destination** dans le profil de publication. Pour résoudre le problème, placez un fichier d’espace réservé, tel qu’un fichier. txt dans le dossier de données de l' *application\_* , assurez-vous que vous n’avez pas sélectionné l’option **exclure l’application\_les données** et redéployez.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>« Un objet COM qui a été séparé de son RCW sous-jacent ne peut pas être utilisé ».

### <a name="scenario"></a>Scénario

Vous avez correctement utilisé la publication en un clic pour déployer votre application et vous commencez à obtenir cette erreur :

Échec de la tâche de déploiement Web. (Impossible de terminer la demande d’URL de l’agent distant'<https://serverurl.com/msdeploy.axd?site=sitename>'.)  
 Impossible de terminer la demande d’URL de l’agent distant'<https://url/msdeploy.axd?site=sitename>'.  
La demande a été abandonnée : la demande a été annulée.  
L’objet COM qui a été séparé de son wrapper RCW sous-jacent ne peut pas être utilisé.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

La fermeture et le redémarrage de Visual Studio sont généralement nécessaires pour résoudre cette erreur.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Le déploiement échoue parce que les informations d’identification de l’utilisateur utilisées pour la publication ne disposent pas de l’autorité setACL

### <a name="scenario"></a>Scénario

La publication échoue avec une erreur qui indique que vous n’avez pas l’autorité nécessaire pour définir les autorisations du dossier (le compte d’utilisateur que vous utilisez ne dispose pas de l’autorité setACL).

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, Visual Studio définit les autorisations de lecture sur le dossier racine du site et les autorisations d’écriture sur le dossier de données de l’application\_. Si vous savez que les autorisations par défaut sur les dossiers du site sont correctes et qu’elles n’ont pas besoin d’être définies, vous désactivez ce comportement en ajoutant **&lt;destination IncludeSetACLProviderOn&gt;false&lt;la&gt;/IncludeSetACLProviderOnDestination** au fichier de profil de publication (pour affecter un seul profil) ou au fichier WPP. targets (pour affecter tous les profils). Pour plus d’informations sur la modification de ces fichiers, consultez [Comment : modifier les paramètres de déploiement dans les fichiers de profil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Erreurs de refus d’accès lorsque l’application tente d’écrire dans un dossier d’application

### <a name="scenario"></a>Scénario

Vos erreurs d’application lors de la tentative de création ou de modification d’un fichier dans l’un des dossiers d’application, car il ne dispose pas d’une autorité d’écriture pour ce dossier.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, Visual Studio définit les autorisations de lecture sur le dossier racine du site et les autorisations d’écriture sur le dossier de données de l’application\_. Si votre application a besoin d’un accès en écriture à un sous-dossier, vous pouvez définir des autorisations pour ce dossier, comme indiqué dans les didacticiels définir des autorisations sur les dossiers et déploiement sur les environnements de production dans ce kit. Si votre application a besoin d’un accès en écriture au dossier racine du site, vous devez l’empêcher de définir un accès en lecture seule sur le dossier racine en ajoutant **&lt;destination IncludeSetACLProviderOn&gt;false&lt;la&gt;/IncludeSetACLProviderOnDestination** au fichier de profil de publication (pour affecter un seul profil) ou au fichier WPP. targets (pour affecter tous les profils). Pour plus d’informations sur la modification de ces fichiers, consultez [Comment : modifier les paramètres de déploiement dans les fichiers de profil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Erreur de configuration-l’attribut targetFramework fait référence à une version qui est ultérieure à la version installée du .NET Framework

### <a name="scenario"></a>Scénario

Vous avez correctement publié un projet Web qui cible ASP.NET 4,5, mais quand vous exécutez l’application (avec le mode customErrors défini sur « désactivé » dans le fichier Web. config), vous recevez l’erreur suivante :

L’attribut’targetFramework’dans l’élément de&gt; de compilation &lt;du fichier Web. config est utilisé uniquement pour cibler la version 4,0 et les versions ultérieures du .NET Framework (par exemple, '&lt;compilation targetFramework = "4.0"&gt;'). L’attribut’targetFramework’référence actuellement une version qui est ultérieure à la version installée du .NET Framework. Spécifiez une version cible valide du .NET Framework ou installez la version requise du .NET Framework.

La boîte de message d’erreur source de la page d’erreur met en surbrillance la ligne suivante du fichier Web. config en raison de l’erreur :

&lt;la compilation targetFramework = "4.5"/&gt;

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le serveur ne prend pas en charge ASP.NET 4,5. Contactez le fournisseur d’hébergement pour déterminer quand et comment la prise en charge de ASP.NET 4,5 peut être ajoutée. Si la mise à niveau du serveur n’est pas une option, vous devez déployer un projet Web qui cible ASP.NET 4 ou version antérieure à la place.

Si vous déployez un projet Web ASP.NET 4 ou version antérieure vers la même destination, activez la case à cocher **Supprimer les fichiers supplémentaires à la destination** sous l’onglet **paramètres** de l’Assistant **publier le site Web** . Si vous ne sélectionnez pas **Supprimer les fichiers supplémentaires à la destination**, vous pouvez continuer à obtenir la page d’erreur de configuration.

La fenêtre **Propriétés** du projet comprend une liste déroulante de la version cible du .NET Framework, mais vous ne pouvez pas résoudre ce problème en changeant simplement ce problème de **.NET Framework 4,5** à **.NET Framework 4**. Si vous remplacez le Framework cible par une version de Framework antérieure, le projet aura toujours des références aux assemblys de la version du Framework ultérieure et ne s’exécutera pas. Vous devez modifier manuellement ces références ou créer un projet qui cible .NET Framework 4 ou une version antérieure. Pour plus d’informations, consultez [.NET Framework ciblage pour les sites Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Erreurs de confiance moyenne

### <a name="scenario"></a>Scénario

Lorsque vous exécutez votre application en production, elle obtient une erreur liée à la confiance moyenne.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

De nombreux fournisseurs d’hébergement tiers exécutent votre site Web en mode de confiance moyenne, ce qui signifie qu’il n’est pas autorisé de faire certaines choses. Par exemple, le code d’application ne peut pas accéder au registre Windows et ne peut pas lire ou écrire des fichiers qui se trouvent en dehors de la hiérarchie des dossiers de votre application. Par défaut, votre application s’exécute en *mode confiance totale* sur votre ordinateur local, ce qui signifie que l’application peut être en mesure d’effectuer des opérations qui échouent lorsque vous la déployez en production.

Vous pouvez configurer l’application pour qu’elle s’exécute avec un niveau de confiance moyen dans l’environnement IIS local afin de résoudre les problèmes. Pour ce faire, ouvrez le fichier *Web. config* de l’application et ajoutez un élément **Trust** dans l’élément **System. Web** , comme illustré dans cet exemple.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

L’application s’exécute maintenant dans un niveau de confiance moyen dans IIS, même sur votre ordinateur local.

Ne le faites pas si vous effectuez un déploiement sur Azure App Service, car Azure ne requiert pas de confiance moyenne. Au moment de l’écriture de ce didacticiel en février 2012, l’utilisation de cette méthode pour que votre application s’exécute en mode de confiance moyenne génère une erreur dans Azure.

Si vous utilisez Migrations Entity Framework Code First et que vous déployez sur un fournisseur d’hébergement qui exécute votre application dans un niveau de confiance moyen, assurez-vous que la version 5,0 ou une version ultérieure est installée. Dans Entity Framework version 4,3, les migrations requièrent une confiance totale pour mettre à jour le schéma de la base de données.

## <a name="http-40417-not-found-error"></a>Erreur HTTP 404,17 introuvable

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé sur votre ordinateur de développement dans IIS, le message d’erreur suivant s’affiche indiquant que le serveur ne peut pas traiter default. aspx :

Erreur HTTP 404,17-introuvable

Le contenu demandé semble être un script et ne sera pas pris en charge par le gestionnaire de fichiers statiques.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

ASP.NET 4,5 peut ne pas être installé sur votre ordinateur. Consultez les étapes du didacticiel déploiement sur IIS en tant qu’environnement de test de cette série qui expliquent comment installer ASP.NET 4,5.

> [!div class="step-by-step"]
> [Précédent](deploying-extra-files.md)
