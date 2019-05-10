---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement vers IIS comme environnement de Test - 5 de 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 624d99ccbb0da1281b8c9cd8503507f22742e7a7
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132312"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement vers IIS comme environnement de Test - 5 de 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel montre comment déployer une application web ASP.NET sur IIS sur l’ordinateur local.

Lorsque vous développez une application, vous tester généralement en l’exécutant dans Visual Studio. Par défaut, cela signifie que vous utilisez le serveur de développement Visual Studio (également appelé « Cassini »). Le serveur de développement Visual Studio, il est facile à tester au cours du développement dans Visual Studio, mais il ne fonctionne pas exactement comme IIS. Par conséquent, il est possible qu’une application s’exécute correctement lorsque vous testez dans Visual Studio, mais échouer lorsqu’elle est déployée sur IIS dans un environnement d’hébergement.

Vous pouvez tester votre application plus fiable des façons suivantes :

1. Utiliser IIS Express ou IIS complet au lieu du serveur de développement Visual Studio lorsque vous testez dans Visual Studio pendant le développement. Cette méthode généralement émule plus précisément comment votre site s’exécutera sous IIS. Toutefois, cette méthode ne pas tester votre processus de déploiement ou de valider que le résultat du processus de déploiement s’exécute correctement.
2. Déployer l’application à IIS sur votre ordinateur de développement à l’aide du même processus que vous utiliserez ultérieurement pour le déployer dans votre environnement de production. Cette méthode valide votre processus de déploiement en plus de vérifier que votre application s’exécutera correctement sous IIS.
3. Déployer l’application sur un environnement de test qui est aussi proche que possible dans votre environnement de production. Étant donné que l’environnement de production pour ces didacticiels est un fournisseur d’hébergement tiers, l’environnement de test idéal serait un deuxième compte avec le fournisseur d’hébergement. Vous utiliseriez cet deuxième compte uniquement pour les tests, mais il serait configurée la même façon que le compte de production.

Ce didacticiel présente les étapes de l’option 2. Conseils pour l’option 3 sont fournie à la fin de la [déploiement dans l’environnement de Production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) (didacticiel), et à la fin de ce didacticiel, il existe des liens vers des ressources pour l’option 1.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configuration de l’Application à exécuter en mode de confiance moyenne

Avant d’installer les services IIS et le déploiement à celui-ci, vous allez modifier un paramètre du fichier Web.config afin que le site s’exécute plus comme elle le sera dans un environnement d’hébergement partagé typique.

Fournisseurs d’hébergement exécutent généralement votre site web *confiance moyenne*, ce qui signifie que certains éléments, il n’est pas autorisé à effectuer. Par exemple, code d’application ne peut pas accéder au Registre Windows et ne peut pas lire ou écrire des fichiers qui sont en dehors de l’arborescence des dossiers de votre application. Par défaut, votre application s’exécute *de confiance élevé* sur votre ordinateur local, ce qui signifie que l’application peut être en mesure d’effectuer des opérations qui échouent lors de son déploiement en production. Par conséquent, pour rendre l’environnement de test que plus précisément l’environnement de production, vous configurerez l’application s’exécute en mode de confiance moyenne.

Dans le fichier Web.config de l’application, ajoutez un **approbation** élément dans le **system.web** élément, comme illustré dans cet exemple.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

L’application s’exécutera désormais en mode de confiance moyenne dans IIS même sur votre ordinateur local. Ce paramètre vous permet d’intercepter dès que possible toutes les tentatives par code d’application de faire quelque chose qui échouait en production.

> [!NOTE]
> Si vous utilisez des Migrations Entity Framework Code First, assurez-vous que que vous disposez de la version 5.0 ou version ultérieure. Dans Entity Framework version 4.3, Migrations requiert une confiance totale pour mettre à jour le schéma de base de données.

## <a name="installing-iis-and-web-deploy"></a>Déployer l’installation IIS et Web

Pour déployer sur IIS sur votre ordinateur de développement, vous devez disposer d’IIS et Web Deploy installé. Ils ne sont pas inclus dans la configuration de Windows 7 par défaut. Si vous avez déjà installé IIS et Web Deploy, passez à la section suivante.

À l’aide de la [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) est la meilleure méthode pour installer IIS et Web Deploy, étant donné que le programme Web Platform Installer installe une configuration recommandée pour IIS et il installe automatiquement la configuration requise pour IIS et Web Déployez si nécessaire.

