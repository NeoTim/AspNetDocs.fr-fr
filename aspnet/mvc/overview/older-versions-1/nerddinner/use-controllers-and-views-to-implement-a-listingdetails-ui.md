---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Utilisez les contrôleurs et les vues pour mettre en œuvre une interface utilisateur d’inscription/détails (fr) Microsoft Docs
author: rick-anderson
description: L’étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une liste de données/expérience de navigation des détails...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 49c7dc977477a4edbfcfc68b166ae7ea3fa22f2a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541310"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 4 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une liste de données/expérience de navigation des détails pour les dîners sur notre site NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner Step 4: Contrôleurs et vues

Avec les cadres Web traditionnels (ASP classique, PHP, ASP.NET Web Forms, etc.), les URL entrantes sont généralement cartographiées sur les fichiers sur disque. Par exemple : une demande d’URL comme «/Products.aspx » ou « Products.php » peut être traitée par un fichier « Products.aspx » ou « Products.php ».

Les cadres MVC basés sur le Web cartographient les URL au code serveur d’une manière légèrement différente. Au lieu de cartographier les URL entrantes vers les fichiers, elles cartographient plutôt les URL selon des méthodes sur les classes. Ces classes sont appelées «Contrôleurs» et elles sont responsables du traitement des demandes HTTP entrantes, du traitement des entrées des utilisateurs, de la récupération et de l’enregistrement des données, et de la détermination de la réponse à renvoyer au client (afficher HTML, télécharger un fichier, rediriger vers une URL différente, etc.).

Maintenant que nous avons construit un modèle de base pour notre application NerdDinner, notre prochaine étape sera d’ajouter un contrôleur à l’application qui en profite pour fournir aux utilisateurs une liste de données / expérience de navigation des détails pour les dîners sur notre site.

### <a name="adding-a-dinnerscontroller-controller"></a>Ajout d’un contrôleur DinnersController

