---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Quelles sont les nouveautés dans ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2. Ce document est également disponible pour téléchargement.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 82a3fd4fe74202ed9a23298390322458cfc029f8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030996"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Nouveautés d’ASP.NET MVC 2
====================
> Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2. Ce document est également disponible pour [télécharger](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Introduction](#_TOC1)   
[La mise à niveau d’un projet ASP.NET MVC 1.0 vers ASP.NET MVC 2](#_TOC2)   
[Nouvelles fonctionnalités](#_TOC3)   
[Programmes d’assistance basés sur des modèles](#_TOC3_1)   
[Zones](#_TOC3_2)   
[Prise en charge pour les contrôleurs asynchrones](#_TOC3_3)   
[Prise en charge de DefaultValueAttribute dans les paramètres de méthode d’Action](#_TOC3_4)   
[Prise en charge pour la liaison de données binaires de classeurs de modèles](#_TOC3_5)   
[ModelMetadata et Classes de ModelMetadataProvider](#_TOC3_6)   
[Prise en charge des attributs DataAnnotations](#_TOC3_7)   
[Fournisseurs de validateur de modèle](#_TOC3_8)   
[Validation côté client](#_TOC3_9)   
[Nouveaux extraits de Code pour Visual Studio 2010](#_TOC3_10)   
[Nouveau filtre d’Action RequireHttpsAttribute](#_TOC3_11)   
[Substituer le verbe HTTP (méthode)](#_TOC3_12)   
[Nouvelle classe HiddenInputAttribute pour les programmes d’assistance basés sur des modèles](#_TOC3_13)   
[Méthode d’assistance Html.ValidationSummary peut afficher des erreurs au niveau du modèle](#_TOC3_14)   
[Modèles T4 dans Visual Studio générer le Code qui est spécifique à la Version cible du .NET Framework](#_TOC3_15)[améliorations d’API](#_TOC4)  
[Modifications avec rupture](#_TOC5)  
[Exclusion de responsabilité](#_TOC6)  

## <a id="_TOC1"></a>  Introduction

ASP.NET MVC 2 s’appuie sur ASP.NET MVC 1.0 et introduit un grand ensemble d’améliorations et fonctionnalités axées sur l’augmentation de la productivité. Cette version est compatible avec ASP.NET MVC 1.0, afin que toutes vos connaissances, compétences, code et extensions pour ASP.NET MVC 1.0 continuent à appliquer.

Pour plus d’informations sur ASP.NET MVC, consultez les ressources suivantes :

- [Documentation d’ASP.NET MVC sur MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Le site Web ASP.NET MVC](https://asp.net/mvc/)
- [Les forums ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  La mise à niveau d’un projet ASP.NET MVC 1.0 vers ASP.NET MVC 2

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1.0 sur le même serveur, ce qui donne la flexibilité d’aux développeurs d’application dans le choix quand mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2. Pour plus d’informations sur la mise à niveau, consultez le document [la mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Nouvelles fonctionnalités

Cette section décrit les fonctionnalités qui ont été introduites dans la version de MVC 2.

### <a id="_TOC3_1"></a>  Programmes d’assistance basés sur des modèles

Programmes d’assistance basés sur des modèles vous permettent d’associer automatiquement les éléments HTML pour le modifier et affichent des types de données. Par exemple, lorsque les données de type System.DateTime sont affichées dans une vue, un élément d’interface utilisateur de sélecteur de dates peut être affiché automatiquement. Cela est similaire au fonctionnement des modèles de champ dans Dynamic Data ASP.NET. Pour plus d’informations, consultez [à l’aide de Helpers basé sur un modèle pour afficher les données](https://go.microsoft.com/fwlink/?LinkId=159062) sur le site Web MSDN.

### <a id="_TOC3_2"></a>  Zones

Zones vous permettent d’organiser un grand projet en plusieurs sections plus petites afin de gérer la complexité d’une grande application Web. En général, chaque section (« zone ») représente une section distincte d’un grand site Web et est utilisée pour regrouper les jeux liés de contrôleurs et de vues. Pour plus d’informations, consultez [Procédure pas à pas : Organisation d’une Application ASP.NET MVC en fonction des zones](https://go.microsoft.com/fwlink/?LinkId=158978) sur le site Web MSDN.

Pour créer une nouvelle zone, dans l’Explorateur de solutions, cliquez sur le projet, cliquez sur Ajouter, puis cliquez sur la zone. Cela affiche une boîte de dialogue qui vous invite à entrer le nom de zone. Après avoir entré le nom de zone, Visual Studio ajoute une nouvelle zone au projet.

La figure suivante montre un exemple de disposition pour un projet avec deux zones, administrateur et les Blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Lorsque vous créez une zone, Visual Studio ajoute une classe qui dérive de AreaRegistration à chaque zone. Cette classe est requise pour inscrire la zone et ses itinéraires, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Le modèle de projet par défaut pour ASP.NET MVC 2 inclut un appel à la méthode RegisterAllAreas dans le code pour le fichier Global.asax. Cette méthode inscrit chaque zone dans le projet en recherchant tous les types qui dérivent de la classe AreaRegistration, instanciation d’une instance du type et puis en appelant la méthode RegisterArea sur l’instance. L’exemple suivant montre comment procéder.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Si vous ne spécifiez pas l’espace de noms dans la méthode RegisterArea en appelant le contexte. Méthode Namespaces.Add, l’espace de noms de la classe d’enregistrement est utilisé par défaut.

### <a id="_TOC3_3"></a>  Prise en charge pour les contrôleurs asynchrones

ASP.NET MVC 2 permet désormais de contrôleurs traiter les demandes de façon asynchrone. Cela peut entraîner des gains de performances en permettant aux serveurs qui appellent fréquemment des opérations de blocage (par exemple, les demandes du réseau) pour appeler à la place des homologues non bloquant. Pour plus d’informations, consultez le [à l’aide d’un contrôleur asynchrone dans ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) sujet sur MSDN.

### <a id="_TOC3_4"></a>  Prise en charge de DefaultValueAttribute dans les paramètres de méthode d’Action

La classe System.ComponentModel.DefaultValueAttribute permet à une valeur par défaut doit être fourni pour le paramètre d’argument à une méthode d’action. Par exemple, supposons que l’itinéraire par défaut suivant est défini :

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Supposons également que la méthode de contrôleur et l’action suivante est définie :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Un de la demande suivante URL appelle la méthode d’action de vue qui est définie dans l’exemple précédent.

- / Article/vue/123
- / Article/vue/123 ? page = 1 (en fait identique à la demande précédente)
- / Article/vue/123 ? page = 2

Sans l’attribut DefaultValueAttribute, la première URL à partir de la liste précédente fonctionnerait pas, car l’argument de la page est un type de valeur non nullable dont la valeur n’a pas été fournie.

Si votre code est écrit en Visual Basic 2010 ou Visual c# 2010, vous pouvez utiliser les paramètres facultatifs au lieu de l’attribut DefaultValueAttribute, comme indiqué dans l’exemple suivant :

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Prise en charge pour la liaison de données binaires de classeurs de modèles

Il existe deux nouvelles surcharges de l’application d’assistance Html.Hidden qui codent les valeurs binaires en tant que chaînes codées en base 64 :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Une utilisation typique consiste à incorporer un horodatage pour un objet dans la vue. Par exemple, votre application peut inclure l’objet produit suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un formulaire de modification peut rendre la propriété d’horodatage dans le formulaire comme illustré dans l’exemple suivant :

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Ce balisage restitue un élément input masqué avec la valeur d’horodatage sous forme de chaîne codée en base 64 qui ressemble à l’exemple suivant :

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Ce formulaire peut être publié sur une méthode d’action qui a un argument de type de produit, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Dans la méthode d’action, la propriété TimeStamp est remplie correctement car la chaîne encodée en base 64 publiée est convertie en un tableau d’octets.

### <a id="_TOC3_6"></a>  ModelMetadata et Classes de ModelMetadataProvider

La classe ModelMetadataProvider fournit une abstraction pour obtenir des métadonnées pour le modèle dans une vue. MVC 2 inclut un fournisseur par défaut qui rend disponibles les métadonnées qui sont exposée par les attributs dans l’espace de noms System.ComponentModel.DataAnnotations. Il est possible de créer des fournisseurs de métadonnées qui fournissent des métadonnées à partir d’autres magasins de données, telles que les bases de données ou des fichiers XML.

La classe ViewDataDictionary expose un objet ModelMetadata qui contient les métadonnées sont extraites à partir du modèle par la classe ModelMetadataProvider. Ainsi, les programmes d’assistance basé sur un modèle consommer ces métadonnées et d’ajuster leur sortie en conséquence.

Pour plus d’informations, consultez la documentation pour le [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) et [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) classes.

### <a id="_TOC3_7"></a>  Prise en charge des attributs DataAnnotations

ASP.NET MVC 2 prend en charge les attributs de validation RangeAttribute, RequiredAttribute, StringLengthAttribute et RegexAttribute (définis dans l’espace de noms System.ComponentModel.DataAnnotations) lorsque vous liez à un modèle afin de fournir une entrée validation.

Pour plus d'informations, voir [Procédure : Valider les données de modèle à l’aide des attributs DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) sur le site Web MSDN. Un exemple de projet qui illustre l’utilisation de ces attributs est disponible pour téléchargement à l’adresse [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Fournisseurs de validateur de modèle

La classe de fournisseur de validation du modèle représente une abstraction qui fournit la logique de validation pour le modèle. ASP.NET MVC inclut un fournisseur par défaut en fonction des attributs de validation qui sont inclus dans l’espace de noms System.ComponentModel.DataAnnotations. Vous pouvez également créer vos propres fournisseurs de validation qui définissent les règles de validation personnalisées et des mappages personnalisés de règles de validation au modèle. Pour plus d’informations, consultez la documentation pour le [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) classe.

### <a id="_TOC3_9"></a>  Validation côté client

La classe de fournisseur de validateur de modèle expose les métadonnées de validation au navigateur sous la forme de données JSON sérialisées qui peuvent être consommées par une bibliothèque de validation côté client. ASP.NET MVC 2 inclut une bibliothèque de validation client et un adaptateur qui prend en charge les attributs de validation d’espace de noms DataAnnotations indiqués précédemment. La classe de fournisseur vous permet également à utiliser d’autres bibliothèques de validation côté client en écrivant un adaptateur qui traite les données JSON et les appels dans l’autre bibliothèque.

### <a id="_TOC3_10"></a>  Nouveaux extraits de Code pour Visual Studio 2010

Un ensemble d’extraits de code HTML pour ASP.NET MVC 2 est installé avec Visual Studio 2010. Pour afficher une liste de ces extraits de code, dans le menu Outils, sélectionnez Gestionnaire des extraits de Code. Pour la langue, sélectionnez HTML, puis pour l’emplacement, ASP.NET MVC 2. Pour plus d’informations sur l’utilisation des extraits de code, consultez la documentation de Visual Studio.

### <a id="_TOC3_11"></a>  Nouveau filtre d’Action RequireHttpsAttribute

ASP.NET MVC 2 inclut une nouvelle classe RequireHttpsAttribute qui peut être appliquée aux contrôleurs et aux méthodes d’action. Par défaut, le filtre redirige une requête HTTP non - SSL à l’équivalent compatibles SSL (HTTPS).

### <a id="_TOC3_12"></a>  Substituer le verbe HTTP (méthode)

Lorsque vous générez un site Web à l’aide du style d’architecture REST, les verbes HTTP servent à déterminer l’action à effectuer pour une ressource. REST requiert qu’applications prenant en charge la gamme complète des verbes HTTP courants, y compris GET, PUT, POST et DELETE.

ASP.NET MVC 2 inclut de nouveaux attributs que vous pouvez appliquer à cette syntaxe compacte de fonctionnalité et de méthodes d’action. Ces attributs permettent à ASP.NET MVC sélectionner une méthode d’action en fonction du verbe HTTP. Dans l’exemple suivant, une demande POST appellera la méthode d’action et une requête PUT appellera la deuxième méthode d’action.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

Dans les versions antérieures d’ASP.NET MVC, ces méthodes d’action requis une syntaxe plus détaillée, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Étant donné que les navigateurs prennent en charge uniquement les verbes GET et HTTP POST, il n’est pas possible de publier sur une action qui nécessite un verbe différent. Par conséquent, il n’est pas possible prendre en charge en mode natif de toutes les demandes RESTful.

Toutefois, pour prendre en charge RESTful demandes lors des opérations POST, ASP.NET MVC 2 introduit une nouvelle méthode d’assistance de HttpMethodOverride HTML. Cette méthode restitue un élément d’entrée masqué qui provoque le formulaire émuler efficacement n’importe quelle méthode HTTP. Par exemple, à l’aide de la méthode d’assistance HttpMethodOverride HTML, vous pouvez avoir une soumission de formulaire apparaissent s’agir d’une requête PUT ou DELETE. Le comportement de HttpMethodOverride affecte les attributs suivants :

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

L’élément input masqué a son nom X-HTTP-Method-Override et sa valeur définie sur le verbe HTTP pour émuler. La valeur de remplacement peut également être spécifiée dans un en-tête HTTP ou dans une valeur de chaîne de requête, comme une paire nom/valeur.

Le remplacement utilisable uniquement lorsque la demande réelle est une demande POST. La valeur de remplacement va être ignorée pour les demandes qui utilisent tous les autres verbes HTTP.

### <a id="_TOC3_13"></a>  Nouvelle classe HiddenInputAttribute pour les programmes d’assistance basés sur des modèles

Vous pouvez appliquer le nouvel attribut HiddenInputAttribute à une propriété de modèle pour indiquer si un élément input masqué doit être restitué lors de l’affichage du modèle dans un éditeur de modèle. (L’attribut définit la valeur implicite UIHint HiddenInput). Propriété d’une valeur d’affichage de l’attribut vous permet de spécifier si la valeur est affichée dans l’éditeur et de modes d’affichage. Lorsqu’une valeur d’affichage est définie sur false, rien ne s’affiche, pas même le balisage HTML qui entoure normalement un champ. La valeur par défaut pour une valeur d’affichage a la valeur true.

Vous pouvez utiliser l’attribut de HiddenInputAttribute dans les scénarios suivants :

- Quand une vue permet aux utilisateurs de modifier l’ID d’un objet et il est nécessaire afficher la valeur, ainsi que pour fournir un élément input masqué qui contient l’ancien ID afin qu’elle puisse être passée sur le contrôleur.
- Lorsqu’une vue permet aux utilisateurs modifier une propriété binaire ne doit jamais être affichée comme une propriété d’horodatage. Dans ce cas, la valeur et le balisage HTML environnant (par exemple, l’étiquette et la valeur) ne sont pas affichés.

L’exemple suivant montre comment utiliser la classe HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Lorsque l’attribut est défini sur true (ou aucun paramètre n’est spécifié), ce qui suit se produit :

- Dans les modèles d’affichage, une étiquette est rendue et la valeur est affichée à l’utilisateur.
- Dans les modèles de l’éditeur, une étiquette est rendue et la valeur est restituée dans un élément input masqué.

Lorsque l’attribut est défini sur false, les événements suivants se produisent :

- Dans les modèles d’affichage, rien n’est restitué pour ce champ.
- Dans les modèles de l’éditeur, aucune étiquette n’est rendu et la valeur est restituée dans un élément input masqué.

### <a id="_TOC3_14"></a>  Méthode d’assistance Html.ValidationSummary peut afficher des erreurs au niveau du modèle

Au lieu d’afficher toujours toutes les erreurs de validation, la méthode d’assistance Html.ValidationSummary a une nouvelle option pour afficher uniquement les erreurs au niveau du modèle. Ainsi, les erreurs au niveau du modèle à afficher dans le résumé des validations et des erreurs spécifiques aux champs s’affichent en regard de chaque champ.

### <a id="_TOC3_15"></a>  Modèles T4 dans Visual Studio générer le Code qui est spécifique à la Version cible du .NET Framework

Une nouvelle propriété est disponible pour les fichiers T4 à partir de l’hôte ASP.NET MVC T4 qui spécifie la version du .NET Framework qui est utilisé par l’application. Ainsi, les modèles T4 générer le code et le balisage qui est spécifique à une version du .NET Framework. Dans Visual Studio 2008, la valeur est toujours .NET 3.5. Dans Visual Studio 2010, la valeur est .NET 3.5 ou .NET 4.

## <a id="_TOC4"></a>  Améliorations d’API

Cette section décrit les modifications apportées aux membres et types d’ASP.NET MVC existantes.

- Ajout d’une méthode de CreateActionInvoker virtuelle protégée dans la classe de contrôleur. Cette méthode est appelée par la propriété ActionInvoker du contrôleur d’et permet l’instanciation différée du demandeur de si aucun demandeur n’est déjà défini.
- Ajout d’une méthode de HandleUnauthorizedRequest virtuelle protégée dans la classe AuthorizeAttribute. Ainsi, les filtres qui dérivent d’AuthorizeAttribute pour contrôler le comportement lorsque l’autorisation échoue.
- Ajouter une méthode Add (chaîne clé, valeur de l’objet) dans la classe ValueProviderDictionary. Cela vous permet d’utiliser la syntaxe d’initialiseur de dictionnaire pour ValueProviderDictionary, comme dans l’exemple suivant :

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Ajouter une opération get\_méthode dans la classe Sys.Mvc.AjaxContext de l’objet. Il s’agit d’une méthode JavaScript qui est similaire à la méthode get\_obtenir de la méthode de données, mais si le type de contenu de la réponse est application/json,\_object retourne l’objet JSON.
- Dans la classe AuthorizationContext, ajouté une propriété du ActionDescriptor.
- Ajouter un jeton UrlParameter.Optional qui peut être utilisé pour contourner des problèmes lors de la liaison à un modèle qui contient une propriété d’ID lors de la propriété est absente dans une publication de formulaire. Pour plus d’informations, consultez l’entrée [des paramètres facultatifs URL ASP.NET MVC 2](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) sur du blog de Phil.

## <a id="_TOC5"></a>  Modifications avec rupture

Les modifications suivantes peuvent provoquer des erreurs dans les applications ASP.NET MVC 1.0 existantes.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Modification du comportement de validation de propriété pour les classes qui implémentent l’interface IDataErrorInfo

Pour les objets de modèle qui permettent d’effectuer la validation IDataErrorInfo, chaque propriété est validée, même si une nouvelle valeur a été définie. Dans ASP.NET MVC 1.0, seules les propriétés qui avait été défini sur nouvelles valeurs ont été validées. Dans ASP.NET MVC 2, la propriété Error d’IDataErrorInfo est appelée uniquement si tous les validateurs de propriété ont réussi.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Script de mappage de script IIS n’est plus disponible dans le programme d’installation

Le script de mappage de scripts IIS est un script de ligne de commande qui est utilisé pour configurer les mappages de script IIS 6 et IIS 7 en mode classique. Le script de mappage de scripts n’est pas nécessaire si vous utilisez le serveur de développement Visual Studio ou si vous utilisez IIS 7 en mode intégré. Les scripts sont disponibles en téléchargement séparé non pris en charge sur le [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>La méthode d’assistance Html.Substitute dans les Futures MVC n’est plus disponible

En raison de modifications dans le comportement de rendu des moteurs d’affichage MVC, la méthode d’assistance Html.Substitute ne fonctionne pas et a été supprimée.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>L’interface IValueProvider remplace toutes les utilisations de IDictionary

Chaque argument de méthode ou propriété acceptant IDictionary dans MVC 1.0 est désormais accepte IValueProvider. Cette modification affecte uniquement les applications qui incluent des fournisseurs de valeurs personnalisés ou des classeurs de modèles personnalisés. Exemples de propriétés et méthodes qui sont affectés par cette modification sont les suivants :

- La propriété ValueProvider des classes ControllerBase et ModelBindingContext.
- Les méthodes TryUpdateModel de la classe de contrôleur.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Nouvelles classes CSS ont été ajoutés dans le fichier Site.css

Le fichier Site.css dans les modèles de projet ASP.NET MVC a été mis à jour pour inclure les nouveaux styles utilisés par la fonctionnalité de validation et les programmes d’assistance basés sur des modèles.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Programmes d’assistance retournent désormais un objet MvcHtmlString

Pour tirer parti de la nouvelle syntaxe d’expression encodage HTML dans ASP.NET 4, le type de retour pour les programmes d’assistance HTML est désormais MvcHtmlString plutôt qu’une chaîne. Si vous utilisez ASP.NET MVC 2 et les programmes d’assistance nouvelle sur ASP.NET 3.5, vous ne serez pas en mesure de tirer parti de la syntaxe de codage HTML ; la nouvelle syntaxe est disponible uniquement lorsque vous exécutez ASP.NET MVC 2 sur 4 d’ASP.NET.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult maintenant répond uniquement aux requêtes HTTP POST

Afin d’atténuer JSON détournements ayant le risque de divulgation d’informations, par défaut, la classe JsonResult est désormais répond uniquement aux requêtes HTTP POST. AJAX obtenir les appels aux méthodes d’action qui retournent un objet JsonResult doivent être modifiés pour utiliser POST à la place. Si nécessaire, vous pouvez remplacer ce comportement en définissant la nouvelle propriété JsonRequestBehavior de JsonResult. Pour plus d’informations sur l’exploitation potentielle, consultez le blog [de piratage JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) sur du blog de Phil.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Modèle et des accesseurs Set de propriété ModelType sur ModelBindingContext est obsolètes

Une nouvelle propriété ModelMetadata définissable a été ajoutée à la classe ModelBindingContext. La nouvelle propriété encapsule le modèle et les propriétés de ModelType. Bien que les propriétés de modèle et ModelType sont obsolètes, pour la compatibilité descendante les accesseurs Get de propriété continue de fonctionner ; ils délèguent à la propriété ModelMetadata pour récupérer la valeur.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Les modifications apportées à la classe de DefaultControllerFactory altérer des fabriques de contrôleurs personnalisés qui en dérivent

La classe de DefaultControllerFactory a été résolue en supprimant la propriété RequestContext. À la place de cette propriété, l’instance de contexte de requête est passé aux méthodes de GetControllerInstance et GetControllerType virtuels protégés. Ce changement affecte les fabriques de contrôleurs personnalisés qui dérivent de DefaultControllerFactory.

Fabriques de contrôleurs personnalisés sont souvent utilisés pour fournir des applications de l’injection de dépendances pour ASP.NET MVC. Pour mettre à jour les fabriques de contrôleur personnalisé pour prendre en charge d’ASP.NET MVC 2, modifiez la signature de méthode ou des signatures pour faire correspondre les signatures de nouveau et utiliser le paramètre de contexte de requête au lieu de la propriété.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>« Zone » est maintenant une clé de la valeur de route réservé

La chaîne « zone » dans les valeurs d’itinéraire comporte maintenant une signification spéciale dans ASP.NET MVC, la même manière que « controller » et « action ». Une conséquence est que si des programmes d’assistance HTML sont fournis avec un dictionnaire de la valeur de routage contenant « zone », les programmes d’assistance n’est plus ajoute « zone » dans la chaîne de requête.

Si vous utilisez la fonctionnalité de zones, veillez à ne pas utiliser {zone} dans le cadre de votre URL de l’itinéraire.


## <a id="_TOC6"></a>  Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d'enregistrement de brevets ou être titulaire de marques, droits d'auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l'objet du présent document. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf indication contraire, les exemples de sociétés, les organisations, les produits, les noms de domaine, adresses de messagerie, logos, personnes, lieux et événements mentionnés sont fictif et aucune association avec n’importe quel ressemblance organisation, produit, nom de domaine, courrier électronique adresse logo, personne ou des événements est destiné, ou doit être déduite.

© 2010 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
