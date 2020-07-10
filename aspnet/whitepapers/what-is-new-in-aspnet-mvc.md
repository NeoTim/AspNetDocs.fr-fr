---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Nouveautés de ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2. Ce document est également disponible au téléchargement.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 1a0a29241d8afecd295b11013b27621b21c9ed52
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "86162703"
---
# <a name="whats-new-in-aspnet-mvc-2"></a>Nouveautés d’ASP.NET MVC 2

> Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2. Ce document est également disponible au [Téléchargement](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)

[Présentation](#_TOC1)   
[Mise à niveau d’un projet ASP.NET MVC 1,0 vers ASP.NET MVC 2](#_TOC2)   
[Nouvelles fonctionnalités](#_TOC3)   
[Assistances basées sur un modèle](#_TOC3_1)   
[Régions](#_TOC3_2)   
[Prise en charge des contrôleurs asynchrones](#_TOC3_3)   
[Prise en charge de DefaultValueAttribute dans les paramètres de méthode d’action](#_TOC3_4)   
[Prise en charge de la liaison de données binaires avec des classeurs de modèles](#_TOC3_5)   
[Classes ModelMetadata et ModelMetadataProvider](#_TOC3_6)   
[Prise en charge des attributs DataAnnotations](#_TOC3_7)   
[Fournisseurs de validateur de modèle](#_TOC3_8)   
[Validation côté client](#_TOC3_9)   
[Nouveaux extraits de code pour Visual Studio 2010](#_TOC3_10)   
[Nouveau filtre d’action RequireHttpsAttribute](#_TOC3_11)   
[Substitution du verbe de méthode HTTP](#_TOC3_12)   
[Nouvelle classe HiddenInputAttribute pour les applications auxiliaires basées sur des modèles](#_TOC3_13)   
[La méthode d’assistance HTML. ValidationSummary peut afficher des erreurs au niveau du modèle](#_TOC3_14)   
Les [modèles T4 dans Visual Studio génèrent du code spécifique à la version cible des](#_TOC3_15)[améliorations apportées](#_TOC4) à l’API .NET Framework  
[Dernières modifications](#_TOC5)  
[AVERTISSEMENT](#_TOC6)  

## <a name="introduction"></a><a id="_TOC1"></a>Présentation

ASP.NET MVC 2 s’appuie sur ASP.NET MVC 1,0 et introduit un grand nombre d’améliorations et de fonctionnalités axées sur l’augmentation de la productivité. Cette version est compatible avec ASP.NET MVC 1,0, de sorte que toutes vos connaissances, compétences, code et extensions pour ASP.NET MVC 1,0 continuent à s’appliquer.

Pour plus d’informations sur ASP.NET MVC, visitez les ressources suivantes :

- [Documentation ASP.NET MVC sur MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Site Web ASP.NET MVC](https://asp.net/mvc/)
- [Forums ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a name="upgrading-an-aspnet-mvc-10-project-to-aspnet-mvc-2"></a><a id="_TOC2"></a>Mise à niveau d’un projet ASP.NET MVC 1,0 vers ASP.NET MVC 2

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1,0 sur le même serveur, ce qui permet aux développeurs d’applications de choisir quand mettre à niveau une application ASP.NET MVC 1,0 vers ASP.NET MVC 2. Pour plus d’informations sur la mise à niveau, consultez le document [mise à niveau d’une Application ASP.net mvc 1,0 vers ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a name="new-features"></a><a id="_TOC3"></a>Nouvelles fonctionnalités

Cette section décrit les fonctionnalités qui ont été introduites dans la version MVC 2.

### <a name="templated-helpers"></a><a id="_TOC3_1"></a>Assistances basées sur un modèle

Les applications auxiliaires basées sur un modèle vous permettent d’associer automatiquement des éléments HTML pour les modifier et les afficher avec des types de données. Par exemple, quand des données de type System. DateTime s’affichent dans une vue, un élément d’interface utilisateur de sélecteur de dates peut être rendu automatiquement. Cela est similaire à la façon dont les modèles de champ fonctionnent dans ASP.NET Dynamic Data. Pour plus d’informations, consultez [utilisation des applications auxiliaires basées sur des modèles pour afficher des données](https://go.microsoft.com/fwlink/?LinkId=159062) sur le site Web MSDN.

### <a name="areas"></a><a id="_TOC3_2"></a>Régions

Les zones vous permettent d’organiser un grand projet en plusieurs sections plus petites afin de gérer la complexité d’une application Web de grande taille. Chaque section (« Area ») représente généralement une section distincte d’un site Web de grande taille et est utilisée pour regrouper des ensembles de contrôleurs et de vues associés. Pour plus d’informations, consultez [procédure pas à pas : organisation d’une Application ASP.NET MVC par zones](https://go.microsoft.com/fwlink/?LinkId=158978) sur le site Web MSDN.

Pour créer une nouvelle zone, dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, cliquez sur Ajouter, puis sur zone. Cela affiche une boîte de dialogue qui vous invite à entrer le nom de la zone. Une fois que vous avez entré le nom de la zone, Visual Studio ajoute une nouvelle zone au projet.

L’illustration suivante montre un exemple de disposition pour un projet avec deux zones, admin et blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Lorsque vous créez une zone, Visual Studio ajoute une classe qui dérive de AreaRegistration à chaque zone. Cette classe est requise pour inscrire la zone et ses itinéraires, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Le modèle de projet par défaut pour ASP.NET MVC 2 comprend un appel à la méthode RegisterAllAreas dans le code du fichier global. asax. Cette méthode inscrit chaque zone dans le projet en recherchant tous les types qui dérivent de la classe AreaRegistration, en instanciant une instance du type, puis en appelant la méthode RegisterArea sur l’instance. L’exemple suivant montre comment procéder.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Si vous ne spécifiez pas l’espace de noms dans la méthode RegisterArea en appelant le contexte. Namespaces. Add, la méthode, l’espace de noms de la classe d’inscription est utilisé par défaut.

### <a name="support-for-asynchronous-controllers"></a><a id="_TOC3_3"></a>Prise en charge des contrôleurs asynchrones

ASP.NET MVC 2 permet désormais aux contrôleurs de traiter les demandes de façon asynchrone. Cela peut entraîner des gains de performances en permettant aux serveurs qui appellent fréquemment des opérations bloquantes (telles que les demandes réseau) d’appeler à la place des homologues non bloquants. Pour plus d’informations, consultez la rubrique [utilisation d’un contrôleur asynchrone dans ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) sur MSDN.

### <a name="support-for-defaultvalueattribute-in-action-method-parameters"></a><a id="_TOC3_4"></a>Prise en charge de DefaultValueAttribute dans les paramètres de méthode d’action

La classe System. ComponentModel. DefaultValueAttribute permet de fournir une valeur par défaut pour le paramètre d’argument à une méthode d’action. Par exemple, supposons que l’itinéraire par défaut suivant est défini :

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Supposons également que le contrôleur et la méthode d’action suivants sont définis :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

L’une des URL de requête suivantes appellera la méthode d’action de vue définie dans l’exemple précédent.

- /Article/View/123
- /Article/View/123 ? page = 1 (en réalité, le même que la demande précédente)
- /Article/View/123 ? page = 2

Sans l’attribut DefaultValueAttribute, la première URL de la liste précédente ne fonctionnerait pas, car l’argument de page est un type valeur non Nullable dont la valeur n’a pas été fournie.

Si votre code est écrit en Visual Basic 2010 ou Visual C# 2010, vous pouvez utiliser des paramètres facultatifs à la place de l’attribut DefaultValueAttribute, comme indiqué dans l’exemple suivant :

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a name="support-for-binding-binary-data-with-model-binders"></a><a id="_TOC3_5"></a>Prise en charge de la liaison de données binaires avec des classeurs de modèles

Il existe deux nouvelles surcharges de l’assistance HTML. Hidden qui encodent les valeurs binaires en tant que chaînes codées en base 64 :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Une utilisation classique consiste à incorporer un horodatage pour un objet dans la vue. Par exemple, votre application peut inclure l’objet Product suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un formulaire de modification peut afficher la propriété TimeStamp dans le formulaire, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Ce balisage rend un élément d’entrée masqué avec la valeur d’horodatage sous la forme d’une chaîne encodée en base 64 qui ressemble à l’exemple suivant :

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Ce formulaire peut être publié dans une méthode d’action qui a un argument de type Product, comme illustré dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Dans la méthode d’action, la propriété TimeStamp est remplie correctement, car la chaîne encodée en base 64 publiée est convertie en un tableau d’octets.

### <a name="modelmetadata-and-modelmetadataprovider-classes"></a><a id="_TOC3_6"></a>Classes ModelMetadata et ModelMetadataProvider

La classe ModelMetadataProvider fournit une abstraction pour obtenir les métadonnées du modèle dans une vue. MVC 2 comprend un fournisseur par défaut qui rend disponibles les métadonnées exposées par les attributs dans l’espace de noms System. ComponentModel. DataAnnotations. Il est possible de créer des fournisseurs de métadonnées qui fournissent des métadonnées à partir d’autres magasins de données, tels que des bases de données ou des fichiers XML.

La classe ViewDataDictionary expose un objet ModelMetadata qui contient les métadonnées extraites du modèle par la classe ModelMetadataProvider. Cela permet aux applications auxiliaires basées sur des modèles d’utiliser ces métadonnées et d’ajuster leur sortie en conséquence.

Pour plus d’informations, consultez la documentation pour les classes [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) et [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) .

### <a name="support-for-dataannotations-attributes"></a><a id="_TOC3_7"></a>Prise en charge des attributs DataAnnotations

ASP.NET MVC 2 prend en charge l’utilisation des attributs de validation RangeAttribute, RequiredAttribute, StringLengthAttribute et RegexAttribute (définis dans l’espace de noms System. ComponentModel. DataAnnotations) quand vous établissez une liaison à un modèle afin de fournir la validation d’entrée.

Pour plus d’informations, consultez [Comment : valider des données de modèle à l’aide d’attributs DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) sur le site Web MSDN. Un exemple de projet illustrant l’utilisation de ces attributs est disponible en téléchargement à l’adresse [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753) .

### <a name="model-validator-providers"></a><a id="_TOC3_8"></a>Fournisseurs de validateur de modèle

La classe de fournisseur de validation de modèle représente une abstraction qui fournit la logique de validation pour le modèle. ASP.NET MVC inclut un fournisseur par défaut basé sur les attributs de validation inclus dans l’espace de noms System. ComponentModel. DataAnnotations. Vous pouvez également créer vos propres fournisseurs de validation qui définissent des règles de validation personnalisées et des mappages personnalisés de règles de validation pour le modèle. Pour plus d’informations, consultez la documentation de la classe [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) .

### <a name="client-side-validation"></a><a id="_TOC3_9"></a>Validation côté client

La classe de fournisseur de validateur de modèle expose des métadonnées de validation au navigateur sous la forme de données sérialisées JSON qui peuvent être consommées par une bibliothèque de validation côté client. ASP.NET MVC 2 comprend une bibliothèque de validation client et un adaptateur qui prend en charge les attributs de validation d’espace de noms DataAnnotations notés précédemment. La classe de fournisseur vous permet également d’utiliser d’autres bibliothèques de validation client en écrivant un adaptateur qui traite les données JSON et appelle la bibliothèque de remplacement.

### <a name="new-code-snippets-for-visual-studio-2010"></a><a id="_TOC3_10"></a>Nouveaux extraits de code pour Visual Studio 2010

Un ensemble d’extraits de code HTML pour ASP.NET MVC 2 est installé avec Visual Studio 2010. Pour afficher la liste de ces extraits, dans le menu Outils, sélectionnez Gestionnaire des extraits de code. Pour la langue, sélectionnez HTML, et pour emplacement, sélectionnez ASP.NET MVC 2. Pour plus d’informations sur l’utilisation des extraits de code, consultez la documentation de Visual Studio.

### <a name="new-requirehttpsattribute-action-filter"></a><a id="_TOC3_11"></a>Nouveau filtre d’action RequireHttpsAttribute

ASP.NET MVC 2 comprend une nouvelle classe RequireHttpsAttribute qui peut être appliquée aux méthodes et aux contrôleurs d’action. Par défaut, le filtre redirige une requête non-SSL (HTTP) vers l’équivalent SSL (HTTPs).

### <a name="overriding-the-http-method-verb"></a><a id="_TOC3_12"></a>Substitution du verbe de méthode HTTP

Lorsque vous créez un site Web à l’aide du style architectural REST, des verbes HTTP sont utilisés pour déterminer l’action à effectuer pour une ressource. REST requiert que les applications prennent en charge l’ensemble des verbes HTTP courants, y compris les verbes d’extraction, de placement, de publication et de suppression.

ASP.NET MVC 2 comprend de nouveaux attributs que vous pouvez appliquer aux méthodes d’action et à la syntaxe compacte des fonctionnalités. Ces attributs permettent à ASP.NET MVC de sélectionner une méthode d’action basée sur le verbe HTTP. Dans l’exemple suivant, une requête de publication appellera la première méthode d’action et une demande PUT appellera la deuxième méthode action.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

Dans les versions antérieures de ASP.NET MVC, ces méthodes d’action nécessitaient une syntaxe plus détaillée, comme illustré dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Étant donné que les navigateurs ne prennent en charge que les verbes d’extraction et de publication HTTP, il n’est pas possible d’effectuer une publication sur une action qui requiert un verbe différent. Par conséquent, il n’est pas possible de prendre en charge en mode natif toutes les demandes RESTful.

Toutefois, pour prendre en charge les demandes RESTful pendant les opérations de publication, ASP.NET MVC 2 introduit une nouvelle méthode d’assistance HTML HttpMethodOverride. Cette méthode restitue un élément d’entrée masqué qui amène le formulaire à émuler efficacement toute méthode HTTP. Par exemple, à l’aide de la méthode d’assistance HTML HttpMethodOverride, vous pouvez faire en sorte qu’une soumission de formulaire apparaisse soit une demande PUT ou DELETE. Le comportement de HttpMethodOverride affecte les attributs suivants :

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

L’élément INPUT masqué a son nom X-HTTP-Method-override et sa valeur définie sur le verbe HTTP à émuler. La valeur de remplacement peut également être spécifiée dans un en-tête HTTP ou dans une valeur de chaîne de requête en tant que paire nom/valeur.

Le remplacement ne peut être utilisé que lorsque la demande réelle est une demande de publication. La valeur de remplacement sera ignorée pour les demandes qui utilisent un autre verbe HTTP.

### <a name="new-hiddeninputattribute-class-for-templated-helpers"></a><a id="_TOC3_13"></a>Nouvelle classe HiddenInputAttribute pour les applications auxiliaires basées sur des modèles

Vous pouvez appliquer le nouvel attribut HiddenInputAttribute à une propriété de modèle pour indiquer si un élément d’entrée masqué doit être affiché lors de l’affichage du modèle dans un modèle d’éditeur. (L’attribut définit une valeur UIHint implicite de HiddenInput). La propriété DisplayValue de l’attribut vous permet de spécifier si la valeur est affichée en mode éditeur et en mode d’affichage. Quand DisplayValue est défini sur false, rien ne s’affiche, pas même le balisage HTML qui entoure normalement un champ. La valeur par défaut de DisplayValue est true.

Vous pouvez utiliser l’attribut HiddenInputAttribute dans les scénarios suivants :

- Lorsqu’un affichage permet aux utilisateurs de modifier l’ID d’un objet et qu’il est nécessaire d’afficher la valeur, ainsi que de fournir un élément d’entrée masqué contenant l’ancien ID afin qu’il puisse être renvoyé au contrôleur.
- Lorsqu’un affichage permet aux utilisateurs de modifier une propriété binaire qui ne doit jamais être affichée, telle qu’une propriété horodateur. Dans ce cas, la valeur et le balisage HTML environnant (tels que l’étiquette et la valeur) ne sont pas affichés.

L’exemple suivant montre comment utiliser la classe HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Lorsque l’attribut a la valeur true (ou qu’aucun paramètre n’est spécifié), voici ce qui se produit :

- Dans les modèles d’affichage, une étiquette est affichée et la valeur est affichée à l’utilisateur.
- Dans les modèles d’éditeur, une étiquette est rendue et la valeur est rendue dans un élément d’entrée masqué.

Lorsque l’attribut a la valeur false, voici ce qui se produit :

- Dans les modèles d’affichage, rien n’est restitué pour ce champ.
- Dans les modèles de l’éditeur, aucune étiquette n’est restituée et la valeur est rendue dans un élément d’entrée masqué.

### <a name="htmlvalidationsummary-helper-method-can-display-model-level-errors"></a><a id="_TOC3_14"></a>La méthode d’assistance HTML. ValidationSummary peut afficher des erreurs au niveau du modèle

Au lieu de toujours afficher toutes les erreurs de validation, la méthode d’assistance HTML. ValidationSummary a une nouvelle option pour afficher uniquement les erreurs au niveau du modèle. Cela permet d’afficher les erreurs au niveau du modèle dans le résumé des validations et les erreurs spécifiques aux champs à afficher en regard de chaque champ.

### <a name="t4-templates-in-visual-studio-generate-code-that-is-specific-to-the-target-version-of-the-net-framework"></a><a id="_TOC3_15"></a>Les modèles T4 dans Visual Studio génèrent du code spécifique à la version cible du .NET Framework

Une nouvelle propriété est disponible pour les fichiers T4 à partir de l’hôte ASP.NET MVC T4 qui spécifie la version du .NET Framework utilisé par l’application. Cela permet aux modèles T4 de générer le code et le balisage spécifiques à une version du .NET Framework. Dans Visual Studio 2008, la valeur est toujours .NET 3,5. Dans Visual Studio 2010, la valeur est soit .NET 3,5 soit .NET 4.

## <a name="api-improvements"></a><a id="_TOC4"></a>Améliorations des API

Cette section décrit les modifications apportées aux types et aux membres ASP.NET MVC existants.

- Ajout d’une méthode CreateActionInvoker virtuelle protégée dans la classe Controller. Cette méthode est appelée par la propriété ActionInvoker du contrôleur et autorise l’instanciation différée du demandeur si aucun demandeur n’est déjà défini.
- Ajout d’une méthode HandleUnauthorizedRequest virtuelle protégée dans la classe AuthorizeAttribute. Cela permet aux filtres qui dérivent de AuthorizeAttribute de contrôler le comportement lors de l’échec de l’autorisation.
- Ajout d’une méthode Add (clé de chaîne, valeur d’objet) dans la classe ValueProviderDictionary. Cela vous permet d’utiliser la syntaxe de l’initialiseur de dictionnaire pour ValueProviderDictionary, comme dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Ajout d’une \_ méthode d’objet d’extraction dans la classe Sys. Mvc. AjaxContext. Il s’agit d’une méthode JavaScript qui est similaire à la \_ méthode obtenir des données, mais si le type de contenu de la réponse est application/JSON, la fonction obtenir \_ l’objet retourne l’objet JSON.
- Ajout d’une propriété ActionDescriptor dans la classe AuthorizationContext.
- Ajout d’un jeton UrlParameter. optional qui peut être utilisé pour contourner des problèmes lors de la liaison à un modèle qui contient une propriété ID lorsque la propriété est absente dans une publication de formulaire. Pour plus d’informations, consultez l’entrée [ASP.net les paramètres d’URL facultatifs MVC 2](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) sur le blog de Phil Haack.

## <a name="breaking-changes"></a><a id="_TOC5"></a>Modifications avec rupture

Les modifications suivantes peuvent provoquer des erreurs dans les applications ASP.NET MVC 1,0 existantes.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Modification du comportement de validation des propriétés pour les classes qui implémentent IDataErrorInfo

Pour les objets de modèle qui utilisent IDataErrorInfo pour effectuer la validation, chaque propriété est validée, qu’une nouvelle valeur ait été définie ou non. Dans ASP.NET MVC 1,0, seules les propriétés dont les nouvelles valeurs ont été définies ont été validées. Dans ASP.NET MVC 2, la propriété Error de IDataErrorInfo est appelée uniquement si tous les validateurs de propriété ont réussi.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Le script de mappage de script IIS n’est plus disponible dans le programme d’installation

Le script de mappage de script IIS est un script de ligne de commande utilisé pour configurer des mappages de scripts pour IIS 6 et IIS 7 en mode classique. Le script de mappage de script n’est pas nécessaire si vous utilisez la Serveur Visual Studio Development ou si vous utilisez IIS 7 en mode intégré. Les scripts sont disponibles sous la forme d’un téléchargement distinct non pris en charge sur la [webstack ASP.net](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>La méthode d’assistance HTML. substitut dans MVC futures n’est plus disponible

En raison des modifications apportées au comportement de rendu des moteurs d’affichage MVC, la méthode d’assistance HTML. Substitute ne fonctionne pas et a été supprimée.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>L’interface IValueProvider remplace toutes les utilisations de IDictionary

Chaque argument de propriété ou de méthode qui a accepté IDictionary dans MVC 1,0 accepte désormais IValueProvider. Cette modification affecte uniquement les applications qui incluent des fournisseurs de valeurs personnalisés ou des classeurs de modèles personnalisés. Voici quelques exemples de propriétés et de méthodes qui sont affectées par cette modification :

- La propriété ValueProvider des classes ControllerBase et ModelBindingContext.
- Méthodes TryUpdateModel de la classe Controller.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>De nouvelles classes CSS ont été ajoutées dans le fichier site. CSS

Le fichier site. CSS dans les modèles de projet MVC ASP.NET a été mis à jour pour inclure les nouveaux styles utilisés par les fonctionnalités de validation et par les applications auxiliaires basées sur un modèle.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Les applications d’assistance retournent désormais un objet MvcHtmlString

Pour tirer parti de la nouvelle syntaxe d’expression d’encodage HTML dans ASP.NET 4, le type de retour pour les applications auxiliaires HTML est désormais MvcHtmlString au lieu d’une chaîne. Si vous utilisez ASP.NET MVC 2 et les nouvelles applications d’assistance sur ASP.NET 3,5, vous ne pourrez pas tirer parti de la syntaxe d’encodage HTML. la nouvelle syntaxe est disponible uniquement lorsque vous exécutez ASP.NET MVC 2 sur ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult répond désormais uniquement aux requêtes HTTP HTTP

Afin d’atténuer les attaques de détournement JSON susceptibles de divulguer des informations, par défaut, la classe JsonResult répond désormais uniquement aux requêtes HTTP. Les appels aux méthodes d’action Ajax qui retournent un objet JsonResult doivent être modifiés pour utiliser à la place la méthode de publication. Si nécessaire, vous pouvez remplacer ce comportement en définissant la nouvelle propriété JsonRequestBehavior de JsonResult. Pour plus d’informations sur les attaques potentielles, consultez le billet de blog « [piratage JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) » sur le blog de Phil Haack.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Les accesseurs set de propriété Model et ModelType sur ModelBindingContext sont obsolètes

Une nouvelle propriété ModelMetadata définissable a été ajoutée à la classe ModelBindingContext. La nouvelle propriété encapsule les propriétés Model et ModelType. Bien que les propriétés Model et ModelType soient obsolètes, pour des raisons de compatibilité descendante, les accesseurs get de propriété fonctionnent toujours. ils délèguent à la propriété ModelMetadata pour récupérer la valeur.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Les modifications apportées à la classe DefaultControllerFactory rompent les fabriques de contrôleurs personnalisés qui en dérivent

La classe DefaultControllerFactory a été corrigée en supprimant la propriété RequestContext. À la place de cette propriété, l’instance de contexte de requête est passée aux méthodes GetControllerInstance et GetControllerType virtuelles protégées. Cette modification affecte les fabriques de contrôleurs personnalisés qui dérivent de DefaultControllerFactory.

Les fabriques de contrôleurs personnalisés sont souvent utilisées pour fournir une injection de dépendances pour les applications MVC ASP.NET. Pour mettre à jour les fabriques de contrôleurs personnalisés pour prendre en charge ASP.NET MVC 2, modifiez la ou les signatures de la méthode pour qu’elles correspondent aux nouvelles signatures, puis utilisez le paramètre de contexte de requête au lieu de la propriété.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>« Area » est désormais une clé de valeur de route réservée

La chaîne « Area » dans les valeurs d’itinéraire a désormais une signification spéciale dans ASP.NET MVC, de la même façon que « Controller » et « action ». L’une des conséquences est que si des applications auxiliaires HTML sont fournies avec un dictionnaire de valeurs de route contenant « Area », les applications d’assistance n’ajouteront plus « Area » dans la chaîne de requête.

Si vous utilisez la fonctionnalité zones, veillez à ne pas utiliser {Area} dans le cadre de votre URL de routage.

## <a name="disclaimer"></a><a id="_TOC6"></a>AVERTISSEMENT

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans le présent document reflètent l'opinion de Microsoft Corporation sur les sujets abordés à la date de publication. Microsoft se doit de s'adapter aux conditions fluctuantes du marché, et cette opinion ne peut être considérée comme un engagement de sa part. Microsoft ne peut garantir la véracité de toute information présentée après la date de publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L’utilisateur est tenu d’observer la réglementation relative aux droits d’auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Les produits mentionnés dans ce document peuvent faire l'objet de brevets, de dépôts de brevets en cours, de marques, de droits d'auteur ou d'autres droits de propriété intellectuelle et industrielle de Microsoft. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf mention contraire, les exemples de sociétés, d’organisations, de produits, de noms de domaine, d’adresses de messagerie, de logos, de personnes, de lieux et d’événements mentionnés ici sont fictifs et toute ressemblance avec des sociétés, organisations, produits, noms de domaine, adresses électroniques, logos, personnes, lieux ou événements réels est purement fortuite et involontaire.

© 2010 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