Pour exécuter Web Platform Installer pour installer IIS et Web Deploy, utilisez le lien suivant. Si vous avez déjà installé IIS, Web Deploy ou l’un de leurs composants obligatoires, le programme Web Platform Installer installe uniquement ce qui est manquant.

- [Installer IIS et Web Deploy à l’aide de WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Configuration du Pool d’applications par défaut .NET 4

Après avoir installé IIS, exécutez **Gestionnaire des services Internet** pour vous assurer que le .NET Framework version 4 est affecté au pool d’applications par défaut.

À partir de la Windows **Démarrer** menu, sélectionnez **exécuter**, entrez « inetmgr », puis cliquez sur **OK**. (Si le **exécuter** commande ne figure pas dans votre **Démarrer** menu, vous pouvez appuyer sur la clé de Windows et R pour l’ouvrir. Ou avec le bouton droit de la barre des tâches, cliquez sur **propriétés**, sélectionnez le **Menu Démarrer** , cliquez sur **personnaliser**, puis sélectionnez **exécuter la commande**.)

Dans le **connexions** volet, développez le nœud du serveur, puis sélectionnez **Pools d’applications**. Dans le **Pools d’applications** volet, si **DefaultAppPool** est attribué pour le .NET framework version 4, comme dans l’illustration suivante, passez à la section suivante.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Si vous voyez uniquement deux pools d’applications et de ces deux valeurs sont définies sur le .NET Framework 2.0, vous devez installer ASP.NET 4 dans IIS :

- Ouvrez une fenêtre d’invite de commandes en cliquant en **invite de commandes** dans le Windows **Démarrer** menu et en sélectionnant **exécuter en tant qu’administrateur**. Puis exécutez [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) pour installer ASP.NET 4 dans IIS, en utilisant les commandes suivantes. (Dans les systèmes 64 bits, remplacez « Framework » avec « Framework64 »).

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Cette commande crée les nouveaux pools d’applications pour le .NET Framework 4, mais le pool d’applications par défaut sera toujours égale à 2.0. Vous allez déployer une application qui cible .NET 4 pour ce pool d’applications, donc vous devez modifier le pool d’applications pour .NET 4.

Si vous avez fermé **Gestionnaire des services Internet**, exécutez-le à nouveau, développez le nœud du serveur, puis cliquez sur **Pools d’applications** pour afficher le **Pools d’applications** volet à nouveau.

Dans le **Pools d’applications** volet, cliquez sur **DefaultAppPool**, puis, dans le **Actions** volet, cliquez sur **paramètres de base**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Dans le **modifier le Pool d’Application** boîte de dialogue, choisissez **version du .NET Framework** à **.NET Framework v4.0.30319** et cliquez sur **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Vous êtes maintenant prêt à publier sur IIS.

## <a name="publishing-to-iis"></a>Publication sur IIS

Il existe plusieurs façons, vous pouvez déployer à l’aide de Visual Studio 2010 et Web Deploy :

- Utilisez Visual Studio publication en un clic.
- Créer un *package de déploiement* et installez-le à l’aide du Gestionnaire des services Internet UI. Le package de déploiement se compose d’un *.zip* fichier qui contient tous les fichiers et les métadonnées nécessaires pour installer un site dans IIS.
- Créer un package de déploiement et l’installer à l’aide de la ligne de commande.

Le processus que vous avez parcouru dans les didacticiels précédents pour configurer Visual Studio pour automatiser les tâches de déploiement s’applique à toutes ces trois méthodes. Dans ces didacticiels, vous allez utiliser la première de ces méthodes. Pour plus d’informations sur l’utilisation de packages de déploiement, consultez [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

Avant la publication, assurez-vous que vous exécutez Visual Studio en mode administrateur. (Dans le Kit Windows 7 **Démarrer** menu, cliquez sur l’icône pour la version de Visual Studio que vous utilisez et sélectionnez **exécuter en tant qu’administrateur**.) Le mode administrateur est requis pour la publication uniquement lorsque vous publiez sur IIS sur l’ordinateur local.

Dans **l’Explorateur de solutions**, cliquez sur le projet ContosoUniversity (pas le projet ContosoUniversity.DAL) et sélectionnez **publier**.

Le **publier le site Web** Assistant s’affiche.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Dans la liste déroulante, sélectionnez  **&lt;nouveau... &gt;**.

Dans le **nouveau profil** boîte de dialogue, entrez « Test », puis cliquez sur **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Ce nom est que le même que le nœud central de la Web.Test.config de transformer le fichier que vous avez créé précédemment. Cette correspondance est ce qui provoque les transformations Web.Test.config à appliquer lors de la publication à l’aide de ce profil.

L’Assistant passe automatiquement à la **connexion** onglet.

Dans le **l’URL du Service** , entrez *localhost*.

Dans le **Site/application** , entrez *Default Web Site/ContosoUniversity*.

Dans le **URL de Destination** , entrez `http://localhost/ContosoUniversity`.

Le **URL de Destination** paramètre n’est pas nécessaire. Visual Studio a terminé de déployer l’application, il s’ouvre automatiquement votre navigateur par défaut à cette URL. Si vous ne souhaitez pas le navigateur s’ouvre automatiquement après le déploiement, laissez cette zone vide.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Cliquez sur **valider la connexion** pour vérifier que les paramètres sont corrects et que vous pouvez vous connecter à IIS sur l’ordinateur local.

Une coche verte vérifie que la connexion est établie.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Cliquez sur **suivant** pour accéder à la **paramètres** onglet.

Le **Configuration** zone de liste déroulante spécifie la configuration de build à déployer. La valeur par défaut est mise en production, ce qui est ce que vous voulez.

Laissez le **supprimer les fichiers supplémentaires à la destination** case à cocher désactivée. Dans la mesure où il s’agit de votre premier déploiement, il n’est tous les fichiers dans le dossier de destination encore.

Dans le **bases de données** section, entrez la valeur suivante dans la zone chaîne de connexion pour **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Le processus de déploiement placera cette chaîne de connexion dans le fichier Web.config déployé, car **utiliser cette chaîne de connexion lors de l’exécution** est sélectionné.

Également sous **SchoolContext**, sélectionnez **appliquer des Migrations Code First**. Cette option provoque le processus de déploiement configurer le fichier Web.config déployé pour spécifier le `MigrateDatabaseToLatestVersion` initialiseur. Cet initialiseur met automatiquement à jour la base de données vers la dernière version lors de l’application accède à la base de données pour la première fois après le déploiement.

Dans la zone chaîne de connexion pour **DefaultConnection**, entrez la valeur suivante :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Laissez **base de données de mise à jour** désactivée. La base de données d’appartenance est déployé en copiant le fichier .sdf dans application\_données et vous ne souhaitez pas que le processus de déploiement pour continuer avec cette base de données.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Cliquez sur **suivant** pour accéder à la **aperçu** onglet.

Dans le **aperçu** , cliquez sur **démarrer l’aperçu** pour afficher la liste des fichiers qui seront copiés.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Cliquez sur **Publier**.

Si Visual Studio n’est pas en mode administrateur, vous pouvez obtenir un message d’erreur qui indique une erreur d’autorisations. Dans ce cas, fermez Visual Studio, ouvrez-le en mode administrateur et réessayez la publication.

Si Visual Studio est en mode administrateur, le **sortie** rapports réussie de la fenêtre générer et publier.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Le navigateur s’ouvre automatiquement à la page d’accueil de l’Université de Contoso s’exécutant dans IIS sur l’ordinateur local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test dans l’environnement de Test

Notez que l’indicateur de l’environnement affiche « (Test) » au lieu de « (Dev) », qui montre que le *Web.config* transformation pour l’indicateur de l’environnement a réussi.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Exécutez le **étudiants** page pour vérifier ne qu’aucun étudiant à la base de données déployée. Lorsque vous sélectionnez cette page peut prendre quelques minutes à charger, car le Code First crée la base de données, puis exécute le `Seed` (méthode). (Il n’a pas le faire quand vous étiez sur la page d’accueil, car l’application n’a pas tenter d’accéder à la base de données encore.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Exécutez le **formateurs** page pour vérifier que le premier Code amorcée avec des données de l’instructeur :

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Sélectionnez **étudiants ajouter** à partir de la **étudiants** menu, ajouter un étudiant et afficher le nouvel étudiant dans la **étudiants** page pour vérifier que vous pouvez écrire avec succès à la base de données :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

À partir de la **cours** menu, sélectionnez **crédits de la mise à jour**. Le **crédits de la mise à jour** page requiert des autorisations d’administrateur, par conséquent, le **Log In** page s’affiche. Entrez les informations d’identification du compte administrateur que vous avez créé plus haut (« admin » et « Pas$ w0rd »). Le **mise à jour crédits** page s’affiche, qui vérifie que le compte d’administrateur que vous avez créé dans le didacticiel précédent a été correctement déployé sur l’environnement de test.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Vérifiez qu’un *Elmah* dossier existe avec uniquement le fichier d’espace réservé qu’il contient.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Révision des modifications de Web.config automatique pour les Migrations Code First

Ouvrez le *Web.config* fichier dans l’application déployée sur *C:\inetpub\wwwroot\ContosoUniversity* et vous pouvez voir où le processus de déploiement configuré Migrations Code First pour automatiquement mettre à jour la base de données vers la dernière version.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Le processus de déploiement créé également une nouvelle chaîne de connexion pour les Migrations Code First à utiliser exclusivement pour la mise à jour le schéma de base de données :

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Cette chaîne de connexion supplémentaires vous permet de spécifier un compte d’utilisateur pour les mises à jour du schéma de base de données et un autre compte d’utilisateur pour l’accès de données d’application. Par exemple, vous pouvez attribuer la base de données\_rôle de propriétaire pour les Migrations Code First et db\_datareader et db\_datawriter des rôles à l’application. Il s’agit d’un modèle courant dans défense qui empêche le code potentiellement malveillant dans l’application à partir de la modification du schéma de base de données. (Par exemple, cela peut se produire dans une attaque d’injection SQL.) Ce modèle n’est pas utilisé par ces didacticiels. Il ne s’applique pas à SQL Server Compact, et il ne s’applique pas lorsque vous migrez vers SQL Server dans un didacticiel plus loin dans cette série. Le site de Cytanium propose qu’un seul compte d’utilisateur pour accéder à la base de données SQL Server que vous créez au Cytanium. Si vous êtes en mesure d’implémenter ce modèle dans votre scénario, vous pouvez le faire en effectuant les étapes suivantes :

1. Dans le **paramètres** onglet de la **publier le site Web** Assistant, entrez la chaîne de connexion qui spécifie un utilisateur avec les autorisations de mise à jour de schéma de base de données complète et désactivez le **utiliser cette chaîne de connexion lors de l’exécution** case à cocher. Dans le fichier Web.config déployé, cela devient le `DatabasePublish` chaîne de connexion.
2. Créer une transformation du fichier Web.config pour la chaîne de connexion que vous souhaitez que l’application à utiliser au moment de l’exécution.

Vous avez maintenant déployé votre application à IIS sur votre ordinateur de développement et testée il. Cela vérifie que le processus de déploiement copie le contenu de l’application vers l’emplacement approprié (à l’exclusion de fichiers que vous ne voulez pas déployer) et également que Web Deploy IIS correctement configuré pendant le déploiement. Dans le didacticiel suivant, vous allez exécuter un test supplémentaire qui recherche une tâche de déploiement qui n’a pas encore été effectuée : définition des autorisations de dossier sur le *Elmah* dossier.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur l’exécution d’IIS ou IIS Express dans Visual Studio, consultez les ressources suivantes :

- [Présentation d’IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sur le site IIS.net.
- [Présentation d’IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sur le blog Guthrie.
- [Guide pratique pour Spécifiez le serveur Web pour les projets Web dans Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principales différences entre IIS et le serveur de développement ASP.NET](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sur le site ASP.NET.
- [Tester votre ASP.NET MVC ou les applications Web sur IIS 7 en 30 secondes](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) sur le blog de Rick Anderson. Cette entrée fournit des exemples de pourquoi le test avec le serveur Visual Studio Development (Cassini) n’est pas aussi fiable que le test dans IIS Express, et pourquoi le test dans IIS Express n’est pas aussi fiable que le test dans IIS.

Pour plus d’informations sur les problèmes pouvant survenir lorsque votre application s’exécute en mode de confiance moyenne, consultez [hébergeant les Applications ASP.NET en mode de confiance moyenne](http://www.4guysfromrolla.com/articles/100307-1.aspx) sur les 4 Guys Rolla site.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
