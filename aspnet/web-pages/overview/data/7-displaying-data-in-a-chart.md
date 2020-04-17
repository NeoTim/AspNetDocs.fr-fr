---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Affichage des données dans un graphique avec ASP.NET pages Web (Razor) Microsoft Docs
author: rick-anderson
description: Ce chapitre explique comment afficher les données dans un graphique. Dans les chapitres précédents, vous avez appris à afficher les données manuellement et dans une grille. Ce chapitre explique...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: ad55d4898ee39e0239ef8b0bd5eea6387af3c816
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542883"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Affichage des données dans un graphique avec ASP.NET pages Web (Razor)

par [Microsoft](https://github.com/microsoft)

> Cet article explique comment utiliser un graphique pour afficher des données dans un `Chart` site Web ASP.NET Pages Web (Razor) en utilisant l’aide.
> 
> **Ce que vous apprendrez**:
> 
> - Comment afficher les données dans un graphique.
> - Comment style graphiques en utilisant des thèmes intégrés.
> - Comment enregistrer les graphiques et comment les mettre en cache pour de meilleures performances.
> 
> Voici les ASP.NET fonctionnalités de programmation introduites dans l’article :
> 
> - L’aide. `Chart`
> 
> > [!NOTE]
> > Les informations contenues dans cet article s’appliquent aux pages Web ASP.NET 1.0 et Web Pages 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>L’aide graphique

Lorsque vous souhaitez afficher vos données sous forme `Chart` graphique, vous pouvez utiliser l’aide. L’aide `Chart` peut rendre une image qui affiche des données dans une variété de types de graphiques. Il prend en charge de nombreuses options pour le formatage et l’étiquetage. L’aide `Chart` peut rendre plus de 30 types de graphiques, y compris tous les types de graphiques que vous pourriez être familier avec de Microsoft Excel ou d’autres outils &#8212; graphiques de zone, graphiques à barres, graphiques de colonnes, graphiques en ligne, et graphiques à secteurs, ainsi que des graphiques plus spécialisés comme les graphiques boursiers.

| Description **du graphique** ![de zone : Image du type de graphique de secteur](7-displaying-data-in-a-chart/_static/image1.jpg) | **Description du graphique** ![à barres : Image du type de graphique de barre](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Description du graphique** ![de colonne : Image du type de graphique de colonne](7-displaying-data-in-a-chart/_static/image3.jpg) | **Description du graphique** ![de ligne : Image du type de graphique de ligne](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Description du graphique** ![à secteurs: Image du type de graphique à tarte](7-displaying-data-in-a-chart/_static/image5.jpg) | **Description du graphique** ![boursier: Image du type stock chart](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Éléments de graphique

Les graphiques montrent des données et des éléments supplémentaires comme des légendes, des haches, des séries, etc. L’image suivante montre de nombreux éléments graphiques que `Chart` vous pouvez personnaliser lorsque vous utilisez l’aide. Cet article vous montre comment définir certains (pas tous) de ces éléments.

![Description: Photo montrant les éléments du graphique](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Création d’un graphique à partir de données

Les données que vous affichez dans un graphique peuvent provener d’un tableau, des résultats retournés à partir d’une base de données ou des données qui se trouvent dans un fichier XML.

### <a name="using-an-array"></a>Utilisation d’un tableau

Comme expliqué dans [Introduction à ASP.NET Web Pages Programmation En utilisant la Syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), un tableau vous permet de stocker une collection d’articles similaires dans une seule variable. Vous pouvez utiliser des tableaux pour contenir les données que vous souhaitez inclure dans votre graphique.

Cette procédure montre comment vous pouvez créer un graphique à partir de données dans les tableaux, en utilisant le type de graphique par défaut. Il montre également comment afficher le graphique dans la page.

1. Créer un nouveau fichier nommé *ChartArrayBasic.cshtml*.
2. Remplacer le contenu existant par les éléments suivants : 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Le code crée d’abord un nouveau graphique et définit sa largeur et sa hauteur. Vous spécifiez `AddTitle` le titre du graphique en utilisant la méthode. Pour ajouter des données, vous utilisez la `AddSeries` méthode. Dans cet exemple, `name`vous `xValue`utilisez `yValues` le , `AddSeries` , et les paramètres de la méthode. Le `name` paramètre est affiché dans la légende du graphique. Le `xValue` paramètre contient un tableau de données qui s’affiche le long de l’axe horizontal du graphique. Le `yValues` paramètre contient un tableau de données qui est utilisé pour tracer les points verticaux du graphique.

    La `Write` méthode rend en fait le graphique. Dans ce cas, parce que vous n’avez pas spécifier un type de graphique, l’aide `Chart` rend son graphique par défaut, qui est un graphique de colonne.
3. Exécutez la page dans le navigateur. Le navigateur affiche le graphique. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilisation d’une requête de base de données pour les données graphiques

Si les informations que vous souhaitez cartographier sont dans une base de données, vous pouvez exécuter une requête de base de données, puis utiliser les données des résultats pour créer le graphique. Cette procédure vous montre comment lire et afficher les données de la base de données créée dans [l’article Introduction à travailler avec une base de données dans ASP.NET sites de pages Web](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Ajoutez un dossier *App\_Data* à la racine du site Web si le dossier n’existe pas déjà.
2. Dans le dossier *App\_Data,* ajoutez le fichier de base de données nommé *SmallBakery.sdf* qui est décrit dans [Introduction to Working with a Database in ASP.NET Sites Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Créer un nouveau fichier nommé *ChartDataQuery.cshtml*.
4. Remplacer le contenu existant par les éléments suivants :   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Le code ouvre d’abord la base de données `db`SmallBakery et l’attribue à une variable nommée . Cette variable `Database` représente un objet qui peut être utilisé pour lire et écrire à la base de données. Ensuite, le code exécute une requête SQL pour obtenir le nom et le prix de chaque produit. Le code crée un nouveau graphique et passe la requête `DataBindTable` de base de données en appelant la méthode du graphique. Cette méthode comporte deux `dataSource` paramètres : le paramètre est pour `xField` les données de la requête, et le paramètre vous permet de définir quelle colonne de données est utilisée pour l’axe x du graphique.

    Comme alternative à `DataBindTable` l’utilisation de `AddSeries` la méthode, vous pouvez utiliser la méthode de l’aide. `Chart` La `AddSeries` méthode vous permet `xValue` `yValues` de définir les paramètres et les paramètres. Par exemple, au `DataBindTable` lieu d’utiliser la méthode comme celle-ci:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Vous pouvez `AddSeries` utiliser la méthode comme celle-ci:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Les deux rendent les mêmes résultats. La `AddSeries` méthode est plus flexible car vous pouvez spécifier le type de graphique et les données plus explicitement, mais la `DataBindTable` méthode est plus facile à utiliser si vous n’avez pas besoin de flexibilité supplémentaire.
5. Exécutez la page dans un navigateur. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilisation de données XML

La troisième option pour la cartographie est d’utiliser un fichier XML comme données pour le graphique. Cela exige que le fichier XML dispose également d’un fichier schéma *(fichier .xsd)* qui décrit la structure XML. Cette procédure vous montre comment lire les données d’un fichier XML.

1. Dans le dossier *App\_Data,* créez un nouveau fichier XML nommé *data.xml*.
2. Remplacez le XML existant par ce qui suit, qui est quelques données XML sur les employés dans une entreprise fictive. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Dans le dossier *\_App Data,* créez un nouveau fichier XML nommé *data.xsd*. (Notez que la prolongation de cette période est *.xsd*.)
4. Remplacer le XML existant par les éléments suivants : 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Dans la racine du site, créer un nouveau fichier nommé *ChartDataXML.cshtml*.
6. Remplacer le contenu existant par les éléments suivants : 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Le code crée `DataSet` d’abord un objet. Cet objet est utilisé pour gérer les données qui sont lues à partir du fichier XML et l’organiser en fonction des informations contenues dans le fichier schéma. (Notez que le haut du `using SystemData`code comprend l’instruction . Cela est nécessaire pour pouvoir travailler `DataSet` avec l’objet. Pour plus d’informations, voir [ &quot;Utilisation des&quot; déclarations et des noms entièrement qualifiés](#SB_UsingStatements) plus tard dans cet article.)

    Ensuite, le code `DataView` crée un objet basé sur le jeu de données. La vue des données fournit un objet que le graphique peut lier à &#8212; qui est, lire et tracer. Le graphique se lie aux `AddSeries` données à l’aide de la méthode, comme `xValue` vous `yValues` l’avez vu `DataView` plus tôt lors de la cartographie des données de tableau, sauf que cette fois le et les paramètres sont réglés à l’objet.

    Cet exemple vous montre également comment spécifier un type de graphique particulier. Lorsque les données sont `AddSeries` ajoutées `chartType` dans la méthode, le paramètre est également défini pour afficher un graphique à secteurs.
7. Exécutez la page dans un navigateur. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Déclarations « utilisations » et noms entièrement qualifiés
> 
> Le cadre .NET qui ASP.NET pages Web avec la syntaxe Razor est basé sur plusieurs milliers de composants (classes). Pour rendre gérable de travailler avec toutes ces classes, ils sont organisés en *espaces de noms*, qui sont un peu comme les bibliothèques. Par exemple, `System.Web` l’espace nom contient des classes `System.Xml` qui prennent en charge la communication par navigateur/serveur, l’espace nom contient des classes qui sont utilisées pour créer et lire des fichiers XML, et l’espace `System.Data` nom contient des classes qui vous permettent de travailler avec des données.
> 
> Afin d’accéder à n’importe quelle classe donnée dans le cadre .NET, le code doit connaître non seulement le nom de classe, mais aussi l’espace de nom que la classe est en. Par exemple, afin d’utiliser `Chart` l’aide, `System.Web.Helpers.Chart` le code doit trouver`System.Web.Helpers`la classe, qui`Chart`combine l’espace de nom ( ) avec le nom de classe ( ). C’est ce qu’on appelle le nom *entièrement qualifié* de la classe &#8212; son emplacement complet et sans ambiguïté dans l’immensité du cadre .NET. Dans le code, cela ressemblerait à ce qui suit:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Cependant, il est lourd (et sujet aux erreurs) d’avoir à utiliser ces longs noms entièrement qualifiés chaque fois que vous voulez vous référer à une classe ou un aide. Par conséquent, pour le rendre plus facile à utiliser les noms de classe, vous pouvez *importer* les espaces de nom qui vous intéressent, qui est généralement juste une poignée parmi les nombreux espaces de noms dans le cadre .NET. Si vous avez importé un namespace, vous pouvez`Chart`utiliser juste un nom`System.Web.Helpers.Chart`de classe ( ) au lieu du nom entièrement qualifié (). Lorsque votre code s’exécute et rencontre un nom de classe, il peut regarder dans les espaces de nom que vous avez importés pour trouver cette classe.
> 
> Lorsque vous utilisez ASP.NET Pages Web avec la syntaxe Razor pour créer des pages `WebPage` Web, vous utilisez généralement le même ensemble de classes à chaque fois, y compris la classe, les différents aides, et ainsi de suite. Pour vous éviter le travail d’importation des espaces de noms pertinents chaque fois que vous créez un site Web, ASP.NET est configuré de sorte qu’il importe automatiquement un ensemble d’espaces de noms de base pour chaque site Web. C’est pourquoi vous n’avez pas eu à faire face à des espaces de nom ou d’importer jusqu’à présent; toutes les classes avec lesquelles vous avez travaillé sont dans des espaces de nom qui sont déjà importés pour vous.
> 
> Cependant, parfois, vous devez travailler avec une classe qui n’est pas dans un espace de nom qui est automatiquement importé pour vous. Dans ce cas, vous pouvez soit utiliser le nom entièrement qualifié de cette classe, soit vous pouvez importer manuellement l’espace nom qui contient la classe. Pour importer un espace nomin, vous utilisez la `using` déclaration (dans`import` Visual Basic), comme vous l’avez vu dans un exemple plus tôt l’article.
> 
> Par exemple, `DataSet` la classe `System.Data` est dans l’espace nom. L’espace `System.Data` nom n’est pas automatiquement disponible pour ASP.NET pages Razor. Par conséquent, pour `DataSet` travailler avec la classe en utilisant son nom entièrement qualifié, vous pouvez utiliser le code comme ceci:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Si vous devez `DataSet` utiliser la classe à plusieurs reprises, vous pouvez importer un espace de nom comme celui-ci et ensuite utiliser le nom de classe dans le code:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Vous pouvez `using` ajouter des instructions pour tous les autres espaces nom de cadre .NET que vous souhaitez référencer. Cependant, comme indiqué, vous n’aurez pas besoin de le faire souvent, parce que la plupart des classes que vous travaillerez avec sont dans les espaces de nom qui sont importés automatiquement par ASP.NET pour une utilisation dans *.cshtml* et *.vbhtml* pages.

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Affichage de graphiques à l’intérieur d’une page Web

Dans les exemples que vous avez vus jusqu’à présent, vous créez un graphique, puis le graphique est rendu directement au navigateur comme un graphique. Dans de nombreux cas, cependant, vous voulez afficher un graphique dans le cadre d’une page, pas seulement par lui-même dans le navigateur. Pour ce faire, il faut un processus en deux étapes. La première étape consiste à créer une page qui génère le graphique, comme vous l’avez déjà vu.

La deuxième étape consiste à afficher l’image résultante dans une autre page. Pour afficher l’image, `<img>` vous utilisez un élément HTML, de la même manière que vous pouvez afficher n’importe quelle image. Cependant, au lieu de faire référence à un `<img>` fichier *.jpg* ou *.png,* l’élément fait référence au fichier *.cshtml* qui contient l’aide `Chart` qui crée le graphique. Lorsque la page d’affichage s’exécute, l’élément `<img>` obtient la sortie de l’aide `Chart` et rend le graphique.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Créer un fichier nommé *ShowChart.cshtml*.
2. Remplacer le contenu existant par les éléments suivants : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Le code `<img>` utilise l’élément pour afficher le graphique que vous avez créé plus tôt dans le fichier *ChartArrayBasic.cshtml.*
3. Exécutez la page Web dans un navigateur. Le fichier *ShowChart.cshtml* affiche l’image graphique basée sur le code contenu dans le fichier *ChartArrayBasic.cshtml.*

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Styling a Chart

L’aide `Chart` prend en charge un grand nombre d’options qui vous permettent de personnaliser l’apparence du graphique. Vous pouvez définir des couleurs, des polices, des bordures, et ainsi de suite. Un moyen facile de personnaliser l’apparence d’un graphique est d’utiliser un *thème*. Les thèmes sont des collections d'informations qui spécifient de quelle manière restituer un graphique à l'aide de couleurs, d'étiquettes, de palettes, de bordures et d'effets. (Notez que le style d’un graphique n’indique pas le type de graphique.)

Le tableau suivant répertorie les thèmes intégrés.

| Thème | Description |
| --- | --- |
| `Vanilla` | Affiche des colonnes rouges sur un fond blanc. |
| `Blue` | Affiche des colonnes bleues sur un fond de gradient bleu. |
| `Green` | Affiche des colonnes bleues sur un fond de gradient vert. |
| `Yellow` | Affiche des colonnes oranges sur un fond de gradient jaune. |
| `Vanilla3D` | Affiche des colonnes rouges 3D sur un fond blanc. |

Vous pouvez spécifier le thème à utiliser lorsque vous créez un nouveau graphique.

1. Créez un nouveau fichier nommé *ChartStyleGreen.cshtml*.
2. Remplacer le contenu existant de la page par les éléments suivants :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Ce code est le même que l’exemple précédent qui `theme` utilise la `Chart` base de données pour les données, mais ajoute le paramètre quand il crée l’objet. Ce qui suit montre le code modifié :

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Exécutez la page dans un navigateur. Vous voyez les mêmes données qu’auparavant, mais le graphique semble plus poli: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Enregistrer un graphique

Lorsque vous `Chart` utilisez l’aide comme vous l’avez vu jusqu’à présent dans cet article, l’aide re-crée le graphique à partir de zéro chaque fois qu’il est invoqué. Si nécessaire, le code du graphique ré-interroge également la base de données ou relit le fichier XML pour obtenir les données. Dans certains cas, cela peut être une opération complexe, comme si la base de données que vous interrogez est grande, ou si le fichier XML contient beaucoup de données. Même si le graphique n’implique pas beaucoup de données, le processus de création dynamique d’une image prend des ressources serveur, et si de nombreuses personnes demandent la page ou les pages qui affichent le graphique, il peut y avoir un impact sur les performances de votre site Web.

Pour vous aider à réduire l’impact potentiel de la création d’un graphique, vous pouvez créer un graphique la première fois que vous en avez besoin, puis l’enregistrer. Lorsque le graphique est nécessaire à nouveau, plutôt que de le régénérer, vous pouvez simplement aller chercher la version enregistrée et le rendre.

Vous pouvez enregistrer un graphique de ces façons :

- Cachez le graphique dans la mémoire de l’ordinateur (sur le serveur).
- Enregistrez le graphique sous forme de fichier d’image.
- Enregistrez le graphique sous forme de fichier XML. Cette option vous permet de modifier le graphique avant de l’enregistrer.

### <a name="caching-a-chart"></a>Caching a Chart

Une fois que vous avez créé un graphique, vous pouvez le mettre en cache. Le cachement d’un graphique signifie qu’il n’a pas besoin d’être recréé s’il doit être affiché à nouveau. Lorsque vous enregistrez un graphique dans le cache, vous lui donnez une clé qui doit être unique à ce graphique.

Les graphiques enregistrés sur le cache peuvent être supprimés si le serveur est faible sur la mémoire. En outre, le cache est effacé si votre application redémarre pour une raison quelconque. Par conséquent, la façon standard de travailler avec un graphique en cache est de toujours vérifier d’abord si elle est disponible dans le cache, et si ce n’est pas le cas, puis de le créer ou de le recréer.

1. À la racine de votre site Web, créez un fichier nommé *ShowCachedChart.cshtml*.
2. Remplacer le contenu existant par les éléments suivants : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    L’étiquette `<img>` `src` comprend un attribut qui pointe vers le fichier *ChartSaveToCache.cshtml* et passe une clé à la page comme une chaîne de requête. La clé contient &quot;la valeur&quot;myChartKey . Le fichier *ChartSaveToCache.cshtml* contient l’aide `Chart` qui crée le graphique. Vous allez créer cette page dans un instant.

    À la fin de la page il ya un lien vers une page nommée *ClearCache.cshtml*. C’est une page que vous allez également créer sous peu. Vous avez besoin de la *ClearCache.cshtml* seulement pour tester la mise en cache pour cet exemple - ce n’est pas un lien ou une page que vous incluriez normalement lorsque vous travaillez avec des graphiques en cache.
3. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSaveToCache.cshtml*.
4. Remplacer le contenu existant par les éléments suivants :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Le code vérifie d’abord si quelque chose a été adopté comme la valeur clé dans la chaîne de requête. Si c’est le cas, le code essaie `GetFromCache` de lire un graphique hors du cache en appelant la méthode et en la passant la clé. S’il s’avère qu’il n’y a rien dans le cache sous cette clé (ce qui se produirait la première fois que le graphique est demandé), le code crée le graphique comme d’habitude. Lorsque le graphique est terminé, le code `SaveToCache`l’enregistre dans le cache en appelant . Cette méthode nécessite une clé (de sorte que le graphique peut être demandé plus tard), et la quantité de temps que le graphique doit être enregistré dans le cache. (L’heure exacte à laquelle vous cachez un graphique dépendrait de la fréquence à laquelle vous pensiez que les données qu’il représente pourraient changer.) La `SaveToCache` méthode nécessite `slidingExpiration` également un paramètre &#8212; si cela est réglé pour vrai, le compteur de délai d’attente est réinitialisé chaque fois que le graphique est consulté. Dans ce cas, cela signifie en effet que l’entrée de cache du graphique expire 2 minutes après la dernière fois que quelqu’un a accédé au graphique. (L’alternative à l’expiration coulissante est l’expiration absolue, ce qui signifie que l’entrée du cache expirerait exactement 2 minutes après qu’elle a été mise dans le cache, peu importe la fréquence à laquelle elle avait été consultée.)

    Enfin, le code `WriteFromCache` utilise la méthode pour aller chercher et rendre le graphique à partir du cache. Notez que cette `if` méthode est en dehors du bloc qui vérifie le cache, car il obtiendra le graphique à partir du cache si le graphique était là pour commencer ou a dû être généré et enregistré dans le cache.

    Notez que dans `AddTitle` l’exemple, la méthode comprend un chrono. (Il ajoute la date actuelle et `DateTime.Now` l’heure &#8212; &#8212; au titre.)
5. Créez une nouvelle page nommée *ClearCache.cshtml* et remplacez son contenu par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Cette page `WebCache` utilise l’aide pour supprimer le graphique qui est mis en cache dans *ChartSaveToCache.cshtml*. Comme indiqué précédemment, vous n’avez normalement pas besoin d’avoir une page comme celle-ci. Vous le créez ici uniquement pour faciliter le test de mise en cache.
6. Exécutez la page Web *ShowCachedChart.cshtml* dans un navigateur. La page affiche l’image graphique en fonction du code contenu dans le fichier *ChartSaveToCache.cshtml.* Prenez note de ce que dit l’ampoule dans le titre du graphique. 

    ![Description: Image du tableau de base avec timestamp dans le titre du graphique](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Fermez le navigateur.
8. Exécutez à nouveau le *ShowCachedChart.cshtml.* Notez que le timestamp est le même qu’avant, ce qui indique que le graphique n’a pas été régénéré, mais a plutôt été lu à partir du cache.
9. Dans *ShowCachedChart.cshtml*, cliquez sur le lien **de cache Clear.** Cela vous emmène à *ClearCache.cshtml*, qui rapporte que le cache a été effacé.
10. Cliquez sur le **lien Return to ShowCachedChart.cshtml,** ou re-run *ShowCachedChart.cshtml* de WebMatrix. Notez que cette fois, le timestamp a changé, parce que le cache a été effacé. Par conséquent, le code a dû régénérer le graphique et le remettre dans le cache.

### <a name="saving-a-chart-as-an-image-file"></a>Enregistrer un graphique comme fichier d’image

Vous pouvez également enregistrer un graphique sous forme de fichier d’image (par exemple, sous forme de fichier *.jpg)* sur le serveur. Vous pouvez ensuite utiliser le fichier d’image comme vous le feriez n’importe quelle image. L’avantage est que le fichier est stocké plutôt que enregistré dans un cache temporaire. Vous pouvez enregistrer une nouvelle image graphique à des moments différents (par exemple, toutes les heures) et ensuite tenir un registre permanent des modifications qui se produisent au fil du temps. Notez que vous devez vous assurer que votre application Web a la permission d’enregistrer un fichier sur le dossier sur le serveur où vous souhaitez mettre le fichier d’image.

1. À la racine de votre site Web, créez un dossier nommé * \_ChartFiles* s’il n’existe pas déjà.
2. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSave.cshtml*.
3. Remplacer le contenu existant par les éléments suivants :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Le code vérifie d’abord si le fichier *.jpg* existe en appelant la `File.Exists` méthode. Si le fichier n’existe pas, `Chart` le code crée un nouveau à partir d’un tableau. Cette fois, le `Save` code appelle `path` la méthode et passe le paramètre pour spécifier le chemin de fichier et le nom de fichier de l’endroit où enregistrer le graphique. Dans le corps de `<img>` la page, un élément utilise le chemin pour pointer vers le fichier *.jpg* pour afficher.
4. Exécutez le fichier *ChartSave.cshtml.*
5. Retour sur WebMatrix. Notez qu’un fichier d’image nommé *chart01.jpg* a été enregistré dans le * \_dossier ChartFiles.*

### <a name="saving-a-chart-as-an-xml-file"></a>Enregistrer un graphique comme fichier XML

Enfin, vous pouvez enregistrer un graphique en tant que fichier XML sur le serveur. Un avantage d’utiliser cette méthode sur la mise en cache du graphique ou l’enregistrement du graphique à un fichier est que vous pouvez modifier le XML avant d’afficher le graphique si vous le vouliez. Votre application doit avoir lu/écrire des autorisations pour le dossier sur le serveur où vous souhaitez mettre le fichier d’image.

1. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSaveXml.cshtml*.
2. Remplacer le contenu existant par les éléments suivants :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Ce code est similaire au code que vous avez vu plus tôt pour stocker un graphique dans le cache, sauf qu’il utilise un fichier XML. Le code vérifie d’abord si le fichier `File.Exists` XML existe en appelant la méthode. Si le fichier existe, le `Chart` code crée un nouvel `themePath` objet et passe le nom de fichier comme paramètre. Cela crée le graphique basé sur tout ce qui est dans le fichier XML. Si le fichier XML n’existe pas déjà, le code `SaveXml` crée un graphique comme d’habitude, puis appelle pour l’enregistrer. Le graphique est rendu `Write` en utilisant la méthode, comme vous l’avez vu avant.

    Comme avec la page qui a montré la mise en cache, ce code inclut un timestamp dans le titre du graphique.
3. Créez une nouvelle page nommée *ChartDisplayXMLChart.cshtml* et ajoutez la marque suivante : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Exécutez la page *ChartDisplayXMLChart.cshtml.* Le graphique est affiché. Prenez note de l’éd du temps dans le titre du graphique.
5. Fermez le navigateur.
6. Dans WebMatrix, cliquez à droite sur le **Refresh** * \_dossier ChartFiles,* cliquez sur Refresh, puis ouvrez le dossier. Le fichier *XMLChart.xml* dans ce dossier `Chart` a été créé par l’aide. 

    ![Description: Le dossier _ChartFiles montrant le fichier XMLChart.xml créé par l’aide graphique.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Exécutez à nouveau la page *ChartDisplayXMLChart.cshtml.* Le graphique affiche le même timetamp que la première fois que vous avez couru la page. C’est parce que le graphique est généré à partir de la XML que vous avez enregistré plus tôt.
8. Dans WebMatrix, * \_* ouvrez le dossier ChartFiles et supprimez le fichier *XMLChart.xml.*
9. Exécutez la page *ChartDisplayXMLChart.cshtml* une fois de plus. Cette fois, le timetamp est `Chart` mis à jour, parce que l’aide a dû recréer le fichier XML. Si vous le * \_* souhaitez, vérifiez le dossier ChartFiles et remarquez que le fichier XML est de retour.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à travailler avec une base de données dans ASP.NET sites Web](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Utilisation de caching dans ASP.NET sites de pages Web pour améliorer les performances](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Catégorie graphique](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (ASP.NET référence de l’API pages Web sur MSDN)
