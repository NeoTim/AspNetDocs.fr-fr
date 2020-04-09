---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET et Web Tools 2012.2 Notes de sortie (fr) Microsoft Docs
author: rick-anderson
description: Publier des notes pour ASP.NET et Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: abd6d8ce0646852a194369589cb730fc98ecb3ad
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676306"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET et Web Tools 2012.2 - Notes de publication

> Ce document décrit la publication de ASP.NET et Web Tools 2012.2. Il s’agit d’une mise à jour de Visual Studio Web Tooling and ASP.NET.

- [Notes d’installation](#_Installation)
- [Documentation](#_Documentation)
- [Support](#_Support)
- [Exigences logicielles](#_Software_Requirements)
- [Nouvelles fonctionnalités dans ASP.NET et web Tools 2012.2](#_New_Features_in)

    - [Outils](#_Tooling)
    - [publication Web](#_Web_Publishing)
    - [modèles MVC ASP.NET](#_Templates)
    - [API Web ASP.NET](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL conviviales ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problèmes connus et changements de rupture](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notes d'installation

ASP.NET et Web Tools 2012.2 pour Visual Studio 2012 peuvent être installés à l’aide [de l’installateur Web Platform](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Il s’agit d’une mise à jour de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web, qui est nécessaire. Si vous n’avez pas visual Studio installé, Visual Studio Express 2012 pour Le Web sera installé.

Vous pouvez également installer manuellement ASP.NET et Web Tools 2012.2. Vous devez avoir Visual Studio 2012 ou Visual Studio Express 2012 pour l’installation web. Ensuite, utilisez les instructions suivantes : 

1. Téléchargez [ASP.NET et Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) installateur de Download Center.
2. Lorsque invité cliquez sur Run. Vous pouvez également enregistrer le fichier pour l’exécuter plus tard.
3. Vérifiez la version de Visual Studio que vous mettre à jour. Pour ce faire, vous pouvez lancer le Visual Studio que vous souhaitez mettre à jour. Cliquez ensuite sur l’élément menu d’aide.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Si vous voyez &quot;l’élément de menu sur Microsoft&quot; Visual Studio 2012 pour le Web, puis téléchargez [Web Developer Tools 2012.2 - Visual Studio Express 2012 pour Web](https://go.microsoft.com/fwlink/?LinkID=282228). Sinon télécharger [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Lorsque invité cliquez sur Run. Vous pouvez également enregistrer le fichier pour l’exécuter plus tard.

> [!NOTE]
> ASP.NET et Web Tools 2012.2 communiqué n’inclut pas SQL Server Data Tools. SQL Server et Windows Azure SQL Databases fournit un ensemble plus riche d’outillage de base de données, y compris le développement hors ligne soutenu par un projet, la comparaison des schémas et des capacités améliorées de déploiement de bases de données. Pour plus d’informations ou pour [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)installer SQL Server Data Tools visitez .

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentation

Des tutoriels et d’autres informations sur ASP.NET et web Tools 2012.2 https://www.asp.net)sont disponibles sur ASP.NET site Web ( .

<a id="_Support"></a>
## <a name="support"></a>Support

ASP.NET et Web Tools 2012.2 est officiellement publié et pris en charge. Vous pouvez utiliser votre canal de support normal. Vous pouvez également poser des questions aux[https://forums.asp.net/](https://forums.asp.net/)forums ASP.NET (), où les membres de la communauté ASP.NET sont souvent en mesure de fournir un soutien informel.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Le ASP.NET et Web Tools 2012.2 nécessite Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nouvelles fonctionnalités dans ASP.NET et web Tools 2012.2

Cette section décrit les fonctionnalités qui ont été introduites dans la version ASP.NET et Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Outils

- Inspecteur de page 

    - Prendre en charge la cartographie de sélection JavaScript permettant à Page Inspector de cartographier les éléments qui ont été ajoutés dynamiquement à la page vers le code JavaScript correspondant.
    - La possibilité de voir les mises à jour CSS en temps réel.
    - Pour plus d’informations, lisez [CSS Auto-Sync et JavaScript Selection Mapping dans Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Éditeur 

    - Support syntaxe en surbrillance pour CoffeeScript, Mustache, Handlebars et JsRender.
    - L’éditeur HTML fournit Intellisense pour les fixations Knockout.
    - Support d’édition et de compilateur LESS pour permettre la construction de CSS dynamiques en utilisant LESS.
    - Pâte JSON comme une classe .NET. En utilisant cette commande de pâte spéciale pour coller JSON dans un fichier de code C ou VB.NET, et Visual Studio générera automatiquement des classes .NET déduites du JSON.
- Le support mobile d’émulateur ajoute des crochets d’extensibility afin que des émulateurs tiers puissent être installés comme VSIX. Les émulateurs installés apparaîtront dans le décrochage F5, de sorte que les développeurs peuvent prévisualiser leurs sites Web sur une variété d’appareils mobiles. En savoir plus sur cette fonctionnalité dans l’entrée de blog scott Hanselman sur [la nouvelle intégration BrowserStack avec Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>publication Web

- Les projets de sites Web ont maintenant la même expérience de publication que les projets d’applications Web, y compris la publication sur windows Azure Web Sites.
- Publier des &#8211; sélectifs pour un ou plusieurs fichiers que vous pouvez effectuer les actions suivantes (après publication à un point de terminaison Web Deploy) : 

    - Publier des fichiers sélectionnés.
    - Voir la différence entre un fichier local et un fichier distant.
    - Mettre à jour le fichier local avec le fichier distant ou mettre à jour le fichier à distance avec le fichier local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>modèles MVC ASP.NET

- Le nouveau modèle d’application Facebook facilite l’écriture d’applications de canevas Facebook. En quelques étapes simples, vous pouvez créer une application Facebook qui obtient les données d’un utilisateur connecté et s’intègre à ses amis. Le modèle comprend une nouvelle librairie qui se charge de tous les raccordements qu’implique la création d’une application Facebook, notamment l’authentification, les autorisations, l’accès aux données Facebook, etc. Pour plus d’informations sur [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)l’utilisation du modèle d’application Facebook voir .
- Un nouveau modèle MVC d’application à une page permet aux développeurs de créer des applications Web côté client interactives à l’aide des codes HTML 5, CSS 3, ainsi que des bibliothèques JavaScript prisées Knockout et jQuery, sur l’API Web ASP.NET. Le modèle comprend une application de liste "todo" qui démontre des pratiques courantes pour la construction d’une application JavaScript HTML5 qui utilise un serveur RESTful API. Vous pouvez en [https://www.asp.net/single-page-application](../../../single-page-application/index.md)savoir plus à .
- Vous pouvez maintenant créer un VSIX qui ajoute de nouveaux modèles au dialogue ASP.NET MVC New Project. Découvrez comment ici:[https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Le paquet FixedDisplayModes &#8211; modèles de projet MVC ont été mis à jour pour inclure le nouveau forfait NuGet 'FixedDisplayModes', qui contient une solution de contournement pour un bogue dans MVC 4. Pour plus d’informations sur le correctif contenu dans[https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)le paquet, se référer à ce blog ( ) de l’équipe MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET’API Web a été enrichie de plusieurs nouvelles fonctionnalités :

- ASP.NET Web API OData
- ASP.NET recherche de l’API Web
- ASP.NET Page d’aide à l’API Web

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData vous donne la flexibilité dont vous avez besoin pour construire des points de terminaison OData avec une riche logique d’affaires sur n’importe quelle source de données. Avec ASP.NET Web API OData vous contrôlez la quantité de sémantique OData que vous souhaitez exposer. ASP.NET Web API OData est inclus dans les modèles de projet ASP.NET MVC 4[http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)et est également disponible chez NuGet ( ).

ASP.NET Web API OData prend actuellement en charge les fonctionnalités suivantes :

- Activez la sémantique de requête d’OData en appliquant l’attribut [queryable].
- Validez facilement les requêtes OData et limitez l’ensemble des options, opérateurs et fonctions de requêtes pris en charge.
- Paramètre se lient directement à ODataQueryOptions pour obtenir une représentation abstraite de l’arbre syntaxe de la requête qui peut ensuite être validée et appliquée à un IQueryable ou IEnumerable.
- Activez le pagination axée sur le service et la génération de liens de page suivante en spécifiant des limites de résultat sur l’attribut [queryable].
- Demandez un décompte indéfecté du nombre total de ressources correspondantes à l’aide de $inlinecount.
- Contrôlez la propagation nulle.
- Tous les opérateurs en $filter.
- Inférer un modèle de données d’entité par convention ou personnaliser explicitement un modèle d’une manière similaire à Entity Framework Code-First.
- Exposer les ensembles d’entités en dérivé de EntitySetController.
- Des conventions simples et personnalisables pour exposer les propriétés de navigation, manipuler des liens et mettre en œuvre des actions OData.
- Itinéraire simplifié à l’aide de la méthode d’extension MapODataRoute.
- Prendre en charge la version en exposant plusieurs modèles EDM.
- Exposez les documents de service et les $metadata afin que vous puissiez générer des clients (.NET, Windows Phone, Windows Store, etc.) pour votre API Web.
- Prise en charge des formats OData Atom, JSON et JSON verbose.
- Créer, mettre à jour, partiellement mettre à jour (PATCH) et supprimer les entités.
- Interroger et manipuler les relations entre les entités.
- Créez des liens relationnels qui s’endlent vers vos itinéraires.
- Types complexes.
- Héritage de type entité.
- Propriétés de collecte.
- Enums.
- Actions OData.
- Construit sur la même base que WCF Data[http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)Services, à savoir ODataLib ( ).

Pour plus d’informations sur ASP.NET Web API OData voir [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET recherche de l’API Web

ASP.NET Web API Tracing intègre les données de traçage de vos API Web avec .NET Tracing. Il est maintenant activé par défaut dans le modèle de projet Web API. Les données de traçage de vos API Web sont envoyées à la fenêtre de sortie et sont mises à disposition via IntelliTrace. ASP.NET web API Tracing vous permet de retracer les informations sur votre API Web lorsqu’elles sont hébergées sur Windows Azure grâce à l’intégration avec [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Vous pouvez également installer et activer ASP.NET recherche d’API Web dans n’importe[http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)quelle application à l’aide du paquet NuGet de traçage de l’API Web ASP.NET ( ).

Pour plus d’informations sur la configuration et l’utilisation de ASP.NET Web API Tracing voir [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Page d’aide à l’API Web

La ASP.NET Page d’aide à l’API Web est maintenant incluse par défaut dans le modèle du projet Web API. La ASP.NET Page d’aide à l’API Web génère automatiquement de la documentation pour les API Web, y compris les paramètres HTTP, les méthodes HTTP prises en charge, les paramètres et les charges utiles de demande et de réponse par exemple. La documentation est automatiquement tirée des commentaires de votre code. Vous pouvez également ajouter la ASP.NET Page d’aide à l’API Web à n’importe[http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)quelle application utilisant le paquet NuGet de la page d’aide à l’API Web ASP.NET ( .

Pour plus d’informations sur la mise en place et [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)la personnalisation de la ASP.NET Page d’aide à l’API Web voir .

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>SignalR ASP.NET

ASP.NET SignalR facilite l’ajout de fonctionnalités Web en temps réel à votre application ASP.NET, en utilisant des photos WebS si elles sont disponibles et en revenant automatiquement à d’autres techniques lorsqu’elle ne l’est pas.

Pour plus d’informations sur l’utilisation de ASP.NET SignalR voir [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL conviviales ASP.NET

ASP.NET FriendlyURLs permet aux développeurs de formulaires Web de générer des URL plus propres (sans l’extension .aspx). Il nécessite peu ou pas de configuration et peut être utilisé avec des applications existantes ASP.NET v4.0. La fonction FriendlyURLs permet également aux développeurs d’ajouter plus facilement un support mobile à leurs applications, en soutenant le passage entre les vues de bureau et les vues mobiles.

Pour plus d’informations sur l’installation et l’utilisation de ASP.NET URL amicales voir [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et changements de rupture

Cette section décrit les problèmes connus et les changements de rupture qui sont dans la ASP.NET et Web Tools 2012.2 communiqué.

### <a name="installation-issues"></a>Problèmes d'installation

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Installations hors d’ordre de Visual Studio 2012

L’installation d’un SKU supplémentaire de Visual Studio 2012 après l’installation de la ASP.NET et Web Tools 2012.2 nécessitera une opération de réparation. Examinez la séquence suivante :

1. Installer Visual Studio 2012 Express pour le Web
2. Installer des outils ASP.NET et Web 2012.2
3. Installer Visual Studio 2012 Professionnel, Premium ou Ultimate

L’étape 2 n’entraînerait que l’installation de mises à jour pour Express pour le Web. Pour s’assurer que le SKU supplémentaire installé au cours de l’étape 3 contient la mise à jour, vous devrez réparer le ASP.NET et web Tools 2012.2 pour installer les mises à jour pour le dernier SKU installé. Cela s’applique également si les SKU dans les étapes 1 et 3 sont inversés.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installation de Microsoft ASP.NET et Web Tools 2012.2 lorsque Visual Studio est ouvert

Si VS est ouvert lors de l’installation de Microsoft ASP.NET et Web Tools 2012.2, Visual Studio pourrait se retrouver dans un mauvais état. Il est recommandé aux utilisateurs de fermer toutes les instances de Visual Studio avant de procéder à l’installation.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annulation de ASP.NET et Web Tools 2012.2 configuration au milieu de l’installation

L’annulation de ASP.NET et Web Tools 2012.2 configuration au milieu de l’installation laissera Visual Studio dans un mauvais état. Pour résoudre ce problème, suivez ces étapes : 

- Accédez à Ajout/Suppression de programmes.
- Désinstaller Microsoft ASP.NET et Web Tools 2012.2, s’il est présent.
- Reinstall Microsoft ASP.NET et Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Après avoir désinstallé ASP.NET et Web Tools 2012.2, il manque des modèles MVC 4 ASP.NET et des modèles de sites Web Razor v2

Le désinstallation ASP.NET et les outils Web 2012.2 désinstallera également tous les modèles de ASP.NET MVC 4 et Razor v2 du site Web de Visual Studio 2012.

La solution de contournement est de réparer votre installation Visual Studio 2012 pour réinstaller ASP.NET modèles de site Web MVC 4 et Razor v2.

### <a name="tooling-issues"></a>Problèmes d’outillage

#### <a name="nuget-error-reported-during-project-creation"></a>Erreur de NuGet signalée lors de la création d’un projet

Après l’installation de ASP.NET et d’outils Web 2012.2, vous pouvez voir l’erreur suivante lors de la création d’un projet MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

Le ASP.NET et Web Tools 2012.2 navires NuGet 2.1 et permettra de mettre à niveau l’extension dans Visual Studio 2012. Dans certains cas, l’installateur VSIX ne mettra pas à jour correctement le VSIX. Les étapes suivantes vous permettront de résoudre ce problème :

1. Démarrer Visual Studio 2012 en tant qu’administrateur
2. Allez à&gt;Outils- Extensions et Mises à jour et désinstaller NuGet.
3. Fermez Visual Studio
4. Naviguez vers le dossier d’installation ASP.NET et Web Tools 2012.2 :

    1. Pour Visual Studio 2012: **Program Files-Microsoft ASP.NET.NET Web Stack-Visual Studio 2012**
    2. Pour Visual Studio 2012 Express pour Le Web : **Program Files-Microsoft ASP.NET.NET Web Stack-Visual Studio Express 2012 pour Le Web**
5. Double clic sur le NuGet.Tools.vsix pour réinstaller NuGet

### <a name="web-api-issues"></a>Questions d’API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Problèmes de parsing dans $filter et dateTime littérales

Le parser OData URI ne parvient pas à analyser les littérations partielles de date correctement. Par exemple, $filter-début eq datetime'2012-12-31T12:00' ne parvient pas à analyser correctement. Une solution de contournement est d’utiliser le tout littéral, $filter-début eq datetime'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData ne prend pas en charge les noms de propriété insensibles aux cas.

OData ne prend pas en charge les noms de propriété insensibles aux cas dans les requêtes OData et le chemin odata. Voir workitems:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Si les utilisateurs ont boîtier différent sur le côté client javascript et côté serveur, ils seront probablement rencontrer ce problème. Cette question est par conception dans le protocole odata. Cependant, de nombreux utilisateurs signale ce problème. Pour le contourner, les utilisateurs doivent corriger leurs cas dans l’URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Les conventions de routage OData par défaut ne prennent pas en charge POST/PUT sur la propriété de navigation.

Les conventions de routage OData par défaut ne prennent pas en charge POST/PUT sur la propriété de navigation. Voir workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Nous manquons cette convention couramment utilisée dans les conventions par défaut.

Pour y contourner la route, les utilisateurs doivent étendre la nouvelle convention de routage pour la soutenir.

### <a name="facebook-template-issues"></a>Problèmes de modèle Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Le modèle d’application Facebook ne fonctionne qu’à l’aide de .NET 4.5

Vous devez sélectionner .NET 4.5 dans la liste de dépôt de cadre dans le dialogue du nouveau projet pour voir le modèle d’application Facebook dans ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Contrôleur de mise à jour en temps réel

Le modèle d’application Facebook permet à l’utilisateur de créer facilement un contrôleur d’API Web pour gérer les mises à jour en temps réel de Facebook. Si votre machine de développement est derrière NAT, votre contrôleur peut ne pas fonctionner sans autre configuration réseau. Voir ici pour plus de détails:[http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Les valeurs des chaînes de requête entrent en conflit avec les paramètres de Facebook OAuth

Les champs suivants entrent en conflit avec l’URL de rappel de Facebook OAuth. N’ajoutez pas vos propres valeurs de chaîne de requête\_avec\_les noms suivants : code, erreur, description d’erreur, raison d’erreur.

#### <a name="using-page-inspector-with-facebook-template"></a>Utilisation de l’inspecteur de page avec le modèle Facebook

Vous ne pouvez pas utiliser la fonction Page Inspector dans Visual Studio 2012 tout en débogage de votre application Facebook. L’inspecteur de la page ne prend actuellement pas en charge les iframes.

### <a name="single-page-application-template-issues"></a>Problèmes de modèle d’application à une page unique

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Avec la mise à jour JQuery 1.9/Knockout 2.2.1, lors de l’exécution par défaut du projet MVC SPA, le nouvel élément todo modifier l’événement de mise au point n’est pas géré correctement.

Avec la mise à jour JQuery 1.9/Knockout 2.2.1, lors de l’exécution par défaut du projet MVC SPA, le nouveau montage d’éléments todo n’entre plus la nouvelle boîte de modification d’élément todo après avoir entré le nouvel élément todo sur la liste de todo.

Pour effectuer la [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)référence de la solution de contournement, et faire un correctif similaire au code d’échantillon suivant :

Fichier todo.model.js  
 fonction todolist (données), ajouter ce qui suit :  
 **self.isSelected -ko.observable (faux);**

fonction todoList.prototype.addTodo, ajouter le texte noirci suivant:  
 **self.isSelected (vrai);**  
 self.newTodoTitle();&quot;&quot;

Fichier index.cshtml, ajouter le texte noirci suivant:  
 &lt;formulaire de liaison&quot;de donnéesMD soumettre : addTodo&quot;&gt;  
 &lt;Classe&quot;d’entréemd&quot; addTodo type&quot;'&quot;valeur de données de texte:&quot; newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: 'blur: addTodo '&quot; /&gt;  
 &lt;/forme&gt;
