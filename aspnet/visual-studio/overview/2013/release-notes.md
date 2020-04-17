---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET et web Tools pour Visual Studio 2013 Notes de sortie (fr) Microsoft Docs
author: rick-anderson
description: Ce document décrit la sortie de ASP.NET et d’outils Web pour Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 4494b4acdd30384e1def89b7fff9bad30933e38e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543494"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools pour Visual Studio 2013 - Notes de publication

par [Microsoft](https://github.com/microsoft)

> Ce document décrit la sortie de ASP.NET et d’outils Web pour Visual Studio 2013.

## <a name="contents"></a>Contents

- [Notes d’installation](#TOC1)
- [Documentation](#TOC2)
- [Exigences logicielles](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités dans ASP.NET et outils Web pour Visual Studio 2013

- [Un ASP.NET](#TOC6)
- [Nouvelle expérience de projet Web](#newproj)
- [ASP.NET échafaudage](#scaffold)
- [Lien du navigateur](#browser-link)
- [Améliorations de l’éditeur Web Visual Studio](#web-editor)
- [Azure App Service Web Apps Support in Visual Studio](#waws)
- [Améliorations de publication Web](#publish)
- [NuGet 2.7](#nuget)
- [Formulaires web ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [API Web ASP.NET 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Composants Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET App Suspend](#TOC15)
- [Problèmes connus et changements de rupture](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notes d'installation

ASP.NET et web Tools pour Visual Studio 2013 sont regroupés dans l’installateur principal et peuvent être téléchargés [ici](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentation

Des tutoriels et d’autres informations sur ASP.NET et outils Web pour Visual Studio 2013 sont disponibles sur le [site Web ASP.NET.](https://www.asp.net/)

<a id="TOC4"></a>
## <a name="software-requirements"></a>Configuration logicielle

ASP.NET et Web Tools nécessite Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités dans ASP.NET et outils Web pour Visual Studio 2013

Les sections suivantes décrivent les caractéristiques qui ont été introduites dans la version.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Un ASP.NET

Avec la sortie de Visual Studio 2013, nous avons fait un pas vers l’unification de l’expérience de l’utilisation de technologies ASP.NET, afin que vous puissiez facilement mélanger et assortir ceux que vous voulez. Par exemple, vous pouvez démarrer un projet à l’aide de MVC et ajouter facilement des pages Web Forms au projet plus tard, ou envoyer des API Web dans le cadre d’un projet Web Forms. Une ASP.NET est tout au sujet de rendre plus facile pour vous en tant que développeur de faire les choses que vous aimez dans ASP.NET. Quelle que soit la technologie que vous choisissez, vous pouvez avoir confiance que vous construisez sur le cadre sous-jacent de confiance de One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nouvelle expérience de projet Web

Nous avons amélioré l’expérience de la création de nouveaux projets web dans Visual Studio 2013. Dans le dialogue **New ASP.NET Web Project,** vous pouvez sélectionner le type de projet que vous souhaitez, configurer toute combinaison de technologies (formulaires Web, MVC, API Web), configurer des options d’authentification et ajouter un projet de test unitaire.

![Nouveau projet de ASP.NET](release-notes/_static/image1.png)

Le nouveau dialogue vous permet de modifier les options d’authentification par défaut pour de nombreux modèles. Par exemple, lorsque vous créez un projet ASP.NET Web Forms, vous pouvez sélectionner l’une des options suivantes :

- Aucune authentification
- Comptes d’utilisateurs individuels (ASP.NET adhésion ou fournisseur social se connecter)
- Comptes organisationnels (Annuaire actif dans une application Internet)
- Authentification Windows (Annuaire actif dans une application intranet)

![Options d’authentification](release-notes/_static/image2.png)

Pour plus d’informations sur le nouveau processus de création de projets web, voir [Créer ASP.NET projets Web dans Visual Studio 2013](creating-web-projects-in-visual-studio.md). Pour plus d’informations sur les nouvelles options d’authentification, voir [ASP.NET Identité](#TOC8) plus tard dans ce document.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET échafaudage

ASP.NET Scaffolding est un cadre de génération de code pour les applications Web ASP.NET. Il est facile d’ajouter du code de plaque chauffante à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, les échafaudages se limitaient à ASP.NET projets MVC. Avec Visual Studio 2013, vous pouvez maintenant utiliser des échafaudages pour n’importe quel projet ASP.NET, y compris web Forms. Visual Studio 2013 ne prend pas actuellement en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser des échafaudages avec des formulaires Web en ajoutant des dépendances MVC au projet. La prise en charge de la génération de pages pour les formulaires Web sera ajoutée dans une prochaine mise à jour.

Lors de l’utilisation d’échafaudages, nous nous assurons que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous commencez par un projet ASP.NET Web Forms, puis utilisez des échafaudages pour ajouter un contrôleur d’API Web, les paquets et références NuGet requis sont ajoutés automatiquement à votre projet.

Pour ajouter des échafaudages MVC à un projet Web Forms, ajoutez un **nouvel article échafaudé** et sélectionnez **les dépendances MVC 5** dans la fenêtre de dialogue. Il existe deux options pour l’échafaudage MVC; Minimal et complet. Si vous sélectionnez Minimal, seuls les paquets NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option Complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.

Le support pour les contrôleurs async d’échafaudage utilise les nouvelles fonctionnalités async de Entity Framework 6.

Pour plus d’informations et de tutoriels, voir [ASP.NET Aperçu de l’échafaudage](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Lien de navigateur - Canal SignalR entre le navigateur et Visual Studio

La nouvelle fonctionnalité [Browser Link](using-browser-link.md) vous permet de connecter plusieurs navigateurs à Visual Studio et de les rafraîchir en cliquant sur un bouton dans la barre d’outils. Vous pouvez connecter plusieurs navigateurs à votre site de développement, y compris les émulateurs mobiles, et cliquez sur rafraîchir pour rafraîchir tous les navigateurs en même temps. Browser Link expose également une API pour permettre aux développeurs d’écrire des extensions de lien de navigateur.

![](release-notes/_static/image3.png)

En permettant aux développeurs de tirer parti de l’API Browser Link, il devient possible de créer des scénarios très avancés qui franchissent les frontières entre Visual Studio et n’importe quel navigateur connecté. Web Essentials profite de l’API pour créer une expérience intégrée entre Visual Studio et les outils de développement du navigateur, en contrôlant à distance les émulateurs mobiles et bien plus encore.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web Visual Studio

Visual Studio 2013 comprend un nouvel éditeur HTML pour les fichiers Razor et les fichiers HTML dans les applications Web. Le nouvel éditeur HTML fournit un schéma unifié unique basé sur HTML5. Il a l’achèvement automatique d’accolade, jQuery UI et l’attribut d’AngularJS IntelliSense, attribut IntelliSense Grouping, ID et nom de classe Intellisense, et d’autres améliorations comprenant de meilleures performances, le formatage et SmartTags.

La capture d’écran suivante démontre l’utilisation de l’attribut Bootstrap IntelliSense dans l’éditeur HTML.

![Intellisense dans l’éditeur HTML](release-notes/_static/image4.png)

Visual Studio 2013 est également livré avec des éditeurs CoffeeScript et LESS intégrés. L’éditeur LESS est livré avec toutes les fonctionnalités cool de l’éditeur CSS et a @import Intellisense spécifique pour les variables et mixines à travers tous les documents LESS de la chaîne.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Azure App Service Web Apps Support in Visual Studio

Dans Visual Studio 2013 avec l’Azure SDK pour .NET 2.2, vous pouvez utiliser **Server Explorer** pour interagir directement avec vos applications web distantes. Vous pouvez vous connecter à votre compte Azure, créer de nouvelles applications Web, configurer des applications, afficher des journaux en temps réel, et plus encore. Peu de temps après la sortie de SDK 2.2, vous pourrez courir en mode débogé à distance en Azure. La plupart des nouvelles fonctionnalités d’Azure App Service Web Apps fonctionnent également dans Visual Studio 2012 lorsque vous installez la version actuelle de l’Azure SDK pour .NET.

Pour plus d’informations, consultez les ressources suivantes :

- [Créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Dépanner une application web dans le Service d’application Microsoft Azure à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Améliorations de publication Web

Visual Studio 2013 comprend de nouvelles fonctionnalités web publiées améliorées. En voici quelques-uns :

- Automatisez facilement [le chiffrement des fichiers Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ce lien et les deux points suivants à la documentation sur MSDN qui pourraient ne pas être disponibles avant la fin de la journée le 10/17.)
- Automatisez facilement [la prise d’une application hors connexion pendant le déploiement](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurez Web Deploy pour [utiliser les vérifications de fichiers au lieu de la date](https://go.microsoft.com/fwlink/?LinkId=325531) de dernière modification pour déterminer quels fichiers doivent être copiés sur le serveur.
- Publiez rapidement des fichiers sélectionnés individuels (y compris Web.config) lorsque vous utilisez le FTP ou le système de fichiers publiez des méthodes ainsi qu’avec Web Deploy.

Pour plus d’informations sur ASP.NET déploiement web, voir [le site ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 comprend un riche ensemble de nouvelles fonctionnalités qui sont décrites en détail à [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet supprime également la nécessité de fournir un consentement explicite pour la fonction de restauration de paquets de NuGet pour télécharger des paquets. Le consentement (et la case à cocher associée dans le dialogue des préférences de NuGet) est maintenant accordé en installant NuGet. Maintenant, la restauration de paquet fonctionne simplement par défaut.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formulaires web ASP.NET

### <a name="one-aspnet"></a>Un ASP.NET

Les modèles de projet Web Forms s’intègrent parfaitement à la nouvelle expérience One ASP.NET. Vous pouvez ajouter le support MVC et Web API à votre projet Web Forms, et vous pouvez configurer l’authentification à l’aide de l’assistant de création de projet One ASP.NET. Pour plus d’informations, voir [Créer ASP.NET projets Web dans Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles du projet Web Forms prennent en charge le nouveau cadre ASP.NET Identity. En outre, les modèles prennent désormais en charge la création d’un projet intranet Web Forms. Pour plus d’informations, voir [Méthodes d’authentification](creating-web-projects-in-visual-studio.md#auth) dans **la création de projets Web ASP.NET dans Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Les modèles Web Forms utilisent [Bootstrap](http://twitter.github.io/bootstrap/) pour offrir un look élégant et réactif et sentir que vous pouvez facilement personnaliser. Pour plus d’informations, voir [Bootstrap dans les modèles de projets web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Un ASP.NET

Les modèles de projet Web MVC s’intègrent parfaitement à la nouvelle expérience One ASP.NET. Vous pouvez personnaliser votre projet MVC et configurer l’authentification à l’aide de l’assistant de création de projet One ASP.NET. Un tutoriel d’introduction pour ASP.NET MVC 5 peut être trouvé à [Getting Started avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Pour plus d’informations sur la mise à niveau des projets MVC 4 à MVC 5, voir [Comment mettre à niveau un ASP.NET MVC 4 et Web API Project pour ASP.NET MVC 5 et Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet MVC ont été mis à jour pour utiliser ASP.NET Identity pour l’authentification et la gestion de l’identité. Un tutoriel comportant l’authentification Facebook et Google et la nouvelle API d’adhésion peut être trouvé à [créer une ASP.NET application MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) et créer une application ASP.NET [MVC avec auth et SQL DB et déployer à Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Le modèle de projet MVC a été mis à jour pour utiliser [Bootstrap](http://getbootstrap.com/) pour fournir un look élégant et réactif et sentir que vous pouvez facilement personnaliser. Pour plus d’informations, voir [Bootstrap dans les modèles de projets web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtres d’authentification

Les filtres d’authentification sont un nouveau type de filtre dans ASP.NET MVC qui s’exécutent avant les filtres d’autorisation dans le pipeline MVC ASP.NET et vous permettent de spécifier la logique d’authentification par action, par contrôleur, ou globalement pour tous les contrôleurs. Les filtres d’authentification traitent les informations d’identification de la demande et fournissent un principe correspondant. Les filtres d’authentification peuvent également ajouter des défis d’authentification en réponse aux demandes non autorisées.

### <a name="filter-overrides"></a>Remplacements de filtres

Vous pouvez maintenant remplacer les filtres appliqués à une méthode d’action ou à un contrôleur donné en spécifiant un filtre de remplacement. Les filtres de remplacement spécifient un ensemble de types de filtres qui ne doivent pas être exécutés pour une portée donnée (action ou contrôleur). Cela vous permet de configurer des filtres qui s’appliquent à l’échelle mondiale, mais ensuite exclure certains filtres globaux de l’application à des actions ou des contrôleurs spécifiques.

### <a name="attribute-routing"></a>Routage par attributs

ASP.NET MVC prend désormais en charge le routage d’attributs, [http://attributerouting.net](http://attributerouting.net)grâce à une contribution de Tim McCall, l’auteur de . Avec le routage d’attribut, vous pouvez spécifier vos itinéraires en annotant vos actions et contrôleurs.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>API Web ASP.NET 2

### <a name="attribute-routing"></a>Routage par attributs

ASP.NET’API Web prend désormais en charge le routage des attributs, [http://attributerouting.net](http://attributerouting.net)grâce à une contribution de Tim McCall, l’auteur de . Avec le routage d’attribut, vous pouvez spécifier vos itinéraires d’API Web en annotant vos actions et contrôleurs comme ceci :

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Le routage d’attribut vous donne plus de contrôle sur les URL dans votre API Web. Par exemple, vous pouvez facilement définir une hiérarchie de ressources à l’aide d’un seul contrôleur API :

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Le routage d’attribut fournit également une syntaxe pratique pour spécifier des paramètres optionnels, des valeurs par défaut et des contraintes d’itinéraire :

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Pour plus d’informations sur le routage des attributs, voir [Attribut Routing dans l’API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Les modèles de projets Web API et Single Page Application prennent désormais en charge l’autorisation à l’aide d’OAuth 2.0. OAuth 2.0 est un cadre permettant aux clients d’accéder à des ressources protégées. Il fonctionne pour une variété de clients, y compris les navigateurs et les appareils mobiles.

La prise en charge d’OAuth 2.0 est basée sur de nouveaux intermédiaires de sécurité fournis par les composants Microsoft OWIN pour l’authentification du porteur et la mise en œuvre du rôle du serveur d’autorisation. Alternativement, les clients peuvent être autorisés à l’aide d’un serveur d’autorisation organisationnelle, comme Azure Active Directory ou ADFS dans Windows Server 2012 R2.

### <a name="odata-improvements"></a>Améliorations OData

**Soutien aux $select, aux $expand, aux $batch et aux $value**

ASP.NET Web API OData bénéficie désormais d’un soutien total pour $select, $expand et $value. Vous pouvez également utiliser $batch pour la demande de lotage et de traitement des ensembles de modification.

Les options $select et $expand vous permettent de modifier la forme des données qui sont retournées à partir d’un point de terminaison OData. Pour plus d’informations, voir [Introducing $select et $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Amélioration de l’extéabilité**

Les formateurs OData sont désormais extensibles. Vous pouvez ajouter des métadonnées d’entrée Atom, prendre en charge les entrées de flux et de liaison multimédia, ajouter des annotations de instance et personnaliser la façon dont les liens sont générés.

**Soutien sans type**

Vous pouvez maintenant construire des services OData sans avoir besoin de définir les types CLR pour vos types d’entités. Au lieu de cela, vos contrôleurs OData peuvent prendre ou retourner des instances **d’IEdmObject**, qui sont les formateurs OData sérialiser / déséialiser.

**Réutiliser un modèle existant**

Si vous avez déjà un modèle de données d’entités existant (EDM), vous pouvez maintenant le réutiliser directement, au lieu d’avoir à en construire un nouveau. Par exemple, si vous utilisez Entity Framework, vous pouvez utiliser le MED que EF construit pour vous.

### <a name="request-batching"></a>Demande Batching

Le lotage de demande combine plusieurs opérations en une seule demande DE MESSAGE HTTP, pour réduire le trafic réseau et fournir une interface utilisateur plus fluide et moins bavarde. ASP.NET’API Web prend désormais en charge plusieurs stratégies de lotage de demandes :

- Utilisez le $batch point de terminaison d’un service OData.
- Emballez plusieurs demandes en une seule demande multipartite MIME.
- Utilisez un format de lotage personnalisé.

Pour activer le lotage des demandes, il vous suffit d’ajouter un itinéraire avec un gestionnaire de lots à votre configuration Web API :

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Vous pouvez également contrôler si les demandes ou exécutés séquentiellement ou dans n’importe quel ordre.

### <a name="portable-aspnet-web-api-client"></a>Portable ASP.NET Web API Client

Vous pouvez maintenant utiliser le ASP.NET client Web API pour créer des bibliothèques de classe portables qui fonctionnent sur vos applications Windows Store et Windows Phone 8. Vous pouvez également créer des formateurs portables qui peuvent être partagés entre les clients et les serveurs.

### <a name="improved-testability"></a>Amélioration de la testabilité

L’API Web 2 facilite beaucoup le test de test de vos contrôleurs API. Il suffit d’instantané de votre contrôleur API avec votre message de demande et la configuration, puis appelez la méthode d’action que vous souhaitez tester. Il est également facile de se moquer de la classe **UrlHelper,** pour les méthodes d’action qui effectuent la génération de liens.

### <a name="ihttpactionresult"></a>IHttpActionResult

Vous pouvez maintenant implémenter IHttpActionResult pour encapsuler le résultat de vos méthodes d’action Web API. Un IHttpActionResult retourné d’une méthode d’action Web API est exécuté par le ASP.NET’heure d’exécution de l’API Web pour produire le message de réponse résultant. Une IHttpActionResult peut être renvoyée de n’importe quelle action API Web pour simplifier les tests unitaires de votre implémentation d’API Web. Pour plus de commodité, un certain nombre d’implémentations IHttpActionResult sont fournis hors de la boîte, y compris les résultats pour le retour de codes de statut spécifiques, de contenu formaté ou de réponses négociées en contenu.

### <a name="httprequestcontext"></a>HttpRequestContext

Le nouveau **httpRequestContext** suit tout état qui est lié à la demande, mais n’est pas immédiatement disponible à partir de la demande. Par exemple, vous pouvez utiliser le **texte httpRequestContext** pour obtenir des données d’itinéraire, le principal associé à la demande, le certificat client, **l’UrlHelper** et la racine de chemin virtuel. Vous pouvez facilement créer un **httpRequestContext** à des fins de test unitaire.

Étant donné que le principal de la demande est transmis avec la demande au lieu de s’appuyer sur **Thread.CurrentPrincipal**, le directeur est maintenant disponible tout au long de la durée de vie de la demande alors qu’il est dans le pipeline Web API.

### <a name="cors"></a>CORS

Grâce à une autre grande contribution de Brock Allen, ASP.NET prend désormais en charge entièrement le partage des demandes d’origine croisée (CORS).

La sécurité des navigateurs empêche une page web d’adresser des demandes AJAX à un autre domaine. [CORS](http://www.w3.org/TR/cors/) est une norme W3C qui permet à un serveur d’assouplir la même stratégie d’origine. Grâce au mécanisme CORS, un serveur peut autoriser explicitement certaines demandes multi-origines tout en en refusant d’autres.

Web API 2 prend désormais en charge CORS, y compris la manipulation automatique des demandes de prévol. Pour plus d’informations, consultez la section [Activation des demandes multi-origines dans l’API web ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtres d’authentification

Les filtres d’authentification sont un nouveau type de filtre dans ASP.NET’API Web qui s’exécutent avant les filtres d’autorisation dans le pipeline ASP.NET’API Web et vous permettent de spécifier la logique d’authentification par action, par contrôleur ou globalement pour tous les contrôleurs. Les filtres d’authentification traitent les informations d’identification de la demande et fournissent un principe correspondant. Les filtres d’authentification peuvent également ajouter des défis d’authentification en réponse aux demandes non autorisées.

### <a name="filter-overrides"></a>Remplacements de filtre

Vous pouvez maintenant remplacer les filtres appliqués à une méthode d’action ou à un contrôleur donné, en spécifiant un filtre de remplacement. Les filtres de remplacement spécifient un ensemble de types de filtres qui ne devraient pas fonctionner pour une portée donnée (action ou contrôleur). Cela vous permet d’ajouter des filtres globaux, mais ensuite exclure certains d’actions spécifiques ou contrôleurs.

### <a name="owin-integration"></a>Intégration OWIN

ASP.NET’API Web prend désormais en charge L’OWIN et peut être exécuté sur n’importe quel hôte capable OWIN. Sont également inclus un **HostAuthenticationFilter** qui fournit l’intégration avec le système d’authentification OWIN.

Avec l’intégration OWIN, vous pouvez auto-héberger l’API Web dans votre propre processus aux côtés d’autres intermédiaires OWIN, tels que SignalR. Pour plus d’informations, voir [Utilisez OWIN à l’auto-hôte ASP.NET’API Web](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET Signalr 2.0

Les sections suivantes décrivent les caractéristiques de SignalR 2.0.

- [Construit sur OWIN](#builtonowin)
- [MapHubs et MapConnection sont maintenant MapSignalR](#MapSignalR)
- [Soutien cross-Domaine](#crossdomain)
- [Support iOS et Android via MonoTouch et MonoDroid](#mobile)
- [Portable .NET Client](#portable)
- [Nouveau forfait Auto-hôte](#selfhost)
- [Support serveur compatible avec l’arrière](#backwardcompat)
- [Prise en charge du serveur supprimé pour .NET 4.0](#remove40)
- [Envoi d’un message à une liste de clients et de groupes](#messagelist)
- [Envoi d’un message à un utilisateur spécifique](#sendtouser)
- [Un meilleur support de traitement des erreurs](#errorhandling)
- [Tests unitaires plus faciles des hubs](#unittesting)
- [Gestion d’erreur JavaScript](#javascripterror)

Pour un exemple de la façon de mettre à niveau un projet existant de 1.x à SignalR 2.0, voir [Mise à niveau d’un signalR 1.x Projet](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Construit sur OWIN

SignalR 2.0 est entièrement construit sur [OWIN (l’interface Web ouverte pour .NET)](http://owin.org/). Cette modification rend le processus de configuration de SignalR beaucoup plus cohérent entre les applications SignalR hébergées sur le Web et auto-hébergées, mais a également nécessité un certain nombre de modifications API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs et MapConnection sont maintenant MapSignalR

Pour la compatibilité avec les normes OWIN, ces méthodes ont été rebaptisées à `MapSignalR`. `MapSignalR`appelé sans paramètres cartographiera tous `MapHubs` les hubs (comme le fait la version 1.x); pour cartographier les objets **persistants** individuels, spécifiez le type de connexion comme paramètre de type, et l’extension d’URL pour la connexion comme premier argument.

La `MapSignalR` méthode est appelée dans une classe de démarrage Owin. Visual Studio 2013 contient un nouveau modèle pour une classe de start-up Owin; pour utiliser ce modèle, faites ce qui suit :

1. Cliquer à droite sur le projet
2. Sélectionnez **Ajouter**, **Nouvel article...**
3. Sélectionnez **Owin Startup classe**. Nommez la nouvelle classe **Startup.cs**.

Dans une **application Web,** la classe `MapSignalR` de démarrage Owin contenant la méthode est ensuite ajoutée au processus de démarrage d’Owin à l’aide d’une entrée dans le nœud des paramètres d’application du fichier Web.Config, tel qu’indiqué ci-dessous.

Dans une **application auto-hébergée,** la classe Startup est `WebApp.Start` passée comme le paramètre de type de la méthode.

**Cartographier les hubs et les connexions dans SignalR 1.x (à partir du fichier d’application global dans une application web) :** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Cartographier les hubs et les connexions dans SignalR 2.0 (à partir d’un fichier de classe Owin Startup) :** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

Dans une **application auto-hébergée,** la classe Startup est `WebApp.Start` adoptée comme le paramètre de type pour la méthode, comme indiqué ci-dessous.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Soutien cross-Domaine

Dans SignalR 1.x, les demandes de domaine transversal ont été contrôlées par un seul drapeau EnableCrossDomain. Ce drapeau contrôlait à la fois les demandes de JSONP et de CORS. Pour une plus grande flexibilité, tout le support CORS a été supprimé de la composante serveur de SignalR (les clients JavaScript utilisent toujours CORS normalement s’il est détecté que le navigateur le prend en charge), et de nouveaux middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.

Dans SignalR 2.0, Si JSONP est nécessaire sur le client (pour prendre en charge les `EnableJSONP` demandes `HubConfiguration` de `true`cross-domain dans les anciens navigateurs), il devra être activé explicitement en définissant sur l’objet à , comme indiqué ci-dessous. JSONP est désactivé par défaut, car il est moins sûr que CORS.

Pour ajouter le nouveau middleware CORS dans SignalR `Microsoft.Owin.Cors` 2.0, `UseCors` ajoutez la bibliothèque à votre projet et appelez avant votre middleware SignalR, comme indiqué dans la section ci-dessous.

**Ajout de Microsoft.Owin.Cors à votre projet**: Pour installer cette bibliothèque, exécutez la commande suivante dans la console Package Manager :

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Cette commande ajoutera la version 2.0.0 du paquet à votre projet.

**Appel UseCors**

Les extraits de code suivants démontrent comment implémenter des connexions cross-domain dans SignalR 1.x et 2.0.

**Mise en œuvre des demandes inter-domaines dans SignalR 1.x (à partir du fichier d’application global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Mise en œuvre des demandes de cross-domain dans SignalR 2.0 (à partir d’un fichier de code C)**

Le code suivant montre comment activer CORS ou JSONP dans un projet SignalR 2.0. Cet exemple `Map` de `RunSignalR` code `MapSignalR`utilise et au lieu de , de sorte que le middleware CORS fonctionne uniquement `MapSignalR`pour les demandes SignalR qui nécessitent une prise en charge CORS (plutôt que pour tout le trafic sur le chemin spécifié dans .) `Map` peut également être utilisé pour tout autre middleware qui doit s’exécuter pour un préfixe URL spécifique, plutôt que pour l’ensemble de l’application.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>Support iOS et Android via MonoTouch et MonoDroid

Un support a été ajouté pour les clients iOS et Android utilisant des composants MonoTouch et MonoDroid de la [bibliothèque Xamarin](https://xamarin.com/). Pour plus d’informations sur la façon de les utiliser, voir [En utilisant des composants Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Ces composants seront disponibles dans le [Xamarin Store](https://store.xamarin.com/) lorsque la version SignalR RTW sera disponible.

<a id="portable"></a>Client portable .NET

Pour mieux faciliter le développement multiplateforme, les clients Silverlight, WinRT et Windows Phone ont été remplacés par un seul client .NET portable qui prend en charge les plates-formes suivantes :

- NET 4,5
- Silverlight 5
- WinRT (.NET pour Windows Store Apps)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nouveau forfait Auto-hôte

Il existe maintenant un package NuGet pour faciliter le démarrage avec SignalR Self-Host (applications SignalR qui sont hébergées dans un processus ou une autre application, plutôt que d’être hébergées dans un serveur web). Pour mettre à niveau un projet d’auto-hôte construit avec SignalR 1.x, supprimez le package Microsoft.AspNet.SignalR.Owin, et ajoutez le package Microsoft.AspNet.SignalR.SelfHost. Pour plus d’informations sur le démarrage avec le paquet d’auto-hôte, voir [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Support serveur compatible avec l’arrière

Dans les versions précédentes de SignalR, les versions du paquet SignalR utilisées dans le client et le serveur devaient être identiques. Afin de prendre en charge les applications à haut client qui seraient difficiles à mettre à jour, SignalR 2.0 prend désormais en charge l’utilisation d’une version serveur plus récente avec un client plus âgé. **Remarque : SignalR 2.0 ne prend pas en charge les serveurs construits avec des versions plus anciennes avec des clients plus récents.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Prise en charge du serveur supprimé pour .NET 4.0

SignalR 2.0 a supprimé la prise en charge de l’interopérabilité du serveur avec .NET 4.0. .NET 4.5 doit être utilisé avec les serveurs SignalR 2.0. Il y a toujours un client .NET 4.0 pour SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Envoi d’un message à une liste de clients et de groupes

Dans SignalR 2.0, il est possible d’envoyer un message à l’aide d’une liste d’ID clients et de groupes. Les extraits de code suivants démontrent comment le faire.

**Envoi d’un message à une liste de clients et de groupes utilisant PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Envoi d’un message à une liste de clients et de groupes utilisant Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Envoi d’un message à un utilisateur spécifique

Cette fonctionnalité permet aux utilisateurs de spécifier ce que l’utilisateurId est basé sur un IRequest via une nouvelle interface IUserIdProvider:

**L’interface IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Par défaut, il y aura une implémentation qui utilise la IPrincipal.Identity.Name de l’utilisateur comme nom d’utilisateur.

Dans les hubs, vous serez en mesure d’envoyer des messages à ces utilisateurs via une nouvelle API :

**Utilisation de l’API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Un meilleur support de traitement des erreurs

Les utilisateurs peuvent désormais lancer **HubException** à partir de n’importe quelle invocation hub. Le constructeur de la **HubException** peut prendre un message de chaîne et un objet des données d’erreur supplémentaires. SignalR va auto-sérialiser l’exception et l’envoyer au client où il sera utilisé pour rejeter / échouer l’invocation méthode de moyeu.

Le réglage détaillé des exceptions de hub de **l’exposition** n’a aucune incidence sur **HubException** étant renvoyé au client ou non; il est toujours envoyé.

**Code côté serveur démontrant l’envoi d’un HubException au client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Code client JavaScript démontrant la réponse à un HubException envoyé à partir du serveur**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET code client démontrant la réponse à un HubException envoyé à partir du serveur**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Tests unitaires plus faciles des hubs

SignalR 2.0 comprend `IHubCallerConnectionContext` une interface appelée sur Hubs qui facilite la création de fausses invocations côté client. Les extraits de code suivants démontrent l’utilisation de cette interface avec des harnais de test populaires [xUnit.net](https://github.com/xunit/xunit) et [moq](https://code.google.com/p/moq/).

**SignalR d’essai d’unité avec xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Test d’unité SignalR avec moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestion d’erreur JavaScript

Dans SignalR 2.0, tous les rappels de gestion d’erreurs JavaScript renvoient des objets d’erreur JavaScript au lieu de chaînes brutes. Cela permet à SignalR de faire circuler des informations plus riches à vos gestionnaires d’erreurs. Vous pouvez obtenir l’exception interne de la `source` propriété de l’erreur.

**Code client JavaScript qui gère l’exception Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nouveau système d’adhésion ASP.NET

ASP.NET Identity est le nouveau système d’adhésion pour les applications ASP.NET. ASP.NET Identity facilite l’intégration des données de profil spécifiques aux utilisateurs avec les données d’application. ASP.NET Identity vous permet également de choisir le modèle de persistance des profils d’utilisateurs dans votre application. Vous pouvez stocker les données dans une base de données SQL Server ou un autre magasin de données, y compris dans des magasins de données NoSQL tels que des tables Azure Storage. Pour plus d’informations, consultez [les comptes d’utilisateurs individuels](creating-web-projects-in-visual-studio.md#indauth) dans La création de **projets Web ASP.NET dans Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Authentification basée sur les revendications

ASP.NET prend désormais en charge l’authentification fondée sur les revendications, où l’identité de l’utilisateur est représentée comme un ensemble de réclamations d’un émetteur de confiance. Les utilisateurs peuvent être authentifiés à l’aide d’un nom d’utilisateur et d’un mot de passe conservés dans une base de données d’applications, ou en utilisant des fournisseurs d’identité sociale (par exemple : Comptes Microsoft, Facebook, Google, Twitter), ou à l’aide de comptes organisationnels via Azure Active Directory ou Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Intégration avec Azure Active Directory et Windows Server Active Directory

Vous pouvez maintenant créer ASP.NET projets qui utilisent Azure Active Directory ou Windows Server Active Directory (AD) pour l’authentification. Pour plus d’informations, voir [Comptes organisationnels](creating-web-projects-in-visual-studio.md#orgauth) dans **la création de projets Web ASP.NET dans Visual Studio 2013**.

### <a name="owin-integration"></a>Intégration OWIN

ASP.NET authentification est maintenant basée sur les intermédiaires OWIN qui peuvent être utilisés sur n’importe quel hôte basé sur OWIN. Pour plus d’informations sur OWIN, voir la section [suivante des composants Microsoft OWIN.](#TOC7)

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

[Open Web Interface for .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs Web .NET et les applications web. OWIN découple l’application web à partir du serveur, rendant les applications Web host-agnostic. Par exemple, vous pouvez héberger une application Web basée sur OWIN dans IIS ou l’auto-héberger dans un processus personnalisé.

Les modifications introduites dans les composants Microsoft OWIN (également connu sous le nom de projet Katana) comprennent de nouveaux composants de serveur et d’hôte, de nouvelles bibliothèques d’aide et middleware, et de nouveaux middleware d’authentification.

Pour plus d’informations sur OWIN et Katana, voir [Ce qui est nouveau dans OWIN et Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Remarque : Les applications [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) ne peuvent pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.**

**Remarque : les demandes [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) doivent être exécutées en pleine confiance.**

### <a name="new-servers-and-hosts"></a>Nouveaux serveurs et hôtes

Avec cette version, de nouveaux composants ont été ajoutés pour permettre des scénarios d’auto-hôte. Ces composants comprennent les paquets NuGet suivants :

- **Microsoft.Owin.Host.httpListener**. Fournit un serveur OWIN qui utilise **HttpListener** pour écouter les demandes HTTP et les diriger dans le pipeline OWIN.
- **Microsoft.Owin.Hosting** fournit une bibliothèque pour les développeurs qui souhaitent héberger en eux-mêmes un pipeline OWIN dans un processus personnalisé, comme une application de console ou un service Windows.
- **OwinHost**. Fournit un exécutant autonome qui `Microsoft.Owin.Hosting` enveloppe et vous permet d’auto-héberger un pipeline OWIN sans avoir à écrire une application d’hôte personnalisé.

En outre, `Microsoft.Owin.Host.SystemWeb` le paquet permet maintenant middleware de fournir des conseils au serveur **SystemWeb,** indiquant que le middleware doit être appelé au cours d’une ASP.NET étape spécifique du pipeline. Cette fonctionnalité est particulièrement utile pour l’authentification middleware, qui devrait fonctionner tôt dans le pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Bibliothèques d’aide et Middleware

Bien que vous puissiez écrire des composants OWIN en utilisant uniquement les `Microsoft.Owin` définitions de fonction et de type de la spécification OWIN, le nouveau paquet fournit un ensemble plus convivial d’abstractions. Ce paquet combine plusieurs paquets antérieurs (p. ex., `Owin.Extensions`) `Owin.Types`en un seul modèle d’objet bien structuré qui peut ensuite être facilement utilisé par d’autres composants OWIN. En fait, la majorité des composants Microsoft OWIN utilisent maintenant ce paquet.

> [!NOTE]
> Les applications [OWIN](http://www.owin.org) ne peuvent pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.

> [!NOTE]
> Les demandes [OWIN](http://www.owin.org) doivent être exécutées en pleine confiance.

Cette version comprend également le paquet Microsoft.Owin.Diagnostics, qui comprend middleware pour valider une application OWIN en cours d’exécution, ainsi que l’erreur-page middleware pour aider à enquêter sur les échecs.

### <a name="authentication-components"></a>Composants d’authentification

Les composants d’authentification suivants sont disponibles.

- **Microsoft.Owin.Security.ActiveDirectory**. Permet l’authentification à l’aide de services d’annuaire sur site ou en nuage.
- **Microsoft.Owin.Security.Cookies** permet l’authentification à l’aide de cookies. Ce paquet a `Microsoft.Owin.Security.Forms`été précédemment nommé .
- **Microsoft.Owin.Security.Facebook** permet l’authentification à l’aide du service OAuth de Facebook.
- **Microsoft.Owin.Security.Google** permet l’authentification à l’aide du service OpenID de Google.
- **Microsoft.Owin.Security.Jwt** permet l’authentification à l’aide de jetons JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** permet l’authentification à l’aide de comptes Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fournit un serveur d’autorisation OAuth ainsi que middleware pour authentifier les jetons porteurs.
- **Microsoft.Owin.Security.Twitter** permet l’authentification à l’aide du service basé sur l’Ouath de Twitter.

Cette version comprend `Microsoft.Owin.Cors` également le paquet, qui contient middleware pour le traitement des demandes HTTP d’origine croisée.

> [!NOTE]
> Le support pour la signature de JWT a été supprimé dans la version finale de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Pour une liste de nouvelles fonctionnalités et d’autres modifications dans Entity Framework 6, voir [Entity Framework Version History](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 comprend les nouvelles fonctionnalités suivantes :

- Prise en charge de l’édition De l’onglet. Auparavant, la commande **Format Document,** l’indentage automatique et le formatage automatique dans Visual Studio ne fonctiondaient pas correctement lors de l’utilisation de **l’option Keep Tabs.** Cette modification corrige le formatage Visual Studio pour le code Razor pour le formatage de l’onglet.
- Prendre en charge les règles de réécriture d’URL lors de la génération de liens.
- Suppression de l’attribut transparent de sécurité.
  > [!NOTE]
  > Il s’agit d’un changement de rupture, et rend Razor 3 incompatible avec MVC4 et plus tôt, tandis que Razor 2 est incompatible avec MVC5 ou des assemblages compilés contre MVC5.

Razor 3 questions corrigées dans Visual Studio 2013 à partir de versions pré-version peuvent être trouvées [ici](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET App Suspend

ASP.NET App Suspend est une fonctionnalité qui change la donne dans le cadre .NET 4.5.1 qui modifie radicalement l’expérience utilisateur et le modèle économique pour héberger un grand nombre de sites ASP.NET sur une seule machine. Pour plus d’informations, voir [ASP.NET App Suspendre - responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et changements de rupture

Cette section décrit les problèmes connus et les changements radicaux dans les outils ASP.NET et Web pour Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [La nouvelle restauration de paquet ne fonctionne pas sur Mono lors de l’utilisation du fichier SLN](https://nuget.codeplex.com/workitem/3596) - sera corrigée dans un téléchargement nuget.exe à venir et [nuGet.CommandLine mise](http://www.nuget.org/packages/NuGet.CommandLine/) à jour du paquet.
- [La nouvelle restauration de paquet ne fonctionne pas avec les projets Wix](https://nuget.codeplex.com/workitem/3598) - sera fixé dans un téléchargement nuget.exe à venir et [nuGet.CommandLine mise](http://www.nuget.org/packages/NuGet.CommandLine/) à jour du paquet.
- [La restauration automatique de paquet ne fonctionne pas pour des projets sous un dossier de solution](https://nuget.codeplex.com/workitem/3625) - sera fixée dans NuGet 2.8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`ne revient `IQueryable<T>` pas toujours, comme `$select` `$expand`nous avons ajouté un soutien pour et .

    Nos échantillons `ODataQueryOptions<T>` antérieurs pour toujours `ApplyTo` jeté `IQueryable<T>`la valeur de retour de . Cela a fonctionné plus tôt parce que`$filter` `$orderby`les `$skip` `$top`options de requête que nous avons pris en charge plus tôt ( , , , ) ne changent pas la forme de la requête. Maintenant que `$select` nous `$expand` soutenons et `ApplyTo` la `IQueryable<T>` valeur de retour de ne sera pas toujours.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si vous utilisez le code de l’échantillon de plus `$select` tôt, il continuera à travailler si le client n’envoie pas et `$expand`. Toutefois, si vous `$select` souhaitez `$expand` prendre en charge et que vous devez modifier ce code pour cela.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url ou RequestContext.Url est nul lors d’une demande de lot**

    Dans un scénario de lotage, **UrlHelper** est nul lorsqu’il est accessible à partir de **Request.Url** ou **RequestContext.Url**.

    Ce numéro est actuellement suivi ici: [BatchRequestContext.Url est nul pour la demande de lotage](http://aspnetwebstack.codeplex.com/workitem/1301).

    La solution de contournement pour ce numéro est de créer une nouvelle instance **d’UrlHelper**, comme dans l’exemple suivant:

    **Création d’une nouvelle instance d’UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Lorsque vous utilisez MVC5 et OrgAuth, si vous avez des vues qui font la validation AntiForgerToken, vous pouvez rencontrer l’erreur suivante lorsque vous publiez des données à la vue:

    **Erreur** :

    *Erreur de serveur dans l’application « / ».*

    <em>Une réclamation de<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>type<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' ou ' n’était pas présente sur l’identité des revendications fournies. Pour permettre un soutien symbolique anti-contrefaçon avec authentification basée sur les revendications, veuillez vérifier que le fournisseur de sinistres configuré fournit ces deux allégations sur les instances d’identification des réclamations qu’il génère. Si le fournisseur de sinistres configuré utilise plutôt un type de réclamation différent comme identifiant unique, il peut être configuré en définissant la propriété statique AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Solution** :

    Ajoutez la ligne suivante dans Global.asax pour la corriger :

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ceci sera fixé pour la prochaine version.
2. Après la mise à niveau d’une application MVC4 vers MVC5, construisez la solution et lancez-la. Vous devriez voir l’erreur suivante :

    [A] System.Web.WebPages.Razor.Configuration.HostSection ne peut pas être jeté à [B]System.Web.WebPages.Razor.Configuration.HostSection. Type A provient de 'System.Web.WebPages.Razor, Version 2.0.0.0, Culture neutre, PublicKeyToken-31bf3856ad364e35' dans le contexte 'Default' à l’emplacement\_'C:'windows’Microsoft.Net’assembly’GAC MSIL-System.Web.WebPages.Razor.v4.0\_2.0.0\_\_31bf3856ad36e35-System.WebPage.Web.Web.Razorll. Type B provient de 'System.Web.WebPages.Razor, Version 3.0.0.0, Culture’neutre, PublicKeyToken-31bf3856ad364e35' dans le contexte 'Default' à l’emplacement 'C:'Windows’Microsoft.NET-Framework’v4.0.30319'Temporary ASP.NET Files’root 6d05bbd0-e8b5908e-assembly’dl3'c9cbca63-f8910382\_6273ce01-System.Web.WebPages.Razor.dll'.

    Pour corriger l’erreur ci-dessus, ouvrez *tous les* fichiers Web.config (y compris ceux du dossier Vues) dans votre projet et faites ce qui suit :

   1. Mettez à jour tous les événements de la version "4.0.0.0" de "System.Web.Mvc" à "5.0.0.0".
   2. Mettre à jour tous les événements de la version "2.0.0.0" &quot;de "System.Web.Helpers", System.Web.WebPages&quot; et &quot;System.WebPages.Razor&quot; à "3.0.0.0"

      Par exemple, après avoir apporté les modifications ci-dessus, les fixations d’assemblage devraient ressembler à ceci :

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Pour plus d’informations sur la mise à niveau des projets MVC 4 à MVC 5, voir [Comment mettre à niveau un ASP.NET MVC 4 et Web API Project pour ASP.NET MVC 5 et Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Lors de l’utilisation de la validation côté client avec jQuery Nonobtrusive Validation, le message de validation est parfois incorrect pour un élément d’entrée HTML avec type 'numéro'. L’erreur de validation d’une valeur requise (« Le champ d’âge est requis ») est affichée lorsqu’un numéro invalide est entré au lieu du message correct qu’un numéro valide est requis.

    Ce problème se trouve couramment avec le code échafaudé pour un modèle avec une propriété d’intégrage sur les vues Créer et modifier.

    Pour contourner cette question, changez l’assistant rédacteur en chef de :

    `@Html.EditorFor(person => person.Age)`

    Par :

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ne soutient plus la confiance partielle. Les projets liés aux binaires MVC ou WebAPI doivent supprimer [l’attribut SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) et [l’attribut AllowPartiallyTrustedCallers.](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) La suppression de ces attributs éliminera les erreurs de compilateur telles que ce qui suit.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Notez, comme un effet secondaire de ce que vous ne pouvez pas utiliser 4.0 et 5.0 assemblages dans la même application. Vous devez les mettre à jour tous à 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Le modèle SPA avec l’autorisation de Facebook peut causer l’instabilité dans l’IE tandis que le site Web est hébergé dans la zone intranet

Le modèle SPA fournit un journal externe avec Facebook. Lorsque le projet créé avec le modèle est en cours d’exécution localement, la connexion peut entraîner IE à planter.

Solution :

1. Hébergez le site Web en zone Internet; Ou

2. Testez le scénario dans un navigateur autre que IE.

### <a name="web-forms-scaffolding"></a>Génération de modèles automatique Web Forms

Web Forms Scaffolding a été supprimé de VS2013 et sera disponible dans une prochaine mise à jour de Visual Studio. Cependant, vous pouvez toujours utiliser des échafaudages dans le cadre d’un projet Web Forms en ajoutant des dépendances MVC et en générant des échafaudages pour MVC. Votre projet contiendra une combinaison de formulaires Web et de MVC.

Pour ajouter du MVC à votre projet Web Forms, ajoutez un nouvel article échafaudé et sélectionnez **des dépendances MVC 5**. Sélectionnez soit Minimal ou Full selon que vous avez besoin de tous les fichiers de contenu, tels que les scripts. Ensuite, ajoutez un élément échafaudé pour MVC, qui créera des vues et un contrôleur dans votre projet.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC et Web API Échafaudage - HTTP 404, erreur non trouvée

Si une erreur est rencontrée lors de l’ajout d’un objet échafaudé à un projet, il est possible que votre projet soit laissé dans un état incohérent. Certaines des modifications apportées à l’échafaudage seront annulées, mais d’autres changements, comme les paquets NuGet installés, ne seront pas annulés. Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lors de la navigation vers des éléments échafaudés.

Solution de contournement :

- Pour corriger cette erreur pour MVC, ajoutez un nouvel article échafaudé et sélectionnez les dépendances MVC 5 (minimales ou complètes). Ce processus ajoutera toutes les modifications requises à votre projet.
- Pour corriger cette erreur pour l’API Web :

  1. Ajoutez la classe WebApiConfig à votre projet.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurez WebApiConfig.Register\_dans la méthode De démarrage d’application dans Global.asax comme suit :

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
