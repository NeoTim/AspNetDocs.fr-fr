---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Utilisation de ASP.NET MVC avec différentes versions de l’IIS (VB) Microsoft Docs
author: rick-anderson
description: Dans ce tutoriel, vous apprenez à utiliser ASP.NET MVC, et URL Routing, avec différentes versions des services d’information Internet. Vous apprenez différentes stratégies...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e04ae14026e6d5dd1e603be6c52ff6876a468cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541999"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Utilisation d’ASP.NET MVC avec différentes versions d’IIS (VB)

par [Microsoft](https://github.com/microsoft)

> Dans ce tutoriel, vous apprenez à utiliser ASP.NET MVC, et URL Routing, avec différentes versions des services d’information Internet. Vous apprenez différentes stratégies pour utiliser ASP.NET MVC avec IIS 7.0 (mode classique), IIS 6.0, et les versions antérieures de l’IIS.

Le cadre MVC ASP.NET dépend de ASP.NET Routing pour acheminer les demandes de navigateur pour les actions de contrôleur. Afin de profiter de ASP.NET Routing, vous devrez peut-être effectuer des étapes de configuration supplémentaires sur votre serveur web. Tout dépend de la version des Services d’information Internet (IIS) et du mode de traitement des demandes pour votre demande.

Voici un résumé des différentes versions de l’IIS :

- IIS 7.0 (mode intégré) - Aucune configuration spéciale nécessaire à l’utilisation de ASP.NET Routing.
- IIS 7.0 (mode classique) - Vous devez effectuer une configuration spéciale pour utiliser ASP.NET Routing.
- IIS 6.0 ou moins - Vous devez effectuer une configuration spéciale pour utiliser ASP.NET Routing.

La dernière version de l’IIS est la version 7.5 (sur Win7). IIS 7 de l’IIS est inclus avec Windows Server 2008 ET VISTA/SP1 et plus. Vous pouvez également installer IIS 7.0 sur n’importe quelle [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)version du système d’exploitation Vista à l’exception de Home Basic (voir ).

L’IIS 7.0 prend en charge deux modes de traitement des demandes. Vous pouvez utiliser le mode intégré ou le mode classique. Vous n’avez pas besoin d’effectuer des étapes de configuration spéciales lors de l’utilisation de l’IIS 7.0 en mode intégré. Cependant, vous devez effectuer une configuration supplémentaire lors de l’utilisation de l’IIS 7.0 en mode classique.

Microsoft Windows Server 2003 comprend IIS 6.0. Vous ne pouvez pas mettre à niveau IIS 6.0 à IIS 7.0 lors de l’utilisation du système d’exploitation Windows Server 2003. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation de l’IIS 6.0.

Microsoft Windows XP Professional comprend IIS 5.1. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation de l’IIS 5.1.

Enfin, Microsoft Windows 2000 et Microsoft Windows 2000 Professional comprend IIS 5.0. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation de l’IIS 5.0.

## <a name="integrated-versus-classic-mode"></a>Mode intégré par rapport au mode classique

L’IIS 7.0 peut traiter les demandes à l’aide de deux modes de traitement des demandes différents : intégrés et classiques. Le mode intégré offre de meilleures performances et plus de fonctionnalités. Le mode classique est inclus pour la compatibilité vers l’arrière avec les versions antérieures de l’IIS.

Le mode de traitement des demandes est déterminé par le pool d’applications. Vous pouvez déterminer quel mode de traitement est utilisé par une application Web particulière en déterminant le pool d’applications associé à l’application. Procédez comme suit :

1. Lancer le gestionnaire des services d’information sur Internet
2. Dans la fenêtre Connexions, sélectionnez une application
3. Dans la fenêtre Actions, cliquez sur le lien **Paramètres de base** pour ouvrir la boîte de dialogue Edit Application (voir figure 1)
4. Prenez note du pool d’applications sélectionné.

Par défaut, IIS est configuré pour prendre en charge deux pools d’applications : **DefaultAppPool** et **Classic .NET AppPool**. Si DefaultAppPool est sélectionné, votre application s’exécute en mode traitement des demandes intégrée. Si Classic .NET AppPool est sélectionné, votre application est en cours d’exécution en mode de traitement des demandes classique.

[![Boîte de dialogue New Project](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figure 1**: Détection du mode de traitement des demandes[(Cliquez pour voir l’image grandeur nature](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))

Notez que vous pouvez modifier le mode de traitement des demandes dans la boîte de dialogue Edit Application. Cliquez sur le bouton Sélectionner et modifiez le pool d’applications associé à l’application. Réalisez qu’il y a des problèmes de compatibilité lors de la modification d’une application ASP.NET du mode classique au mode intégré. Pour plus d’informations, consultez les articles suivants :

- Mise à niveau ASP.NET 1.1 à IIS 7.0 sur Windows Vista et Windows Server 2008 --[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- ASP.NET intégration avec l’IIS 7.0 -[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Si une application ASP.NET utilise le DefaultAppPool, alors vous n’avez pas besoin d’effectuer des étapes supplémentaires pour obtenir ASP.NET Routing (et donc ASP.NET MVC) pour fonctionner. Toutefois, si l’application ASP.NET est configurée pour utiliser l’AppPool .NET classique, puis continuer à lire, vous avez plus de travail à faire.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Utilisation de ASP.NET MVC avec des versions anciennes de l’IIS

Si vous avez besoin d’utiliser ASP.NET MVC avec une version plus ancienne de l’IIS que l’IIS 7.0, ou si vous devez utiliser IIS 7.0 en mode classique, alors vous avez deux options. Tout d’abord, vous pouvez modifier le tableau d’itinéraire pour utiliser les extensions de fichiers. Par exemple, au lieu de demander une URL comme /Store/Details, vous demanderiez une URL comme /Store.aspx/Détails.

La deuxième option est de créer quelque chose appelé une *carte de script wildcard*. Une carte de script wildcard vous permet de cartographier chaque demande dans le cadre ASP.NET.

Si vous n’avez pas accès à votre serveur Web (par exemple, votre ASP.NET application MVC est hébergée par un fournisseur de services Internet), alors vous devrez utiliser la première option. Si vous ne souhaitez pas modifier l’apparence de vos URL et que vous avez accès à votre serveur Web, vous pouvez utiliser la deuxième option.

Nous explorons chaque option en détail dans les sections suivantes.

## <a name="adding-extensions-to-the-route-table"></a>Ajout d’extensions à la table de route

La façon la plus simple d’obtenir ASP.NET Routing pour travailler avec les anciennes versions d’IIS est de modifier votre tableau d’itinéraire dans le fichier Global.asax. Le fichier Global.asax par défaut et non modifié dans la liste 1 configure une route nommée l’itinéraire par défaut.

**Liste 1 - Global.asax (non modifié)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

L’itinéraire par défaut configuré dans la liste 1 vous permet d’acheminer les URL qui ressemblent à ceci :

/Accueil/Index

/Produit/Détails/3

/Produit

Malheureusement, les anciennes versions de l’IIS ne transmettront pas ces demandes au cadre ASP.NET. Par conséquent, ces demandes ne seront pas acheminées vers un contrôleur. Par exemple, si vous faites une demande de navigateur pour l’URL /Home/Index, vous obtiendrez la page d’erreur dans la figure 2.

[![Boîte de dialogue New Project](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figure 2**: Recevoir une erreur 404 Non trouvée ([Cliquez pour voir l’image grandeur nature](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

Les anciennes versions de l’IIS ne cartographient que certaines demandes au cadre ASP.NET. La demande doit être pour une URL avec la bonne extension de fichier. Par exemple, une demande pour /SomePage.aspx est cartographiée sur le cadre ASP.NET. Cependant, une demande pour /SomePage.htm ne le fait pas.

Par conséquent, pour obtenir ASP.NET Routing au travail, nous devons modifier l’itinéraire par défaut afin qu’il comprend une extension de fichiers qui est cartographié sur le cadre ASP.NET.

Ceci est fait à `registermvc.wsf`l’aide d’un script nommé . Il a été inclus avec le ASP.NET `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`MVC 1 version en , mais à partir de ASP.NET 2 [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)ce script a été déplacé à l’avenir ASP.NET, disponible à .

L’exécution de ce script enregistre une nouvelle extension .mvc avec IIS. Après avoir enregistré l’extension .mvc, vous pouvez modifier vos itinéraires dans le fichier Global.asax afin que les itinéraires utilisent l’extension .mvc.

Le fichier Global.asax modifié dans Listing 2 fonctionne avec des versions plus anciennes de l’IIS.

**Liste 2 - Global.asax (modifié avec des extensions)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Important : n’oubliez pas de construire votre ASP.NET application MVC à nouveau après avoir modifié le fichier Global.asax.

Il y a deux changements importants au fichier Global.asax dans la liste 2. Il y a maintenant deux itinéraires définis dans le Global.asax. Le modèle URL pour l’itinéraire par défaut, le premier itinéraire, ressemble maintenant à:

'contrôleur'.mvc/'action'/'id'

L’ajout de l’extension .mvc modifie le type de fichiers interceptés par le module ASP.NET Routing. Avec ce changement, l’application ASP.NET MVC achemine désormais les demandes comme suit :

/Home.mvc/Index/

/Product.mvc/Détails/3

/Product.mvc/

La deuxième route, la route Racine, est nouvelle. Ce modèle d’URL pour l’itinéraire Racine est une chaîne vide. Cet itinéraire est nécessaire pour faire correspondre les demandes faites contre la racine de votre application. Par exemple, l’itinéraire Root correspondra à une demande qui ressemble à ceci :

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Après avoir apporté ces modifications à votre tableau d’itinéraire, vous devrez vous assurer que tous les liens de votre application sont compatibles avec ces nouveaux modèles d’URL. En d’autres termes, assurez-vous que tous vos liens incluent l’extension .mvc. Si vous utilisez la méthode d’assistance Html.ActionLink() pour générer vos liens, alors vous ne devriez pas avoir besoin d’apporter des modifications.

Au lieu d’utiliser le script registermvc.wcf, vous pouvez ajouter une nouvelle extension à l’IIS qui est cartographiée sur le cadre ASP.NET à la main. Lors de l’ajout d’une nouvelle extension vous-même, assurez-vous que la case à cocher étiqueté vérifier **que le fichier existe** n’est pas vérifié.

## <a name="hosted-server"></a>Serveur hébergé

Vous n’avez pas toujours accès à votre serveur web. Par exemple, si vous hébergez votre application ASP.NET MVC à l’aide d’un fournisseur d’hébergement Internet, vous n’aurez pas nécessairement accès à l’IIS.

Dans ce cas, vous devez utiliser l’une des extensions de fichiers existantes qui sont cartographiées dans le cadre ASP.NET. Des exemples d’extensions de fichiers cartographiées pour ASP.NET comprennent les extensions .aspx, .axd et .ashx.

Par exemple, le fichier Global.asax modifié dans La liste 3 utilise l’extension .aspx au lieu de l’extension .mvc.

**Liste 3 - Global.asax (modifié avec des extensions .aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Le fichier Global.asax dans Listing 3 est exactement le même que le fichier Global.asax précédent, sauf pour le fait qu’il utilise l’extension .aspx au lieu de l’extension .mvc. Vous n’avez pas à effectuer de configuration sur votre serveur web distant pour utiliser l’extension .aspx.

## <a name="creating-a-wildcard-script-map"></a>Création d’une carte de script Wildcard

Si vous ne souhaitez pas modifier les URL pour votre application ASP.NET MVC, et que vous avez accès à votre serveur Web, alors vous avez une option supplémentaire. Vous pouvez créer une carte de script wildcard qui cartographie toutes les demandes au serveur web au cadre ASP.NET. De cette façon, vous pouvez utiliser la table d’itinéraire MVC par défaut ASP.NET avec IIS 7.0 (en mode classique) ou IIS 6.0.

Sachez que cette option provoque l’IIS à intercepter chaque demande faite contre le serveur web. Cela comprend les demandes d’images, les pages ASP classiques et les pages HTML. Par conséquent, permettre à une carte de script wildcard de ASP.NET a des implications de performance.

Voici comment vous activez une carte de script wildcard pour IIS 7.0 :

1. Sélectionnez votre application dans la fenêtre Connexions
2. Assurez-vous que la vue **Des fonctionnalités** est sélectionnée
3. Double clic sur le bouton **Handler Mappings**
4. Cliquez sur le lien **Add Wildcard Script Map** (voir figure 3)
5. Entrez le chemin vers\_le fichier aspnet isapi.dll (Vous pouvez copier ce chemin à partir de la carte de script PageHandlerFactory)
6. Entrez le nom MVC
7. Cliquez sur le bouton **OK**

[![Boîte de dialogue New Project](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figure 3**: Création d’une carte de script wildcard avec IIS 7.0 ([Cliquez pour voir l’image grandeur nature](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))

Suivez ces étapes pour créer une carte de script wildcard avec IIS 6.0 :

1. Cliquez à droite sur un site Web et sélectionnez propriétés
2. Sélectionnez l’onglet **Home Directory**
3. Cliquez sur le bouton **Configuration**
4. Sélectionnez l’onglet **Cartographies**
5. Cliquez sur le bouton **Insert** (voir figure 4)
6. Coller le chemin vers l’aspnet\_isapi.dll dans le champ Exécutable (vous pouvez copier ce chemin à partir de la carte de script pour les fichiers .aspx)
7. Décocher la case à cocher étiqueté **Vérifier que le fichier existe**
8. Cliquez sur le bouton **OK**

[![Boîte de dialogue New Project](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figure 4**: Création d’une carte de script wildcard avec IIS 6.0 ([Cliquez pour voir l’image grandeur nature](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))

Après avoir permis des cartes de script wildcard, vous devez modifier le tableau d’itinéraire dans le fichier Global.asax afin qu’il inclut une route Racine. Sinon, vous obtiendrez la page d’erreur dans la figure 5 lorsque vous faites une demande pour la page racine de votre application. Vous pouvez utiliser le fichier Global.asax modifié dans la liste 4.

[![Boîte de dialogue New Project](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figure 5**: Erreur d’itinéraire Racine manquante ([Cliquez pour voir l’image grandeur nature](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))

**Liste 4 - Global.asax (modifié avec root route)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Après avoir permis une carte de script wildcard pour IIS 7.0 ou IIS 6.0, vous pouvez faire des demandes qui fonctionnent avec la table d’itinéraire par défaut qui ressemble à ceci:

/

/Accueil/Index

/Produit/Détails/3

/Produit

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était d’expliquer comment vous pouvez utiliser ASP.NET MVC lors de l’utilisation d’une version plus ancienne de l’IIS (ou IIS 7.0 en mode classique). Nous avons discuté de deux méthodes pour obtenir ASP.NET Routing de travailler avec les anciennes versions de l’IIS: Modifier la table d’itinéraire par défaut ou de créer une carte de script wildcard.

La première option vous oblige à modifier les URL utilisées dans votre application ASP.NET MVC. Un avantage très important de cette première option est que vous n’avez pas besoin d’accéder à un serveur web afin de modifier le tableau d’itinéraire. Cela signifie que vous pouvez utiliser cette première option, même lors de l’hébergement de votre application ASP.NET MVC avec une société d’hébergement Internet.

La deuxième option est de créer une carte de script wildcard. L’avantage de cette deuxième option est que vous n’avez pas besoin de modifier vos URL. L’inconvénient de cette deuxième option est qu’elle peut avoir une incidence sur les performances de votre application ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
