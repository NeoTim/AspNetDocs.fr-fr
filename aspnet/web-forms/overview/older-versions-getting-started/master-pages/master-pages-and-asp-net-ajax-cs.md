---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Pages maîtres et ASP.NET AJAX (c#) | Microsoft Docs
author: rick-anderson
description: Présente les options d’à l’aide d’ASP.NET AJAX et les pages maîtres. Examine à l’aide de la classe ScriptManagerProxy ; Explique comment les différents fichiers JS sont chargés dependi...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc435e4b2b1eeedaab424695715e5ec51e116d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381859"
---
# <a name="master-pages-and-aspnet-ajax-c"></a>Pages maîtres et ASP.NET AJAX (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Présente les options d’à l’aide d’ASP.NET AJAX et les pages maîtres. Examine à l’aide de la classe ScriptManagerProxy ; Explique comment les différents fichiers JS sont chargés selon que le ScriptManager est utilisé dans le contrôleur de page ou la page de contenu.


## <a name="introduction"></a>Introduction

Sur plusieurs années, ont été création de plus en plus de développeurs [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-activé des applications web. Un site Web compatible AJAX utilise un nombre de technologies web associés pour offrir une expérience utilisateur plus réactive. Création d’applications ASP.NET compatibles AJAX est étonnamment facile grâce à Microsoft [infrastructure ASP.NET AJAX](../../../../ajax/index.md). ASP.NET AJAX est intégrée à ASP.NET 3.5 et Visual Studio 2008 ; Il est également disponible en téléchargement séparé pour les applications ASP.NET 2.0.

Lorsque vous créez des pages web compatibles AJAX avec l’infrastructure ASP.NET AJAX, vous devez ajouter précisément un [contrôle ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) à chaque page qui utilise l’infrastructure. Comme son nom l’indique, ScriptManager gère le script côté client utilisé dans les pages web compatibles AJAX. Au minimum, ScriptManager émet un code HTML qui indique au navigateur pour télécharger les fichiers JavaScript cette composition de la bibliothèque de Client ASP.NET AJAX. Il peut également être utilisé pour inscrire des fichiers JavaScript personnalisés, les services web compatibles sur le script et fonctionnalités de service d’application personnalisée.

Si votre maître utilise de site des pages (comme il le devrait), il est nécessairement inutile ajouter un contrôle ScriptManager à chaque page de contenu unique ; au lieu de cela, vous pouvez ajouter un contrôle ScriptManager à la page maître. Ce didacticiel montre comment ajouter le contrôle ScriptManager à la page maître. Il explique également comment utiliser le contrôle ScriptManagerProxy pour inscrire des scripts personnalisés et des services de script dans une page de contenu spécifique.

> [!NOTE]
> Ce didacticiel ne pas Explorer conception ou la création d’applications web compatibles AJAX avec l’infrastructure ASP.NET AJAX. Pour plus d’informations sur l’utilisation d’AJAX, consultez le [vidéos d’ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) et [didacticiels](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), ainsi que les ressources répertoriées dans la section obtenir des informations supplémentaires à la fin de ce didacticiel.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examiner le balisage émis par le contrôle ScriptManager

Le contrôle ScriptManager émet une balise qui indique au navigateur pour télécharger les fichiers JavaScript cette composition de la bibliothèque de Client ASP.NET AJAX. Il ajoute également un peu de code JavaScript intégré à la page qui initialise cette bibliothèque. Le balisage suivant montre le contenu est ajouté à la sortie rendue d’une page qui inclut un contrôle ScriptManager :


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Le `<script src="url"></script>` balises demandent au navigateur à télécharger et exécuter le fichier JavaScript à *url*. Le ScriptManager émet trois de ces balises ; une référence au fichier `WebResource.axd`, tandis que les deux autres référencent le fichier `ScriptResource.axd`. Ces fichiers n’existent pas réellement en tant que fichiers dans votre site Web. Au lieu de cela, lorsqu’une demande de l’un de ces fichiers arrive au niveau du serveur web, le moteur ASP.NET examine la chaîne de requête et retourne le contenu de JavaScript approprié. Le script fourni par ces trois fichiers JavaScript externes constituent la bibliothèque de Client de l’infrastructure ASP.NET AJAX. L’autre `<script>` balises émis par le ScriptManager incluent un script inline qui initialise cette bibliothèque.

Les références de script externe et un script inline émis par le ScriptManager sont essentiels pour une page qui utilise l’infrastructure ASP.NET AJAX, mais il n’est pas nécessaire pour les pages qui n’utilisent pas le framework. Par conséquent, vous pouvez motif qu’il convient d’ajouter uniquement un ScriptManager à ces pages qui utilisent l’infrastructure ASP.NET AJAX. Et cela suffit, mais si vous disposez de plusieurs pages qui utilisent l’infrastructure, vous obtiendrez Ajout du contrôle ScriptManager à toutes les pages - une tâche répétitive, pour le moins. Vous pouvez également ajouter un ScriptManager à la page maître, qui injecte ensuite ce script nécessaire dans toutes les pages de contenu. Avec cette approche, vous n’avez pas besoin de n’oubliez pas d’ajouter un ScriptManager vers une nouvelle page qui utilise l’infrastructure ASP.NET AJAX, car il est déjà fourni par la page maître. Parcours de l’étape 1 en ajoutant un ScriptManager à la page maître.

> [!NOTE]
> Si vous envisagez d’inclure une fonctionnalité AJAX au sein de l’interface utilisateur de votre page maître, puis, vous devez en la matière : vous devez inclure le ScriptManager dans la page maître.


L’inconvénient de l’ajout de ScriptManager à la page maître est que le script ci-dessus est émis dans *chaque* page, indépendamment de si elle est nécessaire. Cela entraîne clairement un gaspillage de bande passante pour les pages qui ont le ScriptManager inclus (via la page maître), mais n’utilisez pas toutes les fonctionnalités de l’infrastructure ASP.NET AJAX. Mais simplement combien la bande passante est gaspillée ?

- Le contenu réel émis par le ScriptManager (illustré ci-dessus) des totaux d’un peu plus de 1 Ko.
- Les trois fichiers de script externe référencés par le `<script>` élément, toutefois, comprendre environ 450 Ko de données non compressées ; dans un site Web qui utilise la compression gzip, cette bande passante totale peut être réduite près de 100 Ko. Toutefois, ces fichiers de script sont mis en cache par le navigateur pendant un an, ce qui signifie qu’ils doivent uniquement être téléchargé en une seule fois et peuvent ensuite être réutilisés dans d’autres pages sur le site.

Dans le meilleur des cas, puis, lorsque les fichiers de script sont mis en cache, le coût total est 1 Ko, qui est négligeable. Dans le pire des cas, toutefois - c'est-à-dire lorsque les fichiers de script n’ont pas encore été téléchargées et le serveur web n’utilise pas toutes les formes de compression : le positionnement de la bande passante est environ 450 Ko, ce qui peut ajouter n’importe où à partir d’une seconde ou deux via une connexion haut débit à une minute pour  utilisateurs sur les modems d’accès à distance. La bonne nouvelle est que les fichiers de script externe sont mis en cache par le navigateur, ce pire des cas se produit rarement.

> [!NOTE]
> Si vous vous sentez toujours placer le contrôle ScriptManager dans la page maître, envisagez le formulaire Web (le `<form runat="server">` balisage dans la page maître). Chaque page ASP.NET qui utilise le modèle de publication (postback) doit inclure exactement un seul formulaire Web. Ajout d’un formulaire Web ajoute du contenu supplémentaire : un nombre de champs de formulaire masqué, le `<form>` balise lui-même, et, si nécessaire, une fonction JavaScript pour initier une publication à partir du script. Ce balisage n’est pas nécessaire pour les pages qui ne publication (postback). Ce balisage superflu peut être supprimé en supprimant le formulaire Web à partir de la page maître et en l’ajoutant manuellement à chaque page de contenu qui en a besoin. Toutefois, les avantages d’avoir le formulaire Web dans la page maître compensent les inconvénients à partir de sorte qu’il est ajouté inutilement à certaines pages de contenu.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Étape 1 : Ajout d’un contrôle ScriptManager à la Page maître

Chaque page web qui utilise l’infrastructure ASP.NET AJAX doit contenir exactement un seul contrôle ScriptManager. En raison de cette exigence, il est généralement judicieux de placer un seul contrôle ScriptManager sur la page maître afin que toutes les pages de contenu ont le contrôle ScriptManager automatiquement inclus. En outre, le ScriptManager doit être placée avant les contrôles serveur ASP.NET AJAX, telles que les contrôles UpdatePanel et UpdateProgress. Par conséquent, il est préférable de placer le ScriptManager avant tous les contrôles ContentPlaceHolder dans le formulaire Web.

Ouvrez le `Site.master` page maître et ajoutez un contrôle ScriptManager à la page dans un formulaire Web, mais avant que le `<div id="topContent">` élément (voir Figure 1). Si vous utilisez Visual Web Developer 2008 ou Visual Studio 2008, le contrôle ScriptManager se trouve dans la boîte à outils dans l’onglet Extensions AJAX. Si vous utilisez Visual Studio 2005, vous devez tout d’abord installer l’infrastructure ASP.NET AJAX et ajouter les contrôles à la boîte à outils. Visitez le [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) pour obtenir le framework pour ASP.NET 2.0.

Après avoir ajouté le ScriptManager à la page, modifier son `ID` de `ScriptManager1` à `MyManager`.


[![AJJ le ScriptManager à la Page maître](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figure 01**: Ajouter le ScriptManager à la Page maître ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Étape 2 : À l’aide de l’infrastructure ASP.NET AJAX à partir d’une Page de contenu

Avec le contrôle ScriptManager ajouté à la page maître, nous pouvons maintenant ajouter des fonctionnalités ASP.NET AJAX framework à n’importe quelle page de contenu. Nous allons créer une nouvelle page ASP.NET qui affiche un produit sélectionné de façon aléatoire à partir de la base de données Northwind. Nous allons utiliser le contrôle Timer de l’infrastructure ASP.NET AJAX pour mettre à jour de cet affichage toutes les 15 secondes, affichant un nouveau produit.

Commencez par créer une nouvelle page dans le répertoire racine nommé `ShowRandomProduct.aspx`. N’oubliez pas de lier cette nouvelle page à la `Site.master` page maître.


[![Aune nouvelle Page ASP.NET pour le site Web de jj](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figure 02**: Ajouter une nouvelle Page ASP.NET pour le site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image6.png))


N’oubliez pas que dans le [ *spécifiant le titre, les balises Meta et les autres en-têtes HTML dans la Page maître* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) didacticiel, nous avons créé une classe de page de base personnalisée nommée `BasePage` qui a généré le titre de la page s’il s’agissait pas explicitement défini. Accédez à la `ShowRandomProduct.aspx` code-behind de la page de classe et de la dériver de `BasePage` (au lieu d’à partir de `System.Web.UI.Page`).

Enfin, mettez à jour le `Web.sitemap` fichier à inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour le Master leçon d’Interaction de la Page contenu :


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

L’ajout de ce `<siteMapNode>` élément est reflété dans les leçons liste (voir Figure 5).

### <a name="displaying-a-randomly-selected-product"></a>Affichage d’un produit sélectionné de façon aléatoire

Retour à `ShowRandomProduct.aspx`. Dans le concepteur, faites glisser un contrôle UpdatePanel à partir de la boîte à outils dans le `MainContent` contrôle de contenu et définir son `ID` propriété `ProductPanel`. Le contrôle UpdatePanel représente une région de l’écran qui peut être mis à jour en mode asynchrone via une publication de page partielle.

Notre première tâche consiste à afficher des informations sur un produit sélectionné de façon aléatoire dans le contrôle UpdatePanel. Nous allons faire glisser un contrôle DetailsView dans le contrôle UpdatePanel. Définir le contrôle DetailsView `ID` propriété `ProductInfo` et d’effacer les sa `Height` et `Width` propriétés. Développez la balise active de DetailsView et, dans la liste déroulante Choisir la Source de données, choisissez de lier le contrôle DetailsView à un nouveau contrôle SqlDataSource nommé `RandomProductDataSource`.


[![Bchercher le contrôle DetailsView à un nouveau contrôle SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figure 03**: Lier le contrôle DetailsView à un nouveau contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurer le contrôle SqlDataSource pour se connecter à la base de données Northwind via le `NorthwindConnectionString` (que nous avons créée dans le [ *interagir avec la Page maître depuis la Page de contenu* ](interacting-with-the-content-page-from-the-master-page-cs.md) didacticiel). Lorsque la configuration de l’instruction select choisir de spécifier une instruction SQL personnalisée, puis entrez la requête suivante :


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Le `TOP 1` mot clé dans le `SELECT` clause retourne uniquement le premier enregistrement retourné par la requête. Le [ `NEWID()` fonction](https://msdn.microsoft.com/library/ms190348.aspx) génère une nouvelle [valeur d’identificateur global unique (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) et peut être utilisé dans un `ORDER BY` clause pour retourner les enregistrements de la table dans un ordre aléatoire.


[![Configurer SqlDataSource pour retourner une seule, sélectionné aléatoirement les enregistrement](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figure 04**: Configurer SqlDataSource pour retourner un seul enregistrement du sélectionné au hasard ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image12.png))


À l’issue de l’Assistant, Visual Studio crée un BoundField pour les deux colonnes retournées par la requête ci-dessus. À ce stade balisage déclaratif de votre page doit ressembler à ce qui suit :


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

La figure 5 illustre le `ShowRandomProduct.aspx` page lorsqu’ils sont affichés via un navigateur. Cliquez sur le bouton d’actualisation de votre navigateur pour recharger la page ; Vous devez voir le `ProductName` et `UnitPrice` valeurs pour un nouvel enregistrement sélectionné de façon aléatoire.


[![A Nom et le prix du produit aléatoire est affiché](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figure 05**: Nom et le prix d’un produit aléatoire est affiché ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Un nouveau produit s’affiche automatiquement toutes les 15 secondes

L’infrastructure ASP.NET AJAX inclut un contrôle Timer qui effectue une publication (postback) à une heure spécifiée ; sur publication (postback) de la minuterie `Tick` événement est déclenché. Si le contrôle Timer est placé dans un UpdatePanel, il déclenche une publication de page partielle, au cours de laquelle nous pouvons relier les données au contrôle DetailsView afin d’afficher un nouveau produit sélectionné de façon aléatoire.

Pour ce faire, faites glisser un minuteur à partir de la boîte à outils et déposez-la dans le contrôle UpdatePanel. Modification de la minuterie `ID` à partir de `Timer1` à `ProductTimer` et son `Interval` propriété 60000 15000. Le `Interval` propriété indique le nombre de millisecondes écoulées entre publications (postback) ; la valeur 15000 provoque le minuteur déclencher une publication de page partielle toutes les 15 secondes. À ce stade balisage déclaratif de votre minuterie doit ressembler à ce qui suit :


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Créer un gestionnaire d’événements pour le minuteur `Tick` événement. Dans ce gestionnaire d’événements, nous avons besoin relier les données au contrôle DetailsView en appelant le DetailsView `DataBind` (méthode). Cela indique le contrôle DetailsView à récupérer de nouveau les données à partir de son contrôle de source de données, qui sélectionne et afficher une nouvelle façon aléatoire enregistrement sélectionné (comme lors de la recharger la page en cliquant sur le bouton Actualiser du navigateur).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

C’est aussi simple que cela ! Visitez la page via un navigateur. Initialement, les informations d’un produit aléatoire s’affiche. Si vous regardez patiemment l’écran, vous remarquerez que, après 15 secondes, plus d’informations sur un nouveau produit remplace comme par magie l’affichage existant.

Pour mieux voir ce qui se passe ici, nous allons ajouter un contrôle d’étiquette pour le contrôle UpdatePanel qui affiche l’heure de que dernière mise à jour de l’affichage. Ajoutez un contrôle Web Label dans le contrôle UpdatePanel, définissez son `ID` à `LastUpdateTime`, désactivez sa `Text` propriété. Ensuite, créez un gestionnaire d’événements pour l’UpdatePanel `Load` événement et l’affichage de l’heure actuelle dans l’étiquette. (L’UpdatePanel `Load` événement est déclenché à chaque publication de page partiel ou complet.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Avec cette modification terminée, la page comprend le temps que le produit actuellement affiché a été chargé. Figure 6 montre la page lors de la première visite. Figure 7 illustre les 15 secondes plus tard de la page une fois que le contrôle Timer a « cochée » et le contrôle UpdatePanel a été actualisé pour afficher des informations sur un nouveau produit.


[![A Au hasard le produit sélectionné s’affiche lors du chargement de Page](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figure 06**: Un produit sélectionné au hasard s’affiche lors du chargement de Page ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![E15 secondes tout qu'un nouveau aléatoirement sélectionné produit s’affiche.](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figure 07**: Toutes les 15 secondes une nouvelle façon aléatoire sélectionné produit s’affiche ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Étape 3 : Utilisation du contrôle ScriptManagerProxy

Ainsi que d’y compris le script nécessaire pour l’infrastructure ASP.NET AJAX bibliothèque cliente, ScriptManager peut également inscrire des fichiers JavaScript personnalisés, références aux Services Web compatibles avec script et l’authentification personnalisée, l’autorisation et les services de profil. Généralement, ces personnalisations sont spécifiques à une page donnée. Toutefois, si les fichiers, les références de Service Web ou l’authentification de script personnalisé, d’autorisation ou de services de profil sont référencées dans le ScriptManager dans la page maître puis ils sont inclus dans *tous les* pages dans le site Web.

Pour ajouter les personnalisations de ScriptManager liées sur une base de page par page utilisent le contrôle ScriptManagerProxy. Vous pouvez ajouter un contrôle ScriptManagerProxy à une page de contenu et puis enregistrez le fichier JavaScript personnalisé, référence de Service Web, ou d’authentification, d’autorisation ou de service de profil à partir de la ScriptManagerProxy ; Cela a pour effet d’inscription de ces services pour la page de contenu particulier.

> [!NOTE]
> Une page ASP.NET peut uniquement posséder pas plus d’un contrôle ScriptManager présents. Par conséquent, vous ne peut pas ajouter un contrôle ScriptManager à une page de contenu si le contrôle ScriptManager est déjà défini dans la page maître. Le seul but du ScriptManagerProxy consiste à fournir un moyen aux développeurs de définir le ScriptManager dans la page maître, mais ont toujours la possibilité d’ajouter des personnalisations de ScriptManager sur une base de page par page.


Pour afficher le contrôle ScriptManagerProxy en action, nous allons augmenter le contrôle UpdatePanel dans `ShowRandomProduct.aspx` d’inclure un bouton qui utilise un script côté client pour suspendre ou reprendre le contrôle Timer. Le contrôle Timer a trois méthodes côté client que nous pouvons utiliser pour atteindre cet objectif souhaité :

- `_startTimer()` -démarre le contrôle Timer
- `_raiseTick()` -, ainsi le contrôle Timer « cycle », publication et déclenchement son `Tick` événement sur le serveur
- `_stopTimer()` -arrête le contrôle Timer

Nous allons créer un fichier JavaScript avec une variable nommée `timerEnabled` et une fonction nommée `ToggleTimer`. Le `timerEnabled` variable indique si le contrôle Timer est actuellement activé ou désactivé ; la valeur par défaut est true. Le `ToggleTimer` fonction accepte deux paramètres d’entrée : une référence pour la côté client et le bouton de Pause/reprise `id` valeur du contrôle Timer. Cette fonction active ou désactive la valeur de `timerEnabled`, obtient une référence au contrôle Timer, démarre ou arrête le minuteur (selon la valeur de `timerEnabled`) et met à jour le texte d’affichage du bouton « Pause » ou « Resume ». Cette fonction est appelée chaque fois que l’utilisateur clique sur le bouton Pause/reprise.

Commencez par créer un nouveau dossier dans le site Web nommé `Scripts`. Ensuite, ajoutez un nouveau fichier au dossier Scripts nommé `TimerScript.js` de type fichier JScript.


[![Ajj un nouveau fichier JavaScript dans le dossier Scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figure 08**: Ajouter un nouveau fichier JavaScript à la `Scripts` dossier ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![A Nouveau fichier JavaScript a été ajoutée au site Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figure 09**: Un nouveau fichier JavaScript a été ajouté au site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Ensuite, ajoutez le script suivant au fichier TimerScript.js :


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Nous devons maintenant enregistrer ce fichier JavaScript personnalisé dans `ShowRandomProduct.aspx`. Retour à `ShowRandomProduct.aspx` et ajoutez un contrôle ScriptManagerProxy à la page ; définir son `ID` à `MyManagerProxy`. Pour inscrire un code JavaScript personnalisé fichier sélectionner le contrôle ScriptManagerProxy dans le concepteur et passez à la fenêtre Propriétés. Une des propriétés est intitulée Scripts. Si cette propriété est sélectionnée, l’éditeur de collections ScriptReference illustré Figure 10. Cliquez sur le bouton Ajouter pour incluent une nouvelle référence de script, puis entrez le chemin d’accès au fichier de script dans la propriété de chemin d’accès : `~/Scripts/TimerScript.js`.


[![Ajj une référence de Script pour le contrôle ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figure 10**: Ajouter une référence de Script pour le contrôle ScriptManagerProxy ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Après l’ajout de la référence de script le contrôle ScriptManagerProxy's déclarative balisage est mis à jour pour inclure un `<Scripts>` collection avec un seul `ScriptReference` entrée, comme l’extrait de balisage suivant illustre :


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

Le `ScriptReference` entrée indique le ScriptManagerProxy à inclure une référence au fichier JavaScript dans son balisage rendu. Autrement dit, en vous inscrivant personnalisé de script dans le contrôle ScriptManagerProxy le `ShowRandomProduct.aspx` sortie de rendu de la page inclut maintenant un autre `<script src="url"></script>` balise : `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Nous pouvons maintenant appeler le `ToggleTimer` fonction définie dans `TimerScript.js` à partir du script client dans le `ShowRandomProduct.aspx` page. Ajoutez le code HTML suivant dans le contrôle UpdatePanel :


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Cela affiche un bouton avec le texte « Pause ». Chaque fois que vous cliquez dessus, la fonction JavaScript `ToggleTimer` est appelé, en passant une référence pour le bouton et la valeur d’id du contrôle Timer (`ProductTimer`). Notez la syntaxe pour obtenir le `id` valeur du contrôle Timer. `<%=ProductTimer.ClientID%>` émet la valeur de la `ProductTimer` du contrôle Timer `ClientID` propriété. Dans le [ *d’affectation de noms ID de contrôle dans les Pages de contenu* ](control-id-naming-in-content-pages-cs.md) didacticiel, nous avons abordé les différences entre le côté serveur `ID` valeur et la côté client qui en résulte `id` valeur et comment `ClientID` retourne côté client `id`.

Figure 11 illustre cette page lors de la première visite via un navigateur. La minuterie est en cours d’exécution et met à jour les informations de produit affiché toutes les 15 secondes. Figure 12 illustre l’écran une fois que le bouton Pause a été cliqué. En cliquant sur le bouton Pause arrête le minuteur et met à jour le texte du bouton pour « Reprendre ». Les informations de produit seront Actualiser (et continuer à actualiser toutes les 15 secondes) une fois que l’utilisateur clique sur Reprendre.


[![CCliquez sur le bouton Pause pour arrêter le contrôle Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figure 11**: Cliquez sur le bouton Pause pour arrêter le contrôle Timer ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![CCliquez sur le bouton de reprise pour redémarrer la minuterie](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figure 12**: Cliquez sur le bouton de reprise pour redémarrer la minuterie ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Récapitulatif

Lorsque vous créez des applications web compatibles AJAX à l’aide de l’infrastructure ASP.NET AJAX, il est impératif que chaque page web compatible AJAX incluent un contrôle ScriptManager. Pour faciliter ce processus, nous pouvons ajouter un ScriptManager à la page maître, plutôt que d’avoir à mémoriser ajouter un ScriptManager à chaque page de contenu. Étape 1 a montré comment ajouter le ScriptManager à la page maître pendant l’étape 2 étudié l’implémentation des fonctionnalités AJAX dans une page de contenu.

Si vous devez ajouter des scripts personnalisés, des références aux Services Web compatibles avec script, ou d’authentification personnalisés, l’autorisation ou des services de profil à une page de contenu particulier, ajoutez un contrôle ScriptManagerProxy à la page contenue puis configurez le il les personnalisations. Étape 3 examiné comment le contrôle ScriptManagerProxy permet d’enregistrer un fichier JavaScript personnalisé dans une page de contenu spécifique.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Infrastructure ASP.NET AJAX](../../../../ajax/index.md)
- [ASP.NET AJAX didacticiels](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vidéos ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Interface utilisateur Interactive de construction avec Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilisation de NEWID pour trier les enregistrements de façon aléatoire](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Utilisation du contrôle Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Suivant](specifying-the-master-page-programmatically-cs.md)
