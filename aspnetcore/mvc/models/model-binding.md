---
title: Liaison de données dans ASP.NET Core
author: tdykstra
description: Découvrez comment la liaison de modèle dans ASP.NET Core MVC mappe les données des requêtes HTTP à des paramètres de méthode d’action.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 11/13/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 1dc9b41328ed78440622acc1865b6f088d394403
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037046"
---
# <a name="model-binding-in-aspnet-core"></a>Liaison de données dans ASP.NET Core

Par [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introduction à la liaison de modèle

La liaison de modèle dans ASP.NET Core MVC mappe les données des requêtes HTTP à des paramètres de méthode d’action. Les paramètres peuvent être des types simples, comme des chaînes, des entiers ou des nombres à virgule flottante, ou ils peuvent être des types complexes. Il s’agit d’une fonctionnalité intéressante de MVC, car le mappage des données entrantes à une contrepartie est un scénario souvent répété, quelle que soit la taille ou la complexité des données. MVC résout ce problème en rendant la liaison transparente : les développeurs ne doivent pas continuer à réécrire une version légèrement différente de ce même code dans chaque application. Écrire le code de votre propre convertisseur de texte en type est fastidieux et sujet à erreur.

## <a name="how-model-binding-works"></a>Fonctionnement de la liaison de modèle

Quand MVC reçoit une requête HTTP, il la route vers une méthode d’action spécifique d’un contrôleur. Il détermine la méthode d’action à exécuter en fonction de ce qui se trouve dans les données de la route, puis il lie les valeurs de la requête HTTP aux paramètres de cette méthode d’action. Considérez par exemple l’URL suivante :

`http://contoso.com/movies/edit/2`

Le modèle de route se présentant comme ceci, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` route vers le contrôleur `Movies` et sa méthode d’action `Edit`. Il accepte également un paramètre facultatif nommé `id`. Le code de la méthode d’action doit ressembler à ceci :

```csharp
public IActionResult Edit(int? id)
   ```

Remarque : Les chaînes dans l’itinéraire d’URL ne respectent pas la casse.

MVC tente de lier les données de la requête aux paramètres de l’action avec le nom. MVC recherche des valeurs pour chaque paramètre en utilisant le nom du paramètre et les noms de ses propriétés définissables publiques. Dans l’exemple ci-dessus, le seul paramètre d’action est nommé `id`, que MVC lie à la valeur portant le même nom dans les valeurs de la route. En plus des valeurs de la route, MVC lie les données des différentes parties de la requête, ceci selon un ordre défini. Voici une liste des sources de données dans l’ordre où la liaison de modèle les recherche :

1. `Form values`: Il s’agit des valeurs de formulaire qui vont dans la requête HTTP à l’aide de la méthode POST. (notamment les requêtes POST jQuery).

2. `Route values`: Le jeu de valeurs de route fournies par [routage](xref:fundamentals/routing)

3. `Query strings`: Partie de la chaîne de requête de l’URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Remarque : Valeurs de formulaire, données d’itinéraire et interrogent les chaînes sont stockées en tant que paires nom-valeur.

Comme la liaison de modèle a demandé une clé nommée `id` et qu’aucun élément n’est nommé `id` dans les valeurs de formulaire, il est passé aux valeurs de la route pour rechercher cette clé. Dans notre exemple, il trouve une correspondance. La liaison est effectuée et la valeur est convertie un entier dont la valeur est 2. La même requête qui utiliserait Edit(string id) convertirait en une chaîne « 2 ».

Jusqu’à présent, l’exemple utilise des types simples. Dans MVC, les types simples sont tous les types primitifs .NET ou les types avec un convertisseur de type chaîne. Si le paramètre de la méthode d’action était une classe comme le type `Movie`, qui contient à la fois des types simples et des comme complexes comme propriétés, la liaison de modèle de MVC pourrait le gérer correctement. Il utilise la réflexion et la récursivité pour parcourir les propriétés des types complexes en recherchant des correspondances. La liaison de modèle recherche le modèle *nom_paramètre.nom_propriété* pour lier des valeurs aux propriétés. S’il ne trouve pas de valeurs correspondantes de ce formulaire, il tente de lier en utilisant seulement le nom de propriété. Pour des types comme `Collection`, la liaison de modèle recherche des correspondances avec *nom_paramètre[index]* ou simplement avec *[index]*. La liaison de modèle traite les types `Dictionary` de la même façon, en demandant *nom_paramètre[clé]* ou simplement *[clé]*, à condition que les clés soient des types simples. Les clés qui sont prises en charge correspondent aux noms de champ HTML et aux helpers de balise générés pour le même type de modèle. Ceci permet l’aller-retour des valeurs, de sorte que les champs de formulaire restent remplis avec l’entrée de l’utilisateur pour lui faciliter la tâche, par exemple quand les données liées à partir d’une création ou d’une modification n’ont pas été validées.

Pour rendre possible la liaison de modèle, la classe doit avoir un constructeur public par défaut et des propriétés publiques accessibles en écriture à lier. Quand la liaison de modèle se produit, la classe est instanciée avec le constructeur public par défaut, puis les propriétés peuvent être définies.

Quand un paramètre est lié, la liaison de modèle cesse de rechercher des valeurs avec ce nom et elle passe à la liaison du paramètre suivant. Sinon, le comportement de la liaison de modèle par défaut définit les paramètres à leurs valeurs par défaut en fonction de leur type :

* `T[]`: À l’exception des tableaux de type `byte[]`, liaison définit les paramètres de type `T[]` à `Array.Empty<T>()`. Les tableaux de type `byte[]` ont la valeur `null`.

* Types de référence : Liaison crée une instance d’une classe avec le constructeur par défaut sans définir ses propriétés. Cependant, la liaison de modèle définit les `string` sur `null`.

* Types Nullable : Types Nullable sont définies sur `null`. Dans l’exemple ci-dessus, la liaison de modèle définit `id` sur `null`, car il est de type `int?`.

* Types de valeur : Types valeur non nullable du type `T` sont définies sur `default(T)`. Par exemple, la liaison de modèle définit un paramètre `int id` sur 0. Envisagez d’utiliser la validation de modèle ou des types Nullables au lieu de travailler avec les valeurs par défaut.

Si la liaison échoue, MVC ne génère pas d’erreur. Chaque action acceptant une entrée utilisateur doit vérifier la propriété `ModelState.IsValid`.

Remarque : Chaque entrée dans le contrôleur `ModelState` propriété est un `ModelStateEntry` contenant un `Errors` propriété. Il est rarement nécessaire interroger cette collection vous-même. Utilisez plutôt `ModelState.IsValid`.

En outre, MVC doit prendre en compte certains types de données spéciaux lors de la liaison de modèle :

* `IFormFile`, `IEnumerable<IFormFile>`: Un ou plusieurs fichiers chargés qui font partie de la requête HTTP.

* `CancellationToken`: Utilisé pour annuler l’activité dans les contrôleurs asynchrones.

Ces types peuvent être liés à des paramètres d’action ou à des propriétés sur un type de classe.

Une fois que la liaison de modèle est terminée, la [validation](validation.md) se produit. La liaison de modèle par défaut fonctionne bien pour la grande majorité des scénarios de développement. Elle est également extensible : si vous avez des besoins spécifiques, vous pouvez donc personnaliser le comportement intégré.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personnaliser le comportement de la liaison de modèle avec des attributs

MVC contient plusieurs attributs que vous pouvez utiliser pour spécifier son comportement de liaison de modèle par défaut vers une autre source. Par exemple, vous pouvez spécifier si la liaison est obligatoire pour une propriété, ou si elle ne doit jamais se produire, avec les attributs `[BindRequired]` ou `[BindNever]`. Vous pouvez aussi remplacer la source de données par défaut et spécifier la source de données du classeur de modèles. Voici une liste des attributs de liaison de modèle :

* `[BindRequired]`: Cet attribut ajoute une erreur d’état de modèle si la liaison ne peut pas se produire.

* `[BindNever]`: Indique le binder de modèle ne jamais lier à ce paramètre.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Utilisez-les pour spécifier la source de liaison exacte que vous souhaitez appliquer.

* `[FromServices]`: Cet attribut utilise [l’injection de dépendances](../../fundamentals/dependency-injection.md) pour lier les paramètres à partir des services.

* `[FromBody]`: Utilisez les formateurs configurés pour lier des données à partir du corps de demande. Le formateur est sélectionné en fonction du type de contenu de la requête.

* `[ModelBinder]`: Utilisé pour remplacer le binder de modèle par défaut, source de liaison et nom.

Les attributs sont très utiles quand vous devez remplacer le comportement par défaut de la liaison de modèle.

## <a name="customize-model-binding-and-validation-globally"></a>Personnaliser la validation et la liaison de modèle de manière globale

Le comportement du système de validation et de liaison de modèle est régi par [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) qui décrit :

* Comment un modèle doit être lié.
* Comment a lieu la validation sur le type et ses propriétés.

Vous pouvez configurer les aspects du comportement du système de manière globale en ajoutant un fournisseur de détails à [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders). MVC compte quelques fournisseurs de détails intégrés qui autorisent la configuration du comportement telle que la désactivation de la validation ou de la liaison de modèle pour certains types.

Pour désactiver la liaison de modèle sur tous les modèles d’un certain type, ajoutez un [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider) dans `Startup.ConfigureServices`. Par exemple, pour désactiver la liaison de modèle sur tous les modèles de type `System.Version` :

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

Pour désactiver la validation sur les propriétés d’un certain type, ajoutez un [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider) dans `Startup.ConfigureServices`. Par exemple, pour désactiver la validation sur les propriétés de type `System.Guid` :

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a>Lier des données mises en forme du corps de la requête

Les données des requêtes peuvent exister dans différents formats, notamment JSON, XML et beaucoup d’autres. Quand vous utilisez l’attribut [FromBody] pour indiquer que vous voulez lier un paramètre à des données du corps de la requête, MVC utilise un ensemble de formateurs configuré pour gérer les données de la requête en fonction du type de contenu. Par défaut, MVC inclut une classe `JsonInputFormatter` pour la gestion des données JSON, mais vous pouvez ajouter des formateurs supplémentaires pour la gestion du format XML et d’autres formats personnalisés.

> [!NOTE]
> Il peut y avoir au plus un paramètre par action décorée avec `[FromBody]`. Le runtime d’ASP.NET Core MVC délègue la responsabilité de lire le flux de la requête au formateur. Une fois que le flux de la requête est lu pour un paramètre, il n’est généralement pas possible de relire le flux de la requête pour lier d’autres paramètres `[FromBody]`.

> [!NOTE]
> `JsonInputFormatter` est le formateur par défaut et est basé sur [Json.NET](https://www.newtonsoft.com/json).

ASP.NET Core sélectionne les formateurs d’entrée en fonction de l’en-tête [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) et du type de paramètre, sauf si un attribut qui lui est appliqué spécifie un autre formateur. Si vous voulez utiliser XML ou un autre format, vous devez le configurer dans le fichier *Startup.cs*, mais il peut être nécessaire d’obtenir d’abord une référence à `Microsoft.AspNetCore.Mvc.Formatters.Xml` en utilisant NuGet. Votre code de démarrage doit ressembler à ceci :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Le code du fichier *Startup.cs* contient une méthode `ConfigureServices` avec un argument `services`, que vous pouvez utiliser pour créer des services pour votre application ASP.NET Core. Dans l’exemple, nous ajoutons un formateur XML en tant que service, que MVC fournira pour cette application. L’argument `options` passé dans la méthode `AddMvc` vous permet d’ajouter et de gérer des filtres, des formateurs et d’autres options du système depuis MVC dès le démarrage de l’application. Appliquez ensuite l’attribut `Consumes` aux classes de contrôleur ou aux méthodes d’action pour travailler avec le format souhaité.

### <a name="custom-model-binding"></a>Liaison de modèle personnalisée

Vous pouvez étendre la liaison de modèle en écrivant vos propres classeurs de modèles personnalisés. Découvrez plus d’informations sur la [liaison de modèle personnalisée](../advanced/custom-model-binding.md).
