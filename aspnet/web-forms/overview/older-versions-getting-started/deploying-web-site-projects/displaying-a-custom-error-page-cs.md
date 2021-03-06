---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Affichage d’une page d’erreurs personnalisée (C#) | Microsoft Docs
author: rick-anderson
description: Qu’est-ce que l’utilisateur voit quand une erreur d’exécution se produit dans une application Web ASP.NET ? La réponse dépend de la façon dont la &lt; configuration customErrors du site Web &gt; ...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 777948732690443d63f1fb2afd6f72a33a496c44
ms.sourcegitcommit: 0d583ed9253103f3e50b6d729276e667591cdd41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86211053"
---
# <a name="displaying-a-custom-error-page-c"></a>Affichage d’une page d’erreur personnalisée (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Qu’est-ce que l’utilisateur voit quand une erreur d’exécution se produit dans une application Web ASP.NET ? La réponse dépend de la configuration de customErrors du site Web &lt; &gt; . Par défaut, les utilisateurs voient apparaître un écran jaune clair qui projettera qu’une erreur d’exécution s’est produite. Ce didacticiel montre comment personnaliser ces paramètres pour afficher une page d’erreurs personnalisées esthétiquement attrayante qui correspond à l’apparence de votre site.

## <a name="introduction"></a>Introduction

Dans un monde parfait, il n’y aurait pas d’erreurs d’exécution. Les programmeurs écrivaient du code avec Nary un bogue et une validation d’entrée utilisateur robuste, et les ressources externes telles que les serveurs de base de données et les serveurs de messagerie ne passeront jamais en mode hors connexion. Bien entendu, en réalité, les erreurs sont inévitables. Les classes du .NET Framework signalent une erreur en levant une exception. Par exemple, l’appel de la méthode Open d’un objet SqlConnection établit une connexion à la base de données spécifiée par une chaîne de connexion. Toutefois, si la base de données est en cours ou si les informations d’identification de la chaîne de connexion ne sont pas valides, la méthode Open lève une exception `SqlException` . Les exceptions peuvent être gérées par l’utilisation de `try/catch/finally` blocs. Si le code d’un `try` bloc lève une exception, le contrôle est transféré au bloc catch approprié où le développeur peut tenter de récupérer à partir de l’erreur. S’il n’existe aucun bloc catch correspondant, ou si le code qui a levé l’exception n’est pas dans un bloc try, l’exception démonte la pile des appels dans la recherche de `try/catch/finally` blocs.

Si l’exception se propage jusqu’au runtime ASP.NET sans être gérée, l' [ `Error` événement](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) de la [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)est déclenché et la page d' *erreur* configurée s’affiche. Par défaut, ASP.NET affiche une page d’erreur qui est affectueusement appelée [écran jaune de décès](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Il existe deux versions du YSOD : l’une montre les détails de l’exception, une trace de la pile et d’autres informations utiles aux développeurs pour le débogage de l’application (voir la **figure 1**). l’autre indique simplement qu’une erreur d’exécution s’est produite (voir la **figure 2**).

Les détails de l’exception YSOD sont particulièrement utiles pour le débogage de l’application par les développeurs, mais l’indication d’un YSOD pour les utilisateurs finaux est déconseillée et non professionnelle. Au lieu de cela, les utilisateurs finaux doivent être dirigés vers une page d’erreur qui gère l’apparence et la convivialité d’un site, avec une prose plus conviviale décrivant la situation. La bonne nouvelle, c’est que la création d’une page d’erreurs personnalisée est très simple. Ce didacticiel commence par un œil sur ASP. Pages d’erreurs différentes du réseau. Il montre ensuite comment configurer l’application Web pour montrer aux utilisateurs une page d’erreurs personnalisée en face d’une erreur.

### <a name="examining-the-three-types-of-error-pages"></a>Examen des trois types de pages d’erreur

Lorsqu’une exception non gérée se produit dans une application ASP.NET, l’un des trois types de pages d’erreur s’affiche :

- L’écran jaune Détails de l’exception de la page erreur de décès,
- L’écran d’erreur d’exécution jaune de la page d’erreur de décès, ou
- Une page d’erreurs personnalisée

Les développeurs de pages d’erreur les plus connus sont les détails de l’exception YSOD. Par défaut, cette page s’affiche pour les utilisateurs qui visitent localement et, par conséquent, la page qui s’affiche quand une erreur se produit lors du test du site dans l’environnement de développement. Comme son nom l’indique, les détails de l’exception YSOD fournissent des détails sur l’exception, le type, le message et la trace de la pile. En outre, si l’exception a été déclenchée par du code dans la classe code-behind de votre page ASP.NET et si l’application est configurée pour le débogage, les détails de l’exception YSOD affichent également cette ligne de code (et quelques lignes de code au-dessus et en dessous).

La **figure 1** montre la page Détails de l’exception YSOD. Notez l’URL dans la fenêtre d’adresse du navigateur : `http://localhost:62275/Genre.aspx?ID=foo` . Rappelez-vous que la `Genre.aspx` page répertorie les révisions de livres dans un genre particulier. Elle nécessite que la `GenreId` valeur (a `uniqueidentifier` ) soit transmise via la chaîne de chaîne. par exemple, l’URL appropriée pour consulter les révisions de fiction est `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146` . Si une valeur autre qu’une `uniqueidentifier` valeur est passée via la chaîne de chaîne (par exemple « foo »), une exception est levée.

> [!NOTE]
> Pour reproduire cette erreur dans l’application Web de démonstration disponible au téléchargement, vous pouvez soit consulter directement, soit `Genre.aspx?ID=foo` cliquer sur le lien « générer une erreur d’exécution » dans `Default.aspx` .

Notez les informations sur les exceptions présentées à la **figure 1**. Le message d’exception « échec de la conversion lors de la conversion d’une chaîne de caractères en uniqueidentifier » est présent en haut de la page. Le type de l’exception, `System.Data.SqlClient.SqlException` , est également indiqué. Il y a également la trace de la pile.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figure 1**: détails de l’exception YSOD contient des informations sur l’exception  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image3.png))

