---
title: Tag Helper Image dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Image.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 5eb74a6698911a1c594d11573192cb1b9ed53b49
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030106"
---
# <a name="image-tag-helper-in-aspnet-core"></a>Tag Helper Image dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net)

Le Tag Helper Image améliore la balise `<img>` afin de fournir un comportement de cache busting pour les fichiers image statiques.

Une chaîne de cache busting est une valeur unique représentant le hachage du fichier image statique ajouté à l’URL de la ressource. La chaîne unique invite les clients (et certains proxys) à recharger l’image depuis le serveur web hôte et non depuis le cache du client.

Si la source de l’image (`src`) est un fichier statique sur le serveur web hôte :

* Une chaîne de cache-busting unique est ajoutée en tant que paramètre de requête à la source de l’image.
* Si le fichier sur le serveur web hôte change, une URL de demande unique qui inclut le paramètre de demande mis à jour est générée.

Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.

## <a name="image-tag-helper-attributes"></a>Attributs de Tag Helper Image

### <a name="src"></a>src

Pour activer le Tag Helper Image, l’attribut `src` est obligatoire sur l’élément `<img>`.

La source de l’image (`src`) doit pointer vers un fichier statique physique sur le serveur. Si la source `src` est un URI distant, le paramètre de la chaîne de requête de cache-busting n’est pas généré.

### <a name="asp-append-version"></a>asp-append-version

Quand `asp-append-version` est spécifié avec une valeur `true` et un attribut `src`, le Tag Helper Image est appelé.

L’exemple suivant utilise un Tag Helper Image :

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true" />
```

Si le fichier statique existe dans le répertoire */wwwroot/images/*, le code HTML généré est semblable au suivant (le hachage sera différent) :

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM" />
```

La valeur affectée au paramètre `v` est la valeur de hachage du fichier *asplogo.png* sur le disque. Si le serveur web ne peut pas obtenir l’accès en lecture au fichier statique, aucun paramètre `v` n’est ajouté à l’attribut `src` dans le balisage affiché.

## <a name="hash-caching-behavior"></a>Comportement de mise en cache du hachage

Le Tag Helper Image utilise le fournisseur de cache sur le serveur web local pour stocker le hachage `Sha512` calculé d’un fichier donné. Si le fichier est demandé plusieurs fois, le hachage n’est pas recalculé. Le cache est invalidé par un observateur de fichier qui est associé au fichier quand le hachage `Sha512` du fichier est calculé. Quand le fichier change sur le disque, un nouveau hachage est calculé et mis en cache.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:performance/caching/memory>
