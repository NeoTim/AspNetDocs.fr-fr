---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET et Web Tools 2013.2 pour Visual Studio 2013 Notes de sortie (fr) Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 41b0f3ad43846bc5799ecd398188b0f6ffcc0d66
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543624"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools 2013.2 pour Visual Studio 2013 - Notes de publication

par [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notes d'installation

ASP.NET et web Tools pour Visual Studio 2013.2 sont regroupés dans l’installateur principal et peuvent être téléchargés dans le cadre de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentation

Des tutoriels et d’autres informations sur ASP.NET et les outils Web pour Visual Studio 2013.2 sont disponibles sur le [site Web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Configuration logicielle

ASP.NET et web Tools pour Visual Studio 2013.2 nécessite Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nouvelles fonctionnalités dans ASP.NET et outils Web pour Visual Studio 2013.2

Les sections suivantes décrivent les caractéristiques qui ont été introduites dans la version.

- [Un ASP.NET modèles de projets](#oneaspnet)
- [Prendre en charge SSL lors du lancement d’applications Web sur IIS Express](#ssl)
- [Améliorations de l’éditeur Web Visual Studio](#vswebeditor)
- [Lien du navigateur](#browserlink)
- [Prise en charge des applications Web Azure App Service dans Visual Studio](#waws)
- [Créer des ressources Azure à distance lors de la création d’un nouveau projet Web](#AzureResources)
- [Améliorations de publication Web](#webpublish)
- [ASP.NET échafaudage](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Formulaires web ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET API Web 2.1.2](#webapi)
- [ASP.NET Pages Web 3.1.2](#webpages)
- [Cadre de l’entité 6.1](#ef)
- [ASP.NET Identité 2.0.0](#identity)
- [Composants Microsoft OWIN](#owin)
- [ASP.NET Signalr 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Un ASP.NET modèles de projets

- Mises à jour pour ASP.NET modèles de projet pour prendre en charge la confirmation du compte et la réinitialisation des mots de passe.
- Mettre à jour ASP.NET modèle d’API Web pour prendre en charge l’authentification à l’aide de comptes organisationnels sur place.
- Le modèle SPA ASP.NET contient maintenant une authentification basée sur des vues côté MVC et serveur. Le modèle dispose d’un contrôleur WebAPI qui ne peut être consulté que par les utilisateurs authentifiés.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Prendre en charge SSL lors du lancement d’applications Web sur IIS Express

Pour éliminer l’avertissement de sécurité lors de la navigation et de la débogage HTTPS sur localhost, nous avons ajouté un dialogue pour permettre à Internet Explorer et Chrome de faire confiance au certificat auto-signé IIS express SSL.

Par exemple, une propriété de projet Web peut être définie pour utiliser SSL. Cliquez sur F4 pour faire le dialogue des propriétés. Changer **SSL Enabled** à vrai. Copiez l’URL SSL.

![Propriété SSL Enabled](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Définissez l’onglet Web de la page de propriété du projet `https://localhost:44300/` Web pour utiliser l’URL BASÉE sur HTTPS (l’URL SSL sera à moins que vous ayez déjà créé des sites Web SSL.)

![Définir l’URL du projet (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Appuyez sur Ctrl+F5 pour exécuter l’application. Suivez les instructions pour faire confiance au certificat autosigné qu’IIS Express a généré.

![Avertissement SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lisez le dialogue **d’avertissement de sécurité** et cliquez ensuite sur **Oui** si vous voulez installer le certificat représentant localhost.

![Avertissement de sécurité](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Le site sera affiché dans IE ou Chrome sans l’avertissement de certificat dans le navigateur.

![Page HTTPS sans avertissements](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox utilise son propre magasin de certificats, de sorte qu’il affichera un avertissement.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web Visual Studio

- **Nouvel élément du projet JSON et éditeur**: Nous avons ajouté un élément de projet JSON et rédacteur en chef à Visual Studio. Les caractéristiques actuelles de l’éditeur JSON comprennent la colorisation, la validation syntaxique, l’achèvement de l’accolade, la définition, le réglage d’options d’outils et plus encore.

    ![Rédacteur en chef de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense prend désormais en charge [JSON Schema](http://json-schema.org/) v3 et v4. Il ya une boîte combo schéma pour choisir les schémas existants, modifier le chemin schéma local, ou tout simplement glisser déposer un fichier JSON projet à elle pour obtenir le chemin relatif.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON Schema éditeur](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nouveau rédacteur en chef de Sass (SCSS)**: Nous avons ajouté LESS dans VS2013 RTM, et nous avons maintenant un élément de projet Sass et un éditeur. Les caractéristiques de l’éditeur Sass sont comparables à l’éditeur LESS, et comprennent la colorisation, variable et Mixins IntelliSense, commentaire / uncomment, informations rapides, formatage, validation syntaxe, décrivant, définition goto, cueilleur de couleurs, outils de réglage d’option, etc.

    ![Ajouter un nouvel article: Feuille de style SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Éditeur de feuille de modèle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nouveau picker URL dans HTML, Razor, CSS, LESS et Sass documents:** VS 2013 expédié sans cueilleur d’URL en dehors des pages Web Forms. Le nouveau cueilleur d’URL pour HTML, Razor, CSS, LESS et Sass est un cueilleur de dactylographie sans dialogue et fluide qui comprend «. et filtre les listes de fichiers de manière appropriée pour les balises et les liens img.

    ![URL Picker pour l’étiquette d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL Picker pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL Picker pour CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Mises à jour de l’éditeur LESS en ajoutant plus de fonctionnalités**
- **Knockout Intellisense Upgrade**: Nous avons ajouté une syntaxe KnockOut non standard pour VS intelliSense, "ko-vs-éditeur viewModel:" syntaxe. Il peut être utilisé pour lier à plusieurs modèles de vue sur une page en utilisant des commentaires dans la forme:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Nous avons également ajouté un soutien pour l’intelliSense ViewModel imbriqué, afin que vous puissiez forer dans des objets profondément imbriqués sur le ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    L’IntelliSense affiché est l’IntelliSense complet de l’objet JavaScript.

    ![Intellisense montrant l’objet JavaScript complet](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nouveau picker URL dans HTML, Razor, CSS, LESS et Sass documents**: VS 2013 expédié sans cueilleur d’URL en dehors des pages Web Forms. Le nouveau cueilleur d’URL pour HTML, Razor, CSS, LESS et Sass est un cueilleur de dactylographie sans dialogue et fluide qui comprend «. et filtre les listes de fichiers de manière appropriée pour les balises et les liens img.

    ![URL Picker pour l’étiquette d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL Picker pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL Picker pour CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Lien du navigateur

- Browser Link prend désormais en charge les connexions HTTPS et répertoriera cela dans Dashboard avec d’autres connexions tant que le certificat est approuvé par navigateur.
- Cartographie statique des sources HTML
- Support SPA pour la cartographie des données
- Données cartographiques auto-mises à jour

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Prise en charge des applications Web Azure App Service dans Visual Studio

- **Soutenez azure connectez-vous.**
- **Débogage à distance et vision à distance pour les applications web**: Nous prenons désormais en charge le [débogage à distance pour les applications Web dans Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) et la vue à distance des fichiers de contenu web dans l’explorateur serveur.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Créer des ressources Azure à distance lors de la création d’un nouveau projet Web

Nous avons ajouté une case à cocher Azure ["Create Remote Resources"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) sur le nouveau dialogue d’application web. En le choisissant, vous serez en mesure d’intégrer l’expérience de création d’une nouvelle application web, la mise en place du site d’édition Azure pour les tests, et la création de profil de publication en quelques étapes simples.

![Nouveau projet avec des ressources Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publier sur Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Améliorations de publication Web

- Améliorer l’expérience utilisateur pour la publication.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET échafaudage

- **Soutien Enum:** Si votre modèle utilise Enums, alors l’échafaudage MVC générera un dropdown pour Enum. Cela utilise les aides Enum dans MVC.
- **Support Bootstrap**: Mise à jour des modèles EditorFor dans MVC Scaffolding afin qu’ils utilisent les classes Bootstrap.
- **Support de paquets**: MVC et Web API Scaffolders ajouteront 5.1 paquets pour MVC et Web API

Les captures d’écran suivantes montrent des modèles d’échafaudage.

- Code modèle :

     ![Code modèle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilez le code modèle, cliquez à droite et sélectionnez **Ajouter**, **Nouvel article échafaudé**.

     ![Ajouter un nouvel article échafaudé](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Choisissez **MVC5 Controller avec vues, en utilisant Entity Framework**:

     ![Ajouter un nouveau contrôleur MVC5 avec vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Ajouter Contrôleur** à l’aide du modèle:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Vérifiez le code généré, par exemple Views/WeekdayModels/Edit.cshtml contient `@Html.EnumDropDownListFor`: ![Voir contenant EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Exécutez la page pour voir la boîte combo enum générée, remarquez que si une valeur peut être nulle, une chaîne vide peut être choisie pour la boîte de combo. Par exemple, la page **Créer** affiche ce qui suit :

    ![Boîte de combo permettant la chaîne vide](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM sortira en avril 2014. Voici les points saillants des notes de sortie, mais s’il vous plaît vérifier les [notes de libération complète](http://docs.nuget.org/docs/release-notes/nuget-2.8) pour plus d’informations sur ces changements.

- **Target Windows Phone 8.1 Applications**: NuGet 2.8.1 prend désormais en charge le ciblage des applications Windows Phone 8.1 en utilisant les surnoms-cadres cibles 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' et 'WPA81'.

- **Résolution de correction pour les dépendances**: Lors de la résolution des dépendances du paquet, NuGet a historiquement mis en œuvre une stratégie de sélection de la version de paquets majeure et mineure la plus basse qui satisfait les dépendances du paquet. Contrairement à la version majeure et mineure, cependant, la version patch a toujours été résolu à la version la plus élevée. Bien que le comportement ait été bien intentionné, il a créé un manque de déterminisme pour installer des paquets avec des dépendances.
- **Commutateur De dépendance**: Bien que NuGet 2.8 modifie le comportement *par défaut* pour résoudre les dépendances, il ajoute également un contrôle plus précis sur le processus de résolution de dépendance via le commutateur -DependencyVersion dans la console de gestionnaire de paquet. Le commutateur permet de résoudre les dépendances à la version la plus basse possible (comportement par défaut), la version la plus élevée possible, ou la version mineure ou patch la plus élevée. Ce commutateur ne fonctionne que pour l’installation-paquet dans la commande de coquille de puissance.
- **Attribut DependencyVersion**: En plus de l’interrupteur -DependencyVersion détaillé ci-dessus, NuGet a également permis à la possibilité de définir un nouvel attribut dans le fichier nuget.config définissant quelle est la valeur par défaut, si le commutateur -DependencyVersion n’est pas spécifié dans une invocation de l’installation-paquet. Cette valeur sera également respectée par le Dialogue NuGet Package Manager pour toute installation d’opérations de colis. Pour définir cette valeur, ajoutez l’attribut ci-dessous à votre fichier nuget.config :

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Aperçu NuGet Operations With -WhatIf:** Certains paquets NuGet peuvent avoir des graphiques de dépendance profonde, et en tant que tel, il peut être utile lors d’une installation, désinstaller, ou mettre à jour l’opération pour d’abord voir ce qui va se passer. NuGet 2.8 ajoute la powerShell standard - et si le commutateur à l’installation-paquet, désinstaller-paquet, et les commandes de mise à jour-paquet pour permettre de visualiser la fermeture complète des paquets auxquels la commande sera appliquée.
- **Forfait déclassement**: Il n’est pas rare d’installer une version préreléase d’un paquet afin d’étudier de nouvelles fonctionnalités et ensuite décider de revenir à la dernière version stable. Avant NuGet 2.8, il s’agissait d’un processus en plusieurs étapes de désinstaller le paquet de préréédition et ses dépendances, puis d’installer la version antérieure. Avec NuGet 2.8, cependant, le paquet de mise à jour va maintenant faire reculer la fermeture totale du paquet (par exemple l’arbre de dépendance du paquet) à la version précédente.
- **Dépendances au développement**: De nombreux types de capacités peuvent être livrés sous forme de paquets NuGet - y compris des outils qui sont utilisés pour optimiser le processus de développement. Ces composants, bien qu’ils puissent jouer un rôle déterminant dans l’élaboration d’un nouveau paquet, ne devraient pas être considérés comme une dépendance du nouveau paquet lorsqu’il sera publié plus tard. NuGet 2.8 permet à un paquet de s’identifier dans le fichier .nuspec comme un développementDépendance. Une fois installées, ces métadonnées seront également ajoutées au fichier packages.config du projet dans lequel le paquet a été installé. Lorsque ce fichier packages.config est analysé plus tard pour les dépendances NuGet pendant le pack nuget.exe, il exclut les dépendances marquées comme dépendances de développement.
- **Fichiers individuels.config pour différentes plateformes**: Lors du développement d’applications pour plusieurs plates-formes cibles, il est courant d’avoir des fichiers de projets différents pour chacun des environnements de construction respectifs. Il est également courant de consommer différents paquets NuGet dans différents fichiers de projet, car les paquets ont différents niveaux de support pour différentes plates-formes. NuGet 2.8 fournit une meilleure prise en charge de ce scénario en créant différents fichiers packages.config pour différents fichiers de projet spécifiques à la plate-forme.
- **Repli à Local Cache**: Bien que les paquets NuGet soient généralement consommés à partir d’une galerie isolée comme la [galerie NuGet](http://www.nuget.org) à l’aide d’une connexion réseau, il existe de nombreux scénarios où le client n’est pas connecté. Sans connexion réseau, le client NuGet n’a pas été en mesure d’installer avec succès des paquets - même lorsque ces paquets étaient déjà sur la machine du client dans le cache NuGet local. NuGet 2.8 ajoute un repli automatique de cache à la console de gestionnaire de paquets.

    La fonction de repli de cache ne nécessite aucun argument de commande spécifique. En outre, le repli de cache fonctionne actuellement seulement dans la console de gestionnaire de paquet - le comportement ne fonctionne pas actuellement dans le dialogue de gestionnaire de paquet.
- **Corrections bug**: L’une des principales corrections de bogues apportées a été l’amélioration des performances de la commande de mise à jour-paquet-reinstall.

    En plus de ces fonctionnalités et le correctif de performance susmentionné, cette version de NuGet comprend également de nombreuses autres corrections de bogues. Le communiqué a été abordé au total de 181 questions. Pour une liste complète des éléments de travail fixés dans NuGet 2.8, veuillez consulter le [Tracker NuGet Issue](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) pour cette version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formulaires web ASP.NET

- Les modèles Web Forms montrent maintenant comment faire la confirmation de compte et la réinitialisation de mot de passe pour ASP.NET identité.
- Le contrôle de source de données d’entité et le fournisseur de données dynamiques pour le cadre d’entité 6. Pour plus de détails, veuillez consulter le blog MSDN suivant : [Dynamic Data provider et EntityDataSource control for Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Améliorations de routage d’attribut](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Support Bootstrap pour les modèles d’éditeur](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Soutien enum dans les points de vue](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Soutien discret pour les attributs MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Soutenir le contexte de «ce» dans l’Ajax discret](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Diverses [corrections de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET API Web 2.1.2

- [Gestion globale des erreurs](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Améliorations de routage d’attribut](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Améliorations de page d’aide](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Soutien IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateur de type média BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Un meilleur support pour les filtres async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analyse de requête pour la bibliothèque de formatage client](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Diverses [corrections de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Pages Web 3.1.2

- Diverses [corrections de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Cadre de l’entité 6.1

Entity Framework a été mis à jour à la version 6.1 pour l’exécution et l’outillage. Entity Framework (EF) 6.1 est une mise à jour mineure du cadre d’entité 6 et comprend un certain nombre de corrections de bogues et de nouvelles fonctionnalités. Pour obtenir des informations détaillées sur EF6.1, y compris des liens vers la documentation pour les nouvelles fonctionnalités, voir [l’historique de la version cadre de l’entité](https://msdn.microsoft.com/data/jj574253). Les nouvelles fonctionnalités de cette version incluent :

- **La consolidation de l’outillage** fournit un moyen cohérent de créer un nouveau modèle EF. Cette fonctionnalité étend l’assistant ADO.NET Entity Data Model pour prendre en charge la création de modèles Code First, y compris l’ingénierie inverse à partir d’une base de données existante. Ces fonctionnalités étaient auparavant disponibles en qualité bêta dans les outils électriques EF.
- **Le traitement des défaillances de transaction** fournit le nouveau [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) qui utilise la capacité nouvellement introduite d’intercepter les opérations de transaction. Le **CommitFailureHandler** permet la récupération automatique des défaillances de connexion tout en engageant une transaction.
- **IndexAttribute** permet de préciser les index en plaçant un attribut sur une propriété (ou propriété) dans votre modèle Code First. Code d’abord créera ensuite un index correspondant dans la base de données.
- **L’API de cartographie publique** donne accès aux informations dont EF dispose sur la façon dont les propriétés et les types sont cartographiés sur les colonnes et les tableaux de la base de données. Dans les versions précédentes, cette API était interne.
- **Possibilité de configurer les intercepteurs via le fichier App/Web.config**(permettant d’ajouter des intercepteurs sans recompliser l’application).
- **DatabaseLogger** est un nouvel intercepteur qui facilite l’enregistrement de toutes les opérations de base de données à un fichier. En combinaison avec la fonctionnalité précédente, cela vous permet d’activer facilement la connexion des opérations de base de données pour une application déployée, sans avoir besoin de recalculer.
- **La détection des changements du modèle de migrations** a été améliorée de sorte que les migrations échafaudées sont plus précises; le processus de détection des changements a également été grandement amélioré.
- **Amélioration du rendement,** y compris la réduction des opérations de base de données lors de l’initialisation, l’optimisation de la comparaison de l’égalité nulle dans les requêtes LINQ, la génération de vision plus rapide (création de modèle) dans plus de scénarios, et une matérialisation plus efficace des entités suivies avec de multiples associations.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identité 2.0.0

- **Authentification à deux facteurs**: ASP.NET Identity prend désormais en charge l’authentification à deux facteurs. L’authentification à deux facteurs fournit une couche supplémentaire de sécurité à vos comptes d’utilisateurs dans le cas où votre mot de passe est compromis. Il existe également une protection pour les attaques de force brute contre les deux codes de facteur.
- **Verrouillage du compte:** Fournit un moyen de verrouiller l’utilisateur si l’utilisateur entre son mot de passe ou les codes à deux facteurs incorrectement. Le nombre de tentatives invalides et la durée de l’lock-out des utilisateurs peuvent être configurés. Un développeur peut désactiver le lock-out de compte pour certains comptes d’utilisateurs s’il en a besoin.
- **Confirmation du compte:** Le système ASP.NET Identity prend désormais en charge la confirmation du compte. Il s’agit d’un scénario assez commun dans la plupart des sites Web aujourd’hui où, lorsque vous vous inscrivez pour un nouveau compte sur le site, vous êtes tenu de confirmer votre e-mail avant de pouvoir faire quoi que ce soit dans le site. La confirmation par e-mail est utile car elle empêche la création de faux comptes. Ceci est extrêmement utile si vous utilisez le courrier électronique comme méthode de communication avec les utilisateurs de votre site Web tels que les sites Forum, les banques, le commerce électronique ou les sites Web sociaux.
- **Réinitialisation du mot de passe :** Password Reset est une fonctionnalité où l’utilisateur peut réinitialiser ses mots de passe s’il a oublié son mot de passe.
- **Timbre de sécurité (Signez partout):** Prend en charge un moyen de régénérer le jeton de sécurité pour l’utilisateur dans les cas où l’utilisateur change son mot de passe ou toute autre information liée à la sécurité, comme la suppression d’une connexion associée (comme Facebook, Google, Compte Microsoft, et ainsi de suite). Ceci est nécessaire pour s’assurer que les jetons générés avec l’ancien mot de passe sont invalidés. Dans le projet d’exemple, si vous modifiez le mot de passe de l’utilisateur, un nouveau jeton est généré pour l’utilisateur et tous les jetons précédents sont invalidés. Cette fonctionnalité fournit une couche supplémentaire de sécurité à votre application puisque lorsque vous changez votre mot de passe, vous serez déconnecté de partout (tous les autres navigateurs) où vous vous êtes connecté à cette application.
- **Faites le type de clé primaire être extensible pour les utilisateurs et les rôles**: dans ASP.NET Identity 1.0, le type de clé principale pour les utilisateurs de table et les rôles était des chaînes. Cela signifie que lorsque le système d’identité ASP.NET a été maintenu dans SQL Server en utilisant Entity Framework, nous utilisions nvarchar. Il y a eu de nombreuses discussions autour de cette implémentation par défaut sur Stack Overflow et basée sur les commentaires entrants. Nous avons fourni un crochet d’extabilité où vous pouvez spécifier quelle devrait être la clé principale de votre tableau des utilisateurs et des rôles. Ce crochet d’extabilité est particulièrement utile si vous migrez votre application et que l’application stockait les UserIds sont des GUIDs ou des ints.
- **Support IQueryable sur les utilisateurs et les rôles**: Ajout d’une prise en charge pour IQueryable sur UsersStore et RolesStore, vous pouvez facilement obtenir la liste des utilisateurs et des rôles.
- **Supprimer l’opération de prise de charge par l’intermédiaire de l’UserManager**
- **Indexation sur UserName**: Dans ASP.NET mise en œuvre du Cadre d’entités d’identité, nous avons ajouté un index unique sur le nom d’utilisateur en utilisant le nouvel IndexAttribute dans EF 6.1.0. Cela permet de s’assurer que les noms d’utilisateur sont toujours uniques et il n’y avait pas d’état de course dans lequel vous pourriez vous retrouver avec des noms d’utilisateur en double.
- **Validateur de mot de passe amélioré :** Le validateur de mot de passe qui a été expédié dans ASP.NET Identity 1.0 était un validateur de mot de passe assez basique qui ne validait que la longueur minimale. Il y a un nouveau validateur de mot de passe qui vous donne plus de contrôle sur la complexité du mot de passe. Veuillez noter que même si vous activez tous les paramètres de ce mot de passe, nous vous encourageons à activer l’authentification à deux facteurs pour les comptes d’utilisateurs.
- **IdentityFactory Middleware/ CreatePerOwinContext**:

    - **User Manager**: Vous pouvez utiliser la mise en œuvre d’Usine pour obtenir une instance d’UtilisateurManager du contexte OWIN. Ce modèle est similaire à ce que nous utilisons pour obtenir AuthenticationManager du contexte OWIN pour SignIn et SignOut. Il s’agit d’un moyen recommandé d’obtenir une instance d’UtilisateurManager par demande pour la demande.
    - **DbContextFactory**: ASP.NET Identity utilise entity Framework pour persister le système d’identité dans SQL Server. Pour ce faire, le système d’identité a une référence à l’applicationDbContext. Le DbContextFactory Middleware renvoie une instance de l’ApplicationDbContext par demande que vous pouvez utiliser dans votre application.
- **ASP.NET’identitaire Échantillons NuGet paquet**: Le paquet Échantillons NuGet peut faciliter l’installation et l’exécution d’échantillons pour ASP.NET Identity et suivre les meilleures pratiques. Il s’agit d’un échantillon ASP.NET application MVC. Veuillez modifier le code en fonction de votre application avant de la déployer en production. L’échantillon doit être installé dans une application ASP.NET vide. Pour plus d’informations sur le paquet, rendez-vous sur le blog suivant: [Annonce RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

Il y avait beaucoup de bugs qui ont été corrigés dans cette version. Veuillez consulter les [notes de sortie pour la version 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) pour plus d’informations détaillées.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET Signalr 2.0.2

Il y avait beaucoup de bugs qui ont été corrigés dans cette version. Veuillez consulter les [notes de sortie pour la version 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) pour plus d’informations détaillées.
