---
uid: web-api/overview/advanced/dependency-injection
title: L’Injection de dépendances dans ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET pour ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 138ccb5800e801d382c11e3989ec3e3c074a79fe
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115702"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Injection de dépendances dans ASP.NET Web API 2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Web API 2
> - [Unity Application Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (version 5 fonctionne également)

## <a name="what-is-dependency-injection"></a>Quelle est l’Injection de dépendances ?

Une *dépendance* est un objet qui nécessite un autre objet. Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données. Nous allons illustrer avec un exemple. Tout d’abord, nous allons définir un modèle de domaine :

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide d’Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Maintenant, nous allons définir un contrôleur d’API Web qui prend en charge les requêtes GET `Product` entités. (Je laisse les POST et les autres méthodes par souci de simplicité.) Voici une première tentative de :

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Notez que la classe de contrôleur dépend `ProductRepository`, et nous allons laisser le contrôleur de créer le `ProductRepository` instance. Toutefois, il est déconseillé de coder en dur la dépendance de cette façon, pour plusieurs raisons.

- Si vous souhaitez remplacer `ProductRepository` avec une implémentation différente, vous devez également modifier la classe de contrôleur.
- Si le `ProductRepository` possède des dépendances, vous devez configurer dans le contrôleur. Pour un grand projet avec plusieurs contrôleurs, votre code de configuration devienne répartis sur votre projet.
- Il est difficile de test unitaire, étant donné que le contrôleur est codé en dur pour interroger la base de données. Pour un test unitaire, vous devez utiliser un référentiel simulacre ou stub, ce qui n’est pas possible avec la conception actuelle.

Nous pouvons résoudre ces problèmes en *injection* le référentiel dans le contrôleur. Tout d’abord, refactoriser la `ProductRepository` classe dans une interface :

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Puis fournissez le `IProductRepository` comme paramètre de constructeur :

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Cet exemple utilise [l’injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Vous pouvez également utiliser *l’injection de setter*vous permet de définir la dépendance par une méthode setter ou une propriété.

Mais maintenant il existe un problème, étant donné que votre application ne crée pas directement le contrôleur. API Web crée le contrôleur lorsqu’il achemine la demande et API Web ne sait pas quoi que ce soit sur `IProductRepository`. Il s’agit là qu’intervient le résolveur de dépendance d’API Web.

## <a name="the-web-api-dependency-resolver"></a>Le résolveur de dépendance d’API Web

API Web définit les **IDependencyResolver** interface pour la résolution des dépendances. Voici la définition de l’interface :

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Le **IDependencyScope** interface possède deux méthodes :

- **GetService** crée une instance d’un type.
- **GetServices** crée une collection d’objets d’un type spécifié.

Le **IDependencyResolver** hérite de la méthode **IDependencyScope** et ajoute le **BeginScope** (méthode). Je parlerai étendues plus loin dans ce didacticiel.

Lors de l’API Web crée une instance de contrôleur, il appelle d’abord **IDependencyResolver.GetService**, en passant le type de contrôleur. Vous pouvez utiliser ce hook d’extensibilité pour créer le contrôleur, résolution des dépendances. Si **GetService** retourne null, recherche un constructeur sans paramètre sur la classe de contrôleur API Web.

## <a name="dependency-resolution-with-the-unity-container"></a>Résolution des dépendances avec le conteneur Unity

Bien que vous pouvez écrire un complète **IDependencyResolver** implémentation à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre les API Web et les conteneurs d’inversion de contrôle existants.

Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez des types auprès du conteneur et ensuite utilisez le conteneur pour créer des objets. Le conteneur détermine automatiquement les relations de dépendance. De nombreux conteneurs d’inversion de contrôle vous permettent également de vous permettent de contrôler des éléments tels que la durée de vie et la portée.

> [!NOTE]
> « IoC » est l’acronyme « d’inversion de contrôle », qui est un modèle général où un framework appelle du code d’application. Un conteneur IoC construit vos objets, ce qui le flux habituel de contrôle « inverse ».

Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) à partir de Microsoft Patterns &amp; pratiques. (Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), et [StructureMap ](http://structuremap.github.io/documentation/).) Vous pouvez utiliser le Gestionnaire de Package NuGet pour installer Unity. À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Si le **GetService** méthode ne peut pas résoudre un type, elle doit retourner **null**. Si le **GetServices** méthode ne peut pas résoudre un type, elle doit retourner un objet de collection vide. Ne pas lever d’exceptions pour les types inconnus.

## <a name="configuring-the-dependency-resolver"></a>Configurer le résolveur de dépendance

Définir le résolveur de dépendance sur le **DependencyResolver** propriété de global **HttpConfiguration** objet.

Le code suivant enregistre le `IProductRepository` interface avec Unity et crée ensuite un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Portée des dépendances et durée de vie de contrôleur

Les contrôleurs sont créés par demande. Pour gérer les durées de vie des objets, **IDependencyResolver** utilise le concept d’un *étendue*.

Le résolveur de dépendance attachée à la **HttpConfiguration** objet a une portée globale. Lors de l’API Web crée un contrôleur, il appelle **BeginScope**. Cette méthode retourne un **IDependencyScope** qui représente une étendue enfant.

API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur. Lors de la demande est terminée, les API Web appelle **Dispose** sur l’étendue enfant. Utilisez le **Dispose** méthode pour supprimer les dépendances du contrôleur.

Comment implémenter **BeginScope** varie selon le conteneur IoC. Pour Unity, étendue correspond à un conteneur enfant :

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La plupart des conteneurs IoC ont des équivalents similaire.
