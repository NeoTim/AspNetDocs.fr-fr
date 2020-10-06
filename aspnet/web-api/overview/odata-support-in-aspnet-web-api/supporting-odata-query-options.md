---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Prise en charge des options de requête OData dans API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Vue d’ensemble avec les exemples de code montre les options de requête OData de prise en charge dans API Web ASP.NET 2 pour ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 96820fab7ac89885058962f44ded86cb0184ee97
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "86188617"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Prise en charge des options de requête OData dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

Cette vue d’ensemble des exemples de code illustre les options de requête OData de prise en charge dans API Web ASP.NET 2 pour ASP.NET 4. x. 

OData définit des paramètres qui peuvent être utilisés pour modifier une requête OData. Le client envoie ces paramètres dans la chaîne de requête de l’URI de la demande. Par exemple, pour trier les résultats, un client utilise le paramètre $orderby :

`http://localhost/Products?$orderby=Name`

La spécification OData appelle ces paramètres pour les *options de requête*. Vous pouvez activer les options de requête OData pour n’importe quel contrôleur d’API Web de votre projet &#8212; le contrôleur n’a pas besoin d’être un point de terminaison OData. Cela vous permet d’ajouter des fonctionnalités, telles que le filtrage et le tri, à toute application API Web.

Avant d’activer les options de requête, consultez la rubrique aide sur la [sécurité OData](odata-security-guidance.md).