L’autre type de YSOD est l’erreur d’exécution YSOD, et est illustré à la **figure 2**. L’erreur d’exécution YSOD informe le visiteur qu’une erreur d’exécution s’est produite, mais il n’inclut pas d’informations sur l’exception qui a été levée. (Toutefois, il fournit des instructions sur la façon de rendre les détails de l’erreur visibles en modifiant le `Web.config` fichier, qui fait partie de ce qui rend ce YSOD non professionnel.)

Par défaut, l’erreur d’exécution YSOD est présentée aux utilisateurs visitant à distance (via http://www.yoursite.com) , comme indiqué par l’URL dans la barre d’adresse du navigateur de la **figure 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo` . Les deux écrans YSOD différents sont disponibles car les développeurs souhaitent connaître les détails de l’erreur, mais ces informations ne doivent pas être affichées sur un site actif, car elles peuvent révéler des failles de sécurité potentielles ou d’autres informations sensibles à toute personne visitant votre site.

> [!NOTE]
> Si vous suivez et que vous utilisez DiscountASP.NET comme hôte Web, vous remarquerez peut-être que l’erreur d’exécution YSOD ne s’affiche pas lors de la visite du site actif. Cela est dû au fait que DiscountASP.NET a leurs serveurs configurés pour afficher les détails de l’exception YSOD par défaut. La bonne nouvelle, c’est que vous pouvez remplacer ce comportement par défaut en ajoutant une `<customErrors>` section à votre `Web.config` fichier. La section « Configuration de la page d’erreur qui s’affiche » examine la `<customErrors>` section en détail.

[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figure 2**: l’erreur d’exécution YSOD n’inclut pas les détails de l’erreur  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image6.png))

Le troisième type de page d’erreur est la page d’erreurs personnalisée, qui est une page Web que vous créez. L’avantage d’une page d’erreurs personnalisée est que vous disposez d’un contrôle total sur les informations qui s’affichent à l’utilisateur, ainsi que l’apparence et la convivialité de la page. la page d’erreurs personnalisée peut utiliser la même page maître et les mêmes styles que les autres pages. La section « utilisation d’une page d’erreur personnalisée » vous guide tout au long de la création d’une page d’erreurs personnalisée et de sa configuration pour qu’elle s’affiche dans l’événement d’une exception non gérée. La **figure 3** présente une crête de cette page d’erreur personnalisée. Comme vous pouvez le voir, l’apparence de la page d’erreur est bien plus professionnelle que l’un des écrans jaunes de décès indiqués dans les figures 1 et 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figure 3**: une page d’erreurs personnalisée offre une apparence et une convivialité plus adaptées  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image9.png))

