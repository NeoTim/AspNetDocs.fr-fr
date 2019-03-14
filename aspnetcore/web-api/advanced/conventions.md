---
title: Utiliser les conventions d’API web
author: pranavkm
description: Découvrez plus d’informations sur les conventions d’API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 5ae96b213a19464045e1d0b1a76f8eb81089dc5b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029936"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="16d46-103">Utiliser les conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="16d46-103">Use web API conventions</span></span>

<span data-ttu-id="16d46-104">Par [Pranav Krishnamoorthy](https://github.com/pranavkm) et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="16d46-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="16d46-105">ASP.NET Core 2.2 et ses versions ultérieures incluent un moyen d’extraire la [documentation des API](xref:tutorials/web-api-help-pages-using-swagger) courantes et de l’appliquer à plusieurs actions, à plusieurs contrôleurs ou à tous les contrôleurs au sein d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="16d46-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="16d46-106">Les conventions des API web sont un substitut permettant de décorer des actions individuelles avec [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="16d46-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="16d46-107">Une convention vous permet de :</span><span class="sxs-lookup"><span data-stu-id="16d46-107">A convention allows you to:</span></span>

* <span data-ttu-id="16d46-108">Définir les types de retours les plus courants et les codes d’état retournés à partir d’un type d’action spécifique.</span><span class="sxs-lookup"><span data-stu-id="16d46-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="16d46-109">Identifier les actions qui s’écartent de la norme définie.</span><span class="sxs-lookup"><span data-stu-id="16d46-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="16d46-110">ASP.NET Core MVC 2.2 et ses versions ultérieures incluent un ensemble de conventions par défaut dans `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="16d46-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="16d46-111">Les conventions sont basées sur le contrôleur (*ValuesController.cs*) fourni dans le modèle de projet de l’**API** ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16d46-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="16d46-112">Si vos actions suivent les profils dans le modèle, vous devriez normalement réussir à utiliser les conventions par défaut.</span><span class="sxs-lookup"><span data-stu-id="16d46-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="16d46-113">Si les conventions par défaut ne répondent pas à vos besoins, consultez [Créer des conventions d’API web](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="16d46-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="16d46-114">À l’exécution, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> comprend les conventions.</span><span class="sxs-lookup"><span data-stu-id="16d46-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="16d46-115">`ApiExplorer` est l’abstraction de MVC pour communiquer avec des générateurs de documents [OpenAPI](https://www.openapis.org/) (également nommé Swagger).</span><span class="sxs-lookup"><span data-stu-id="16d46-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="16d46-116">Les attributs provenant de la convention appliquée sont associés à une action et sont inclus dans la documentation OpenAPI de l’action.</span><span class="sxs-lookup"><span data-stu-id="16d46-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="16d46-117">Les [analyseurs d’API](xref:web-api/advanced/analyzers) comprennent aussi les conventions.</span><span class="sxs-lookup"><span data-stu-id="16d46-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="16d46-118">Si votre action n’est pas conventionnelle (par exemple, elle retourne un code d’état qui n’est pas documenté par la convention appliquée), un avertissement vous encourage à documenter le code d’état.</span><span class="sxs-lookup"><span data-stu-id="16d46-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="16d46-119">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16d46-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="16d46-120">Appliquer des conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="16d46-120">Apply web API conventions</span></span>

<span data-ttu-id="16d46-121">Les conventions ne se combinent pas : chaque action peut être associée à une seule convention.</span><span class="sxs-lookup"><span data-stu-id="16d46-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="16d46-122">Les conventions plus spécifiques sont prioritaires par rapport à celles qui sont moins spécifiques.</span><span class="sxs-lookup"><span data-stu-id="16d46-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="16d46-123">La sélection est non déterministe quand plusieurs conventions de même priorité s’appliquent à une action.</span><span class="sxs-lookup"><span data-stu-id="16d46-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="16d46-124">Les options suivantes sont disponibles pour appliquer une convention à une action, de la plus spécifique à la moins spécifique :</span><span class="sxs-lookup"><span data-stu-id="16d46-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="16d46-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; s’applique à des actions individuelles, et spécifie le type de convention et la méthode de convention qui s’applique.</span><span class="sxs-lookup"><span data-stu-id="16d46-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="16d46-126">Dans l’exemple suivant, la méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` du type de convention par défaut est appliquée à l’action `Update` :</span><span class="sxs-lookup"><span data-stu-id="16d46-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="16d46-127">La méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` applique les attributs suivants à l’action :</span><span class="sxs-lookup"><span data-stu-id="16d46-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

<span data-ttu-id="16d46-128">Pour plus d’informations sur `[ProducesDefaultResponseType]`, consultez [Réponse par défaut](https://swagger.io/docs/specification/describing-responses/#default).</span><span class="sxs-lookup"><span data-stu-id="16d46-128">For more information on `[ProducesDefaultResponseType]`, see [Default Response](https://swagger.io/docs/specification/describing-responses/#default).</span></span>

1. <span data-ttu-id="16d46-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un contrôleur &mdash; applique le type de convention spécifié à toutes les actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="16d46-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="16d46-130">Une méthode de convention est décorée avec des indicateurs qui déterminent les actions auxquelles la méthode de convention s’applique.</span><span class="sxs-lookup"><span data-stu-id="16d46-130">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="16d46-131">Pour plus d’informations sur les indicateurs, consultez [Créer des conventions d’API web](#create-web-api-conventions)).</span><span class="sxs-lookup"><span data-stu-id="16d46-131">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="16d46-132">Dans l’exemple suivant, l’ensemble de conventions par défaut est appliqué à toutes les actions dans *ContactsConventionController* :</span><span class="sxs-lookup"><span data-stu-id="16d46-132">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="16d46-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un assembly &mdash; applique le type de convention spécifié à tous les contrôleurs dans l’assembly actif.</span><span class="sxs-lookup"><span data-stu-id="16d46-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="16d46-134">Il est recommandé d’appliquer des attributs de niveau assembly au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="16d46-134">As a recommendation, apply assembly-level attributes in the *Startup.cs* file.</span></span>

    <span data-ttu-id="16d46-135">Dans l’exemple suivant, l’ensemble de conventions par défaut est appliqué à tous les contrôleurs dans l’assembly :</span><span class="sxs-lookup"><span data-stu-id="16d46-135">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="16d46-136">Créer des conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="16d46-136">Create web API conventions</span></span>

<span data-ttu-id="16d46-137">Si les conventions d’API par défaut ne répondent pas à vos besoins, créez vos propres conventions.</span><span class="sxs-lookup"><span data-stu-id="16d46-137">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="16d46-138">Une convention est :</span><span class="sxs-lookup"><span data-stu-id="16d46-138">A convention is:</span></span>

* <span data-ttu-id="16d46-139">Un type statique avec des méthodes.</span><span class="sxs-lookup"><span data-stu-id="16d46-139">A static type with methods.</span></span>
* <span data-ttu-id="16d46-140">Capable de définir des [types de réponse](#response-types) et des [exigences de noms](#naming-requirements) sur les actions.</span><span class="sxs-lookup"><span data-stu-id="16d46-140">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="16d46-141">Types de réponse</span><span class="sxs-lookup"><span data-stu-id="16d46-141">Response types</span></span>

<span data-ttu-id="16d46-142">Ces méthodes sont annotées avec des attributs `[ProducesResponseType]` ou `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="16d46-142">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="16d46-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="16d46-143">For example:</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="16d46-144">Si des attributs de métadonnées plus spécifiques sont absents, l’application de cette convention à un assembly considère que :</span><span class="sxs-lookup"><span data-stu-id="16d46-144">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="16d46-145">La méthode de convention s’applique à toute action nommée `Find`.</span><span class="sxs-lookup"><span data-stu-id="16d46-145">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="16d46-146">Un paramètre nommé `id` est présent sur l’action `Find`.</span><span class="sxs-lookup"><span data-stu-id="16d46-146">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="16d46-147">Exigences de dénomination</span><span class="sxs-lookup"><span data-stu-id="16d46-147">Naming requirements</span></span>

<span data-ttu-id="16d46-148">Les attributs `[ApiConventionNameMatch]` et `[ApiConventionTypeMatch]` peuvent être appliqués à la méthode de convention qui détermine les actions auxquelles ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="16d46-148">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="16d46-149">Exemple :</span><span class="sxs-lookup"><span data-stu-id="16d46-149">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="16d46-150">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="16d46-150">In the preceding example:</span></span>

* <span data-ttu-id="16d46-151">L’option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` appliquée à la méthode indique que la convention correspond à n’importe action préfixée par « Find ».</span><span class="sxs-lookup"><span data-stu-id="16d46-151">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="16d46-152">Les exemples d’actions correspondantes incluent `Find`, `FindPet`, et `FindById`.</span><span class="sxs-lookup"><span data-stu-id="16d46-152">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="16d46-153">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` appliqué au paramètre indique que la convention correspond aux méthodes avec un seul paramètre se terminant par l’identificateur du suffixe.</span><span class="sxs-lookup"><span data-stu-id="16d46-153">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="16d46-154">Les exemples incluent des paramètres comme `id` ou `petId`.</span><span class="sxs-lookup"><span data-stu-id="16d46-154">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="16d46-155">`ApiConventionTypeMatch` peut être appliqué de façon similaire à des types pour contraindre le type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="16d46-155">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="16d46-156">Un argument `params[]` indique les paramètres restants pour lesquels une correspondance explicite n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="16d46-156">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16d46-157">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="16d46-157">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
