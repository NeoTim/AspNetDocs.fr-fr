---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Réutilisation de l’interface utilisateur à l’aide de Pages Et de partielles Microsoft Docs
author: rick-anderson
description: L’étape 7 examine les moyens d’appliquer le « principe DRY » dans nos modèles de vue afin d’éliminer la duplication du code, en utilisant des modèles de vue partiel et des pages maîtresses.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f381c4424a9fa0718cd234beeb01ce41bc4ca61e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542597"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Réutiliser l’interface utilisateur avec des pages maîtres et des vues partielles

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 7 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 7 examine les moyens d’appliquer le « principe DRY » dans nos modèles de vue afin d’éliminer la duplication du code, en utilisant des modèles de vue partiel et des pages maîtresses.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner Step 7: Partielles et Pages Maîtresses

L’une des philosophies de conception ASP.NET MVC embrasse est le principe «Ne vous répétez pas» (communément appelé «DRY»). Une conception DRY permet d’éliminer la duplication du code et de la logique, ce qui rend finalement les applications plus rapides à construire et plus faciles à entretenir.

Nous avons déjà vu le principe DRY appliqué dans plusieurs de nos scénarios NerdDinner. Quelques exemples : notre logique de validation est mise en œuvre dans notre couche de modèle, ce qui lui permet d’être appliquée à la fois à travers modifier et créer des scénarios dans notre contrôleur ; nous ré utilisons le modèle de vue « NotFound » dans les méthodes d’action Edit, Détails et Supprimer ; nous utilisons un modèle de nommage de convention avec nos modèles de vue, qui élimine la nécessité de spécifier explicitement le nom lorsque nous appelons la méthode d’aide View() ; et nous ré utilisons la classe DinnerFormViewModel pour les scénarios d’action Edit et Create.

Examinons maintenant les moyens d’appliquer le « principe DRY » dans nos modèles de vue afin d’éliminer également la duplication du code.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Re-visiter nos modèles d’édition et de création de vue

Actuellement, nous utilisons deux modèles de vue différents - "Edit.aspx" et "Create.aspx" - pour afficher notre interface utilisateur forme de dîner. Une comparaison visuelle rapide d’entre eux met en évidence la similitude qu’ils sont. Voici à quoi ressemble la forme de création :

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Et voici à quoi ressemble notre formulaire "Edit":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Il n’y a pas beaucoup de différence? Outre le titre et le texte d’en-tête, la disposition du formulaire et les contrôles d’entrée sont identiques.

Si nous ouvrons les modèles de vue "Edit.aspx" et "Create.aspx", nous constaterons qu’ils contiennent la mise en page de formulaires identiques et le code de contrôle des entrées. Cette duplication signifie que nous finissons par avoir à faire des changements deux fois chaque fois que nous introduisons ou changer une nouvelle propriété de dîner - ce qui n’est pas bon.

### <a name="using-partial-view-templates"></a>Utilisation de modèles de vue partielle

ASP.NET MVC prend en charge la capacité de définir des modèles de « vue partielle » qui peuvent être utilisés pour encapsuler la logique de rendu de vue pour une sous-partie d’une page. Les « partielles » fournissent un moyen utile de définir la logique de rendu de vue une fois, puis de la réutiliser à plusieurs endroits à travers une application.

Pour aider à "DRY-up" notre Edit.aspx et Create.aspx voir le modèle de duplication, nous pouvons créer un modèle de vue partiel nommé "DinnerForm.ascx" qui encapsule la disposition du formulaire et les éléments d’entrée communs aux deux. Nous le ferons en cliquant à droite sur notre répertoire /Vues/Dîners et en choisissant la commande du menu "Add-&gt;View":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Ceci affichera le dialogue " Add View ". Nous nommerons la nouvelle vue que nous voulons créer "DinnerForm", sélectionnez la case à cocher "Créer une vue partielle" dans le dialogue, et indiquer que nous allons lui passer une classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio créera pour nous un nouveau modèle de vue "DinnerForm.ascx".

