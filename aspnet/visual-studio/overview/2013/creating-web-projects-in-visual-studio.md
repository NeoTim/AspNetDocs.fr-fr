---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Création de projets Web ASP.NET dans Visual Studio 2013 (fr) Microsoft Docs
author: tdykstra
description: Ce sujet explique les options pour créer des projets web ASP.NET dans Visual Studio 2013 avec Update 3 Voici quelques-unes des nouvelles fonctionnalités pour le développement web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676047"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Création de projets web ASP.NET dans Visual Studio 2013

par [Tom Dykstra](https://github.com/tdykstra)

> Ce sujet explique les options pour créer des projets web ASP.NET dans Visual Studio 2013 avec Update 3
> 
> Voici quelques-unes des nouvelles fonctionnalités pour le développement web par rapport aux versions antérieures de Visual Studio:
> 
> - Une simple interface utilisateur pour la création de projets qui offrent [un soutien à plusieurs cadres ASP.NET (Formulaires](#add) Web, MVC et API Web).
> - [ASP.NET Identity](#indauth), un nouveau système d’adhésion ASP.NET qui fonctionne de la même façon dans tous les cadres ASP.NET et fonctionne avec un logiciel d’hébergement Web autre que l’IIS.
> - L’utilisation de [Bootstrap](#bootstrap) pour fournir une conception réactive et des capacités de thème.
> - Nouvelles fonctionnalités pour les formulaires Web qui étaient autrefois offertes uniquement pour MVC, telles que la [création automatique de projets](#testproj) de test et un [modèle de site Intranet](#winauth).
> 
> Pour plus d’informations sur la façon de créer des projets web pour Azure Cloud Services ou Azure Mobile Services, voir [Get Started with Azure Cloud Services et ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) et créer une application [leaderboard avec Azure Mobile Services .NET Backend](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prérequis

Cet article s’applique à [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) avec [Mise à jour 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installé.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projets d’applications Web par rapport aux projets de sites Web

ASP.NET vous donne le choix entre deux types de projets web : *les projets d’applications web* et *les projets de sites Web.* Nous recommandons des projets d’applications Web pour le nouveau développement, et cet article ne s’applique qu’aux projets d’applications Web. Pour de plus amples renseignements, consultez [Web Projets d’applications par rapport aux projets de sites Web en studio visuel](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) sur le site MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Aperçu de la création de projets d’applications Web

Les étapes suivantes montrent comment créer un projet web :

1. Cliquez sur **Nouveau Projet** dans la page **Démarrer** ou dans le menu **Fichier.**
2. Dans le dialogue **new Project,** cliquez sur **Web** dans la vitre gauche et **ASP.NET application Web** dans le volet du milieu.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image1.png)

    Vous pouvez choisir **Cloud** dans la vitre gauche pour créer un [service Cloud Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), ou [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Ce sujet ne couvre pas ces modèles.
3. Dans le volet droit, cliquez sur les **aperçus d’applications Add à la** case à cocher du projet si vous voulez une surveillance de la santé et de l’utilisation pour votre application. Pour plus d’informations, consultez l’article [Analyse des performances dans les applications web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Spécifiez **le nom du**projet, **l’emplacement**, et d’autres options, puis cliquez **sur OK**.

    La boîte de dialogue **Nouveau projet ASP.NET** apparaît.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image2.png)
5. Cliquez sur un modèle.

    ![Sélectionner un modèle](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si vous souhaitez ajouter un support pour des cadres supplémentaires non inclus dans le modèle, cliquez sur la case à cocher appropriée. (Dans l’exemple montré, vous pouvez ajouter MVC et/ou l’API Web à un projet Web Forms.)

    ![Ajouter des cadres](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si vous souhaitez ajouter un projet de test unitaire, cliquez sur **Ajouter les tests unitaires**.

    ![Ajouter des tests unitaires](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si vous voulez une méthode d’authentification différente de ce que le modèle fournit par défaut, cliquez sur **Change Authentication**.

    ![Configurer le bouton d’authentification](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurer le dialogue d’authentification](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Créer une application web ou une machine virtuelle en Azure

Visual Studio comprend des fonctionnalités qui facilitent le travail avec les services Azure pour héberger des applications Web. Par exemple, vous pouvez faire tout ce qui suit à partir de l’IDE Visual Studio:

- Créez et gérez des applications Web ou des machines virtuelles qui rendent votre application disponible sur Internet.
- Afficher les journaux créés par l’application pendant qu’il s’exécute dans le cloud.
- Exécutez en mode débogé à distance pendant que l’application s’exécute dans le nuage.
- Afficher et gérer d’autres services Azure tels que les bases de données SQL.

Vous pouvez [créer un compte Azure](https://www.windowsazure.com/pricing/free-trial/) qui inclut des services de base tels que des applications Web gratuitement, et si vous êtes un abonné MSDN, vous pouvez [activer les avantages](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) qui vous donnent des crédits mensuels vers des services Azure supplémentaires. 

Par défaut, la boîte de dialogue **New ASP.NET Project** vous permet de créer une application web ou une machine virtuelle pour un nouveau projet web. Si vous ne souhaitez pas créer une nouvelle application web ou une nouvelle machine virtuelle, effacez **l’Hôte dans la** case à cocher en nuage.

![Créer des ressources distantes](creating-web-projects-in-visual-studio/_static/image8.png)

La légende de la case à cocher peut être **l’hôte dans le nuage** ou **créer des ressources à distance**, et dans les deux cas, l’effet est le même. Si vous laissez la case à cocher sélectionnée, Visual Studio crée une application web dans Azure App Service par défaut. Vous pouvez utiliser la boîte de dépôt pour changer cela en une **machine virtuelle** si vous préférez. Si vous n’êtes pas déjà connecté à Azure, vous êtes invité pour les informations d’identification Azure. Une fois que vous vous êtes inscrit, une boîte de dialogue vous permet de configurer les ressources que Visual Studio créera pour votre projet. L’illustration suivante montre le dialogue pour une application web; différentes options apparaissent si vous choisissez de créer une machine virtuelle.

![Configurer les paramètres de l’application Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Pour plus d’informations sur la façon d’utiliser ce processus pour créer des ressources Azure, voir [Get Started with Azure et ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) et créer une machine virtuelle pour un site web avec Visual [Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Le reste de cet article fournit plus d’informations sur les modèles disponibles et leurs options. L’article introduit également Bootstrap, le cadre de mise en page et de thème utilisé dans les modèles.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modèles de projets Web Visual Studio 2013

Visual Studio utilise des modèles pour créer des projets Web. Un modèle de projet peut créer des fichiers et des dossiers dans le nouveau projet, installer des paquets NuGet et fournir un code d’échantillon pour une application de travail rudimentaire. Les modèles implémentent les dernières normes Web et visent à démontrer les meilleures pratiques pour l’utilisation des technologies ASP.NET ainsi que vous donner un bon départ sur la création de votre propre application.

Visual Studio 2013 fournit les choix suivants pour les modèles de projets Web pour les projets qui ciblent .NET 4.5 ou les versions ultérieures du cadre .NET:

- [Modèle vide](#empty)
- [Modèle de formulaires Web](#wf)
- [Modèle MVC](#mvc)
- [Modèle Web API](#webapi)
- [Modèle d’application à une page unique](#spa)
- [Modèle de service mobile Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modèles Visual Studio 2012](#vs2012)

Vous pouvez également installer une extension Visual Studio qui fournit un [modèle Facebook](#facebook).

Pour plus d’informations sur la façon de créer des projets qui ciblent .NET 4, voir [Visual Studio 2012 Templates](#vs2012) plus tard dans ce sujet.

Pour plus d’informations sur la façon de créer des applications ASP.NET pour les clients mobiles, voir [Support mobile en ASP.NET](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modèle vide

Le modèle Empty fournit les dossiers et les fichiers stricts minimums pour une application web ASP.NET, comme un fichier de projet (*.csproj* ou .* vbproj*) et un fichier *Web.config.* Vous pouvez ajouter un support pour les formulaires Web, MVC et/ou l’API Web en utilisant les cases à cocher sous **les dossiers Add et les références de base pour : l’étiquette.**

Pour le modèle Empty, aucune option d’authentification n’est disponible. La fonctionnalité d’authentification est implémentée dans les applications d’échantillons, et le modèle Empty ne crée pas d’application d’échantillon.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modèle de formulaires Web

Le cadre Web Forms fournit les fonctionnalités suivantes qui vous permettent de construire rapidement des sites Web riches en interfaces utilisateur et en fonctionnalités d’accès aux données :

- Un designer WYSIWYG dans Visual Studio.
- Contrôles de serveur qui rendent HTML et que vous pouvez personnaliser en définissant les propriétés et les styles.
- Un riche assortiment de contrôles pour l’accès aux données et l’affichage des données.
- Un modèle d’événement qui expose les événements que vous pouvez programmer comme vous programmez une application client telle que WPF.
- Préservation automatique de l’état (données) entre les demandes HTTP.

En général, la création d’une application Web Forms nécessite moins d’efforts de programmation que la création de la même application en utilisant le cadre MVC ASP.NET. Cependant, Web Forms n’est pas seulement pour le développement rapide d’applications. Il existe de nombreuses applications et cadres commerciaux complexes construits au-dessus des formulaires Web.

Étant donné qu’une page Web Forms et les contrôles de la page génèrent automatiquement une grande partie de la majoration qui est envoyée au navigateur, vous n’avez pas le genre de contrôle à grain fin sur le HTML que ASP.NET MVC offre. Le modèle déclaratif pour configurer les pages et les contrôles minimise la quantité de code que vous devez écrire, mais cache une partie du comportement de HTML et HTTP. Par exemple, il n’est pas toujours possible de spécifier exactement quelle marge pourrait être générée par un contrôle.

Le cadre Web Forms ne se prête pas aussi facilement qu’ASP.NET MVC à des pratiques de développement basées sur des modèles telles que [le développement axé sur les tests,](http://en.wikipedia.org/wiki/Test-driven_development)la séparation des [préoccupations,](http://en.wikipedia.org/wiki/Separation_of_concerns) [l’inversion du contrôle et l’injection de](http://en.wikipedia.org/wiki/Inversion_of_control) [dépendance.](http://en.wikipedia.org/wiki/Dependency_injection) Si vous voulez écrire du code pris en compte de cette façon, vous pouvez; ce n’est tout simplement pas aussi automatique que dans le cadre ASP.NET MVC. Le projet [ASP.NET Web Forms MVP](http://webformsmvp.com/) montre une approche qui facilite la séparation des préoccupations et de la testabilité tout en maintenant le développement rapide que Web Forms a été conçu pour offrir. Microsoft SharePoint est construit sur Web Forms MVP.

Le modèle Web Forms crée un exemple d’application Web Forms qui utilise [Bootstrap](#bootstrap) pour fournir une conception réactive et des fonctionnalités de thème. L’illustration suivante montre la page d’accueil.

![Page d’accueil de l’application Web Forms](creating-web-projects-in-visual-studio/_static/image10.png)

Pour plus d’informations sur les formulaires Web, voir [ASP.NET formulaires Web](https://asp.net/web-forms). Pour plus d’informations sur ce que le modèle Web Forms fait pour vous, voir [Construire une application de base Web Forms à l’aide de Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modèle MVC

ASP.NET MVC a été conçu pour faciliter les pratiques de développement basées sur les modèles telles que [le développement axé sur les tests,](http://en.wikipedia.org/wiki/Test-driven_development)la séparation des [préoccupations,](http://en.wikipedia.org/wiki/Separation_of_concerns) [l’inversion du contrôle](http://en.wikipedia.org/wiki/Inversion_of_control)et [l’injection](http://en.wikipedia.org/wiki/Dependency_injection)de dépendance . Le cadre encourage la séparation de la couche logique d’entreprise d’une application web de sa couche de présentation. En divisant l’application en modèles (M), vues (V) et contrôleurs (C), ASP.NET MVC peut faciliter la gestion de la complexité dans les applications plus grandes.

Avec ASP.NET MVC, vous travaillez plus directement avec HTML et HTTP que dans Web Forms. Par exemple, les formulaires Web peuvent automatiquement préserver l’état entre les demandes HTTP, mais vous devez coder explicitement dans MVC. L’avantage du modèle MVC est qu’il vous permet de prendre un contrôle complet sur exactement ce que votre application fait et comment elle se comporte dans l’environnement web. L’inconvénient est que vous devez écrire plus de code.

MVC a été conçu pour être extensible, offrant aux développeurs d’énergie la possibilité de personnaliser le cadre pour leurs besoins d’application. En outre, le ASP.NET code source MVC est disponible sous licence OSI.

Le modèle MVC crée un exemple d’application MVC 5 qui utilise [Bootstrap](#bootstrap) pour fournir une conception réactive et des fonctionnalités de thème. L’illustration suivante montre la page d’accueil.

![Demande d’échantillon MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Pour plus d’informations sur MVC, voir [ASP.NET MVC](https://asp.net/mvc). Pour plus d’informations sur la façon de sélectionner le modèle MVC 4, voir [les modèles Visual Studio 2012](#vs2012) plus tard dans cet article.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modèle d’API Web

Le modèle Web API crée un exemple de service Web basé sur l’API Web, y compris les pages d’aide API basées sur MVC.

ASP.NET Web API est une infrastructure qui facilite le développement de services HTTP disponibles sur un large éventail de clients, tels que des navigateurs et des appareils mobiles. ASP.NET’API Web est une plate-forme idéale pour la construction de services RESTful sur le cadre .NET.

Le modèle Web API crée un exemple de service Web. Les illustrations suivantes montrent des exemples de pages d’aide.

![Page d’aide Web API](creating-web-projects-in-visual-studio/_static/image12.png)

![Page d’aide Web API pour GET API](creating-web-projects-in-visual-studio/_static/image13.png)

Pour plus d’informations sur l’API Web, voir [ASP.NET’API Web](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Single Page Application Template (Modèle d’application à page unique)

Le modèle d’application à une page unique (SPA) crée une application d’échantillon qui utilise JavaScript, HTML 5 et [KnockoutJS](http://knockoutjs.com/) sur le client, et ASP.NET’API Web sur le serveur.

La seule option d’authentification pour le modèle SPA est [les comptes utilisateur individuels](#indauth).

L’illustration suivante montre l’état initial de l’application de l’échantillon que le modèle SPA construit.

![Application d’échantillon de SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Pour plus d’informations sur la façon de créer une application en utilisant le modèle SPA, voir [Web API - External Authentication Services](../../../web-api/overview/security/external-authentication-services.md).

Pour plus d’informations sur ASP.NET applications à une page unique, et sur les modèles SPA supplémentaires qui utilisent des cadres JavaScript autres que KnockoutJS, voir les ressources suivantes:

- [ASP.NET application à une page unique](../../../single-page-application/index.md).
- [Comprendre les caractéristiques de sécurité dans le modèle SPA pour VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applications à une page : Construisez des applications Web modernes et réactives avec des ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modèle Facebook

Vous pouvez installer une [extension Visual Studio qui fournit un modèle Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ce modèle crée un exemple d’application qui est conçu pour s’exécuter à l’intérieur du site Web Facebook. Il est basé sur ASP.NET MVC et utilise l’API Web pour mettre à jour les fonctionnalités en temps réel.

Aucune option d’authentification n’est disponible pour le modèle Facebook car les applications Facebook s’exécutent dans le site Facebook et s’appuient sur l’authentification de Facebook.

Pour plus d’informations sur ASP.NET applications Facebook, voir [Mise à jour de l’API Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modèles Visual Studio 2012

Le dialogue de création de projet web Visual Studio 2013 n’a pas accès à certains modèles qui étaient disponibles dans Visual Studio 2012. Si vous souhaitez utiliser l’un de ces modèles, vous pouvez cliquer sur le nœud Visual Studio 2012 dans la vitre gauche de la boîte de dialogue Visual Studio New Project.

![Modèles Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Le nœud **Visual Studio 2012** vous permet de choisir les modèles Web suivants auxquels vous n’avez pas accès dans la liste par défaut des modèles de Visual Studio 2013 :

- Application Web ASP.NET MVC 4
- Application Web ASP.NET Dynamic Data Entities
- ASP.NET CONTRÔLE des serveurs AJAX
- ASP.NET’extension de contrôle du serveur AJAX
- contrôle du serveur ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap dans les modèles de projets web Visual Studio 2013

Les modèles de projet Visual Studio 2013 utilisent [Bootstrap](http://getbootstrap.com/), un cadre de mise en page et de thème créé par Twitter. Bootstrap utilise CSS3 pour fournir une conception réactive, ce qui signifie que les mises en page peuvent s’adapter dynamiquement à différentes tailles de fenêtre de navigateur. Par exemple, dans une fenêtre large du navigateur, la page d’accueil créée par le modèle Web Forms ressemble à l’illustration suivante :

![Page d’accueil de l’application Web Forms](creating-web-projects-in-visual-studio/_static/image16.png)

Rendre la fenêtre plus étroite, et les colonnes disposées horizontalement se déplacent dans l’arrangement vertical :

![Arrangement de colonne verticale de Bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Réduisez un peu plus la fenêtre, et le menu horizontal du haut se transforme en une icône que vous pouvez cliquer pour vous étendre dans un menu orienté verticalement :

![Icône de menu Bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu vertical Bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

Vous pouvez également utiliser la fonction de thème de Bootstrap pour effectuer facilement un changement dans l’apparence et la sensation de l’application. Par exemple, vous pouvez faire les étapes suivantes pour changer le thème.

1. Dans votre navigateur, [http://Bootswatch.com](http://Bootswatch.com)allez à , choisissez un thème, puis cliquez sur **Télécharger**. (Cela télécharge *bootstrap.min.css* par défaut; si vous voulez examiner le code CSS, obtenez *bootstrap.css* au lieu de la version minifiée.)
2. Copiez le contenu du fichier CSS téléchargé.
3. Dans Visual Studio, créez un nouveau fichier **Style Sheet** nommé *bootstrap-theme.css* dans le dossier *Content* et collez-y le code CSS téléchargé.
4. Ouvrez *App\_Start/Bundle.config* et *changez bootstrap.css* à *bootstrap-theme.css*.

Exécutez le projet à nouveau, et l’application a un nouveau look. L’illustration suivante montre l’effet du thème Amelia :

![Bootstrap Amelia thème](creating-web-projects-in-visual-studio/_static/image20.png)

De nombreux thèmes Bootstrap sont disponibles, à la fois des versions gratuites et premium. Bootstrap offre également une grande variété de composants d’interface utilisateur, tels que [des baisses, des](http://twitter.github.io/bootstrap/components.html#dropdowns) [groupes de boutons](http://twitter.github.io/bootstrap/components.html#buttonGroups)et [des icônes.](http://twitter.github.io/bootstrap/base-css.html#images) Pour plus d’informations sur Bootstrap, voir [le site Bootstrap](http://twitter.github.io/bootstrap/).

Si vous utilisez le concepteur Web Forms dans Visual Studio, notez que le concepteur ne prend pas en charge CSS3, de sorte qu’il ne montre pas avec précision tous les effets des thèmes Bootstrap ou des changements de mise en page sensibles. Toutefois, les pages Web Forms s’affichent correctement lorsqu’elles sont visualisées avec un navigateur.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Ajout d’un soutien pour des cadres supplémentaires

Lorsque vous sélectionnez un modèle, la case à cocher pour le cadre (s) utilisé par le modèle est automatiquement sélectionnée. Par exemple, si vous sélectionnez le modèle **Web Forms,** la case à cocher **des formulaires Web** est sélectionnée et vous ne pouvez pas l’effacer.

![Sélectionner un modèle](creating-web-projects-in-visual-studio/_static/image21.png)

![Ajouter des cadres](creating-web-projects-in-visual-studio/_static/image22.png)

Vous pouvez sélectionner la case à cocher pour un cadre qui n’est pas inclus dans le modèle afin d’ajouter un support pour ce cadre lorsque le projet est créé. Par exemple, pour activer l’utilisation de pages Web Forms *.aspx* lorsque vous avez sélectionné le modèle MVC, sélectionnez la case à cocher **des formulaires Web.** Ou pour activer MVC lorsque vous utilisez le modèle Web Forms, cliquez sur la case à cocher **MVC.** L’ajout d’un cadre permet un soutien à l’heure de conception ainsi qu’au temps d’exécution. Par exemple, si vous ajoutez le support MVC à un projet Web Forms, vous serez en mesure d’échafauder les contrôleurs et les vues.

Si vous combinez web Forms et MVC dans un projet et activez [des URL amicales](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) dans les formulaires Web, il peut y avoir des problèmes de routage inattendus lorsqu’une URL a plusieurs cibles possibles. Les itinéraires qui sont définis en premier auront préséance. Par exemple, si `Home` vous avez un contrôleur et une `http://contoso.com/home` page *Home.aspx,* l’URL `EnableFriendlyUrls` ira à `MapRoute` *Home.aspx* si vous appelez la méthode avant d’appeler la `MapRoute` `EnableFriendlyUrls`méthode en *RouteConfig.cs*, ou la même URL ira à la vue par défaut pour votre `Home` contrôleur si vous appelez avant .

L’ajout d’un cadre n’ajoute aucune fonctionnalité d’application d’échantillon. Par exemple, si vous ajoutez le support Web Forms lorsque vous avez sélectionné le modèle MVC, aucun fichier de page d’accueil *Par défaut.aspx* n’est créé. Seuls les dossiers, les fichiers et les références requis pour prendre en charge le cadre sont ajoutés. Pour cette raison, l’ajout de cadres ne change pas les options d’authentification, qui sont implémentées par code dans les applications d’échantillons créées par les modèles. Par exemple, si vous sélectionnez le modèle vide et ajoutez des formulaires Web ou un support MVC, le bouton **Configurer l’authentification** sera toujours désactivé.

Les sections suivantes décrivent brièvement l’effet de chaque case à cocher.

### <a name="add-web-forms-support"></a>Ajouter le support des formulaires Web

Crée des dossiers de données et *de modèles* *\_d’applications* vides et un fichier *Global.asax.* Ceux-ci sont déjà créés par tous les modèles autres que le modèle vide, de sorte que la sélection de la case à cocher Web Formulaires ne fait aucune différence pour d’autres modèles.

Le modèle Web Forms permet aux URL amicales par défaut, mais lorsque vous ajoutez le support Web Forms à d’autres modèles en sélectionnant la case à cocher Web Les URL amicales ne sont pas automatiquement activées.

### <a name="add-mvc-support"></a>Ajouter le support MVC

Installe des forfaits MVC, Razor et WebPages NuGet, crée des *données d’applications\_* vides, *des contrôleurs,* des *modèles*et des dossiers *Vues,* crée le dossier *App\_Start* avec *RouteConfig.cs* fichier et crée le fichier *Global.asax.*

### <a name="add-web-api-support"></a>Ajouter le support Web API

Installe des paquets WebApi et Newtonsoft.Json NuGet, crée des *données d’applications\_* vides, des contrôleurs et des *dossiers* *De modèles,* crée le dossier App *\_Start* avec *WebApiConfig.cs* fichier et crée le fichier *Global.asax.*

<a id="auth"></a>
## <a name="authentication-methods"></a>Méthodes d’authentification

Visual Studio 2013 offre plusieurs options d’authentification pour les modèles Web Forms, MVC et Web API :

- [Pas d’authentification](#noauth)
- [Comptes d’utilisateurs individuels](#indauth) (ASP.NET identité, anciennement connu sous le nom ASP.NET adhésion)
- [Comptes organisationnels](#orgauth) (Windows Server Active Directory ou Azure Active Directory)
- [Authentification windows](#winauth) (Intranet)

![Configurer le dialogue d’authentification](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Aucune authentification

Si vous sélectionnez **No Authentication**, l’application d’échantillon ne contiendra pas de pages Web pour vous connecter, aucune interface utilisateur indiquant qui est connecté, aucune catégorie d’entité pour une base de données d’adhésion, et aucune chaîne de connexion pour une base de données d’adhésion.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Comptes d'utilisateur individuels

Si vous sélectionnez **des comptes utilisateur individuels,** l’application d’échantillon sera configurée pour utiliser ASP.NET Identity (anciennement connue sous le nom d’adhésion ASP.NET) pour l’authentification de l’utilisateur. ASP.NET Identity permet à un utilisateur d’enregistrer un compte, en créant un nom d’utilisateur et un mot de passe sur le site ou en vous connectant à des fournisseurs sociaux tels que Facebook, Google, Microsoft Account ou Twitter. Le magasin de données par défaut pour les profils d’utilisateurs dans ASP.NET Identity est une base de données SQL Server LocalDB, que vous pouvez déployer sur SQL Server ou Azure SQL Database pour le site de production.

Dans Visual Studio 2013, ces fonctionnalités sont les mêmes que dans Visual Studio 2012, mais le code sous-jacent pour le système d’adhésion ASP.NET a été réécrit. Les avantages de la nouvelle base de code comprennent les éléments suivants :

- Le nouveau système d’adhésion est basé sur [OWIN](http://owin.org/) plutôt que sur le module ASP.NET d’authentification des formulaires. Cela signifie que vous pouvez utiliser le même mécanisme d’authentification, que vous utilisiez des formulaires Web ou MVC dans IIS, ou que vous auto-hébergez l’API Web ou SignalR.
- La nouvelle base de données d’adhésion est gérée par Entity Framework Code First, et toutes les tables sont représentées par des classes d’entités que vous pouvez modifier. Cela signifie que vous pouvez facilement personnaliser le schéma de base de données et l’interface utilisateur Web liée au profil pour répondre à vos propres besoins, et vous pouvez facilement déployer vos mises à jour à l’aide de Code First Migrations.

Le nouveau système d’adhésion est implémenté automatiquement dans les nouveaux modèles, et il peut être mis en œuvre manuellement dans tout projet qui cible .NET 4.5 ou plus tard.

ASP.NET Identity est un bon choix si vous créez un site Internet qui est principalement pour les clients externes. Si votre organisation utilise Active Directory ou Office 365 et que vous souhaitez créer un projet qui permet une connexion unique pour les employés et les partenaires d’affaires, **l’option Comptes organisationnels** pourrait être un meilleur choix.

Pour plus d’informations sur l’option Comptes utilisateur individuels, voir les ressources suivantes :

- [www.asp.net/identity](../../../identity/index.md). Documentation sur ASP.NET identité sur le site Web ASP.NET.
- [Créez une application MVC 5 ASP.NET avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Montre également comment personnaliser les données de profil utilisateur.
- [API Web - Services d’authentification externe](../../../web-api/overview/security/external-authentication-services.md)
- [Ajout de connexions externes à votre application ASP.NET dans Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Comptes professionnels

Si vous sélectionnez **des comptes organisationnels,** l’application d’échantillon sera configurée pour utiliser Windows Identity Foundation (WIF) pour l’authentification basée sur les comptes d’utilisateurs dans Azure Active Directory (Azure AD, qui comprend Office 365) ou Windows Server Active Directory. Pour plus d’informations, voir [options d’authentification du compte organisationnel](#orgauthoptions) plus tard dans ce sujet.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Authentification Windows

Si vous sélectionnez **l’authentification de Windows,** l’application d’échantillon sera configurée pour utiliser le module Windows Authentication IIS pour l’authentification. L’application affichera le domaine et l’identifiant utilisateur de l’annuaire actif ou du compte machine local qui est connecté à Windows mais n’inclura pas l’enregistrement de l’utilisateur ou l’interface utilisateur de connexion. Cette option est destinée aux sites Web Intranet.

Alternativement, vous pouvez créer un site Intranet qui utilise l’authentification AD en choisissant [l’option Sur place sous comptes organisationnels](#orgauthonprem). L’option On-Premises utilise Windows Identity Foundation (WIF) au lieu du module d’authentification Windows. Certaines étapes supplémentaires sont nécessaires pour configurer l’option On-Premises, mais WIF permet des fonctionnalités qui ne sont pas disponibles avec le module d’authentification Windows. Par exemple, avec WIF, vous pouvez configurer l’accès à l’application dans active Répertoire et les données d’annuaire de requête.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Options d’authentification de compte organisationnel

Le dialogue **Configure Authentication** vous offre plusieurs options pour Azure Active Directory (Azure AD, qui comprend Office 365) ou Windows Server Active Directory (AD) authentification de compte:

- [Cloud - Organisation unique](#orgauthsingle) (Azure AD, ou AD en utilisant l’intégration des annuaires avec Azure AD)
- [Cloud - Multi Organisation](#orgauthmulti) (Azure AD, ou AD en utilisant l’intégration des annuaires avec Azure AD)
- [Sur place](#orgauthonprem) (AD)

Si vous souhaitez essayer l’une des options Azure AD mais n’avez pas encore de compte, [cliquez ici pour vous inscrire à un compte Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si vous choisissez l’une des options Azure AD, votre projet nécessite une base de données et vous devez vous connecter à un compte d’administrateur mondial pour votre locataire Azure AD. Entrez le nom et le mot admin@contoso.onmicrosoft.comde passe d’un compte organisationnel (par exemple) qui a des autorisations administratives pour votre locataire Azure AD.
> 
> **N’entrez pas les informations d’identification contoso@hotmail.comd’un compte Microsoft (par exemple) dans la boîte de dialogue de connexion.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - Authentification d’une organisation unique

![Authentification d’une organisation unique](creating-web-projects-in-visual-studio/_static/image24.png)

Choisissez cette option si vous souhaitez activer l’authentification des comptes d’utilisateurs définis dans un [locataire](https://technet.microsoft.com/library/jj573650.aspx)Azure AD . Par exemple, le site est contoso.com et il sera mis à la disposition des employés de la société Contoso qui sont dans le contoso.onmicrosoft.com locataire. Vous ne pourrez pas configurer Azure AD pour permettre aux utilisateurs d’autres locataires d’accéder à l’application.

#### <a name="domain"></a>Domain

Entrez le domaine Azure AD dans lequel vous souhaitez `contoso.onmicrosoft.com`configurer l’application, par exemple : . Si vous avez un [domaine personnalisé](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), comme `contoso.com` au lieu de `contoso.onmicrosoft.com`, vous pouvez entrer que ici.

#### <a name="access-level"></a>Niveau d’accès

Si l’application doit interroger ou mettre à jour les informations d’annuaire en utilisant l’API graphique, choisissez **Un Signe-On, Lire les données d’annuaire** ou **de signe unique, lire et écrire des données d’annuaire**. Sinon, choisissez **Single Sign-On**. Pour plus d’informations, consultez [les niveaux d’accès aux applications](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) et en utilisant [l’API graphique pour interroger Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI d’ID d’application

Par défaut, le modèle crée une application ID URI pour vous en apenvoyant le nom du projet au domaine Azure AD. Par exemple, si le `Example` nom du `contoso.onmicrosoft.com`projet est et `https://contoso.onmicrosoft.com/Example`le domaine est , l’application ID URI devient . Si vous souhaitez spécifier manuellement l’id URI de l’application, élargissez la section **Plus d’options** et entrez l’ID URI d’application dans la boîte de texte. L’application ID URI `https://`doit commencer par .

Par défaut, si une application déjà provisionnée dans Azure AD a la même application ID URI que celle que Visual Studio utilise pour le projet, le projet sera connecté à l’application existante au lieu d’en fournir une nouvelle. Si vous souhaitez qu’une nouvelle demande soit provisionnée dans ce cas, effacez la **surmortalité de l’entrée de l’application si l’on a déjà la même carte d’identité.**

Si la case à cocher **Overwrite** est effacée, et Visual Studio trouve une application existante avec la même application ID URI, il crée une nouvelle URI en appending un numéro à l’URI qu’il allait utiliser. Par exemple, supposons `Example`que le nom du projet est , vous laissez la boîte de texte vide, vous effacer `https://contoso.onmicrosoft.com/Example`la case à cocher **Overwrite,** et le locataire Azure AD a déjà une application avec l’URI . Dans ce cas, une nouvelle demande sera provisionnée `https://contoso.onmicrosoft.com/Example_20130619330903`avec une demande ID URI comme .

#### <a name="provisioning-the-application-in-azure-ad"></a>Provision de l’application dans Azure AD

Afin de fournir l’application dans Azure AD ou de connecter le projet à une application existante, Visual Studio a besoin des informations d’identification d’un administrateur mondial pour le domaine. Lorsque vous cliquez **sur OK** dans la boîte de dialogue **Configure Authentication,** vous êtes invité pour le nom d’utilisateur et le mot de passe d’un administrateur global pour le domaine que vous avez spécifié. Plus tard, lorsque vous cliquez sur **Create Project** dans le dialogue new **ASP.NET Project,** Visual Studio fournit l’application dans Azure AD. Notez que dans le cadre de ce processus Visual Studio intègre des valeurs secrètes client dans le fichier Web.config qui expirent un an après la création.

Pour plus d’informations sur la façon de créer des applications qui utilisent l’authentification **Cloud - Organisation Unique,** voir les ressources suivantes :

- [Authentification Azure](../2012/windows-azure-authentication.md)
- [Ajout de l’authentification à votre application web à l’aide d’Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Développement d’applications ASP.NET avec Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Secure ASP.NET’API Web avec les composants Azure AD et Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Les tutoriels n’ont pas encore été mis à jour pour Visual Studio 2013; certains de ce que les tutoriels vous dirigent à faire manuellement est automatisé dans Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - Authentification multi-organisations

![Authentification d’organisation multiple](creating-web-projects-in-visual-studio/_static/image25.png)

Choisissez cette option si vous souhaitez activer l’authentification des comptes d’utilisateurs définis dans plusieurs [locataires](https://technet.microsoft.com/library/jj573650.aspx)Azure AD . Par exemple, le site est contoso.com et il sera mis à la disposition des employés de la société Contoso qui sont dans le locataire contoso.onmicrosoft.com, et les employés de la société Fabrikam qui sont dans le fabrikam.onmicrosoft.com locataire.

Les paramètres que vous entrez et l’étape de provisionnement d’application sont similaires à [l’authentification d’une seule organisation](#orgauthsingle).

Pour plus d’informations sur la façon de créer des applications qui utilisent l’authentification **Cloud - Multi Organization,** voir les ressources suivantes :

- [Intégration d’applications Web faciles avec Azure &amp; Active Directory, ASP.NET Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) sur le blog Active Directory Team.
- [Développer des applications Web multi-locataires avec tutoriel Azure AD.](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) Le tutoriel n’a pas encore été mis à jour pour Visual Studio 2013; une partie de ce que le tutoriel vous ordonne de faire manuellement est automatisé dans Visual Studio 2013.
- [Vous devez vous inscrire avec vos propres organisations multiples ASP.NET’application avant de pouvoir vous connecter](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci qui explique comment résoudre un problème commun que les gens rencontrent lors de la création d’un projet qui utilise l’authentification multi-organisation.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Authentification organisationnelle sur place

![Authentification organisationnelle sur place](creating-web-projects-in-visual-studio/_static/image26.png)

Choisissez cette option si vous souhaitez activer l’authentification des comptes d’utilisateurs définis dans Windows Server Active Directory (AD), et vous ne souhaitez pas utiliser Azure AD. Vous pouvez utiliser cette option pour créer un site Intranet ou un site Internet. Pour un site Internet, utilisez Active Directory Federation Services (ADFS) pour donner accès à la DA. Pour plus d’informations, voir [Utilisez l’option d’authentification organisationnelle sur place (ADFS) avec ASP.NET dans Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Pour un site Intranet, en tant qu’alternative, vous pouvez choisir [l’authentification Windows](#winauth) au lieu de cette option. Pour l’option d’authentification Windows, vous n’avez pas à fournir une URL de document de métadonnées. Toutefois, Windows Authentication ne vous donne pas la possibilité de contrôler l’accès à l’application dans Active Directory ou de demander des données d’annuaire.

#### <a name="on-premises-authority"></a>Autorité sur place

Entrez une URL qui indique le document de métadonnées. Le document de métadonnées contient les coordonnées de l’autorité. L’application utilisera ces coordonnées pour générer le flux de connexion Web.

#### <a name="application-id-uri"></a>URI d’ID d’application

Fournir une URI unique que AD peut utiliser pour identifier cette application, ou laisser vide pour laisser Visual Studio en créer une.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Étapes suivantes

Ce document a fourni une aide de base pour la création d’un nouveau projet web ASP.NET dans Visual Studio 2013. Pour plus d’informations sur l’utilisation [https://www.asp.net/visual-studio/](../../index.md)de Visual Studio pour le développement web, voir .
