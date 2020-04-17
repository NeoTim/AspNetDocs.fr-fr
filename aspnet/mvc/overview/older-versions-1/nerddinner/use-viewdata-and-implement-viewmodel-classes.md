---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Utilisez ViewData et Implémentez les classes ViewModel (fr) Microsoft Docs
author: rick-anderson
description: 'L’étape 6 montre comment permettre le soutien à des scénarios d’édition de formulaires plus riches, et discute également de deux approches qui peuvent être utilisées pour transmettre des données des contrôleurs aux vues : ...'
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 7fa2af2a55d12bbe11b29dff594823a1e5ea0152
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541102"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Utiliser ViewData et implémenter des classes ViewModel

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 6 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 6 montre comment permettre le soutien à des scénarios d’édition de formulaires plus riches, et discute également de deux approches qui peuvent être utilisées pour transmettre des données des contrôleurs aux vues : ViewData et ViewModel.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner Step 6: ViewData et ViewModel

Nous avons couvert un certain nombre de scénarios de publication de formulaires, et discuté de la façon d’implémenter créer, mettre à jour et supprimer (CRUD) support. Nous allons maintenant prendre notre mise en œuvre DinnersController plus loin et permettre le soutien pour des scénarios d’édition de formulaires plus riches. Tout en faisant cela, nous allons discuter de deux approches qui peuvent être utilisées pour transmettre les données des contrôleurs aux vues: ViewData et ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Transmettre les données des contrôleurs à View-Templates

L’une des caractéristiques déterminantes du modèle MVC est la stricte « séparation des préoccupations » qu’il aide à appliquer entre les différents composants d’une application. Les modèles, les contrôleurs et les vues ont chacun des rôles et des responsabilités bien définis, et ils communiquent entre eux de manière bien définie. Cela permet de promouvoir la testabilité et la réutilisation du code.

Lorsqu’une classe Controller décide de rendre une réponse HTML à un client, elle est responsable de transmettre explicitement au modèle de vue toutes les données nécessaires pour rendre la réponse. Les modèles de vue ne doivent jamais effectuer de récupération de données ou de logique d’application - et devraient plutôt se limiter à seulement avoir le code de rendu qui est chassé du modèle / données qui lui sont transmis par le contrôleur.

À l’heure actuelle, les données du modèle transmises par notre classe DinnersController à nos modèles de vue sont simples et directes - une liste d’objets de dîner dans le cas de Index(), et un seul objet de dîner dans le cas de Détails (), Edit (), Créer () et Supprimer (). Comme nous ajoutons plus de capacités d’interface utilisateur à notre application, nous allons souvent avoir besoin de passer plus que juste ces données pour rendre les réponses HTML dans nos modèles de vue. Par exemple, nous pourrions vouloir changer le champ "Pays" dans notre Edit and Create vues d’être une boîte de texte HTML à une liste de décrocheurs. Plutôt que de coder dur la liste des noms de pays dans le modèle de vue, nous pourrions vouloir le générer à partir d’une liste de pays soutenus que nous peuplerons dynamiquement. Nous aurons besoin d’un moyen de passer à la fois l’objet Dîner *et* la liste des pays pris en charge de notre contrôleur à nos modèles de vue.

Examinons deux façons d’y parvenir.

### <a name="using-the-viewdata-dictionary"></a>Utilisation du dictionnaire ViewData

La classe de base Controller expose une propriété de dictionnaire "ViewData" qui peut être utilisée pour transmettre des éléments de données supplémentaires des contrôleurs aux vues.

Par exemple, pour prendre en charge le scénario où nous voulons changer la boîte de texte "Pays" dans notre vue Edit d’être une boîte de texte HTML à une liste de décrocheurs, nous pouvons mettre à jour notre méthode d’action Edit() pour passer (en plus d’un objet Dîner) un objet SelectList qui peut être utilisé comme modèle d’une liste de délabrement des pays.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Le constructeur de la SelectList ci-dessus accepte une liste de comtés pour remplir la liste de baisse avec, ainsi que la valeur actuellement sélectionnée.

Nous pouvons ensuite mettre à jour notre modèle de vue Edit.aspx pour utiliser la méthode d’assistance Html.DropDownList() au lieu de la méthode d’assistance Html.TextBox() que nous avons utilisée précédemment :

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

La méthode d’assistance Html.DropDownList() ci-dessus prend deux paramètres. Le premier est le nom de l’élément de formulaire HTML à la sortie. Le second est le modèle "SelectList" que nous avons passé via le dictionnaire ViewData. Nous utilisons le mot clé C "as" pour lancer le type dans le dictionnaire comme une Liste de sélection.

Et maintenant, lorsque nous passons notre application et accédons à l’URL */Dîners/Edit/1* dans notre navigateur, nous verrons que notre interface utilisateur de modification a été mise à jour pour afficher une liste de dropdownlist des pays au lieu d’une boîte de texte:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Parce que nous rendons également le modèle de vue d’édition de la méthode HTTP-POST Edit (dans les scénarios lorsque des erreurs se produisent), nous voulons nous assurer que nous mettons également à jour cette méthode pour ajouter la SelectList à ViewData lorsque le modèle de vue est rendu dans les scénarios d’erreur:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Et maintenant, notre scénario d’édition DinnersController prend en charge un DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Utilisation d’un modèle ViewModel