Nous pouvons ensuite copier /coller la mise en page du formulaire en double / code de contrôle des entrées de notre Edit.aspx/ Create.aspx modèles de vue dans notre nouveau "DinnerForm.ascx" modèle de vue partielle:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Nous pouvons ensuite mettre à jour nos modèles de vue Edit et Créer pour appeler le modèle partiel DinnerForm et éliminer la duplication du formulaire. Nous pouvons le faire en appelant Html.RenderPartial ("DinnerForm") dans nos modèles de vue:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Vous pouvez explicitement qualifier le chemin du modèle partiel que vous souhaitez lorsque vous appelez Html.RenderPartial (par exemple : «Vues/Dîners/DinnerForm.ascx»). Dans notre code ci-dessus, cependant, nous profitons du modèle de nommage basé sur la convention au sein de ASP.NET MVC, et il suffit de spécifier "DinnerForm" comme le nom de la partie à rendre. Lorsque nous faisons cela ASP.NET MVC regardera d’abord dans le répertoire de vues basée sur la convention (pour DinnersController ce serait /Vues / Dîners). S’il ne trouve pas le modèle partiel là-bas, il sera alors le chercher dans le /Views / Répertoire partagé.

Lorsque Html.RenderPartial() est appelé avec juste le nom de la vue partielle, ASP.NET MVC passera à la vue partielle des mêmes objets de dictionnaire de modèle et de ViewData utilisés par le modèle de vue d’appel. Alternativement, il existe des versions surchargées de Html.RenderPartial () qui vous permettent de passer un autre objet modèle et / ou ViewData dictionnaire pour la vue partielle à utiliser. Ceci est utile pour les scénarios où vous ne voulez passer qu’un sous-ensemble du modèle complet / ViewModel.

