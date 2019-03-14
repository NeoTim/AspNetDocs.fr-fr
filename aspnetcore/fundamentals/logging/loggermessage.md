---
title: Journalisation avancée avec LoggerMessage dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser LoggerMessage pour créer des délégués pouvant être mis en cache et nécessitant moins d’allocations d’objets pour les scénarios de journalisation à hautes performances.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: a0080a20fed2d8fc295e55822c11d5731c6910ca
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048616"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Journalisation avancée avec LoggerMessage dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Les fonctionnalités [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) créent des délégués pouvant être mis en cache qui nécessitent moins d’allocations d’objet et de charge de calcul par rapport aux [méthodes d’extension de journaliseur](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), telles que `LogInformation`, `LogDebug` et `LogError`. Pour les scénarios de journalisation hautes performances, utilisez le modèle `LoggerMessage`.

`LoggerMessage` procure les avantages suivants en termes de performances par rapport aux méthodes d’extension de journaliseur :

* Les méthodes d’extension de journaliseur nécessitent la conversion (« boxing ») de types de valeur, tels que `int`, en `object`. Utilisant des champs `Action` statiques et des méthodes d’extension avec des paramètres fortement typés, le modèle `LoggerMessage` évite le boxing.
* Les méthodes d’extension de journaliseur doivent analyser le modèle de message (chaîne de format nommé) chaque fois qu’un message de journal est écrit. `LoggerMessage` requiert l’analyse d’un modèle une seule fois quand le message est défini.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple d’application illustre les fonctionnalités `LoggerMessage` avec un système de suivi de citations de base. L’application ajoute et supprime des citations à l’aide d’une base de données en mémoire. À mesure que ces opérations se produisent, des messages de journal sont générés à l’aide du modèle `LoggerMessage`.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crée un délégué `Action` pour la journalisation d’un message. Les surcharges `Define` permettent de passer jusqu’à six paramètres de type à une chaîne de format nommée (modèle).

La chaîne fournie à la méthode `Define` est un modèle et non pas une chaîne interpolée. Les espaces réservés sont remplis dans l’ordre dans lequel les types sont spécifiés. Les noms d’espace réservé dans le modèle doivent être descriptifs et cohérents d’un modèle à l’autre. Ils servent de noms de propriété dans les données de journal structurées. Nous vous recommandons d’utiliser la [casse Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) pour les noms d’espace réservé. Par exemple, `{Count}`, `{FirstName}`.

Chaque message de journal est une `Action` contenue dans un champ statique créé par `LoggerMessage.Define`. Par exemple, l’exemple d’application crée un champ afin de décrire un message de journal pour une demande GET pour la page Index (*Internal/LoggerExtensions.cs*) :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Pour l’`Action`, spécifiez :

* Le niveau du journal
* Un identificateur d’événement unique ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) avec le nom de la méthode d’extension statique
* Le modèle de message (chaîne de format nommée) 

Une demande pour la page Index de l’exemple d’application définit :

