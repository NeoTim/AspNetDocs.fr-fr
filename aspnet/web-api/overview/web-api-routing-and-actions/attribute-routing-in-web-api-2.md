---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Itinéraire d’attribut dans ASP.NET’API Web 2 (fr) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: bb68fe8e6769619029a3fa039d6f0d6f3303afbe
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675383"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Itinéraire d’attribut dans ASP.NET API Web 2

par [Mike Wasson](https://github.com/MikeWasson)

*Routing* est la façon dont l’API Web correspond à une URI à une action. Web API 2 prend en charge un nouveau type de routage, appelé *itinéraire d’attribut*. Comme son nom l’indique, le routage d’attribut utilise des attributs pour définir les itinéraires. Le routage d’attribut vous donne plus de contrôle sur les URL dans votre API Web. Par exemple, vous pouvez facilement créer des URL qui décrivent les hiérarchies de ressources.

Le style antérieur de routage, appelé itinéraire basé sur les conventions, est toujours entièrement pris en charge. En fait, vous pouvez combiner les deux techniques dans le même projet.

Ce sujet montre comment activer le routage d’attribut et décrit les différentes options pour le routage d’attribut. Pour un tutoriel de bout en bout qui utilise le routage d’attribut, voir [Créer une API REST avec Routing Attribut dans l’API Web 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Prérequis

[Studio visuel 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Édition communautaire, professionnelle ou d’entreprise

Vous pouvez également utiliser NuGet Package Manager pour installer les paquets nécessaires. Du menu **Tools** de Visual Studio, sélectionnez **NuGet Package Manager,** puis sélectionnez **La console Package Manager**. Entrez la commande suivante dans la fenêtre console De gestionnaire de paquet :

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Pourquoi attribuer le routage?

La première version de l’API Web a utilisé le routage *basé sur les conventions.* Dans ce type de routage, vous définissez un ou plusieurs modèles d’itinéraire, qui sont essentiellement des chaînes paramétrisées. Lorsque le cadre reçoit une demande, il correspond à l’URI par rapport au modèle d’itinéraire. Pour plus d’informations sur le routage basé sur les congrès, voir [Routing dans ASP.NET’API Web](routing-in-aspnet-web-api.md).

L’un des avantages de l’itinéraire fondé sur les conventions est que les modèles sont définis en un seul endroit, et que les règles d’itinéraire sont appliquées uniformément dans tous les contrôleurs. Malheureusement, le routage fondé sur les conventions rend difficile le soutien de certains modèles d’URI qui sont courants dans les API RESTful. Par exemple, les ressources contiennent souvent des ressources pour enfants : les clients ont des commandes, les films ont des acteurs, les livres ont des auteurs, et ainsi de suite. Il est naturel de créer des URI qui reflètent ces relations :

`/customers/1/orders`

Ce type d’URI est difficile à créer à l’aide d’un itinéraire basé sur la convention. Bien que cela puisse être fait, les résultats ne s’évoluent pas bien si vous avez de nombreux contrôleurs ou types de ressources.

Avec le routage d’attribut, il est trivial de définir une voie pour cette URI. Il vous suffit d’ajouter un attribut à l’action du contrôleur :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Voici quelques autres modèles qui attribuent le routage rend facile.

**Contrôle de version d’API**

Dans cet exemple, "/api/v1/products" serait acheminé vers un contrôleur différent de "/api/v2/products".

`/api/v1/products`
`/api/v2/products`

**Segments URI surchargés**

Dans cet exemple, "1" est un numéro de commande, mais des cartes "en attente" à une collection.

`/orders/1`
`/orders/pending`

**Types de paramètres multiples**

Dans cet exemple, "1" est un numéro de commande, mais "2013/06/16" précise une date.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Permettre le routage d’attribut

Pour activer le routage des attributs, appelez **MapHttpAttributeRoutes** pendant la configuration. Cette méthode d’extension est définie dans la classe **System.Web.Http.HttpConfigurationExtensions.**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Le routage d’attribut peut être combiné avec le routage [basé sur la convention.](routing-in-aspnet-web-api.md) Pour définir les itinéraires basés sur les conventions, appelez la méthode **MapHttpRoute.**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Pour plus d’informations sur la configuration de l’API Web, voir [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Note: Migreing From Web API 1

Avant l’API Web 2, les modèles de projet Web API ont généré du code comme celui-ci :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si le routage d’attribut est activé, ce code jettera une exception. Si vous mettez à niveau un projet d’API Web existant pour utiliser le routage d’attribut, assurez-vous de mettre à jour ce code de configuration à ce qui suit :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Pour plus d’informations, voir [Configuring Web API avec ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Ajout d’attributs d’itinéraire

Voici un exemple d’itinéraire défini à l’aide d’un attribut :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Les &quot;clients de la chaîne/customerId/commandes&quot; sont le modèle URI pour l’itinéraire. L’API Web tente de faire correspondre la demande URI au modèle. Dans cet exemple, les « clients » et les « commandes » sont des segments littérals, et « CustomerId » est un paramètre variable. Les URL suivantes correspondraient à ce modèle :

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Vous pouvez restreindre l’appariement en utilisant des [contraintes](#constraints), décrites plus tard dans ce sujet.

Notez &quot;que le&quot; paramètre «customerId» dans le modèle d’itinéraire correspond au nom du *paramètre customerId* dans la méthode. Lorsque l’API Web invoque l’action du contrôleur, il essaie de lier les paramètres d’itinéraire. Par exemple, si l’URI est, `http://example.com/customers/1/orders`l’API Web tente de lier la valeur "1" au *paramètre customerId* dans l’action.

Un modèle URI peut avoir plusieurs paramètres :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Toutes les méthodes de contrôleur qui n’ont pas d’attribut d’itinéraire utilisent le routage basé sur la convention. De cette façon, vous pouvez combiner les deux types de routage dans le même projet.

## <a name="http-methods"></a>HTTP Methods

L’API Web sélectionne également les actions basées sur la méthode HTTP de la demande (GET, POST, etc.). Par défaut, l’API Web recherche un match insensible au cas avec le début du nom de la méthode du contrôleur. Par exemple, une `PutCustomers` méthode de contrôleur nommée correspond à une demande HTTP PUT.

Vous pouvez passer outre à cette convention en décorant la méthode avec tous les attributs suivants :

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

L’exemple suivant cartographie la méthode CreateBook aux demandes HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Pour toutes les autres méthodes HTTP, y compris les méthodes non standard, utilisez **l’attribut AcceptVerbs,** qui prend une liste de méthodes HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Préfixes d’itinéraire

Souvent, les itinéraires dans un contrôleur commencent tous avec le même préfixe. Par exemple :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Vous pouvez définir un préfixe commun pour un contrôleur entier en utilisant **l’attribut [RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilisez un tilde sur l’attribut de méthode pour remplacer le préfixe de l’itinéraire :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Le préfixe de l’itinéraire peut inclure des paramètres :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Contraintes d’itinéraire

Les contraintes d’itinéraire vous permettent de limiter la façon dont les paramètres du modèle d’itinéraire sont appariés. La syntaxe &quot;générale est «paramètre:contrainte».&quot; Par exemple :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Ici, le premier itinéraire ne &quot;sera&quot; choisi que si le segment d’id de l’URI est un intégrant. Dans le cas contraire, le deuxième itinéraire sera choisi.

Le tableau suivant énumère les contraintes qui sont prises en charge.

| Contrainte | Description | Exemple |
| --- | --- | --- |
| alpha | Correspond aux caractères de l’alphabet latin majuscule ou inférieur (a-z, A-Z) | X:alpha |
| bool | Correspond à une valeur Boolean. | X:bool |
| DATETIME | Correspond à une valeur **DateTime.** | X: date d’actualité |
| Décimal | Correspond à une valeur décimale. | X:décimale |
| double | Correspond à une valeur de 64 bits de point flottant. | X:double |
| float | Correspond à une valeur de 32 bits de point flottant. | X:float |
| guid | Correspond à une valeur GUID. | X:guid |
| int | Correspond à une valeur integer 32 bits. | X:int |
| length | Assorti une chaîne avec la longueur spécifiée ou dans une plage spécifiée de longueurs. | X:longueur(6) X:longueur(1,20) |
| long | Correspond à une valeur integer 64 bits. | X:long |
| max | Assortie un intégrant avec une valeur maximale. | X:max(10) |
| Maxlength | Assortie une corde d’une longueur maximale. | X:maxlength(10) |
| min | Assortie un intégrant d’une valeur minimale. | X:min(10) |
| minlength | Assorti une chaîne d’une longueur minimale. | X:minlength(10) |
| range | Correspond à un intégrant dans une gamme de valeurs. | X:gamme (10,50) |
| regex | Correspond à une expression régulière. | X:regex (d{3}{3}-d{4}-d $) |

Notez que certaines des &quot;contraintes, telles que min&quot;, prendre des arguments entre parenthèses. Vous pouvez appliquer de multiples contraintes à un paramètre, séparé par un côlon.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Contraintes d’itinéraire personnalisé

Vous pouvez créer des contraintes d’itinéraire personnalisées en implémentant l’interface **IHttpRouteConstraint.** Par exemple, la contrainte suivante limite un paramètre à une valeur non nulle.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Le code suivant montre comment enregistrer la contrainte :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Vous pouvez maintenant appliquer la contrainte dans vos itinéraires :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Vous pouvez également remplacer l’ensemble de la classe **DefaultInlineConstraintResolver** en implémentant l’interface **IInlineConstraintResolver.** Cela remplacera toutes les contraintes intégrées, à moins que votre mise en œuvre **d’IInlineConstraintResolver** les ajoute spécifiquement.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Paramètres URI optionnels et valeurs par défaut

Vous pouvez rendre un paramètre URI facultatif en ajoutant un point d’interrogation au paramètre d’itinéraire. Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de la méthode.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Dans cet `/api/books/locale/1033` exemple, et `/api/books/locale` retourner la même ressource.

Vous pouvez également spécifier une valeur par défaut à l’intérieur du modèle d’itinéraire, comme suit :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

C’est presque la même chose que l’exemple précédent, mais il ya une légère différence de comportement lorsque la valeur par défaut est appliquée.

- Dans le premier exemple (« lcid : int ? »), la valeur par défaut de 1033 est attribuée directement au paramètre de la méthode, de sorte que le paramètre aura cette valeur exacte.
- Dans le deuxième exemple (« lcid : int-1033 »), la valeur par défaut de « 1033 » passe par le processus de liaison de modèle. Le reliure de modèle par défaut convertira « 1033 » à la valeur numérique 1033. Cependant, vous pouvez brancher un classeur de modèle personnalisé, qui pourrait faire quelque chose de différent.

(Dans la plupart des cas, à moins d’avoir des liants modèles personnalisés dans votre pipeline, les deux formulaires seront équivalents.)

<a id="route-names"></a>
## <a name="route-names"></a>Noms d’itinéraire

Dans l’API Web, chaque itinéraire a un nom. Les noms d’itinéraires sont utiles pour générer des liens, de sorte que vous pouvez inclure un lien dans une réponse HTTP.

Pour spécifier le nom de l’itinéraire, définissez la propriété **Name** sur l’attribut. L’exemple suivant montre comment définir le nom de l’itinéraire, et aussi comment utiliser le nom de l’itinéraire lors de la génération d’un lien.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordre d’itinéraire

Lorsque le cadre tente de faire correspondre un URI à un itinéraire, il évalue les itinéraires dans un ordre particulier. Pour spécifier l’ordre, définissez la propriété **de l’Ordre** sur l’attribut de l’itinéraire. Les valeurs inférieures sont évaluées en premier. La valeur de commande par défaut est nulle.

Voici comment la commande totale est déterminée :

1. Comparez la propriété **de l’Ordre** de l’attribut de l’itinéraire.
2. Regardez chaque segment URI dans le modèle d’itinéraire. Pour chaque segment, commandez comme suit :

    1. Segments littérales.
    2. Paramètres d’itinéraire avec contraintes.
    3. Paramètres d’itinéraire sans contraintes.
    4. Segments de paramètres Wildcard avec des contraintes.
    5. Segments de paramètres Wildcard sans contraintes.
3. Dans le cas d’une égalité, les itinéraires sont ordonnés par une comparaison de chaîne ordinaire insensible au cas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle d’itinéraire.

Voici un exemple. Supposons que vous définissiez le contrôleur suivant :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Ces itinéraires sont commandés comme suit.

1. commandes/détails
2. commandes/id
3. commandes/'customerName'
4. commandes/\*date de
5. commandes/en attente

Notez que les « détails » sont un segment littéral et apparaissent avant « id », mais « en attente » apparaît en dernier parce que la propriété **de l’Ordre** est de 1. (Cet exemple suppose qu’il n’y a pas de clients nommés «détails» ou «en attente». En général, essayez d’éviter les voies ambigues. Dans cet exemple, un `GetByCustomer` meilleur modèle d’itinéraire pour est "clients / clientName" )