| **Sujet secondaire: &lt;Pourquoi&gt; % &lt;%&gt;au lieu de % ?** |
| --- |
| Une des choses subtiles que vous pourriez avoir remarqué &lt;avec&gt; le code &lt;ci-dessus, c’est que nous utilisons un bloc % % au lieu d’un bloc de % à %&gt; lorsque vous appelez Html.RenderPartial(). &lt;% blocs&gt; de % dans ASP.NET indiquent qu’un développeur veut rendre &lt;une valeur spécifiée (par exemple : % « Bonjour » %&gt; rendrait « Bonjour »). &lt;%&gt; % blocs au lieu d’indiquer que le développeur veut exécuter le code, &lt;et que toute sortie rendue&gt;en eux doit être faite explicitement (par exemple: % Response.Write ("Bonjour") % . La raison pour &lt;laquelle&gt; nous utilisons un bloc % % avec notre code Html.RenderPartial ci-dessus est parce que la méthode Html.RenderPartial() ne retourne pas une chaîne, et extrade plutôt le contenu directement sur le flux de sortie du modèle de vue d’appel. Il le fait pour des raisons d’efficacité de performance, et ce faisant, il évite la nécessité de créer un objet à chaîne temporaire (potentiellement très grand). Cela réduit l’utilisation de la mémoire et améliore le débit global d’application. Une erreur commune lors de l’utilisation html.RenderPartial() est d’oublier d’ajouter &lt;un&gt; semi-colon à la fin de l’appel quand il est dans un bloc % % %. Par exemple, ce code provoquera une &lt;erreur de compilateur: % Html.RenderPartial ("DinnerForm") %&gt; Vous devez plutôt écrire: &lt;% Html.RenderPartial ("DinnerForm"); %&gt; C’est parce que &lt;% %&gt; blocs sont des instructions de code autonomes, et lors de l’utilisation des instructions de code C doit être résiliée avec un semi-colon. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Utilisation de modèles de vue partiel pour clarifier le code

Nous avons créé le modèle de vue partiel "DinnerForm" pour éviter de dupliquer la logique de rendu de vue dans plusieurs endroits. C’est la raison la plus courante pour créer des modèles de vue partiel.

Parfois, il est toujours logique de créer des vues partielles, même quand ils ne sont appelés qu’en un seul endroit. Les modèles de vue très compliqués peuvent souvent devenir beaucoup plus faciles à lire lorsque leur logique de rendu de vue est extraite et divisée en un ou plusieurs modèles partiels bien nommés.

Par exemple, considérez l’extrait de code ci-dessous du fichier Site.master dans notre projet (que nous examinerons sous peu). Le code est relativement simple à lire - en partie parce que la logique d’afficher un lien de connexion / logout en haut à droite de l’écran est encapsulé dans le "LogOnUserControl" partielle:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Chaque fois que vous vous trouvez se confondre en essayant de comprendre le balisage html / code dans un view-template, considérez si elle ne serait pas plus claire si certains d’entre eux a été extrait et refactorisé en vues partielles bien nommées.

### <a name="master-pages"></a>Pages maîtres

En plus de prendre en charge des vues partielles, ASP.NET MVC prend également en charge la possibilité de créer des modèles de « page maîtresse » qui peuvent être utilisés pour définir la mise en page commune et le html de haut niveau d’un site. Les contrôles des titulaires de places de contenu peuvent ensuite être ajoutés à la page principale pour identifier les régions remplaçables qui peuvent être remplacées ou « remplies » par des vues. Cela fournit un moyen très efficace (et DRY) d’appliquer une mise en page commune à travers une application.

Par défaut, les nouveaux projets MVC ASP.NET ont un modèle de page principale qui leur est automatiquement ajouté. Cette page principale s’appelle "Site.master" et vit dans le dossier "Views"Partagés:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Le fichier Par défaut Site.master ressemble ci-dessous. Il définit le html extérieur du site, ainsi qu’un menu pour la navigation en haut. Il contient deux contrôles remplaçables des titulaires de places de contenu , l’un pour le titre, et l’autre pour l’endroit où le contenu principal d’une page doit être remplacé :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tous les modèles de vue que nous avons créés pour notre application NerdDinner ("List", "Détails", "Edit", "Create", "NotFound", etc.) ont été basés sur ce modèle Site.master. Ceci est indiqué via l’attribut "MasterPageFile" qui &lt;a été&gt; ajouté par défaut à la directive top % - Page % lorsque nous avons créé nos vues à l’aide du dialogue "Add View":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ce que cela signifie, c’est que nous pouvons modifier le contenu Site.master, et avoir les modifications automatiquement appliquées et utilisées lorsque nous rendons l’un de nos modèles de vue.

Mettons à jour la section d’en-tête de notre site.master afin que l’en-tête de notre application soit "NerdDinner" au lieu de "My MVC Application". Mettons également à jour notre menu de navigation afin que le premier onglet soit « Trouver un dîner » (géré par la méthode d’action HomeController’s Index() ), et ajoutons un nouvel onglet appelé « Host a Dinner » (manipulé par la méthode d’action Créer du DinnersController)) :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Lorsque nous lisons le fichier Site.master et actualisons notre navigateur, nous verrons nos modifications d’en-tête apparaître sur toutes les vues de notre application. Par exemple :

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Et avec l’URL */Dinners/Edit/[id]:*

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>étape suivante

Les partielles et les pages maîtresses offrent des options très flexibles qui vous permettent d’organiser proprement les vues. Vous constaterez qu’ils vous aideront à éviter de dupliquer le contenu/code de vue, et à rendre vos modèles de vue plus faciles à lire et à entretenir.

Revoyons maintenant le scénario d’inscription que nous avons construit plus tôt et permettons un support de mise à l’emploi évolutif.

> [!div class="step-by-step"]
> [Suivant précédent](use-viewdata-and-implement-viewmodel-classes.md)
> [Next](implement-efficient-data-paging.md)