- [Activation des options de requête OData](#enable)
- [Exemples de requêtes](#examples)
- [Pagination pilotée par le serveur](#server-paging)
- [Limitation des options de requête](#limiting_query_options)
- [Appel direct des options de requête](#ODataQueryOptions)
- [Validation de requête](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Activation des options de requête OData

L’API Web prend en charge les options de requête OData suivantes :

| Option | Description |
| --- | --- |
| $expand | Développe les entités associées en ligne. |
| $filter | Filtre les résultats en fonction d’une condition booléenne. |
| $inlinecount | Indique au serveur d’inclure le nombre total d’entités correspondantes dans la réponse. (Utile pour la pagination côté serveur.) |
| $orderby | Trie les résultats. |
| $select | Sélectionne les propriétés à inclure dans la réponse. |
| $skip | Ignore les n premiers résultats. |
| $top | Retourne uniquement les n premiers résultats. |

Pour utiliser les options de requête OData, vous devez les activer explicitement. Vous pouvez les activer globalement pour l’ensemble de l’application, ou les activer pour des contrôleurs ou des actions spécifiques.

Pour activer les options de requête OData globalement, appelez **EnableQuerySupport** sur la classe **HttpConfiguration** au démarrage :

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

La méthode **EnableQuerySupport** active les options de requête globalement pour toute action de contrôleur qui retourne un type **IQueryable** . Si vous ne souhaitez pas que les options de requête soient activées pour l’ensemble de l’application, vous pouvez les activer pour des actions de contrôleur spécifiques en ajoutant l’attribut **[interrogeable]** à la méthode d’action.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Exemples de requêtes

Cette section présente les types de requêtes qui sont possibles à l’aide des options de requête OData. Pour plus d’informations sur les options de requête, reportez-vous à la documentation OData sur [www.OData.org](http://www.odata.org/).

Pour plus d’informations sur les $expand et les $select, consultez [utilisation de $Select, $Expand et $value dans API Web ASP.net OData](using-select-expand-and-value.md).

**Pagination pilotée par le client**

Pour les jeux d’entités volumineux, le client peut vouloir limiter le nombre de résultats. Par exemple, un client peut afficher 10 entrées à la fois, avec des liens « suivant » pour obtenir la page de résultats suivante. Pour ce faire, le client utilise les options $top et $skip.

`http://localhost/Products?$top=10&$skip=20`

L’option $top indique le nombre maximal d’entrées à retourner et l’option $skip indique le nombre d’entrées à ignorer. L’exemple précédent extrait les entrées 21 à 30.

**Filtrage**

L’option $filter permet à un client de filtrer les résultats en appliquant une expression booléenne. Les expressions de filtre sont très puissantes. elles incluent des opérateurs logiques et arithmétiques, des fonctions de chaîne et des fonctions de date.

| Retourne tous les produits dont la catégorie est égale à « jouets ». | `http://localhost/Products?$filter=Category` « Jouets » EQ |
| --- | --- |
| Retourne tous les produits dont le prix est inférieur à 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Opérateurs logiques : retournent tous les produits où Price >= 5 et Price <= 15. | `http://localhost/Products?$filter=Price` GE 5 et prix le 15 |
| Fonctions de chaîne : retourne tous les produits dont le nom comporte « ZZ ». | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Fonctions de date : retournent tous les produits avec la version finale après 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Tri**

Pour trier les résultats, utilisez le filtre $orderby.

| Triez par prix. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Triez par prix dans l’ordre décroissant (le plus élevé au plus bas). | `http://localhost/Products?$orderby=Price desc` |
| Triez par catégorie, puis triez par prix dans l’ordre décroissant des catégories. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Pagination pilotée par le serveur

Si votre base de données contient des millions d’enregistrements, vous ne souhaitez pas les envoyer tous en une seule charge utile. Pour éviter cela, le serveur peut limiter le nombre d’entrées qu’il envoie dans une réponse unique. Pour activer la pagination du serveur, définissez la propriété **pageSize** dans l’attribut **interrogeable** . La valeur est le nombre maximal d’entrées à retourner.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Si votre contrôleur retourne le format OData, le corps de la réponse contient un lien vers la page suivante de données :

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Le client peut utiliser ce lien pour extraire la page suivante. Pour connaître le nombre total d’entrées dans le jeu de résultats, le client peut définir l’option de requête $inlinecount avec la valeur « AllPages ».

`http://localhost/Products?$inlinecount=allpages`

La valeur « AllPages » indique au serveur d’inclure le nombre total dans la réponse :

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Les liens de page suivante et le nombre Inline requièrent tous deux le format OData. En effet, OData définit des champs spéciaux dans le corps de la réponse pour contenir le lien et le nombre.

Pour les formats non-OData, il est toujours possible de prendre en charge les liens de page suivante et le nombre Inline, en encapsulant les résultats de la requête dans un objet **PageResult &lt; T &gt; ** . Toutefois, il requiert un peu plus de code. Voici un exemple :

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Voici un exemple de réponse JSON :

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitation des options de requête

Les options de requête donnent au client un grand contrôle sur la requête exécutée sur le serveur. Dans certains cas, vous souhaiterez peut-être limiter les options disponibles pour des raisons de sécurité ou de performances. L’attribut **[interrogeable]** possède des propriétés intégrées pour ce. Voici quelques exemples.

Autorisez uniquement les $skip et les $top pour prendre en charge la pagination et rien d’autre :

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Autorisez le classement uniquement par certaines propriétés, pour empêcher le tri sur les propriétés qui ne sont pas indexées dans la base de données :

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Autoriser la fonction logique « EQ », mais pas d’autres fonctions logiques :

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

N’autorisez pas les opérateurs arithmétiques :

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Vous pouvez restreindre globalement les options en construisant une instance **QueryableAttribute** et en la transmettant à la fonction **EnableQuerySupport** :

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Appel direct des options de requête

Au lieu d’utiliser l’attribut **[interrogeable]** , vous pouvez appeler les options de requête directement dans votre contrôleur. Pour ce faire, ajoutez un paramètre **ODataQueryOptions** à la méthode de contrôleur. Dans ce cas, vous n’avez pas besoin de l’attribut **[interrogeable]** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

L’API Web remplit le **ODataQueryOptions** à partir de la chaîne de requête URI. Pour appliquer la requête, transmettez un **IQueryable** à la méthode **ApplyTo** . La méthode retourne un autre **IQueryable**.

Pour les scénarios avancés, si vous n’avez pas de fournisseur de requêtes **IQueryable** , vous pouvez examiner le **ODataQueryOptions** et traduire les options de requête dans un autre formulaire. (Par exemple, consultez le billet de blog de RaghuRam Nadiminti [translation des requêtes OData vers HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), qui comprend également un [exemple](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validation de requête

L’attribut **[interrogeable]** valide la requête avant de l’exécuter. L’étape de validation est effectuée dans la méthode **QueryableAttribute. ValidateQuery** . Vous pouvez également personnaliser le processus de validation.

Consultez également les conseils sur la [sécurité OData](odata-security-guidance.md).

Tout d’abord, substituez l’une des classes de validateur qui est définie dans l’espace de noms **Web. http. OData. Query. Validators** . Par exemple, la classe Validator suivante désactive l’option « DESC » pour l’option $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Sous-classe l’attribut **[interrogeable]** pour remplacer la méthode **ValidateQuery** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Ensuite, définissez votre attribut personnalisé globalement ou par contrôleur :

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Si vous utilisez **ODataQueryOptions** directement, définissez le validateur sur les options :

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
