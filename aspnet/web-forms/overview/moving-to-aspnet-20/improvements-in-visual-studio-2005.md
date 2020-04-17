---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Améliorations dans Visual Studio 2005 Microsoft Docs
author: rick-anderson
description: Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations aux projets Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: e98771614bf4e0095f8ff596e7cdb26c8c9de1ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542974"
---
# <a name="improvements-in-visual-studio-2005"></a>Améliorations de Visual Studio 2005

par [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations aux projets Web.

Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations aux projets Web. Aussi puissant que Visual Studio .NET 2002 et 2003 sont, il y avait beaucoup de plaintes dans la façon dont les projets Web ont été traités. Visual Studio 2005 ajoute un nombre important de nouvelles fonctionnalités afin de répondre à ces plaintes. Pour ceux qui préfèrent la façon dont Visual Studio .NET 2003 géré compilation d’applications Web, voir [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).

Dans ce module, couvrez bien l’amélioration de la création, de la gestion et du développement de projets Web. Dans un module ultérieur, bien couvrir les améliorations dans la construction de projets Web et leur déploiement.

## <a name="frontpage-server-extensions"></a>Extensions de serveur FrontPage

Visual Studio .NET 2002 et 2003 ont exigé des extensions de serveur FrontPage sur la boîte afin de créer ou de construire des projets Web. Les développeurs avaient le choix entre deux modes d’accès différents (Extensions de serveur FrontPage ou mode d’accès aux fichiers), les deux ont utilisé Des extensions de serveur FrontPage pour effectuer des tâches telles que la définition de la racine d’application dans l’IIS, etc.

Visual Studio 2005 supprime la dépendance à l’égard des extensions de serveur FrontPage pour les projets locaux. Visual Studio 2005 accède désormais directement à la métabase IIS au lieu d’utiliser les extensions de serveur FrontPage. Visual Studio 2005 ajoute également un support pour FTP qui permet l’accès à distance du projet sans nécessiter d’extensions de serveur FrontPage.

Pour les développeurs qui souhaitent utiliser FrontPage Server Extensions dans leurs projets, l’option est toujours disponible. Toutefois, d’après les commentaires solides de la communauté des développeurs ASP.NET, ce n’est pas une exigence.

> [!NOTE]
> Les extensions de serveur FrontPage sont toujours nécessaires pour la création de projets à distance, l’ouverture, etc.

## <a name="aspnet-development-server"></a>serveur de développement ASP.NET

Visual Studio 2005 expédie avec un nouveau serveur Web appelé ASP.NET Development Server. (Ce serveur Web était auparavant connu sous le nom de Cassini.)

Il y a plusieurs avantages du serveur de développement ASP.NET.

- Il est maintenant possible pour les non-administrateurs de développer et de déboiffer contre un serveur Web.
- Le serveur de développement ASP.NET cartographie dynamiquement les répertoires virtuels à n’importe quel endroit dans le système de fichiers permettant des emplacements de projet flexibles.
- Les utilisateurs de Windows XP Professional qui utilisent déjà IIS seront désormais en mesure de créer de nouvelles applications Web qui n’affecteront pas la structure de fichier ou de dossier de leur site Web par défaut dans IIS.

Aucune configuration spéciale n’est requise pour profiter du serveur de développement ASP.NET. Lorsqu’un projet Web hébergé sur le système de fichiers est déboché ou parcouru, Visual Studio 2005 démarre automatiquement une instance du serveur de développement ASP.NET sur un port aléatoire pour desservir la demande.

Plus d’informations seront couvertes sur le serveur de développement ASP.NET plus tard dans ce module.

## <a name="improved-file-management"></a>Amélioration de la gestion des fichiers

Dans Visual Studio 2002 et 2003, un fichier de projet (.vbproj pour VB.NET et .csproj pour C) a stocké des informations sur tous les fichiers de l’application Web. L’écran Solution Explorer est basé sur les informations de fichier dans le fichier du projet. Pour cette raison, le Solution Explorer afficherait souvent des informations inexactes dans les cas où des éditeurs externes étaient utilisés. Visual Studio 2002 et 2003 remplacerait souvent les modifications de fichiers ou n’afficheraient pas la version la plus récente des fichiers.

Visual Studio 2005 élimine le dossier du projet. Au lieu de cela, il lit les informations de fichier et de dossier directement à partir du disque, résultant en un affichage précis des fichiers dans votre projet. Étant donné que le dossier Références dans Visual Studio 2002 et 2003 ne représente pas un dossier réel dans votre application Web, Visual Studio 2005 supprime également le dossier Références de Solution Explorer. Pour accéder aux références de votre projet dans Visual Studio 2005, vous devez utiliser les pages Propriété pour le projet.

## <a name="creating-web-projects"></a>Création de projets Web

Les développeurs Web ont de nombreuses nouvelles options disponibles pour la création de projets dans Visual Studio 2005. Les sites Web peuvent maintenant être créés n’importe où dans le système de fichiers et peuvent ensuite être débogés ou parcourus à l’aide du nouveau serveur de développement ASP.NET. Les développeurs peuvent également créer de nouveaux sites Web à l’aide de FTP.

Cliquez ici pour visionner une vidéo pas à pas de la création de projets Web dans Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Ouvrez la vidéo plein écran](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Projets de système de fichiers

Comme vous l’avez vu dans la vidéo pas à pas, vous pouvez choisir de créer des sites Web sur le système de fichiers soit sur la machine locale ou sur un emplacement distant via une part de fichier. Les sites Web qui sont créés sur le système de fichiers sont parcourus et débogés à l’aide du serveur de développement ASP.NET.

> [!NOTE]
> Le serveur de développement ASP.NET peut causer une certaine confusion pour les clients. Si un projet Web est créé sur le système de fichiers dans la structure d’annuaire DE l’IISs (c.-à-d. c:/inetpub/wwwroot), le site Web sera toujours consulté via le serveur de développement ASP.NET lorsqu’il sera lancé à partir de Visual Studio 2005. Par conséquent, toute configuration DE la SIE (c.-à-d. les méthodes d’authentification) n’est pas applicable.

Le projet Web par défaut supprime également un grand nombre des frais généraux en ne comprend qu’une page Default.aspx, default.cs fichier, et un dossier App/_Data. Le web.config et les dossiers spéciaux (c’est-à-dire l’application/_code) sont ajoutés au besoin. Votre projet web ne comprend que les fichiers et dossiers dont vous avez besoin.

### <a name="http-projects"></a>Projets HTTP

Les projets HTTP peuvent être soit des projets qui sont créés sur un site Web local de l’IIS ou sur un site Web éloigné. L’emplacement par `http://localhost`défaut du projet est . Si vous cliquez sur le bouton Parcourir, il existe deux options HTTP : local IIS et Remote Site. La principale différence dans ces deux options est la méthode dans laquelle les informations du site Web est affichée dans le dialogue Choisir l’emplacement et dans la façon dont les fichiers sont copiés sur le serveur Web.

L’option Local IIS lit les informations du site à partir de la métabase sur la machine locale et les fichiers sont copiés à l’aide du système de fichiers. L’option Remote Site utilise les extensions de serveur FrontPage et les informations et fichiers du site sont copiés à l’aide d’appels RPC HTTP et FrontPage Server Extensions.

> [!NOTE]
> Le fichier vs/_tmp.htm et l’accès/_aspx/_ver.aspx ne sont plus utilisés pour déterminer les informations de version.

L’option HTTP par défaut est local IIS. Cette option lit la métabase DE l’IIS pour déterminer quels sites sont disponibles et l’emplacement dans lequel créer du contenu. Vous pouvez sélectionner un dossier différent ou un répertoire virtuel en le sélectionnant dans la vue de l’arbre. Vous pouvez également créer un nouvel annuaire virtuel, marquer des dossiers comme applications, ainsi que supprimer les répertoires virtuels existants de cette boîte de dialogue.

![Le Dialogue d’emplacement de choix](improvements-in-visual-studio-2005/_static/image1.gif)

**Figure 1**: Le dialogue de localisation de choisir

Contrairement aux versions précédentes de Visual Studio, si vous vérifiez la case à **cocher Use Secure Sockets Layer** et que le certificat SSL ne correspond pas à l’URL que vous naviguez, vous serez présenté avec un dialogue d’alerte de sécurité vous demandant si vous souhaitez procéder. Utilisation de Visual Studio .NET 2003, si le certificat n’était pas un correspondant, la création du projet échouerait.

![Alerte de sécurité concernant le certificat SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figure 2**: Alerte de sécurité concernant le certificat SSL

### <a name="note-on-host-headers"></a>Note sur les en-têtes hôtes

Si vous créez une application Web sur un site lié à une adresse IP spécifique, vous devrez vous assurer qu’un en-tête d’hôte est configuré. Dans le cas contraire, `http://localhost`Visual Studio créera le site à , mais l’adresse IP ne se résoudra pas correctement lorsque le site est parcouru ou débogé de l’intérieur de l’IDE.

Si vous sélectionnez l’option Site à distance, le dialogue change pour vous permettre d’entrer l’URL de destination pour le nouveau site Web. Cette URL doit être activée sur un serveur compatible frontPage Server Extensions. Si vous souhaitez travailler avec votre serveur Web local à l’aide des extensions de serveur FrontPage, vous pouvez utiliser l’option Site à distance et spécifier une URL locale.

![Création d’un site Web sur un serveur distant](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figure 3**: Création d’un site Web sur un serveur distant

Lors de la création d’une application sur un site distant via SSL, si le certificat SSL ne correspond pas, le dialogue de confirmation est légèrement différent du dialogue affiché lors de l’utilisation de l’option local IIS.

![L’alerte de sécurité du site à distance](improvements-in-visual-studio-2005/_static/image3.gif)

**Figure 4**: L’alerte de sécurité du site à distance

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 introduit l’option de créer des sites Web via FTP. Lorsque vous utilisez cette option, l’IDE crée les fichiers localement dans le dossier d’accès aux utilisateurs, puis utilise FTP pour déplacer les fichiers vers l’emplacement FTP.

> [!NOTE]
> L’emplacement du dossier temporaire est&lt;c:/Documents and Settings/ User&gt;/Local&lt;&gt;Settings/Temp/VWDWebCache/ Server /MD&lt;nom de l’application&gt;

Lors de l’utilisation de l’option FTP, vous serez présenté avec un dialogue Choisir emplacement. Vous entrez les informations de connexion FTP requises dans ce dialogue tel qu’indiqué ci-dessous.

![Le Dialogue de localisation Choisir pour FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figure 5**: Le dialogue de localisation de choix pour FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratoire : Configuration du site FTP et création d’un projet

Les étapes suivantes configurent le site FTP de sorte qu’un utilisateur dispose d’un emplacement que seul il peut télécharger via FTP.

### <a name="install-the-ftp-service"></a>Installer le service FTP

1. Ouvrez les programmes d’élimination, sélectionnez les composants Windows Ajouter/Supprimer
2. Sélectionnez des services d’information Internet (serveur d’application sur Windows 2003) et cliquez sur **Détails**.
3. Vérifiez **le service du protocole de transfert de fichiers (FTP)** et cliquez sur **OK**.
4. Cliquez **sur Next** pour installer le service FTP.

### <a name="create-a-new-folder-for-content"></a>Créer un nouveau dossier pour le contenu

1. Dans Windows Explorer, créez un nouveau dossier appelé **User1** à l’intérieur de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurer les dossiers et les autorisations sur les dossiers.

1. Ouvrez les services d’information Sur Internet en passant par les outils administratifs. Vous aurez maintenant un dossier FTP Sites sous le nœud nom de l’ordinateur.
2. Élargir **les sites FTP**.
3. Cliquez à droite sur le **site FTP par défaut**, sélectionnez **Nouveau**, puis **Répertoire virtuel**, puis cliquez sur **Next**.
4. Entrez **User1** pour le nom d’annuaire virtuel et cliquez sur **Next**.
5. Entrez **c:/inetpub/wwwroot/User1** pour le chemin et cliquez sur **Next**.
6. Cliquez **sur Next,** puis **terminez** pour compléter l’assistant.
7. Cliquez à droite sur l’annuaire virtuel **User1** sous le site FTP par défaut et sélectionnez **les propriétés**.
8. Vérifiez la case **à cocher e** et cliquez **sur OK** pour fermer le dialogue.
9. Cliquer à droite **Par défaut FTP Site** et sélectionner les **propriétés**.
10. Sur l’onglet **Comptes de sécurité,** décocher **autoriser les connexions anonymes**.
11. Cliquez **oui** dans le dialogue demandant si vous voulez continuer.
12. Cliquez sur **OK** pour fermer la boîte de dialogue.
13. Élargissez le **site Web par défaut** sous le nœud Des sites **Web.**
14. Cliquez à droite sur l’annuaire **User1** et sélectionnez **les propriétés**
15. Dans la section **Paramètres d’application,** cliquez sur **Créer** pour marquer le dossier en tant qu’application.
16. Cliquez sur **OK** pour fermer la boîte de dialogue.
17. Fermez le snap-in des services d’information Sur Internet.

### <a name="create-web-project"></a>Créer un projet web

1. Open Visual Studio 2005.
2. Dans le menu **Du fichier,** sélectionnez **nouveau site Web**.
3. Dans le décrochage **de l’emplacement,** sélectionnez **FTP**.
4. Cliquez sur **Parcourir**.
5. Entrez **localhost** dans la boîte à texte **Server.**
6. Entrez **User1** dans la boîte à texte de l’annuaire.
7. Cliquez sur **Ouvrir**. L’emplacement ftP sera inscrit dans le dialogue du nouveau site Web.
8. Cliquez sur **OK**.
9. Décochez **anonymez-vous** dans le dialogue FTP Log On, entrez vos informations d’identification et cliquez **sur OK**.
10. Quelle est l’URL du projet ? (L’URL du projet sera affichée dans Solution Explorer.)
11. Dans le menu **Construire,** sélectionnez **Build Web Site** ou Build **Solution**.
12. Cliquez à droite sur Default.aspx dans Solution Explorer et sélectionnez **View in Browser**.
13. Dans l’URL du site `http://localhost/user1` Web Le dialogue requis, entrez pour l’URL et cliquez **sur OK**.

> [!NOTE]
> Si vous obtenez une erreur indiquant une incapacité à charger le type /_Default, assurez-vous que vous exécutez ASP.NET 2.0 sur votre site Web et non une version antérieure. Vous pouvez le faire à partir de l’onglet ASP.NET dans les services d’information Internet.

## <a name="opening-web-projects"></a>Ouverture de projets Web

L’ouverture de projets Web est similaire à la création de projets. Les sections suivantes appellent les zones à garder un œil sur tout en travaillant dans l’IDE. Il couvre également le travail avec des projets Web utilisant HTTP et FTP.

Pour ouvrir un projet Web, sélectionnez le site Web ouvert dans le menu Fichier. Vous serez invité avec le même dialogue De localisation De choix couvert précédemment et vous avez les quatre mêmes options à votre disposition: Système de fichiers, local IIS, FTP, et le site à distance.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Système de fichiers

Comme indiqué précédemment dans ce module, Visual Studio n’utilise plus de fichier de projet. Par conséquent, si vous choisissez d’ouvrir un site Web à partir du système de fichiers, vous avez effectivement la possibilité de choisir n’importe quel dossier que vous souhaitez, même si le dossier que vous choisissez n’a pas été créé comme un projet Web initialement dans Visual Studio. Par exemple, vous pouvez choisir d’ouvrir le dossier Mes Documents comme site Web et Visual Studio sera heureux de l’ouvrir et d’afficher vos fichiers comme indiqué ci-dessous.

![Mes documents ouverts en tant que site Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figure 6**: *Mes documents* ouverts en tant que site Web

Parce que Visual Studio ne crée des fichiers et des dossiers supplémentaires que si nécessaire, aucun fichier ou dossier supplémentaire n’est ajouté à l’emplacement que vous ouvrez. Un effet secondaire de cette architecture est qu’elle vous empêche de nicher des sites Web sur le système de fichiers. Par exemple, considérez la structure d’annuaire suivante.

Projet Web chez C:/MyWebSite

Un autre projet web à C:/MyWebSite/Nested

Lorsque vous ouvrez le site Web à c:/MyWebSite, le dossier Nested apparaîtra sous le dossier de cette application.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Lors de l’ouverture de sites Web via HTTP, les paramètres sont lus soit à partir de la métabase IIS (Local IIS) ou en utilisant FrontPage Server Extensions (Site à distance.) S’il existe des applications Web imbriquées, celles-ci sont également affichées avec une icône les identifiant comme une application. Si vous êtes familier avec le travail avec des applications web dans FrontPage, le comportement dans Visual Studio 2005 est similaire.

Même si Visual Studio affichera une icône pour les applications qui sont imbriquées sous l’application qui est actuellement ouverte dans l’IDE, il ne vous permettra pas de les étendre pour voir leur contenu. Vous pouvez, cependant, double-cliquer sur eux pour les ouvrir. Lorsque vous le faites, vous serez présenté avec un dialogue vous incitant soit à ouvrir l’application web (et remplacer la solution actuellement ouverte) ou ajouter l’application Web à votre solution actuelle.

![Double-clic une icône d’application imbriquée vous présente ce dialogue](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figure 7**: Une icône d’application imbriquée vous présente ce dialogue

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Site FTP

Lorsque vous ouvrez un site via FTP, les fichiers sont tous copiés localement à votre dossier temporaire. Le chemin complet pour l’emplacement de stockage local est affiché dans le volet Propriétés pour le projet et est créé en utilisant le format suivant.

C:/Documents and&lt;Settings/ User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/MD nom de l’application&lt;&gt;

Lors de l’utilisation de FTP, Visual Studio devra spécifier l’URL de base de votre projet afin que vous puissiez le parcourir comme indiqué ci-dessous. Si vous ne spécifiez pas une URL de base, Visual Studio vous en demandera la première fois que vous tenterez de parcourir une page sur le site Web.

![Spécifier une URL de base pour les sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figure 8**: Spécifier une URL de base pour les sites FTP

## <a name="improvements-in-compilation"></a>Améliorations de la compilation

Travailler avec des applications Web dans Visual Studio 2005 est nettement plus rapide que les versions précédentes. Cela est dû en grande partie aux changements dans l’architecture de compilation.

Dans Visual Studio 2002 et 2003, les applications Web ont été compilées en une seule assemblée primaire résidant dans le dossier /bin. Dans Visual Studio 2005, un dossier App/_Code a été ajouté. Les cours et autres codes non-UI sont ajoutés au dossier App/_Code. Lorsque Visual Studio construit le projet, tous les fichiers du dossier App/_Code sont compilés dans un seul fichier App/_Code.dll. Le résultat de ce changement est que les versions ultérieures sont beaucoup plus rapides que dans les versions précédentes.

> [!NOTE]
> L’utilitaire de ligne de commande MSBuild peut également être utilisé pour construire des applications Web ASP.NET. Cet outil sera couvert dans le module 9.

Une autre amélioration de compilation est la nouvelle option Build Page sur le menu Build. Cette fonctionnalité permet à un développeur de reconstruire uniquement la page actuelle (avec, bien sûr, et les dépendances) de sorte que les changements peuvent être compilés plus rapidement. Parce que C n’offre pas de compilation de fond aux fins de la mise à jour IntelliSense, etc, ils bénéficieront énormément de cette fonctionnalité, car il permettra à IntelliSense d’être mis à jour rapidement en reconstruisant simplement une seule page.

Les propriétés Build pour un projet vous permettent de configurer le type de build qui se produit avant l’exécution de la page de démarrage. Les développeurs peuvent choisir de ne construire la page actuelle que pour que Visual Studio puisse commencer à débogage des applications plus rapidement après les modifications du code.

![L’action de démarrage de la page Build](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figure 9**: L’action de démarrage de la page Build

Une autre grande amélioration de Visual Studio et l’architecture ASP.NET est dans le domaine de l’édition et de continuer. Dans Visual Studio 2005, les développeurs peuvent commencer à débogage d’un projet et apporter des modifications de code sur le projet sans détacher le débbugger. En fait, vous pouvez littéralement commencer à débogage d’un projet, ajouter une nouvelle classe, ajouter du code à cette classe, ajouter du code à votre page qui crée une nouvelle instance de cette classe et d’exécuter une méthode de la classe, le tout sans détacher le débbugger. Exécution du nouveau code est littéralement aussi facile que de rafraîchir le navigateur!

Cliquez ici pour voir une vidéo pas à pas de l’édition et continuer fonctionnalité dans Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Ouvrez la vidéo plein écran](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La robuste modification et la poursuite des fonctionnalités dans ASP.NET 2.0 et Visual Studio 2005 est due à un changement architectural pour les applications ASP.NET. Dans ASP.NET 1.x, les applications créées dans Visual Studio 2002/2003 ont été compilées dans un assemblage primaire qui a été stocké dans le dossier /bin. Toutes les classes, pages, etc pour l’application ont été compilées dans celui DLL. Ensuite, au moment de l’exécution, ASP.NET compiler tous les contrôles, balisage, et ASP.NET code dans les pages et copier ces DLLs dans le dossier temporaire ASP.NET.

Dans Visual Studio 2005 à l’aide de ASP.NET 2.0, les deux modèles de compilation ci-dessus (un pour Visual Studio et un pour ASP.NET à l’exécution) ont été fusionnés en un modèle de compilation commune. Cela signifie que tous les problèmes de compilation sont maintenant pris au cours de l’étape de développement au lieu de l’heure de exécution. Il permet également de prendre en charge le concepteur et IntelliSense pour des fonctionnalités telles que les contrôles des utilisateurs et les pages maîtresses.

Cliquez ici pour voir une vidéo pas à pas de support de concepteur pour les contrôles des utilisateurs.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Ouvrez la vidéo plein écran](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Lorsqu’un contrôle utilisateur est supprimé @Register d’une page, la directive reste dans la balisage et doit être supprimée manuellement afin d’éviter les erreurs de analyse si le contrôle de l’utilisateur est supprimé du site Web.

Une autre amélioration dans le modèle de compilation Visual Studio est la fonction Publish Web Site. Étant donné que la fonction Publier précompile le site Web, les développeurs peuvent profiter des performances supplémentaires de ne pas avoir à compiler quoi que ce soit sur demande. Il précalcie également tout le code source dans le dossier App/_Code dans un DLL de sorte qu’aucun code source ne doit être déployé.

![Le Dialogue du site Web de publication](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figure 10**: Le dialogue du site Web de publication

> [!NOTE]
> L’utilitaire aspnet/_compile.exe peut également être utilisé pour pré-compiler une application Web ASP.NET. Cet outil sera couvert dans le module 9.

Lorsque vous publiez un site Web, les fichiers précompilés sont stockés dans le dossier de fichiers ASP.NET temporaires tel qu’indiqué ci-dessous. Les fichiers avec une extension de fichier *.compiled* sont des fichiers XML qui définissent les dépendances pour certains DLL. Tous les contrôles Webform ou utilisateur sont compilés en DLLs aléatoires qui commencent par *App /_Web /_*.

Si vous quittez le *Allow ce site précompilé pour être vérifiée boîte à cocher updatable,* la balisage à l’intérieur de vos formulaires Web et les contrôles de l’utilisateur ne sera pas pré-compilé dans un DLL vous permettant d’apporter des modifications après le déploiement. Si vous préférez verrouiller la majoration de sorte que les modifications apportées au contenu déployé ne sont pas autorisées, décochez cette boîte.

La *case de nomage fixe Use et les assemblages d’une seule page* vous permettent de désactiver la compilation de lots afin que chaque page soit compilée dans un assemblage à nom fixe. Laisser cette boîte non cochée vous permet de profiter de la compilation de lots.

Le nom fort d’Enable sur la case à *cocher des assemblages précompilés* vous permet de nommer fort vos assemblages précompilés.

> [!NOTE]
> Dans ASP.NET 1.x, des assemblages de nom fort ont dû être installés dans le Cache de l’Assemblée mondiale (GAC). En ASP.NET 2.0, vous n’êtes pas tenu d’installer des assemblages de nom fort dans le GAC.

![Un ASP.NET applications pré-compilées fichiers](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figure 11**: Un ASP.NET demandes de fichiers pré-compilés

> [!NOTE]
> Dans l’application ci-dessus, il n’y avait pas de fichier web.config. S’il y avait eu, il aurait été appelé *PrecompiledApp.config* après le processus de site Web de publication.

## <a name="improvements-in-deployment"></a>Améliorations du déploiement

Comme pour Visual Studio 2002 et 2003, Visual Studio 2005 propose une fonctionnalité Copy Project. Cependant, la fonctionnalité a été renforcée dans Visual Studio 2005 et est maintenant appelé Copy Web Site.

Le dialogue Copy Web Site est divisé en un cadre gauche et un cadre droit. Le cadre gauche s’appelle le site Web Source et le cadre droit s’appelle le site Web à distance. Une chose qui peut confondre certains développeurs, c’est que le site affiché dans le cadre droit n’est pas nécessairement un site distant. Il pourrait s’agir d’un site sur le système de fichiers local ou sur l’instance locale de l’IIS. En outre, le site affiché dans le cadre gauche n’est pas nécessairement le site Web source parce que le dialogue vous permet de publier à partir du site Web à distance *au* site Web source.

Si vous copiez un projet sur un site Web distant, ce site doit avoir installé les extensions de serveur FrontPage. Si ce n’est pas le cas, vous devrez vous connecter à l’aide de FTP. D’autre part, si vous copiez un projet à l’instance locale IIS, Les extensions de serveur FrontPage ne sont pas nécessaires.

> [!NOTE]
> Si vous essayez de créer un nouveau site Web sur l’instance locale IIS et que les extensions de serveur FrontPage 2002 sont installées, vous obtiendrez un message d’erreur vous indiquant que la création de sites Web n’est pas prise en charge sur un serveur SharePoint. Dans ce cas, vous avez la possibilité d’installer les extensions de serveur FrontPage 2000 ou de supprimer les extensions de serveur FrontPage.

Cliquez ici pour une vidéo pas à pas de la fonction Copy Site Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Ouvrez la vidéo plein écran](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Améliorations dans Le débogage

Il y a quatre améliorations clés dans le débogage dans Visual Studio 2005.

- Débogage localement en tant que non-administrateur est possible hors de la boîte.
- L’attribut Debug pour l’élément Compilation est désormais faux par défaut.
- La configuration et la configuration de débogage à distance sont plus faciles qu’auparavant.
- Vous pouvez maintenant déboiffer un site Web ouvert via un emplacement FTP.

## <a name="debugging-as-a-non-administrator"></a>Débogage en tant que non-administrateur

L’ajout du serveur de développement ASP.NET permet aux non-administrateurs de débocher facilement ASP.NET applications dès la sortie de la boîte. Lorsqu’une application ASP.NET fonctionnant sur le système de fichiers local est débaillée, Visual Studio lance le serveur de développement ASP.NET dans le cadre de l’utilisateur connecté. Cet utilisateur peut alors déboiffer cette application sans aucune configuration supplémentaire.

## <a name="debug-is-false-by-default"></a>Debug est faux par défaut

Dans ASP.NET 1.x, *l’attribut de déboug* dans *l’élément* compilation du fichier web.config a été mis à *vrai* par défaut. Il a toujours été recommandé que les développeurs définissant cet attribut à *faux* avant de déployer une application à la production, mais parce que la plupart des développeurs ne comprennent pas pleinement les conséquences de laisser l’attribut debug mis à vrai, ils l’ont tout simplement laissé tel quel.

Le problème le plus grave avec avoir l’attribut de débog réglé à vrai, c’est qu’il désactive ASP.NETs modèle de compilation de lots. Par conséquent, chaque page est compilée dans un DLL séparé. Si une application Web se compose de milliers de pages (pas du jamais vu par tous les moyens), cela signifie que plusieurs milliers de petits DL seront créés par cette application. Bien que ces DLL sont de petite taille, ils ne sont pas chargés dans un endroit particulier dans la mémoire. Par conséquent, ils causent la fragmentation de la mémoire du système et peuvent contribuer à outOfMemoryException occurrences.

Dans ASP.NET 2.0, l’attribut de déboug est mis à faux par défaut. Comme vous l’avez déjà vu, lorsqu’un développeur débrille une application ASP.NET dans Visual Studio 2005, ils sont invités à ajouter un fichier web.config avec débogage activé. Cela entraîne les mêmes inconvénients qui étaient présents dans ASP.NET 1.x, mais maintenant le développeur est clairement averti que l’attribut doit être réinitialisé à faux avant de déplacer l’application à la production.

## <a name="remote-debugging-setup-and-configuration"></a>Installation et configuration de débogage à distance

Dans Visual Studio 2002/2003, le débogage à distance s’est appuyé sur le gestionnaire de machine Debug (mdm.exe) et le processus vs7jit.exe. Pour cette raison, dépannage des problèmes de débogage à distance était souvent une boîte noire pour les clients et il n’était souvent pas beaucoup mieux pour PSS.

Visual Studio 2005 supprime la dépendance sur les processus mdm.exe et vs7jit.exe. Au lieu de cela, il utilise maintenant le service Remote Debug Monitor (msvsmon.exe.)

L’exigence de débogage dans Visual Studio 2005 à distance est assez simple. Vous devez exécuter msvsmon.exe sur le serveur distant avant de déboguer. Vous pouvez installer le moniteur Debug à distance à partir du CD Visual Studio ou vous pouvez simplement exécuter msvsmon.exe à partir d’une part sans rien installer du tout sur le serveur Web.

Lorsque vous exécutez msvsmon.exe, il est probable qu’il se plaindra des ports bloqués pour le débogage à distance. Heureusement, vous pouvez facilement débloquer les ports à partir de droite dans le dialogue d’avertissement comme indiqué ci-dessous.

![Notification que Le pare-feu Windows bloque le débogage à distance](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figure 12**: Notification que le pare-feu Windows bloque le débogage à distance

Une fois que vous avez débloqué les ports nécessaires pour débogage, vous verrez le moniteur de débogage à distance comme indiqué ci-dessous. À partir de cette interface, vous pouvez surveiller les connexions et modifier facilement les autorisations de débogage.

![Le moniteur de débogage à distance](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figure 13**: Le moniteur de débogage à distance

Il est également possible de déboiffer à distance une application Web ouverte via FTP. Les étapes sont les mêmes que celles précédemment couvertes. Toutefois, vous devrez spécifier une URL de base pour naviguer dans le projet FTP tel que décrit précédemment dans ce module.

## <a name="lab-2"></a>Laboratoire 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Débugging à distance avec Visual Studio 2005

Ce laboratoire vous guidera à travers le débogage à distance avec Visual Studio 2005.

Cliquez ici pour une vidéo pas à pas de ce laboratoire.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Ouvrez la vidéo plein écran](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Ce laboratoire vous oblige à avoir deux machines, l’une en cours d’exécution Visual Studio 2005 et l’autre en cours d’exécution IIS 5 ou plus.

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web sur le serveur distant.

> [!NOTE]
> Vous pouvez créer le site Web sur une instance IIS à distance ou via FTP.

1. Depuis le serveur Web distant, localisez msvsmon.exe sur la machine de développement à l’aide d’un chemin UNC et exécutez-le.  
 L’emplacement par défaut pour msvsmon.exe est //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Si on les demande de débloquer les ports pour le débogage à distance, faites-le.
3. De la machine de développement, ouvrez le code-derrière pour Default.aspx et définissez un point d’arrêt dans la méthode Page/_Load.
4. Commencez à débogage de la machine de développement.

Vous devriez atteindre le point d’arrêt comme prévu.

## <a name="aspnet-development-server"></a>serveur de développement ASP.NET

Comme nous l’avons déjà mentionné, Visual Studio 2005 expédie avec un serveur Web appelé le serveur de développement ASP.NET. (Le serveur de développement ASP.NET est parfois appelé Cassini.) Ce serveur Web est un moyen pratique de naviguer et de déboguer les applications Web en cours d’exécution sur le système de fichiers.

Le serveur de développement ASP.NET est un serveur Web restreint. Il n’autorise pas les connexions à distance, il n’autorise aucune demande d’un utilisateur autre que l’utilisateur qui a lancé le serveur Web. Il n’a pas non plus la capacité de servir les pages ASP. Seules ASP.NET ressources et ressources HTML (y compris les images, les fichiers CSS, etc.) sont servies.

Le serveur de développement ASP.NET peut être lancé via la ligne de commande en exécutant le fichier WebDev.WebServer.exe situé à*/*/*/* c:/Windows/Microsoft.NET/Framework/v2.0./ / . Le dialogue suivant affiche les paramètres disponibles.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figure 14**

> [!NOTE]
> Le serveur de développement ASP.NET n’est pas pris en charge lorsqu’il est lancé explicitement via la ligne de commande.
