---
uid: web-pages/readme/overview
title: WebMatrix Readme | Microsoft Docs
author: rick-anderson
description: WebMatrix et ASP.NET Web Pages (Razor) version 1.0 Lisez-moi
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050716"
---
<a name="webmatrix-readme"></a>Fichier Lisez-moi de WebMatrix
====================
13 janvier 2011

## <a name="contents"></a>Sommaire

> [!NOTE]
> Ce fichier Lisez-moi s’applique à la 1.0 version de WebMatrix.


- [Vue d’ensemble](#Overview)
- [Installation](#Installation_Notes)
- [Comment publier des Applications](#InstructionsForPublishingApplications)
- [Modifications et les problèmes](#ChangesAndIssues)

    - [WebMatrix 1.0 Installation](#Known_Issues_Installation)
    - [Pages Web ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installation d’Applications](#Known_Issues_Installing_Applications)
    - [Publication d’Applications](#Known_Issues_Publishing_Applications)
- [Pour plus d'informations](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Vue d'ensemble

> Microsoft WebMatrix 1.0 est une pile de développement web gratuit qui s’installe en quelques minutes. Il s’intègre un serveur web de base de données et infrastructures pour créer une expérience unique et intégrée de programmation. Vous pouvez utiliser WebMatrix pour simplifier la manière dont votre code, tester, publier votre propre site Web ASP.NET ou PHP ou vous pouvez utiliser WebMatrix pour démarrer un nouveau site Web à l’aide d’applications open source populaires telles que DotNetNuke, Umbraco, WordPress ou Joomla. WebMatrix utilise le même serveur web puissant, moteur de base de données et infrastructures que ceux qui exécutera votre site Web sur internet, ce qui rend la transition du développement en production fluide et homogène.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Pour installer WebMatrix 1.0, vous devez d’abord installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Après avoir installé Web Platform Installer, vous pouvez l’utiliser pour installer WebMatrix.
> 
> Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Comment publier des Applications

> Consultez [obtenir des Instructions détaillées pour la publication d’Applications](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Modifications et les problèmes

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problèmes d’Installation 1.0 de WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problème : WebMatrix 1.0 est disponible uniquement sur les plateformes qui prennent en charge de Microsoft .NET Framework 4

> Le .NET Framework version 4 est requis pour WebMatrix. Dans certains cas, le programme d’installation de WebMatrix 1.0 permet d’essayer d’installer sur une plateforme qui ne fait pas partie de l’ensemble de la configuration prise en charge. En particulier, Vista Windows sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix, mais le composant .NET Framework 4 échoue et bloquer votre installation.
> 
> **Solution de contournement**  
> Installer sur une plateforme prise en charge, ce qui inclut :
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 ou ultérieur
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problème : Ne peut pas installer WebMatrix 1.0 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1

> **Solution de contournement**  
> Installer [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problème : Certains assemblys pour SQL Server Compact 4.0 ne sont pas installés dans le GAC

> Les assemblys managés pour SQL Server Compact 4.0 ne sont pas placés dans le global assembly cache (GAC) lorsque vous installez SQL Server Compact 4.0 sur un ordinateur 64 bits et de l’ordinateur a uniquement le .NET Framework 3.5 SP1 Client Profile installé. Les assemblys managés qui ne sont pas installés dans le GAC sont :
> 
> - *System.Data.SqlServerCe.dll* (fournisseur ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Solution de contournement**  
> Désinstaller SQL Server Compact 4.0. Téléchargez et installez la version complète de .NET Framework 3.5 SP1 à partir de l’emplacement suivant :  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (indépendant)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Puis réinstallez SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problème : Impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande

> La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.
> 
> **Solution de contournement**  
> Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pages web ASP.NET

Cette section du document décrit les nouvelles fonctionnalités, des modifications et des problèmes connus avec la 1.0 version d’ASP.NET Web Pages avec syntaxe Razor.

- [Nouvelles fonctionnalités](#NewFeatures)
- [Modifications](#Changes)
- [Problèmes](#Issues)

#### <a id="NewFeatures"></a>  Nouvelles fonctionnalités

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nouveau : Paramètre de configuration ajouté pour désactiver le Gestionnaire de package

> Un nouveau `asp:AdminManagerEnabled` clé est disponible pour le `<appSettings>` élément dans le *web.config* fichier, ce qui vous permet de désactiver complètement le Gestionnaire de package. La valeur par défaut pour cet élément est la valeur est true, ce qui signifie que s’il n’est pas inclus dans le *web.config* fichier, le Gestionnaire de package est activé. Pour désactiver le Gestionnaire de package, ajoutez l’élément suivant à la *web.config* fichier à la racine du site Web :
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Modifications

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Modifier : la clé de « webPages:AdminFolderVirtualPath » renommée en « asp : AdminFolderVirtualPath »

> Le `webPages:AdminFolderVirtualPath` clé qui peut être ajouté à la *web.config* fichier pour spécifier l’emplacement du Gestionnaire de package a été renommé pour utiliser le `asp:` espace de noms au lieu du `webPages` espace de noms. Si vous avez utilisé cet élément, vous devez le renommer dans le fichier de configuration.


#### <a id="Issues"></a>  Problèmes connus

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problème : Mots de passe pour les utilisateurs d’appartenance n’est plus reconnus

> L’algorithme pour la création et le stockage des mots de passe de l’appartenance (connexion) a été modifié pour être plus sûr. Par conséquent, les mots de passe stockés pour les membres (utilisateurs) créés dans les versions bêta d’ASP.NET Razor ne seront pas reconnues. 
> 
> **Solution de contournement** si le site n’a pas encore été mis en production, supprimer les enregistrements d’utilisateur à partir de la base de données d’appartenance. Si la base de données est en ligne, par programmation régénérer des mots de passe existants dans la base de données d’appartenance.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problème : Comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance

> Pour initialiser le fournisseur d’appartenances pour un site Web ASP.NET Razor, vous appelez le `WebSecurity.InitializeDatabaseConnection` (méthode). (Dans WebMatrix, du modèle Starter Site inclut un appel à cette méthode dans le  *\_AppStart.cshtml* fichier.) Si le `autoCreateTables` paramètre de cette méthode est définie sur true (par défaut, il est défini sur true dans le modèle Starter Site), et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne lève pas une erreur. Au lieu de cela, il crée automatiquement la table.
> 
> Cela peut poser un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais passez le nom de table incorrect à la `WebSecurity.InitializeDatabaseConnection` (méthode). Étant donné que la méthode ne pas par défaut déclenche une erreur si la table spécifiée n’existe pas, et parce qu’il crée à la place une nouvelle table, l’application peut apparaître à travailler. Toutefois, code d’application qui s’appuie sur la table utilisateur personnalisée (et sur les champs qu’il contient) peut finit par échouer avec des erreurs inattendues.
> 
> **Solution de contournement**  
> Assurez-vous que le nom transmis dans le `InitializeDatabaseConnection` correspondances méthode le profil utilisateur figurant dans la base de données d’appartenance, ou vous assurer que le `autoCreateTables` paramètre est défini sur false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problème : Message d’erreur « le Module d’administration requiert l’accès à ~/App\_données »

> Dans certaines circonstances, essayez de créer des utilisateurs ou de travailler avec le système d’appartenance ASP.NET peut provoquer la page pour afficher l’erreur *le Module d’administration requiert l’accès à ~/App\_données*. Cela se produit si le compte IIS ou IIS Express sous lequel s’exécute ne dispose pas des autorisations pour créer et écrire dans le *application\_données* dossier sous la racine du site Web. 
> 
> **Solution de contournement** créer manuellement un *application\_données* dossier pour le site Web. Assurez-vous que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations en lecture/écriture pour les dossiers racine de l’application et pour les sous-dossiers, par exemple application\_données. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problème : Erreur « Impossible de générer une instance utilisateur de SQL Server »

> Si une application Web de WebMatrix utilise SQL Server Express et est en cours d’exécution IIS 7.5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’utilisateur local d’application en cours d’exécution.
> 
> **Solution de contournement** vous assurer que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et des sous-dossiers comme *application\_données*. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problème : Les fichiers qui contient les ressources du Gestionnaire de package ou les mots de passe de gestionnaire de package sont étant délivrables sous IIS 6.0 et versions antérieures

> Si vous déployez une application ASP.NET Web Pages (Razor) qui a été créée à l’aide de la version RC2, et si l’application contient un *password.txt* ou *packagesources.txt* de fichiers sous */App\_ Données/admin*, IIS 6.0 renverra au fichier si nécessaire, les exposant potentiellement les mots de passe pour votre instance de gestionnaire de package. 
> 
> **Solution de contournement** renommer le *password.txt* ou *packagesources.txt* fichier *password.config* ou *packagesources.config*. Par défaut, IIS 6.0 ne traite pas les fichiers qui ont le *.config* extension. (Dans IIS 7, aucun fichier dans le *application\_données* dossier sont pris en charge, vous n’avez pas besoin de renommer les fichiers.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problème : Désinstallation des packages installés à l’aide de la version bêta 3 ne supprime pas complètement les composants de package

> Si vous installé un package à l’aide du Gestionnaire de package dans la version bêta 3 et réessayez de désinstaller à l’aide de la version actuelle, le package n’est pas complètement désinstallé. À l’aide du Gestionnaire de package **désinstallation** bouton supprime certains composants, mais laisse le code de la bibliothèque du package et ne pas mettre à jour le *package.config* fichier.
> 
> **Solution de contournement**   
> Procédez comme suit :  
> 1. Supprimer le *application\_Data\packages* dossier. Cette opération supprime tous les packages.   
> 2. Supprimer le *packages.config* fichier à la racine du site Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problème : Dans Visual Studio, appeler le Gestionnaire de package basé sur le web met l’application hors connexion

> Si vous travaillez dans Visual Studio (pas WebMatrix) et que vous utilisez le  *\_administrateur* fonctionnalité pour démarrer le Gestionnaire de package, Visual Studio met l’application hors connexion et publie le *application\_ Offline.htm* dans la racine du site Web, ce qui interrompt votre capacité à utiliser le Gestionnaire de package.
> 
> [!NOTE]
> Bien que vous verriez généralement ce comportement lors de l’utilisation de l’interface de gestionnaire de package basé sur le web, le même comportement se produit si vous ajoutez, supprimez ou modifiez des fichiers dans le *application\_données* dossier.
> 
> **Solution de contournement**   
> Pour utiliser des packages dans Visual Studio, utilisez l’extension NuGet au lieu du Gestionnaire de package basé sur le web. Pour plus d’informations, consultez le [documentation de NuGet](https://docs.microsoft.com/nuget/). Si vous travaillez avec d’autres fichiers dans le *application\_données* dossier, envisagez de conserver les fichiers ailleurs pour éviter ce problème. Si ce n’est pas pratique, supprimez le *application\_offline.htm* fichier manuellement ou patienter jusqu'à ce que le site revient en ligne automatiquement (par défaut, après 30 secondes).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problème : Visual Studio IntelliSense et projet de modèles disponibles uniquement dans ASP.NET MVC version 3

> L’installation d’ASP.NET Web Pages ne pas également installe outils pour Visual Studio telles que des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages.
> 
> **Solution de contournement** pour utiliser des modèles de projet et IntelliSense pour les applications de Pages Web ASP.NET dans Visual Studio, installer ASP.NET MVC 3 RC via Web Platform Installer ou le [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problème : Flux de lecture ou d’autres données externes via un serveur proxy

> Si le serveur qui exécute le site est derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le *web.config* fichier afin d’être en mesure de lire les informations provenant de l’extérieur de votre site. Par exemple, si vous utilisez le `ReCaptcha` helper, l’application d’assistance communique avec le service reCAPTCHA, mais peut être bloquée par votre serveur proxy. De même, les flux sont utilisés dans les Pages Web ASP.NET, par exemple, le flux utilisé par le Gestionnaire de package, peuvent nécessiter la configuration du proxy.
> 
> Si vous rencontrez des problèmes dans avec un service externe ou fonctionnent correctement avec le flux du package, placez les éléments suivants dans la racine de votre application *web.config* fichier :
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Pour plus d’informations sur la configuration d’un serveur proxy, consultez [ &lt;proxy&gt; , élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problème : Désinstaller le .NET Framework version 4 désactive ASP.NET Web Pages avec syntaxe Razor

> Si vous désinstallez le Kit de développement .NET Framework version 4, puis le réinstaller, ASP.NET Web Pages avec syntaxe Razor est désactivés. Pages avec la *.cshtml* extension ne s’exécutent pas correctement. ASP.NET Web Pages enregistre un assembly dans la racine de la machine *web.config* des fichiers et la suppression de .NET Framework supprime ce fichier. Réinstallez le .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence de l’assembly d’ASP.NET Web Pages.
> 
> **Solution de contournement** après la réinstallation de .NET Framework, réinstallez ASP.NET Web Pages avec syntaxe Razor. Cette opération ajoute l’élément suivant à la *web.config* fichier à la racine de l’ordinateur, qui est en général à l’emplacement suivant :  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problème : URL sans extension ne trouvent pas les fichiers.cshtml/.vbhtml sur IIS 7 ou IIS 7.5

> Sur IIS 7 ou IIS 7.5, les requêtes avec une URL semblable à la suivante ne sont pas en mesure de trouver des pages qui ont le *.cshtml* ou *.vbhtml* extension :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Le problème survient car la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7.5. Le scénario voyez généralement cette est que vous ne voyez pas le problème lorsque vous testez localement à l’aide d’IIS Express, mais que vous le rencontrez lorsque vous déployez votre site Web sur un site Web d’hébergement.
> 
> **Solution de contournement**
> 
> - Si vous avez un contrôle sur l’ordinateur du serveur, sur l’ordinateur serveur installer la mise à jour qui est décrite dans [une mise à jour est disponible que permet certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont les URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).
> - Si vous n’avez pas de contrôle sur l’ordinateur du serveur (par exemple, vous déployez sur un site Web d’hébergement), ajoutez le code suivant du site Web *web.config* fichier : 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problème : Déploiement d’une application sur un ordinateur qui n’a pas de SQL Server Compact installé

> Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur où SQL Server Compact n’est pas installé. Microsoft WebMatrix 1.0 automatiquement des copies de ces fichiers binaires et effectue approprié *web.config* transformations de fichiers.
> 
> **Solution de contournement** si vous devez copier ces fichiers et rendre le *web.config* modifications de fichiers manuellement, effectuer les opérations suivantes :
> 
> 1. Copiez les assemblys du moteur de base de données à la *Bin* dossier (et sous-dossiers) de l’application sur l’ordinateur cible :  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>à</em></strong>\Bin\x86*
>    - Copie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>à</strong><em>\Bin\amd64</em>
> 
> 2. Dans le dossier racine du site Web, créez ou ouvrez un *web.config* fichier. (Dans WebMatrix, 1.0, ce type de fichier est disponible si vous cliquez sur **tous les** dans le **choisir un Type de fichier** boîte de dialogue.)
> 3. Ajoutez l’élément suivant en tant qu’enfant de le `<configuration>` élément (pas à l’intérieur de la `<system.web>` élément) :
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problème : « Database » et « WebGrid » helpers ne fonctionnent pas dans Medium Trust en Visual Basic

> Si vous utilisez Visual Basic (création *.vbhtml* fichiers), le `Database` et `WebGrid` helpers ne fonctionnera pas si l’application est configurée pour utiliser la confiance moyenne.
> 
> **Solution de contournement**  
> Si vous utilisez Visual Studio 2010, vous pouvez résoudre ce problème en installant la version Service Pack 1. Jusqu'à ce que la version finale de la version SP1 est disponible, vous pouvez télécharger la version bêta du SP1 depuis le [Microsoft Visual Studio 2010 Service Pack 1 bêta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page du Microsoft Download Center.   
>   
> Si ce n’est pas pratique, ou si vous n’utilisez pas Visual Studio 2010, vous pouvez temporairement définie l’application pour utiliser la confiance totale.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problème : Ressources de « ApplicationPart » sont accessibles en externe

> Si un assembly contient des objets qui dérive de la `ApplicationPart` classe, que les ressources de l’assembly sont exposées par le `ResourceRouteHandler` classe. Considérez par exemple l’URL suivante :  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Cette requête télécharge toutes les chaînes de ressources dans le *System.Web.WebPages.Administration.dll* assembly. Toutes les ressources incorporées (même ceux qui ne sont pas destinées à être traitées en tant que contenu statique) sont téléchargées. Si les ressources intégrées contiennent des informations sensibles, cela peut représenter un risque de sécurité. 
> 
> **Solution de contournement**   
> Si vous créez un **ApplicationPart** d’objet, assurez-vous que les ressources incorporées associés **ApplicationPart** assembly de l’objet ne contiennent pas d’informations sensibles.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Pour plus d’informations sur les problèmes d’installation de WebMatrix, consultez [problèmes d’Installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.


Cette section du document décrit les problèmes connus pour l’environnement de développement WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problème : Modifications dans le nom d’utilisateur ou le mot de passe d’une chaîne de connexion de base de données dans un fichier web.config ne sont pas répercutées dans l’espace de travail de bases de données

> **Solution de contournement**  
> 
> 1. Dans le *web.config* , modifiez le nom de la base de données dans la chaîne de connexion (par exemple, ajoutez « 1 » à ce dernier).
> 2. Enregistrer le *web.config* fichier.
> 3. Cliquez sur **bases de données** et actualiser.
> 4. Modifier le nom de la base de données dans la chaîne de connexion dans le *web.config* fichier vers le nom de base de données d’origine.
> 5. Enregistrer le *web.config* fichier.
> 6. Cliquez sur **bases de données** et actualiser.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problème : Impossible de supprimer les dossiers créés par WebMatrix

> Si WebMatrix est en cours d’exécution à l’aide d’autorisations élevées (autrement dit, vous avez démarré à l’aide de WebMatrix le **exécuter en tant qu’administrateur** option dans Windows), les dossiers qui sont créés par WebMatrix ne peut pas être supprimés à l’aide de l’Explorateur Windows.
> 
> **Solution de contournement**  
> Exécutez l’Explorateur Windows à l’aide d’autorisations élevées. Procédez comme suit :  
> 
> 1. Dans Windows, cliquez sur **Démarrer**.
> 2. Entrez « Windows Explorer » et cliquez sur l’entrée pour **Windows Explorer**.
> 3. Cliquez sur **exécuter en tant qu’administrateur**. Vous pouvez ensuite supprimer les dossiers.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problème : WebMatrix 1.0 est impossible d’effectuer certaines tâches qui requièrent une élévation

> WebMatrix 1.0 ne peut pas effectuer certaines tâches qui requièrent une élévation, telles que l’installation des composants supplémentaires dans les situations suivantes :
> 
> - Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas des privilèges d’administrateur et contrôle de compte utilisateur (UAC) est désactivé.
> - Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solution de contournement**  
> La plupart des tâches dans WebMatrix 1.0 ne nécessitent pas d’autorisation d’administrateur. Pour les autres, vous pouvez effectuer l’opération en tant qu’administrateur, ou procédez comme suit :
> 
> - Sur Windows Vista ou Windows 7, activer l’UAC.
> - Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité Administrateurs.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problème : « Site à partir de la galerie Web » est désactivé.

> Le **Site à partir de la galerie Web** option est désactivée si le serveur Web Platform Installer 3.0 n’est pas installé.
> 
> **Solution de contournement**  
> Installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problème : Google Chrome n’est pas disponible en tant qu’une option d’exécution

> Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sur le **accueil** onglet.
> 
> **Solution de contournement**  
> Certaines versions de Google Chrome n’inscrivez pas eux-mêmes correctement avec la fonctionnalité programmes par défaut dans Windows. Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le *personnaliser et contrôle Google Chrome* menu, cliquez sur *Options*, puis cliquez sur *Make Google Chrome mon navigateur par défaut*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problème : La boîte de dialogue « Foreign Key » n’autorise pas entrer une clé primaire

> Le **clé étrangère** boîte de dialogue ne vous permet pas à entrer le nom de clé primaire à partir de la table de clé primaire.
> 
> **Solution de contournement**  
> Ceci est intentionnel. Vous n’avez pas besoin d’entrer le nom de la clé primaire à partir de la table de clé primaire.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problème : IntelliSense n’est pas disponible dans WebMatrix pour la syntaxe Razor, C#, ou Visual Basic

> IntelliSense est prise en charge dans WebMatrix pour HTML et CSS. Toutefois, il n’est pas disponible pour d’autres langages. 
> 
> **Solution de contournement**   
> Aucun.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problème : IntelliSense pour CSS et HTML suggère des éléments qui ne sont pas appropriées en fonction du contexte

> IntelliSense pour le balisage dans WebMatrix prend en charge le HTML à l’aide du [schéma XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) et à l’aide de CSS le [CSS 2.1 schéma](http://www.w3.org/TR/CSS2/). IntelliSense étant basé sur ces schémas spécifiques, certaines balises, des attributs ou propriétés peuvent suggérées qui ne sont pas appropriées pour la définition de style ou de la page actuelle. Pour HTML, il peut également entraîner suggestions inattendues dans le contenu qui peut être interprétée en tant que XHTML incorrect (par exemple, lorsque les balises ne sont pas fermées). Ce problème peut être plus visible si le point d’insertion est à l’intérieur d’une balise incomplète ; Dans ce cas, IntelliSense peut suggérer des balises d’ouverture ou proposer d’autres suggestions incorrectes. 
> 
> **Solution de contournement**   
> Pour HTML, assurez-vous que vous travaillez dans une page XHTML correcte et complet. Pour le CSS, il n’existe aucune solution de contournement.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problème : IntelliSense n’est pas appelée en cours de frappe

> Dans certains cas, IntelliSense ne peut pas être appelée comme code HTML ou CSS est entré dans l’éditeur. En particulier, cela peut se produire lorsque le point d’insertion est directement en regard d’un autre élément ou à la fin d’un fichier. 
> 
> **Solution de contournement**   
> Assurez-vous qu’il y a un espace blanc autour du point d’insertion et que le point d’insertion n’est pas à la fin d’un fichier. Vous pouvez également appeler IntelliSense manuellement en appuyant sur Ctrl + espace.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problème : Aucune interface utilisateur n’est disponible pour la désactivation d’IntelliSense

> WebMatrix 1.0 ne fournit aucune interface utilisateur ou un mouvement de la désactivation d’IntelliSense. 
> 
> **Solution de contournement**   
> Démarrez WebMatrix à l’aide de la commande suivante, qui inclut un commutateur qui désactive IntelliSense :  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express a son propre fichier readme, qui est disponible à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact a son propre fichier readme, qui est disponible à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Pour plus d’informations sur les problèmes qui impliquent l’installation de SQL Server Compact comme faisant partie de WebMatrix, consultez [problèmes d’Installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.

### <a id="Known_Issues_Installing_Applications"></a>  Installation d’Applications

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problème : Installation d’une application peut prendre un certain temps si le dossier Mes Documents de l’utilisateur est redirigé vers un partage réseau

> **Solution de contournement**  
> Aucun. L’application peut prendre un certain temps à installer, mais ne s’installe correctement.


### <a id="Known_Issues_Publishing_Applications"></a>  Publication d’Applications

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problème : Erreur « Requises autorisations ne peuvent pas être acquises » lors de la publication d’une base de données SQL Compact

> WebMatrix ne prend pas entièrement en charge déployer les fichiers binaires de prise en charge pour SQL Server Compact sur un serveur qui est en cours d’exécution .NET Framework version 3.5 avec une configuration de niveau de confiance moyen.
> 
> **Solution de contournement**  
> La solution préférée consiste à installer le .NET Framework 4 sur le serveur. Vous pouvez également effectuer les opérations suivantes :
> 
> 1. Ajoutez les éléments suivants à la `SecurityClasses` section *Web\_MediumTrust.config* fichier :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Créer une nouveau jeu d’autorisations dans le *Web\_MediumTrust.config* fichier avec les autorisations requises suivantes :
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Appliquer la jeu d’autorisations à SQL Server Compact en plaçant les éléments suivants dans le *Web\_MediumTrust.config* fichier :
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problème : Applications web galerie et PhpBB affichent une erreur « Le Service n’est pas disponible » après la publication

> Dans certaines circonstances, la publication d’une application provoque une erreur « le service n’est pas disponible ».
> 
> **Solution de contournement**  
> Dans WebMatrix, ajoutez une barre oblique inverse (\) à la fin du nom du serveur dans le **paramètres de publication** fenêtre puis publier à nouveau l’application.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problème : Mise en page du site Web de Moodle et les liens sont rompus après la publication

> Une fois que vous publiez une application de Moodle, l’application ne fonctionne pas correctement.
> 
> **Solution de contournement**  
> Dans WebMatrix, ajoutez une barre oblique (/) à la fin de la **nom du Site** champ dans le **paramètres de publication** fenêtre puis publier à nouveau l’application.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problème : Publication nopCommerce échoue avec une erreur de base de données

> NopCommerce publication échoue et signale une erreur de base de données, comme « insérer dans le nop\_table du journal a échoué. »
> 
> **Solution de contournement**  
> 
> 1. Dans WebMatrix, cliquez sur **exécuter** pour lancer nopCommerce localement.
> 2. Connectez-vous à la page d’administration.
> 3. Cliquez sur le **système** menu.
> 4. Cliquez sur le **journal** option.
> 5. Cliquez sur le **effacer le journal** bouton.
> 6. Republiez nopCommerce.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problème : Silverstripe CMS affiche une « erreur HTTP 500 PHP FCGI » lorsque vous téléchargez un site publié

> **Solution de contournement**  
> Après avoir cliqué sur **téléchargement publiée site**, ignorer `silverstripe-cache/manifest_main` dans **publier l’aperçu**. Ce fichier est utilisé pour la mise en cache et est spécifique à chaque ordinateur.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problème : SubText affiche « Erreur de serveur dans l’Application '/' » lorsque vous téléchargez un site publié

> **Solution de contournement**  
> Ouvrez le site *web.config* fichier et remplacez l’ID utilisateur et le mot de passe dans la chaîne de connexion de base de données avec les informations d’identification d’administrateur SQL Server (les informations d’identification « sa »).
> 
> Vous pouvez également procéder comme suit afin d’accorder le compte d’utilisateur que vous êtes connecté avec `db_owner` autorisations :
> 
> 1. Installer SQL Server Management Studio à l’aide de Web Platform Installer.
> 2. Se connecter à l’instance de SQL Server Express locale (par défaut, `.\SQLEXPRESS`).
> 3. Cliquez sur **bases de données** &gt; *[localSubtextDatabase]* &gt; **sécurité** &gt; **utilisateurs** &gt; *[localSubtextUser*] (valeur par défaut est `subtextuser`], avec le bouton droit, puis cliquez sur **propriétés**.
> 4. Sélectionnez **db\_propriétaire** dans la section de l’appartenance au rôle.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problème : Site peuvent ne pas fonctionne après la publication si le champ « URL de Destination » n’est pas précédé par http:// ou https://

> Dans le **paramètres de publication** boîte de dialogue, si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionne après le déploiement.
> 
> **Solution de contournement**  
> Assurez-vous que l’option avant de publier un site, l’URL de destination dans le **paramètres de publication** boîte de dialogue commence par `http://` ou `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problème : Publication d’une base de données MySQL échoue avec l’erreur « Échec de publication de la base de données. Cela peut se produire si la base de données distante ne peut pas exécuter le script. »

> L’erreur peut se produire pour plusieurs raisons. Vous pouvez voir cette erreur est si le script de base de données contient un guillemet simple (') et le jeu de caractères par défaut de la destination MySQL de base de données n’est pas au format UTF-8.
> 
> **Solution de contournement**  
> Définir le caractère par défaut définie pour la base de données MySQL à distance au format UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problème : Certains liens ne sont pas visibles dans DotNetNuke après avoir publié ou de téléchargement du site

> Si vous publiez ou téléchargez un site DotNetNuke, vous devrez peut-être effacer le cache pour obtenir les nouveaux liens à afficher sur le site.
> 
> **Solution de contournement**
> 
> 1. Connectez-vous en tant que « Host ».
> 2. Accédez au menu hôte et sélectionnez **paramètres de l’hôte**.
> 3. Faites défiler et sous **paramètres avancés**, développez **les paramètres de performances**.
> 4. Cliquez sur le **vider le Cache** lien pour les pages.
> 5. Accédez au bas de la page et redémarrer l’application.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problème : Certains liens dans AtomSite sont interrompues après avoir téléchargé un site publié

> **Solution de contournement**  
> Dans le *service.config* fichier, *users.config* fichier et tous *.xml* fichiers, remplacez la chaîne d’URL (par exemple, `http://myhost.com/atomsite`) avec celui local (par exemple, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problème : Basée sur MySQL les applications telles que WordPress ne pas publier et de signaler une erreur de base de données

> Par défaut, WebMatrix installé MySQL avec le jeu de caractères UTF-8. Si vous installez MySQL sur votre propre, et le jeu de caractères n’est pas UTF-8 (par exemple, il est Latin1), le processus de publication pour les bases de données peut échouer.
> 
> **Solution de contournement**
> 
> 1. Modifier le jeu de caractères pour MySQL au format UTF-8. (Pour plus d’informations, consultez [serveur du jeu de caractères et le classement](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sur le site Web MySQL.)
> 2. Réinstallez l’application.
> 3. Republier l’application.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problème : « Télécharger le site publié » échoue pour les applications qui ont le programme d’installation basé sur navigateur

> Certaines applications (par exemple, Kentico CMS) vous obligent à les lancer dans le navigateur pour exécuter le programme d’installation après l’installation telles que la création d’une base de données. Si vous publiez une application comme celle-ci sans terminer l’installation basée sur navigateur, tentez de télécharger le même site à partir d’un serveur distant échoue.
> 
> **Solution de contournement**  
> Terminer le programme d’installation basée sur navigateur avant de publier le site.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problème : « Télécharger le site publié » échoue avec une erreur de base de données pour DotNetNuke et CMS Kooboo

> Si vous essayez de télécharger une application à partir d’un serveur et que vous disposez des informations d’identification d’administrateur dans la chaîne de connexion de base de données dans le **paramètres de publication** boîte de dialogue, vous pouvez voir l’erreur suivante dans le journal de publication :
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solution de contournement**  
> Si cela est possible, republiez le site (ou publication) à l’aide des informations d’identification non-administrateur pour la base de données.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Pour plus d'informations

Pour plus d’informations sur WebMatrix 1.0, consultez les sites Web suivants :

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tous droits réservés. [Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).
