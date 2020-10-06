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
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="9b4f0-103">Prise en charge des options de requête OData dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="9b4f0-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="9b4f0-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9b4f0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9b4f0-105">Cette vue d’ensemble des exemples de code illustre les options de requête OData de prise en charge dans API Web ASP.NET 2 pour ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="9b4f0-106">OData définit des paramètres qui peuvent être utilisés pour modifier une requête OData.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="9b4f0-107">Le client envoie ces paramètres dans la chaîne de requête de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="9b4f0-108">Par exemple, pour trier les résultats, un client utilise le paramètre $orderby :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="9b4f0-109">La spécification OData appelle ces paramètres pour les *options de requête*.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="9b4f0-110">Vous pouvez activer les options de requête OData pour n’importe quel contrôleur d’API Web de votre projet &#8212; le contrôleur n’a pas besoin d’être un point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="9b4f0-111">Cela vous permet d’ajouter des fonctionnalités, telles que le filtrage et le tri, à toute application API Web.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="9b4f0-112">Avant d’activer les options de requête, consultez la rubrique aide sur la [sécurité OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="9b4f0-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="9b4f0-113">Activation des options de requête OData</span><span class="sxs-lookup"><span data-stu-id="9b4f0-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="9b4f0-114">Exemples de requêtes</span><span class="sxs-lookup"><span data-stu-id="9b4f0-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="9b4f0-115">Pagination pilotée par le serveur</span><span class="sxs-lookup"><span data-stu-id="9b4f0-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="9b4f0-116">Limitation des options de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="9b4f0-117">Appel direct des options de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="9b4f0-118">Validation de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="9b4f0-119">Activation des options de requête OData</span><span class="sxs-lookup"><span data-stu-id="9b4f0-119">Enabling OData Query Options</span></span>

<span data-ttu-id="9b4f0-120">L’API Web prend en charge les options de requête OData suivantes :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="9b4f0-121">Option</span><span class="sxs-lookup"><span data-stu-id="9b4f0-121">Option</span></span> | <span data-ttu-id="9b4f0-122">Description</span><span class="sxs-lookup"><span data-stu-id="9b4f0-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b4f0-123">$expand</span><span class="sxs-lookup"><span data-stu-id="9b4f0-123">$expand</span></span> | <span data-ttu-id="9b4f0-124">Développe les entités associées en ligne.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="9b4f0-125">$filter</span><span class="sxs-lookup"><span data-stu-id="9b4f0-125">$filter</span></span> | <span data-ttu-id="9b4f0-126">Filtre les résultats en fonction d’une condition booléenne.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="9b4f0-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="9b4f0-127">$inlinecount</span></span> | <span data-ttu-id="9b4f0-128">Indique au serveur d’inclure le nombre total d’entités correspondantes dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="9b4f0-129">(Utile pour la pagination côté serveur.)</span><span class="sxs-lookup"><span data-stu-id="9b4f0-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="9b4f0-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="9b4f0-130">$orderby</span></span> | <span data-ttu-id="9b4f0-131">Trie les résultats.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-131">Sorts the results.</span></span> |
| <span data-ttu-id="9b4f0-132">$select</span><span class="sxs-lookup"><span data-stu-id="9b4f0-132">$select</span></span> | <span data-ttu-id="9b4f0-133">Sélectionne les propriétés à inclure dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="9b4f0-134">$skip</span><span class="sxs-lookup"><span data-stu-id="9b4f0-134">$skip</span></span> | <span data-ttu-id="9b4f0-135">Ignore les n premiers résultats.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-135">Skips the first n results.</span></span> |
| <span data-ttu-id="9b4f0-136">$top</span><span class="sxs-lookup"><span data-stu-id="9b4f0-136">$top</span></span> | <span data-ttu-id="9b4f0-137">Retourne uniquement les n premiers résultats.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="9b4f0-138">Pour utiliser les options de requête OData, vous devez les activer explicitement.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="9b4f0-139">Vous pouvez les activer globalement pour l’ensemble de l’application, ou les activer pour des contrôleurs ou des actions spécifiques.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="9b4f0-140">Pour activer les options de requête OData globalement, appelez **EnableQuerySupport** sur la classe **HttpConfiguration** au démarrage :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="9b4f0-141">La méthode **EnableQuerySupport** active les options de requête globalement pour toute action de contrôleur qui retourne un type **IQueryable** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="9b4f0-142">Si vous ne souhaitez pas que les options de requête soient activées pour l’ensemble de l’application, vous pouvez les activer pour des actions de contrôleur spécifiques en ajoutant l’attribut **[interrogeable]** à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="9b4f0-143">Exemples de requêtes</span><span class="sxs-lookup"><span data-stu-id="9b4f0-143">Example Queries</span></span>

