---
title: Tag Helper Cache distribué dans ASP.NET Core
author: pkellner
description: Découvrez comment utiliser le Tag Helper Cache distribué.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043916"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Tag Helper Cache distribué dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net) et [Luke Latham](https://github.com/guardrex)

Le Tag Helper Cache distribué permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans une source de cache distribué.

Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.

Le Tag Helper Cache distribué hérite de la même classe de base que le Tag Helper Cache. Tous les attributs [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) sont disponibles pour Tag Helper distribué.

Le Tag Helper Cache distribué utilise [l’injection de constructeurs](xref:fundamentals/dependency-injection#constructor-injection-behavior). L’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est passée dans le constructeur du Tag Helper Cache distribué. Si aucune implémentation concrète de `IDistributedCache` n’est créée dans `Startup.ConfigureServices` (*Startup.cs*), le Tag Helper Cache distribué utilise le même fournisseur en mémoire pour le stockage des données mises en cache que le [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Attributs de Tag Helper Cache distribué

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Attributs partagés avec le Tag Helper Cache

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

Le Tag Helper Cache distribué hérite de la même classe que le Tag Helper Cache. Pour obtenir une description de ces attributs, consultez le [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>name

| Type d’attribut | Exemple                               |
| -------------- | ------------------------------------- |
| Chaîne         | `my-distributed-cache-unique-key-101` |

`name` est obligatoire. L’attribut `name` est utilisé en tant que clé pour chaque instance de cache stockée. Contrairement au Tag Helper Cache qui affecte une clé de cache à chaque instance selon le nom de la page Razor et l’emplacement dans la page Razor, le Tag Helper Cache distribué base uniquement sa clé sur l’attribut `name`.

Exemple :

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implémentations IDistributedCache de Tag Helper Cache distribué

Il existe deux implémentations d’<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> intégrées à ASP.NET Core. L’une est basée sur SQL Server et l’autre sur Redis. Pour obtenir les détails de ces implémentations, consultez <xref:performance/caching/distributed>. Les deux implémentations impliquent la définition d’une instance de `IDistributedCache` dans `Startup`.

Aucun attribut de balise n’est spécifiquement associé à l’utilisation d’une implémentation d’`IDistributedCache`.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