Prenez un moment pour inspecter la barre d’adresses du navigateur dans la **figure 3**. Notez que la barre d’adresse affiche l’URL de la page d’erreurs personnalisée ( `/ErrorPages/Oops.aspx` ). Dans les figures 1 et 2, les écrans jaunes de décès s’affichent dans la même page que celle d’où provient l’erreur ( `Genre.aspx` ). La page d’erreurs personnalisée reçoit l’URL de la page où l’erreur s’est produite via le `aspxerrorpath` paramètre QueryString.

## <a name="configuring-which-error-page-is-displayed"></a>Configuration de la page d’erreurs affichée

Les trois pages d’erreur possibles s’affichent en fonction de deux variables :

- Les informations de configuration de la `<customErrors>` section, et
- Si l’utilisateur visite le site localement ou à distance.

La [ `<customErrors>` section](https://msdn.microsoft.com/library/h0hfz6fc.aspx) dans `Web.config` a deux attributs qui affectent la page d’erreur affichée : `defaultRedirect` et `mode` . L'attribut `defaultRedirect` est facultatif. S’il est fourni, il spécifie l’URL de la page d’erreurs personnalisée et indique que la page d’erreurs personnalisée doit s’afficher à la place de l’erreur d’exécution YSOD. L' `mode` attribut est obligatoire et accepte l’une des trois valeurs suivantes : `On` , `Off` ou `RemoteOnly` . Ces valeurs ont le comportement suivant :

- `On`-indique que la page d’erreurs personnalisée ou l’erreur d’exécution YSOD est présentée à tous les visiteurs, qu’ils soient locaux ou distants.
- `Off`-Spécifie que les détails de l’exception YSOD sont affichés à tous les visiteurs, qu’ils soient locaux ou distants.
- `RemoteOnly`-indique que la page d’erreurs personnalisée ou l’erreur d’exécution YSOD est présentée aux visiteurs distants, tandis que les détails de l’exception YSOD s’affichent pour les visiteurs locaux.

À moins que vous ne spécifiiez sinon, ASP.NET agit comme si vous aviez défini l’attribut mode sur `RemoteOnly` et n’avait pas spécifié de `defaultRedirect` valeur. En d’autres termes, le comportement par défaut est que les détails de l’exception YSOD sont affichés aux visiteurs locaux alors que l’erreur d’exécution YSOD est présentée aux visiteurs distants. Vous pouvez remplacer ce comportement par défaut en ajoutant une `<customErrors>` section à l’application Web`Web.config file.`

## <a name="using-a-custom-error-page"></a>Utilisation d’une page d’erreurs personnalisée

Chaque application Web doit avoir une page d’erreurs personnalisée. Il fournit une alternative plus professionnelle à l’erreur d’exécution YSOD, il est facile à créer et la configuration de l’application pour utiliser la page d’erreurs personnalisée ne prend que quelques instants. La première étape consiste à créer la page d’erreurs personnalisée. J’ai ajouté un nouveau dossier à l’application de révisions de livres nommée `ErrorPages` et ajouté à cette nouvelle page ASP.NET nommée `Oops.aspx` . Faites en sorte que la page utilise la même page maître que le reste des pages de votre site afin qu’elle hérite automatiquement de la même apparence.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figure 4**: créer une page d’erreurs personnalisée

Consacrez ensuite quelques minutes à la création du contenu de la page d’erreur. J’ai créé une page d’erreurs personnalisée plutôt simple, avec un message indiquant qu’une erreur inattendue s’est produite et un lien vers la page d’accueil du site.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figure 5**: concevoir votre page d’erreurs personnalisée  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image14.png))

