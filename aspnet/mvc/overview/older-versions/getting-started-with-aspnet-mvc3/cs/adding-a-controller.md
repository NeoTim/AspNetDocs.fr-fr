---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Ajout d’un contrôleur (c#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui, je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 18c8f2a95222a28d95950b34c816539f723542f2
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396484"
---
# <a name="adding-a-controller-c"></a>Ajout d’un contrôleur (C#)

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique. [Téléchargez la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.


MVC est l’acronyme *model-view-controller*. MVC est un modèle de développement d’applications sont bien structurée et facile à gérer. Applications basées sur MVC contiennent :

- Contrôleurs : Classes qui gèrent les demandes entrantes vers l’application, récupérer des données de modèle, puis spécifiez les modèles de vue qui retournent une réponse au client.
- Modèles : Classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.
- Affichage : Fichiers de modèles que votre application utilise pour générer dynamiquement des réponses HTML.

Nous allons être couvrant tous ces concepts dans cette série de didacticiels et vous montrer comment les utiliser pour créer une application.

Nous allons commencer en créant une classe de contrôleur. Dans **l’Explorateur de solutions**, cliquez sur le *contrôleurs* dossier, puis sélectionnez **ajouter un contrôleur**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nommez votre contrôleur « HelloWorldController ». Laissez le modèle par défaut en tant que **contrôleur vide** et cliquez sur **ajouter**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Notez que dans **l’Explorateur de solutions** un nouveau fichier a été créé nommé *HelloWorldController.cs*. Le fichier est ouvert dans l’IDE.

![](adding-a-controller/_static/image5.png)

À l’intérieur de la `public class HelloWorldController` bloc, créez deux méthodes qui ressemblent le code suivant. Le contrôleur retourne une chaîne de code HTML par exemple.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Votre contrôleur est nommé `HelloWorldController` et la première méthode ci-dessus est nommée `Index`. Nous allons l’appeler à partir d’un navigateur. Exécutez l’application (appuyez sur F5 ou Ctrl + F5). Dans le navigateur, ajoutez « HelloWorld » pour le chemin d’accès dans la barre d’adresses. (Par exemple, dans l’illustration ci-dessous, ses `http://localhost:43246/HelloWorld.`) la page dans le navigateur ressemblera à la capture d’écran suivante. Dans la méthode ci-dessus, le code a renvoyé une chaîne directement. Vous avez demandé le système pour retourner uniquement des codes HTML, et c’était le cas !

![](adding-a-controller/_static/image6.png)

ASP.NET MVC appelle les classes de l’autre contrôleur (et différentes méthodes d’action au sein de celles-ci) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format comme celui-ci pour déterminer quel code pour appeler :

`/[Controller]/[ActionName]/[Parameters]`

La première partie de l’URL détermine la classe de contrôleur à exécuter. Par conséquent, */HelloWorld* mappe à la `HelloWorldController` classe. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/Index* provoquerait la `Index` méthode de la `HelloWorldController` classe à exécuter. Notez que nous n’avions pour accéder à */HelloWorld* et `Index` méthode a été utilisée par défaut. Il s’agit, car une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur s’il n’est pas explicitement spécifié.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne « This is the Welcome action method... ». Le mappage de MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![](adding-a-controller/_static/image7.png)

Nous allons modifier légèrement l’exemple afin que vous pouvez passer des informations de paramètre à partir de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? nom = Scott&amp;numtimes = 4*). Modifier votre `Welcome` méthode pour inclure les deux paramètres comme indiqué ci-dessous. Notez que le code utilise la fonctionnalité de paramètre facultatif de c# pour indiquer que le `numTimes` paramètre par défaut 1 si aucune valeur n’est passée pour ce paramètre.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le système mappe automatiquement les paramètres nommés à partir de la chaîne de requête dans la barre d’adresses aux paramètres de votre méthode.

![](adding-a-controller/_static/image8.png)

Dans les deux exemples de ces le contrôleur a fait la partie « VC » de MVC, autrement dit, le travail de la vue et contrôleur. Le contrôleur retourne directement du HTML. En règle générale, vous ne voulez contrôleurs retournent directement, du HTML dans la mesure où cela devient très difficile de revenir en code. Au lieu de cela, nous allons utiliser généralement un fichier de modèle de vue séparé afin de générer la réponse HTML. Penchons-nous maintenant à la façon dont nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](intro-to-aspnet-mvc-3.md)
> [Suivant](adding-a-view.md)
