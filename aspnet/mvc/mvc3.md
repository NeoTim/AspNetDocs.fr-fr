---
uid: mvc/mvc3
title: ASP.NET MVC
author: rick-anderson
description: (inclut la mise à jour des outils d’avril 2011) ASP.NET MVC 3 est un cadre pour la construction d’applications Web évolutives et basées sur des normes à l’aide de modèles de conception bien établis...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 6fe734dc0d0106bb346df154e1e03f97b307567a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675136"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC

> *(inclut la mise à jour des outils d’avril 2011)*
> 
> ASP.NET MVC 3 est un cadre pour la construction d’applications Web évolutives et basées sur des normes en utilisant des modèles de conception bien établis et la puissance de ASP.NET et du cadre .NET.
> 
> Il installe côte à côte avec ASP.NET MVC 2, alors commence à l’utiliser aujourd’hui!
> 
> Téléchargez [l’installateur ici](https://microsoft.com/download/details.aspx?id=4211)

## <a name="top-features"></a>Caractéristiques principales

- Système intégré d’échafaudage extensible via NuGet
- Modèles de projets HTML 5 activés
- Vues expressives, y compris le nouveau moteur Razor View
- Crochets puissants avec injection de dépendance et filtres d’action mondiale
- Support JavaScript riche avec JavaScript discret, validation jQuery, et liaison JSON
- *Lire la liste complète des fonctionnalités [ci-dessous](#overview)*

## <a name="top-links"></a>Liens haut

Quoi de neuf dans ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 Libéré](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express et Orchard - The Microsoft January Web Release in Context](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Annonce de la sortie de ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notes de sortie pour ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation et aide

- Installer ASP.NET MVC 3 à l’aide de [l’installateur de plate-forme Web (recommandé)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installer ASP.NET MVC 3 à l’aide de [l’installateur exécutable](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installer [ASP.NET MVC 3 pour Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lire [l’Intro pour ASP.NET tutoriel MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenez de l’aide et discutez ASP.NET MVC 3 dans les [forums](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 Overview

ASP.NET MVC 3 s’appuie sur ASP.NET MVC 1 et 2, ajoutant de grandes fonctionnalités qui simplifient votre code et permettent une plus grande extensibility. Ce sujet donne un aperçu de bon nombre des nouvelles fonctionnalités qui sont incluses dans cette version, organisées dans les sections suivantes :

- [Échafaudage extensible avec intégration MvcScaffold](#BM_MvcScaffolding)
- [Modèles de projets HTML 5 activés](#BM_HTML5)
- [Le moteur Razor View](#BM_TheRazorViewEngine)
- [Prise en charge des moteurs à vue multiple](#BM_Support_for_Multiple_View_Engines)
- [Améliorations de contrôleur](#BM_Controller_Improvements)
- [JavaScript et Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Améliorations de validation de modèle](#BM_Model_Validation_Improvements)
- [Améliorations de l’injection de dépendance](#BM_Dependency_Injection_Improvements)
- [Autres nouvelles fonctionnalités](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Échafaudage extensible avec intégration MvcScaffold

Le nouveau système d’échafaudage facilite la prise en charge et le démarrage de l’utilisation de manière productive si vous êtes entièrement nouveau dans le cadre, et pour automatiser les tâches de développement communes si vous êtes expérimenté et savez déjà ce que vous faites.

Ceci est soutenu par le nouveau paquet *d’échafaudage* NuGet appelé **MvcScaffolding**. Le terme « échafaudage » est utilisé par de nombreuses technologies logicielles pour signifier « générer rapidement un contour de base de votre logiciel que vous pouvez ensuite modifier et personnaliser ». Le paquet d’échafaudages que nous créons pour ASP.NET MVC est très bénéfique dans plusieurs scénarios :

- **Si vous apprenez ASP.NET MVC pour la première fois,** car il vous donne un moyen rapide d’obtenir un peu utile, le code de travail, que vous pouvez ensuite modifier et adapter en fonction de vos besoins. Il vous sauve du traumatisme de regarder une page blanche et n’ayant aucune idée par où commencer!
- **Si vous connaissez ASP.NET bien MVC et explorez maintenant une nouvelle technologie add-on** comme un mapper objet-relationnel, un moteur de vue, une bibliothèque d’essai, etc, parce que le créateur de cette technologie peut avoir également créé un paquet d’échafaudage pour elle.
- **Si votre travail implique de créer à plusieurs reprises des classes ou des fichiers similaires d’une certaine sorte,** parce que vous pouvez créer des échafaudages personnalisés qui la sortie des montages de test, scripts de déploiement, ou tout ce dont vous avez besoin. Tout le monde dans votre équipe peut utiliser vos échafaudages personnalisés, aussi.

Voici d’autres caractéristiques de MvcScaffolding :

- Soutien aux projets C et VB
- Prise en charge des moteurs de vue Razor et ASPX
- Prend en charge l’échafaudage dans ASP.NET zones MVC et en utilisant des dispositions/maîtres de vue personnalisés
- Vous pouvez facilement personnaliser la sortie en éditant des modèles T4
- Vous pouvez ajouter des échafaudages entièrement nouveaux à l’aide de la logique PowerShell personnalisée et des modèles T4 personnalisés. Ces paramètres (et tous les paramètres personnalisés que vous les avez donnés) apparaissent automatiquement dans la liste d’achèvement de l’onglet de la console.
- Vous pouvez obtenir des paquets NuGet contenant des échafaudages supplémentaires pour différentes technologies (p. ex., il y en a une preuve de concept pour LINQ à SQL maintenant) et les mélanger et les assortir

La mise à jour ASP.NET MVC 3 Tools comprend un excellent support Visual Studio pour ce système d’échafaudage, tels que :

- Add Controller Dialog prend désormais en charge l’échafaudage automatique complet des actions de contrôleurs de création, de lecture, de mise à jour et de suppression et des vues correspondantes. Par défaut, cet échafaudage le code d’accès aux données à l’aide du code EF First.
- Ajouter Controller Dialog prend en charge les *échafaudages extensibles* via des paquets NuGet tels que *MvcScaffolding*. Cela permet de brancher des échafaudages personnalisés dans le dialogue qui vous permettrait de créer des échafaudages pour d’autres technologies d’accès aux données telles que NHibernate ou même JET avec ODBCDirect si vous êtes si incliné!

Pour plus d’informations sur les échafaudages dans ASP.NET MVC 3, voir les ressources suivantes :

- La série d’après de Steve Sanderson, y compris: 

    1. [Introduction : Échafaudez votre projet ASP.NET MVC 3 avec le package MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilisation standard : Cas et options d’utilisation typiques](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relations individuelles à plusieurs](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Actions d’échafaudage et tests unitaires](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Dépassement des modèles T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ce post: Création d’échafaudages personnalisés](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman post de sa session PDC 2010 [Construire un blog avec Microsoft "Unnamed Package of Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notes de sortie MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>MODÈLES de projet HTML 5

Le dialogue new Project comprend une case à cocher activer les versions HTML 5 des modèles de projet. Ces modèles tirent parti de Modernizr 1.7 pour fournir un support de compatibilité pour HTML 5 et CSS 3 dans les navigateurs de bas niveau.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Le moteur Razor View

ASP.NET MVC 3 est livré avec un nouveau moteur de vue nommé Razor qui offre les avantages suivants:

- La syntaxe de rasoir est propre et concise, nécessitant un nombre minimum de frappes.
- Razor est facile à apprendre, en partie parce qu’il est basé sur des langues existantes comme le CMD et visual Basic.
- Visual Studio comprend IntelliSense et coloration de code pour la syntaxe Razor.
- Les vues razor peuvent être testées sans exiger que vous exéciez l’application ou lancez un serveur web.

Voici quelques nouvelles fonctionnalités de Razor :

- `@model`syntaxe pour spécifier le type passé à la vue.
- `@* *@`syntaxe commentaire.
- La possibilité de spécifier les défauts (tels que `layoutpage`) une fois pour un site entier.
- La `Html.Raw` méthode pour afficher le texte sans l’encoder HTML.
- Prise en charge du partage du code entre plusieurs vues*\_(viewstart.cshtml* ou * \_viewstart.vbhtml* fichiers).

Razor comprend également de nouveaux aides HTML, tels que les suivants :

- `Chart`. Rend un graphique, offrant les mêmes caractéristiques que le contrôle graphique dans ASP.NET 4.
- `WebGrid`. Rend une grille de données, avec des fonctionnalités de pagination et de tri.
- `Crypto`. Utilise des algorithmes de hachage pour créer des mots de passe bien salés et hachés.
- `WebImage`. Rend une image.
- `WebMail`. Envoie un message électronique.

Pour plus d’informations sur Razor, voir les ressources suivantes :

- [Le blog de Scott Guthrie présentant Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Le blog de Scott Guthrie @model présentant le mot-clé](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Le blog de Scott Guthrie présentant des mises en page Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API Référence rapide](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notes de sortie MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Prise en charge des moteurs à vue multiple

La boîte de dialogue **Add View** dans ASP.NET MVC 3 vous permet de choisir le moteur de vue avec lequel vous souhaitez travailler, et la boîte de dialogue **New Project** vous permet de spécifier le moteur de vue par défaut pour un projet. Vous pouvez choisir le moteur Web Forms view (ASPX), Razor, ou un moteur de vue open-source tels que [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Améliorations de contrôleur

### <a name="global-action-filters"></a>Filtres d’action mondiales

Parfois, vous voulez effectuer la logique soit avant qu’une méthode d’action s’exécute ou après une méthode d’action s’exécute. Pour ce faire, ASP.NET MVC 2 a fourni des filtres d’action. Les filtres d’action sont des attributs personnalisés qui fournissent un moyen déclaratif d’ajouter un comportement avant action et post-action à des méthodes d’action spécifiques du contrôleur. Cependant, dans certains cas, vous pouvez spécifier un comportement pré-action ou post-action qui s’applique à toutes les méthodes d’action. MVC 3 vous permet de spécifier `GlobalFilters` des filtres globaux en les ajoutant à la collection. Pour plus d’informations sur les filtres d’action mondiale, voir les ressources suivantes :

- [Le blog de Scott Guthrie sur le MVC 3 Aperçu](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrage dans ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nouvelle propriété "ViewBag"

Les contrôleurs MVC `ViewData` 2 prennent en charge une propriété qui vous permet de transmettre des données à un modèle de vue à l’aide d’une API de dictionnaire en retard. Dans MVC 3, vous pouvez également utiliser `ViewBag` une syntaxe un peu plus simple avec la propriété pour accomplir le même but. Par exemple, au `ViewData["Message"]="text"`lieu d’écrire , vous pouvez écrire `ViewBag.Message="text"`. Vous n’avez pas besoin de définir des `ViewBag` classes fortement typées pour utiliser la propriété. Parce qu’il s’agit d’une propriété dynamique, vous pouvez plutôt simplement obtenir ou définir des propriétés et il les résoudra dynamiquement au moment de l’exécution. En `ViewBag` interne, les propriétés sont `ViewData` stockées sous forme de paires de noms/valeurs dans le dictionnaire. (Remarque : dans la plupart des versions pré-version de MVC 3, la `ViewBag` propriété a été nommée la `ViewModel` propriété.)

### <a name="new-actionresult-types"></a>Nouveaux types "ActionResult"

Les `ActionResult` types suivants et les méthodes d’assistance correspondantes sont nouveaux ou améliorés dans MVC 3 :

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Renvoie un code d’état HTTP 404 au client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retourne une redirection temporaire (code d’état HTTP 302) ou une redirection permanente (code d’état HTTP 301), selon un paramètre Boolean. En conjonction avec ce changement, la classe [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) a `RedirectPermanent`maintenant `RedirectToRoutePermanent`trois `RedirectToActionPermanent`méthodes pour effectuer des redirections permanentes: , , et . Ces méthodes renvoient `RedirectResult` une `Permanent` instance `true`de la propriété réglée à .
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Renvoie un code de statut HTTP spécifié par l’utilisateur.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Améliorations JavaScript et Ajax

Par défaut, Ajax et les aides de validation dans MVC 3 utilisent une approche JavaScript discrète. JavaScript discret évite d’injecter JavaScript en ligne en HTML. Cela rend votre HTML plus petit et moins encombré, et il est plus facile d’échanger ou de personnaliser les bibliothèques JavaScript. Les aides de validation dans MVC 3 utilisent également le `jQueryValidate` plugin par défaut. Si vous voulez un comportement MVC 2, vous pouvez désactiver JavaScript discret à l’aide d’un paramètre de fichiers *web.config.* Pour plus d’informations sur les améliorations JavaScript et Ajax, voir les ressources suivantes:

- [Introduction de base à JavaScript discret sur le site Wikipédia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson’s Unobtrusive JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson’s Unobtrusive JavaScript Validation Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Création d’une application MVC 3 avec Razor et JavaScript discret](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial sur le site ASP.NET)
- [Notes de sortie MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validation côté client activée par défaut

Dans les versions antérieures de MVC, `Html.EnableClientValidation` vous devez appeler explicitement la méthode à partir d’une vue afin de permettre la validation côté client. Dans MVC 3, cela n’est plus nécessaire car la validation côté client est activée par défaut. (Vous pouvez désactiver cela à l’aide d’un paramètre dans le fichier *web.config.)*

Pour que la validation côté client fonctionne, vous devez toujours faire référence aux bibliothèques de validation jQuery et jQuery appropriées sur votre site. Vous pouvez héberger ces bibliothèques sur votre propre serveur ou les référencer à partir d’un réseau de diffusion de contenu (CDN) comme les CDN de Microsoft ou Google.

### <a name="remote-validator"></a>Validateur à distance

ASP.NET MVC 3 prend en charge la nouvelle classe [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) qui vous permet de profiter du support de validation à distance du plug-in de la validation à distance de la validation de jQuery Validation. Cela permet à la bibliothèque de validation côté client d’appeler automatiquement une méthode personnalisée que vous définissez sur le serveur afin d’effectuer une logique de validation qui ne peut être faite côté serveur.

Dans l’exemple `Remote` suivant, l’attribut précise que la `UserNameAvailable` validation `UsersController` du client appellera une action nommée sur le groupe afin de valider le `UserName` champ.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

L’exemple suivant montre le contrôleur correspondant.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Pour plus d’informations `Remote` sur la façon d’utiliser l’attribut, voir [Comment : Implémenter la validation à distance dans ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) dans la bibliothèque MSDN.

### <a name="json-binding-support"></a>Soutien contraignant JSON

ASP.NET MVC 3 comprend un support contraignant JSON intégré qui permet aux méthodes d’action de recevoir des données codées par JSON et de les lier aux paramètres de méthode d’action. Cette capacité est utile dans les scénarios impliquant des modèles clients et la liaison des données. (Les modèles clients vous permettent de formater et d’afficher un seul élément de données ou un ensemble d’éléments de données en utilisant des modèles qui s’exécutent sur le client.) MVC 3 vous permet de connecter facilement les modèles clients avec des méthodes d’action sur le serveur qui envoient et reçoivent des données JSON. Pour plus d’informations sur le support contraignant JSON, voir la section **Améliorations JavaScript et AJAX** du blog [MVC 3 Preview de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Améliorations de validation de modèle

### <a name="dataannotations-metadata-attributes"></a>Attributs de métadonnées "DataAnnotations"

ASP.NET MVC 3 prend `DataAnnotations` en charge les `DisplayAttribute`attributs des métadonnées tels que .

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

La `ValidationAttribute` classe a été améliorée dans le cadre `IsValid` .NET 4 pour soutenir une nouvelle surcharge qui fournit plus d’informations sur le contexte de validation actuel, comme quel objet est validé. Cela permet des scénarios plus riches où vous pouvez valider la valeur actuelle en fonction d’une autre propriété du modèle. Par exemple, `CompareAttribute` le nouvel attribut vous permet de comparer les valeurs de deux propriétés d’un modèle. Dans l’exemple `ComparePassword` suivant, la `Password` propriété doit correspondre au terrain afin d’être valide.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Validation Interfaces

[L’interface IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) vous permet d’effectuer la validation au niveau du modèle, et elle vous permet de fournir des messages d’erreur de validation spécifiques à l’état du modèle global, ou entre deux propriétés dans le modèle. MVC 3 récupère maintenant `IValidatableObject` les erreurs de l’interface lors de la liaison du modèle, et signale ou met automatiquement en évidence les champs affectés dans une vue à l’aide des aides html intégrées.

[L’interface IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) permet à ASP.NET MVC de découvrir à l’heure de l’exécution si un validateur a un support pour la validation du client. Cette interface a été conçue pour qu’elle puisse être intégrée à une variété de cadres de validation.

Pour plus d’informations sur les interfaces de validation, consultez la section Améliorations de validation des **modèles** du billet de [blog MVC 3 Preview de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Toutefois, notez que la référence à "IValidateObject" dans le blog devrait être "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Améliorations de l’injection de dépendance

ASP.NET MVC 3 offre un meilleur soutien pour l’application de l’injection de dépendance (DI) et pour l’intégration avec l’injection de dépendance ou l’inversion des conteneurs de contrôle (CIO). Le soutien à l’DI a été ajouté dans les domaines suivants :

- Contrôleurs (enregistrement et injection d’usines de contrôleurs, contrôleurs d’injection).
- Vues (enregistrement et injection de moteurs de vue, injection de dépendances dans les pages de vision).
- Filtres d’action (localisation et injection de filtres).
- Les liants modèles (enregistrement et injection).
- Fournisseurs de validation de modèles (enregistrement et injection).
- Modèles de métadonnées (enregistrement et injection).
- Fournisseurs de valeur (enregistrement et injection).

MVC 3 prend en charge la bibliothèque de localisation des `IServiceLocator` services [communs](https://github.com/unitycontainer/commonservicelocator) et tout conteneur DI qui prend en charge l’interface de cette bibliothèque. Il prend également `IDependencyResolver` en charge une nouvelle interface qui facilite l’intégration des cadres DI.

Pour plus d’informations sur DI dans MVC 3, voir les ressources suivantes:

- [Brad Wilson série de billets de blog sur l’emplacement de service](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notes de sortie MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Autres nouvelles fonctionnalités

### <a name="nuget-integration"></a>Intégration NuGet

ASP.NET MVC 3 installe et permet automatiquement à NuGet dans le cadre de sa configuration. NuGet est un gestionnaire de forfait open-source gratuit qui facilite la recherche, l’installation et l’utilisation de bibliothèques et d’outils .NET dans vos projets. Il fonctionne avec tous les types de projets Visual Studio (y compris ASP.NET Web Forms et ASP.NET MVC).

NuGet permet aux développeurs qui maintiennent des projets open source (par exemple, des projets comme Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks et Elmah) de regrouper leurs bibliothèques et de les enregistrer dans une galerie en ligne. Il est alors facile pour les développeurs .NET qui veulent utiliser l’une de ces bibliothèques pour trouver le paquet et l’installer dans les projets qu’ils travaillent sur.

Avec la mise à jour ASP.NET 3 outils, les modèles de projet comprennent des bibliothèques JavaScript préinstallées, de sorte qu’ils sont updatable via NuGet. Entity Framework Code First est également préinstallé comme un paquet NuGet.

Pour plus d'informations sur NuGet, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Caching de sortie à page partielle

ASP.NET MVC a pris en charge la mise en cache de sortie des réponses pleine page depuis la version 1. MVC 3 prend également en charge la mise en cache de sortie à une page partielle, ce qui vous permet de mettre facilement en cache les régions ou les fragments d’une réponse. Pour plus d’informations sur la mise en cache, consultez la section **De la mise en cache** partielle de la page du billet de blog de Scott [Guthrie sur le candidat de sortie MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) et la section **De cachage de sortie d’action pour enfants** des notes de sortie [MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Contrôle granulaire sur la validation des demandes

ASP.NET MVC a une validation de demande intégrée qui aide automatiquement à se protéger contre les attaques d’injection XSS et HTML. Cependant, parfois, vous souhaitez désactiver explicitement la validation des demandes, par exemple si vous souhaitez laisser les utilisateurs poster du contenu HTML (par exemple, dans les entrées de blog ou le contenu CMS). Vous pouvez maintenant ajouter un attribut [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) aux modèles ou afficher des modèles pour désactiver la validation des demandes sur une base par propriété lors de la liaison du modèle. Pour plus d’informations sur la validation des demandes, consultez les ressources suivantes :

- La section **JavaScript et Validation discrète** dans [le billet de blog de Scott Guthrie sur le candidat de sortie MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notes de sortie MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible "Nouveau Projet" Dialog Box

Dans ASP.NET MVC 3, vous pouvez ajouter des modèles de projet, des moteurs de vue et des cadres de projets d’essai unitaire à la boîte de dialogue **du nouveau projet.**

### <a name="template-scaffolding-improvements"></a>Modèles d’améliorations d’échafaudage

ASP.NET modèles d’échafaudage MVC 3 font un meilleur travail d’identification des propriétés clés primaires sur les modèles et de les manipuler de façon appropriée que dans les versions antérieures de MVC. (Par exemple, les modèles d’échafaudage s’assurent maintenant que la clé principale n’est pas échafaudée comme champ de forme modifiable.)

Par défaut, les échafaudages `Html.EditorFor` Créer et modifier `Html.TextBoxFor` utilisent maintenant l’aide au lieu de l’aide. Cela améliore le support pour les métadonnées sur le modèle sous la forme d’attributs d’annotation de données lorsque la boîte de dialogue **Add View** génère une vue.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nouvelles surcharges pour "Html.LabelFor" et "Html.LabelForModel"

De nouvelles surcharges de méthode `LabelFor` `LabelForModel` ont été ajoutées pour les méthodes et les méthodes d’aide. Les nouvelles surcharges vous permettent de spécifier ou de remplacer le texte de l’étiquette.

### <a name="sessionless-controller-support"></a>Support de contrôleur sans session

Dans ASP.NET MVC 3, vous pouvez indiquer si vous voulez qu’un classe de contrôleur utilise l’état de session, et si oui, si l’état de session doit être lu/ écrit ou lu uniquement. Pour plus d’informations sur le support du contrôleur sans session, voir [MVC 3 Notes de sortie](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nouvelle classe "AdditionalMetadataAttribute"

Vous pouvez utiliser l’attribut [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) pour remplir le `ModelMetadata.AdditionalValues` dictionnaire d’une propriété modèle. Par exemple, si un modèle de vue a une propriété qui ne doit être affichée qu’à un administrateur, vous pouvez annoter cette propriété comme indiqué dans l’exemple suivant :

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Ces métadonnées sont mises à la disposition de n’importe quel modèle d’affichage ou d’éditeur lorsqu’un modèle de vue de produit est rendu. C’est à vous d’interpréter les informations sur les métadonnées.

### <a name="accountcontroller-improvements"></a>Améliorations de AccountController

Le AccountController dans le modèle de projet Internet a été grandement amélioré.

### <a name="new-intranet-project-template"></a>Nouveau modèle de projet Intranet

Un nouveau modèle de projet Intranet est inclus qui permet l’authentification de Windows et supprime le AccountController.
