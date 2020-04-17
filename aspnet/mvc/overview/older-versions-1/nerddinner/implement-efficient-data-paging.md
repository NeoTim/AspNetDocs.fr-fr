---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Mettre en œuvre un collectement de données efficaces . Microsoft Docs
author: rick-anderson
description: Étape 8 montre comment ajouter le soutien de la recherche de nourriture à notre /Dîners URL de sorte qu’au lieu d’afficher 1000s de dîners à la fois, nous allons seulement afficher 10 dîners à venir à ...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: a833553fe44b62b136f7eb55c7e00eca0b0462c6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541323"
---
# <a name="implement-efficient-data-paging"></a>Implémenter une pagination des données efficace

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 8 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> Étape 8 montre comment ajouter le soutien de la recherche de nourriture à notre /Dîners URL de sorte qu’au lieu d’afficher 1000s de dîners à la fois, nous allons seulement afficher 10 dîners à venir à la fois - et permettre aux utilisateurs finaux de page en arrière et en avant à travers la liste entière d’une manière amicale SEO.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner Étape 8: Soutien à la recherche de la nourriture

Si notre site est un succès, il y aura des milliers de dîners à venir. Nous devons nous assurer que nos échelles d’interface utilisateur pour gérer tous ces dîners, et permet aux utilisateurs de les parcourir. Pour ce faire, nous ajouterons un support de mise en service à notre URL */Dîners* afin qu’au lieu d’afficher des 1000 dîners à la fois, nous n’afficherons que 10 dîners à venir à la fois - et permettrons aux utilisateurs finaux de passer en revue la liste entière d’une manière amicale se DIF.

### <a name="index-action-method-recap"></a>Index() Récapitulatif de la méthode d’action

La méthode d’action Index() au sein de notre classe DinnersController ressemble actuellement ci-dessous :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Lorsqu’une demande est faite à l’URL */Dîners,* elle récupère une liste de tous les dîners à venir, puis rend une liste de tous :

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Comprendre IQueryable&lt;T&gt;

*IQueryable&lt;&gt; T* est une interface qui a été introduite avec LINQ dans le cadre de .NET 3.5. Il permet de puissants scénarios d'« exécution différée » dont nous pouvons profiter pour mettre en œuvre un support de mise en œuvre.

Dans notre DinnerRepository nous revenons&lt;une&gt; séquence IQueryable Dinner de notre méthode FindUpcomingDinners() :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

L’objet IQueryable&lt;Dinner&gt; retourné par notre méthode FindUpcomingDinners() résume une requête pour récupérer les objets du dîner de notre base de données en utilisant LINQ à SQL. Fait important, il n’exécutera pas la requête contre la base de données jusqu’à ce que nous tentions d’accéder / itérer sur les données dans la requête, ou jusqu’à ce que nous appelons la méthode ToList () sur elle. Le code appelant notre méthode FindUpcomingDinners() peut choisir d’ajouter d’autres opérations/filtres " enchaînés " à l’objet IQueryable&lt;Dinner&gt; avant d’exécuter la requête. LINQ à SQL est alors assez intelligent pour exécuter la requête combinée contre la base de données lorsque les données sont demandées.

Pour mettre en œuvre la logique de la radionerie, nous pouvons mettre à jour notre méthode d’action&lt;De&gt; l’indice DinnersController() afin qu’elle applique des opérateurs supplémentaires de « Skip » et de « Take » à la séquence de dîner IQueryable retournée avant d’appeler ToList () dessus :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Le code ci-dessus saute sur les 10 premiers dîners à venir dans la base de données, puis retourne 20 dîners. LINQ à SQL est assez intelligent pour construire une requête SQL optimisée qui effectue cette logique de saut dans la base de données SQL - et non dans le serveur Web. Cela signifie que même si nous avons des millions de dîners à venir dans la base de données, seuls les 10 que nous voulons seront récupérés dans le cadre de cette demande (ce qui la rend efficace et évolutive).

### <a name="adding-a-page-value-to-the-url"></a>Ajout d’une valeur "page" à l’URL

Au lieu de coder dur une plage de page spécifique, nous voulons que nos URL incluent un paramètre de « page » qui indique la gamme de dîner qu’un utilisateur demande.

