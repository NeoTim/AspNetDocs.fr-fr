---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Création de Pages d’aide pour l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042526"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Création de Pages d’aide pour l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Lorsque vous créez une API web, il est souvent utile de créer une page d’aide, afin que les autres développeurs sache comment appeler votre API. Vous pouvez créer manuellement toute la documentation, mais il est préférable de créer automatiquement autant que possible.

Pour faciliter cette tâche, l’API Web ASP.NET fournit une bibliothèque pour générer automatiquement des pages d’aide en cours d’exécution.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Création de Pages d’aide de l’API

Installer [ASP.NET et Web Tools 2012.2 mise à jour](https://go.microsoft.com/fwlink/?LinkId=282650). Cette mise à jour intègre des pages d’aide dans le modèle de projet API Web.

Ensuite, créez un projet ASP.NET MVC 4 et sélectionnez le modèle de projet API Web. Le modèle de projet crée un contrôleur de l’exemple d’API nommé `ValuesController`. Le modèle crée également les pages d’aide API. Tous les fichiers de code pour la page d’aide sont placés dans le dossier des zones du projet.

![](creating-api-help-pages/_static/image2.png)

Lorsque vous exécutez l’application, la page d’accueil contient un lien vers la page d’aide API. À partir de la page d’accueil, le chemin d’accès relatif est /Help.

![](creating-api-help-pages/_static/image3.png)

Ce lien vous dirige vers une page de résumé des API.

![](creating-api-help-pages/_static/image4.png)

La vue MVC pour cette page est définie dans Areas/HelpPage/Views/Help/Index.cshtml. Vous pouvez modifier cette page pour modifier la mise en page introduction, titre, styles et ainsi de suite.

La partie principale de la page est une table d’API, regroupées par contrôleur. Les entrées de table sont générées dynamiquement, à l’aide de la **IApiExplorer** interface. (Je reviendrai sur cette interface ultérieurement.) Si vous ajoutez un nouveau contrôleur d’API, la table est automatiquement mis à jour en cours d’exécution.

La colonne « API » répertorie la méthode HTTP et un URI relatif. La colonne « Description » contient la documentation pour chaque API. Initialement, la documentation est simplement texte d’espace réservé. Dans la section suivante, je vous montrerai comment ajouter la documentation à partir de commentaires XML.

Chaque API comporte un lien vers une page avec des informations plus détaillées, y compris le corps de demande et de réponse d’exemple.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Ajout de Pages d’aide à un projet existant

Vous pouvez ajouter des pages d’aide à un projet d’API Web existant à l’aide du Gestionnaire de Package NuGet. Cette option est utile de que commencer à partir d’un autre modèle de projet que le modèle « API Web ».

À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**. Dans le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) fenêtre, tapez les commandes suivantes :

Pour un **c#** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Pour un **Visual Basic** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Il existe deux packages, pour c# et l’autre pour Visual Basic. Veillez à utiliser celui qui correspond à votre projet.

Cette commande installe les assemblys nécessaires et ajoute les vues MVC pour les pages d’aide (situés dans le dossier zones/HelpPage). Vous devez manuellement ajouter un lien vers la page d’aide. L’URI est /Help. Pour créer un lien dans une vue razor, ajoutez ce qui suit :

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

En outre, assurez-vous qu’inscrire les zones. Dans le fichier Global.asax, ajoutez le code suivant à la **Application\_Démarrer** méthode, s’il y ne figure pas déjà :

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Ajout de Documentation de l’API

Par défaut, l’aide de pages contiennent des chaînes d’espace réservé pour la documentation. Vous pouvez utiliser [commentaires de documentation XML](https://msdn.microsoft.com/library/b2s063f7.aspx) pour créer la documentation. Pour activer cette fonctionnalité, ouvrez le fichier de zones/HelpPage/application\_Start/HelpPageConfig.cs et supprimez les commentaires de la ligne suivante :

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Activez maintenant la documentation XML. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**. Sélectionnez le **Build** page.

![](creating-api-help-pages/_static/image6.png)

Sous **sortie**, vérifiez **fichier de documentation XML**. Dans la zone d’édition, tapez « application\_Data/XmlDocument.xml ».

![](creating-api-help-pages/_static/image7.png)

Ensuite, ouvrez le code pour le `ValuesController` contrôleur d’API, qui est défini dans /Controllers/ValuesControler.cs. Ajouter des commentaires de documentation pour les méthodes de contrôleur. Exemple :

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Conseil : Si vous positionnez le signe insertion sur la ligne au-dessus de la méthode et que vous tapez les trois barres obliques, Visual Studio insère automatiquement les éléments XML. Vous pouvez ensuite renseigner les champs vides.


Maintenant générer et exécuter à nouveau l’application et naviguer vers les pages d’aide. Les chaînes de documentation doivent apparaître dans la table API.

![](creating-api-help-pages/_static/image8.png)

La page d’aide lit les chaînes à partir du fichier XML en cours d’exécution. (Lorsque vous déployez l’application, veillez à déployer le fichier XML.)

## <a name="under-the-hood"></a>Sous le capot

Les pages d’aide sont appuient sur le **ApiExplorer** (classe), qui fait partie de l’infrastructure API Web. Le **ApiExplorer** classe fournit la matière première pour la création d’une page d’aide. Pour chaque API, **ApiExplorer** contient un **ApiDescription** qui décrit l’API. Pour cela, une « API » est définie comme la combinaison de méthodes HTTP et l’URI relatif. Par exemple, Voici certaines API distinctes :

- OBTENIR /api/Products
- OBTENIR/API/produits / {id}
- VALIDER les produits/api /

Si une action de contrôleur prend en charge plusieurs méthodes HTTP, le **ApiExplorer** traite chaque méthode comme une API distincte.

Pour masquer une API à partir de la **ApiExplorer**, ajouter le **ApiExplorerSettings** à l’action et un ensemble d’attributs *IgnoreApi* sur true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Vous pouvez également ajouter cet attribut au contrôleur, pour exclure l’ensemble du contrôleur.

La classe ApiExplorer Obtient les chaînes de documentation à partir de la **IDocumentationProvider** interface. Comme vous l’avez vu précédemment, la bibliothèque de Pages d’aide fournit un **IDocumentationProvider** qui obtient la documentation à partir de chaînes de documentation XML. Le code se trouve dans /Areas/HelpPage/XmlDocumentationProvider.cs. Vous pouvez obtenir documentation à partir d’une autre source en écrivant votre propre **IDocumentationProvider**. Pour rattacher, appelez le **SetDocumentationProvider** méthode d’extension, définie dans **HelpPageConfigurationExtensions**

**ApiExplorer** appelle automatiquement la **IDocumentationProvider** interface permettant d’obtenir des chaînes de documentation pour chaque API. Elle les stocke dans le **Documentation** propriété de la **ApiDescription** et **ApiParameterDescription** objets.

## <a name="next-steps"></a>Étapes suivantes

Vous n’êtes pas limité aux pages d’aide indiqués ici. En fait, **ApiExplorer** n’est pas limité à la création de pages d’aide. Yao Huang Lin a écrit qu'un excellent blog publie pour susciter votre réflexion prêts à l’emploi :

- [Ajout d’un Client de Test simple à la Page d’aide ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Faire les API vous aider à la Page Web ASP.NET à fonctionner sur les services auto-hébergés](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Génération au moment du design de help page (ou client) pour l’API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personnalisations de Page d’aide avancées](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