Une fois la page d’erreurs terminée, configurez l’application Web pour qu’elle utilise la page d’erreurs personnalisée à la place de l’erreur d’exécution YSOD. Pour ce faire, spécifiez l’URL de la page d’erreur dans l’attribut de la `<customErrors>` section `defaultRedirect` . Ajoutez le balisage suivant au fichier de votre application `Web.config` :

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Le balisage ci-dessus configure l’application pour afficher les détails de l’exception YSOD aux utilisateurs visitant localement, tout en utilisant la page d’erreur personnalisée \. aspx pour les utilisateurs visitant à distance. Pour voir cela en action, déployez votre site Web dans l’environnement de production, puis accédez à la page genre. aspx sur le site actif avec une valeur QueryString non valide. Vous devez voir la page d’erreurs personnalisées (reportez-vous à la **figure 3**).

Pour vérifier que la page d’erreurs personnalisée s’affiche uniquement pour les utilisateurs distants, visitez la `Genre.aspx` page avec une chaîne de commande non valide à partir de l’environnement de développement. Vous devez toujours voir les détails de l’exception YSOD (reportez-vous à la **figure 1**). Le `RemoteOnly` paramètre garantit que les utilisateurs qui visitent le site sur l’environnement de production voient la page d’erreurs personnalisées alors que les développeurs qui travaillent localement continuent de voir les détails de l’exception.

## <a name="notifying-developers-and-logging-error-details"></a>Notification des développeurs et journalisation des détails des erreurs

Les erreurs qui se produisent dans l’environnement de développement ont été provoquées par le développeur assis sur son ordinateur. Elle affiche les informations de l’exception dans les détails de l’exception YSOD et elle connaît les étapes qu’elle exécutait lorsque l’erreur s’est produite. Mais lorsqu’une erreur se produit sur la production, le développeur n’a aucune connaissance qu’une erreur s’est produite, à moins que l’utilisateur final visitant le site ne prenne le temps de signaler l’erreur. Et même si l’utilisateur n’est pas en mesure d’alerter l’équipe de développement qu’une erreur s’est produite, sans connaître le type d’exception, le message et la trace de la pile, il peut être difficile de diagnostiquer la cause de l’erreur, ce qui lui permet de résoudre le problème.

Pour ces raisons, il est primordial que toute erreur dans l’environnement de production soit enregistrée dans un magasin persistant (par exemple, une base de données) et que les développeurs soient avertis de cette erreur. La page d’erreurs personnalisée peut sembler être un bon emplacement pour la journalisation et la notification. Malheureusement, la page d’erreurs personnalisée n’a pas accès aux détails de l’erreur et ne peut donc pas être utilisée pour consigner ces informations. La bonne nouvelle, c’est qu’il existe plusieurs façons d’intercepter les détails de l’erreur et de les enregistrer, et les trois didacticiels suivants explorent cette rubrique plus en détail.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Utilisation de différentes pages d’erreur personnalisées pour différents États des erreurs HTTP

Quand une exception est levée par une page ASP.NET et n’est pas gérée, l’exception est détournée jusqu’au runtime ASP.NET, qui affiche la page d’erreurs configurée. Si une demande arrive dans le moteur ASP.NET mais ne peut pas être traitée pour une raison quelconque, le fichier demandé est peut-être introuvable ou les autorisations de lecture ont été désactivées pour le fichier, puis le moteur ASP.NET génère un `HttpException` . Cette exception, comme les exceptions levées à partir de pages ASP.NET, se propage jusqu’à l’exécution, provoquant l’affichage de la page d’erreur appropriée.

Ce que cela signifie pour l’application Web en production, c’est que si un utilisateur demande une page qui est introuvable, la page d’erreur personnalisée s’affiche. La **figure 6** illustre un tel exemple. Étant donné que la demande concerne une page inexistante ( `NoSuchPage.aspx` ), une `HttpException` exception est levée et la page d’erreur personnalisée s’affiche (Notez la référence à `NoSuchPage.aspx` dans le `aspxerrorpath` paramètre QueryString).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figure 6**: le runtime ASP.NET affiche la page d’erreurs configurée en réponse à une demande non valide ([cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image17.png))

Par défaut, tous les types d’erreurs provoquent l’affichage de la même page d’erreurs personnalisée. Toutefois, vous pouvez spécifier une page d’erreurs personnalisée différente pour un code d’état HTTP spécifique à l’aide des `<error>` éléments enfants dans la `<customErrors>` section. Par exemple, pour qu’une autre page d’erreur s’affiche en cas d’erreur de page introuvable, qui a un code d’état HTTP de 404, mettez à jour la `<customErrors>` section pour inclure le balisage suivant :

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Avec cette modification en place, chaque fois qu’un utilisateur visitant à distance une ressource ASP.NET qui n’existe pas, il est redirigé vers la `404.aspx` page d’erreur personnalisée au lieu de `Oops.aspx` . Comme le montre la **figure 7** , la `404.aspx` page peut inclure un message plus spécifique que la page d’erreurs personnalisées générale.

