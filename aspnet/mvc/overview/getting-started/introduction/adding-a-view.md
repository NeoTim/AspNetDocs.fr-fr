---
title: Ajout d’une vue à une application MVC
author: Rick-Anderson
description: Ajout d’une vue à une application MVC
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 42469611f94b374d6692a1c2017aced77a0a414c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59403855"
---
# <a name="adding-a-view"></a>Ajout d’une vue

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section vous allez modifier la `HelloWorldController` classe à utiliser la vue fichiers de modèle proprement encapsulent le processus de génération des réponses HTML à un client. 

Vous allez créer un fichier de modèle de vue à l’aide du [moteur d’affichage Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Modèles de vue Razor ont une *.cshtml* extension de fichier et offrent un moyen élégant pour créer le HTML de sortie à l’aide de c#. Razor réduit le nombre de caractères et des touches du clavier nécessaires lors de l’écriture d’un modèle de vue et permet un rapide, fluide de flux de travail de codage.

Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Modifier le `Index` méthode à appeler les contrôleurs [vue](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) méthode, comme indiqué dans le code suivant :

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Le `Index` méthode ci-dessus utilise un modèle de vue pour générer une réponse HTML au navigateur. Méthodes de contrôleur (également appelé [méthodes d’action](http://rachelappel.com/asp.net-mvc-actionresults-explained)), comme le `Index` méthode ci-dessus, retournent généralement une [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou une classe dérivée de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), types non primitifs comme chaîne.

Bouton droit sur le *Views\HelloWorld* dossier puis cliquez sur **ajouter**, puis cliquez sur **Page de vue MVC 5 avec disposition (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
Dans le **spécifier le nom de l’élément** boîte de dialogue, entrez *Index*, puis cliquez sur **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Dans le **sélectionner une Page de disposition** boîte de dialogue, acceptez la valeur par défaut  **\_Layout.cshtml** et cliquez sur **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Dans la boîte de dialogue ci-dessus, le *Views\Shared* dossier est sélectionné dans le volet gauche. Si vous aviez un fichier de disposition personnalisée dans un autre dossier, vous pouvez le sélectionner. Nous allons parler le fichier de disposition plus loin dans le didacticiel

Le *MvcMovie\Views\HelloWorld\Index.cshtml* fichier est créé.

![](adding-a-view/_static/image4.png)

Ajoutez le balisage en surbrillance suivant.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Bouton droit sur le *Index.cshtml* fichier et sélectionnez **afficher dans le navigateur**.

![PI](adding-a-view/_static/image5.png)

Vous pouvez également avec le bouton droit cliquez sur le *Index.cshtml* fichier et sélectionnez **afficher dans l’inspecteur de Page.** Consultez le [didacticiel de l’inspecteur de Page](../../views/using-page-inspector-in-aspnet-mvc.md) pour plus d’informations.

Vous pouvez également exécuter l’application et accédez à la `HelloWorld` contrôleur (`http://localhost:xxxx/HelloWorld`). Le `Index` méthode dans votre contrôleur n’avez pas beaucoup de travail ; il a simplement exécuté l’instruction `return View()`, lequel spécifié que la méthode doit utiliser un fichier de modèle de vue pour restituer une réponse au navigateur. Étant donné que vous n’avez pas explicitement spécifier le nom du fichier de modèle de vue à utiliser, ASP.NET MVC par défaut la valeur à l’aide de la *Index.cshtml* afficher le fichier dans le *\Views\HelloWorld* dossier. L’image ci-dessous montre la chaîne &quot;Hello from our View Template !&quot; codées en dur dans la vue.

![](adding-a-view/_static/image6.png)

Il semble assez bonne. Toutefois, notez que la barre de titre affiche « Index – mon Application ASP.NET, » et le lien big en haut de la page indique « Nom de l’Application ». Selon la façon dont vous augmentez votre fenêtre de navigateur, vous devrez peut-être cliquer sur les trois barres dans le coin supérieur droit pour afficher l’à le **accueil**, **sur**, **Contact**, **Inscrire** et **connectez-vous** des liens.

## <a name="changing-views-and-layout-pages"></a>Modification des vues et des Pages de disposition

Tout d’abord, vous souhaitez modifier le &quot;nom de l’Application&quot; lien en haut de la page. Ce texte est courant de chaque page. Il est effectivement implémenté dans un seul endroit dans le projet, même si elles apparaissent sur chaque page dans l’application. Accédez à la */Views/Shared* dossier **l’Explorateur de solutions** et ouvrez le  *\_Layout.cshtml* fichier. Ce fichier s’appelle un *page de disposition* et se trouve dans le dossier partagé qui utilisent toutes les autres pages.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modèles de disposition permettent de spécifier la disposition du conteneur HTML de votre site au même endroit et de l’appliquer sur plusieurs pages de votre site. Recherchez la ligne `@RenderBody()`. `RenderBody` est un espace réservé dans lequel toutes les pages spécifiques aux vues que vous créez s’affichent, &quot;encapsulées&quot; dans la page de disposition. Par exemple, si vous sélectionnez le **sur** lien, le *Views\Home\About.cshtml* vue est restituée à l’intérieur de la `RenderBody` (méthode).

Changez le contenu de l’élément title. Modifier le [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) dans le modèle de disposition &quot;nom de l’Application&quot; à &quot;MVC Movie&quot; et le contrôleur `Home` à `Movies`. Le fichier de disposition complète est illustré ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Exécuter l’application et notez qu’il indique à présent &quot;MVC Movie &quot;. Cliquez sur le **sur** lien et voir comment cette page affiche &quot;MVC Movie&quot;, trop. Nous avons pu apporter la modification une fois dans le modèle de disposition et avoir toutes les pages sur le site de refléter le nouveau titre.

![](adding-a-view/_static/image8.png)

Lorsque nous avons créé le *Views\HelloWorld\Index.cshtml* fichier, elle contenait le code suivant :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Le code Razor ci-dessus consiste à définir explicitement la page de disposition. Examiner le *vues\\_ViewStart.cshtml* fichier, elle contient le même balisage Razor exact. Le *[vues\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* fichier définit la disposition commune pour toutes les vues utilisent, par conséquent, vous pouvez insérer un commentaire out ou supprimez ce code à partir de la *Views\HelloWorld\ Index.cshtml* fichier.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Vous pouvez utiliser la propriété `Layout` pour définir un mode de disposition différent ou lui affecter la valeur `null`. Aucun fichier de disposition n’est donc utilisé.

Maintenant, nous allons modifier le titre de la vue Index.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis, dans l’en-tête secondaire (le `<h2>` élément). Vous allez les modifier légèrement pour voir quel morceau du code modifie quelle partie de l’application.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Pour indiquer le titre HTML à afficher, le code ci-dessus affecte un `Title` propriété de la `ViewBag` objet (qui se trouve dans le *Index.cshtml* modèle de vue). Notez que le modèle de disposition ( *Views\Shared\\_Layout.cshtml* ) utilise cette valeur dans le `<title>` élément dans le cadre de la `<head>` section du contenu HTML que nous avons modifié précédemment.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

L’utilisation de cette `ViewBag` approche, vous pouvez facilement passer d’autres paramètres entre votre modèle de vue et de votre fichier de disposition.

Exécutez l'application. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.) Le titre du navigateur est créé avec le `ViewBag.Title` nous avons défini dans le *Index.cshtml* afficher le modèle et les autres &quot;-Movie App&quot; ajouté dans le fichier de disposition.

Notez également comment le contenu dans le *Index.cshtml* afficher le modèle a été fusionné avec la  *\_Layout.cshtml* afficher le modèle et une seule réponse HTML a été envoyée au navigateur. Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.

![](adding-a-view/_static/image9.png)

Notre petit peu de &quot;données&quot; (dans ce cas le &quot;Hello from our View Template !&quot; message) est codé en dur, cependant. L’application MVC a un &quot;V&quot; (vue) et vous avez un &quot;C&quot; (contrôleur), mais aucun &quot;M&quot; (model) encore. Peu de temps, nous allons étudier comment créer une base de données et de récupérer des données de modèle à partir de celui-ci.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant nous accédez à une base de données et que vous parler de modèles, cependant, nous allons tout d’abord parler de passer des informations à partir du contrôleur à une vue. Classes de contrôleur sont appelées en réponse à une demande d’URL entrante. Une classe de contrôleur est où vous écrivez le code qui gère le navigateur entrant demande, récupère les données à partir d’une base de données et finalement détermine le type de réponse à envoyer au navigateur. Vue modèles peuvent ensuite être utilisés à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.

Contrôleurs sont chargés de fournir les données ou les objets sont requis pour un modèle de vue restituer une réponse au navigateur. Une meilleure pratique : **Un modèle de vue ne doit jamais exécuter la logique métier ou interagir directement avec une base de données**. Au lieu de cela, un modèle de vue doit fonctionner uniquement avec les données qui sont qui lui sont fournies par le contrôleur. Leur gestion &quot;séparation des préoccupations&quot; vous aide à maintenir votre code concis, testable et plus facile à gérer.

Actuellement, le `Welcome` méthode d’action dans le `HelloWorldController` classe prend un `name` et un `numTimes` paramètre, puis sort les valeurs directement dans le navigateur. Au lieu que le contrôleur restitue cette réponse sous forme de chaîne, modifions-le pour utiliser un modèle de vue à la place. Le modèle de vue génère une réponse dynamique, ce qui signifie que vous devez passer les bits de données appropriés du contrôleur à la vue pour générer la réponse. Vous pouvez faire avec le contrôleur de placer les données dynamiques (paramètres) dont le modèle de vue a besoin dans un `ViewBag` objet auquel le modèle de vue peut ensuite accéder.

Retour à la *HelloWorldController.cs* du fichier et changer la `Welcome` méthode pour ajouter un `Message` et `NumTimes` valeur pour le `ViewBag` objet. `ViewBag` est un objet dynamique, ce qui signifie que vous pouvez placer tout ce que vous voulez le `ViewBag` objet ne possède aucune propriété définie jusqu'à ce que vous placez un élément qu’il contient. Le [système de liaison de modèle ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés (`name` et `numTimes`) à partir de la chaîne de requête dans la barre d’adresses aux paramètres de votre méthode. Le fichier *HelloWorldController.cs* complet ressemble à ceci :

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Maintenant le `ViewBag` objet contient des données qui seront passées à la vue automatiquement. Ensuite, vous avez besoin d’un modèle de vue Bienvenue ! Dans le **Build** menu, sélectionnez **générer la Solution** (ou Ctrl + Maj + B) pour vous assurer que le projet est compilé. Bouton droit sur le *Views\HelloWorld* dossier puis cliquez sur **ajouter**, puis cliquez sur **Page de vue MVC 5 avec disposition (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
Dans le **spécifier le nom de l’élément** boîte de dialogue, entrez *Bienvenue*, puis cliquez sur **OK**.   
  
Dans le **sélectionner une Page de disposition** boîte de dialogue, acceptez la valeur par défaut  **\_Layout.cshtml** et cliquez sur **OK**.  
  
![](adding-a-view/_static/image11.png)   

Le *MvcMovie\Views\HelloWorld\Welcome.cshtml* fichier est créé.

Remplacez le balisage dans le *Welcome.cshtml* fichier. Vous allez créer une boucle qui déclare &quot;Hello&quot; aussi souvent que l’utilisateur indique qu’il doit. L’ensemble *Welcome.cshtml* fichier est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Exécutez l’application et accédez à l’URL suivante :

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Maintenant les données sont extraites de l’URL et passées au contrôleur en utilisant le [binder de modèle](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Le contrôleur empaquette les données dans un `ViewBag` objet et passe cet objet à la vue. Puis, la vue affiche les données au format HTML à l’utilisateur.

![](adding-a-view/_static/image12.png)

Dans l’exemple ci-dessus, nous avons utilisé un `ViewBag` objet à passer des données à partir du contrôleur à une vue. Plus loin dans ce didacticiel, nous allons faire de même à l’aide d’un modèle de vue. L’approche de modèle de vue pour passer des données est généralement beaucoup préférée à l’approche de sac d’affichage. Consultez le billet de blog [V fortement typée des vues dynamiques](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) pour plus d’informations. 

Hé bien, ce qui était un type d’un &quot;M&quot; pour le modèle, mais pas le type de base de données. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