L’approche du dictionnaire ViewData a l’avantage d’être assez rapide et facile à mettre en œuvre. Certains développeurs n’aiment pas utiliser des dictionnaires à chaîne, cependant, puisque les fautes de frappe peuvent conduire à des erreurs qui ne seront pas pris à compiler-temps. Le dictionnaire ViewData non typé nécessite également l’utilisation de l’opérateur « as » ou de la coulée lorsque vous utilisez une langue fortement typée comme le C dans un modèle de vue.

Une approche alternative que nous pourrions utiliser est une approche souvent appelée le modèle "ViewModel". Lors de l’utilisation de ce modèle, nous créons des classes fortement typées qui sont optimisées pour nos scénarios de vue spécifiques, et qui exposent les propriétés pour les valeurs/contenu dynamiques nécessaires à nos modèles de vue. Nos classes de contrôleur peuvent alors remplir et passer ces classes optimisées pour afficher à notre modèle de vue à utiliser. Cela permet la sécurité de type, la vérification du temps de compilation et l’intellisense d’éditeur dans les modèles de vue.

Par exemple, pour permettre des scénarios d’édition de formulaires de dîner, nous pouvons créer une classe "DinnerFormViewModel" comme ci-dessous qui expose deux propriétés fortement typées: un objet de dîner, et le modèle SelectList nécessaire pour peupler les pays dropdownlist:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Nous pouvons ensuite mettre à jour notre méthode d’action Edit() pour créer le DinnerFormViewModel à l’aide de l’objet Dinner que nous récupérons à partir de notre référentiel, puis le transmettre à notre modèle de vue :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Nous mettrons ensuite à jour notre modèle de vue afin qu’il s’attende à un "DinnerFormViewModel" au lieu d’un objet "Dîner" en changeant l’attribut "héritière" en haut de la page edit.aspx comme si:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Une fois que nous faisons cela, l’intellisense de la propriété "Modèle" dans notre modèle de vue sera mis à jour pour refléter le modèle d’objet du type DinnerFormViewModel nous le transmettons:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Nous pouvons ensuite mettre à jour notre code de vue pour en faire fonctionner. Remarquez ci-dessous comment nous ne changeons pas les noms des éléments d’entrée que nous créons (les éléments de formulaire seront toujours nommés "Titre", "Pays") - mais nous mettons à jour les méthodes d’aide HTML pour récupérer les valeurs en utilisant la classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Nous mettrons également à jour notre méthode de publication Edit pour utiliser la classe DinnerFormViewModel lors du rendu des erreurs :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Nous pouvons également mettre à jour nos méthodes d’action Créer () pour réutiliser exactement la même classe *DinnerFormViewModel* pour permettre aux pays DropDownList au sein de ceux-ci ainsi. Voici la mise en œuvre HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Voici la mise en œuvre de la méthode HTTP-POST Créer:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Et maintenant, nos écrans Edit et Create prennent en charge les listes de descente pour choisir le pays.

### <a name="custom-shaped-viewmodel-classes"></a>Cours ViewModel en forme de coutume

Dans le scénario ci-dessus, notre classe DinnerFormViewModel expose directement l’objet modèle De dîner comme une propriété, avec une propriété de modèle SelectList de soutien. Cette approche fonctionne très bien pour les scénarios où l’interface utilisateur HTML que nous voulons créer dans notre modèle de vue correspond relativement étroitement à nos objets de modèle de domaine.

Pour les scénarios où ce n’est pas le cas, une option que vous pouvez utiliser est de créer une classe ViewModel en forme de coutume dont le modèle d’objet est plus optimisé pour la consommation par la vue - et qui pourrait sembler complètement différent de l’objet de modèle de domaine sous-jacent. Par exemple, il pourrait potentiellement exposer différents noms de propriété et/ou propriétés agrégées collectées à partir d’objets modèles multiples.

Les classes ViewModel en forme de coutume peuvent être utilisées à la fois pour transmettre les données des contrôleurs aux vues à rendre, ainsi que pour aider à gérer les données de formulaire affichées à la méthode d’action d’un contrôleur. Pour ce scénario ultérieur, vous pouvez avoir la méthode d’action mettre à jour un objet ViewModel avec les données publiées sous forme, puis utiliser l’instance ViewModel pour cartographier ou récupérer un objet modèle de domaine réel.

Les classes ViewModel en forme de coutume peuvent offrir beaucoup de flexibilité, et sont quelque chose à étudier chaque fois que vous trouvez le code de rendu dans vos modèles de vue ou le code de publication de formulaire à l’intérieur de vos méthodes d’action commence à devenir trop compliqué. C’est souvent un signe que vos modèles de domaine ne correspondent pas proprement à l’interface utilisateur que vous génévez, et qu’une classe Vuemodel en forme de coutume intermédiaire peut vous aider.

### <a name="next-step"></a>étape suivante

Examinons maintenant comment nous pouvons utiliser des partielles et des pages-maîtres pour réutilisation et partager l’interface utilisateur sur notre application.

> [!div class="step-by-step"]
> [Suivant précédent](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Next](re-use-ui-using-master-pages-and-partials.md)
