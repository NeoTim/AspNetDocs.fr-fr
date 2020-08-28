---
title: Ajout d’une vue à une application MVC
author: Rick-Anderson
description: Ajout d’une vue à une application MVC
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: b8400036cc689d3cd2fec54b3191252d296ade41
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045011"
---
# <a name="adding-a-view"></a>Ajout d’une vue

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Dans cette section, vous allez modifier la `HelloWorldController` classe pour utiliser les fichiers de modèle de vue afin d’encapsuler correctement le processus de génération de réponses html sur un client. 

Vous allez créer un fichier de modèle de vue à l’aide du [moteur de vue Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Les modèles de vue Razor ont une extension de fichier *. cshtml* et offrent un moyen élégant de créer une sortie HTML à l’aide de C#. Razor réduit le nombre de caractères et de frappes de touches requis lors de l’écriture d’un modèle de vue, et permet un flux de travail de codage fluide rapide.

Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Modifiez la `Index` méthode pour appeler la méthode de [vue](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) contrôleurs, comme illustré dans le code suivant :

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

La `Index` méthode ci-dessus utilise un modèle de vue pour générer une réponse HTML au navigateur. Les méthodes de contrôleur (également appelées [méthodes d’action](http://rachelappel.com/asp.net-mvc-actionresults-explained)), telles que la `Index` méthode ci-dessus, retournent généralement un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou une classe dérivée de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), et non des types primitifs tels que String.

Cliquez avec le bouton droit sur le dossier *Views\HelloWorld* , cliquez sur **Ajouter**, puis sur **page de vue MVC 5 avec disposition (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
Dans la boîte de dialogue **spécifier un nom pour l’élément** , entrez *index*, puis cliquez sur **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Dans la boîte de dialogue **Sélectionner une page de disposition** , acceptez le format par défaut ** \_ . cshtml** , puis cliquez sur **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Dans la boîte de dialogue ci-dessus, le dossier *Views\Shared* est sélectionné dans le volet gauche. Si vous avez un fichier de disposition personnalisé dans un autre dossier, vous pouvez le sélectionner. Nous parlerons du fichier de disposition plus loin dans ce didacticiel.

Le fichier *MvcMovie\Views\HelloWorld\Index.cshtml* est créé.

![](adding-a-view/_static/image4.png)

Ajoutez le balisage en surbrillance suivant.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Cliquez avec le bouton droit sur le fichier *index. cshtml* , puis sélectionnez **afficher dans le navigateur**.

![PI](adding-a-view/_static/image5.png)

Vous pouvez également cliquer avec le bouton droit sur le fichier *index. cshtml* et sélectionner **afficher dans inspecteur de page.** Pour plus d’informations, consultez le [didacticiel inspecteur de page](../../views/using-page-inspector-in-aspnet-mvc.md) .

Vous pouvez également exécuter l’application et accéder au `HelloWorld` contrôleur ( `http://localhost:xxxx/HelloWorld` ). La `Index` méthode dans votre contrôleur n’a pas fait de nombreuses tâches. elle a simplement exécuté l’instruction `return View()` , qui spécifie que la méthode doit utiliser un fichier de modèle de vue pour afficher une réponse dans le navigateur. Étant donné que vous n’avez pas spécifié explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC utilise par défaut le fichier de vue *index. cshtml* dans le dossier *\Views\HelloWorld* L’image ci-dessous montre la chaîne &quot; Hello de notre modèle de vue ! &quot; codé en dur dans la vue.

![](adding-a-view/_static/image6.png)

Semble assez bon. Toutefois, Notez que la barre de titre du navigateur affiche « index-My ASP.NET application » et que le lien Big situé en haut de la page indique « nom de l’application ». Selon la taille de votre fenêtre de navigateur, vous devrez peut-être cliquer sur les trois barres situées en haut à droite pour afficher les liens vers la **page d’hébergement**, à **propos**de, **contacter**, **inscrire** et **ouvrir une session** .

## <a name="changing-views-and-layout-pages"></a>Modification des vues et des pages de disposition

Tout d’abord, vous souhaitez modifier le &quot; lien du nom de l’application &quot; en haut de la page. Ce texte est commun à chaque page. Elle est implémentée dans un seul emplacement du projet, même si elle apparaît sur chaque page de l’application. Accédez au dossier */Views/Shared* dans **Explorateur de solutions** et ouvrez le fichier * \_ Layout. cshtml* . Ce fichier s’appelle une *page de disposition* et se trouve dans le dossier partagé que toutes les autres pages utilisent.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Les modèles de disposition vous permettent de spécifier la disposition du conteneur HTML de votre site dans un emplacement unique, puis de l’appliquer sur plusieurs pages de votre site. Recherchez la ligne `@RenderBody()`. `RenderBody` est un espace réservé dans lequel toutes les pages spécifiques aux vues que vous créez s’affichent, &quot;encapsulées&quot; dans la page de disposition. Par exemple, si vous sélectionnez le lien **à propos** , la vue *Views\Home\About.cshtml* est rendue à l’intérieur de la `RenderBody` méthode.

Changez le contenu de l’élément title. Remplacez le [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) dans le modèle de mise en page du nom de l' &quot; application &quot; à &quot; la vidéo MVC &quot; et le contrôleur de `Home` par `Movies` . Le fichier de disposition complet est illustré ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Exécutez l’application et Notez qu’elle indique à présent le &quot; film MVC &quot; . Cliquez sur le lien **à propos** de pour voir comment cette page affiche &quot; également le film MVC &quot; . Nous avons été en mesure d’apporter la modification une fois dans le modèle de disposition et de faire en sorte que toutes les pages du site reflètent le nouveau titre.

![](adding-a-view/_static/image8.png)

Lorsque nous avons créé le fichier *Views\HelloWorld\Index.cshtml* , il contenait le code suivant :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Le code Razor ci-dessus définit explicitement la page de disposition. Examinez les *vues \\ _ViewStart fichier. cshtml* , elle contient exactement le même balisage Razor. Le fichier *[Views \\ _ViewStart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* définit la disposition commune que toutes les vues utiliseront. vous pouvez donc commenter ou supprimer ce code du fichier *Views\HelloWorld\Index.cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Vous pouvez utiliser la propriété `Layout` pour définir un mode de disposition différent ou lui affecter la valeur `null`. Aucun fichier de disposition n’est donc utilisé.

À présent, nous allons modifier le titre de la vue index.

Ouvrez *MvcMovie\Views\HelloWorld\Index.cshtml*. Il y a deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis dans l’en-tête secondaire (l' `<h2>` élément). Vous allez les modifier légèrement pour voir quel morceau du code modifie quelle partie de l’application.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Pour indiquer le titre HTML à afficher, le code ci-dessus définit une `Title` propriété de l' `ViewBag` objet (qui se trouve dans le modèle de vue *index. cshtml* ). Notez que le modèle de disposition ( *Views\Shared \\ _Layout. cshtml* ) utilise cette valeur dans l’élément dans le `<title>` cadre de la `<head>` section du code HTML que nous avons modifiée précédemment.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

À l’aide de cette `ViewBag` approche, vous pouvez facilement passer d’autres paramètres entre votre modèle de vue et votre fichier de disposition.

Exécutez l'application. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur CTRL + F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.) Le titre du navigateur est créé avec le `ViewBag.Title` défini dans le modèle de vue *index. cshtml* et l' &quot; application de vidéo supplémentaire &quot; ajoutée au fichier de disposition.

Notez également que le contenu du modèle de vue *index. cshtml* a été fusionné avec le modèle de vue * \_ Layout. cshtml* et qu’une réponse html unique a été envoyée au navigateur. Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.

![](adding-a-view/_static/image9.png)

Notre petite quantité de &quot; données &quot; (dans ce cas &quot; , le Bonjour de notre modèle de vue ! &quot; message) est cependant codée en dur. L’application MVC a une &quot; V &quot; (vue) et vous disposez d’un &quot; C &quot; (contrôleur), mais pas &quot; encore de M &quot; (Model). Nous allons bientôt vous guider dans la création d’une base de données et l’extraction des données du modèle.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant de passer à une base de données et de parler des modèles, nous allons tout d’abord parler de la transmission des informations du contrôleur à une vue. Les classes de contrôleur sont appelées en réponse à une demande d’URL entrante. Une classe de contrôleur est l’endroit où vous écrivez le code qui gère les demandes de navigateur entrantes, récupère les données d’une base de données et décide finalement le type de réponse à renvoyer au navigateur. Les modèles de vue peuvent ensuite être utilisés à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.

Les contrôleurs sont chargés de fournir les données ou objets requis pour qu’un modèle de vue affiche une réponse au navigateur. Bonne pratique : **un modèle de vue ne doit jamais exécuter de logique métier ni interagir directement avec une base de données**. Au lieu de cela, un modèle de vue doit fonctionner uniquement avec les données qui lui sont fournies par le contrôleur. Le maintien &quot; de cette séparation des préoccupations &quot; vous permet de nettoyer votre code, de le rendre testable et le plus facile à gérer.

Actuellement, la `Welcome` méthode d’action de la `HelloWorldController` classe prend un `name` et un paramètre, puis `numTimes` génère les valeurs directement dans le navigateur. Plutôt que de faire en sorte que le contrôleur rende cette réponse sous forme de chaîne, nous allons modifier le contrôleur pour qu’il utilise un modèle de vue à la place. Le modèle de vue génère une réponse dynamique, ce qui signifie que vous devez passer les bits de données appropriés du contrôleur à la vue pour générer la réponse. Pour ce faire, vous devez faire en sorte que le contrôleur place les données dynamiques (paramètres) dont le modèle de vue a besoin dans un `ViewBag` objet auquel le modèle de vue peut accéder.

Revenez au fichier *HelloWorldController.cs* et modifiez la `Welcome` méthode pour ajouter une `Message` `NumTimes` valeur et à l' `ViewBag` objet. `ViewBag` est un objet dynamique, ce qui signifie que vous pouvez y placer tout ce que vous souhaitez. l' `ViewBag` objet n’a pas de propriétés définies tant que vous ne placez pas de contenu dans celui-ci. Le [système de liaison de modèle MVC ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés ( `name` et `numTimes` ) de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode. Le fichier *HelloWorldController.cs* complet ressemble à ceci :

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

À présent, l' `ViewBag` objet contient des données qui seront transmises automatiquement à la vue. Ensuite, vous avez besoin d’un modèle de vue de bienvenue ! Dans le menu **générer** , sélectionnez **générer la solution** (ou appuyez sur Ctrl + Maj + B) pour vous assurer que le projet est compilé. Cliquez avec le bouton droit sur le dossier *Views\HelloWorld* , cliquez sur **Ajouter**, puis sur **page de vue MVC 5 avec disposition (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
Dans la boîte de dialogue **spécifier le nom de l’élément** , entrez *Welcome*, puis cliquez sur **OK**.   
  
Dans la boîte de dialogue **Sélectionner une page de disposition** , acceptez le format par défaut ** \_ . cshtml** , puis cliquez sur **OK**.  
  
![](adding-a-view/_static/image11.png)   

Le fichier *MvcMovie\Views\HelloWorld\Welcome.cshtml* est créé.

Remplacez le balisage dans le fichier *Welcome. cshtml* . Vous allez créer une boucle qui indique &quot; Hello autant de &quot; fois que l’utilisateur l’indique. Le fichier *Welcome welcome. cshtml* est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Exécutez l’application et accédez à l’URL suivante :

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

À présent, les données sont extraites de l’URL et transmises au contrôleur à l’aide du [classeur de modèles](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Le contrôleur empaquette les données dans un `ViewBag` objet et transmet cet objet à la vue. La vue affiche ensuite les données au format HTML pour l’utilisateur.

![](adding-a-view/_static/image12.png)

Dans l’exemple ci-dessus, nous avons utilisé un `ViewBag` objet pour passer des données du contrôleur à une vue. Plus loin dans ce didacticiel, nous allons faire de même à l’aide d’un modèle de vue. L’approche du modèle de vue pour passer des données est généralement bien préférée à l’approche de la vue de conteneur. Pour plus d’informations, consultez l’entrée de blog des [vues fortement typées V dynamiques](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) . 

Eh bien, il s’agissait d’un type de &quot; M &quot; pour le modèle, mais pas du type de base de données. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md) 
>  [Suivant](adding-a-model.md)
