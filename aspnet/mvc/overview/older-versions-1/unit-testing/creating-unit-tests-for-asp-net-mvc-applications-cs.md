---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Création de Tests unitaires pour les Applications ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Découvrez comment créer des tests unitaires pour les actions de contrôleur. Dans ce didacticiel, Stephen Walther montre comment tester si une action de contrôleur retourne une section...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 1193d7dc6fc29dfdac5637c9391a82f9f3566073
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59407729"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Création de tests unitaires pour les applications ASP.NET MVC (C#)

par [Stephen Walther](https://github.com/StephenWalther)

[Télécharger le PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Découvrez comment créer des tests unitaires pour les actions de contrôleur. Dans ce didacticiel, Stephen Walther montre comment tester si une action de contrôleur retourne une vue particulière, retourne un jeu de données particulier ou retourne un autre type de résultat d’action.


L’objectif de ce didacticiel consiste à montrer comment vous pouvez écrire des tests unitaires pour les contrôleurs dans votre ASP.NET MVC applications. Explique comment créer trois différents types de tests unitaires. Vous découvrez comment tester la vue retournée par une action de contrôleur, comment afficher les données retournées par une action de contrôleur de test et comment tester si une action de contrôleur vous redirige vers une deuxième action de contrôleur.

## <a name="creating-the-controller-under-test"></a>Création du contrôleur de Test

Nous allons commencer en créant le contrôleur que nous souhaitons tester. Le contrôleur, nommé le `ProductController`, est contenue dans le Listing 1.

**Liste 1 : `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

Le `ProductController` contient deux méthodes d’action nommées `Index()` et `Details()`. Les deux méthodes d’action retournent une vue. Notez que le `Details()` action accepte un paramètre nommé ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test de la vue retournée par un contrôleur

Imaginez que nous voulons tester ou non la `ProductController` retourne la vue de droite. Nous souhaitons vous assurer qu’au moment où le `ProductController.Details()` action est appelée, la vue de détails est retournée. La classe de test dans le Listing 2 contient un test unitaire pour tester la vue retournée par la `ProductController.Details()` action.

**Listing 2 : `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

La classe dans le Listing 2 inclut une méthode de test nommée `TestDetailsView()`. Cette méthode contient trois lignes de code. La première ligne de code crée une nouvelle instance de la `ProductController` classe. La deuxième ligne de code appelle le contrôleur `Details()` méthode d’action. Enfin, la dernière ligne de contrôles du code ou non la vue retournée par la `Details()` action est le mode Détails.

Le `ViewResult.ViewName` propriété représente le nom de la vue retournée par un contrôleur. Un avertissement sur le test de cette propriété. Il existe deux façons qu’un contrôleur peut retourner une vue. Un contrôleur peut retourner explicitement un affichage semblable à ceci :

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

Vous pouvez également le nom de la vue peut être déduit à partir du nom de l’action de contrôleur comme suit :

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Cette action de contrôleur retourne également une vue nommée `Details`. Toutefois, le nom de la vue est déduit à partir du nom d’action. Si vous souhaitez tester le nom de la vue, vous devez retourner explicitement le nom de la vue à partir de l’action du contrôleur.

Vous pouvez exécuter le test unitaire dans le Listing 2 soit en entrant la combinaison de touches **Ctrl + R, A** ou en cliquant sur le **exécuter tous les Tests de la Solution** bouton (voir Figure 1). Si le test réussit, vous verrez la fenêtre Résultats des tests dans la Figure 2.


[![RAnnuler l’ensemble des Tests dans la Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Figure 01**: Exécuter tous les Tests dans la Solution ([cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Success !](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Figure 02**: Opération réussie ([Cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>La vue de données de test retourné par un contrôleur

Un contrôleur MVC transmet les données à une vue à l’aide de ce que l'on appelle *`View Data`*. Par exemple, imaginez que vous souhaitez afficher les détails d’un produit spécifique lorsque vous appelez le `ProductController Details()` action. Dans ce cas, vous pouvez créer une instance d’un `Product` classe (défini dans votre modèle) et passez l’instance à la `Details` vue en tirant parti de `View Data`.

Modifié `ProductController` dans le Listing 3 inclut une mise à jour `Details()` action qui retourne un produit.

**Liste 3 : `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

Tout d’abord, le `Details()` action crée une nouvelle instance de la `Product` classe qui représente un ordinateur portable. Ensuite, l’instance de la `Product` classe est passée comme deuxième paramètre à la `View()` (méthode).

Vous pouvez écrire des tests unitaires pour vérifier si les données attendues sont contenue dans la vue données. Le test unitaire dans les tests de la liste 4 déterminant si un produit qui représente un ordinateur portable est retourné lorsque vous appelez le `ProductController Details()` méthode d’action.

**Liste 4 – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

Dans la liste 4, le `TestDetailsView()` méthode teste les données d’affichage retournée en appelant le `Details()` (méthode). Le `ViewData` est exposée en tant que propriété sur le `ViewResult` retournée en appelant le `Details()` (méthode). Le `ViewData.Model` propriété contient le produit passé à la vue. Le test vérifie simplement que le produit contenu dans les données de la vue a le nom d’ordinateur portable.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Test du résultat d’Action retournée par un contrôleur

Une action de contrôleur plus complexe pouvant retourner différents types de résultats d’actions en fonction des valeurs des paramètres transmis à l’action du contrôleur. Une action de contrôleur peut retourner divers types de résultats d’actions, y compris un `ViewResult`, `RedirectToRouteResult`, ou `JsonResult`.

Par exemple, la modification `Details()` action dans la liste 5 retourne le `Details` afficher lorsque vous transmettez un Id de produit valide à l’action. Si vous passez un produit non valide Id--un Id avec une valeur inférieure à 1--, puis vous êtes redirigé vers le `Index()` action.

**Liste 5 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Vous pouvez tester le comportement de la `Details()` action avec le test unitaire de la liste 6. Le test unitaire de la liste 6 vérifie que vous êtes redirigé vers la `Index` afficher lorsqu’un Id avec la valeur -1 est passé à la `Details()` (méthode).

**Liste 6 : `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Lorsque vous appelez le `RedirectToAction()` méthode dans une action de contrôleur, l’action du contrôleur retourne un `RedirectToRouteResult`. Les vérifications de test si le `RedirectToRouteResult` redirige l’utilisateur à une action de contrôleur nommée `Index`.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à créer des tests unitaires pour les actions de contrôleur MVC. Tout d’abord, vous avez appris à vérifier si la vue de droite est retournée par une action de contrôleur. Vous avez appris comment utiliser le `ViewResult.ViewName` propriété pour vérifier le nom d’une vue.

Ensuite, nous avons examiné comment vous pouvez tester le contenu de `View Data`. Vous avez appris comment vérifier si le produit approprié a été retourné dans `View Data` après l’appel d’une action de contrôleur.

Enfin, nous avons abordé la façon dont vous pouvez tester si les différents types de résultats d’action sont renvoyées à partir d’une action de contrôleur. Vous avez appris comment tester si un contrôleur retourne un `ViewResult` ou un `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Suivant](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