<span data-ttu-id="9b4f0-144">Cette section présente les types de requêtes qui sont possibles à l’aide des options de requête OData.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="9b4f0-145">Pour plus d’informations sur les options de requête, reportez-vous à la documentation OData sur [www.OData.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="9b4f0-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="9b4f0-146">Pour plus d’informations sur les $expand et les $select, consultez [utilisation de $Select, $Expand et $value dans API Web ASP.net OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="9b4f0-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="9b4f0-147">**Pagination pilotée par le client**</span><span class="sxs-lookup"><span data-stu-id="9b4f0-147">**Client-Driven Paging**</span></span>

<span data-ttu-id="9b4f0-148">Pour les jeux d’entités volumineux, le client peut vouloir limiter le nombre de résultats.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="9b4f0-149">Par exemple, un client peut afficher 10 entrées à la fois, avec des liens « suivant » pour obtenir la page de résultats suivante.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="9b4f0-150">Pour ce faire, le client utilise les options $top et $skip.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="9b4f0-151">L’option $top indique le nombre maximal d’entrées à retourner et l’option $skip indique le nombre d’entrées à ignorer.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="9b4f0-152">L’exemple précédent extrait les entrées 21 à 30.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-152">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="9b4f0-153">**Filtrage**</span><span class="sxs-lookup"><span data-stu-id="9b4f0-153">**Filtering**</span></span>

<span data-ttu-id="9b4f0-154">L’option $filter permet à un client de filtrer les résultats en appliquant une expression booléenne.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="9b4f0-155">Les expressions de filtre sont très puissantes. elles incluent des opérateurs logiques et arithmétiques, des fonctions de chaîne et des fonctions de date.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="9b4f0-156">Retourne tous les produits dont la catégorie est égale à « jouets ».</span><span class="sxs-lookup"><span data-stu-id="9b4f0-156">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="9b4f0-157">`http://localhost/Products?$filter=Category` « Jouets » EQ</span><span class="sxs-lookup"><span data-stu-id="9b4f0-157">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="9b4f0-158">Retourne tous les produits dont le prix est inférieur à 10.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-158">Return all products with price less than 10.</span></span> | <span data-ttu-id="9b4f0-159">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="9b4f0-159">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="9b4f0-160">Opérateurs logiques : retournent tous les produits où Price >= 5 et Price <= 15.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="9b4f0-161">`http://localhost/Products?$filter=Price` GE 5 et prix le 15</span><span class="sxs-lookup"><span data-stu-id="9b4f0-161">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="9b4f0-162">Fonctions de chaîne : retourne tous les produits dont le nom comporte « ZZ ».</span><span class="sxs-lookup"><span data-stu-id="9b4f0-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="9b4f0-163">Fonctions de date : retournent tous les produits avec la version finale après 2005.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="9b4f0-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="9b4f0-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="9b4f0-165">**Tri**</span><span class="sxs-lookup"><span data-stu-id="9b4f0-165">**Sorting**</span></span>

<span data-ttu-id="9b4f0-166">Pour trier les résultats, utilisez le filtre $orderby.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="9b4f0-167">Triez par prix.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="9b4f0-168">Triez par prix dans l’ordre décroissant (le plus élevé au plus bas).</span><span class="sxs-lookup"><span data-stu-id="9b4f0-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="9b4f0-169">Triez par catégorie, puis triez par prix dans l’ordre décroissant des catégories.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="9b4f0-170">Pagination pilotée par le serveur</span><span class="sxs-lookup"><span data-stu-id="9b4f0-170">Server-Driven Paging</span></span>

<span data-ttu-id="9b4f0-171">Si votre base de données contient des millions d’enregistrements, vous ne souhaitez pas les envoyer tous en une seule charge utile.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="9b4f0-172">Pour éviter cela, le serveur peut limiter le nombre d’entrées qu’il envoie dans une réponse unique.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="9b4f0-173">Pour activer la pagination du serveur, définissez la propriété **pageSize** dans l’attribut **interrogeable** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="9b4f0-174">La valeur est le nombre maximal d’entrées à retourner.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="9b4f0-175">Si votre contrôleur retourne le format OData, le corps de la réponse contient un lien vers la page suivante de données :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="9b4f0-176">Le client peut utiliser ce lien pour extraire la page suivante.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="9b4f0-177">Pour connaître le nombre total d’entrées dans le jeu de résultats, le client peut définir l’option de requête $inlinecount avec la valeur « AllPages ».</span><span class="sxs-lookup"><span data-stu-id="9b4f0-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="9b4f0-178">La valeur « AllPages » indique au serveur d’inclure le nombre total dans la réponse :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="9b4f0-179">Les liens de page suivante et le nombre Inline requièrent tous deux le format OData.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="9b4f0-180">En effet, OData définit des champs spéciaux dans le corps de la réponse pour contenir le lien et le nombre.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>

<span data-ttu-id="9b4f0-181">Pour les formats non-OData, il est toujours possible de prendre en charge les liens de page suivante et le nombre Inline, en encapsulant les résultats de la requête dans un objet \*\*PageResult &lt; T &gt; \*\* .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="9b4f0-182">Toutefois, il requiert un peu plus de code.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-182">However, it requires a bit more code.</span></span> <span data-ttu-id="9b4f0-183">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="9b4f0-184">Voici un exemple de réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="9b4f0-185">Limitation des options de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-185">Limiting the Query Options</span></span>

<span data-ttu-id="9b4f0-186">Les options de requête donnent au client un grand contrôle sur la requête exécutée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="9b4f0-187">Dans certains cas, vous souhaiterez peut-être limiter les options disponibles pour des raisons de sécurité ou de performances.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="9b4f0-188">L’attribut **[interrogeable]** possède des propriétés intégrées pour ce.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="9b4f0-189">Voici quelques exemples.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-189">Here are some examples.</span></span>

<span data-ttu-id="9b4f0-190">Autorisez uniquement les $skip et les $top pour prendre en charge la pagination et rien d’autre :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="9b4f0-191">Autorisez le classement uniquement par certaines propriétés, pour empêcher le tri sur les propriétés qui ne sont pas indexées dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="9b4f0-192">Autoriser la fonction logique « EQ », mais pas d’autres fonctions logiques :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="9b4f0-193">N’autorisez pas les opérateurs arithmétiques :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="9b4f0-194">Vous pouvez restreindre globalement les options en construisant une instance **QueryableAttribute** et en la transmettant à la fonction **EnableQuerySupport** :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="9b4f0-195">Appel direct des options de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="9b4f0-196">Au lieu d’utiliser l’attribut **[interrogeable]** , vous pouvez appeler les options de requête directement dans votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="9b4f0-197">Pour ce faire, ajoutez un paramètre **ODataQueryOptions** à la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="9b4f0-198">Dans ce cas, vous n’avez pas besoin de l’attribut **[interrogeable]** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="9b4f0-199">L’API Web remplit le **ODataQueryOptions** à partir de la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="9b4f0-200">Pour appliquer la requête, transmettez un **IQueryable** à la méthode **ApplyTo** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="9b4f0-201">La méthode retourne un autre **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="9b4f0-202">Pour les scénarios avancés, si vous n’avez pas de fournisseur de requêtes **IQueryable** , vous pouvez examiner le **ODataQueryOptions** et traduire les options de requête dans un autre formulaire.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="9b4f0-203">(Par exemple, consultez le billet de blog de RaghuRam Nadiminti [translation des requêtes OData vers HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), qui comprend également un [exemple](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="9b4f0-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="9b4f0-204">Validation de requête</span><span class="sxs-lookup"><span data-stu-id="9b4f0-204">Query Validation</span></span>

<span data-ttu-id="9b4f0-205">L’attribut **[interrogeable]** valide la requête avant de l’exécuter.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="9b4f0-206">L’étape de validation est effectuée dans la méthode **QueryableAttribute. ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="9b4f0-207">Vous pouvez également personnaliser le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-207">You can also customize the validation process.</span></span>

<span data-ttu-id="9b4f0-208">Consultez également les conseils sur la [sécurité OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="9b4f0-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="9b4f0-209">Tout d’abord, substituez l’une des classes de validateur qui est définie dans l’espace de noms **Web. http. OData. Query. Validators** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="9b4f0-210">Par exemple, la classe Validator suivante désactive l’option « DESC » pour l’option $orderby.</span><span class="sxs-lookup"><span data-stu-id="9b4f0-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="9b4f0-211">Sous-classe l’attribut **[interrogeable]** pour remplacer la méthode **ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="9b4f0-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="9b4f0-212">Ensuite, définissez votre attribut personnalisé globalement ou par contrôleur :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="9b4f0-213">Si vous utilisez **ODataQueryOptions** directement, définissez le validateur sur les options :</span><span class="sxs-lookup"><span data-stu-id="9b4f0-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