> [!NOTE]
> Consultez les [pages d’erreur 404, une fois de plus](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) pour obtenir des conseils sur la création de pages d’erreur 404 efficaces.

[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)

**Figure 7**: la page d’erreur personnalisée 404 affiche un message plus ciblé que`Oops.aspx`  
([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image20.png)) 

Étant donné que vous savez que la `404.aspx` page n’est accessible que lorsque l’utilisateur effectue une requête pour une page qui n’a pas été trouvée, vous pouvez améliorer cette page d’erreur personnalisée pour inclure les fonctionnalités permettant à l’utilisateur de résoudre ce type d’erreur spécifique. Par exemple, vous pouvez créer une table de base de données qui mappe des URL incorrectes connues à des URL correctes, puis faire en sorte que la `404.aspx` page d’erreurs personnalisée exécute une requête sur cette table et suggère des pages que l’utilisateur peut essayer d’atteindre.

> [!NOTE]
> La page d’erreurs personnalisée s’affiche uniquement quand une demande est effectuée sur une ressource gérée par le moteur ASP.NET. Comme nous l’avons vu dans les [principales différences entre IIS et le](core-differences-between-iis-and-the-asp-net-development-server-cs.md) didacticiel de serveur de développement ASP.net, le serveur Web peut traiter certaines demandes lui-même. Par défaut, le serveur Web IIS traite les demandes de contenu statique comme les images et les fichiers HTML sans appeler le moteur ASP.NET. Par conséquent, si l’utilisateur demande un fichier image qui n’existe pas, il récupère le message d’erreur 404 par défaut d’IIS plutôt que ASP. Page d’erreurs configurée sur le réseau.

## <a name="summary"></a>Récapitulatif

Lorsqu’une exception non gérée se produit dans une application ASP.NET, l’utilisateur affiche l’une des trois pages d’erreur suivantes : l’écran des détails de l’exception jaune de décès ; Écran d’erreur d’exécution jaune de décès ; ou une page d’erreurs personnalisée. La page d’erreur qui s’affiche dépend de la configuration de l’application `<customErrors>` et de la visite locale ou à distance de l’utilisateur. Le comportement par défaut consiste à afficher les détails de l’exception YSOD pour les visiteurs locaux et l’erreur d’exécution YSOD aux visiteurs distants.

Tandis que l’erreur d’exécution YSOD masque les informations d’erreur potentiellement sensibles de la part de l’utilisateur visitant le site, il s’interrompt de l’apparence et de la convivialité de votre site. Une meilleure approche consiste à utiliser une page d’erreurs personnalisée, qui implique la création et la conception de la page d’erreurs personnalisée et la spécification de son URL dans l' `<customErrors>` attribut de la section `defaultRedirect` . Vous pouvez même avoir plusieurs pages d’erreurs personnalisées pour différents États des erreurs HTTP.

La page d’erreurs personnalisée est la première étape d’une stratégie complète de gestion des erreurs pour un site Web en production. L’alerte du développeur de l’erreur et la journalisation de ses détails sont également des étapes importantes. Les trois didacticiels suivants explorent les techniques de notification d’erreurs et de journalisation.

Bonne programmation !

### <a name="further-reading"></a>Pour aller plus loin

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Pages d’erreurs, une fois de plus](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Instructions de conception pour les exceptions](https://msdn.microsoft.com/library/ms229014.aspx)
- [Pages d’erreurs conviviales](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gestion et levée des exceptions](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Utilisation correcte des pages d’erreurs personnalisées dans ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)
- [Vue d'ensemble du traçage ASP.NET](/previous-versions/aspnet/bb386420(v%3Dvs.100))

> [!div class="step-by-step"]
> [Précédent](strategies-for-database-development-and-deployment-cs.md) 
>  [Suivant](processing-unhandled-exceptions-cs.md)
