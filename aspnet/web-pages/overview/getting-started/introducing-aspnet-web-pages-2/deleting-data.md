---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Présentation des Pages Web ASP.NET - suppression de la base de données | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment supprimer une entrée de la base de données individuelle. Il part du principe que vous avez terminé la série grâce à la mise à jour des données de base de données dans ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028606"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Présentation des Pages Web ASP.NET - suppression de la base de données
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment supprimer une entrée de la base de données individuelle. Il part du principe que vous avez terminé la série via [la mise à jour la base de données dans ASP.NET Web Pages](updating-data.md).
> 
> Ce que vous allez apprendre :
> 
> - La sélection d’un enregistrement individuel à partir d’une liste d’enregistrements.
> - Comment supprimer un enregistrement unique d’une base de données.
> - Comment vérifier que l’utilisateur a cliqué sur un bouton spécifique dans un formulaire.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Le `WebGrid` helper.
> - Le code SQL `Delete` commande.
> - Le `Database.Execute` méthode à exécuter une instance SQL `Delete` commande.


## <a name="what-youll-build"></a>Ce que vous allez générer

Dans le didacticiel précédent, vous avez appris à mettre à jour un enregistrement de base de données existant. Ce didacticiel est similaire, mais au lieu de la mise à jour l’enregistrement, vous allez le supprimer. Les processus sont similaires, sauf que la suppression est plus simple, ce didacticiel est donc court.

Dans le *films* page, vous allez mettre à jour le `WebGrid` helper afin qu’elle affiche un **supprimer** lien en regard de chaque film pour accompagner le **modifier** lien que vous avez ajouté précédemment.

![Page de films montrant un lien Supprimer pour chaque film](deleting-data/_static/image1.png)

Comme avec la modification, lorsque vous cliquez sur le **supprimer** lien, il permet d’accéder à une autre page, où les informations de film sont déjà dans un formulaire :

![Supprimer la page de film avec un film affiché](deleting-data/_static/image2.png)

Vous pouvez ensuite cliquer sur le bouton pour supprimer définitivement un enregistrement.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Ajout d’un lien de suppression à la liste de films

Vous commencerez en ajoutant un **supprimer** lier à la `WebGrid` helper. Ce lien est similaire à la **modifier** lien que vous avez ajouté dans le didacticiel précédent.

Ouvrez le *Movies.cshtml* fichier.

Modifier le `WebGrid` balisage dans le corps de la page en ajoutant une colonne. Voici le balisage modifié :

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

La nouvelle colonne est celle-ci :

[!code-html[Main](deleting-data/samples/sample2.html)]

La manière de configurer la grille, le **modifier** colonne est plus à gauche dans la grille et le **supprimer** colonne est plus à droite. (Il existe une virgule après le `Year` colonne maintenant, au cas où vous n’avez pas remarquer que.) Il n’a rien de spécial sur où ces colonnes lien go, et vous pouvez aussi facilement les placer en regard de chacun des autres. Dans ce cas, ils sont distincts pour les rendre plus difficile à se mélanger.

![Page de films avec des liens de modification et les détails marquée pour montrer qu’ils ne sont pas en regard de chacun des autres](deleting-data/_static/image3.png)

La nouvelle colonne affiche un lien (`<a>` élément) dont le texte indique « Supprimer ». La cible du lien (son `href` attribut) est un code qui au final correspond à quelque chose comme cette URL, avec la `id` valeur différente pour chaque film :

[!code-css[Main](deleting-data/samples/sample3.css)]

Ce lien appellera une page nommée *DeleteMovie* et passez-lui l’ID du film que vous avez sélectionné.