#### <a name="using-a-querystring-value"></a>Utilisation d’une valeur De requête

Le code ci-dessous montre comment nous pouvons mettre à jour notre méthode d’action Index() pour prendre en charge un paramètre de requête et activer les URL comme */Dîners?page 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

La méthode d’action Index() ci-dessus a un paramètre nommé «page». Le paramètre est déclaré comme un integer nul (c’est ce qu’int? indique). Cela signifie que l’URL */Dîners?page 2* fera passer une valeur de "2" comme valeur de paramètre. *L’URL /Dîners* (sans valeur de requête) fera passer une valeur nulle.

Nous multiplions la valeur de la page par la taille de la page (dans ce cas 10 rangées) pour déterminer combien de dîners à sauter. Nous utilisons [l’opérateur de fusion nul Cmd (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) qui est utile lorsqu’il s’agit de types nuls. Le code ci-dessus attribue à la page la valeur de 0 si le paramètre de la page est nul.

#### <a name="using-embedded-url-values"></a>Utilisation des valeurs URL intégrées

Une alternative à l’utilisation d’une valeur de requête serait d’intégrer le paramètre de la page dans l’URL réelle elle-même. Par exemple : */Dîners/Page/2* ou */Dîners/2*. ASP.NET MVC comprend un puissant moteur de routage d’URL qui facilite la prise en charge de scénarios comme celui-ci.

Nous pouvons enregistrer des règles de routage personnalisées qui cartographient n’importe quel format URL ou URL entrant à n’importe quel classe de contrôleur ou méthode d’action que nous voulons. Tout ce que nous devons faire est d’ouvrir le fichier Global.asax dans notre projet :

![](implement-efficient-data-paging/_static/image2.png)

Et puis enregistrez une nouvelle règle de cartographie en utilisant la méthode MapRoute () aide comme le premier appel aux itinéraires. MapRoute() ci-dessous:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Ci-dessus, nous enregistrons une nouvelle règle de routage nommé "UpcomingDinners". Nous vous indiquons qu’il a le format URL "Dîners / Page / page" - où la page est une valeur de paramètre intégrée dans l’URL. Le troisième paramètre de la méthode MapRoute() indique que nous devrions cartographier les URL qui correspondent à ce format à la méthode d’action Index() de la classe DinnersController.

Nous pouvons utiliser exactement le même code Index () que nous avions avant avec notre scénario de requête - sauf maintenant notre "page" paramètre viendra de l’URL et non pas la requête:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Et maintenant, quand nous exécuteons l’application et tapez dans */Dîners* nous verrons les 10 premiers dîners à venir:

![](implement-efficient-data-paging/_static/image3.png)

Et quand nous tapons *dans /Dîners / Page / 1* nous verrons la page suivante des dîners:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Ajout de l’interface utilisateur de navigation de page

La dernière étape pour compléter notre scénario de mise en œuvre sera de mettre en œuvre "prochaine" et "précédente" interface utilisateur de navigation dans notre modèle de vue pour permettre aux utilisateurs de sauter facilement sur les données du dîner.

Pour mettre cela en œuvre correctement, nous aurons besoin de connaître le nombre total de dîners dans la base de données, ainsi que le nombre de pages de données que cela se traduit. Nous devrons alors calculer si la valeur de la « page » actuellement demandée est au début ou à la fin des données, et afficher ou cacher l’interface utilisateur « précédente » et « prochaine » en conséquence. Nous pourrions mettre en œuvre cette logique dans notre méthode d’action Index(). Alternativement, nous pouvons ajouter une classe d’aide à notre projet qui résume cette logique d’une manière plus résyvrable.

Voici une simple classe d’aide "PaginatedList"&lt;qui&gt; dérive de la classe de collecte Liste T intégrée dans le cadre .NET. Il implémente une classe de collecte ré-utilisable qui peut être utilisée pour paginate n’importe quelle séquence de données IQueryable. Dans notre application NerdDinner, nous allons le&lt;faire&gt; fonctionner sur les résultats IQueryable Dinner,&gt; mais il&lt;pourrait&gt; tout aussi bien être utilisé contre IQueryable&lt;Product ou IQueryable Customer résultats dans d’autres scénarios d’application:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Remarquez ci-dessus comment il calcule puis expose des propriétés comme "PageIndex", "PageSize", "TotalCount", et "TotalPages". Il expose également deux propriétés d’aide "HasPreviousPage" et "HasNextPage" qui indiquent si la page de données de la collection est au début ou à la fin de la séquence originale. Le code ci-dessus fera exécuter deux requêtes SQL - la première pour récupérer le nombre total d’objets Du dîner (cela ne renvoie pas les objets - il effectue plutôt une déclaration "SELECT COUNT" qui renvoie un entier), et le second pour récupérer seulement les rangées de données dont nous avons besoin à partir de notre base de données pour la page actuelle de données.

Nous pouvons ensuite mettre à jour notre méthode d’aide DinnersController.Index() pour créer un dîner&lt;&gt; PaginatedList à partir de notre résultat DinnerRepository.FindUpcomingDinners() et le transmettre à notre modèle de vue:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Nous pouvons ensuite mettre à jour le modèle de vue «Views-Dîners-Index.aspx» pour hériter de&lt;ViewPage NerdDinner.Helpers.PaginatedList&lt;&gt; &gt; Dinner&lt;au lieu de ViewPage&lt;IEnumerable Dinner&gt;&gt;, puis ajouter le code suivant au bas de notre view-template pour afficher ou cacher l’interface utilisateur de navigation suivante et précédente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Remarquez ci-dessus comment nous utilisons la méthode d’aide Html.RouteLink() pour générer nos hyperliens. Cette méthode est similaire à la méthode d’aide Html.ActionLink() que nous avons utilisée précédemment. La différence est que nous produisons l’URL en utilisant la règle de routage "UpcomingDinners" que nous mettons en place dans notre fichier Global.asax. Cela garantit que nous allons générer des URL à notre index () méthode d’action qui ont le format: */Dîners / Page / page / page -* où la valeur «page» est une variable que nous fournissons ci-dessus en fonction de l’actuel PageIndex.

Et maintenant, quand nous avons exécuté notre application à nouveau, nous allons voir 10 dîners à la fois dans notre navigateur:

![](implement-efficient-data-paging/_static/image5.png)

Nous avons &lt; &lt; &lt; &gt; &gt; &gt; également et l’interface utilisateur de navigation au bas de la page qui nous permet de sauter vers l’avant et vers l’arrière sur nos données à l’aide de moteurs de recherche URL accessibles:

![](implement-efficient-data-paging/_static/image6.png)

| **Sujet secondaire : Comprendre les implications&lt;de IQueryable T&gt;** |
| --- |
| IQueryable&lt;&gt; T est une fonctionnalité très puissante qui permet une variété de scénarios d’exécution différés intéressants (comme le pagination et les requêtes basées sur la composition). Comme avec toutes les fonctionnalités puissantes, vous voulez être prudent avec la façon dont vous l’utilisez et assurez-vous qu’il n’est pas abusé. Il est important de reconnaître que&lt;le&gt; retour d’un T IQueryable résultat de votre référentiel permet d’appeler le code d’identification pour y annexer les méthodes d’opérateur enchaîné, et ainsi participer à l’exécution ultime de requête. Si vous ne voulez pas fournir le code d’appel&lt;&gt; de cette capacité,&lt;&gt; alors vous devriez revenir IList T ou IEnumerable T résultats - qui contiennent les résultats d’une requête qui a déjà exécuté. Pour les scénarios de pagination cela vous obligerait à pousser la logique réelle de pagination des données dans la méthode de dépôt appelé. Dans ce scénario, nous pourrions mettre à jour notre méthode FindUpcomingDinners() finder&lt; d’avoir&gt; une signature qui soit retourné un PaginatedList: PaginatedList Dinner FindUpcomingDinners (int pageIndex, int pageSize)&lt;-&gt; Ou revenir sur un dîner&lt;&gt;IList , et utiliser un "totalCount" param pour retourner le nombre total de dîners: IList Dinner FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) |

### <a name="next-step"></a>étape suivante

Examinons maintenant comment nous pouvons ajouter l’authentification et le support d’autorisation à notre application.

> [!div class="step-by-step"]
> [Suivant précédent](re-use-ui-using-master-pages-and-partials.md)
> [Next](secure-applications-using-authentication-and-authorization.md)