Nous commencerons par cliquer à droite sur le dossier "Controllers" dans notre projet web, puis sélectionner la commande de menu **Add-Controller&gt;** (vous pouvez également exécuter cette commande en tapant Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Cela fera ressortir le dialogue "Add Controller":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Nous nommerons le nouveau contrôleur "DinnersController" et cliquez sur le bouton "Ajouter". Visual Studio ajoutera ensuite un fichier DinnersController.cs sous notre répertoire de «Contrôleurs» :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Il ouvrira également la nouvelle classe DinnersController au sein de l’éditeur de code.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Ajout d’index() et de détails() Méthodes d’action à la classe DinnersController

Nous voulons permettre aux visiteurs utilisant notre application de parcourir une liste des dîners à venir, et leur permettre de cliquer sur n’importe quel dîner dans la liste pour voir des détails spécifiques à ce sujet. Pour ce faire, nous publierons les URL suivantes de notre application :

| **URL** | **Objectif** |
| --- | --- |
| */Dîners/* | Afficher une liste HTML des dîners à venir |
| */Dîners/Détails/[id]* | Afficher les détails d’un dîner spécifique indiqué par un paramètre "id" intégré dans l’URL, qui correspondra au DînerID du dîner dans la base de données. Par exemple : /Dîners/Détails/2 afficherait une page HTML avec des détails sur le dîner dont la valeur DinnerID est de 2. |

Nous publierons les premières implémentations de ces URL en ajoutant deux « méthodes d’action » publiques à notre classe DinnersController comme ci-dessous :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Nous allons ensuite exécuter l’application NerdDinner et utiliser notre navigateur pour les invoquer. La saisie de l’URL *"/Dîners/"* fera fonctionner notre méthode *Index()* et elle renvoiera la réponse suivante :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

La saisie de l’URL *"/Dîners/Détails/2"* fera fonctionner notre méthode *Détails()* et renvoiera la réponse suivante :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Vous vous demandez peut-être - comment ASP.NET MVC savait-il créer notre classe DinnersController et invoquer ces méthodes? Pour comprendre que nous allons jeter un coup d’oeil rapide à la façon dont le routage fonctionne.

### <a name="understanding-aspnet-mvc-routing"></a>Comprendre ASP.NET MVC Routing

ASP.NET MVC comprend un puissant moteur de routage d’URL qui offre beaucoup de flexibilité dans le contrôle de la façon dont les URL sont cartographiées pour les classes de contrôleurs. Il nous permet de personnaliser complètement la façon dont ASP.NET MVC choisit la classe de contrôleur à créer, quelle méthode invoquer sur elle, ainsi que configurer différentes façons que les variables peuvent être automatiquement analysées à partir de l’URL / requête et passé à la méthode comme des arguments de paramètres. Il offre la flexibilité d’optimiser totalement un site pour seO (optimisation des moteurs de recherche) ainsi que de publier n’importe quelle structure URL que nous voulons à partir d’une application.

Par défaut, les nouveaux projets ASP.NET MVC sont livrés avec un ensemble préconfiguré de règles de routage URL déjà enregistrées. Cela nous permet de démarrer facilement sur une application sans avoir à configurer explicitement quoi que ce soit. Les enregistrements des règles d’itinéraire par défaut peuvent être trouvés dans la catégorie «Application» de nos projets - que nous pouvons ouvrir en cliquant à deux fois le fichier «Global.asax» dans la racine de notre projet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Les règles de routage MVC par défaut ASP.NET sont enregistrées dans le cadre de la méthode «RegisterRoutes» de cette classe :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Les "routes. MapRoute ()" méthode d’appel ci-dessus enregistre une règle de routage par défaut qui cartographie les URL entrantes aux classes de contrôleur en utilisant le format URL: "/contrôleur/ action/ 'id'" - où "contrôleur" est le nom de la classe de contrôleur à instanter, "action" est le nom d’une méthode publique à invoquer sur elle, et "id" est un paramètre facultatif intégré dans l’URL qui peut être passé comme un argument à la méthode. Le troisième paramètre transmis à l’appel de méthode "MapRoute()" est un ensemble de valeurs par défaut à utiliser pour le contrôleur/action/id valeurs dans le cas où ils ne sont pas présents dans l’URL (Contrôleur 'Home'"Index", Id'").

Voici un tableau qui montre comment une variété d’URL sont cartographiées à l’aide de la règle d’itinéraire par défaut «<em>/contrôleurs » / «action» / id »</em>:

| **URL** | **Classe de contrôleur** | **Méthode d'action** | **Paramètres passés** |
| --- | --- | --- | --- |
| */Dîners/Détails/2* | DînersController | Détails(id) | id 2 |
| */Dîners/Edit/5* | DînersController | Modifier(id) | id 5 |
| */Dîners/Créer* | DînersController | Create() | N/A |
| */Dîners* | DînersController | Indice() | N/A |
| */accueil* | AccueilController | Indice() | N/A |
| */* | AccueilController | Indice() | N/A |

Les trois dernières lignes montrent les valeurs par défaut (Contrôleur - Accueil, Action et Index, Id et "). Étant donné que la méthode « Index » est enregistrée comme nom d’action par défaut si l’on n’est pas spécifié, les URL «/Dîners » et « Accueil » font invoquer la méthode d’action Index() sur leurs classes de contrôleur. Étant donné que le contrôleur "Home" est enregistré comme contrôleur par défaut si l’on n’est pas spécifié, l’URL "/" provoque la création du HomeController, et la méthode d’action Index() sur elle à être invoquée.

Si vous n’aimez pas ces règles de routage par URL par défaut, la bonne nouvelle est qu’elles sont faciles à modifier - il suffit de les modifier dans la méthode RegisterRoutes ci-dessus. Pour notre application NerdDinner, cependant, nous n’allons pas changer l’une des règles de routage URL par défaut - au lieu de cela, nous allons simplement les utiliser tel quel.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Utilisation du DinnerRepository de nos DînersController

Remplaçons maintenant notre mise en œuvre actuelle des méthodes d’action De l’Indice des dînerscontroller() et détails() par des implémentations qui utilisent notre modèle.

Nous utiliserons la classe DinnerRepository que nous avons construite plus tôt pour mettre en œuvre le comportement. Nous allons commencer par ajouter une déclaration "en utilisant" qui fait référence à l’espace de nom "NerdDinner.Models", puis déclarer un exemple de notre DinnerRepository comme un champ sur notre classe DinnerController.

Plus tard dans ce chapitre, nous allons introduire le concept de "Dependency Injection" et montrer une autre façon pour nos contrôleurs d’obtenir une référence à un DinnerRepository qui permet de meilleurs tests unitaires - mais pour l’instant nous allons simplement créer un exemple de notre ligne DinnerRepository comme ci-dessous.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Maintenant, nous sommes prêts à générer une réponse HTML en arrière à l’aide de nos objets de modèle de données récupérés.

### <a name="using-views-with-our-controller"></a>Utilisation de vues avec notre contrôleur

Bien qu’il soit possible d’écrire du code dans nos méthodes d’action pour assembler HTML et ensuite utiliser la méthode *d’aide Response.Write()* pour le renvoyer au client, cette approche devient assez lourde rapidement. Une bien meilleure approche est pour nous d’effectuer uniquement l’application et la logique des données à l’intérieur de nos méthodes d’action DinnersController, puis de passer les données nécessaires pour rendre une réponse HTML à un modèle distinct de «vue» qui est responsable de la sortie de la représentation HTML de celui-ci. Comme nous le verrons dans un instant, un modèle de « vue » est un fichier texte qui contient généralement une combinaison de balisage HTML et de code de rendu intégré.

Séparer notre logique de contrôleur de notre rendu de vue apporte plusieurs grands avantages. En particulier, il permet d’appliquer une « séparation claire des préoccupations » entre le code d’application et le code de formatage/rendu de l’interface utilisateur. Cela rend beaucoup plus facile de tester la logique d’application unitaire indépendamment de la logique de rendu de l’interface utilisateur. Il est plus facile de modifier plus tard les modèles de rendu de l’interface utilisateur sans avoir à apporter des modifications au code d’application. Et il peut rendre plus facile pour les développeurs et les concepteurs de collaborer ensemble sur des projets.

Nous pouvons mettre à jour notre classe DinnersController pour indiquer que nous voulons utiliser un modèle de vue pour renvoyer une réponse HTML UI en changeant les signatures de méthode de nos deux méthodes d’action d’avoir un type de retour de "vide" à la place d’avoir un type de retour de "ActionResult". Nous pouvons ensuite appeler la méthode *d’aide View()* sur la classe de base Controller pour revenir à un objet "ViewResult" comme ci-dessous:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La signature de la méthode *d’aide View()* que nous utilisons ci-dessus ressemble ci-dessous:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Le premier paramètre de la méthode *d’assistance View ()* est le nom du fichier de modèle de vue que nous voulons utiliser pour rendre la réponse HTML. Le deuxième paramètre est un objet modèle qui contient les données dont le modèle d’entrée a besoin pour rendre la réponse HTML.

Dans notre méthode d’action Index() que nous appelons la méthode *d’aide View()* et indiquant que nous voulons rendre une liste HTML des dîners à l’aide d’un modèle de vue « Index ». Nous passons le modèle de vue d’une séquence d’objets De dîner pour générer la liste à partir de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dans notre méthode d’action Détails() nous essayons de récupérer un objet Dîner en utilisant l’id fourni dans l’URL. Si un dîner valide est trouvé, nous appelons la *méthode d’aide View(),* indiquant que nous voulons utiliser un modèle de vue "Détails" pour rendre l’objet de dîner récupéré. Si un dîner invalide est demandé, nous rendons un message d’erreur utile qui indique que le dîner n’existe pas en utilisant un modèle de vue "NotFound" (et une version surchargée de la *vue ()* méthode d’assistance qui prend juste le nom du modèle):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Mettons maintenant en œuvre les modèles de vue "NotFound", "Details" et "Index".

### <a name="implementing-the-notfound-view-template"></a>Mise en œuvre du modèle de vue « nonfound »

Nous commencerons par mettre en œuvre le modèle de vue " NotFound ", qui affiche un message d’erreur convivial indiquant que le dîner demandé ne peut pas être trouvé.

Nous allons créer un nouveau modèle de vue en plaçant notre curseur de texte dans une méthode d’action de contrôleur, puis cliquez à droite et choisissez la commande de menu "Add View" (nous pouvons également exécuter cette commande en tapant Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Cela fera ressortir un dialogue "Add View" comme ci-dessous. Par défaut, le dialogue pré-peuplera le nom de la vue à créer pour correspondre au nom de la méthode d’action dans laquelle le curseur était lorsque le dialogue a été lancé (dans ce cas "Détails"). Parce que nous voulons d’abord implémenter le modèle "NotFound", nous allons passer outre à ce nom de vue et le définir à la place d’être "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio créera un nouveau modèle de vue "NotFound.aspx" pour nous dans le répertoire "Views-Dîners" (qu’il créera également si l’annuaire n’existe pas déjà):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Il ouvrira également notre nouveau modèle de vue "NotFound.aspx" dans l’éditeur de code:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Les modèles de vue par défaut ont deux « régions de contenu » où nous pouvons ajouter du contenu et du code. La première nous permet de personnaliser le «titre» de la page HTML renvoyée. La seconde nous permet de personnaliser le « contenu principal » de la page HTML renvoyée.

Pour implémenter notre modèle de vue « NotFound », nous ajouterons du contenu de base :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Nous pouvons ensuite l’essayer dans le navigateur. Pour ce faire, nous allons demander l’URL *"/Dîners/Détails/9999".* Il s’agira d’un dîner qui n’existe pas actuellement dans la base de données, et provoquera notre méthode d’action DinnersController.Details() pour rendre notre modèle de vue " NotFound " :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Une chose que vous remarquerez dans la capture d’écran ci-dessus, c’est que notre modèle de vue de base a hérité d’un tas de HTML qui entoure le contenu principal sur l’écran. C’est parce que notre modèle de vue utilise un modèle de « page maîtresse » qui nous permet d’appliquer une mise en page cohérente sur toutes les vues sur le site. Nous discuterons de la façon dont les pages maîtresses fonctionnent plus dans une partie ultérieure de ce tutoriel.

### <a name="implementing-the-details-view-template"></a>Mise en œuvre du modèle de vue « Détails »

Implémentons maintenant le modèle de vue « Détails » qui générera du HTML pour un seul modèle de dîner.

Nous allons le faire en plaçant notre curseur de texte dans la méthode d’action Détails, puis cliquez à droite et choisissez la commande de menu "Add View" (ou appuyez sur Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Cela fera ressortir le dialogue "Add View". Nous conserverons le nom de vue par défaut ("Détails"). Nous sélectionnerons également la case à cocher " Créer une vue fortement typée " dans le dialogue et sélectionner (à l’aide du dropdown de la combobox) le nom du type de modèle que nous passons du contrôleur à la vue. Pour cette vue, nous passons un objet de dîner (le nom entièrement qualifié pour ce type est: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Contrairement au modèle précédent, où nous avons choisi de créer une " vue vide ", cette fois nous choisirons d’envoyer automatiquement "échafaudage" la vue à l’aide d’un modèle "Détails". Nous pouvons l’indiquer en modifiant le «contenu de vue» drop-down dans le dialogue ci-dessus.

"Échafaudage" générera une mise en œuvre initiale de notre modèle de vue des détails basé sur l’objet Dîner que nous y transmettons. Cela nous permet de commencer rapidement sur notre mise en œuvre du modèle de vue.

Lorsque nous cliquons le bouton "Ajouter", Visual Studio créera pour nous un nouveau fichier de modèle de vue "Details.aspx" dans notre répertoire "Views-Dîners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Il ouvrira également notre nouveau modèle de vue "Details.aspx" dans l’éditeur de code. Il contiendra une mise en œuvre initiale d’un échafaudage d’une vue de détails basée sur un modèle de dîner. Le moteur d’échafaudage utilise la réflexion .NET pour examiner les propriétés publiques exposées sur la classe l’a passé, et ajoutera le contenu approprié basé sur chaque type qu’il trouve:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Nous pouvons demander l’URL *"/Dîners/Détails/1"* pour voir à quoi ressemble cette implémentation d’échafaudages "détails" dans le navigateur. L’utilisation de cette URL affichera l’un des dîners que nous avons ajoutés manuellement à notre base de données lorsque nous l’avons créée pour la première fois :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Cela nous met en marche rapidement, et nous fournit une mise en œuvre initiale de notre vue Details.aspx. Nous pouvons ensuite aller le modifier pour personnaliser l’interface utilisateur à notre satisfaction.

Lorsque nous regardons le modèle Details.aspx de plus près, nous constaterons qu’il contient html statique ainsi que le code de rendu intégré. &lt;%&gt; de pépites de code d’exécution &lt;de code&gt; lorsque le modèle de vue rend, et les pépites de code de % à % exécutent le code contenu en elles, puis rendent le résultat au flux de sortie du modèle.

Nous pouvons écrire du code dans notre vue qui accède à l’objet modèle "Dîner" qui a été passé de notre contrôleur à l’aide d’une propriété "Modèle" fortement typée. Visual Studio nous fournit un code-intellisense complet lors de l’accès à cette propriété "Modèle" au sein de l’éditeur:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Faisons quelques réglages de sorte que la source de notre modèle de vue détails final ressemble ci-dessous:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Lorsque nous accédons à nouveau à l’URL *"/Dîners/Détails/1",* elle rendra désormais comme ci-dessous :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Mise en œuvre du modèle de vue « Index »

Mettons maintenant en œuvre le modèle de vue « Index » qui générera une liste des dîners à venir. Pour ce faire, nous positionnerons notre curseur de texte dans la méthode d’action Index, puis cliquez à droite et choisissez la commande de menu "Add View" (ou appuyez sur Ctrl-M, Ctrl-V).

Dans le dialogue "Add View", nous conserverons le modèle de vue nommé "Index" et sélectionnerons la case à cocher "Créer une vue fortement typée". Cette fois, nous choisirons de générer automatiquement un modèle de vue "List", et de sélectionner "NerdDinner.Models.Dinner" que le type de modèle passé à la vue (qui parce que nous avons indiqué que nous créons un "List" échafaudage fera le dialogue Add View de supposer que nous passons une séquence d’objets Dîner de notre contrôleur à la vue):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio créera pour nous un nouveau fichier de modèle de vue "Index.aspx" dans notre répertoire "Views-Dîners". Il va "échafaudage" une mise en œuvre initiale en elle qui fournit une liste de table HTML des dîners que nous passons à la vue.

Lorsque nous exécuteons l’application et accédons à l’URL *"/Dîners/"* elle rendra notre liste de dîners comme tel:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solution de table ci-dessus nous donne une mise en page de grille-comme de nos données de dîner - qui n’est pas tout à fait ce que nous voulons pour notre consommateur face à la liste dîner. Nous pouvons mettre à jour le modèle de vue Index.aspx &lt;et&gt; le modifier pour énumérer moins de colonnes de données, et utiliser un élément ul pour les rendre au lieu d’un tableau en utilisant le code ci-dessous:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Nous utilisons le mot clé "var" dans la déclaration ci-dessus avant que nous boucle sur chaque dîner dans notre modèle. Ceux qui ne connaissent pas le C 3.0 pourraient penser que l’utilisation de «var» signifie que l’objet du dîner est en retard. Cela signifie plutôt que le compilateur utilise l’inférence de type contre la propriété fortement tapée "Modèle" (qui est de type "IEnumerable&lt;Dinner&gt;") et la compilation de la variable locale "dîner" comme un type de dîner - ce qui signifie que nous obtenons intellisense plein et compiler-temps de contrôle pour elle dans les blocs de code:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Lorsque nous avons frappé rafraîchir sur *l’URL /Dîners* dans notre navigateur notre vue mise à jour ressemble maintenant ci-dessous:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Cela est à la recherche mieux - mais n’est pas encore tout à fait là. Notre dernière étape est de permettre aux utilisateurs finaux de cliquer sur les dîners individuels dans la liste et de voir les détails à leur sujet. Nous metndons en œuvre cela en rendant les éléments hyperliens HTML qui lient à la méthode d’action Détails sur notre DinnersController.

Nous pouvons générer ces hyperliens dans notre vue Index de deux façons. La première consiste à &lt;créer&gt; manuellement HTML un &lt;élément&gt; comme ci-dessous, où nous embed % % blocs dans l’élément &lt;HTML:&gt;

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Une autre approche que nous pouvons utiliser est de tirer parti de la méthode intégrée "Html.ActionLink()" aide &lt;dans&gt; ASP.NET MVC qui prend en charge la création programmatique d’un HTML un élément qui relie à une autre méthode d’action sur un contrôleur:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Le premier paramètre de la méthode d’aide Html.ActionLink () est le lien-texte à afficher (dans ce cas le titre du dîner), le deuxième paramètre est le nom d’action Controller que nous voulons générer le lien vers (dans ce cas la méthode Détails), et le troisième paramètre est un ensemble de paramètres à envoyer à l’action (mis en œuvre comme un type anonyme avec le nom de propriété / valeurs). Dans ce cas, nous spécifions le paramètre "id" du dîner vers lequel nous voulons lier, et parce que la règle de routage par défaut de l’URL dans ASP.NET MVC est la méthode d’aide Html.ActionLink() de «Contrôleur/ Action» génère la sortie suivante :

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Pour notre vue Index.aspx, nous allons utiliser l’approche de méthode d’aide Html.ActionLink() et avoir chaque dîner dans le lien de liste vers l’URL de détails appropriées:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Et maintenant, quand nous avons frappé *l’URL /Dîners* notre liste de dîner ressemble ci-dessous:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Lorsque nous cliquons l’un des dîners dans la liste, nous naviguerons pour voir les détails à ce sujet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nommage basé sur les conventions et structure d’annuaire « Vues »

ASP.NET les applications MVC par défaut utilisent une structure de nommage d’annuaire basée sur la convention lors de la résolution des modèles de vue. Cela permet aux développeurs d’éviter d’avoir à se qualifier pleinement d’un chemin de localisation lors du référencement des vues de l’intérieur d’une classe Controller. Par défaut ASP.NET MVC cherchera le fichier de modèle\[de vue\* dans l’annuaire de l’application .

Par exemple, nous avons travaillé sur la classe DinnersController, qui fait explicitement référence à trois modèles de vue : « Index », « Détails » et « NotFound ». ASP.NET MVC cherchera par défaut ces points de vue dans *l’annuaire « Vues »Dîners* sous notre répertoire racine d’application :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Remarquez ci-dessus comment il y a actuellement trois classes de contrôleur dans le projet (DinnersController, HomeController et AccountController - les deux dernières ont été ajoutées par défaut lorsque nous avons créé le projet), et il y a trois sous-répertoires (un pour chaque contrôleur) dans le répertoire « Vues ».

Les points de vue référencés par les contrôleurs De la maison et des comptes résoudront automatiquement leurs modèles de vue à partir des répertoires respectifs *« Vues »Accueil* et *« Vues ».* Le sous-répertoire *« Vues partagées* » permet de stocker des modèles de vue qui sont réutiliser plusieurs contrôleurs au sein de l’application. Lorsque ASP.NET MVC tente de résoudre un modèle de vue, il vérifiera d’abord dans *l’annuaire spécifique du «Contrôleur de vues»,\[* et s’il ne peut pas y trouver le modèle de vue, il examinera dans *l’annuaire « Vues partagées* ».

Lorsqu’il s’agit de nommer des modèles de vue individuels, les directives recommandées sont d’avoir le modèle de vue partager le même nom que la méthode d’action qui l’a fait rendre. Par exemple, au-dessus de notre méthode d’action « Index » utilise la vue « Index » pour rendre le résultat de vue, et la méthode d’action « Détails » utilise la vue « Détails » pour rendre ses résultats. Il est donc facile de voir rapidement quel modèle est associé à chaque action.

Les développeurs n’ont pas besoin de spécifier explicitement le nom du modèle de vue lorsque le modèle de vue a le même nom que la méthode d’action invoquée sur le contrôleur. Nous pouvons au lieu de cela simplement passer l’objet modèle à la méthode d’aide "View()" (sans spécifier le nom de vue), et ASP.NET MVC inférera automatiquement que nous voulons utiliser le modèle de vue *ActionName\["Views ControllerName"\[* sur le disque pour le rendre.

Cela nous permet de nettoyer un peu notre code de contrôleur, et d’éviter de dupliquer le nom deux fois dans notre code:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Le code ci-dessus est tout ce qui est nécessaire pour implémenter une belle liste dîner / expérience des détails pour le site.

#### <a name="next-step"></a>étape suivante

Nous avons maintenant une belle expérience de navigation dîner construit.

Permettons maintenant le support d’édition de formulaires CRUD (Créer, lire, mettre à jour, supprimer).

> [!div class="step-by-step"]
> [Suivant précédent](build-a-model-with-business-rule-validations.md)
> [Next](provide-crud-create-read-update-delete-data-form-entry-support.md)