Ce didacticiel n’entrerai pas dans les détails de la construction de ce lien, car il est presque identique à la **modifier** lien du didacticiel précédent ([la mise à jour la base de données dans ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Création de la Page Delete

Vous pouvez désormais créer la page qui sera la cible pour le **supprimer** lien dans la grille.

> [!NOTE] 
> 
> **Important** la technique de tout d’abord sélectionner un enregistrement à supprimer, puis en utilisant une page distincte et un bouton pour confirmer le processus est extrêmement importante pour la sécurité. Comme vous avez lu dans les didacticiels précédents, ce qui *n’importe quel* tri de modification à votre site Web doit *toujours* être effectuée à l’aide d’un formulaire &mdash; , autrement dit, en utilisant une opération HTTP POST. Si vous l’avez fait possible de modifier le site suffit de cliquer sur un lien (c'est-à-dire, en utilisant une opération GET), les personnes pourrait faire des demandes simples à votre site et supprimer vos données. Même un moteur de recherche qui est l’indexation de votre site peut supprimer par inadvertance des données simplement en suivant les liens.
> 
> Lorsque votre application permet de modifier un enregistrement, vous devez présenter l’enregistrement à l’utilisateur pour la modification de toute façon. Mais vous pouvez être tenté d’ignorer cette étape pour la suppression d’un enregistrement. N’ignorez pas cette étape, cependant. (Il est également utile pour les utilisateurs voient l’enregistrement et de confirmer qu’ils vous supprimez l’enregistrement ils destinés.)
> 
> Dans un ensemble de didacticiels suivant, vous verrez comment ajouter la fonctionnalité de connexion pour un utilisateur devra se connecter avant la suppression d’un enregistrement.


Créez une page nommée *DeleteMovie.cshtml* et remplacer celui qui figure dans le fichier avec le balisage suivant :

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Ce balisage est similaire à la *EditMovie* pages, à ceci près qu’au lieu d’utiliser des zones de texte (`<input type="text">`), le balisage comprend `<span>` éléments. Il n’existe rien ici à modifier. Il vous suffit est d’afficher les détails de films afin que les utilisateurs peuvent s’assurer qu’ils vous supprimez le film de droite.

Le balisage contient déjà un lien qui permet à l’utilisateur de revenir à la page de liste de films.

Comme dans le *EditMovie* page, l’ID du film sélectionné est stocké dans un champ masqué. (Il est passé dans la page en premier lieu en tant que valeur de chaîne de requête.) Il existe un `Html.ValidationSummary` appel qui affiche les erreurs de validation. Dans ce cas, l’erreur peut être qu’aucun ID de film a été passé à la page ou que l’ID de film n’est pas valide. Cette situation peut se produire si une personne a exécuté cette page sans un film en sélectionnant d’abord le *films* page.

La légende du bouton est **supprimer un film**, et son attribut de nom est défini sur `buttonDelete`. Le `name` attribut servira dans le code pour identifier le bouton qui a envoyé le formulaire.

Vous devrez écrire du code pour 1) lire les détails de film lorsque la page s’affiche tout d’abord et (2) supprimer le film lorsque l’utilisateur clique sur le bouton.

## <a name="adding-code-to-read-a-single-movie"></a>Ajout de Code pour lire un film

En haut de la *DeleteMovie.cshtml* page, ajoutez le bloc de code suivant :

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Ce balisage est le même que le code correspondant dans le *EditMovie* page. Il obtient l’ID de film en dehors de la chaîne de requête et utilise l’ID pour lire un enregistrement à partir de la base de données. Le code inclut le test de validation (`IsInt()` et `row != null`) pour vous assurer que l’ID de film passé à la page est valide.

N’oubliez pas que ce code doit s’exécutent uniquement la première fois que la page s’exécute. Vous ne souhaitez pas relire l’enregistrement de film à partir de la base de données lorsque l’utilisateur clique sur le **supprimer un film** bouton. Par conséquent, du code pour lire le film est à l’intérieur d’un test qui dit que `if(!IsPost)` &mdash; , autrement dit, *si la demande n’est pas une opération post (envoi d’un formulaire)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Ajout de Code pour supprimer le film sélectionné

Pour supprimer le film lorsque l’utilisateur clique sur le bouton, ajoutez le code suivant à l’intérieur de l’accolade fermante de la `@` bloc :

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Ce code est similaire au code de mise à jour un enregistrement existant, mais il est plus simple. Le code exécute essentiellement une SQL `Delete` instruction.

 Comme dans le *EditMovie* page, le code se trouve dans un `if(IsPost)` bloc. Cette fois-ci, le `if()` condition est un peu plus compliquée : 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Il existe deux conditions ici. La première est que la page est soumise, comme vous l’avez vu avant &mdash; `if(IsPost)`.

La deuxième condition est `!Request["buttonDelete"].IsEmpty()`, ce qui signifie que la demande a un objet nommé `buttonDelete`. Il est vrai, c’est un moyen indirect de bouton qui a envoyé le formulaire de test. Si un formulaire contient plusieurs boutons d’envoi, uniquement le nom du bouton sur lequel l’utilisateur a cliqué sur apparaît dans la demande. Par conséquent, logiquement, si le nom d’un bouton spécifique apparaît dans la demande &mdash; ou comme indiqué dans le code, si ce bouton n’est pas vide &mdash; qui est le bouton qui a envoyé le formulaire.

Le `&&` moyen de l’opérateur « et » (AND logique). Par conséquent, l’ensemble `if` condition est...

*Cette demande est une publication (pas une demande de première)*  
  
 AND  
  
*Le* `buttonDelete` *bouton a été le bouton qui a envoyé le formulaire.*

Ce formulaire (en réalité, cette page) contienne un seul bouton, c’est pourquoi le test supplémentaire pour `buttonDelete` techniquement n’est pas obligatoire. Cependant, vous êtes sur le point d’effectuer une opération qui va supprimer définitivement les données. Vous voulez donc veillez possible que vous effectuez l’opération uniquement lorsque l’utilisateur a explicitement demandé. Par exemple, que vous développée plus loin de cette page et ajouté des autres boutons. Même dans ce cas, le code qui supprime le film s’exécute uniquement si le `buttonDelete` bouton l’utilisateur a cliqué.

Comme dans le *EditMovie* page, vous obtenez l’ID à partir du champ masqué et puis exécutez la commande SQL. La syntaxe pour la `Delete` instruction est :

`DELETE FROM table WHERE ID = value`

Il est essentiel d’inclure la `WHERE` clause et le code. Si vous omettez la clause WHERE, *tous les enregistrements dans la table seront supprimés*. Comme vous l’avez vu, vous passez la valeur d’ID pour la commande SQL à l’aide d’un espace réservé.

## <a name="testing-the-movie-delete-process"></a>Tester le processus de suppression de film

Vous pouvez maintenant tester. Exécutez le *films* page, puis cliquez sur **supprimer** en regard d’un film. Lorsque le *DeleteMovie* page s’affiche, cliquez sur **supprimer un film**.

![Supprimer la page de film avec le bouton Supprimer un film mis en surbrillance](deleting-data/_static/image4.png)

Lorsque vous cliquez sur le bouton, le code supprime les films et retourne à la liste de films. Vous pouvez y rechercher le film supprimé et confirmez qu’il est bien été supprimé.

## <a name="coming-up-next"></a>Prochaine

Le didacticiel suivant vous montre comment accorder à toutes les pages sur votre site d’une apparence commune et la disposition.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Intégralité de la Page de film (mis à jour avec les liens de suppression)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Liste complète de la Page de DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET en utilisant la syntaxe Razor](../introducing-razor-syntax-c.md)
- [Instruction DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) sur le site W3Schools

> [!div class="step-by-step"]
> [Précédent](updating-data.md)
> [Suivant](layouts.md)
