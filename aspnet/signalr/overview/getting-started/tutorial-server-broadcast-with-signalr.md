---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Diffusion du serveur avec SignalR 2 Microsoft Docs'
author: tdykstra
description: Ce tutoriel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion du serveur.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676096"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Diffusion du serveur avec SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Ce tutoriel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion du serveur. La diffusion du serveur signifie que le serveur démarre les communications envoyées aux clients.

L’application que vous allez créer dans ce tutoriel simule un ticker stock, un scénario typique pour la fonctionnalité de diffusion du serveur. Périodiquement, le serveur met au sort les prix des actions et diffuse les mises à jour de tous les clients connectés. Dans le navigateur, les nombres **%** et les symboles du **Changement** et les colonnes changent dynamiquement en réponse aux notifications du serveur. Si vous ouvrez des navigateurs supplémentaires à la même URL, ils affichent tous les mêmes données et les mêmes modifications aux données simultanément.

![Créer du web](tutorial-server-broadcast-with-signalr/_static/image1.png)

Dans ce tutoriel, vous allez :

> [!div class="checklist"]
> * Créer le projet
> * Configurer le code du serveur
> * Examiner le code serveur
> * Configurer le code client
> * Examiner le code client
> * Test de l’application
> * Activation de la journalisation

> [!IMPORTANT]
> Si vous ne souhaitez pas passer à travers les étapes de la construction de l’application, vous pouvez installer le paquet SignalR.Sample dans un nouveau projet d’application Web vide ASP.NET. Si vous installez le paquet NuGet sans effectuer les étapes de ce tutoriel, vous devez suivre les instructions dans le fichier *readme.txt.* Pour exécuter le paquet, vous devez ajouter une `ConfigureSignalR` classe de démarrage OWIN qui appelle la méthode dans le paquet installé. Vous recevrez une erreur si vous n’ajoutez pas la classe de démarrage OWIN. Voir la section [d’échantillons Install the StockTicker](#install-the-stockticker-sample) de cet article.

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.

## <a name="create-the-project"></a>Créer le projet

Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide.

1. Dans Visual Studio, créez une application Web ASP.NET.

    ![Créer du web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Dans la **nouvelle application Web ASP.NET - fenêtre SignalR.StockTicker,** laissez **Vide** sélectionné et sélectionnez **OK**.

## <a name="set-up-the-server-code"></a>Configurer le code du serveur

Dans cette section, vous configurez le code qui s’exécute sur le serveur.

### <a name="create-the-stock-class"></a>Créer la classe Stock

Vous commencez par créer la classe de modèle *Stock* que vous utiliserez pour stocker et transmettre des informations sur un stock.

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **classe**.

1. Nommez la classe *Stock* et ajoutez-le au projet.

1. Remplacez le code dans le *fichier Stock.cs* avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Les deux propriétés que vous définirez `Symbol` lorsque vous créez des stocks `Price`sont (par exemple, MSFT pour Microsoft) et . Les autres propriétés dépendent de `Price`comment et quand vous définissez . La première fois `Price`que vous définissez, `DayOpen`la valeur se propage à . Après cela, lorsque `Price`vous définissez, `Change` `PercentChange` l’application calcule `Price` les `DayOpen`valeurs et les valeurs de la propriété en fonction de la différence entre et .

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Créer les classes StockTickerHub et StockTicker

Vous utiliserez l’API SignalR Hub pour gérer l’interaction serveur-client. Une `StockTickerHub` classe qui dérive de `Hub` la classe SignalR s’occupera des connexions de réception et des appels de méthode des clients. Vous devez également maintenir les `Timer` données de stock et exécuter un objet. L’objet `Timer` déclenchera périodiquement des mises à jour de prix indépendantes des connexions client. Vous ne pouvez pas mettre `Hub` ces fonctions dans une classe, parce que les hubs sont transitoires. L’application `Hub` crée un exemple de classe pour chaque tâche sur le hub, comme les connexions et les appels du client au serveur. Ainsi, le mécanisme qui conserve les données sur les stocks, met à jour les prix et diffuse les mises à jour des prix doit fonctionner dans une classe distincte. Vous nommerez la `StockTicker`classe .

![Diffusion de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Vous ne voulez qu’une seule instance de la `StockTicker` classe pour s’exécuter `StockTickerHub` sur le serveur, de sorte que vous aurez besoin de configurer une référence de chaque instance à l’instance singleton. `StockTicker` La `StockTicker` classe doit diffuser aux clients parce qu’elle a `StockTicker` les données `Hub` d’actions et déclenche des mises à jour, mais n’est pas une classe. La `StockTicker` classe doit obtenir une référence à l’objet de contexte de connexion SignalR Hub. Il peut ensuite utiliser l’objet de contexte de connexion SignalR pour diffuser aux clients.

#### <a name="create-stocktickerhubcs"></a>Créer des StockTickerHub.cs

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **un nouvel article**.

1. Dans **Ajouter un nouvel article - SignalR.StockTicker**, sélectionnez Le signal**web** **Visual** > **CMD** >  **installé,** > puis sélectionnez **La classe De hub SignalR (v2)**.

1. Nommez la classe *StockTickerHub* et ajoutez-la au projet.

    Cette étape crée le fichier de classe *StockTickerHub.cs.* Simultanément, il ajoute un ensemble de fichiers de script et de références d’assemblage qui prend en charge SignalR au projet.

1. Remplacez le code dans le fichier *StockTickerHub.cs* avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Enregistrez le fichier .

L’application utilise la classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) pour définir les méthodes que les clients peuvent appeler sur le serveur. Vous définissez une `GetAllStocks()`méthode : . Lorsqu’un client se connecte d’abord au serveur, il appelle cette méthode pour obtenir une liste de tous les stocks avec leurs prix actuels. La méthode peut fonctionner de `IEnumerable<Stock>` façon synchrone et revenir parce qu’elle renvoie les données de la mémoire.

Si la méthode devait obtenir les données en faisant quelque chose qui impliquerait l’attente, comme une recherche de base de données ou un appel de service Web, vous pouvez spécifier `Task<IEnumerable<Stock>>` comme valeur de retour pour activer le traitement asynchrone. Pour plus d’informations, voir [ASP.NET SignalR Hubs API Guide - Server - Quand exécuter asynchronement](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

L’attribut `HubName` précise comment l’application fera référence au code Hub dans JavaScript sur le client. Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une `stockTickerHub`version camelCase du nom de classe, qui dans ce cas serait .

Comme vous le verrez plus `StockTicker` tard lorsque vous créez la classe, l’application crée une instance singleton de cette classe dans sa propriété statique. `Instance` Cette instance singleton de est dans la `StockTicker` mémoire, peu importe combien de clients se connectent ou se déconnectent. C’est le `GetAllStocks()` cas de cette méthode pour retourner les informations actuelles sur les stocks.

#### <a name="create-stocktickercs"></a>Créer des StockTicker.cs

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **classe**.

1. Nommez la classe *StockTicker* et ajoutez-la au projet.

1. Remplacez le code dans le *fichier StockTicker.cs* avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Étant donné que tous les threads seront en cours d’exécution de la même instance de code StockTicker, la classe StockTicker doit être thread-safe.

### <a name="examine-the-server-code"></a>Examiner le code serveur

Si vous examinez le code serveur, il vous aidera à comprendre comment fonctionne l’application.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Stockage de l’instance singleton dans un champ statique

Le code initialise `_instance` le champ statique `Instance` qui soutient la propriété avec une instance de la classe. Parce que le constructeur est privé, c’est le seul exemple de la classe que l’application peut créer. L’application utilise [l’initialisation paresseuse](/dotnet/framework/performance/lazy-initialization) pour le `_instance` domaine. Ce n’est pas pour des raisons de performance. Il s’agit de s’assurer que la création d’instance est sans fil.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Chaque fois qu’un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub en cours d’exécution dans un thread séparé obtient l’instance StockTicker singleton de la propriété statique, comme vous l’avez `StockTicker.Instance` vu plus tôt dans la `StockTickerHub` classe.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Stockage des données sur les stocks dans un

Le constructeur initialise `_stocks` la collecte avec quelques `GetAllStocks` données d’exemple de stock, et retourne les stocks. Comme vous l’avez vu plus `StockTickerHub.GetAllStocks`tôt, cette collection d’actions est retournée par , qui est une méthode de serveur dans la classe que les `Hub` clients peuvent appeler.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La collecte des stocks est définie comme un type [concurrentdictionnaire](https://msdn.microsoft.com/library/dd287191.aspx) pour la sécurité des threads. Comme alternative, vous pouvez utiliser un objet [De dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) et verrouiller explicitement le dictionnaire lorsque vous y modifiez.

Pour cette application d’exemple, il est acceptable de stocker les données d’application en mémoire et de perdre les données lorsque l’application se débarrasse de l’instance. `StockTicker` Dans une application réelle, vous travailleriez avec un magasin de données back-end comme une base de données.

#### <a name="periodically-updating-stock-prices"></a>Mise à jour périodique des cours des actions

Le constructeur démarre `Timer` un objet qui appelle périodiquement des méthodes qui mettent à jour les cours des actions sur une base aléatoire.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer`appels `UpdateStockPrices`, qui passe en null dans le paramètre de l’état. Avant de mettre à jour les `_updateStockPricesLock` prix, l’application prend un verrou sur l’objet. Le code vérifie si un autre thread est `TryUpdateStockPrice` déjà mise à jour des prix, puis il appelle sur chaque stock de la liste. La `TryUpdateStockPrice` méthode décide de changer le cours de l’action, et combien de le changer. Si le cours de l’action change, l’application appelle `BroadcastStockPrice` à diffuser le changement de cours de l’action à tous les clients connectés.

Le `_updatingStockPrices` drapeau désigné [volatil](https://msdn.microsoft.com/library/x13ttww7.aspx) pour s’assurer qu’il est sans fil.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

Dans une application `TryUpdateStockPrice` réelle, la méthode appellerait un service web pour rechercher le prix. Dans ce code, l’application utilise un générateur de nombres aléatoires pour effectuer des modifications au hasard.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtenir le contexte SignalR afin que la classe StockTicker puisse diffuser aux clients

Parce que les changements de `StockTicker` prix proviennent ici de l’objet, c’est l’objet qui doit appeler une `updateStockPrice` méthode sur tous les clients connectés. Dans `Hub` une classe, vous avez une API `StockTicker` pour appeler les `Hub` méthodes client, mais ne `Hub` dérive pas de la classe et n’a pas de référence à un objet. Pour diffuser aux clients `StockTicker` connectés, la classe doit obtenir `StockTickerHub` l’exemple de contexte SignalR pour la classe et l’utiliser pour appeler des méthodes sur les clients.

Le code obtient une référence au contexte SignalR quand il crée l’instance de classe singleton, `Clients` passe cette référence au constructeur, et le constructeur le met dans la propriété.

Il ya deux raisons pour lesquelles vous voulez obtenir le contexte qu’une seule fois: obtenir le contexte est une tâche coûteuse, et l’obtenir une fois s’assure que l’application préserve l’ordre prévu de messages envoyés aux clients.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obtenir `Clients` la propriété du contexte et `StockTickerClient` de le mettre dans la propriété vous permet d’écrire du code pour appeler les méthodes client qui ressemble à ce qu’il ferait dans une `Hub` classe. Par exemple, pour diffuser à `Clients.All.updateStockPrice(stock)`tous les clients que vous pouvez écrire .

La `updateStockPrice` méthode que vous `BroadcastStockPrice` appelez n’existe pas encore. Vous l’ajouterez plus tard lorsque vous rédigerez du code qui s’exécute sur le client. Vous pouvez `updateStockPrice` vous `Clients.All` référer ici parce que c’est dynamique, ce qui signifie que l’application évaluera l’expression au moment de l’exécution. Lorsque cet appel de méthode s’exécute, SignalR enverra le nom de la méthode `updateStockPrice`et la valeur de paramètre au client, et si le client a une méthode nommée, l’application appellera cette méthode et lui transmettra la valeur de paramètre.

`Clients.All`signifie envoyer à tous les clients. SignalR vous offre d’autres options pour spécifier à quels clients ou groupes de clients envoyer. Pour plus d’informations, voir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Enregistrer l’itinéraire SignalR

Le serveur doit savoir quelle URL intercepter et directement à SignalR. Pour ce faire, ajoutez une classe de démarrage OWIN :

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **un nouvel article**.

1. Dans **Ajouter un nouvel article - SignalR.StockTicker** sélectionnez Le**Web** **visual CMD** >  **installé,** > puis sélectionnez **OWIN Startup Class**.

1. Nommez la classe *Startup* et sélectionnez **OK**.

1. Remplacez le code par défaut dans le fichier *Startup.cs* avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Vous avez maintenant fini de configurer le code serveur. Dans la section suivante, vous configurerez le client.

## <a name="set-up-the-client-code"></a>Configurer le code client

Dans cette section, vous configurez le code qui s’exécute sur le client.

### <a name="create-the-html-page-and-javascript-file"></a>Créez la page HTML et le fichier JavaScript

La page HTML affichera les données et le fichier JavaScript organisera les données.

#### <a name="create-stocktickerhtml"></a>Créer StockTicker.html

Tout d’abord, vous ajouterez le client HTML.

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **la page HTML**.

1. Nommez le fichier *StockTicker* et sélectionnez **OK**.

1. Remplacez le code par défaut dans le fichier *StockTicker.html* avec ce code :

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Le HTML crée une table avec cinq colonnes, une rangée d’en-têtes, et une ligne de données avec une seule cellule qui s’étend sur les cinq colonnes. La ligne de données montre "chargement..." momentanément lorsque l’application démarre. Le code JavaScript supprimera cette ligne et ajoutera dans ses rangées de place avec les données de stock récupérées à partir du serveur.

    Les balises de script spécifient :

    * Le fichier de script jQuery.

    * Le fichier de script de base SignalR.

    * Le fichier de script SignalR procurations.

    * Un fichier de script StockTicker que vous créerez plus tard.

    L’application génère dynamiquement le fichier de script SignalR proxies. Il spécifie l’URL "/signaleur/hubs" et définit les méthodes proxy pour `StockTickerHub.GetAllStocks`les méthodes de la classe Hub, dans ce cas, pour . Si vous préférez, vous pouvez générer ce fichier JavaScript manuellement en utilisant [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). N’oubliez pas de désactiver la `MapHubs` création de fichiers dynamique dans l’appel de méthode.

1. Dans **Solution Explorer**, élargir **Scripts**.

    Les bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    > [!IMPORTANT]
    > Le gestionnaire de paquet installera une version ultérieure des scripts SignalR.

1. Mettre à jour les références de script dans le bloc de code pour correspondre aux versions des fichiers script du projet.

1. Dans **Solution Explorer**, clic droit *StockTicker.html*, puis sélectionnez **Set comme Page de départ**.

#### <a name="create-stocktickerjs"></a>Créer StockTicker.js

Maintenant, créez le fichier JavaScript.

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **Le fichier JavaScript**.

1. Nommez le fichier *StockTicker* et sélectionnez **OK**.

1. Ajoutez ce code au fichier *StockTicker.js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examiner le code client

Si vous examinez le code client, il vous aidera à apprendre comment le code client interagit avec le code serveur pour faire fonctionner l’application.

#### <a name="starting-the-connection"></a>Démarrage de la connexion

`$.connection`se réfère aux procurations SignalR. Le code obtient une référence `StockTickerHub` au proxy pour `ticker` la classe et le met dans la variable. Le nom de procuration est le `HubName` nom qui a été défini par l’attribut:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Après avoir défini toutes les variables et fonctions, la dernière ligne de code du `start` fichier est para parasciné la connexion SignalR en appelant la fonction SignalR. La `start` fonction exécute asynchronement et renvoie un [objet différé jQuery](http://api.jquery.com/category/deferred-object/). Vous pouvez appeler la fonction accomplie pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtenir tous les stocks

La `init` fonction `getAllStocks` appelle la fonction sur le serveur et utilise les informations que le serveur retourne pour mettre à jour la table de stock. Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est pascal-cased sur le serveur. La règle de la camelcasing ne s’applique qu’aux méthodes, et non aux objets. Par exemple, vous `stock.Symbol` `stock.Price`vous `stock.symbol` référez à et , pas ou `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Dans `init` la méthode, l’application crée HTML pour une ligne de `formatStock` table pour `stock` chaque objet d’actions reçu du serveur en appelant aux propriétés de format de l’objet, puis en appelant `supplant` pour remplacer les propriétaires de place dans la `rowTemplate` variable avec les valeurs de propriété `stock` d’objet. Le HTML résultant est ensuite joint à la table de stock.

> [!NOTE]
> Vous `init` appelez en le `callback` passant comme une fonction qui exécute `start` après la fonction asynchrone se termine. Si vous `init` avez appelé comme une `start`déclaration JavaScript séparée après l’appel , la fonction échouerait parce qu’il fonctionnerait immédiatement sans attendre la fonction de départ pour finir d’établir la connexion. Dans ce cas, la `init` fonction `getAllStocks` essaierait d’appeler la fonction avant que l’application n’établisse une connexion serveur.

#### <a name="getting-updated-stock-prices"></a>Obtenir des prix des actions mis à jour

Lorsque le serveur change le prix d’une action, il appelle les `updateStockPrice` clients connectés. L’application ajoute la fonction à `stockTicker` la propriété client du proxy pour le rendre disponible pour les appels à partir du serveur.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La `updateStockPrice` fonction formate un objet stock reçu du serveur dans `init` une ligne de table de la même manière que dans la fonction. Au lieu d’enchanger la ligne à la table, il trouve la ligne actuelle du stock dans la table et remplace cette rangée par la nouvelle.

## <a name="test-the-application"></a>Test de l’application

Vous pouvez tester l’application pour vous assurer qu’elle fonctionne. Vous verrez toutes les fenêtres du navigateur afficher la table de stock en direct avec des prix des actions fluctuant.

1. Dans la barre d’outils, activez **Script Debugging,** puis sélectionnez le bouton de lecture pour exécuter l’application en mode Debug.

    ![Capture d’écran de l’utilisateur tournant sur le mode de débogage et la sélection de jeu.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Une fenêtre du navigateur s’ouvrira affichant la **table de stock en direct**. Le tableau des actions montre d’abord le "chargement..." ligne, puis, après un court laps de temps, l’application affiche les données initiales des stocks, puis les prix des actions commencent à changer.

1. Copiez l’URL du navigateur, ouvrez deux autres navigateurs et collez les URL dans les barres d’adresse.

    L’affichage initial du stock est le même que le premier navigateur et les modifications se produisent simultanément.

1. Fermez tous les navigateurs, ouvrez un nouveau navigateur et passez à la même URL.

    L’objet De singleton StockTicker a continué à s’exécuter dans le serveur. Le **tableau des actions en direct** montre que les actions ont continué de changer. Vous ne voyez pas le tableau initial avec zéro chiffre de changement.

1. Fermez le navigateur.

## <a name="enable-logging"></a>Activation de la journalisation

SignalR dispose d’une fonction d’enregistrement intégrée que vous pouvez activer sur le client pour aider au dépannage. Dans cette section, vous activez l’enregistrement et voyez des exemples qui montrent comment les journaux vous indiquent laquelle des méthodes de transport suivantes SignalR utilise :

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par l’IIS 8 et les navigateurs actuels.

* [Événements envoyés par le serveur,](http://en.wikipedia.org/wiki/Server-sent_events)pris en charge par des navigateurs autres qu’Internet Explorer.

* [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.

* [Ajax long sondage](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), soutenu par tous les navigateurs.

Pour une connexion donnée, SignalR choisit la meilleure méthode de transport que le serveur et le support client.

1. Ouvert *StockTicker.js*.

1. Ajoutez cette ligne de code mise en évidence pour activer la connexion immédiatement avant le code qui initialise la connexion à la fin du fichier :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Appuyez sur **F5** pour mener à bien le projet.

1. Ouvrez la fenêtre d’outils de développeur de votre navigateur et sélectionnez la Console pour voir les journaux. Vous devrez peut-être rafraîchir la page pour voir les journaux de SignalR négocier le mode de transport pour une nouvelle connexion.

    * Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), le mode de transport est **WebSockets**.

    * Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7.5), le mode de transport est **iframe**.

    * Si vous exécutez Firefox 19 sur Windows 8 (IIS 8), le mode de transport est **WebSockets**.

        > [!TIP]
        > Dans Firefox, installez l’add-in Firebug pour obtenir une fenêtre Console.

    * Si vous exécutez Firefox 19 sur Windows 7 (IIS 7.5), le mode de transport est **envoyé par serveur.**

## <a name="install-the-stockticker-sample"></a>Installer l’échantillon StockTicker

[L’échantillon Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application StockTicker. Le package NuGet comprend plus de fonctionnalités que la version simplifiée que vous avez créée à partir de zéro. Dans cette section du tutoriel, vous installez le paquet NuGet et passez en revue les nouvelles fonctionnalités et le code qui les implémente.

> [!IMPORTANT]
> Si vous installez le paquet sans effectuer les étapes antérieures de ce tutoriel, vous devez ajouter une classe de démarrage OWIN à votre projet. Ce fichier readme.txt pour le paquet NuGet explique cette étape.

### <a name="install-the-signalrsample-nuget-package"></a>Installer le paquet NuGet SignalR.Sample

1. Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez Manage **NuGet Packages**.

1. Dans **NuGet Package manager: SignalR.StockTicker**, **sélectionnez Browse**.

1. À partir de **la source de paquet**, sélectionnez **nuget.org**.

1. Entrez *SignalR.Sample* dans la boîte de recherche et sélectionnez **Microsoft.AspNet.SignalR.Sample** > **Install**.

1. Dans **Solution Explorer**, étendre le dossier *SignalR.Sample.*

    L’installation du paquet SignalR.Sample a créé le dossier et son contenu.

1. Dans le dossier *SignalR.Sample,* cliquez à droite *StockTicker.html*, puis sélectionnez **Set As Start Page**.

    > [!NOTE]
    > L’installation du paquet NuGet SignalR.Sample peut modifier la version de jQuery que vous avez dans votre dossier *Scripts.* Le nouveau fichier *StockTicker.html* que le paquet installe dans le dossier *SignalR.Sample* sera en phase avec la version jQuery que le paquet installe, mais si vous voulez exécuter votre fichier *StockTicker.html* d’origine à nouveau, vous pourriez avoir à mettre à jour la référence jQuery dans le script tag en premier.

### <a name="run-the-application"></a>Exécution de l'application

 Le tableau que vous avez vu dans la première application avait des fonctionnalités utiles. L’application complète de ticker stock affiche de nouvelles fonctionnalités : une fenêtre horizontalement défilante qui montre les données de stock et les stocks qui changent de couleur au fur et à mesure qu’ils montent et tombent.

1. Appuyez sur **F5** pour exécuter l'application.

     Lorsque vous exécutez l’application pour la première fois, le «marché» est «fermé» et vous voyez une table statique et une fenêtre de ticker qui ne fait pas défiler.

1. Sélectionnez **Marché ouvert**.

    ![Capture d’écran du ticker en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * La boîte **Live Stock Ticker** commence à faire défiler horizontalement, et le serveur commence à diffuser périodiquement des changements de cours de l’action sur une base aléatoire.

    * Chaque fois qu’un cours de l’action change, l’application met à jour à la fois la **table d’actions en direct** et le **Ticker actions en direct**.

    * Lorsque le changement de prix d’une action est positif, l’application affiche le stock avec un fond vert.

    * Lorsque le changement est négatif, l’application affiche le stock avec un fond rouge.

1. Sélectionnez **Close Market**.

    * Les mises à jour de table s’arrêtent.

    * Le ticker cesse de faire défiler.

1. Sélectionnez **Réinitialiser**.

    * Toutes les données de stock sont réinitialisées.

    * L’application restaure l’état initial avant que les changements de prix ne commencent.

1. Copiez l’URL du navigateur, ouvrez deux autres navigateurs et collez les URL dans les barres d’adresse.

1. Vous voyez les mêmes données mises à jour dynamiquement en même temps dans chaque navigateur.

1. Lorsque vous sélectionnez l’un des contrôles, tous les navigateurs répondent de la même manière en même temps.

### <a name="live-stock-ticker-display"></a>Affichage en direct stock Ticker

**L’écran Live Stock Ticker** est une `<div>` liste non ordonnée dans un élément formaté en une seule ligne par les styles CSS. L’application initialise et met à jour le ticker de la même `<li>` manière que la `<li>` table : `<ul>` en remplaçant les détenteurs de place dans une chaîne de modèle et en ajoutant dynamiquement les éléments à l’élément. L’application comprend le défilement `animate` en utilisant la fonction jQuery pour varier `<div>`la marge gauche de la liste non ordonnée dans le .

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

Le code HTML de ticker de stock :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

Le code CSS de ticker de stock :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Le code jQuery qui le fait faire défiler:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Méthodes supplémentaires sur le serveur que le client peut appeler

Pour ajouter de la flexibilité à l’application, il existe des méthodes supplémentaires que l’application peut appeler.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Échantillon StockTickerHub.cs

La `StockTickerHub` classe définit quatre méthodes supplémentaires que le client peut appeler :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

L’application `OpenMarket` `CloseMarket`appelle `Reset` , , et en réponse aux boutons en haut de la page. Ils démontrent le modèle d’un client déclenchant un changement d’état immédiatement propagé à tous les clients. Chacune de ces méthodes `StockTicker` appelle une méthode dans la classe qui provoque le changement d’état du marché, puis diffuse le nouvel état.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

Dans `StockTicker` la catégorie, l’application maintient l’état du marché avec une `MarketState` propriété qui retourne une `MarketState` valeur enum:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Chacune des méthodes qui modifient l’état du `StockTicker` marché le font à l’intérieur d’un bloc de verrouillage parce que la classe doit être sans fil:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Pour s’assurer que ce code `_marketState` est sans `MarketState` fil, `volatile`le champ qui soutient la propriété désignée :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Les `BroadcastMarketStateChange` `BroadcastMarketReset` méthodes et les méthodes sont similaires à la méthode BroadcastStockPrice que vous avez déjà vue, sauf qu’elles appellent différentes méthodes définies chez le client :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Fonctions supplémentaires sur le client que le serveur peut appeler

La `updateStockPrice` fonction gère maintenant à la fois la table `jQuery.Color` et l’affichage ticker, et il utilise pour clignoter les couleurs rouges et vertes.

De nouvelles fonctions dans *SignalR.StockTicker.js* activent et désactivent les boutons en fonction de l’état du marché. Ils arrêtent ou démarrent également le défilement horizontal **Live Stock Ticker.** Étant donné que de `ticker.client`nombreuses fonctions sont ajoutées à , l’application utilise la [fonction d’extension jQuery](http://api.jquery.com/jQuery.extend/) pour les ajouter.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuration supplémentaire du client après l’établissement de la connexion

Une fois que le client établit la connexion, il a un travail supplémentaire à faire :

* Découvrez si le marché est ouvert ou `marketOpened` `marketClosed` fermé pour appeler le approprié ou la fonction.

* Fixez les appels de méthode serveur aux boutons.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Les méthodes du serveur ne sont pas câblées vers les boutons jusqu’à ce que l’application ait établi la connexion. C’est ainsi que le code ne peut pas appeler les méthodes du serveur avant qu’elles ne soient disponibles.

## <a name="additional-resources"></a>Ressources supplémentaires

Dans ce tutoriel, vous avez appris à programmer une application SignalR qui diffuse des messages à partir du serveur à tous les clients connectés. Vous pouvez maintenant diffuser des messages périodiquement et en réponse aux notifications de n’importe quel client. Vous pouvez utiliser le concept d’instance singleton multi-threaded pour maintenir l’état du serveur dans des scénarios de jeu en ligne multi-joueurs. Par exemple, voir [le jeu ShootR basé sur SignalR](https://github.com/NTaylorMullen/ShootR).

Pour les tutoriels qui montrent des scénarios de communication peer-to-peer, voir [Getting Started with SignalR](introduction-to-signalr.md) et [Real-Time Update with SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Pour en savoir plus sur SignalR, voir les ressources suivantes :

* [ASP.NET SignalR](../../index.md)
* [Projet SignalR](http://signalr.net/)
* [SignalR GitHub et échantillons](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous allez :

> [!div class="checklist"]
> * Création du projet
> * Configurer le code du serveur
> * Examen du code serveur
> * Configurer le code client
> * Examen du code client
> * Test de l’application
> * Enregistrement activé

Avancez à l’article suivant pour apprendre à créer une application Web en temps réel qui utilise ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Créez une application web en temps réel avec SignalR](real-time-web-applications-with-signalr.md)