* Le niveau de journal sur `Information`
* L’ID d’événement sur `1` avec le nom de la méthode `IndexPageRequested`
* Le modèle de message (chaîne de format nommée) sur une chaîne

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Des magasins de journalisation structurée peuvent utiliser le nom d’événement quand il est fourni avec l’ID d’événement pour enrichir la journalisation. Par exemple, [Serilog](https://github.com/serilog/serilog-extensions-logging) utilise le nom d’événement.

L’`Action` est appelée par le biais d’une méthode d’extension fortement typée. La méthode `IndexPageRequested` journalise un message pour une demande GET pour la page Index dans l’exemple d’application :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` est appelé sur le journaliseur dans la méthode `OnGetAsync` dans *Pages/Index.cshtml.cs* :

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Examinez la sortie de la console de l’application :

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Pour passer des paramètres à un message de journal, définissez jusqu’à six types au moment de la création du champ statique. L’exemple d’application journalise une chaîne au moment de l’ajout d’une citation en définissant un type `string` pour le champ `Action` :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Le modèle de message de journal du délégué reçoit ses valeurs d’espace réservé des types fournis. L’exemple d’application définit un délégué pour l’ajout d’une citation, où le paramètre Quote est de type `string` :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

La méthode d’extension statique pour l’ajout d’une citation, `QuoteAdded`, reçoit la valeur d’argument quote et la passe au délégué `Action` :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

Dans le modèle de page de la page Index (*Pages/Index.cshtml.cs*), `QuoteAdded` est appelé pour journaliser le message :

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Examinez la sortie de la console de l’application :

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

L’exemple d’application implémente un modèle `try`&ndash;`catch` pour la suppression de citations. Un message d’information est journalisé chaque fois qu’une opération de suppression réussit. Un message d’erreur est journalisé chaque fois qu’une opération de suppression donne lieu à la levée d’une exception. Le message de journal lié à l’échec d’une opération de suppression inclut la trace des exceptions (*Internal/LoggerExtensions.cs*) :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Notez la manière dont l’exception est passée au délégué dans `QuoteDeleteFailed` :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

Dans le modèle de page pour la page Index, la réussite de la suppression d’une citation se traduit par l’appel de la méthode `QuoteDeleted` sur le journaliseur. Quand une citation à supprimer n’est pas trouvée, une `ArgumentNullException` est levée. L’exception est interceptée par l’instruction `try`&ndash;`catch` et journalisée par le biais de l’appel de la méthode `QuoteDeleteFailed` sur le journaliseur dans le bloc `catch` (*Pages/Index.cshtml.cs*) :

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Quand une citation est correctement supprimée, voici à quoi ressemble la sortie de la console de l’application :

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Quand la suppression d’une citation échoue, voici à quoi ressemble la sortie de la console de l’application. Notez que l’exception est incluse dans le message de journal :

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crée un délégué `Func` pour la définition d’une [étendue de journal](xref:fundamentals/logging/index#log-scopes). Les surcharges `DefineScope` permettent de passer jusqu’à trois paramètres de type à une chaîne de format nommée (modèle).

Comme c’est le cas avec la méthode `Define`, la chaîne fournie à la méthode `DefineScope` est un modèle et non pas une chaîne interpolée. Les espaces réservés sont remplis dans l’ordre dans lequel les types sont spécifiés. Les noms d’espace réservé dans le modèle doivent être descriptifs et cohérents d’un modèle à l’autre. Ils servent de noms de propriété dans les données de journal structurées. Nous vous recommandons d’utiliser la [casse Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) pour les noms d’espace réservé. Par exemple, `{Count}`, `{FirstName}`.

Définissez une [étendue de journal](xref:fundamentals/logging/index#log-scopes) à appliquer à une série de messages de journal à l’aide de la méthode [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).

L’exemple d’application a un bouton **Clear All** (Effacer tout) pour supprimer toutes les citations de la base de données. Les citations sont supprimées une par une. Chaque fois qu’une citation est supprimée, la méthode `QuoteDeleted` est appelée sur le journaliseur. Une étendue de journal est ajoutée à ces messages de journal.

Activez `IncludeScopes` dans la section du journaliseur de console *d’appsettings.json* :

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Pour créer une étendue de journal, ajoutez un champ destiné à contenir un délégué `Func` pour l’étendue. L’exemple d’application crée un champ intitulé `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*) :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Utilisez `DefineScope` pour créer le délégué. Vous pouvez spécifier jusqu’à trois types à utiliser comme arguments de modèle quand le délégué est appelé. L’exemple d’application utilise un modèle de message qui inclut le nombre de citations supprimées (un type `int`) :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Fournissez une méthode d’extension statique pour le message de journal. Incluez tous les paramètres de type pour les propriétés nommées qui s’affichent dans le modèle de message. L’exemple d’application prend un `count` de citations à supprimer et retourne `_allQuotesDeletedScope` :

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

L’étendue inclut les appels d’extension de journalisation dans un bloc `using` :

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Examinez les messages de journal dans la sortie de la console de l’application. Le résultat suivant montre trois citations supprimées, message d’étendue de journal compris :

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Journalisation](xref:fundamentals/logging/index)
